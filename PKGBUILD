# Maintainer: Joe User <joe.user@example.com>

pkgname=patch
pkgver=2.7.1
pkgrel=1
pkgdesc="A utility to apply patch files to original sources"
arch=('i686' 'x86_64')
url="https://www.gnu.org/software/patch/patch.html"
license=('GPL')
groups=('base-devel')
#depends=('glibc')
depends=('opencl-headers')
#sudo apt -y install qtbase5-dev libqt5svg5-dev libqt5websockets5-dev \
#     libqt5opengl5-dev libqt5x11extras5-dev libprotoc-dev libzmq-dev
#cmake -DCMAKE_BUILD_TYPE=Release ..
#cmake -S src/PlotJuggler -B build/PlotJuggler -DCMAKE_INSTALL_PREFIX=install
#cmake --build build/PlotJuggler --config RelWithDebInfo --parallel --target install


# cmake -S pj -B pjbuild -DCMAKE_INSTALL_PREFIX=pjinstall  -DCMAKE_BUILD_TYPE=Release
#cmake --build pjbuild --config Release --parallel --target install

makedepends=('ed')
optdepends=('ed: for "patch -e" functionality')
source=("ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz"{,.sig})
md5sums=('e9ae5393426d3ad783a300a338c09b72'
         'SKIP')
#python3 -m venv env
#source ./env/bin/activate
#pip3 install pkgconfig jinja2 Cython && pip3 install --no-cache-dir -r <(

build() {
        cd "$srcdir/$pkgname-$pkgver"
        ./configure --prefix=/usr
        make
}
package() {
        cd "$srcdir/$pkgname-$pkgver"
        make DESTDIR="$pkgdir/" install
}
