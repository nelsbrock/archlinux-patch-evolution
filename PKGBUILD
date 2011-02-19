# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=2.32.2
pkgrel=2
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=('i686' 'x86_64')
license=('GPL')
depends=('libnotify>=0.5.2' 'gtkhtml>=3.32.2' 'evolution-data-server>=2.32.2' 'gstreamer0.10>=0.10.30' 'libpst>=0.6.41' 'libytnef>=0.1.5' 'gnome-desktop>=2.32.0' 'libcanberra>=0.26' 'gtkimageview>=1.6.4' 'libunique' 'desktop-file-utils')
makedepends=('gnome-doc-utils>=0.20.1' 'intltool' 'networkmanager')
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.32/${pkgname}-${pkgver}.tar.bz2)
sha256sums=('1831c7f5f00b4755869cf84ef3e3f79c4705a33e0659e72b6c1ffa4a59d4a4bb')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --disable-scrollkeeper \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --with-krb5=/usr
  make
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install
  cd plugins/tnef-attachments
  make
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain evolution-${_majorver} ${pkgdir}/etc/gconf/schemas/*.schemas
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
