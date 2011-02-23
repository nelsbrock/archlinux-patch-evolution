# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=2.91.90
pkgrel=1
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=('i686' 'x86_64')
license=('GPL')
depends=('gnome-desktop' 'evolution-data-server' 'gtkhtml' 'libcanberra' 'libsm' 'gstreamer0.10' 'libunique3' 'libnotify' 'libpst' 'libytnef')
makedepends=('intltool' 'gnome-doc-utils' 'networkmanager')
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.91/${pkgname}-${pkgver}.tar.bz2)
sha256sums=('a9577fca8a10280c3a0303ebca8b861893f01031f74b92e8838e0f5dcd0a2935')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --disable-scrollkeeper \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --with-krb5=/usr \
      --disable-image-inline
  make
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install
  cd plugins/tnef-attachments
  make
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain evolution-2.92 ${pkgdir}/etc/gconf/schemas/*.schemas
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
