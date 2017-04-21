# Maintainer: Archer777 <NAME at gmx dot com>
# Contributor: Army
# Contributor: Dave Reisner <d@falconindy.com>
# Contributor: Karol Cichy <slothck@gmail.com>

_pkgname=conky
pkgname=conky-cli
pkgver=1.10.5
pkgrel=1
pkgdesc="Lightweight system monitor for X, without X11 dependencies"
url='https://github.com/brndnmtthws/conky'
license=('BSD' 'GPL')
arch=('i686' 'x86_64')
provides=('conky')
conflicts=('conky')
makedepends=('cmake' 'docbook2x' 'docbook-xml' 'man-db' 'git')
depends=('curl' 'lua' 'wireless_tools' 'libxml2')
source=("https://github.com/brndnmtthws/${_pkgname}/archive/v${pkgver}.tar.gz"
        'lua53.patch')

sha1sums=('a560c356ce4c3e2918601743b57d19a18fe01a72'
          'a3a74542b6524e5663ad37aaba292b48e8bea3b1')

prepare() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p1 -i ../lua53.patch # lua_gettable returns an int in lua-5.3
}

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"

	cmake \
		-D CMAKE_BUILD_TYPE=Release \
		-D MAINTAINER_MODE=ON \
        -D BUILD_HDDTEMP=OFF \
        -D BUILD_PORT_MONITORS=OFF \
		-D BUILD_WEATHER_METAR=ON \
		-D BUILD_WEATHER_XOAP=ON \
        -D BUILD_X11=OFF \
        -D BUILD_XDAMAGE=OFF \
        -D BUILD_XFT=OFF \
		-D BUILD_WLAN=ON \
		-D BUILD_MPD=ON \
		-D CMAKE_INSTALL_PREFIX=/usr \
		.

	make
}

package() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install
	install -Dm644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 extras/vim/syntax/conkyrc.vim "${pkgdir}"/usr/share/vim/vimfiles/syntax/conkyrc.vim
	install -Dm644 extras/vim/ftdetect/conkyrc.vim "${pkgdir}"/usr/share/vim/vimfiles/ftdetect/conkyrc.vim
}
