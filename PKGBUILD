# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(evolution evolution-bogofilter evolution-spamassassin)
pkgver=3.22.1+8+gf95d465
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://wiki.gnome.org/Apps/Evolution"
arch=(i686 x86_64)
license=(GPL)
depends=(gnome-desktop evolution-data-server libcanberra libpst libytnef
         dconf gtkspell3 libcryptui)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter
             spamassassin highlight gnome-common gtk-doc yelp-tools git)
options=(!emptydirs)
_commit=f95d465ae1af5ff7dcdd57e7dd465a1208162f0a  # gnome-3-22
source=("git://git.gnome.org/evolution#commit=$_commit")
sha256sums=('SKIP')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/^EVOLUTION_//;s/_/./g;s/-/+/g'
}

prepare() {
  cd $pkgbase
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgbase

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

  cd $pkgbase
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
