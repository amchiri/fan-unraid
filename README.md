# Corsair Fan Control — Unraid Plugin

Contrôle automatique des ventilateurs du **Corsair Commander Pro** basé sur la température CPU.

## Fonctionnalités

- Courbes de température → vitesse fan automatique
- 3 profils : Silencieux / Équilibré / Performance
- Mode Manuel (PWM fixe)
- Dashboard temps réel : RPM + températures
- Page WebUI intégrée dans le GUI Unraid (Settings → Utilities)
- Daemon en arrière-plan, intervalle configurable
- Config persistante sur la clé USB

## Prérequis

- Unraid 6.9+
- Corsair Commander Pro (USB ID: `1B1C:0C10`)
- Kernel module `corsaircpro` (inclus dans Unraid 6.9+)

## Installation

Dans l'interface Unraid, va dans **Plugins → Install Plugin** et colle l'URL :

```
https://raw.githubusercontent.com/TON_USERNAME/corsair-fan-unraid/main/plugin/corsair-fan.plg
```

## Structure du repo

```
corsair-fan-unraid/
├── plugin/
│   └── corsair-fan.plg          # Fichier d'installation Unraid
├── source/
│   ├── corsair-fan-daemon       # Script daemon (bash)
│   ├── CorsairFan.page          # Page WebUI (PHP)
│   └── config.default           # Config par défaut
└── README.md
```

## Courbes par défaut

| Temp CPU | Silencieux | Équilibré | Performance |
|----------|-----------|-----------|-------------|
| 30°C     | 20%       | 30%       | 40%         |
| 50°C     | 35%       | 50%       | 70%         |
| 70°C     | 60%       | 80%       | 94%         |
| 80°C     | 80%       | 94%       | 100%        |

## Logs

```bash
tail -f /tmp/corsair-fan/daemon.log
cat /tmp/corsair-fan/status.json
```
