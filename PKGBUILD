# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Andrea Scarpino <andrea@archlinux.org>

_os="$( \
  uname \
    -o)"
_libc="glibc"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_pkg=libb64
pkgname="${_pkg}"
_majver="1.2"
_minver="1"
pkgver="${_majver}.${_minver}"
pkgrel=5
pkgdesc='Base64 Encoding/Decoding Routines'
url="http://${_pkg}.sourceforge.net"
depends=(
  "${_libc}"
)
arch=(
  'x86_64'
  'i686'
  'arm'
  'armv7l'
  'armv6l'
  'aarch64'
  'mips'
  'pentium4'
  'powerpc'
)
license=(
  'custom:Public Domain'
)
_sf_dl="https://downloads.sourceforge.net"
_deb_dl="https://sources.debian.net"
_bufsize_patch="bufsiz-as-buffer-size.diff"
_tarname="${_pkg}-${pkgver}"
source=(
  "${_sf_dl}/${_pkg}/${_tarname}.zip"
  "${_deb_dl}/data/main/libb/${_pkg}/${_majver}-3/debian/patches/${_bufsize_patch}"
)
sha256sums=(
  '20106f0ba95cfd9c35a13c71206643e3fb3e46512df3e2efb2fdbf87116314b2'
  '0a8a978e94a924b4f62795de251aa1a5ceb53a1024ea60193d7f3e5a5794fa20'
)

prepare() {
  cd \
    "${_tarname}"
  patch \
    -p1 \
    -i \
    "${srcdir}/${_bufsize_patch}"
}

_cc_get() {
  command \
    -v \
    "cc" \
    "clang" \
    "gcc" | \
    head \
      -n \
      1
}

build() {
  local \
    _cc
  _cc="$( \
    _cc_get)"
  cd \
    "${_tarname}/src"
  export \
    CFLAGS="${CFLAGS} -fPIC"
  make
  export \
    CFLAGS="${CFLAGS} -shared -Wl,-soname,${_pkg}.so.0"
  "${_cc}" \
    ${LDFLAGS} \
    ${CFLAGS} \
    *".o" \
    -o \
      "${_pkg}.so.0"
}

package() {
  cd \
    "${_tarname}"
  install \
    -dm755 \
    "${pkgdir}/usr/lib"
  install \
    "src/${_pkg}.so.0" \
    "${pkgdir}/usr/lib"
  ln \
    -sf \
    "/usr/lib/${_pkg}.so.0" \
    "${pkgdir}/usr/lib/${_pkg}.so"
  cp \
    -r \
    "include" \
    "${pkgdir}/usr"
  install \
    -Dm644 \
    "LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
