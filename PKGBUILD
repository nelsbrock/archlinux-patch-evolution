# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(evolution evolution-bogofilter evolution-spamassassin)
pkgver=3.24.2
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://wiki.gnome.org/Apps/Evolution"
arch=(i686 x86_64)
license=(GPL)
depends=(gnome-desktop evolution-data-server libcanberra libpst libytnef
         dconf gtkspell3 libcryptui gnome-autoar)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter
             spamassassin highlight gtk-doc yelp-tools git cmake)
options=(!emptydirs)
_commit=82c5c091d9427fcc9b675163f6ce089845f5ea48  # tags/EVOLUTION_3_24_2^0
source=("git+https://git.gnome.org/browse/evolution#commit=$_commit")
sha256sums=('SKIP')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/^EVOLUTION_//;s/_/./g;s/-/+/g'
}

build() {
  cd $pkgbase

  cmake -DCMAKE_INSTALL_PREFIX=/usr -DLIBEXEC_INSTALL_DIR=/usr/lib -DSYSCONF_INSTALL_DIR=/etc -DENABLE_SMIME=ON -DENABLE_GTK_DOC=ON .
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
