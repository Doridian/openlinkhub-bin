# Maintainer: jrdn <r7Iq7R1c@protonmail.com>

pkgname=openlinkhub-bin
_pkgnamesrc=OpenLinkHub
_location=/opt/$_pkgnamesrc
_tag=0.3.8
pkgver=0.3.8
pkgrel=1
pkgdesc="Open source Linux interface for iCUE LINK Hub and other Corsair AIOs, Hubs. Binary for amd64/x86_64"
arch=('x86_64')
url="https://github.com/jurkovic-nikola/OpenLinkHub"
license=('GPL3')
groups=()
depends=('systemd')
makedepends=() 
provides=("openlinkhub")
conflicts=("openlinkhub" )
replaces=("openlinkhub")
backup=()
options=()
install=$_pkgnamesrc.install
source=(
	https://github.com/jurkovic-nikola/${_pkgnamesrc}/releases/download/${pkgver}/${_pkgnamesrc}_${pkgver}_amd64.tar.gz
	$_pkgnamesrc.install
	$_pkgnamesrc.sysusers
	$_pkgnamesrc.tmpfiles
	$_pkgnamesrc.service
)
noextract=()
sha256sums=('73e9549dd192c10699ed07ca2024546a0395407fb62e18386697b10c010b2db5'
            'a541e655f649d43250481394549ef8dc0192fdd1c6b155866f57adce9f172d7c'
            '1222f0ea7dc963f3e09db814bacc71d2179232ad582fab8ad0e36cb8a9e69141'
            '18f1759e8642b2be24a11337eca16f148f80bee88b3270a3bb62c7376b428090'
            'cc4c09d90f3c26db239ed159b4b65ddf3ec7b68c044e61ee42c77a1a5fb659de')

prepare() {
	cd $_pkgnamesrc

	## Look for CORSAIR Controller Device and create UDEV rule file
	## Copied nearly verbatim from upstream ./install.sh

	lsusb -d 1b1c: | while read -r line; do
		ids=$(echo "$line" | awk '{print $6}')
		vendor_id=$(echo "$ids" | cut -d':' -f1)
		device_id=$(echo "$ids" | cut -d':' -f2)
		cat > $_pkgnamesrc-udev.rules <<- EOM
		KERNEL=="hidraw*", SUBSYSTEMS=="usb", ATTRS{idVendor}=="$vendor_id", ATTRS{idProduct}=="$device_id", MODE="0666"
		EOM
	done
}

package() {

	## Install package README and LICENSE files
	install -Dm 644 $_pkgnamesrc.sysusers "${pkgdir}"/usr/lib/sysusers.d/$_pkgnamesrc.conf

	## Install parent folders
	install -Dm 644 $_pkgnamesrc.tmpfiles "${pkgdir}"/usr/lib/tmpfiles.d/$_pkgnamesrc.conf

	## Install systemd service unit
	install -Dm 644 "${_pkgnamesrc}.service" "${pkgdir}"/usr/lib/systemd/system/$_pkgnamesrc.service

	## Install systemd service unit
	install -Dm 644 "${_pkgnamesrc}/${_pkgnamesrc}-udev.rules" "${pkgdir}"/etc/udev/rules.d/99-$_pkgnamesrc.rules

	## Install package executable
	install -Dm 755 "${_pkgnamesrc}/${_pkgnamesrc}" "${pkgdir}"$_location/$_pkgnamesrc

	## Install program data
	cp -r "${_pkgnamesrc}"/database/ "${pkgdir}"$_location/database/
	cp -r "${_pkgnamesrc}"/static/ "${pkgdir}"$_location/static/
	cp -r "${_pkgnamesrc}"/web/ "${pkgdir}"$_location/web/

	chmod -R 755 "${pkgdir}"$_location
	chown -R openlinkhub:root "${pkgdir}"$_location

}