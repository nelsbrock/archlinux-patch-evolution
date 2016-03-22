# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=('evolution' 'evolution-bogofilter' 'evolution-spamassassin')
pkgver=3.20.0
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
arch=(i686 x86_64)
license=(GPL)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter spamassassin highlight gnome-desktop evolution-data-server libcanberra libpst libytnef desktop-file-utils dconf gtkspell3 webkitgtk libcryptui gnome-common gtk-doc)
options=('!emptydirs')
url=https://wiki.gnome.org/Apps/Evolution
source=(https://download.gnome.org/sources/$pkgbase/${pkgver:0:4}/$pkgbase-$pkgver.tar.xz)
sha256sums=('6bd0cc0c077363a29026a5194062d981d0b0aa9ffea9e7ffcec1f1fdbe9b502e')

prepare() {
  cd $pkgname-$pkgver
}

build() {
  cd $pkgbase-$pkgver

  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --disable-schemas-compile \
      --disable-autoar \
      --enable-gtk-doc
  
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package_evolution() {
  depends=(gnome-desktop evolution-data-server libcanberra libpst libytnef
           desktop-file-utils dconf gtkspell3 libcryptui webkitgtk)
  install=evolution.install
  optdepends=('highlight: text highlight plugin'
              'evolution-spamassassin: Spamassassin spam check plugin'
              'evolution-bogofilter: Bogofilter spam check plugin')
  groups=('gnome-extra')

  cd $pkgbase-$pkgver
  make DESTDIR="$pkgdir" install

  mkdir ../plugins
  mv "$pkgdir/usr/lib/evolution/modules/module-bogofilter.so" ../plugins/
  mv "$pkgdir/usr/lib/evolution/modules/module-spamassassin.so" ../plugins/
  mv "$pkgdir/usr/share/appdata/evolution-bogofilter.metainfo.xml" ../plugins/
  mv "$pkgdir/usr/share/appdata/evolution-spamassassin.metainfo.xml" ../plugins/
}


package_evolution-bogofilter() {
  depends=("evolution=$pkgver" 'bogofilter')
  install -Dm755 plugins/module-bogofilter.so "$pkgdir/usr/lib/evolution/modules/module-bogofilter.so"
  install -Dm644 plugins/evolution-bogofilter.metainfo.xml "$pkgdir/usr/share/appdata/evolution-bogofilter.metainfo.xml"
}

package_evolution-spamassassin() {
  depends=("evolution=$pkgver" 'spamassassin')
  install -Dm755 plugins/module-spamassassin.so "$pkgdir/usr/lib/evolution/modules/module-spamassassin.so"
  install -Dm644 plugins/evolution-spamassassin.metainfo.xml "$pkgdir/usr/share/appdata/evolution-spamassassin.metainfo.xml"
}
