# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgbase=ca-certificates
pkgname=(ca-certificates-utils ca-certificates)
pkgver=20140923
pkgrel=4
pkgdesc='Common CA certificates'
arch=('any')
url='http://pkgs.fedoraproject.org/cgit/ca-certificates.git'
license=('GPL2')
depends=('sh' 'p11-kit')
makedepends=('asciidoc')
source=(update-ca-trust update-ca-trust.8.txt)
sha256sums=('c737236405320440665c9e7034585fde95d34495fc65789591a7ab76dc7568d5'
            '85e29428788bcb40136eecd7b05fa5e73f73f1468fa96da42991122ba39eb784')

_confdir=/etc/$pkgbase
_datadir=/usr/share/$pkgbase

build() {
	asciidoc.py -v -d manpage -b docbook update-ca-trust.8.txt
	xsltproc --nonet -o update-ca-trust.8 /etc/asciidoc/docbook-xsl/manpage.xsl update-ca-trust.8.xml
}

package_ca-certificates-utils() {
	pkgdesc+=" (utilities)"
	install=ca-certificates-utils.install
	provides=(ca-certificates ca-certificates-java)
	conflicts=(ca-certificates-java)
	replaces=(ca-certificates-java)

	install -D update-ca-trust "${pkgdir}/usr/bin/update-ca-trust"
	install -Dm644 update-ca-trust.8 "${pkgdir}/usr/share/man/man8/update-ca-trust.8"

	install -d "${pkgdir}"{${_confdir},${_datadir}}/trust-source/{anchors,blacklist}

	_extractdir="${pkgdir}${_confdir}/extracted"
	_ssldir="${pkgdir}/etc/ssl"

	install -d "${_ssldir}/certs/java" "${_extractdir}"/{openssl,pem,java}
	ln -sr "${_extractdir}/openssl/ca-bundle.trust.crt" "${_ssldir}/certs/ca-bundle.trust.crt"
	ln -sr "${_extractdir}/pem/tls-ca-bundle.pem" "${_ssldir}/cert.pem"

	# These are inverted (for now?) to ease upgrading
	ln -sr "${_ssldir}/certs/ca-certificates.crt" "${_extractdir}/pem/tls-ca-bundle.pem"
	ln -sr "${_ssldir}/certs/java/cacerts" "${_extractdir}/java/cacerts" 
}

package_ca-certificates() {
	pkgdesc+=" (default providers)"
	depends=(ca-certificates-{mozilla,cacert})
}

# vim:set noet ts=8 sw=8:
