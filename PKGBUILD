# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Maxim Baz <$pkgname at maximbaz dot com>
# Contributor: Fabio 'Lolix' Loli <lolix@disroot.org> -> https://github.com/FabioLolix
# Contributor: Maximilian Kindshofer <maximilian@kindshofer.net>

pkgbase=kitty
pkgname=(kitty kitty-terminfo kitty-shell-integration)
pkgver=0.24.4
pkgrel=1
pkgdesc="A modern, hackable, featureful, OpenGL-based terminal emulator"
arch=('x86_64')
url="https://github.com/kovidgoyal/kitty"
license=('GPL3')
depends=('python3' 'freetype2'  'fontconfig' 'wayland' 'libx11' 'libxkbcommon-x11' 'libxi'
         'hicolor-icon-theme' 'libgl' 'dbus' 'lcms2')
makedepends=('libxinerama' 'libxcursor' 'libxrandr' 'wayland-protocols' 'librsync')
source=("${pkgname}-${pkgver}.tar.xz::https://github.com/kovidgoyal/${pkgbase}/releases/download/v${pkgver}/${pkgbase}-${pkgver}.tar.xz"
        "${pkgname}-${pkgver}.tar.xz.sig::https://github.com/kovidgoyal/${pkgbase}/releases/download/v${pkgver}/${pkgbase}-${pkgver}.tar.xz.sig")
sha512sums=('28b1e2415b5c73ad8922d4ed17a63638c497a91c9aedba71c2f53e77fc89ed49951a2e873bba5bbe85c8eda22107d9ff4e4321010ac33a2d289c2fbd5d3a5b8f'
            'SKIP')
validpgpkeys=('3CE1780F78DD88DF45194FD706BC317B515ACE7C') # Kovid Goyal

build() {
  cd "$srcdir/$pkgname-$pkgver"
  python3 setup.py linux-package --update-check-interval=0
}

package_kitty() {
  depends+=('kitty-terminfo' 'kitty-shell-integration')
  optdepends=('imagemagick: viewing images with icat'
              'libcanberra: playing "bell" sound on terminal bell')

  cd "$srcdir/$pkgname-$pkgver"

  cp -r linux-package "${pkgdir}"/usr

  # completions
  python __main__.py + complete setup bash | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/bash-completion/completions/kitty
  python __main__.py + complete setup fish | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/fish/vendor_completions.d/kitty.fish
  python __main__.py + complete setup zsh  | install -Dm644 /dev/stdin "${pkgdir}"/usr/share/zsh/site-functions/_kitty

  install -Dm644 "${pkgdir}"/usr/share/icons/hicolor/256x256/apps/kitty.png "${pkgdir}"/usr/share/pixmaps/kitty.png

  rm -r "$pkgdir"/usr/share/terminfo
  rm -r "$pkgdir"/usr/lib/kitty/shell-integration

  install -Dm644 docs/_build/html/_downloads/*/kitty.conf "${pkgdir}"/usr/share/doc/${pkgname}/kitty.conf
}

package_kitty-terminfo() {
  pkgdesc='Terminfo for kitty, an OpenGL-based terminal emulator'
  depends=('ncurses')

  mkdir -p "$pkgdir/usr/share/terminfo"
  tic -x -o "$pkgdir/usr/share/terminfo" $pkgbase-$pkgver/terminfo/kitty.terminfo
}

package_kitty-shell-integration() {
  pkgdesc='Shell integration scripts for kitty, an OpenGL-based terminal emulator'

  mkdir -p "$pkgdir/usr/lib/kitty/"
  cp -r "$srcdir/$pkgbase-$pkgver/shell-integration" "$pkgdir/usr/lib/kitty/"
}
