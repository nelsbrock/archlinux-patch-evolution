# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(
  evolution
  evolution-bogofilter
  evolution-spamassassin
)
pkgver=3.52.1
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
url="https://gitlab.gnome.org/GNOME/evolution/-/wikis/home"
arch=(x86_64)
license=(LGPL-2.1-or-later)
depends=(
  at-spi2-core
  cairo
  cmark
  dconf
  enchant
  evolution-data-server
  gdk-pixbuf2
  geocode-glib-2
  glib2
  glibc
  gnome-autoar
  gnome-desktop
  gspell
  gtk3
  hicolor-icon-theme
  libcanberra
  libgweather-4
  libical
  libldap
  libnotify
  libpst
  libsecret
  libsoup3
  libxml2
  libytnef
  nspr
  nss
  pango
  sqlite
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
source=("git+https://gitlab.gnome.org/GNOME/evolution.git#tag=$pkgver")
b2sums=('6a3abaad1d08b7031716fa68c2c43d179a6ae268fc07c9a342532df1e51d7a2e92a8b082ee230f04b983a780453b603c5a203da53165a0cc55214f0e1722230d')

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
    evolution-data-server
    glib2
    glibc
    gtk3
  )

  mv bogofilter/* "$pkgdir"
}

package_evolution-spamassassin() {
  pkgdesc="Spam filtering for Evolution, using SpamAssassin"
  depends=(
    "evolution=$pkgver"
    evolution-data-server
    glib2
    glibc
    gtk3
    spamassassin
  )

  mv spamassassin/* "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
