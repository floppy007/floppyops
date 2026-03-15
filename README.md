# FloppyOps — Proxmox Management Tool

<p align="center">
  <img src="https://floppyops.com/img/hero.png" alt="FloppyOps" width="600">
</p>

**All your Proxmox servers — one dashboard.**

Manage every PVE and PBS node from a single pane of glass. Built for sysadmins who run Proxmox in production.

**Free to use** — 30-day trial after installation, then just register for a free Community license. Email verification with 6-digit code. No credit card, no hidden costs.

> **[Deutsche Version weiter unten](#deutsch)**

---

## Features

### Monitoring & Dashboard
Live overview of all your Proxmox nodes — CPU, RAM, disk, network — organized by location and cluster. Server-Sent Events keep everything in sync without page reloads. Customizable widget dashboard with drag & drop and fullscreen kiosk mode for monitoring walls.

### VM & Container Management
Full lifecycle management for QEMU VMs and LXC containers. Start, stop, reboot, clone, delete, and resize resources. Includes noVNC browser console and RDP download for Windows guests.

### Update Management
Check and install updates across your entire fleet. Schedule maintenance windows, manage APT repositories with fleet-wide templates and audit, and run bulk agent operations — all through the agent.

### Backup & PBS Integration
First-class Proxmox Backup Server support. Browse datastores, trigger backup jobs, restore VMs and containers, manage snapshots, and schedule PVE backup jobs (vzdump) — all from one interface.

### ZFS & Disk Health
Schedule scrubs and trims per pool, monitor SMART health data, track pool status, provision ZFS storage, and receive automated email alerts when disks need attention.

### ZFS Replication (Enterprise+)
Cross-node ZFS replication with automated incremental send/receive. Flexible scheduling, bandwidth limiting, snapshot retention policies, real-time progress tracking, and email alerts on failures.

### UPS Monitoring (Pro+)
Monitor UPS devices via Network UPS Tools (NUT). Battery charge, runtime, load, input/output voltage, temperature — with instant email alerts on power events (on battery, low battery, offline). Dashboard fleet tile and automated shutdown rules.

### Network Management
Full PVE network interface management. View and manage bridges, bonds, VLANs and physical NICs across all cluster nodes. Pending changes workflow, live traffic monitoring (RX/TX per interface), and network topology overview.

### Live Health Alerts & Storage Monitoring
Proactive server monitoring with live PVE API polling. CPU, RAM, disk and storage thresholds are checked automatically with configurable intervals (2–30 minutes). All active storages (ZFS, CIFS, LVM) are monitored against warning/critical thresholds. Server offline detection with automatic status updates and professional HTML email alerts.

### Web Terminal
Execute commands directly on your Proxmox servers from the FloppyOps web interface — no separate SSH client needed. Secure execution via PVE Agent with TLS and token-based authentication.

### Agent Architecture
A lightweight Python agent runs on each node. The PVE API handles monitoring and VM control, while the agent handles system operations (updates, snapshots, ZFS, terminal, replication). SSH is only needed for initial setup.

### Notifications & Alerts
SMTP and Microsoft 365 Graph API support. Define custom alert rules for CPU, RAM, disk, reboot-required, SMART errors, and replication failures. Fully customizable email templates.

### Code Integrity & Security
Automatic detection of code manipulation on every auth request. RSA-signed manifests verify all files. License-bound phone-home reporting flags tampering attempts. Runtime integrity guard protects the running system.

### Networking & SSL
SSL/TLS with Let's Encrypt (HTTP-01 + Cloudflare DNS-01), manual certificate upload, built-in DDNS (IPv4 + IPv6), and maintenance mode during updates.

### Access Control & Audit
Three roles (Admin, Operator, Viewer) with granular permissions. System-wide activity log with filters, full-text search, and export to CSV, PDF, JSON, or Syslog.

### Branding & Themes
Dark and light themes with standard and compact layouts. Enterprise users can white-label with custom logo, colors, and app name.

---

## Releases

| Version | Codename | Highlights |
|---------|----------|------------|
| **v0.9.9** | Health & Monitoring | Live health alerts (PVE API polling), storage threshold alerts, server offline detection, web terminal, ZFS maintenance |
| **v0.9.8** | Infrastructure | UPS monitoring (NUT), network management (bridges/bonds/VLANs), UPS mail alerts & shutdown rules, ZFS replication |
| **v0.9.7** | Security & Activation | Runtime integrity guard, license phone-home, auto-downgrade on tampering, community activation with email verification |
| **v0.9.5** | Settings & Updates | Dedicated updates tab, auto update check, security warnings, HTTPS toggle, cron auto-setup |
| **v0.9.4** | Self-Update | Self-update system (8-phase engine), rollback, maintenance mode, PBS restore+, ZFS reports |
| **v0.9.3** | Alert Split | Community/Pro+ alert rules split, ZFS cron Pro+ gate |
| **v0.9.2** | Integrity | Code integrity (RSA-signed manifests, tamper detection), agent self-protection |
| **v0.9.1** | Licensing | Effective tier, support expiry, piggyback refresh |
| **v0.9.0** | Core | Dashboard, server management, VM/CT control, noVNC, updates, snapshots, ZFS, PBS, terminal, mail, SSL, DDNS, branding, licensing, agent v2.0 |

## Roadmap

| Version | Feature | Tier | Status |
|---------|---------|------|--------|
| **v1.0.0** | VM/CT Provisioning — step-by-step wizard for creating VMs and containers | All | Next |
| **v1.1** | Firewall Management — PVE firewall rules and security groups | Pro+ | Planned |
| **v1.2** | NAS Storage — Synology and QNAP integration | Pro+ | Planned |
| **v1.3** | Notification Channels — Slack, Teams, Telegram, Discord | Pro+ | Planned |
| **v1.4** | 2FA/TOTP + REST API & Webhooks | All / Enterprise+ | Planned |
| **v1.5+** | LDAP/AD, HA Management, Scheduled Reports, Ceph, Ansible GUI | Enterprise+ | Planned |

Full roadmap: [floppyops.com/roadmap](https://floppyops.com/roadmap)

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

Found a bug or have an idea?

- [Report a Bug](https://floppyops.com/bug-report) — via the website (no GitHub account needed)
- [Request a Feature](https://floppyops.com/feature-request) — vote on existing ideas or suggest new ones
- [Open a GitHub Issue](https://github.com/floppy007/floppyops/issues/new/choose) — if you prefer GitHub

All website submissions are automatically synced to GitHub Issues.

## Tech Stack

- PHP 8+, MySQL/MariaDB, Bootstrap 5, Apache2
- Python agent (stdlib-only, TLS, token auth)
- PVE API, PBS API (port 8007), SSH (bootstrap only)

---

<a id="deutsch"></a>

## Deutsch

**Alle Proxmox-Server — ein Dashboard.**

Verwalte jeden PVE- und PBS-Node von einer einzigen Oberflaeche. Entwickelt fuer Sysadmins, die Proxmox produktiv betreiben.

**Kostenlos nutzbar** — 30-Tage-Testphase nach Installation, danach einfach kostenlose Community-Lizenz registrieren. E-Mail-Verifizierung per 6-stelligem Code. Keine Kreditkarte, keine versteckten Kosten.

### Monitoring & Dashboard
Live-Uebersicht aller Proxmox-Nodes — CPU, RAM, Disk, Netzwerk — nach Standort und Cluster gruppiert. Server-Sent Events halten alles in Echtzeit synchron. Anpassbares Widget-Dashboard mit Drag & Drop und Vollbild-Kiosk-Modus fuer Monitoring-Waende.

### VM- & Container-Verwaltung
Komplette Lifecycle-Verwaltung fuer QEMU-VMs und LXC-Container. Starten, Stoppen, Neustarten, Klonen, Loeschen und Ressourcen anpassen. Inklusive noVNC-Browserkonsole und RDP-Download fuer Windows-Gaeste.

### Update-Verwaltung
Updates pruefen und installieren fuer die gesamte Flotte. Wartungsfenster planen, APT-Repositories mit Fleet-weiten Templates und Audit verwalten und Bulk-Agent-Operationen ausfuehren — alles ueber den Agenten.

### Backup & PBS-Integration
Erstklassige Proxmox Backup Server Unterstuetzung. Datastores durchsuchen, Backup-Jobs ausloesen, VMs und Container wiederherstellen, Snapshots verwalten und PVE-Backup-Zeitplaene (vzdump) erstellen — alles in einer Oberflaeche.

### ZFS & Disk Health
Scrubs und Trims pro Pool planen, SMART-Health-Daten ueberwachen, Pool-Status verfolgen, ZFS-Storage erstellen und automatische E-Mail-Alerts bei Disk-Problemen erhalten.

### ZFS-Replikation (Enterprise+)
Cross-Node ZFS-Replikation mit automatisiertem inkrementellen Send/Receive. Flexible Zeitplanung, Bandbreiten-Limitierung, Snapshot-Aufbewahrungsregeln, Echtzeit-Fortschrittsverfolgung und E-Mail-Alerts bei Fehlern.

### USV-Monitoring (Pro+)
USV-Geraete ueber Network UPS Tools (NUT) ueberwachen. Ladezustand, Laufzeit, Last, Ein-/Ausgangsspannung, Temperatur — mit sofortigen E-Mail-Alerts bei Stromereignissen (Batteriebetrieb, niedrig, offline). Dashboard Fleet-Tile und automatische Shutdown-Regeln.

### Netzwerk-Verwaltung
Vollstaendige PVE-Netzwerk-Interface-Verwaltung. Bridges, Bonds, VLANs und physische NICs ueber alle Cluster-Nodes hinweg verwalten. Pending-Changes Workflow, Live-Traffic-Monitoring (RX/TX pro Interface) und Netzwerk-Topologie-Uebersicht.

### Live Health-Alerts & Storage-Monitoring
Proaktives Server-Monitoring mit Live PVE API Polling. CPU, RAM, Disk und Storage-Schwellwerte werden automatisch geprueft mit konfigurierbarem Intervall (2–30 Minuten). Alle aktiven Storages (ZFS, CIFS, LVM) werden gegen Warn-/Kritisch-Schwellwerte ueberwacht. Server-Offline-Erkennung mit automatischer Status-Aktualisierung und professionellen HTML-Mail-Alerts.

### Web-Terminal
Befehle direkt auf Proxmox-Servern ausfuehren ueber die FloppyOps Web-Oberflaeche — kein separater SSH-Client noetig. Sichere Ausfuehrung via PVE Agent mit TLS und Token-basierter Authentifizierung.

### Agent-Architektur
Ein leichtgewichtiger Python-Agent laeuft auf jedem Node. Die PVE-API uebernimmt Monitoring und VM-Steuerung, der Agent handhabt System-Operationen (Updates, Snapshots, ZFS, Terminal, Replikation). SSH wird nur fuer die Ersteinrichtung benoetigt.

### Benachrichtigungen & Alerts
SMTP- und Microsoft-365-Graph-API-Unterstuetzung. Alert-Regeln fuer CPU, RAM, Disk, Reboot-Required, SMART-Fehler und Replikations-Fehler definieren. Vollstaendig anpassbare E-Mail-Templates.

### Code-Integritaet & Sicherheit
Automatische Erkennung von Code-Manipulation bei jedem Auth-Request. RSA-signierte Manifeste pruefen alle Dateien. Lizenzgebundenes Phone-Home Reporting meldet Manipulationsversuche. Runtime Integrity Guard schuetzt das laufende System.

### Netzwerk & SSL
SSL/TLS mit Let's Encrypt (HTTP-01 + Cloudflare DNS-01), manueller Zertifikat-Upload, integrierter DDNS-Service (IPv4 + IPv6) und Wartungsmodus bei Updates.

### Zugriffskontrolle & Audit
Drei Rollen (Admin, Operator, Viewer) mit granularen Berechtigungen. Systemweites Aktivitaetslog mit Filtern, Volltextsuche und Export als CSV, PDF, JSON oder Syslog.

### Branding & Themes
Dark- und Light-Theme mit Standard- und Kompakt-Layout. Enterprise-Nutzer koennen Logo, Farben und App-Name individuell anpassen.

---

### Releases

| Version | Codename | Highlights |
|---------|----------|------------|
| **v0.9.9** | Health & Monitoring | Live Health-Alerts (PVE API Polling), Storage-Schwellwert-Alerts, Server-Offline-Erkennung, Web-Terminal, ZFS-Wartung |
| **v0.9.8** | Infrastruktur | USV-Monitoring (NUT), Netzwerk-Management (Bridges/Bonds/VLANs), USV Mail-Alerts & Shutdown-Regeln, ZFS-Replikation |
| **v0.9.7** | Sicherheit & Aktivierung | Runtime Integrity Guard, Lizenz Phone-Home, Auto-Downgrade bei Manipulation, Community-Aktivierung mit E-Mail-Verifizierung |
| **v0.9.5** | Einstellungen & Updates | Eigener Updates-Tab, automatischer Update-Check, Sicherheitswarnungen, HTTPS-Toggle, Cron Auto-Setup |
| **v0.9.4** | Self-Update | Self-Update System (8-Phasen-Engine), Rollback, Wartungsmodus, PBS Restore+, ZFS-Reports |
| **v0.9.3** | Alert Split | Community/Pro+ Alert-Regeln Split, ZFS-Cron Pro+ Gate |
| **v0.9.2** | Integritaet | Code-Integritaet (RSA-signierte Manifeste, Tamper-Detection), Agent-Selbstschutz |
| **v0.9.1** | Lizenzierung | Effective Tier, Support-Ablauf, Piggyback-Refresh |
| **v0.9.0** | Core | Dashboard, Server-Verwaltung, VM/CT-Steuerung, noVNC, Updates, Snapshots, ZFS, PBS, Terminal, Mail, SSL, DDNS, Branding, Lizenzmodell, Agent v2.0 |

### Roadmap

| Version | Feature | Tier | Status |
|---------|---------|------|--------|
| **v1.0.0** | VM/CT Provisioning — Schritt-fuer-Schritt Wizard zum Erstellen von VMs und Containern | Alle | Naechstes |
| **v1.1** | Firewall Management — PVE-Firewall-Regeln und Security Groups | Pro+ | Geplant |
| **v1.2** | NAS Storage — Synology- und QNAP-Integration | Pro+ | Geplant |
| **v1.3** | Benachrichtigungs-Kanaele — Slack, Teams, Telegram, Discord | Pro+ | Geplant |
| **v1.4** | 2FA/TOTP + REST API & Webhooks | Alle / Enterprise+ | Geplant |
| **v1.5+** | LDAP/AD, HA Management, Scheduled Reports, Ceph, Ansible GUI | Enterprise+ | Geplant |

Vollstaendige Roadmap: [floppyops.com/roadmap](https://floppyops.com/roadmap)

---

### Fehler melden & Features vorschlagen

- [Bug melden](https://floppyops.com/bug-report) — ueber die Website (kein GitHub-Account noetig)
- [Feature vorschlagen](https://floppyops.com/feature-request) — fuer bestehende Ideen abstimmen oder neue einreichen
- [GitHub Issue erstellen](https://github.com/floppy007/floppyops/issues/new/choose) — wer GitHub bevorzugt

Alle Website-Einreichungen werden automatisch mit GitHub Issues synchronisiert.

---

## License

FloppyOps is proprietary software. All rights reserved.

(c) 2026 Florian Hesse / [Comnic-IT.de](https://comnic-it.de)
