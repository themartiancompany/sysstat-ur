# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Martin Devera <devik@cdi.cz>

pkgname=sysstat
pkgver=9.0.6
pkgrel=1
pkgdesc="A collection of performance monitoring tools"
arch=('i686' 'x86_64')
url="http://pagesperso-orange.fr/sebastien.godard/"
license=('GPL')
depends=('glibc')
optdepends=('tk: to use isag'
	    'gnuplot: to use isag')
options=(zipman)
source=(http://pagesperso-orange.fr/sebastien.godard/$pkgname-$pkgver.tar.gz
        sysstat)
md5sums=('c05ca01878a4069199d9af93cbe39c8e'
         'ad46159609a2c13b4a46b506ff847bf6')

build() {
  cd $srcdir/$pkgname-$pkgver

  ./configure --prefix=/usr \
	--mandir=/usr/share/man \
	--enable-install-isag \
	--disable-man-group
  make || return 1
  make DESTDIR=$pkgdir install || return 1

  install -D -m 644 sysstat.sysconfig $pkgdir/etc/sysstat/sysstat && \
  install -D -m 744 sysstat.cron.hourly $pkgdir/etc/cron.hourly/sysstat && \
  install -D -m 744 sysstat.cron.daily $pkgdir/etc/cron.daily/sysstat && \
  install -D -m 755 $srcdir/sysstat $pkgdir/etc/rc.d/sysstat || return 1

  chown -R root:root $pkgdir
}
