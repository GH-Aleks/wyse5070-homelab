# Setup-Notizen (sanitisiert)

Grober Aufbau in der Reihenfolge der Umsetzung:

1. **Debian 13** headless auf der 16-GB-eMMC installiert (externer Installer-USB).
2. **Grundabsicherung**: SSH nur mit Public-Key-Auth, automatische Sicherheitsupdates.
3. **WireGuard-Client**: Persistentes Interface, `wg-quick@wg0` via systemd (Autostart), Keepalive zum Server.
4. **Pi-hole v6** als Docker-Container — wichtig: `--network host`, weil Docker-Bridge DHCP-Broadcasts (Port 67) nicht durchleitet; dazu `NET_ADMIN`/`NET_RAW` Capabilities.
5. **DHCP-Umzug**: Router-DHCP aus, Pi-hole-DHCP an (Range identisch zum Router, statischer Lease für die Kiste selbst).
6. **Autostart-Härtung**: `systemd`-Units mit `Restart=always`, BIOS „Power on after power loss".

## Lektionen (gekürzt)

- Docker + DHCP = `--network host` nötig, sonst schluckt die Bridge die Broadcasts still.
- FTL/DHCP braucht `NET_ADMIN`, sonst CRIT und DNS hängt mit.
- Statische Lease für den Server selbst nicht vergessen, sonst wandert die IP beim DHCP-Umzug und alle Dienste kippen.
