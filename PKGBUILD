# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=11.4.0
pkgrel=1
pkgdesc="a collection of performance monitoring tools (iostat,isag,mpstat,pidstat,sadf,sar)"
arch=('i686' 'x86_64')
url="http://pagesperso-orange.fr/sebastien.godard/"
license=('GPL')
depends=('lm_sensors')
makedepends=('systemd')
optdepends=('tk: to use isag'
	    'gnuplot: to use isag')
options=(zipman)
backup=('etc/conf.d/sysstat'
	'etc/conf.d/sysstat.ioconf')
source=(http://pagesperso-orange.fr/sebastien.godard/$pkgname-$pkgver.tar.xz
	lib64-fix.patch)
md5sums=('ee7b8c0b139a11a7a3418dc0658ca1be'
         '5c7acc9a31b7724b8815daffae219de1')

prepare() {
  cd "$srcdir"/$pkgname-$pkgver
  patch -p1 <"$srcdir"/lib64-fix.patch
  autoreconf
}

build() {
  cd "$srcdir"/$pkgname-$pkgver
  conf_dir=/etc/conf.d ./configure --prefix=/usr \
	--enable-yesterday \
	--mandir=/usr/share/man \
	--enable-install-isag \
	--enable-install-cron \
	--enable-copy-only \
	--disable-man-group
  make -j1
}

package() {
  cd "$srcdir"/$pkgname-$pkgver
  mkdir -p "$pkgdir"/usr/lib/systemd/system
  make DESTDIR="$pkgdir" install
  chown -R root:root "$pkgdir"
  rm -rf "$pkgdir"/etc/rc*
}
