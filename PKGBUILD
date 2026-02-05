# Maintainer: Alice <alice@example.com>
pkgname=gpd-pocket4-utils
pkgver=1.2.0
pkgrel=1
pkgdesc="Utilities for GPD Pocket 4: auto rotation, touchscreen calibration, fan control, and X11 configs"
arch=('any')
url="https://github.com/hitchhooker/gpd-pocket4-utils"
license=('MIT')
depends=('bash' 'iio-sensor-proxy' 'xorg-xrandr' 'xorg-xinput' 'lm_sensors')
optdepends=('gpd-fan-driver: kernel module for fan control'
            'bspwm: for monitor hotplug desktop migration')
backup=('etc/modprobe.d/alsa-gpd-pocket4.conf'
        'etc/X11/xorg.conf.d/20-gpd-pocket4-amdgpu.conf'
        'etc/X11/xorg.conf.d/80-gpd-pocket4-trackpoint.conf')
install=gpd-pocket4-utils.install
source=('autorotate'
        'smoothfan'
        'monitor-hotplug'
        'gpd-pocket4-autorotate.service'
        'gpd-pocket4-fancontrol.service'
        '95-monitor-hotplug.rules'
        'alsa-gpd-pocket4.conf'
        '20-gpd-pocket4-amdgpu.conf'
        '80-gpd-pocket4-trackpoint.conf')
sha256sums=('2aa2553154eb802204037ed62169cc109d25813e879bcd3b5bd8f260ed7c234b'
            'e3f0d5c028d8c2fbe4f53dff24ca9dadbbe16861fcb22732ccbbe9888dce643d'
            '9a24222b3aef249d4e25a905848ef79e195c6ea3d2aece7b67f651dbd575b864'
            '481f936529866fc95c037ee97b233e511e4ae6b8448664daa1a34da56d8fceb1'
            'c0ba0dca56f214e7162bc0bc7af15f402e31c55c975e7e5b4fca853d3620f182'
            'adb452ab49f3042545d11aaf96b0f18c4e8ae29dce3121e8dada3a82815631f2'
            '60100b3af6b53c76a072d0f41be646cd812f089b5d87033957b55cc2615604c1'
            'dad1937c614e35d93886eecd9059e69b3762e977fcd9bc8db4e4a3efc69fad47'
            'c6135f3e429dfde6c5d2b9eb18fceb99a91c00a98fb0bb6d71c1b9cc36f1826f')

package() {
    # Install binaries
    install -Dm755 "$srcdir/autorotate" "$pkgdir/usr/bin/gpd-pocket4-autorotate"
    install -Dm755 "$srcdir/smoothfan" "$pkgdir/usr/bin/gpd-pocket4-fancontrol"
    install -Dm755 "$srcdir/monitor-hotplug" "$pkgdir/usr/bin/gpd-pocket4-monitor-hotplug"

    # Install systemd services
    install -Dm644 "$srcdir/gpd-pocket4-autorotate.service" \
        "$pkgdir/usr/lib/systemd/system/gpd-pocket4-autorotate.service"
    install -Dm644 "$srcdir/gpd-pocket4-fancontrol.service" \
        "$pkgdir/usr/lib/systemd/system/gpd-pocket4-fancontrol.service"

    # Install udev rule for monitor hotplug
    install -Dm644 "$srcdir/95-monitor-hotplug.rules" \
        "$pkgdir/usr/lib/udev/rules.d/95-gpd-pocket4-monitor-hotplug.rules"

    # Install ALSA config (fixes audio)
    install -Dm644 "$srcdir/alsa-gpd-pocket4.conf" \
        "$pkgdir/etc/modprobe.d/alsa-gpd-pocket4.conf"

    # Install Xorg configs
    install -Dm644 "$srcdir/20-gpd-pocket4-amdgpu.conf" \
        "$pkgdir/etc/X11/xorg.conf.d/20-gpd-pocket4-amdgpu.conf"
    install -Dm644 "$srcdir/80-gpd-pocket4-trackpoint.conf" \
        "$pkgdir/etc/X11/xorg.conf.d/80-gpd-pocket4-trackpoint.conf"
}
