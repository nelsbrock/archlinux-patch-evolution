# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(evolution evolution-bogofilter evolution-spamassassin)
pkgver=3.26.2
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://wiki.gnome.org/Apps/Evolution"
arch=(x86_64)
license=(GPL)
depends=(gnome-desktop evolution-data-server libcanberra libpst libytnef
         dconf gtkspell3 libcryptui gnome-autoar)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter
             spamassassin highlight gtk-doc yelp-tools git cmake ninja)
options=(!emptydirs)
_commit=c397294ece1b8f8eab513486fe49f1e362c2c63b  # tags/EVOLUTION_3_26_2^0
source=("git+https://git.gnome.org/browse/evolution#commit=$_commit")
sha256sums=('SKIP')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/^EVOLUTION_//;s/_/./g;s/-/+/g'
}

build() {
  cd $pkgbase

  cmake . -G Ninja \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DLIBEXEC_INSTALL_DIR=/usr/lib \
    -DSYSCONF_INSTALL_DIR=/etc \
    -DENABLE_SMIME=ON \
    -DENABLE_GTK_DOC=ON
  ninja
}

package_evolution() {
  optdepends=('highlight: text highlight plugin'
              'evolution-spamassassin: Spamassassin spam check plugin'
              'evolution-bogofilter: Bogofilter spam check plugin')
  groups=(gnome-extra)

  cd $pkgbase
  DESTDIR="$pkgdir" ninja install

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
