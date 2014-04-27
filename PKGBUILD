# Contributor: system <system at tou dot de>
pkgname=dzen2-xft-xpm-xinerama-svn
pkgver=271
pkgrel=1
pkgdesc="X notification utility with Xinerama, XPM, XFT and gadgets, svn version"
arch=('i686' 'x86_64')
url="http://gotmor.googlepages.com/dzen"
license=('MIT')
depends=('libxpm' 'libxinerama' 'libxft')
makedepends=('subversion' 'gcc')
provides=('dzen2')
conflicts=('dzen2' 'dzen2-gadgets-svn' 'dzen2-svn')
source=(text-margin.diff)

_svntrunk=http://dzen.googlecode.com/svn/trunk/
_svnmod=dzen2

prepare() {
  cd "$srcdir"

  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf $_svnmod-build
  cp -rf $_svnmod $_svnmod-build
  cd $_svnmod-build

  patch -Np0 < ../text-margin.diff
}

build() {
  cd "$srcdir/$_svnmod-build"
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11 \
  CFLAGS=" -Wall -Os ${INCS} -DVERSION=\"${VERSION}\" -DDZEN_XINERAMA -DDZEN_XPM -DDZEN_XFT `pkg-config --cflags xft`" \
  LIBS=" -L/usr/lib -lc -L${X11LIB} -lX11 -lXinerama -lXpm `pkg-config --libs xft`"
}

package() {
  cd "$srcdir/$_svnmod-build"
  make PREFIX=/usr DESTDIR=$pkgdir install

  #license
  install -m644 -D LICENSE $pkgdir/usr/share/licenses/$_svnmod/COPYING

  #docs
  install -d $pkgdir/usr/share/doc/$_svnmod
  install -m644 README* $pkgdir/usr/share/doc/$_svnmod

  #gadgets
  cd gadgets
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
  make PREFIX=/usr DESTDIR=$pkgdir install

  #docs
  install -m644 README* $pkgdir/usr/share/doc/$_svnmod

}
md5sums=('a0e65c3b9eb71a3bddb32a255a946b0c')
