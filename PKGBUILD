# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgbase=ca-certificates
pkgname=(ca-certificates-utils ca-certificates)
pkgver=20140923
pkgrel=10
pkgdesc='Common CA certificates'
arch=('any')
url='http://pkgs.fedoraproject.org/cgit/ca-certificates.git'
license=('GPL2')
makedepends=('asciidoc' 'p11-kit')
source=(update-ca-trust update-ca-trust.8.txt)
sha256sums=('6337e487cdf691d33a3c07c894f133b280093a2c60d267a534b56c316c5d7370'
            '916bc0582b5230aedc7fdada70bc387fbd605dbc9d3d421621e4736c60a8ef41')

build() {
	asciidoc.py -v -d manpage -b docbook update-ca-trust.8.txt
	xsltproc --nonet -o update-ca-trust.8 /etc/asciidoc/docbook-xsl/manpage.xsl update-ca-trust.8.xml
}

package_ca-certificates-utils() {
	pkgdesc+=" (utilities)"
	depends=('bash' 'coreutils' 'findutils' 'p11-kit>=0.23.1')
	install=ca-certificates-utils.install
	provides=(ca-certificates ca-certificates-java)
	conflicts=(ca-certificates-java)
	replaces=(ca-certificates-java)

	install -D update-ca-trust "${pkgdir}/usr/bin/update-ca-trust"
	install -Dm644 update-ca-trust.8 "${pkgdir}/usr/share/man/man8/update-ca-trust.8"

	# Trust source directories
	install -d "${pkgdir}"/{etc,usr/share}/${pkgbase}/trust-source/{anchors,blacklist}

	# Directories used by update-ca-trust (aka "trust extract-compat")
	install -d "${pkgdir}"/etc/{ssl/certs/java,${pkgbase}/extracted}

	# Contrary to Fedora, the /etc/ssl/certs dir is generated
	ln -srT "${pkgdir}"/etc/ssl/{certs/ca-certificates.crt,cert.pem}
}

package_ca-certificates() {
	pkgdesc+=" (default providers)"
	depends=(ca-certificates-{mozilla,cacert})
}

# vim:set noet ts=8 sw=8:
