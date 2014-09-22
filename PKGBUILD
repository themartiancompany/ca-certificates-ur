# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgbase=ca-certificates
pkgname=(ca-certificates-utils ca-certificates)
pkgver=20140923
pkgrel=2
pkgdesc='Common CA certificates'
arch=('any')
url='http://pkgs.fedoraproject.org/cgit/ca-certificates.git'
license=('GPL2')
depends=('sh' 'p11-kit')
makedepends=('asciidoc')
_commit=f81c301
source=("update-ca-trust::$url/plain/update-ca-trust?id=$_commit"
        "update-ca-trust.8.txt::$url/plain/update-ca-trust.8.txt?id=$_commit")
sha256sums=('75ef2f4b0fddd2ca3c69b234a6abb66fd732e4af96814b65dcedb0dd52018381'
            'd31ac2bb5f1941aea0ac1e51861af7be224b6bb85820e30bb30793112aa785ba')

_confdir=/etc/$pkgbase
_datadir=/usr/share/$pkgbase

prepare() {
	sed -i "s:/etc/pki/ca-trust:${_confdir}:g" update-ca-trust
}

package_ca-certificates-utils() {
	pkgdesc+=" (utilities)"
	install=ca-certificates-utils.install
	provides=(ca-certificates ca-certificates-java)
	conflicts=(ca-certificates-java)
	replaces=(ca-certificates-java)

	asciidoc.py -v -d manpage -b docbook update-ca-trust.8.txt
	xsltproc --nonet -o update-ca-trust.8 /etc/asciidoc/docbook-xsl/manpage.xsl update-ca-trust.8.xml

	install -D update-ca-trust "${pkgdir}/usr/bin/update-ca-trust"
	install -Dm644 update-ca-trust.8 "${pkgdir}/usr/share/man/man8/update-ca-trust.8"

	install -d "${pkgdir}"{${_confdir},${_datadir}}/trust-source/{anchors,blacklist}

	_extractdir="${pkgdir}${_confdir}/extracted"
	_ssldir="${pkgdir}/etc/ssl"

	install -d "${_ssldir}/certs/java" "${_extractdir}"/{openssl,pem,java}
	ln -sr "${_extractdir}/openssl/ca-bundle.trust.crt" "${_ssldir}/certs/ca-bundle.trust.crt"
	ln -sr "${_extractdir}/pem/tls-ca-bundle.pem" "${_ssldir}/certs/ca-bundle.crt"
	ln -sr "${_extractdir}/pem/tls-ca-bundle.pem" "${_ssldir}/cert.pem"
	ln -sr "${_extractdir}/java/cacerts" "${_ssldir}/certs/java/cacerts"
}

package_ca-certificates() {
	pkgdesc+=" (default providers)"
	depends=(ca-certificates-{mozilla,cacert})
}

# vim:set noet ts=8 sw=8:
