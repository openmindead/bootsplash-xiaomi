pkgbase=bootsplash-themes
pkgname=('bootsplash-theme-manjaro-mi')
pkgver=0.6
pkgrel=3
url="https://lists.freedesktop.org/archives/dri-devel/2017-December/160242.html"
arch=('x86_64')
license=('GPL')

depends=('linux416' 'systemd')
builddepends=('imagemagick')
options=('!libtool' '!emptydirs')

source=('bootsplash-packer'
	'bootsplash-manjaro-mi.sh'
	'bootsplash-manjaro-mi.initcpio_install'
	'spinner.gif'
	'manjaro-mi.png')

sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

build() {
  cd "$srcdir"
  sh ./bootsplash-manjaro-mi.sh
}

package_bootsplash-theme-manjaro-mi() {
  pkgdesc="Bootsplash Theme 'Manjaro MI'"
  cd "$srcdir"

  install -Dm644 "$srcdir/bootsplash-manjaro-mi" "$pkgdir/usr/lib/firmware/bootsplash-themes/manjaro-mi/bootsplash"
  install -Dm644 "$srcdir/bootsplash-manjaro-mi.initcpio_install" "$pkgdir/usr/lib/initcpio/install/bootsplash-manjaro-mi"
} 
