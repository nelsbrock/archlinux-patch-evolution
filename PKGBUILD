# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(evolution evolution-bogofilter evolution-spamassassin)
pkgver=3.20.5
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://wiki.gnome.org/Apps/Evolution"
arch=(i686 x86_64)
license=(GPL)
depends=(gnome-desktop evolution-data-server libcanberra libpst libytnef
         dconf gtkspell3 libcryptui webkitgtk)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter
             spamassassin highlight gnome-common gtk-doc)
options=(!emptydirs)
source=(https://download.gnome.org/sources/$pkgbase/${pkgver:0:4}/$pkgbase-$pkgver.tar.xz)
sha256sums=('2e13551ce0996963506f0bdde5e01c3b8aa0622849a272ff12877cd595baeb6e')

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
  optdepends=('highlight: text highlight plugin'
              'evolution-spamassassin: Spamassassin spam check plugin'
              'evolution-bogofilter: Bogofilter spam check plugin')
  groups=(gnome-extra)

  cd $pkgbase-$pkgver
  make DESTDIR="$pkgdir" install

  mkdir ../plugins
  mv "$pkgdir/usr/lib/evolution/modules/module-bogofilter.so" ../plugins/
  mv "$pkgdir/usr/lib/evolution/modules/module-spamassassin.so" ../plugins/
  mv "$pkgdir/usr/share/appdata/evolution-bogofilter.metainfo.xml" ../plugins/
  mv "$pkgdir/usr/share/appdata/evolution-spamassassin.metainfo.xml" ../plugins/
}


package_evolution-bogofilter() {
  pkgdesc="Spam filtering for Evolution, using Bogofilter"
  depends=("evolution=$pkgver" bogofilter)

  install -Dm755 plugins/module-bogofilter.so "$pkgdir/usr/lib/evolution/modules/module-bogofilter.so"
  install -Dm644 plugins/evolution-bogofilter.metainfo.xml "$pkgdir/usr/share/appdata/evolution-bogofilter.metainfo.xml"
}

package_evolution-spamassassin() {
  pkgdesc="Spam filtering for Evolution, using SpamAssassin"
  depends=("evolution=$pkgver" spamassassin)

  install -Dm755 plugins/module-spamassassin.so "$pkgdir/usr/lib/evolution/modules/module-spamassassin.so"
  install -Dm644 plugins/evolution-spamassassin.metainfo.xml "$pkgdir/usr/share/appdata/evolution-spamassassin.metainfo.xml"
}
