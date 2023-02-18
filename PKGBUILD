# Maintainer: Nick Egan <ntegan1+2@gmail.com>

pkgname=plotjuggler-openpilot-git
#ntegan@ideapad ~/comma/pj (git)-[comma-master] % git rev-parse --short HEAD
pkgcommit=59bbc62f

pkgver=3.6.0.${pkgcommit}
pkgrel=1
pkgdesc="The Time Series Visualization Tool that you deserve. Without ROS dependencies."
arch=('x86_64')
url="https://github.com/commaai/plotjuggler"
# License? https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=plotjuggler
#license=('MPL-2.0') # pj upstream license
license=('unknown')
#groups=('base-devel')
groups=()
#sudo apt -y install qtbase5-dev libqt5svg5-dev libqt5websockets5-dev \
#     libqt5opengl5-dev libqt5x11extras5-dev libprotoc-dev libzmq-dev
pjdepends=('binutils' 'qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-websockets' 'arrow' 'qtav')
opdepends=('opencl-headers')
depends=(${pjdepends[@]} ${opdepends[@]})
#cmake -DCMAKE_BUILD_TYPE=Release ..
#cmake -S src/PlotJuggler -B build/PlotJuggler -DCMAKE_INSTALL_PREFIX=install
#cmake --build build/PlotJuggler --config RelWithDebInfo --parallel --target install

makedepends=('cmake' 'clang' 'scons')
#optdepends=('ed: for "patch -e" functionality')
optdepends=()
# https://wiki.archlinux.org/title/VCS_package_guidelines#VCS_sources
#source=("ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz"{,.sig})
folder=plotjuggler
#source=("${folder}::git+${url}#branch=comma-master")
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
  source "${pjdir}/venv/bin/activate"
  # from comma pj Dockerfile # maybe install archlinux numpy instead
  pip3 install pkgconfig jinja2 Cython && pip3 install --no-cache-dir -r <(grep -v Cython "${pjdir}/3rdparty/opendbc/requirements.txt")

  printf '%s\n' '  -> Build openpilot submodules and cmake generate...'
  cmake -S "${pjdir}" -B "${pjbuilddir}" -DCMAKE_INSTALL_PREFIX="${pkgdir}/usr"  -DCMAKE_BUILD_TYPE=Release
  deactivate
}
build() {
  pjdir="${srcdir}/${folder}"
  pjbuilddir="${srcdir}/${folder}build"
  # cd $srcdir/$pkgname-$pkgver; configure --prefix=/usr; make
  echo "${srcdir}"
  #cmake --build pjbuild --config Release --parallel --target install
  #mkdir -p "${pkgdir}"
  #chmod 600 "${pkgdir}/.."
  #chmod 600 "${pkgdir}"
  cmake --build "${pjbuilddir}" --config Release --parallel 2
}
package() {
  pjdir="${srcdir}/${folder}"
  pjbuilddir="${srcdir}/${folder}build"
  cmake --build "${pjbuilddir}" --config Release --parallel 2 --target install
  # todo: set BASEDIR for dataload rlog
  mkdir -p "${pkgdir}/usr/lib/openpilot"
  cp -r "${pjdir}/3rdparty/cereal" "${pkgdir}/usr/lib/openpilot"
  cp -r "${pjdir}/3rdparty/opendbc" "${pkgdir}/usr/lib/openpilot"
  echo $pkgdir
  # cd srcdir; make DESTDIR=$pkgdir/ install
}
