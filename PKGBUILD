# $Id$
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgbase=imagemagick
pkgname=('imagemagick' 'imagemagick-doc')
pkgver=6.7.6.0
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.imagemagick.org/"
license=('custom')
makedepends=('libltdl' 'lcms2' 'libxt' 'fontconfig' 'libxext' 'ghostscript' \
             'openexr' 'libwmf' 'librsvg' 'libxml2' 'jasper')
source=(ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.xz \
        perlmagick.rpath.patch)
sha1sums=('878c264d070c70debc514d7da9f4feb7728bfe9f'
          '23405f80904b1de94ebd7bd6fe2a332471b8c283')

build() {
  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}

  sed '/AC_PATH_XTRA/d' -i configure.ac
  autoreconf
  patch -p0 -i ../perlmagick.rpath.patch

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
  depends=('perl' 'libltdl' 'lcms2' 'libxt' 'fontconfig' 'libxext')
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
          'etc/ImageMagick/quantization-table.xml'
          'etc/ImageMagick/sRGB.icc'
          'etc/ImageMagick/thresholds.xml'
          'etc/ImageMagick/type.xml'
          'etc/ImageMagick/type-dejavu.xml'
          'etc/ImageMagick/type-ghostscript.xml'
          'etc/ImageMagick/type-windows.xml')
  options=('!docs' 'libtool' '!emptydirs')

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

  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
  make DESTDIR="${pkgdir}" install-data-html
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick-doc/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick-doc/NOTICE"
}
