# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=11.4.6
pkgrel=1
pkgdesc="a collection of performance monitoring tools (iostat,isag,mpstat,pidstat,sadf,sar)"
arch=('x86_64')
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
sha256sums=('2bc1d98572169a901868afdf0c56f98580191353ba3bc5c7ce6efd12e7a7fa2e'
            'e81090f78fda0aed9e750bea5a3a0b7ef76b44ade558ed569827ffdb5cb62a89')

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
