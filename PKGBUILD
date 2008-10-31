# $Id$
# Maintainer: Eric Belanger <eric@archlinux.org>

# NOTE: ImageMagick builds against an existing installation
# uninstall ImageMagick before building, or build it, install it, build it.

# NOTE 2: To circumvent linking problems (FS#10574), this package must now be built the following way:
# install old package, build new package, install new package, rebuild

pkgname=imagemagick
pkgver=6.4.5.1
pkgrel=1
pkgdesc="An image viewing/manipulation program"
arch=('i686' 'x86_64')
url="http://www.imagemagick.org/"
license=('custom')
depends=('lcms' 'libwmf' 'librsvg' 'libxt' 'gcc-libs' 'ghostscript' 'openexr' 'libtool>=2.2' 'bzip2' 'libxml2' 'jasper')
options=('!makeflags' '!docs' '!libtool')
source=(ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.bz2 \
        libpng_mmx_patch_x86_64.patch add_delegate.patch)
md5sums=('bb56af6fb74e887b0d079e3a350fad03'
         '069980fc2590c02aed86420996259302'
         '7f5851c4450b73d52df55c7e806cc316')

build() {
  cd ${srcdir}/ImageMagick-${pkgver%.*}

  if [ "${CARCH}" = "x86_64" ]; then
    patch -Np1 < ../libpng_mmx_patch_x86_64.patch || return 1
  fi

  patch -p0 < ../add_delegate.patch || return 1
  sed -i "s/with_autotrace='no'/with_autotrace='yes'/" configure || return 1

  LIBS=-lMagickWand ./configure --prefix=/usr --without-modules --disable-static --enable-openmp \
              --with-x --with-wmf --with-openexr --with-xml \
              --with-gslib --with-gs-font-dir=/usr/share/fonts/Type1 \
              --with-perl --with-perl-options="INSTALLDIRS=vendor" \
              --without-gvc --without-djvu --with-jp2 \
              --without-jbig --without-fpx --without-dps || return 1

  make || return 1
  make DESTDIR=${pkgdir} install || return 1
  install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE || return 1
  install -D -m644 NOTICE ${pkgdir}/usr/share/licenses/${pkgname}/NOTICE || return 1

  #Cleaning
  find ${pkgdir} -name '*.bs' -exec rm {} \; || return 1
  find ${pkgdir} -name '.packlist' -exec rm {} \; || return 1
  find ${pkgdir} -name 'perllocal.pod' -exec rm {} \; || return 1
}
