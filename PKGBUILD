# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(evolution evolution-bogofilter evolution-spamassassin)
pkgver=3.28.0+8+gf3e2c90d24
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
_commit=f3e2c90d24020bdc5953ef86b8cd68f98fb6875a  # gnome-3-28
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

  DESTDIR="$pkgdir" ninja -C $pkgbase install

### Split
  for _x in bogofilter spamassassin; do
    mkdir -p $_x/usr/{lib/evolution/modules,share/metainfo}
    mv {"$pkgdir",$_x}/usr/lib/evolution/modules/module-$_x.so
    mv {"$pkgdir",$_x}/usr/share/metainfo/org.gnome.Evolution-$_x.metainfo.xml
  done
}


package_evolution-bogofilter() {
  pkgdesc="Spam filtering for Evolution, using Bogofilter"
  depends=("evolution=$pkgver" bogofilter)

  mv bogofilter/usr "$pkgdir"
}

package_evolution-spamassassin() {
  pkgdesc="Spam filtering for Evolution, using SpamAssassin"
  depends=("evolution=$pkgver" spamassassin)

  mv spamassassin/usr "$pkgdir"
}
