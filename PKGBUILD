# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(
  evolution
  evolution-bogofilter
  evolution-spamassassin
)
pkgver=3.52.2
pkgrel=2
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
  glib2-devel
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
b2sums=('5094abb2d684c559ec50c9d3b1d6f865b5349b8a6360c281fa1a005841e3da72532193cef647f25eba558a1d755d96f409e80470ad070b60252ffed6ae196fc4')

prepare() {
  cd $pkgbase

  # Fix crash when spellchecking without errors
  # https://gitlab.archlinux.org/archlinux/packaging/packages/evolution/-/issues/3
  # https://gitlab.gnome.org/GNOME/evolution/-/issues/2761
  git cherry-pick -n a735ee79e68e0a09fa7dd9a116bde16a4b48b4ad
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
