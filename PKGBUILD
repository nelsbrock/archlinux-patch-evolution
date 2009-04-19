# Maintainer: dale <dale@archlinux.org>

pkgname=evolution
pkgver=2.26.1.1
_majorver=2.26
pkgrel=1
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=(i686 x86_64)
license=('GPL')
depends=('libnotify>=0.4.5' 'gtkhtml>=3.26.1.1' 'evolution-data-server>=2.26.1.1' 'gnome-pilot>=2.0.17' 'gstreamer0.10>=0.10.22')
makedepends=('pkgconfig' 'gnome-doc-utils>=0.16.0' 'intltool' 'networkmanager')
options=('!libtool' '!emptydirs')
install=evolution.install
url=http://www.gnome.org/projects/evolution/
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/${_majorver}/${pkgname}-${pkgver}.tar.bz2)
md5sums=('e24b8c13d350c17ab8332482f60aa144')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  # TODO: add libpst and yTNEF
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
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain evolution-${_majorver} ${pkgdir}/etc/gconf/schemas/*.schemas || return 1
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
