# $Id: $
# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgname=ca-certificates
pkgver=20080514
pkgrel=2
pkgdesc='Common CA certificates'
arch=('i686' 'x86_64')
url='http://www.debian.org'
license=('MPL' 'GPL')
source=("http://ftp.debian.org/debian/pool/main/c/${pkgname}/${pkgname}_${pkgver}.tar.gz" 'no-hooks.patch')
depends=('openssl')
makedepends=('ruby')
install=ca-certificates.install
backup=('etc/ca-certificates.conf')
md5sums=('25ca8aba089ffeb6d5051eee0027b427'
         '1eff5b1fabbe60144846be6772b95aa6')

build() {
	cd $srcdir/$pkgname

	# remove hooks support (uses run-parts which is debian specific)
	patch -p0 -i $srcdir/no-hooks.patch

	install -d -m755 $pkgdir/{etc,usr/{sbin,share/ca-certificates}}

	make
	make install DESTDIR=$pkgdir

	(
	echo "# Automatically generated by ${pkgname}-${pkgver}"
	echo "# $(date -u)"
	echo "# "
	cd $pkgdir/usr/share/ca-certificates
	find . -name '*.crt' | sort | cut -b3-
	) > $pkgdir/etc/ca-certificates.conf
}
