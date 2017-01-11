# $Id$
# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Damir Perisa <damir.perisa@bluewin.ch>
# Contributor: Ben <ben@benmazer.net>

pkgname=mpd
pkgver=0.20.1
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
source=("https://www.musicpd.org/download/${pkgname}/${pkgver%.*}/${pkgname}-${pkgver}.tar.xz"{,.sig}
#source=("https://www.musicpd.org/download/${pkgname}/0.20/${pkgname}-${pkgver}.tar.xz"{,.sig}
        'tmpfiles.d'
        'conf')
sha1sums=('afa490380e094006b50360f13b48df6643030ae6' 'SKIP'
          'f4d5922abb69abb739542d8e93f4dfd748acdad7'
          '291fd5cda9f0845834a553017327c4586bd853f6')

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
