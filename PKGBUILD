# Maintainer: Bruno Pagani (a.k.a. ArchangeGabriel) <bruno.n.pagani at gmail dot com>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Antonio Rojas
# Contributor: Alexey D. <lq07829icatm at rambler.ru>

pkgbase=plasma-workspace
pkgname=plasma-workspace-light
pkgver=5.2.2
pkgrel=1
pkgdesc='KDE Plasma Workspace without milou'
arch=('i686' 'x86_64')
url='https://projects.kde.org/projects/kde/workspace/plasma-workspace'
license=('LGPL')
# note on libxdamage:
# not detected by namcap because libgl depends on it
# but nvidia providing libgl does not depend on libxdamage
depends=('knewstuff' 'kjsembed' 'knotifyconfig' 'libxdamage' 'kwayland'
         'libksysguard' 'libkscreen-frameworks' 'ktexteditor' 'libqalculate'
         'qt5-tools' 'kded' 'kde-cli-tools' 'xorg-xrdb' 'xorg-xsetroot'
         'xorg-xmessage' 'xorg-xprop')
makedepends=('extra-cmake-modules' 'kdoctools' 'kwin' 'krunner' 'baloo-frameworks' 'gpsd')
optdepends=('plasma-workspace-wallpapers: additional wallpapers'
            'gpsd: GPS support for geolocation')
conflicts=('kdebase-workspace' 'plasma-workspace')
provides=('plasma-workspace')
groups=('plasma')
source=("http://download.kde.org/stable/plasma/${pkgver}/${pkgbase}-${pkgver}.tar.xz"
        'kde.pam')
md5sums=('93b4b7e187035635982d3099ba2c8d79'
         '929b182dec8a096206ad493477c09d2c')

prepare() {
  mkdir build

  cd ${pkgbase}-${pkgver}
  # be sure to use the Qt5 version of qtpaths
  sed -i 's:qtpaths:qtpaths-qt5:' startkde/startkde.cmake
}

build() {
  cd build
  cmake ../${pkgbase}-${pkgver} \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=Release \
    -DLIB_INSTALL_DIR=lib \
    -DLIBEXEC_INSTALL_DIR=lib \
    -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
    -DBUILD_TESTING=OFF
  make
}

package() {

  cd build
  make DESTDIR="${pkgdir}" install

  install -D "${srcdir}"/kde.pam \
    "${pkgdir}"/etc/pam.d/kde

  # Remove conflicts with drkonqi
  rm "${pkgdir}"/usr/lib/drkonqi
  rm -r "${pkgdir}"/usr/share/drkonqi
  rm "${pkgdir}"/usr/lib/libKF5XmlRpcClientPrivate.*
}
