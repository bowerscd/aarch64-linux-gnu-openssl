_target=aarch64-linux-gnu
_pkgbase=openssl
pkgname="${_target}-${_pkgbase}"
_ver=1.1.1g
# use a pacman compatible version scheme
pkgver=${_ver/[a-z]/.${_ver//[0-9.]/}}
pkgrel=2
pkgdesc='The Open Source toolkit for Secure Sockets Layer and Transport Layer Security'
arch=('arm64' 'x86_64')
url='https://www.openssl.org'
license=('custom:BSD')
source=("https://www.openssl.org/source/${_pkgbase}-${_ver}.tar.gz"{,.asc})
sha256sums=('ddb04774f1e32f0c49751e21b67216ac87852ceb056b75209af2443400636d46'
	    'b795163edbb1334faa2218f9576242f52623bf0d8c44e0a2527df8261e67bcd9')
validpgpkeys=('SKIP')

_sysroot="/usr/${_target}"

build() {
	cd "$srcdir/${_pkgbase}-$_ver"

	export CPPFLAGS=$(echo "${CPPFLAGS}" | sed -e "s/-march=x86-64//g")
	export CFLAGS=$(echo "${CFLAGS}" | sed -e "s/-march=x86-64//g")

	# mark stack as non-executable: http://bugs.archlinux.org/task/12434
	./Configure --prefix=${_sysroot} --openssldir=/etc/ssl --libdir=lib \
		shared no-ssl3-method linux-aarch64 --cross-compile-prefix="${_target}-" \
		"-Wa,--noexecstack ${CPPFLAGS} ${CFLAGS} ${LDFLAGS}"

	make depend
	make
}

package() {
	cd "$srcdir/${_pkgbase}-$_ver"
	make DESTDIR=$pkgdir install_sw
	install -D -m644 LICENSE "$pkgdir"/"${_sysroot}"/share/licenses/${_pkgbase}/LICENSE
}
