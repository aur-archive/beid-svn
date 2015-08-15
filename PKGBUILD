# Contributor: Stijn Segers <francesco dot borromini at gmail dot com>

pkgname=beid-svn
pkgver=1259
pkgrel=1
pkgdesc="The eID Middleware software for using the Belgian eID on your computer"
arch=('x86_64' 'i686')
url="http://code.google.com/p/eid-mw/"
license=('LGPL')
depends=('pcsclite' 'gtk2')
makedepends=('svn' 'ccache' 'java-runtime')
conflicts=('beid')
replaces=('beid')
install='beid-svn.install'
options=('makeflags')
source=('gcc-4.7.patch')
changelog=$pkgname.changelog

_svnmod=eid-mw-read-only

build() {

    _svntrunk=http://eid-mw.googlecode.com/svn/trunk/

    cd $srcdir/
    if [ -d $_svnmod/.svn ]; then
        msg "SVN tree found, reverting changes and updating to -r$pkgver"
        (cd $_svnmod && svn revert -R . && make distclean; svn up -r $pkgver)
    else
        msg "Checking out SVN tree of -r$pkgver"
        svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
    fi
    
    cd "$srcdir/$_svnmod"
    
    # This is temporary (but filthy) - we need to add the java compiler's location to the PATH
    # in order to use it. Since sensible people have only one java environment installed, we 
    # won't be doing any checking on whether we source twice instead of just once. 
    [ -e /etc/profile.d/jre.sh ] && . /etc/profile.d/jre.sh

    # Patch so GCC 4.7 won't complain
    msg "Applying GCC 4.7 compatibility patch"
    patch -Np1 -i $srcdir/${source[0]}

    # Bootstrap
    msg "Bootstrapping, might take a bit..."
    autoreconf -i --force

    # Run configure
    msg "Configuring beid"
    ./configure --prefix=/usr
    
    # Build
    msg "Running make" 
    make

}

package() {

    cd "$srcdir/$_svnmod"
    msg "Running make install" 
    make DESTDIR=$pkgdir install

}

sha1sums=('2fce498862eccff25aec3ab38b3b481899777e95')
