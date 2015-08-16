# $Id: PKGBUILD 147983 2012-01-29 11:26:22Z ronald $
# Maintainer: Ronald van Haren <ronald.archlinux.org>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: damir <damir@archlinux.org>
# Contributor: Tom K <tomk@runbox.com>
# Contributor: Jed Brown <jed@59A2.org>
# Contributor: Simone Pezzuto <junki.gnu@gmail.com>

pkgname=hdf5-openmpi-fortran
_pkgname=hdf5
pkgver=1.8.14
pkgrel=1
arch=('i686' 'x86_64')
pkgdesc="General purpose library and file format for storing scientific data (OpenMPI version)"
url="http://www.hdfgroup.org/HDF5/"
license=('custom')
depends=('zlib' 'sh' 'openmpi' 'gcc-libs')
makedepends=('time' 'gcc-fortran')
provides=('hdf5' 'hdf5-openmpi')
conflicts=('hdf5' 'hdf5-openmpi')
source=(ftp://ftp.hdfgroup.org/HDF5/current/src/${_pkgname}-${pkgver/_/-}.tar.bz2 
mpi.patch) 
sha1sums=('3c48bcb0d5fb21a3aa425ed035c08d8da3d5483a'
          '658d4a3e537c9c76da3200effa8f95b656a21936')

build() {
  cd "$srcdir/${_pkgname}-${pkgver/_/-}"

  # FS#33343
  patch -Np1 -i "${srcdir}/mpi.patch"

  export CFLAGS="${CFLAGS/O2/O0}"
  export CXXFLAGS="${CFLAGS}"
  ./configure \
    CXX="mpicxx" \
    CC="mpicc" \
    FC="mpif90" \
    F9X="mpif90" \
    RUNPARALLEL="mpirun" \
    OMPI_MCA_disable_memory_allocator=1 \
    --prefix=/usr \
    --with-pthread=/usr/lib/ \
    --enable-linux-lfs \
    --enable-unsupported \
    --enable-shared \
    --disable-static \
    --enable-production=yes \
    --with-zlib \
    --with-default-api-version=v18 \
    --enable-parallel=yes \
    --disable-sharedlib-rpath \
    --enable-fortran \
    --enable-fortran2003
  make
}

package() {
  cd "$srcdir/${_pkgname}-${pkgver/_/-}"

  make -j1 DESTDIR="${pkgdir}" install

  install -d -m755 "$pkgdir/usr/share/$_pkgname"
  mv "$pkgdir"/usr/share/{hdf5_examples,$_pkgname/examples}

  install -d -m755 "$pkgdir/usr/share/licenses/${pkgname}"
  install -m644 "$srcdir/${_pkgname}-${pkgver/_/-}/COPYING" \
          "$pkgdir/usr/share/licenses/${pkgname}/LICENSE" 
}

