# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(evolution evolution-bogofilter evolution-spamassassin)
pkgver=3.34.1
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://wiki.gnome.org/Apps/Evolution"
arch=(x86_64)
license=(GPL)
depends=(gnome-desktop evolution-data-server libcanberra libpst libytnef
         dconf gspell libcryptui gnome-autoar)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter
             spamassassin highlight gtk-doc yelp-tools git cmake)
options=(!emptydirs)
_commit=c86e393989a92cb7ded5df1cdbb263260cd9a802  # tags/3.34.1^0
source=("git+https://gitlab.gnome.org/GNOME/evolution.git#commit=$_commit")
sha256sums=('SKIP')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/^EVOLUTION_//;s/_/./g;s/-/+/g'
}

prepare() {
  cd $pkgbase
}

build() {
  cmake -H$pkgbase -Bbuild \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DLIBEXEC_INSTALL_DIR=/usr/lib \
    -DSYSCONF_INSTALL_DIR=/etc \
    -DENABLE_SMIME=ON \
    -DENABLE_GTK_DOC=ON
  cmake --build build
}

package_evolution() {
  optdepends=('highlight: text highlight plugin'
              'evolution-spamassassin: Spamassassin spam check plugin'
              'evolution-bogofilter: Bogofilter spam check plugin')
  groups=(gnome-extra)

  DESTDIR="$pkgdir" cmake --build build --target install

### Split
  local x
  for x in bogofilter spamassassin; do
    mkdir -p $x/usr/{lib/evolution/modules,share/metainfo}
    mv {"$pkgdir",$x}/usr/lib/evolution/modules/module-$x.so
    mv {"$pkgdir",$x}/usr/share/metainfo/org.gnome.Evolution-$x.metainfo.xml
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
