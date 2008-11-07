# Maintainer: dale <dale@archlinux.org>

pkgname=evolution
pkgver=2.24.1.1
pkgrel=1
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=(i686 x86_64)
license=('GPL')
depends=('libnetworkmanager>=0.6.6' 'libnotify>=0.4.4' 'gtkhtml>=3.24.1.1' 'evolution-data-server>=2.24.1.1' 'gnome-pilot>=2.0.16' 'gstreamer0.10>=0.10.20')
makedepends=('perlxml' 'pkgconfig' 'gnome-doc-utils>=0.14.0' 'intltool' 'iso-codes')
options=('!libtool' '!emptydirs')
install=evolution.install
url=(http://www.gnome.org/projects/evolution/)
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.24/${pkgname}-${pkgver}.tar.bz2)
md5sums=('3a76f0e2493e7497ac8526c31a2bc7a3')

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

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" ${pkgdir}/etc/gconf/schemas/*.schemas || return 1
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
