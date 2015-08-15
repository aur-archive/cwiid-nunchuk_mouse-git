#cwiid-git Contributor:Andrea Tarocchi <valdar@email.it> 
#Maintainer: Jan <jan@next-game.de>
pkgname=cwiid-nunchuk_mouse-git
#pkgver=`date +%Y%m%d`
pkgver=20121230
pkgrel=1
pkgdesc="Cwiid-git with the nunchuk_mouse plugin"
arch=(i686 x86_64)
url="http://github.com/abstrakraft/cwiid"
license=('GPL')

depends=('bluez>=4' 'gtk2' 'python2')
makedepends=('git' 'make')
conflicts=('cwiid')
provides=('cwiid' )
install=cwiid.install
source=(fix_libbluethoot.patch nunchuk_mouse-plugin.patch)
md5sums=('d92745847f5ba534c2914be1d2b36092' '77753f90823e82ffc51a9354c4bfc5c2')

_gitroot=git://github.com/abstrakraft/cwiid.git
_gitname=cwiid
_builddir=$srcdir/$_gitname-build

build() {
  export PYTHON=python2
  export LDFLAGS=

  cd $srcdir/
  msg "Connecting to github.com GIT server...."
  if [ -d $srcdir/$_gitname ] ; then
    pushd $_gitname && git pull origin && popd
    msg "The local files are updated."
  else
    git clone $_gitroot
  fi
  msg "GIT checkout done or server timeout"
  msg "Starting make..."
  rm -rf $_gitname-build
  git clone $_gitname $_gitname-build

  cd $_builddir
  
  git am --signoff < ../../fix_libbluethoot.patch
  patch -p0 < ../../nunchuk_mouse-plugin.patch
  
  aclocal
  autoreconf
  ./configure --prefix=/usr --sysconfdir=/etc --with-python=python`python2 --version 2>&1 | cut -d ' ' -f2` --disable-ldconfig
# OLD CONFIG OPTIONS:
# usefull options: LDFLAGS=-L/usr/lib/python2.7

  make
# OLD MAKE OPTIONS:
# CPPFLAGS=-I/usr/include/python2.7 || return 1
  make DESTDIR="${pkgdir}" install 

# OLD BUILD WAY:
#  aclocal
#  autoreconf
  
#  ./configure --disable-ldconfig --prefix=/usr --sysconfdir=/etc --with-cwiid-plugins-dir=/lib/cwiid/plugins
#  make LDFLAGS=-L../libcwiid || return 1

#  install -d $startdir/pkg/usr/bin
#  install -d $startdir/pkg/etc
#  install -d $startdir/pkg/usr/lib
#  install -d $startdir/pkg/usr/include

#  make install LDFLAGS=-L../libcwiid prefix=$startdir/pkg/usr sysconfdir=$startdir/pkg/etc install

#  install -D -m644 ./wminput/README $pkgdir/usr/share/doc/cwiid/wminput
}
