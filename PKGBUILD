# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=11.7.3
pkgrel=1
pkgdesc="a collection of performance monitoring tools (iostat,isag,mpstat,pidstat,sadf,sar)"
arch=('x86_64')
url="http://pagesperso-orange.fr/sebastien.godard/"
license=('GPL')
depends=('lm_sensors')
makedepends=('systemd')
optdepends=('tk: to use isag'
	    'gnuplot: to use isag')
options=('zipman')
backup=('etc/conf.d/sysstat'
	'etc/conf.d/sysstat.ioconf')
source=("http://pagesperso-orange.fr/sebastien.godard/${pkgname}-${pkgver}.tar.xz"
	      'lib64-fix.patch')
sha512sums=('c752ebba4d25a30147e68d0cb44d93020bb9b44823812c99550f6b34b42fc6f015c28ef0e6609a1e33ae6e4de566ae2d0e8a4711e412a4298baf5721dd15d27f'
            '46ec3eebb12232d30cddba60f16a57cd8d625513cf002d9e501797a6660f9da9cb4116ec81d0c292644fb6d91eb05c7be458da667260b238bcfef532a020b114')

prepare() {
  cd "${srcdir}"/"${pkgname}"-"${pkgver}"
  patch -p1 < "${srcdir}"/lib64-fix.patch
  autoreconf
}

build() {
  cd "${srcdir}"/"${pkgname}"-"${pkgver}"
  conf_dir=/etc/conf.d ./configure --prefix=/usr \
	--enable-yesterday \
	--mandir=/usr/share/man \
	--enable-install-isag \
	--enable-install-cron \
	--enable-copy-only \
	--disable-man-group
  make
}

package() {
  cd "${srcdir}"/"${pkgname}"-"${pkgver}"
  mkdir -p "${pkgdir}"/usr/lib/systemd/system
  make DESTDIR="${pkgdir}" install
  chown -R root:root "${pkgdir}"
  rm -rf "${pkgdir}"/etc/rc*
}
