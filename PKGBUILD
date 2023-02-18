# Maintainer: Nick Egan <ntegan1+2@gmail.com>
pkgname=plotjuggler-openpilot-git
# git rev-parse --short HEAD

pkgcommit=59bbc62f

pkgver=3.6.0.${pkgcommit}
pkgrel=1
pkgdesc="The Time Series Visualization Tool that you deserve. Without ROS dependencies."
arch=('x86_64')
url="https://github.com/commaai/plotjuggler"
# License? https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=plotjuggler
#license=('MPL-2.0') # pj upstream license
license=('unknown')
groups=()
pjdepends=('binutils' 'qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-websockets' 'arrow' 'qtav')
opdepends=('opencl-headers' 'zmq' 'capnproto')
depends=(${pjdepends[@]} ${opdepends[@]})
makedepends=('cmake' 'clang' 'scons' 'python-numpy')
optdepends=()
folder=plotjuggler
source=("${folder}::git+${url}#commit=${pkgcommit}")
md5sums=('SKIP')
prepare() {
  pjdir="${srcdir}/${folder}"
  pjbuilddir="${srcdir}/${folder}build"
  printf '%s\n' '  -> Initializing submodules...'
  git -C "${pjdir}" submodule set-url 3rdparty/opendbc 'https://github.com/commaai/opendbc'
  git -C "${pjdir}" submodule set-url 3rdparty/cereal 'https://github.com/commaai/cereal'
  git -C "${pjdir}" submodule update --init

  printf '%s\n' '  -> Initializing openpilot python requirements venv...'
  python3 -m venv "${pjdir}/venv"
  (
    source "${pjdir}/venv/bin/activate"
    pip3 install pkgconfig jinja2 Cython && pip3 install --no-cache-dir -r <(grep -v 'Cython\|numpy' "${pjdir}/3rdparty/opendbc/requirements.txt")

    printf '%s\n' '  -> Build openpilot submodules and cmake generate...'
    cmake -S "${pjdir}" -B "${pjbuilddir}" -DCMAKE_INSTALL_PREFIX="${pkgdir}/usr" -DCMAKE_BUILD_TYPE=Release
    deactivate
  )
}
build() {
  pjdir="${srcdir}/${folder}"
  pjbuilddir="${srcdir}/${folder}build"
  printf '%s\n' '  -> Building...'
  cmake --build "${pjbuilddir}" --config Release --parallel 2
}
package() {
  pjdir="${srcdir}/${folder}"
  pjbuilddir="${srcdir}/${folder}build"

  printf '%s\n' '  -> Installing...'
  # https://stackoverflow.com/questions/52993166/cmake-target-install-without-build-command-line
  #cmake --build "${pjbuilddir}" --config Release --parallel 2 --target install
  #cmake --install "${pjbuilddir}"
  cmake --build "${pjbuilddir}" --config Release --target install/fast;

  printf '%s\n' '  -> Generating binary wrapper for op cereal dir...'
  mv "${pkgdir}/usr/bin/plotjuggler" "${pkgdir}/usr/bin/pplotjuggler.bin";
  echo '#!/bin/bash' > "${pkgdir}"/usr/bin/plotjuggler;
  echo 'export BASEDIR=/usr/lib/openpilot' >> "${pkgdir}"/usr/bin/plotjuggler;
  echo 'pplotjuggler.bin $@' >> "${pkgdir}"/usr/bin/plotjuggler;
  chmod +x "${pkgdir}"/usr/bin/plotjuggler;

  printf '%s\n' '  -> Copying openpilot libraries...'
  mkdir -p "${pkgdir}/usr/lib/openpilot"
  cp -r "${pjdir}/3rdparty/cereal" "${pkgdir}/usr/lib/openpilot"
  cp -r "${pjdir}/3rdparty/opendbc" "${pkgdir}/usr/lib/openpilot"
}

