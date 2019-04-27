# Maintainer: Vladimir Yerilov <openmindead AT gmail DOT com>
pkgbase=bootsplash-themes
pkgname=('bootsplash-theme-xiaomi')
pkgver=0.7
pkgrel=7
url="https://github.com/openmindead/manjaro-bootsplash-mi"
arch=('x86_64')
license=('GPL')

depends=('systemd')
builddepends=('imagemagick')
optdepends=('bootsplash-systemd: for better interration of bootsplash')
options=('!libtool' '!emptydirs')

source=('bootsplash-packer'
	'bootsplash-xiaomi.sh'
	'bootsplash-xiaomi.initcpio_install'
	'spinner.gif'
	'mi.png')

sha256sums=('51559d3ccfb448b03fa6439faf5869dbd0c2fbda1b5d5bf5d4ba70e60937472a'
            'b1e4806a9c00e8ae3e04a0a10953863fa134c5c43e8a2dba87d40eb3c69e32a2'
            '46ad752854d09c91660833a20143af6657906072ffc353f7a9b70302c7fe0246'
            'b0c806ea7ebf554c4dd7d6dcd74f9a76b66d1ee24e4ffd04c3573f343b886986'
            '6083fc4bc9fd4fac2f5c1c109ec954597c01eece7469641981b948b99f9f62f1')

build() {
  cd "$srcdir"
  sh ./bootsplash-xiaomi.sh
}

package_bootsplash-theme-xiaomi() {
  pkgdesc="'XIAOMI' branded Bootsplash Theme for Manjaro Linux"
  cd "$srcdir"

  install -Dm644 "$srcdir/bootsplash-xiaomi" "$pkgdir/usr/lib/firmware/bootsplash-themes/xiaomi/bootsplash"
  install -Dm644 "$srcdir/bootsplash-xiaomi.initcpio_install" "$pkgdir/usr/lib/initcpio/install/bootsplash-xiaomi"
} 
