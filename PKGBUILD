# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=2.31.92
pkgrel=1
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=('i686' 'x86_64')
license=('GPL')
depends=('libnotify>=0.4.5' 'gtkhtml>=3.31.92' 'evolution-data-server>=2.31.92' 'gstreamer0.10>=0.10.30' 'libpst>=0.6.41' 'libytnef>=0.1.5' 'gnome-desktop>=2.31.92' 'libcanberra>=0.25' 'gtkimageview>=1.6.4' 'libunique')
makedepends=('pkgconfig' 'gnome-doc-utils>=0.20.1' 'intltool' 'networkmanager')
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.31/${pkgname}-${pkgver}.tar.bz2)
sha256sums=('feda2b365063914ee27ab689cfc15442c8b98a8e8e08951e1f28e6c578c068f4')

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
