# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=3.4.0
pkgrel=1
pkgdesc="Manage your email, contacts and schedule"
arch=('i686' 'x86_64')
license=('GPL')
depends=('gnome-desktop' 'evolution-data-server' 'gtkhtml4' 'libcanberra' 'gstreamer0.10' 'libpst' 'libytnef' 'desktop-file-utils' 'hicolor-icon-theme' 'dconf')
makedepends=('intltool' 'gnome-doc-utils' 'networkmanager' 'gnome-common' 'gtk-doc')
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz)
sha256sums=('e7d0eecaa1ee33c15e327b724f2bd7017ea6c732ac764189f0ba2a5ba20bd8e4')

build() {
  cd $pkgname-$pkgver
  
  # Fixed past 3.4.0
  USE_GNOME2_MACROS=1 gnome-autogen.sh

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
