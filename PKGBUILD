# Maintainer: Sven-Hendrik Haase <svenstaro@gmail.com>
# Maintainer: Maxim Baz <$pkgname at maximbaz dot com>
# Contributor: Fabio 'Lolix' Loli <lolix@disroot.org> -> https://github.com/FabioLolix
# Contributor: Maximilian Kindshofer <maximilian@kindshofer.net>

pkgbase=kitty
pkgname=(kitty kitty-terminfo)
pkgver=0.20.3
pkgrel=1
pkgdesc="A modern, hackable, featureful, OpenGL-based terminal emulator"
arch=('x86_64')
url="https://github.com/kovidgoyal/kitty"
license=('GPL3')
depends=('python3' 'freetype2'  'fontconfig' 'wayland' 'libx11' 'libxkbcommon-x11' 'libxi'
         'hicolor-icon-theme' 'libgl' 'libcanberra' 'dbus' 'lcms2')
makedepends=('libxinerama' 'libxcursor' 'libxrandr' 'wayland-protocols')
source=("${pkgname}-${pkgver}.tar.xz::https://github.com/kovidgoyal/${pkgbase}/releases/download/v${pkgver}/${pkgbase}-${pkgver}.tar.xz"
        "${pkgname}-${pkgver}.tar.xz.sig::https://github.com/kovidgoyal/${pkgbase}/releases/download/v${pkgver}/${pkgbase}-${pkgver}.tar.xz.sig")
sha512sums=('9e9d81696bcb0d3894c04b08bc52ccf7aaf533125b3ecfb4c894b8d42dbf8511239c49f1c977d81872ce49fc6b3aa8e8c92b9c944bd31fe59fce6e3750295e58'
            'SKIP')
validpgpkeys=('3CE1780F78DD88DF45194FD706BC317B515ACE7C') # Kovid Goyal

build() {
  cd "$srcdir/$pkgname-$pkgver"
  python3 setup.py linux-package --update-check-interval=0
}

package_kitty() {
  depends+=('kitty-terminfo')
  optdepends=('imagemagick: viewing images with icat')

  cd "$srcdir/$pkgname-$pkgver"

  cp -r linux-package "${pkgdir}"/usr

  # completions
  python __main__.py + complete setup bash | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/bash-completion/completions/kitty
  python __main__.py + complete setup fish | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/fish/vendor_completions.d/kitty.fish
  # doesn't know how to http://zsh.sourceforge.net/Doc/Release/Completion-System.html#Autoloaded-files
  # so we write our own header
  {
      echo "#compdef kitty"
      python __main__.py + complete setup zsh
  } | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/zsh/site-functions/_kitty

  install -Dm644 "${pkgdir}"/usr/share/icons/hicolor/256x256/apps/kitty.png "${pkgdir}"/usr/share/pixmaps/kitty.png

  rm -r "$pkgdir"/usr/share/terminfo

  install -Dm644 docs/_build/html/_downloads/*/kitty.conf "${pkgdir}"/usr/share/doc/${pkgname}/kitty.conf
}

package_kitty-terminfo() {
  pkgdesc='Terminfo for kitty, an OpenGL-based terminal emulator'
  depends=('ncurses')

  mkdir -p "$pkgdir/usr/share/terminfo"
  tic -x -o "$pkgdir/usr/share/terminfo" $pkgbase-$pkgver/terminfo/kitty.terminfo
}
