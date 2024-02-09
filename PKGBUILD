# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(
  evolution
  evolution-bogofilter
  evolution-spamassassin
)
pkgver=3.50.4
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://wiki.gnome.org/Apps/Evolution"
arch=(x86_64)
license=(LGPL-2.1-or-later)
depends=(
  cmark
  enchant
  evolution-data-server
  gnome-autoar
  gnome-desktop
  gspell
  libcanberra
  libgweather-4
  libnotify
  libpst
  libytnef
  webkit2gtk-4.1
)
makedepends=(
  bogofilter
  cmake
  docbook-xsl
  git
  gtk-doc
  highlight
  intltool
  itstool
  networkmanager
  ninja
  spamassassin
  yelp-tools
)
options=(!emptydirs)
_commit=a2d36c9e84a2e7e9c71758b8c65e9bb008a925f7  # tags/3.50.4^0
source=("git+https://gitlab.gnome.org/GNOME/evolution.git#commit=$_commit")
b2sums=('SKIP')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd $pkgbase
}

build() {
  local cmake_options=(
    -DCMAKE_INSTALL_PREFIX=/usr
    -DLIBEXEC_INSTALL_DIR=/usr/lib
    -DSYSCONF_INSTALL_DIR=/etc

    -DENABLE_GTK_DOC=ON
    -DENABLE_SMIME=ON
  )

  cmake -S $pkgbase -B build -G Ninja "${cmake_options[@]}"
  cmake --build build
}

_pick() {
  local p="$1" f d; shift
  for f; do
    d="$srcdir/$p/${f#$pkgdir/}"
    mkdir -p "$(dirname "$d")"
    mv "$f" "$d"
    rmdir -p --ignore-fail-on-non-empty "$(dirname "$f")"
  done
}

package_evolution() {
  depends+=(
    libcamel-1.2.so
    libebook-1.2.so
    libebook-contacts-1.2.so
    libecal-2.0.so
    libedataserver-1.2.so
    libedataserverui-1.2.so
  )
  optdepends=(
    'evolution-bogofilter: Bogofilter spam check plugin'
    'evolution-spamassassin: Spamassassin spam check plugin'
    'highlight: text highlight plugin'
  )
  groups=(gnome-extra)
  license+=(LicenseRef-OpenLDAP-Public-License)

  DESTDIR="$pkgdir" cmake --install build

  cd "$pkgdir"
  for x in bogofilter spamassassin; do
    _pick $x usr/lib/evolution/modules/module-$x.so
    _pick $x usr/share/metainfo/org.gnome.Evolution-$x.metainfo.xml
  done

  install -D -m644 "${srcdir}/evolution/COPYING.OPENLDAP" \
    "${pkgdir}/usr/share/licenses/evolution/COPYING.OPENLDAP"
}


package_evolution-bogofilter() {
  pkgdesc="Spam filtering for Evolution, using Bogofilter"
  depends=(
    "evolution=$pkgver"
    bogofilter
  )

  mv bogofilter/* "$pkgdir"
}

package_evolution-spamassassin() {
  pkgdesc="Spam filtering for Evolution, using SpamAssassin"
  depends=(
    "evolution=$pkgver"
    spamassassin
  )

  mv spamassassin/* "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
