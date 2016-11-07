##
# Repetier host full install, with the internal Slic3r working
# 
# Based on the repetier-host package from AUR
#
# Maintainer: Thibaut Lorrain <fredszaq[at]gmail[dot]com>

pkgname=repetier-host-bundle
pkgver=0.90d
pkgrel=1
pkgdesc="All-in-one 3D printer driving software "
url=('http://www.repetier.com/')
arch=('x86_64' 'i686')
license=('custom')
depends=('mono' 'perl' 'wxgtk')
makedepends=('cpanminus')
provides=('repetier-host')
conflicts=('repetier-host')
source=("http://www.repetier.com/?wpdmact=process&did=NDAuaG90bGluaw=="
        "repetier-host.png"
        "repetier-host.desktop")
PKGEXT=".pkg.tar"

build() {
	cd ${srcdir}/RepetierHost/
	MACHINE_TYPE=`uname -m`
	DIR=/usr/share/repetierHost

	echo "#!/bin/sh" > repetierHost
	echo "cd ${DIR}" >> repetierHost
	echo "mono RepetierHost.exe -home ${DIR}&" >> repetierHost
	chmod 755 repetierHost
	chmod a+rx ../RepetierHost
	chmod -R a+r *
	chmod -R a+x data
	chmod -R a+rwx Slic3r
	chmod a+x installDep*
	rm configureFirst.sh
	rm installDependenciesDebian
	rm installDependenciesFedora

	# install Slic3r deps
	cd Slic3r
	/usr/bin/vendor_perl/cpanm -n -L tmplib Boost::Geometry::Utils Encode::Locale Math::Clipper Math::ConvexHull::MonotoneChain Math::Geometry::Voronoi Math::PlanePath Moo Wx
	mv tmplib/lib/perl5/* lib/
	rm -rf tmplib

	# disable warings in Slic3r (needed to work with perl 5.18)
	sed -i slic3r.pl -e "s_/usr/bin/perl_/usr/bin/perl -X_g"

}

package() {
    mkdir -p $pkgdir/usr/share/
    mkdir -p $pkgdir/usr/bin
    ln -s /usr/share/repetierHost/repetierHost $pkgdir/usr/bin/repetierHost

    install -Dm644 ${srcdir}/RepetierHost/Repetier-Host-licence.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    rm ${srcdir}/RepetierHost/Repetier-Host-licence.txt
    cp -a ${srcdir}/RepetierHost $pkgdir/usr/share/repetierHost
    install -m 644 ${srcdir}/repetier-host.png $pkgdir/usr/share/repetierHost/repetier-host.png
    install -Dm 644 $srcdir/repetier-host.desktop $pkgdir/usr/share/applications/repetier-host.desktop
}

# vim:set ts=2 sw=2 et:
md5sums=('c54187d47cd7bbcda8f418d036b0f1c6'
         '851795e16549ca49267b8a1dff2a88ee'
         '1a1ff6bf0a158516a4cc96aaeb16e714')
