# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=evolution
pkgver=2.29.92.1
pkgrel=1
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=('i686' 'x86_64')
license=('GPL')
depends=('libnotify>=0.4.5' 'gtkhtml>=3.29.92' 'evolution-data-server>=2.29.92' 'gnome-pilot>=2.0.17' 'gstreamer0.10>=0.10.26' 'libpst>=0.6.41' 'libytnef>=0.1.5' 'gnome-desktop>=2.29.92' 'libcanberra>=0.23' 'gtkimageview>=1.6.4')
makedepends=('pkgconfig' 'gnome-doc-utils>=0.19.5' 'intltool' 'networkmanager')
optdepends=('bogofilter: possible junk filter plugin'
            'spamassassin: possible junk filter plugin')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.29/${pkgname}-${pkgver}.tar.bz2)
sha256sums=('c2c2708abdbc11ec2558f5262b4fa79777c08c0dc00a6d0865ff56300d6dcb27')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var \
      --libexecdir=/usr/lib \
      --disable-scrollkeeper \
      --enable-nss=yes \
      --with-openldap=yes \
      --enable-smime=yes \
      --enable-pilot-conduits=yes \
      --with-krb5=/usr || return 1
  make || return 1
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install || return 1
  cd plugins/tnef-attachments || return 1
  make || return 1
  make DESTDIR="${pkgdir}" install || return 1

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain evolution-${_majorver} ${pkgdir}/etc/gconf/schemas/*.schemas || return 1
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
