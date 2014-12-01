# Maintainer: Pierre Schmitz <pierre@archlinux.de>

pkgbase=ca-certificates
pkgname=(ca-certificates-utils ca-certificates)
pkgver=20140923
pkgrel=7
pkgdesc='Common CA certificates'
arch=('any')
url='http://pkgs.fedoraproject.org/cgit/ca-certificates.git'
license=('GPL2')
depends=('sh' 'p11-kit')
makedepends=('asciidoc')
source=(update-ca-trust update-ca-trust.8.txt)
sha256sums=('a06d6bc3cb7021e7d414f899e481c86f7994fbed3861d3dff184ff516a86157c'
            '52f7067ced3771bfa315e4b2a4d078ba0502b663ed4d87a9a18558e14d4ed99e')

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
