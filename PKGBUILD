# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgname=ca-certificates
pkgver=20080514
pkgrel=3
pkgdesc='Common CA certificates'
arch=('i686' 'x86_64')
url='http://packages.qa.debian.org/c/ca-certificates.html'
license=('MPL' 'GPL')
source=("http://ftp.debian.org/debian/pool/main/c/${pkgname}/${pkgname}_${pkgver}.tar.gz")
depends=('run-parts')
makedepends=('ruby')
install=ca-certificates.install
backup=('etc/ca-certificates.conf')
md5sums=('25ca8aba089ffeb6d5051eee0027b427')

build() {
	cd $srcdir/$pkgname

	install -d -m755 $pkgdir/{etc/ca-certificates/update.d,usr/{sbin,share/ca-certificates}}
	install -D -m644 sbin/update-ca-certificates.8 $pkgdir/usr/share/man/man8/update-ca-certificates.8
	make
	make install DESTDIR=$pkgdir

	(
	echo "# Automatically generated by ${pkgname}-${pkgver}-${pkgrel}"
	echo "# "
	cd $pkgdir/usr/share/ca-certificates
	find . -name '*.crt' | sort | cut -b3-
	) > $pkgdir/etc/ca-certificates.conf
}
