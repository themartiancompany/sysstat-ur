# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=10.0.0
pkgrel=1
pkgdesc="a collection of performance monitoring tools (iostat,isag,mpstat,pidstat,sadf,sar)"
arch=('i686' 'x86_64')
url="http://pagesperso-orange.fr/sebastien.godard/"
license=('GPL')
depends=('glibc')
optdepends=('tk: to use isag'
	    'gnuplot: to use isag'
	    'lm_sensors')
options=(zipman)
source=(http://pagesperso-orange.fr/sebastien.godard/$pkgname-$pkgver.tar.gz
        sysstat)
md5sums=('accc17cc1fa855be6b27def77dd92a0d'
         'ad46159609a2c13b4a46b506ff847bf6')

build() {
  cd $srcdir/$pkgname-$pkgver

  ./configure --prefix=/usr \
	--mandir=/usr/share/man \
	--enable-install-isag \
	--disable-man-group
  make
  make DESTDIR=$pkgdir install

  install -D -m 644 sysstat.sysconfig $pkgdir/etc/sysstat/sysstat
  install -D -m 744 cron/sysstat.cron.hourly $pkgdir/etc/cron.hourly/sysstat
  install -D -m 744 cron/sysstat.cron.daily $pkgdir/etc/cron.daily/sysstat
  install -D -m 755 $srcdir/sysstat $pkgdir/etc/rc.d/sysstat

  chown -R root:root $pkgdir
}
