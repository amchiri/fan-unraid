# Corsair Fan Control — Unraid Plugin

Unraid plugin for automatic fan control via the **Corsair Commander Pro**, based on CPU temperature. Also monitors all fans detected on the system (motherboard, GPU, etc.).

## Features

- **3 automatic profiles** with configurable temperature curves: Silent / Balanced / Performance
- **Manual mode** with fixed global PWM
- **Per-fan override** — set any Commander Pro fan to a fixed PWM while the others follow the profile
- **Detects all hwmon fans** — Commander Pro, motherboard chips, GPU fans
- **Real-time dashboard** — RPM, PWM %, CPU and NVMe temperatures
- **Integrated WebUI** in Unraid Settings → Utilities
- **Background daemon** with configurable check interval
- **Persistent config** saved to USB key (`/boot/config/plugins/corsair-fan/config`)
- **Auto-start** on boot via plugin post-install

## Requirements

- Unraid 6.9+
- Corsair Commander Pro (USB ID: `1B1C:0C10`)
- Kernel module `corsaircpro` (included in Unraid 6.9+)

## Installation

In the Unraid UI, go to **Plugins → Install Plugin** and paste:

```
https://raw.githubusercontent.com/amchiri/fan-unraid/main/plugin/corsair-fan.plg
```

The plugin will automatically download the daemon and WebUI page from GitHub, install the default config, and start the daemon.

## Usage

Go to **Settings → Utilities → Corsair Fan Control**.

### Profiles

| Profile | Description |
|---------|-------------|
| Silent | Low RPM, prioritizes quiet operation |
| Balanced | Default — good mix of cooling and noise |
| Performance | Higher RPM, prioritizes cooling |
| Manual | Fixed PWM for all fans (unless overridden per-fan) |

### Per-fan control

Each Commander Pro fan (1–6) can independently follow the global profile or be set to a fixed PWM. Check **Manuel** on any fan card to unlock its individual slider.

### Temperature curves

Curves use the format `TEMP:PWM` separated by spaces. PWM ranges from 0 (off) to 255 (100%).

Example: `30:77 50:128 70:204 80:255`

Default curves:

| CPU Temp | Silent | Balanced | Performance |
|----------|--------|----------|-------------|
| 30°C | 20% | 30% | 40% |
| 40°C | 25% | 40% | 55% |
| 50°C | 35% | 50% | 70% |
| 60°C | 45% | 63% | 82% |
| 70°C | 60% | 80% | 94% |
| 80°C | 80% | 94% | 100% |

## File structure

```
corsair-fan-unraid/
├── plugin/
│   └── corsair-fan.plg          # Unraid plugin installer
├── source/
│   ├── corsair-fan-daemon       # Bash daemon
│   ├── CorsairFan.page          # WebUI (PHP)
│   └── config.default           # Default config
└── README.md
```

## Config file

Located at `/boot/config/plugins/corsair-fan/config` (persists across reboots).

```bash
PROFILE=balanced

CURVE_SILENT="30:51 40:64 50:89 60:115 70:153 80:204"
CURVE_BALANCED="30:77 40:102 50:128 60:160 70:204 80:240"
CURVE_PERFORMANCE="30:102 40:140 50:179 60:210 70:240 80:255"

MANUAL_PWM=102
CHECK_INTERVAL=30

# Per-fan overrides (0-255 for fixed PWM, "auto" to follow profile)
FAN1_PWM=auto
FAN2_PWM=auto
FAN3_PWM=auto
FAN4_PWM=auto
FAN5_PWM=auto
FAN6_PWM=auto
```

## Logs

```bash
tail -f /tmp/corsair-fan/daemon.log
cat /tmp/corsair-fan/status.json
```

## Compatibility

Tested on Unraid 6.12 with Corsair Commander Pro. The fan detection scans all `hwmon` devices — any fan visible to the Linux kernel will appear in the dashboard, even if not controllable through this plugin (e.g. GPU fans managed by their own driver).
