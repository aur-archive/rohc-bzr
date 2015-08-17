# Maintainer: Audric Schiltknecht <storm+arch@chemicalstorm.org>

pkgname=('rohc-bzr')
pkgver=762
pkgrel=1
pkgdesc="RObust Header Compression (ROHC) library"
arch=('i686' 'x86_64')
depends=('glibc')
makedepends=('bzr libpcap')
provides=('rohc')
conflicts=('rohc')
options=('!libtool')
url="http://rohc-lib.org/"
license=('GPL')

_bzrtrunk=lp:rohc
_bzrmod=rohc

prepare() {
  cd "$srcdir"
  msg "Connecting to Bazaar server...."

  if [[ -d "$_bzrmod" ]]; then
    cd "$_bzrmod" && bzr pull "${_bzrtrunk}"
    msg "The local files are updated."
  else
    bzr branch "${_bzrtrunk}" "${_bzrmod}"
  fi

  msg "Bazaar checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_bzrmod-build"
  cp -r "$srcdir/$_bzrmod" "$srcdir/$_bzrmod-build"
  cd "$srcdir/$_bzrmod-build"

  rm -f config.cache
  rm -f config.log

  aclocal
  libtoolize --force
  autoconf
  autoheader
  automake --add-missing

  chmod +x ./configure
}

build() {
  cd "$srcdir/$_bzrmod-build"
  ./configure --prefix=/usr \
              --disable-rohc-debug \
              --enable-rohc-tests \
              --disable-static
  make
}

package() {
  cd "$srcdir/$_bzrmod-build"
  make DESTDIR="${pkgdir}/" install
}

check() {
  cd "$srcdir/$_bzrmod-build"
  make -k check
}

# vim:set ts=2 sw=2 et:
