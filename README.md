# FloppyOps — Proxmox Management Tool

<p align="center">
  <img src="https://floppyops.com/img/hero.png" alt="FloppyOps" width="600">
</p>

**All your Proxmox servers — one dashboard.**

Manage every PVE and PBS node from a single pane of glass. Built for sysadmins who run Proxmox in production.

---

## What it does

### Monitoring & Dashboard

Live overview of all your Proxmox nodes — CPU, RAM, disk, network — organized by location and cluster. Server-Sent Events keep everything in sync without page reloads.

### VM & Container Management

Full lifecycle management for QEMU VMs and LXC containers. Start, stop, reboot, clone, delete, and resize resources. Includes noVNC browser console and RDP download for Windows guests.

### Update Management

Check and install updates across your entire fleet. Schedule maintenance windows, manage APT repositories, and run bulk operations — all through the agent.

### Backup & PBS Integration

First-class Proxmox Backup Server support. Browse datastores, trigger backup jobs, restore VMs and containers, and manage snapshots — all from one interface.

### ZFS & Disk Health

Schedule scrubs and trims per pool, monitor SMART health data, track pool status, and receive automated email alerts when disks need attention.

### Agent Architecture

A lightweight Python agent runs on each node. The PVE API handles monitoring and VM control, while the agent handles system operations (updates, snapshots, ZFS, terminal). SSH is only needed for initial setup.

### Notifications & Alerts

SMTP and Microsoft 365 Graph API support. Define custom alert rules for CPU, RAM, disk, reboot-required, and SMART errors. Fully customizable email templates.

### Access Control & Audit

Three roles (Admin, Operator, Viewer) with granular permissions. System-wide activity log with filters, full-text search, and export to CSV, PDF, JSON, or Syslog.

### Branding & Themes

Dark and light themes with standard and compact layouts. Enterprise users can white-label with custom logo, colors, and app name.

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

Found a bug or have an idea? You can:

- [Report a Bug](https://floppyops.com/bug-report) — via the website (no GitHub account needed)
- [Request a Feature](https://floppyops.com/feature-request) — vote on existing ideas or suggest new ones
- [Open a GitHub Issue](https://github.com/floppy007/floppyops/issues/new/choose) — if you prefer GitHub

All website submissions are automatically synced to GitHub Issues.

## Tech Stack

- PHP 8+, MySQL/MariaDB, Bootstrap 5 (dark theme), Apache2
- Python agent (stdlib-only, TLS, token auth)
- PVE API, PBS API (port 8007), SSH (bootstrap only)

## License

FloppyOps is proprietary software. All rights reserved.

(c) 2026 Florian Hesse / [Comnic-IT.de](https://comnic-it.de)
