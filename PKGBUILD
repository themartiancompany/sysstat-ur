# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=10.1.1
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
	sysstat)
md5sums=('8250cdcbc4a959c8a05e4186fbd13d84'
         '3ce41ebf7330aba01e70b38658afed1f')

build() {
  cd $srcdir/$pkgname-$pkgver
  conf_dir=/etc/conf.d ./configure --prefix=/usr \
	--enable-yesterday \
	--mandir=/usr/share/man \
	--enable-install-isag \
	--disable-man-group
  make
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make DESTDIR=$pkgdir install
  install -D -m 744 cron/sysstat.cron.hourly $pkgdir/etc/cron.hourly/sysstat
  install -D -m 744 cron/sysstat.cron.daily $pkgdir/etc/cron.daily/sysstat
  install -D -m 755 $srcdir/sysstat $pkgdir/etc/rc.d/sysstat
  chown -R root:root $pkgdir
}
