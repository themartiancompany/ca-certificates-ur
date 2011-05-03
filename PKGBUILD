# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgname=ca-certificates
pkgver=20110421
pkgrel=2
pkgdesc='Common CA certificates'
arch=('any')
url='http://packages.qa.debian.org/c/ca-certificates.html'
license=('MPL' 'GPL')
source=("http://ftp.debian.org/debian/pool/main/c/${pkgname}/${pkgname}_${pkgver}.tar.gz")
depends=('bash' 'run-parts' 'openssl' 'findutils' 'coreutils' 'sed')
makedepends=('python2')
install='ca-certificates.install'
backup=('etc/ca-certificates.conf')
sha256sums=('30069a2a39ddd0cda32851a8292d2f489e0bed40a64a19e6e384d4d37d9fc418')

build() {
	cd ${srcdir}/${pkgname}-${pkgver}
	sed 's|/usr/bin/python|/usr/bin/python2|g' -i mozilla/certdata2pem.py
	sed 's|python|python2|g' -i mozilla/Makefile
	make
}

package() {
	cd ${srcdir}/${pkgname}-${pkgver}
	install -d -m755 ${pkgdir}/{etc/ca-certificates/update.d,usr/{sbin,share/ca-certificates},etc/ssl/certs}
	make install DESTDIR=${pkgdir}
	install -D -m644 sbin/update-ca-certificates.8 ${pkgdir}/usr/share/man/man8/update-ca-certificates.8

	(
	echo "# Automatically generated by ${pkgname}-${pkgver}-${pkgrel}"
	echo "# see update-ca-certificates man page"
	echo "# "
	cd ${pkgdir}/usr/share/ca-certificates
	find . -name '*.crt' | sort | cut -b3-
	) > ${pkgdir}/etc/ca-certificates.conf
}
