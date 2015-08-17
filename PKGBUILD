# Contributor: Christofer Bertonha <chritoferbertonha@gmail.com>
# Maintainer: Felix Yan <felixonmars@gmail.com>

pkgname=vtk-python-git
pkgver=20120106
pkgrel=1
pkgdesc='A software system for 3D computer graphics, image processing, and visualization which supports a wide variety of visualization algorithms and advanced modeling techniques.'
arch=('i686' 'x86_64')
url='http://www.vtk.org/'
license=('BSD')
depends=('python2' 'zlib' 'qt' 'libpng' 'libtiff' 'libxml2' 'expat' 'nvidia-cg-toolkit' 'boost-libs' 'libjpeg-turbo' 'hdf5')
makedepends=('cmake' 'python2' 'zlib' 'qt' 'libpng' 'libtiff' 'libxml2' 'expat' 'nvidia-cg-toolkit' 'boost' 'libjpeg-turbo' 'hdf5')
provides=('vtk=$pkgver')
conflicts=('vtk')
source=()
md5sums=()

_gitroot="git://vtk.org/VTK.git"
_gitname="VTK"
_libver="5.9"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
    cd ..
  else
    git clone $_gitroot
  fi
  
  rm -rf "$srcdir/$_gitname-build"
  cp -r "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"
  
  msg "GIT checkout done or server timeout"
  msg "Starting make..."
  
  cmake \
    -DCMAKE_BUILD_TYPE:STRING=Release \
    -DCMAKE_INSTALL_PREFIX:FILEPATH=/usr \
    -DBUILD_TESTING:BOOL=OFF \
    -DBUILD_EXAMPLES:BOOL=OFF \
    -DBUILD_SHARED_LIBS:BOOL=ON \
    -DVTK_USE_CG_SHADERS:BOOL=ON\
    -DVTK_USE_BOOST:BOOL=ON \
    -DVTK_USE_QT:BOOL=ON \
    -DVTK_USE_TK:BOOL=OFF \
    -DVTK_USE_QVTK_QTOPENGL:BOOL=ON \
    -DVTK_USE_SYSTEM_EXPAT:BOOL=ON \
    -DVTK_USE_SYSTEM_FREETYPE:BOOL=ON \
    -DVTK_USE_SYSTEM_HDF5:BOOL=ON \
    -DVTK_USE_SYSTEM_JPEG:BOOL=ON \
    -DVTK_USE_SYSTEM_LIBXML2:BOOL=ON \
    -DVTK_USE_SYSTEM_PNG:BOOL=ON \
    -DVTK_USE_SYSTEM_TIFF:BOOL=ON \
    -DVTK_USE_SYSTEM_ZLIB:BOOL=ON \
    -DVTK_WRAP_PYTHON:BOOL=ON \
    -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
    -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
    -DPYTHON_EXECUTABLE=/usr/bin/python2 \
    -DVTK_PYTHON_SETUP_ARGS:STRING=--root=${pkgdir} \
    ../$_gitname

  make
}

package() {
  cd ${srcdir}/$_gitname-build

  make DESTDIR=${pkgdir} install
  
  # Put an entry in /etc/ld.so.conf.d
  install -dv ${pkgdir}/etc/ld.so.conf.d
  echo "/usr/lib/vtk-${_libver}" > ${pkgdir}/etc/ld.so.conf.d/vtk.conf
}
