# Contributor: system <system at tou dot de>
pkgname=dzen2-xft-xpm-xinerama-svn
_basename=dzen2
pkgver=r271
pkgrel=1
pkgdesc="X notification utility with Xinerama, XPM, XFT and gadgets, svn version"
arch=('i686' 'x86_64')
url="http://gotmor.googlepages.com/dzen"
license=('MIT')
depends=('libxpm' 'libxinerama' 'libxft')
makedepends=('subversion')
provides=('dzen2')
conflicts=('dzen2' 'dzen2-gadgets-svn' 'dzen2-svn')
source=("$_basename::svn+http://dzen.googlecode.com/svn/trunk"
        "text-margin.diff"
        "xft-xpm.diff")
md5sums=('SKIP'
         '34d5548b2ebef0f3441514fdb9efd7d4'
         'd599f8bd7ae7254a43630a517589a7ca')

pkgver() {
  cd "$srcdir/$_basename"
  local ver=$(svnversion)
  printf 'r%s' "${ver//[[:alpha:]]}"
}

prepare() {
  cd "$srcdir/$_basename"

  patch -Np1 -i "$srcdir/text-margin.diff"
  patch -Np1 -i "$srcdir/xft-xpm.diff"
}

build() {
  cd "$srcdir/$_basename"

  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
  make PREFIX=/usr DESTDIR="$pkgdir" install

  #license
  install -m644 -D LICENSE "$pkgdir/usr/share/licenses/$_basename/COPYING"

  #docs
  mkdir -p "$pkgdir/usr/share/doc/$_basename"
  install -m644 README* "$pkgdir/usr/share/doc/$_basename"

  #gadgets
  cd gadgets
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
  make PREFIX=/usr DESTDIR=$startdir/pkg install

  #docs
  install -m644 README* "$pkgdir/usr/share/doc/$_basename"
}
