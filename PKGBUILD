# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=3.4.0.1
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
arch=('i686' 'x86_64')
license=('GPL')
depends=('gnome-desktop' 'evolution-data-server' 'gtkhtml4' 'libcanberra' 'gstreamer0.10' 'libpst' 'libytnef' 'desktop-file-utils' 'hicolor-icon-theme' 'dconf')
makedepends=('intltool' 'gnome-doc-utils' 'networkmanager')
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/$pkgname/${pkgver:0:3}/$pkgname-$pkgver.tar.xz)
sha256sums=('364eacfc54912ba26738729d0f662a821140a712e3607cf569eb342f1dab4f7a')

build() {
  cd $pkgname-$pkgver

  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --with-krb5=/usr \
      --disable-image-inline
  
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0 /g' -e 's/    if test "$export_dynamic" = yes && test -n "$export_dynamic_flag_spec"; then/      func_append compile_command " -Wl,-O1,--as-needed"\n      func_append finalize_command " -Wl,-O1,--as-needed"\n\0/' libtool

  make
  make -C plugins/tnef-attachments
}

package() {
  cd $pkgname-$pkgver
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="$pkgdir" install
  make -C plugins/tnef-attachments DESTDIR="$pkgdir" install

  install -m755 -d "$pkgdir/usr/share/gconf/schemas"
  gconf-merge-schema "$pkgdir/usr/share/gconf/schemas/$pkgname.schemas" --domain evolution-3.2 "$pkgdir"/etc/gconf/schemas/*.schemas
  rm -f "$pkgdir"/etc/gconf/schemas/*.schemas
}
