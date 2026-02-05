# GPD Pocket 4 Utilities

Utilities for GPD Pocket 4 on Arch Linux: automatic screen rotation and smooth fan control.

> **Note:** Screen rotation requires **X11** (uses xrandr).

## Features

- **Auto Screen Rotation** - Automatically rotates display based on device orientation using the built-in accelerometer
- **Smooth Fan Control** - Temperature-averaged fan curve to prevent fan noise spikes during brief CPU bursts

## Requirements

- GPD Pocket 4 (tested on G1628-04-L with AMD Ryzen 7 8840U)
- Arch Linux
- `iio-sensor-proxy` - for accelerometer access
- `gpd-fan` kernel module - for fan control (usually included in kernel or available as DKMS)

## Installation

### From AUR

```bash
yay -S gpd-pocket4-utils
```

### Manual

```bash
git clone https://aur.archlinux.org/gpd-pocket4-utils.git
cd gpd-pocket4-utils
makepkg -si
```

## Usage

### Enable Auto Rotation (X11)

```bash
systemctl enable --now gpd-pocket4-autorotate
```

### Enable Smooth Fan Control

```bash
systemctl enable --now gpd-pocket4-fancontrol
```

## Configuration

### Screen Rotation (X11)

The rotation script uses `xrandr` and assumes display output `eDP-1`. If your display has a different name, check with:

```bash
xrandr --query | grep " connected"
```

The rotation daemon monitors `iio-sensor-proxy` and maps orientations:

| Sensor Output | Screen Rotation |
|---------------|-----------------|
| normal        | normal          |
| left-up       | left            |
| right-up      | right           |
| bottom-up     | inverted        |

### Fan Control

The fan curve uses temperature averaging over 10 seconds to smooth out brief spikes:

| Temperature | Fan PWM | Approximate Speed |
|-------------|---------|-------------------|
| < 50°C      | 0       | Off               |
| 50-60°C     | 80      | Low               |
| 60-70°C     | 120     | Medium            |
| 70-80°C     | 180     | Medium-High       |
| 80-90°C     | 220     | High              |
| > 90°C      | 255     | Full              |

Fan ramps up faster than it ramps down to ensure cooling, while the slower ramp-down prevents noise oscillation.

To restore BIOS fan control:

```bash
systemctl stop gpd-pocket4-fancontrol
# Service automatically restores BIOS control on stop
```

## Boot Configuration (systemd-boot)

GPD Pocket 4 works well with systemd-boot. Example configuration:

### /boot/loader/loader.conf

```
default arch.conf
timeout 3
console-mode max
editor no
```

### /boot/loader/entries/arch.conf

```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /initramfs-linux.img
options root=PARTUUID=xxxx rw quiet splash
```

### Screen Orientation at Boot (Kernel Parameter)

For correct framebuffer orientation during boot, add to kernel options:

```
fbcon=rotate:1
```

Rotation values:
- `0` - normal
- `1` - rotate 90° clockwise
- `2` - rotate 180°
- `3` - rotate 90° counter-clockwise

## Xorg Configuration

For proper display orientation in Xorg, create `/etc/X11/xorg.conf.d/90-gpd-pocket4.conf`:

```
Section "Monitor"
    Identifier "eDP-1"
    Option "Rotate" "right"
EndSection

Section "InputClass"
    Identifier "GPD Touchscreen"
    MatchIsTouchscreen "on"
    Option "TransformationMatrix" "0 1 0 -1 0 1 0 0 1"
EndSection
```

The transformation matrix rotates touch input to match screen rotation:

| Rotation | Matrix                    |
|----------|---------------------------|
| normal   | `1 0 0 0 1 0 0 0 1`       |
| left     | `0 -1 1 1 0 0 0 0 1`      |
| right    | `0 1 0 -1 0 1 0 0 1`      |
| inverted | `-1 0 1 0 -1 1 0 0 1`     |

## Troubleshooting

### Accelerometer not detected

```bash
# Check if iio-sensor-proxy is running
systemctl status iio-sensor-proxy

# Test sensor
monitor-sensor
```

### Fan control not working

```bash
# Check if gpdfan hwmon exists
ls /sys/class/hwmon/*/name | xargs -I{} sh -c 'echo -n "{}: "; cat {}'

# Verify pwm control is available
cat /sys/class/hwmon/hwmon*/pwm1_enable
```

### Wrong display output name

Edit `/usr/bin/gpd-pocket4-autorotate` and change `eDP-1` to your display name.

## License

MIT License - see [LICENSE](LICENSE)

## Contributing

Issues and pull requests welcome.
