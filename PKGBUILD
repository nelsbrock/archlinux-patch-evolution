# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=evolution
pkgname=(
  evolution
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
  patch
  spamassassin
  yelp-tools
)
options=(!emptydirs)
source=("git+https://gitlab.gnome.org/GNOME/evolution.git#tag=$pkgver" remove-user-agent.patch)
b2sums=('5094abb2d684c559ec50c9d3b1d6f865b5349b8a6360c281fa1a005841e3da72532193cef647f25eba558a1d755d96f409e80470ad070b60252ffed6ae196fc4'
        '88aefdb879a3d91f38b454990c928166f983d4cb290d5d89e2eaf188382fa90eb873167faf46247dd9f52861f7f9755edf2418b587e17fbf2a9e53a90bbd1a0d')

prepare() {
  cd $pkgbase

  # Fix crash when spellchecking without errors
  # https://gitlab.archlinux.org/archlinux/packaging/packages/evolution/-/issues/3
  # https://gitlab.gnome.org/GNOME/evolution/-/issues/2761
  git cherry-pick -n a735ee79e68e0a09fa7dd9a116bde16a4b48b4ad

  patch --forward --strip=1 --input=../remove-user-agent.patch
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
  groups=(gnome-extra modified)
  license+=(LicenseRef-OpenLDAP-Public-License)

  DESTDIR="$pkgdir" cmake --install build

  cd "$pkgdir"

  install -D -m644 "${srcdir}/evolution/COPYING.OPENLDAP" \
    "${pkgdir}/usr/share/licenses/evolution/COPYING.OPENLDAP"
}

# vim:set sw=2 sts=-1 et:
