# Maintainer: Alice <alice@example.com>
pkgname=gpd-pocket4-utils
pkgver=1.0.0
pkgrel=1
pkgdesc="Utilities for GPD Pocket 4: auto screen rotation (X11) and smooth fan control"
arch=('any')
url="https://github.com/hitchhooker/gpd-pocket4-utils"
license=('MIT')
depends=('bash' 'iio-sensor-proxy' 'xorg-xrandr' 'lm_sensors')
optdepends=('gpd-fan-driver: kernel module for fan control')
backup=()
install=gpd-pocket4-utils.install
source=('autorotate'
        'smoothfan'
        'gpd-pocket4-autorotate.service'
        'gpd-pocket4-fancontrol.service')
sha256sums=('2b920d435ddd0757ade06eabc1f1f90cfa324ecfe1556d6a05fe1122fbefcf33'
            'e3f0d5c028d8c2fbe4f53dff24ca9dadbbe16861fcb22732ccbbe9888dce643d'
            '481f936529866fc95c037ee97b233e511e4ae6b8448664daa1a34da56d8fceb1'
            'c0ba0dca56f214e7162bc0bc7af15f402e31c55c975e7e5b4fca853d3620f182')

package() {
    # Install binaries
    install -Dm755 "$srcdir/autorotate" "$pkgdir/usr/bin/gpd-pocket4-autorotate"
    install -Dm755 "$srcdir/smoothfan" "$pkgdir/usr/bin/gpd-pocket4-fancontrol"

    # Install systemd services
    install -Dm644 "$srcdir/gpd-pocket4-autorotate.service" \
        "$pkgdir/usr/lib/systemd/system/gpd-pocket4-autorotate.service"
    install -Dm644 "$srcdir/gpd-pocket4-fancontrol.service" \
        "$pkgdir/usr/lib/systemd/system/gpd-pocket4-fancontrol.service"
}
