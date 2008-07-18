# Maintainer: dale <dale@archlinux.org>

pkgname=evolution
pkgver=2.22.3.1
pkgrel=2
pkgdesc="Integrated Workgroup and Personal Information Management for Gnome"
arch=(i686 x86_64)
license=('GPL')
depends=('libnetworkmanager>=0.6.6' 'libnotify>=0.4.4' 'gtkhtml>=3.18.3' 'evolution-data-server>=2.22.3' 'gnome-pilot>=2.0.16')
makedepends=('perlxml' 'pkgconfig' 'gnome-doc-utils>=0.12.2' 'gstreamer0.10>=0.10.20' 'intltool')
options=('!libtool' '!emptydirs')
install=evolution.install
url=(http://www.gnome.org/projects/evolution/)
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/2.22/${pkgname}-${pkgver}.tar.bz2)
md5sums=('5be2cb0584344cfe7bb4944c2fe468be')

build() {
  cd ${startdir}/src/${pkgname}-${pkgver}

  ./configure --prefix=/usr --sysconfdir=/etc \
        --localstatedir=/var \
	--libexecdir=/usr/lib/evolution \
  	--disable-scrollkeeper \
	--enable-nss=yes \
	--with-openldap=yes \
	--enable-smime=yes \
	--enable-pilot-conduits=yes \
	--with-krb5=/usr || return 1
  make || return 1
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR=${startdir}/pkg install || return 1

  install -m755 -d ${startdir}/pkg/usr/share/gconf/schemas
  gconf-merge-schema ${startdir}/pkg/usr/share/gconf/schemas/${pkgname}.schemas ${startdir}/pkg/etc/gconf/schemas/*.schemas || return 1
  rm -f ${startdir}/pkg/etc/gconf/schemas/*.schemas
}
