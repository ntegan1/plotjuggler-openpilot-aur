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

# cmake -S pj -B pjbuild -DCMAKE_INSTALL_PREFIX=pjinstall  -DCMAKE_BUILD_TYPE=Release
#cmake --build pjbuild --config Release --parallel --target install

makedepends=('cmake' 'clang')
#optdepends=('ed: for "patch -e" functionality')
optdepends=()
# https://wiki.archlinux.org/title/VCS_package_guidelines#VCS_sources
#source=("ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz"{,.sig})
folder=plotjuggler
#source=("${folder}::git+${url}#branch=comma-master")
source=("${folder}::git+${url}#commit=${pkgcommit}")
md5sums=('SKIP')
#python3 -m venv env
#source ./env/bin/activate
#pip3 install pkgconfig jinja2 Cython && pip3 install --no-cache-dir -r <(

prepare() {
  # from ffmpeg-git PKGBUILD
  #printf '%s\n' '  -> Running ffmpeg configure script...'
  printf '%s\n' '  -> Initializing submodules...'
  git -C "${srcdir}/${folder}" submodule set-url 3rdparty/opendbc 'https://github.com/commaai/opendbc'
  git -C "${srcdir}/${folder}" submodule set-url 3rdparty/cereal 'https://github.com/commaai/cereal'
  git -C "${srcdir}/${folder}" submodule update --init
}
build() {
  # cd $srcdir/$pkgname-$pkgver; configure --prefix=/usr; make
  echo "${srcdir}"
}
package() {
  echo $pkgdir
  # cd srcdir; make DESTDIR=$pkgdir/ install
}
