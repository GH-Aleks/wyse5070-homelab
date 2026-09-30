# Dell Wyse 5070 Homelab

Ein gebrauchter **Dell Wyse 5070 Thin Client** (Intel Celeron J4105, 4 GB RAM, 16 GB eMMC) läuft bei mir dauerhaft als stiller, stromsparender Home-Server (~6–10 W) mit **Debian 13**.

## Was der Server macht

| Dienst | Zweck |
|---|---|
| **Pi-hole v6** | DNS **und** DHCP für den gesamten Haushalt – Werbe- und Tracker-Blocking auf allen Geräten |
| **WireGuard** | Verschlüsselter Tunnel zu einem VPS – volle Fernadministration ganz ohne Port-Freigaben im Router |
| **Docker** | Ausbau-Plattform für weitere Dienste |
| **systemd** | Autostart und Überwachung aller Dienste |

## Architektur

```
                  Internet
                      |
          [ VPS (WireGuard-Server) ]
                      |
                 WireGuard (UDP)
                      |
               [ DSL-Router ]
                      |
                     LAN
                      |
             [ Dell Wyse 5070 ]
              Pi-hole (DNS/DHCP)
              Docker, systemd
                      |
             alle Haushaltsgeräte
```

- **Keine Port-Freigaben** im Heim-Router – der einzige Eingang ist WireGuard (UDP), Endpunkt ist der VPS.
- Fernwartung läuft ausschließlich: `lokal/remote -> VPS -> WireGuard -> Wyse`.
- BIOS: „Power on after power loss" – nach Stromausfall bootet die Kiste allein wieder hoch, Dienste starten via `systemd` bzw. Docker `restart: always`.
- SSH nur mit Schlüssel-Authentifizierung.

## Tech-Stack

`Debian 13` · `Pi-hole v6` · `WireGuard` · `Docker` · `systemd` · `SSH` · `Bash`

## Hintergrund

Ziel war praktische Erfahrung mit Linux-Server-Betrieb im Kleinen: Netzwerkgrundlagen (DNS, DHCP, VPN), Dauerbetrieb und Automation – bevor es an größere Infrastruktur geht. Aus einem ~30-Euro-Thin-Client wurde so die zentrale Infrastruktur des Heimnetzes.

## Repo-Hinweis

Dieses Repository dokumentiert Aufbau und Architektur. Konfigurationen mit Zugangsdaten, Keys oder Adressen sind bewusst **nicht** enthalten.
