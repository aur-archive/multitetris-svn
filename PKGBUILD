# Maintainer: Padfoot <padfoot at exemail dot com dot au>

pkgname=multitetris-svn
pkgver=6432
pkgrel=2
pkgdesc="Multitouch Tetris style game."
arch=('any')
url="http://multitetris.com/"
license=('GPL3')
depends=('libavg'
         'python2'
         'hicolor-icon-theme')
makedepends=('subversion'
             'gtk-update-icon-cache')
install='multitet.install'
source=("multitet.desktop")
md5sums=('311dcbfdca0444e47876f690cf3c9da9')

_svntrunk=https://www.libavg.de/svn/trunk/avg_media/mtc/multitet
_svnmod=multitet

build() {
    cd "$srcdir"
    msg "Connecting to SVN server..."

    if [[ -d "$_svnmod/.svn" ]]; then
        (cd "$_svnmod" && svn up -r "$pkgver")
    else
        svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
    fi

    msg "SVN checkout done or server timeout"
  
    rm -rf "$srcdir/$_svnmod-build"
    cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
    cd "$srcdir/$_svnmod-build"
  
    sed -i "s/python/python2/g" multitet/*.py
    sed -i "s/python/python2/g" scripts/multitet
}

package() {
    _python_libs=`python2 -c "from distutils import sysconfig; print sysconfig.get_python_lib()"`
    
    mkdir -p \
        "$pkgdir/usr/bin" \
        "${pkgdir}${_python_libs}" \
        "$pkgdir/usr/share/icons/hicolor/scalable/apps" \
        "$pkgdir/usr/share/applications" || return 1

    cd "$srcdir/$_svnmod-build"
    cp -r multitet "${pkgdir}${_python_libs}"
    cp multitet/media/falling-Z.png "$pkgdir/usr/share/icons/hicolor/scalable/apps/multitet.png"
    
    cd ./scripts
    install -D -m755 multitet "$pkgdir/usr/bin"
    
    cd "$srcdir"
    install -D -m755 multitet.desktop "$pkgdir/usr/share/applications"
}
