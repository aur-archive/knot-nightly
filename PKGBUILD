# Maintainer:	Ondřej Surý <ondrej@sury.
# Contributor:	Oleander Reis <oleander@oleander.cc>
# Contributor:	Otto Sabart <seberm[at]gmail[dot]com>

pkgname=knot-nightly
_gitname=knot
pkgver=2.0.0_beta_185_g0cddbbc
pkgrel=1
pkgdesc='high-performance authoritative-only DNS server (nightly)'
url='https://www.knot-dns.cz/'
arch=('i686' 'x86_64')
license=('GPL3')
install=install
depends=('liburcu>=0.5.4' 'openssl>=1.0.0' 'zlib' 'liblmdb' 'fstrm' 'protobuf-c')
makedepends=('autoconf>=2.65' 'automake' 'libtool' 'flex>=2.5.3' 'bison>=2.3' 'git')
source=("git+git://git.nic.cz/knot.git"
        'knot.service')
sha256sums=(SKIP
            'caa870a9c93c57c6311f9e8fb5685a9179bb9839a27a30cc1712c91df0d15090')

pkgver() {
	cd "${srcdir}/${_gitname}"
	git describe | sed -e 's/^v//;s/-/_/g'
}

build() {
	cd "${srcdir}/${_gitname}"
	autoreconf -fi
	./configure \
		--prefix /usr \
		--sysconfdir=/etc \
		--localstatedir=/var/lib \
		--libexecdir=/usr/lib/knot \
		--with-rundir=/run/knot \
		--with-storage=/var/lib/knot \
		--enable-recvmmsg=yes \
		--enable-dnstap \
		--disable-silent-rules

	make
}

package() {
	cd "${srcdir}/${_gitname}"

	make DESTDIR="${pkgdir}/" install
	install -Dm 644 "${srcdir}/knot.service" "${pkgdir}/usr/lib/systemd/system/knot.service"
}

check() {
	cd "${srcdir}/${_gitname}"
	make check
}
