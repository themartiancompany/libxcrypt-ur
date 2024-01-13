# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Christian Hesse <mail@eworm.de>
# Contributor: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Contributor: Truocolo <truocolo@aol.com>

_py="python"
pkgbase=libxcrypt
pkgname=(
  "${pkgbase}"
  "${pkgbase}-compat"
)
pkgver=4.4.36
pkgrel=1
pkgdesc='Modern library for one-way hashing of passwords'
arch=(
  'x86_64'
  'arm'
  'armv7h'
  'aarch64'
  'pentium4'
  'i686'
  'powerpc'
)
url="https://github.com/besser82/${pkgbase}"
license=(
  'LGPL'
)
depends=(
  'glibc'
)
makedepends=(
  "gcc"
)
optdepends=(
  "${_py}-passlib: to enable ka-table option"
)
provides=(
  'libcrypt.so'
)
install="${pkgbase}.install"
validpgpkeys=(
  # Björn 'besser82' Esser
  '678CE3FEE430311596DB8C16F52E98007594C21D'
  )
source=(
  "${url}/releases/download/v${pkgver}/${pkgbase}-${pkgver}.tar.xz"{,.asc}
)
sha256sums=(
  'e5e1f4caee0a01de2aee26e3138807d6d3ca2b8e67287966d1fefd65e1fd8943'
  'SKIP'
)

build() {
  mkdir \
    "build-${pkgbase}" \
    "build-${pkgbase}-compat"

  cd \
    "${srcdir}/build-${pkgbase}"
  "${srcdir}/${pkgbase}-${pkgver}"/configure \
    --prefix=/usr \
    --disable-static \
    --enable-hashes=strong,glibc \
    --enable-obsolete-api=no \
    --disable-failure-tokens
  make
  
  cd \
    "${srcdir}/build-${pkgbase}-compat"
  "${srcdir}/${pkgbase}-${pkgver}"/configure \
    --prefix=/usr \
    --disable-static \
    --enable-hashes=strong,glibc \
    --enable-obsolete-api=glibc \
    --disable-failure-tokens
  make
}

check() {
  cd \
    "build-${pkgbase}"
  make \
    check 
}

package_libxcrypt() {
  cd \
    "build-${pkgbase}"
  make \
    DESTDIR="${pkgdir}" \
    install
}

package_libxcrypt-compat() {
  pkgdesc='Modern library for one-way hashing of passwords - legacy API functions'
  depends=(
    "${pkgbase}"
  )
  cd \
    "build-${pkgbase}-compat"

  make \
    DESTDIR="${pkgdir}" \
    install

  rm \
    -rf \
    "${pkgdir}"/usr/{include,lib/{lib*.so,pkgconfig},share}
}

# vim: ft=sh syn=sh et
