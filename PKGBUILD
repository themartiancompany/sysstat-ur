# SPDX-License-Identifier: AGPL-3.0
# 
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

_os="$( \
  uname  \
   -o)"
_man="true"
_systemd="true"
_sensors="true"
[[ "${_os}" == "Android" ]] && \
  _systemd="false" && \
  _sensors="false" && \
_pkg=sysstat
pkgname="${_pkg}"
pkgver=12.7.5
pkgrel=1
_pkgdesc=(
  "Collection of performance monitoring tools"
  "(iostat,isag,mpstat,pidstat,sadf,sar)"
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
[[ "${_sensors}" == "true" ]] && \
  depends+=(
    'lm_sensors'
  )
makedepends=(
)
[[ "${_systemd}" == "true" ]] && \
  makedepends+=(
    'systemd'
  )
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
  '97a96023b3d7e579184e1394b7a1ff8cc09654359a9f103ba954464aae8f31dbac275ff238e24cce57aa83a212bf18d91734f3f77f7c0ebc9c18b8958730753e'
  '6caae9962a636e5152b0462d0dce835d8f658848723bee2d7ac903a0f170a491882c819457c882bcee41159960fa243fb843a2389c9a4ceeea061a5520e01103'
)

prepare() {
  cd \
    "${srcdir}/${pkgname}-${pkgver}"
  [[ "${_man}" == "false" ]] && \
    rm \
      -rf \
      "man/"*
  [[ "${CARCH}" == "x86_64" ]] || \
  [[ "${CARCH}" == "aarch64" ]] && \
    patch \
      -p1 < \
      "${srcdir}/lib64-fix.patch"
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
    --prefix=/usr
    --mandir=/usr/share/man
    --enable-install-cron
    --enable-copy-only
    # --disable-compress-manpg
  )
  [[ "${_os}" == "Android" ]] && \
    _man_group="$( \
      id \
        -g)" && \
    _configure_vars+=(
      man_group="${_man_group}"
    ) && \
    _configure_opts+=(
      --disable-file-attr
    )
  export \
    "${_configure_vars[@]}"
  [[ "${_sensors}" == "false" ]] && \
    _configure_opts+=(
      --disable-sensors
    )
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
      Makefile
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
    install
  [[ "${_os}" != "Android" ]] && \
    chown \
      -R \
      root:root \
      "${pkgdir}"
  rm \
    -rf \
    "${pkgdir}/etc/rc"*
}

# vim: ft=sh syn=sh et
