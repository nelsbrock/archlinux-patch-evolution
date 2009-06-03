# Maintainer: dale <dale@archlinux.org>

pkgname=evolution
pkgver=2.26.2
_majorver=2.26
pkgrel=1
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=(i686 x86_64)
license=('GPL')
depends=('libnotify>=0.4.5' 'gtkhtml>=3.26.2' 'evolution-data-server>=2.26.2' 'gnome-pilot>=2.0.17' 'gstreamer0.10>=0.10.23' 'libpst>=0.6.37' 'libytnef>=0.1.5')
makedepends=('pkgconfig' 'gnome-doc-utils>=0.16.0' 'intltool' 'networkmanager')
groups=('gnome-extra')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/${_majorver}/${pkgname}-${pkgver}.tar.bz2
	libpst-0.6.37-support.patch)
md5sums=('f93b02141a8e67588ed258d0dc310a0f'
	 'd30473005fae6c2372cfae293ca40ea4')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  # http://bugzilla.gnome.org/show_bug.cgi?id=578945
  patch -Np0 -i "${srcdir}/libpst-0.6.37-support.patch" || return 1
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
