# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Dag Odenhall <dag.odenhall@gmail.com>
# Contributor: Grigorios Bouzakis <grbzks@gmail.com>

pkgname=dwm
pkgver=6.4
pkgrel=1
pkgdesc="A dynamic window manager for X"
url="https://dwm.suckless.org"
arch=('i686' 'x86_64')
license=('MIT')
options=(zipman)
depends=('libx11' 'libxinerama' 'libxft' 'freetype2' 'st' 'dmenu')
install=dwm.install
source=(dwm.desktop
        https://dl.suckless.org/dwm/dwm-$pkgver.tar.gz)
sha256sums=('bc36426772e1471d6dd8c8aed91f288e16949e3463a9933fee6390ee0ccd3f81'
            'fa9c0d69a584485076cfc18809fd705e5c2080dafb13d5e729a3646ca7703a6e')
_sourcedir=$pkgname-$pkgver

prepare() {

  # This package provides a mechanism to provide a custom config.h. Multiple
  # configuration states are determined by the presence of two files in
  # $BUILDDIR:
  #
  # config.h  config.def.h  state
  # ========  ============  =====
  # absent    absent        Initial state. The user receives a message on how
  #                         to configure this package.
  # absent    present       The user was previously made aware of the
  #                         configuration options and has not made any
  #                         configuration changes. The package is built using
  #                         default values.
  # present                 The user has supplied his or her configuration. The
  #                         file will be copied to $_sourcedir and used during
  #                         build.
  #
  # After this test, config.def.h is copied from $_sourcedir to $BUILDDIR to
  # provide an up to date template for the user.
  
  [ -e "$BUILDDIR/config.h" ] && cp "$BUILDDIR/config.h" "$_sourcedir" && [ ! -e "$BUILDDIR/config.def.h" ] && echo \
      'This package can be configured in config.h. Copy the config.def.h that' \
      'was just placed into the package directory to config.h and modify it' \
      'to change the configuration. Or just leave it alone to continue to use' \
      'default values.'
  
  cp "$_sourcedir/config.def.h" "$BUILDDIR"


}

build() {
  cd "$srcdir/$pkgname-$pkgver"
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11 FREETYPEINC=/usr/include/freetype2
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  make PREFIX=/usr DESTDIR="$pkgdir" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/$pkgname/README"
  install -Dm644 "$srcdir/dwm.desktop" "$pkgdir/usr/share/xsessions/dwm.desktop"
}
