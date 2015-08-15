# Maintainer: Spyhawk
#
# Full 64 bit ET:Legacy clients can only connect to ET:Legacy servers running mods providing a 64 bit version.
# If you want to connect to existing  32 bit only mods, install the cross-compiled "etlegacy" package instead.
# Omni-Bot server support requires the 32 bit cross-compiled version.
# The engine is dynamically compiled, while the official precompiled mod libraries are provided.

_pkgbase=etlegacy
pkgbase=etlegacy64
pkgname=('etlegacy64' 'etlegacy64-mod')
pkgver=2.72
pkgrel=3
arch=('x86_64')
url="http://www.etlegacy.com/"
license=('GPL3' 'custom')
makedepends=('cmake' 'zip' 'alsa-lib' 'curl' 'freetype2' 'gcc-libs' 'glew' 'libjpeg-turbo' 'libvorbis' 'sdl2')
source=("https://github.com/etlegacy/$_pkgbase/archive/v$pkgver.tar.gz"
        "http://mirror.etlegacy.com/release/etlegacy-v$pkgver-x86_64.tar.gz")
md5sums=('8bb933efab97bbccae403f61f3eacbcf'
         '105d4d6d7db17812f3d75ebe9da098ee')

build() {
    cd "$_pkgbase-$pkgver"

    cmake . ${cmakeopts[@]} \
        -DCMAKE_LIBRARY_PATH=/usr/lib \
        -DCROSS_COMPILE32=0 \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DINSTALL_DEFAULT_BASEDIR=/usr/lib/etlegacy\
        -DINSTALL_DEFAULT_BINDIR=bin \
        -DINSTALL_DEFAULT_MODDIR=lib/etlegacy \
        -DBUILD_SERVER=1 \
        -DBUILD_CLIENT=1 \
        -DBUILD_MOD=0 \
        -DBUILD_MOD_PK3=0 \
        -DBUILD_PAK3_PK3=0 \
        -DFEATURE_AUTOUPDATE=0 \
        -DINSTALL_OMNIBOT=0

    make
}

package_etlegacy64() {
    pkgdesc="Wolfenstein: Enemy Territory 2.60b compatible client/server (etlegacy engine, 64 bit)"
    depends=('etlegacy64-mod' 'alsa-lib' 'curl' 'freetype2' 'gcc-libs' 'glew' 'libjpeg-turbo' 'libvorbis' 'lua' 'sdl2')
    provides=('etlegacy')
    conflicts=('etlegacy')
    backup=('etc/xdg/etlegacy/etmain/etl_server.cfg'
            'etc/xdg/etlegacy/etmain/legacy.cfg'
            'etc/xdg/etlegacy/etmain/campaigncycle.cfg'
            'etc/xdg/etlegacy/etmain/lmscycle.cfg'
            'etc/xdg/etlegacy/etmain/mapvotecycle.cfg'
            'etc/xdg/etlegacy/etmain/objectivecycle.cfg')
    install=etlegacy.install

    cd "$_pkgbase-$pkgver"

    # engine
    make DESTDIR="$pkgdir/" install

    # mod
    rm -rf $pkgdir/usr/lib/$_pkgbase/legacy

    # assets
    ln -s /usr/share/enemy-territory/etmain/{pak0,pak1,pak2}.pk3 $pkgdir/usr/lib/$_pkgbase/etmain/

    # doc
    mkdir -p $pkgdir/usr/share/doc/$_pkgbase
    #mv $pkgdir/usr/lib/$_pkgbase/INSTALL.txt $pkgdir/usr/share/doc/$_pkgbase/
    install -m 644 docs/game/anticheat.html $pkgdir/usr/share/doc/$_pkgbase/

    # license
    mkdir -p $pkgdir/usr/share/licenses/$_pkgbase
    mv $pkgdir/usr/lib/$_pkgbase/COPYING.txt $pkgdir/usr/share/licenses/$_pkgbase/

    # systemd
    mkdir -p $pkgdir/etc/xdg/$_pkgbase/etmain
    mkdir -p $pkgdir/usr/lib/systemd/system
    install -m 644 misc/etlegacy.conf $pkgdir/etc/xdg/$_pkgbase/
    install -m 644 misc/etlegacy.service $pkgdir/usr/lib/systemd/system/

    # config
    mv $pkgdir/usr/lib/$_pkgbase/etmain/*.cfg $pkgdir/etc/xdg/$_pkgbase/etmain/
    ln -s /etc/xdg/$_pkgbase/etmain/{etl_server,legacy,{campaign,lms,mapvote,objective}cycle}.cfg $pkgdir/usr/lib/$_pkgbase/etmain/

    # server
    mkdir -p $pkgdir/var/lib/$_pkgbase-server
    ln -s /etc/xdg/$_pkgbase/etmain $pkgdir/var/lib/$_pkgbase-server/
}

package_etlegacy64-mod() {
    pkgdesc="Wolfenstein: Enemy Territory 2.60b compatible client/server (etlegacy mod, 64 bit)"
    depends=('enemy-territory-data' 'geoip-database')
    optdepends=('etlegacy-omnibot: artificial intelligence (AI) controlled opponent'
                'geoip-database: GeoLite country geolocation database')
    provides=('etlegacy-mod')
    conflicts=('etlegacy-mod')

    # mod
    mkdir -p $pkgdir/usr/lib/$_pkgbase/legacy
    install -m 644 $srcdir/etlegacy-v$pkgver-x86_64/legacy/{etl_bin,pak3}_v$pkgver.pk3 $pkgdir/usr/lib/$_pkgbase/legacy

    # geoip
    ln -s /usr/share/GeoIP/GeoIP.dat $pkgdir/usr/lib/$_pkgbase/legacy
}

# vim:set ts=4 sw=2 et:
