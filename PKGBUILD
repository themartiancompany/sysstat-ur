# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=10.1.6
pkgrel=1
pkgdesc="a collection of performance monitoring tools (iostat,isag,mpstat,pidstat,sadf,sar)"
arch=('i686' 'x86_64')
url="http://pagesperso-orange.fr/sebastien.godard/"
license=('GPL')
depends=('lm_sensors')
optdepends=('tk: to use isag'
	    'gnuplot: to use isag')
options=(zipman)
backup=('etc/conf.d/sysstat'
	'etc/conf.d/sysstat.ioconf')
source=(http://pagesperso-orange.fr/sebastien.godard/$pkgname-$pkgver.tar.gz
	sysstat.service)
md5sums=('909135c84e36d780d501274e7450f924'
         '12ba479c606620193e8b7c6e982d5088')

build() {
  cd $srcdir/$pkgname-$pkgver
  conf_dir=/etc/conf.d ./configure --prefix=/usr \
	--enable-yesterday \
	--mandir=/usr/share/man \
	--enable-install-isag \
	--disable-man-group
  make -j1
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make DESTDIR=$pkgdir install
  install -D -m 744 cron/sysstat.cron.hourly $pkgdir/etc/cron.hourly/sysstat
  install -D -m 744 cron/sysstat.cron.daily $pkgdir/etc/cron.daily/sysstat
  chown -R root:root $pkgdir
  install -Dm0644 $srcdir/$pkgname.service $pkgdir/usr/lib/systemd/system/$pkgname.service
  mv $pkgdir/usr/bin/nfsiostat $pkgdir/usr/bin/$pkgname-nfsiostat
}
