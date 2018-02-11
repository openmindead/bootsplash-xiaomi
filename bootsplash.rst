====================
The Linux bootsplash
====================

:Date: November, 2017
:Author: Max Staudt <mstaudt@suse.de>


The Linux bootsplash is a graphical replacement for the '``quiet``' boot
option, typically showing a logo and a spinner animation as the system starts.

Currently, it is a part of the Framebuffer Console support, and can be found
as ``CONFIG_BOOTSPLASH`` in the kernel configuration. This means that as long
as it is enabled, it hijacks fbcon's output and draws a splash screen instead.

Purely compiling in the bootsplash will not render it functional - to actually
render a splash, you will also need a splash theme file. See the example
utility and script in ``tools/bootsplash`` for a live demo.



Motivation
==========

- The '``quiet``' boot option only suppresses most messages during boot, but
  errors are still shown.

- A user space implementation can only show a logo once user space has been
  initialized far enough to allow this. A kernel splash can display a splash
  immediately as soon as fbcon can be displayed.

- Implementing a splash screen in user space (e.g. Plymouth) is problematic
  due to resource conflicts.

  For example, if Plymouth is keeping ``/dev/fb0`` (provided via vesafb/efifb)
  open, then most DRM drivers can't replace it because the address space is
  still busy - thus leading to a VRAM reservation error.

  See: https://bugzilla.opensuse.org/show_bug.cgi?id=980750



Command line arguments
======================

``bootsplash.bootfile``
  Which file in the initrd to load.
  Default: none, i.e. a non-functional splash, falling back to showing text.



sysfs run-time configuration
============================

``/sys/devices/platform/bootsplash.0/enabled``
  Enable/disable the bootsplash.
  The system boots with this set to 1, but will not show a splash unless
  a splash theme file is also loaded.


``/sys/devices/platform/bootsplash.0/drop_splash``
  Unload splash data and free memory.

``/sys/devices/platform/bootsplash.0/load_file``
  Load a splash file from ``/lib/firmware/``.
  Note that trailing newlines will be interpreted as part of the file name.



Kconfig
=======

``BOOTSPLASH``
  Whether to compile in bootsplash support
  (depends on fbcon compiled in, i.e. ``FRAMEBUFFER_CONSOLE=y``)



Bootsplash file format
======================

A file specified in the kernel configuration as ``CONFIG_BOOTSPLASH_FILE``
or specified on the command line as ``bootsplash.bootfile`` will be loaded
and displayed as soon as fbcon is initialized.


Main blocks
-----------

There are 3 main blocks in each file:

  - one File header
  -   n Picture headers
  -   m (Blob header + payload) blocks


Structures
----------

The on-disk structures are defined in
``drivers/video/fbdev/core/bootsplash_file.h`` and represent these blocks:

  - ``struct splash_file_header``

    Represents the file header, with splash-wide information including:

      - The magic string "``Linux bootsplash``" on big-endian platforms
        (the reverse on little endian)
      - The file format version (for incompatible updates, hopefully never)
      - The background color
      - Number of picture and blob blocks
      - Animation speed (we only allow one delay for all animations)

    The file header is followed by the first picture header.


  - ``struct splash_picture_header``

    Represents an object (picture) drawn on screen, including its immutable
    properties:
      - Width, height
      - Positioning relative to screen corners or in the center
      - Animation, if any
      - Animation type
      - Number of blobs

    The picture header is followed by another picture header, up until n
    picture headers (as defined in the file header) have been read. Then,
    the (blob header, payload) pairs follow.


  - ``struct splash_blob_header``
    (followed by payload)

    Represents one raw data stream. So far, only picture data is defined.

    The blob header is followed by a payload, then padding to n*16 bytes,
    then (if further blobs are defined in the file header) a further blob
    header.


Alignment
---------

The bootsplash file is designed to be loaded into memory as-is.

All structures are a multiple of 16 bytes long, all elements therein are
aligned to multiples of their length, and the payloads are always padded
up to multiples of 16 bytes. This is to allow aligned accesses in all
cases while still simply mapping the structures over an in-memory copy of
the bootsplash file.


Further information
-------------------

Please see ``drivers/video/fbdev/core/bootsplash_file.h`` for further
details and possible values in the file.



Hooks - how the bootsplash is integrated
========================================

``drivers/video/fbdev/core/fbcon.c``
  ``fbcon_init()`` calls ``bootsplash_init()``, which loads the default
  bootsplash file or the one specified on the kernel command line.

  ``fbcon_switch()`` draws the bootsplash when it's active, and is also
  one of the callers of ``set_blitting_type()``.

  ``set_blitting_type()`` calls ``fbcon_set_dummyops()`` when the
  bootsplash is active, overriding the text rendering functions.

  ``fbcon_cursor()`` will call ``bootsplash_disable()`` when an oops is
  being printed in order to make a kernel panic visible.

``drivers/video/fbdev/core/dummyblit.c``
  This contains the dummy text rendering functions used to suppress text
  output while the bootsplash is shown.

``drivers/tty/vt/keyboard.c``
  ``kbd_keycode()`` can call ``bootsplash_disable()`` when the user
  presses ESC or F1-F12 (changing VT). This is to provide a built-in way
  of disabling the splash manually at any time. 
