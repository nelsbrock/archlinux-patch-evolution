# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=('evolution' 'evolution-bogofilter' 'evolution-spamassassin')
pkgver=3.12.6
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
arch=(i686 x86_64)
license=(GPL)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter spamassassin highlight gnome-desktop evolution-data-server gtkhtml4 libcanberra libpst libytnef psmisc desktop-file-utils hicolor-icon-theme dconf gtkspell3)
options=('!emptydirs')
url=https://wiki.gnome.org/Apps/Evolution
source=(http://ftp.gnome.org/pub/gnome/sources/$pkgbase/${pkgver:0:4}/$pkgbase-$pkgver.tar.xz)
sha256sums=('5f17a564a3c0351c1d0823551f80579ef611d51ab9db1e7f8d2ad7ed126bf76d')

build() {
  cd $pkgbase-$pkgver

  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --disable-schemas-compile
  
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0 /g' -e 's/    if test "$export_dynamic" = yes && test -n "$export_dynamic_flag_spec"; then/      func_append compile_command " -Wl,-O1,--as-needed"\n      func_append finalize_command " -Wl,-O1,--as-needed"\n\0/' libtool

  make
}

package_evolution() {
  depends=(gnome-desktop evolution-data-server gtkhtml4 libcanberra libpst libytnef psmisc
           desktop-file-utils hicolor-icon-theme dconf gtkspell3)
  install=evolution.install
  optdepends=('highlight: text highlight plugin'
              'evolution-spamassassin: Spamassassin spam check plugin'
              'evolution-bogofilter: Bogofilter spam check plugin')
  groups=('gnome-extra')

  cd $pkgbase-$pkgver
  make DESTDIR="$pkgdir" install

  mkdir ../plugins
  mv "$pkgdir/usr/lib/evolution/3.12/modules/module-bogofilter.so" ../plugins/
  mv "$pkgdir/usr/lib/evolution/3.12/modules/module-spamassassin.so" ../plugins/
}


package_evolution-bogofilter() {
  depends=("evolution=$pkgver" 'bogofilter')
  install -Dm755 plugins/module-bogofilter.so "$pkgdir/usr/lib/evolution/3.12/modules/module-bogofilter.so"
}

package_evolution-spamassassin() {
  depends=("evolution=$pkgver" 'spamassassin')
  install -Dm755 plugins/module-spamassassin.so "$pkgdir/usr/lib/evolution/3.12/modules/module-spamassassin.so"
}
