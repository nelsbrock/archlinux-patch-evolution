# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=3.12.2
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
arch=(i686 x86_64)
license=(GPL)
depends=(gnome-desktop evolution-data-server gtkhtml4 libcanberra libpst libytnef psmisc
         desktop-file-utils hicolor-icon-theme dconf gtkspell3)
makedepends=(intltool itstool docbook-xsl networkmanager bogofilter spamassassin highlight)
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin'
            'highlight: text highlight plugin')
groups=('gnome-extra')
options=('!emptydirs')
install=evolution.install
url=https://wiki.gnome.org/Apps/Evolution
source=(http://ftp.gnome.org/pub/gnome/sources/$pkgname/${pkgver:0:4}/$pkgname-$pkgver.tar.xz)
sha256sums=('60742334aaf1e3b9f044c2003c44a37be5905b166e24580e9e6e6c5ae1b9f948')

build() {
  cd $pkgname-$pkgver

  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --disable-schemas-compile
  
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0 /g' -e 's/    if test "$export_dynamic" = yes && test -n "$export_dynamic_flag_spec"; then/      func_append compile_command " -Wl,-O1,--as-needed"\n      func_append finalize_command " -Wl,-O1,--as-needed"\n\0/' libtool

  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
}
