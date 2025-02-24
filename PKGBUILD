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

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

_os="$( \
  uname  \
   -o)"
_man="true"
_systemd="true"
_sensors="true"
if [[ "${_os}" == "Android" ]]; then
  _systemd="false"
  _sensors="false"
fi
_pkg=sysstat
pkgname="${_pkg}"
pkgver=12.7.7
pkgrel=1
_pkgdesc=(
  "Collection of performance monitoring tools"
  "(iostat, isag, mpstat, pidstat, sadf, sar)."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'i686'
  'mips'
  'powerpc'
)
url="https://${_pkg}.github.io"
license=(
  'GPL'
)
depends=(
)
if [[ "${_sensors}" == "true" ]]; then
  depends+=(
    'lm_sensors'
  )
fi
makedepends=(
)
if [[ "${_systemd}" == "true" ]]; then
  makedepends+=(
    'systemd'
  )
fi
optdepends=(
  'tk: to use isag'
  'gnuplot: to use isag'
)
options=(
  'zipman'
)
backup=(
  "etc/conf.d/${_pkg}"
  "etc/conf.d/${_pkg}.ioconf"
)
_http="https://github.com"
_ns="${_pkg}"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${_url}/archive/refs/tags/v${pkgver}.tar.gz"
  'lib64-fix.patch'
)
sha512sums=(
  '38e6bdc05244ccd3e875d47b792e6e22930bb2cd8966918e10db09278a610234930115b2856a84333152c59aa4b4c791d8eec681cf14f006257d06bd008568f2'
  '6caae9962a636e5152b0462d0dce835d8f658848723bee2d7ac903a0f170a491882c819457c882bcee41159960fa243fb843a2389c9a4ceeea061a5520e01103'
)

prepare() {
  cd \
    "${srcdir}/${pkgname}-${pkgver}"
  if [[ "${_man}" == "false" ]]; then
    rm \
      -rf \
      "man/"*
  fi
  if [[ "${CARCH}" == "x86_64" ]] || \
     [[ "${CARCH}" == "aarch64" ]]; then
    patch \
      -p1 < \
      "${srcdir}/lib64-fix.patch"
  fi
  autoreconf
}

build() {
  local \
    _configure_opts=() \
    _configure_vars=() \
    _man_group
   _configure_vars=(
     conf_dir=/etc/conf.d
  )
  _configure_opts=(
    --prefix="/usr"
    --mandir="/usr/share/man"
    --enable-install-cron
    --enable-copy-only
    # --disable-compress-manpg
  )
  if [[ "${_os}" == "Android" ]]; then
    _man_group="$( \
      id \
        -g)"
    _configure_vars+=(
      man_group="${_man_group}"
    )
    _configure_opts+=(
      --disable-file-attr
    )
  fi
  export \
    "${_configure_vars[@]}"
  if [[ "${_sensors}" == "false" ]]; then
    _configure_opts+=(
      --disable-sensors
    )
  fi
  cd \
    "${srcdir}/${_pkg}-${pkgver}"
  ./configure \
    "${_configure_opts[@]}"
  # patch makefile for FULL RELRO builds
  sed \
    -e \
      's|CFLAGS = |CFLAGS += \$(CPPFLAGS) |' \
    -e \
      's|LFLAGS = |LFLAGS = $(LDFLAGS) |' \
    -i \
    "Makefile"
  make
}

package() {
  cd \
    "${srcdir}/${_pkg}-${pkgver}"
  mkdir \
    -p \
    "${pkgdir}/usr/lib/systemd/system"
  make \
    DESTDIR="${pkgdir}" \
    PREFIX="/usr" \
    install
  if [[ "${_os}" != "Android" ]]; then
    chown \
      -R \
      "root:root" \
      "${pkgdir}"
  fi
  rm \
    -rf \
    "${pkgdir}/etc/rc"*
}

# vim: ft=sh syn=sh et
