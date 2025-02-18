# Maintainer: jrdn <r7Iq7R1c@protonmail.com>

pkgname=openlinkhub-bin
_upstreamname=OpenLinkHub
_binlocation=/usr/bin/"${pkgname%-*}"
_applocation=/opt/"${pkgname%-*}"
pkgver=0.5.1
pkgrel=4
pkgdesc="Open source Linux interface for iCUE LINK Hub and other Corsair AIOs, Hubs. [Current Binary Release - amd64/x86_64]"
arch=('x86_64')
url="https://github.com/jurkovic-nikola/OpenLinkHub"
license=('GPL3')
groups=()
depends=('systemd' 'i2c-tools')
makedepends=('systemd' 'tar') 
provides=("${pkgname%-*}")
conflicts=("${pkgname%-*}")
replaces=()
backup=(
	"etc/udev/rules.d/99-openlinkhub.rules"
	)
options=()
install="${pkgname%-*}".install
source=(
	"https://github.com/jurkovic-nikola/${_upstreamname}/releases/download/${pkgver}/${_upstreamname}_${pkgver}_amd64.tar.gz"
	"${pkgname%-*}".install
	"${pkgname%-*}".sysusers
	"${pkgname%-*}".service
)
noextract=("${_upstreamname}_${pkgver}_amd64.tar.gz")
sha256sums=('3fce1e70e90720769ee031a5b1cedfb2f3240cf0972b2d65767f65c57824b469'
            'eb4d6d32e69feeb6892ea2f5c0beb12a5abec06383d79fbe308c19c7c9287c85'
            '5aab700df0d7791722c2723ece369df916e07184407e4778d25a2dd934f12681'
            '430d8196074127257b6b823d7ae72eaa9fedf90f55c70bc121a9467e7648dcc5')

prepare() {
	mkdir -p "${pkgname%-*}"
	tar -xzf "${_upstreamname}_${pkgver}_amd64.tar.gz" -C "${pkgname%-*}" --strip-components=1
}

package() {
	## Install users
	install -Dm 644 "${pkgname%-*}.sysusers" "$pkgdir/usr/lib/sysusers.d/${pkgname%-*}.conf"

	## Install folders
	install -d -m 755 "${pkgdir}$_applocation/"{database,static,web}

	## Install systemd service unit
	install -Dm 644 "${pkgname%-*}.service" "$pkgdir/usr/lib/systemd/system/${pkgname%-*}.service"

	## Install udev rules
	install -bDm 644 "${pkgname%-*}/99-${pkgname%-*}.rules" "$pkgdir/etc/udev/rules.d/99-${pkgname%-*}.rules"

	## Install package executable
	install -Dm 755 "${pkgname%-*}/$_upstreamname" "$pkgdir$_binlocation"

	## Install package data
	cp -r "${pkgname%-*}"/database/* "${pkgdir}"$_applocation/database/
	cp -r "${pkgname%-*}"/static/* "${pkgdir}"$_applocation/static/
	cp -r "${pkgname%-*}"/web/* "${pkgdir}"$_applocation/web/

	## Update permissions
	chmod 755 "${pkgdir}"$_binlocation
	chmod -R 755 "${pkgdir}"$_applocation
	chown -R 473:473 "${pkgdir}"$_applocation
}
