# Maintainer:  Vitalii Kuzhdin <vitaliikuzhdin@gmail.com>

_basename="xf86-video-dummy"
pkgname="${_basename//xf86/xlibre}"
pkgver=0.4.1.1
pkgrel=1
pkgdesc="XLibre dummy video driver"
arch=('aarch64' 'x86_64')
url="https://github.com/X11Libre/${_basename}"
license=('MIT AND X11')
depends=('glibc')
makedepends=('systemd' 'xlibre-server-devel' 'xorgproto' 'X-ABI-VIDEODRV_VERSION=28.0')
provides=("${_basename}")
conflicts=("${_basename}" 'xorg-server<1.20.0' 'X-ABI-VIDEODRV_VERSION<28' 'X-ABI-VIDEODRV_VERSION>=29')
groups=('xlibre-drivers')
_pkgsrc="${_basename}-xlibre-${_basename}-${pkgver}"
source=("${_pkgsrc}.tar.gz::${url}/archive/refs/tags/xlibre-${_basename}-${pkgver}.tar.gz")
b2sums=('f15f8b5461a56dbff429266873e2026b454d8d05547cd9b154f818d317d5ad8ed0cb8e708baa871733aa6a977319e240038b7bbacb8494ff68073d21665faa8d')

build() {
  # Since pacman 5.0.2-2, hardened flags are now enabled in makepkg.conf
  # With them, modules fail to load with undefined symbol.
  # See https://bugs.archlinux.org/task/55102 / https://bugs.archlinux.org/task/54845
  export CFLAGS="${CFLAGS/-fno-plt}"
  export CXXFLAGS="${CXXFLAGS/-fno-plt}"
  export LDFLAGS="${LDFLAGS/-Wl,-z,now}"
  local configure_options=(
    --prefix='/usr'
  )

  cd "${srcdir}/${_pkgsrc}"
  autoreconf -vfi
  ./configure "${configure_options[@]}"
  make
}

package() {
  cd "${srcdir}/${_pkgsrc}"
  make DESTDIR="${pkgdir}" install

  install -vDm644 "README.md" "${pkgdir}/usr/share/doc/${pkgname}/README.md"
  install -vDm644 "COPYING"   "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}
