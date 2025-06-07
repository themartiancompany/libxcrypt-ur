# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer:
#   Christian Hesse
#     <mail@eworm.de>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _cc="clang"
  _libc="ndk-sysroot"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _cc="gcc"
  _libc="glibc"
fi
_py="python"
_pkg=libxcrypt
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
  "${_pkg}-compat"
)
pkgver=4.4.36
pkgrel=1
_pkgdesc=(
  'Modern library for one-way'
  'hashing of passwords.'
)
arch=(
  'arm'
  'armv6l'
  'armv7h'
  'armv7l'
  'aarch64'
  'i686'
  'mips'
  'pentium4'
  'powerpc'
  'x86_64'
)
_http="https://github.com"
_ns="besser82"
url="${_http}/${_ns}/${_pkg}"
license=(
  'LGPL'
)
depends=(
  "${_libc}"
)
makedepends=(
  "${_cc}"
)
_passlib_optdepends=(
  "${_py}-passlib:"
    "to enable ka-table option."
)
optdepends=(
  "${_passlib_optdepends[*]}"
)
provides=(
  "libcrypt"
  "libcrypt.so"
)
conflicts=(
  "libcrypt"
  "libcrypt.so"
)
install="${_pkg}.install"
validpgpkeys=(
  # Björn 'besser82' Esser
  '678CE3FEE430311596DB8C16F52E98007594C21D'
)
_tag="${pkgver}"
_tag_name="pkgver"
_tarname="${_pkg}-${_tag}"
source=(
  "${url}/releases/download/v${pkgver}/${_tarname}.tar.xz"{"",".asc"}
)
sha256sums=(
  'e5e1f4caee0a01de2aee26e3138807d6d3ca2b8e67287966d1fefd65e1fd8943'
  'SKIP'
)

build() {
  local \
    _configure_opts=()
  _configure_opts+=(
    --prefix="/usr"
    --disable-static
    --enable-hashes="strong,glibc"
    --disable-failure-tokens
  )
  mkdir \
    "build-${_pkg}" \
    "build-${_pkg}-compat"
  cd \
    "${srcdir}/build-${_pkg}"
  "${srcdir}/${_tarname}/configure" \
    "${_configure_opts[@]}" \
    --enable-obsolete-api="no"
  make
  cd \
    "${srcdir}/build-${_pkg}-compat"
  "${srcdir}/${_tarname}/configure" \
    "${_configure_opts[@]}" \
    --enable-obsolete-api="glibc"
  make
}

check() {
  cd \
    "build-${_pkg}"
  make \
    check 
}

package_libxcrypt() {
  cd \
    "build-${_pkg}"
  make \
    DESTDIR="${pkgdir}" \
    install
}

package_libxcrypt-compat() {
  local \
    _pkgdesc=()
  _pkgdesc+=(
    'Modern library for one-way'
    'hashing of passwords'
    '- legacy API functions.'
  )
  pkgdesc="${_pkgdesc[*]}"
  depends=(
    "${_pkg}"
  )
  cd \
    "build-${_pkg}-compat"
  make \
    DESTDIR="${pkgdir}" \
    install
  rm \
    -rf \
    "${pkgdir}/usr/"{"include","lib/"{"lib"*".so","pkgconfig"},"share"}
}

# vim: ft=sh syn=sh et
