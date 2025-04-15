_realname=raja
pkgbase=raja-w64-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}")
pkgver=2025.03.0
pkgrel=1
pkgdesc='RAJA Performance Portability Layer (C++)'
arch=('any')
mingw_arch=('mingw64' 'ucrt64')
url='https://github.com/LLNL/RAJA'
license=('spdx:BSD-3-Clause')
depends=()
makedepends=("${MINGW_PACKAGE_PREFIX}-cc"
              "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-cmake")
source=("git+${url}.git#tag=v${pkgver}")
sha256sums=('SKIP')

prepare() {
  cd RAJA
  git submodule init
  git submodule update
}

build() {
  #Static Build
  mkdir -p "${srcdir}/build-${MSYSTEM}-static" && cd "${srcdir}/build-${MSYSTEM}-static"

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
    ${MINGW_PREFIX}/bin/cmake \
      -GNinja \
      -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
      -DBUILD_SHARED_LIBS=OFF \
      ../${_realname}

  ${MINGW_PREFIX}/bin/cmake --build .

  # Shared Build
  mkdir -p "${srcdir}/build-${MSYSTEM}-shared" && cd "${srcdir}/build-${MSYSTEM}-shared"

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
    ${MINGW_PREFIX}/bin/cmake \
      -GNinja \
      -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
      -DBUILD_SHARED_LIBS=ON \
      ../${_realname}

  ${MINGW_PREFIX}/bin/cmake --build .
}

package() {
  #Static Install
  cd "${srcdir}/build-${MSYSTEM}-static"
  DESTDIR=${pkgdir} ${MINGW_PREFIX}/bin/cmake --install .

  #Shared Install
  cd "${srcdir}/build-${MSYSTEM}-shared"
  DESTDIR=${pkgdir} ${MINGW_PREFIX}/bin/cmake --install .

  # local PREFIX_WIN=$(cygpath -wm ${MINGW_PREFIX})
  # for _f in $(find "${pkgdir}${MINGW_PREFIX}"/lib/cmake -type f); do
  #   sed -e "s|${PREFIX_WIN}|\$\{_IMPORT_PREFIX\}|g" -i ${_f}
  #   sed -e "s|${MINGW_PREFIX}|\$\{_IMPORT_PREFIX\}|g" -i ${_f}
  # done
}
