# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgname=ca-certificates
pkgver=20140223
pkgrel=2
pkgdesc='Common CA certificates'
arch=('any')
url='http://packages.qa.debian.org/c/ca-certificates.html'
license=('MPL' 'GPL')
source=("http://ftp.debian.org/debian/pool/main/c/${pkgname}/${pkgname}_${pkgver}.tar.xz")
depends=('bash' 'run-parts' 'openssl' 'findutils' 'coreutils' 'sed')
makedepends=('python2')
install='ca-certificates.install'
backup=('etc/ca-certificates.conf')
sha256sums=('815b7cd97200b0d76450bb3e7d9b65997ac494ab6467b17369f65b2ef94bcb0c')

prepare() {
	cd ${srcdir}/${pkgname}
	sed 's|/usr/bin/python|/usr/bin/python2|g' -i mozilla/certdata2pem.py
	sed 's|python|python2|g' -i mozilla/Makefile
}

build() {
	cd ${srcdir}/${pkgname}
	make SUBDIRS=mozilla
}

package() {
	cd ${srcdir}/${pkgname}
	install -d -m755 ${pkgdir}/{etc/ca-certificates/update.d,usr/{sbin,share/ca-certificates},etc/ssl/certs}
	make install SUBDIRS=mozilla DESTDIR=${pkgdir}
	mv ${pkgdir}/usr/sbin ${pkgdir}/usr/bin
	install -D -m644 sbin/update-ca-certificates.8 ${pkgdir}/usr/share/man/man8/update-ca-certificates.8

	(
	echo "# Automatically generated by ${pkgname}-${pkgver}-${pkgrel}"
	echo "# see update-ca-certificates man page"
	echo "# "
	cd ${pkgdir}/usr/share/ca-certificates
	find . -name '*.crt' | sort | cut -b3-
	) > ${pkgdir}/etc/ca-certificates.conf
}
