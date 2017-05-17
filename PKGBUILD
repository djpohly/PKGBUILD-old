# $Id$
# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Damir Perisa <damir.perisa@bluewin.ch>
# Contributor: Ben <ben@benmazer.net>

pkgname=mpd
pkgver=0.20.7
pkgrel=1
pkgdesc='Flexible, powerful, server-side application for playing music'
url='https://www.musicpd.org/'
license=('GPL')
arch=('i686' 'x86_64')
depends=('libao' 'ffmpeg' 'libmodplug' 'audiofile' 'libshout' 'libmad' 'curl' 'faad2'
         'sqlite' 'jack' 'libmms' 'wavpack' 'avahi' 'libid3tag' 'yajl' 'libmpdclient'
         'zziplib'
         'icu' 'libupnp' 'libnfs' 'libsamplerate' 'libsoxr' 'smbclient' 'libcdio-paranoia'
         'libgme')
makedepends=('boost' 'doxygen')
validpgpkeys=('0392335A78083894A4301C43236E8A58C6DB4512')
#source=("https://www.musicpd.org/download/${pkgname}/${pkgver}/${pkgname}-${pkgver}.tar.xz"{,.sig}
source=("https://www.musicpd.org/download/${pkgname}/${pkgver%.*}/${pkgname}-${pkgver}.tar.xz"{,.sig}
        'tmpfiles.d'
        'conf')
sha256sums=('005ac663b39a76701ba043cce4caef82ac6b0c2f16aae12fdc28e1b3b5b6c780'
            'SKIP'
            'c1683ba35774c85e16c70e89f7e2ed1c09619512b1a273daabbd5e34d40439bd'
            'f40f68205834ca53cea3372e930bfe6c2f9ecc9df3b1605df2fec63a658b2e03')

backup=('etc/mpd.conf')
install=install

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--enable-cdio-paranoia \
		--enable-libmpdclient \
		--enable-jack \
		--enable-soundcloud \
		--enable-pipe-output \
		--disable-pulse \
		--enable-libgme \
		--enable-zzip \
		--disable-sidplay \
		--with-systemduserunitdir=/usr/lib/systemd/user \
		--with-systemdsystemunitdir=/usr/lib/systemd/system \

	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install
	install -Dm644 ../conf "${pkgdir}"/etc/mpd.conf
	install -Dm644 ../tmpfiles.d "${pkgdir}"/usr/lib/tmpfiles.d/mpd.conf
	install -d -g 45 -o 45 "${pkgdir}"/var/lib/mpd{,/playlists}

	sed '/\[Service\]/a User=mpd' -i "${pkgdir}"/usr/lib/systemd/system/mpd.service
	sed '/WantedBy=/c WantedBy=default.target' -i "${pkgdir}"/usr/lib/systemd/system/mpd.service
}
