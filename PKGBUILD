# $Id$
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgbase=imagemagick
pkgname=('imagemagick' 'imagemagick-doc')
pkgver=6.7.3.8
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.imagemagick.org/"
license=('custom')
depends=('libltdl' 'lcms2' 'libxt' 'xz' 'fontconfig' 'libxext' 'libjpeg-turbo')
makedepends=('ghostscript' 'openexr' 'libwmf' 'librsvg' 'libxml2' 'jasper' 'libpng')
source=(ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.xz \
        perlmagick.rpath.patch)
sha1sums=('0414771506d69efd6f3ba3c20ab3320e46656f39'
          '23405f80904b1de94ebd7bd6fe2a332471b8c283')

build() {
  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}

  sed '/AC_PATH_XTRA/d' -i configure.ac
  autoreconf
  patch -p0 -i ../perlmagick.rpath.patch

  LIBS="$LIBS -L/usr/lib/perl5/core_perl/CORE -lperl" \
    ./configure --prefix=/usr --sysconfdir=/etc --with-modules --disable-static \
    --enable-openmp --with-wmf --with-openexr --with-xml --with-lcms2 --with-jp2 \
    --with-gslib --with-gs-font-dir=/usr/share/fonts/Type1 \
    --with-perl --with-perl-options="INSTALLDIRS=vendor" \
    --without-gvc --without-djvu --without-autotrace --without-webp \
    --without-jbig --without-fpx --without-dps --without-fftw --without-lqr
  make
}

check() {
  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
  make check
}

package_imagemagick() {
  pkgdesc="An image viewing/manipulation program"
  optdepends=('ghostscript: for Ghostscript support' 
              'openexr: for OpenEXR support' 
              'libwmf: for WMF support' 
              'librsvg: for SVG support' 
              'libxml2: for XML support' 
              'jasper: for JPEG-2000 support' 
              'libpng: for PNG support')
  backup=('etc/ImageMagick/coder.xml'
          'etc/ImageMagick/colors.xml'
          'etc/ImageMagick/delegates.xml'
          'etc/ImageMagick/log.xml'
          'etc/ImageMagick/magic.xml'
          'etc/ImageMagick/mime.xml'
          'etc/ImageMagick/policy.xml'
          'etc/ImageMagick/sRGB.icc'
          'etc/ImageMagick/thresholds.xml'
          'etc/ImageMagick/type.xml'
          'etc/ImageMagick/type-dejavu.xml'
          'etc/ImageMagick/type-ghostscript.xml'
          'etc/ImageMagick/type-windows.xml')
  options=('!makeflags' '!docs' 'libtool')

  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
  make DESTDIR="${pkgdir}" install
  chmod 755 "${pkgdir}/usr/lib/perl5/vendor_perl/auto/Image/Magick/Magick.so" 
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick/NOTICE"

#Cleaning
  find "${pkgdir}" -name '*.bs' -delete
  rm -f "${pkgdir}"/usr/lib/*.la
}

package_imagemagick-doc() {
  pkgdesc="The ImageMagick documentation (utilities manuals and libraries API)"
  depends=()
  options=('!makeflags')

  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
  make DESTDIR="${pkgdir}" install-data-html
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick-doc/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick-doc/NOTICE"
}
