# FloppyOps — Proxmox Management Tool

<p align="center">
  <img src="https://floppyops.com/img/hero.png" alt="FloppyOps" width="600">
</p>

**All your Proxmox servers — one dashboard.**

Manage every PVE and PBS node from a single pane of glass. Built for sysadmins who run Proxmox in production.

---

**Alle Proxmox-Server — ein Dashboard.**

Verwalte jeden PVE- und PBS-Node von einer einzigen Oberflaeche. Entwickelt fuer Sysadmins, die Proxmox produktiv betreiben.

---

## Features

### Monitoring & Dashboard

Live overview of all your Proxmox nodes — CPU, RAM, disk, network — organized by location and cluster. Server-Sent Events keep everything in sync without page reloads. Customizable widget dashboard with drag & drop and fullscreen kiosk mode for monitoring walls.

Live-Uebersicht aller Proxmox-Nodes — CPU, RAM, Disk, Netzwerk — nach Standort und Cluster gruppiert. Server-Sent Events halten alles in Echtzeit synchron. Anpassbares Widget-Dashboard mit Drag & Drop und Vollbild-Kiosk-Modus fuer Monitoring-Waende.

### VM & Container Management

Full lifecycle management for QEMU VMs and LXC containers. Start, stop, reboot, clone, delete, and resize resources. Includes noVNC browser console and RDP download for Windows guests.

Komplette Lifecycle-Verwaltung fuer QEMU-VMs und LXC-Container. Starten, Stoppen, Neustarten, Klonen, Loeschen und Ressourcen anpassen. Inklusive noVNC-Browserkonsole und RDP-Download fuer Windows-Gaeste.

### Update Management

Check and install updates across your entire fleet. Schedule maintenance windows, manage APT repositories with fleet-wide templates and audit, and run bulk agent operations — all through the agent.

Updates pruefen und installieren fuer die gesamte Flotte. Wartungsfenster planen, APT-Repositories mit Fleet-weiten Templates und Audit verwalten und Bulk-Agent-Operationen ausfuehren — alles ueber den Agenten.

### Backup & PBS Integration

First-class Proxmox Backup Server support. Browse datastores, trigger backup jobs, restore VMs and containers, manage snapshots, and schedule PVE backup jobs (vzdump) — all from one interface.

Erstklassige PBS-Unterstuetzung. Datastores durchsuchen, Backup-Jobs ausloesen, VMs und Container wiederherstellen, Snapshots verwalten und PVE-Backup-Zeitplaene (vzdump) erstellen — alles in einer Oberflaeche.

### ZFS & Disk Health

Schedule scrubs and trims per pool, monitor SMART health data, track pool status, provision ZFS storage, and receive automated email alerts when disks need attention.

Scrubs und Trims pro Pool planen, SMART-Health-Daten ueberwachen, Pool-Status verfolgen, ZFS-Storage erstellen und automatische E-Mail-Alerts bei Disk-Problemen erhalten.

### ZFS Replication (Enterprise+)

Cross-node ZFS replication with automated incremental send/receive. Flexible scheduling, bandwidth limiting, snapshot retention policies, real-time progress tracking, and email alerts on failures.

Cross-Node ZFS-Replikation mit automatisiertem inkrementellen Send/Receive. Flexible Zeitplanung, Bandbreiten-Limitierung, Snapshot-Aufbewahrungsregeln, Echtzeit-Fortschrittsverfolgung und E-Mail-Alerts bei Fehlern.

### Agent Architecture

A lightweight Python agent runs on each node. The PVE API handles monitoring and VM control, while the agent handles system operations (updates, snapshots, ZFS, terminal, replication). SSH is only needed for initial setup.

Ein leichtgewichtiger Python-Agent laeuft auf jedem Node. Die PVE-API uebernimmt Monitoring und VM-Steuerung, der Agent handhabt System-Operationen (Updates, Snapshots, ZFS, Terminal, Replikation). SSH wird nur fuer die Ersteinrichtung benoetigt.

### Notifications & Alerts

SMTP and Microsoft 365 Graph API support. Define custom alert rules for CPU, RAM, disk, reboot-required, SMART errors, and replication failures. Fully customizable email templates.

SMTP- und Microsoft-365-Graph-API-Unterstuetzung. Alert-Regeln fuer CPU, RAM, Disk, Reboot-Required, SMART-Fehler und Replikations-Fehler definieren. Vollstaendig anpassbare E-Mail-Templates.

### Security & Networking

SSL/TLS with Let's Encrypt (HTTP-01 + Cloudflare DNS-01), manual certificate upload, built-in DDNS (IPv4 + IPv6), code integrity verification (RSA-signed), and maintenance mode during updates.

SSL/TLS mit Let's Encrypt (HTTP-01 + Cloudflare DNS-01), manueller Zertifikat-Upload, integrierter DDNS-Service (IPv4 + IPv6), Code-Integritaetspruefung (RSA-signiert) und Wartungsmodus bei Updates.

### Access Control & Audit

Three roles (Admin, Operator, Viewer) with granular permissions. System-wide activity log with filters, full-text search, and export to CSV, PDF, JSON, or Syslog.

Drei Rollen (Admin, Operator, Viewer) mit granularen Berechtigungen. Systemweites Aktivitaetslog mit Filtern, Volltextsuche und Export als CSV, PDF, JSON oder Syslog.

### Branding & Themes

Dark and light themes with standard and compact layouts. Enterprise users can white-label with custom logo, colors, and app name.

Dark- und Light-Theme mit Standard- und Kompakt-Layout. Enterprise-Nutzer koennen Logo, Farben und App-Name individuell anpassen.

---

## Links

| | |
|---|---|
| **Website** | [floppyops.com](https://floppyops.com) |
| **Download** | [release.floppyops.com](https://release.floppyops.com) |
| **Documentation** | [release.floppyops.com/#installation](https://release.floppyops.com/#installation) |
| **Bug Report** | [floppyops.com/bug-report](https://floppyops.com/bug-report) |
| **Feature Request** | [floppyops.com/feature-request](https://floppyops.com/feature-request) |

## Bug Reports & Feature Requests

Found a bug or have an idea? | Einen Bug gefunden oder eine Idee?

- [Report a Bug / Bug melden](https://floppyops.com/bug-report) — via the website (no GitHub account needed)
- [Request a Feature / Feature vorschlagen](https://floppyops.com/feature-request) — vote on existing ideas or suggest new ones
- [Open a GitHub Issue](https://github.com/floppy007/floppyops/issues/new/choose) — if you prefer GitHub

All website submissions are automatically synced to GitHub Issues.

## Tech Stack

- PHP 8+, MySQL/MariaDB, Bootstrap 5, Apache2
- Python agent (stdlib-only, TLS, token auth)
- PVE API, PBS API (port 8007), SSH (bootstrap only)

## License

FloppyOps is proprietary software. All rights reserved.

(c) 2026 Florian Hesse / [Comnic-IT.de](https://comnic-it.de)
