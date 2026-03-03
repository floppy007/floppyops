# Changelog

Alle Änderungen am Projekt.
Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.1.0/).

## [Unreleased]

## [0.9.5-beta2] - 2026-03-03

### Added
- Sicherheits- & Einrichtungswarnungen (Nextcloud-Style): 12 System-Checks in Settings > Allgemein
  - Default-Passwort aktiv, fehlende PHP-Extensions, nicht beschreibbare Verzeichnisse
  - PHP-Version < 8.1, Cron nicht aktiv, Disk-Speicher knapp, display_errors aktiv
  - HTTPS nicht aktiv, gepatchte Dateien, Mail nicht konfiguriert, Update verfügbar
- HTTPS-Toggle: Selbstsigniertes Zertifikat aktivieren/deaktivieren per Button
  - In Sicherheitswarnungen (Settings > Allgemein) und Netzwerk-Tab (Settings > Netzwerk)
- Cron Auto-Setup: Automatische Einrichtung bei Installation (database.php) + manueller Button in Warnung
- Farbkodierte Warnungen: Danger (rot), Warning (gelb), Info (blau), OK (grün) mit Icons und Action-Buttons

### Changed
- Sicherheit & Integrität von Updates-Tab nach Allgemein-Tab verschoben (col-lg-6 neben Einstellungen)
- Sicherheitscheck startet automatisch beim Laden der Settings-Seite (mit Spinner)
- Updates-Tab: Backups-Card neben Version-Card verschoben (Lücke oben rechts behoben)
- Mail-Warnung prüft jetzt SMTP-Host statt is_active Flag (kein False-Positive bei konfigurierter Mail)
- SSLService::getCurrentCertInfo() zeigt Snakeoil-Zertifikate mit is_self_signed Flag

### Fixed
- Dunkle Schriftfarben in Dark-Mode: Custom CSS-Klassen statt Bootstrap-Utilities (data-theme vs data-bs-theme)
- Umlaute in IntegrityService-Messages korrigiert (Integritätsprüfung → Integritätsprüfung etc.)
- Netzwerk-Tab: HTTPS-Status immer sichtbar (aktiv/inaktiv) statt nur bei Deaktivierung

## [0.9.5-beta1] - 2026-02-24

### Added
- Neuer Settings-Tab "Updates" — alle Update-bezogenen Cards aus Settings > Allgemein ausgelagert
- Updates-Tab Badge: Zeigt Anzahl verfügbarer Updates (App-Update + Pending Patches) als Warning-Badge
- Sidebar-Link zeigt direkt auf Updates-Tab wenn Update verfügbar
- Automatischer Update-Check per Cron: Prüft 1x täglich auf App- und Agent-Updates (24h Cache)
- Side-by-side Layout: Version|Integrity, App Auto-Update|Agent Auto-Update, Hotfixes|Backups

### Changed
- Settings > Allgemein auf Disk-Schwellwerte + Server-Uhr reduziert (~77 Zeilen)
- JS aufgeteilt: `settings-updates.js` (~600 Zeilen) + `settings-general.js` (~55 Zeilen)
- Beide JS-Dateien in DOMContentLoaded gewrapped (behebt Timing mit `apiFetch` aus `app.js`)
- Patch-Buttons mit `flex-shrink-0` für konsistente Größe

### Fixed
- `apiFetch()` gibt bereits geparstes JSON zurück — alle `.then(r => r.json())` Ketten entfernt
- Backups-Spinner hing endlos: `loadBackups()` rief `apiFetch()` auf bevor `app.js` geladen war

## [0.9.4-beta5] - 2026-03-02

### Added
- Update-Channel Auswahl: Dropdown in Settings > Allgemein (Auto/Stable/Beta) mit sofortigem Cache-Reset
- Update-Log Terminal: Dunkles Monospace-Fenster im Progress-Modal zeigt alle Phasen live

### Changed
- Update-Channel Erkennung: Automatisch aus APP_VERSION (beta/rc → beta-Channel, sonst stable)
- Beta-Channel prüft zusätzlich Stable-Channel (neueres Stable-Release wird angeboten)
- Backup-Verzeichnis dynamisch: Relativ zum App-Pfad statt hardcoded `/opt/floppyops/backups/`
- Progress-Modal: Größer (modal-lg), Phase + Nachricht in einer Zeile
- App Auto-Update Card: Pro+ Badge mit Glow-Animation statt grauem Overlay (Community sieht Card)
- Changelog-Anzeige: Formatiert mit Überschriften und Aufzählungspunkten statt Plaintext

### Fixed
- Update-Check fand keine Beta-Updates (Channel war auf `stable` hardcoded)
- JS `__()` Funktion: Placeholder-Ersetzung fehlte (`:time` wurde nicht durch Wert ersetzt)
- Settings-Seite crash auf Installationen ohne PatchService (fehlende Abhängigkeit)

## [0.9.4-beta4] - 2026-03-02

### Added
- Self-Update System: One-Click Update via Web-UI (Settings > Allgemein) mit SSE-Fortschrittsanzeige
- Self-Update CLI: `tools/update.php` mit Flags --version, --check, --rollback, --list-backups, --dry-run, --auto, --no-backup
- 8-Phasen Update-Engine: Pre-flight, Download, Verify (SHA256+RSA), Backup (Dateien+DB), Extract, Replace, Post-Update, Final Verify
- Automatische Backups vor jedem Update: Dateien + mysqldump in `/opt/floppyops/backups/`, max 5 Backups
- Rollback-System: Wiederherstellung aus beliebigem Backup (Dateien + Datenbank) per UI oder CLI
- Maintenance Mode: 503-Seite während Datei-Ersetzung, Auto-Cleanup nach 10 Minuten
- App Auto-Update: Cron-basiert mit einstellbarem Zeitplan und Benachrichtigung (Pro+ Feature)
- Data-Migrations Framework: Versionierte PHP-Dateien in `data-migrations/` für Post-Update Hooks
- Backup-Card in Settings: Tabelle mit verfügbaren Backups (Version, Datum, Größe, DB-Status)
- Protected Paths: config/database.php, certs/, public/branding/, logs/, storage/, .env werden nie überschrieben
- Concurrency-Schutz: Lock-File mit PID-Prüfung verhindert parallele Updates
- i18n: ~80 neue Übersetzungs-Keys für Update-Phasen, Buttons, Fehlermeldungen (EN+DE)

## [0.9.4-beta3] - 2026-03-02

### Added
- PBS Restore: Hardware-Konfiguration (Cores/RAM) beim Wiederherstellen anpassbar
- PBS Restore: "Nach Restore starten" Checkbox mit automatischem Start
- PBS Restore: Backup-Typ (VM/CT) pro Einzelbackup statt nur pro VMID-Gruppe
- Roadmap-Dokumentation mit 14 geplanten Features (siehe `docs/roadmap.md`)

### Changed
- PBS VMID-Isolation: Blacklist-Ansatz statt Whitelist — Backups gelöschter VMs bleiben sichtbar
- Setup-Script: Webmin-Installation entfernt (nur Dev-System relevant)

### Fixed
- PBS Restore: `apiRequest()` Return-Type `?array` verursachte TypeError bei UPID-String-Rückgabe
- PBS Restore: `restoreVmFromPbs()` und `restoreContainerFromPbs()` crashten bei String-Result
- PBS Restore: ActivityLog::log() falsche Parameter-Reihenfolge (Description als entity_type)
- PBS Restore: Falscher Backup-Typ bei VMIDs mit gemischten VM/CT-Backups
- PBS Restore: Start-Parameter in PVE API unzuverlässig — jetzt separater Start nach Task-Completion
- PBS Backups: Gelöschte VMs verschwanden aus Backup-Liste durch VMID-Whitelist-Filter
- PBS Backups: Frontend-Filter blockierte Restore-Button für Backups ausserhalb des Clusters
- Frontend: `apiFetch()` crash bei leerer Server-Antwort (empty JSON body)

## [0.9.4-beta2] - 2026-03-01

### Added
- ZFS Stale-Log-Resolver: Cron prüft automatisch ob "running" Logs noch aktiv sind und löst sie auf
- ZFS API-Endpoint `recent-logs`: Live-Aktualisierung der "Letzte Ergebnisse"-Tabelle per AJAX
- ZFS API-Endpoint `resolve-stale`: Manuelles Auslösen der Stale-Log-Prüfung per Button
- ZFS "Offene prüfen" Button in der Letzte-Ergebnisse-Karte
- ZFS Server-Report-Mail: Automatischer Sammel-Report pro Server wenn alle Jobs abgeschlossen
- ZFS Empfänger-Logik: Alert-Regeln + Standort-Mail + ZFS-Cron-E-Mail (3 Quellen, dedupliziert)
- SMART Standort-Anzeige: `location_name` in SMART-Queries und API-Response

### Changed
- ZFS "Letzte Ergebnisse": Kompaktes 2-Spalten-Grid gruppiert nach Standort
- SMART Disk Health: Kompaktes 2-Spalten-Grid gruppiert nach Standort, Server als Untergruppen
- ZFS Mail-Report: Pro Server statt pro Standort, Betreff zeigt Server + Standort
- ZFS Fortschrittsbalken: Immer blau (kein Farbwechsel auf grün bei 80%)
- ZFS E-Mail-Feld: Hinweis dass bei leerem Feld Benachrichtigungs-Regeln gelten
- ZFS Logs Sortierung: Nach Standort, Server, Startzeit
- Bitwarden/1Password Autofill-Prevention für ZFS-Cron-E-Mail-Feld

### Fixed
- ZFS Cron: `$db` Variable wurde vor ZFS-Block nicht initialisiert (Pre-existing Bug)
- ZFS Mail-Template: `server_name` Key-Mismatch behoben (war `server`)
- ZFS Mail-Template: `duration` als Int statt formatiertem String übergeben
- ZFS Mail-Template: `formatDurationMail()` Redeclare-Schutz bei mehrfachem Include
- ZFS Mail-Template: Server-Header ohne verschobenes Pfeil-Icon
- Mail-Absendername: Von "PVE Manager" auf "FloppyOps" korrigiert

## [0.9.4-beta1] - 2026-03-01

### Added
- ZFS Reports an mehrere E-Mail-Empfänger: MailService unterstuetzt komma-getrennte Adressen (SMTP + M365 Graph)
- Kunden-Mail pro Standort: `locations.notify_email` — ZFS-Reports werden automatisch auch an Standort-E-Mails gesendet
- Cron-Generator Hinweis für mehrere Empfänger (z.B. admin + Kunden-Mail)

## [0.9.3-beta1] - 2026-02-28
### Added
- Alert-Regeln Community/Pro+ Split: 4 Community-Regeln editierbar (server_offline, disk_usage, ram_usage, cpu_usage), 3 Pro+-only (disk_usage kritisch, update_failed, smart_error)
- Pro+ Badge Glow-Animation (pulsierendes Orange) für alle Pro+-Kennzeichnungen
- ZFS Cron-Generator Pro+ Gate: Zeitpläne nur ab Pro-Lizenz, Community sieht deaktivierte Section mit Pro+ Badge
- LicenseService: COMMUNITY_ALERT_CONDITIONS, isCommunityAlertCondition(), isAlertConditionAvailable()
- Benachrichtigungs-Blocking: Pro+-only Alert-Conditions (smart_error, update_failed) senden in Community keine Mails
- Website: Aktualisierte Feature-Beschreibungen und Pricing-Karten für Community/Pro+ Split

### Changed
- Alert-Regeln: Delete-Button komplett entfernt (7 Standardregeln nicht löschbar)
- Alert-Regeln: Sortierung nach ID ASC statt created_at DESC
- Community Alert-Limit: 3 → 7 Regeln (4 editierbar + 3 Pro+ only angezeigt)
- ZFS Feature-Beschreibungen: "(Pro+)" Hinweis für Zeitpläne

### Removed
- pricing-draft.html (veraltet)
- WebSocket Test-Dateien (test_ws.html, test_ws_simple.html, public/ws-test.html)

### Fixed
- Ungenutzter PbsServer Import in UpdateController entfernt
- Verwaister Docblock in ZfsController entfernt

## [0.9.2] - 2026-02-28

## [0.9.2-beta1] - 2026-02-24
### Added
- Code-Integrität: RSA-4096 signiertes Manifest mit SHA256-Hashes aller Dateien
- IntegrityService: Tamper-Detection mit Status verified/ok/modified/tampered/unknown
- Vertrauensliste: Admins können bewusst geänderte Dateien ausschliessen
- Settings > Allgemein: Integritäts-Karte mit Status, Dateien-Liste, Vertrauen-Button
- Agent Selbstschutz: EXPECTED_HASH Konstante, Integritäts-Check beim Start, /health Flag
- Build-Pipeline: Manifest-Generator, RSA-Signierung, Agent-Hash-Injection
- Release-Server: manifest_signature in API-Response und Download-Header
- Piggyback: Integritäts-Check alle 24h beim Update-Check
- Agent-Download: SHA256-Vergleich gegen gespeicherten Checksum-Wert
- API-Routen: integrity-check, integrity-exclude

### Changed
- PVE Agent Version 2.0.1 → 2.0.2 (Selbst-Integritätscheck)
- Build-Script: 7 → 10 Schritte (Agent-Hash, Manifest, RSA-Signatur)

## [0.9.1] - 2026-02-24
### Added
- Effective Tier: Release-Server bestimmt effektiven Lizenz-Tier basierend auf Support-Status
- Neue LicenseService-Methoden: refreshEffectiveTier(), getOriginalTier(), isSupportExpired(), getSupportExpiresAt()
- Piggyback-Refresh: Effective Tier wird alle 24h beim Update-Check automatisch aktualisiert
- Warn-Banner in Settings > Lizenz wenn Support-Zeitraum abgelaufen ist

### Fixed
- Support-Status auf Portal Renew-Seite zeigte "Abgelaufen" bei unbegrenztem Support (NULL-Handling)

## [0.9.0] - 2026-02-23
### Added
- Dashboard mit Live-SSE-Updates (CPU, RAM, Disk, Netzwerk)
- Server-Verwaltung (CRUD, Cluster, Locations, Sort-Order)
- VM/CT-Steuerung (Start, Stop, Reboot, Klonen, Löschen, Ressourcen)
- VM/CT Konsole via noVNC (WebSocket-Relay, Auto-Start)
- VM RDP-Verbindung (.rdp Download mit Guest-Agent IP-Erkennung)
- Update-Management (apt update/upgrade, SSE-Streaming, Zeitpläne)
- Repository-Verwaltung (Aktivieren/Deaktivieren, No-Subscription Fix)
- Snapshot-Management (Erstellen, Löschen, Rollback, Tree-View)
- ZFS Wartung (Scrub, Trim, Zeitpläne, Fortschrittsanzeige, Logs)
- SMART Disk Health (Discovery, Check, Warnungen, Alerts)
- PBS Integration (Datastores, Tasks, Backups pro VM, Backup/Restore)
- PBS Update-Management (Updates prüfen/installieren, Zeitpläne)
- Terminal (Befehle ausführen via Agent)
- Error-Logs pro Server (Agent-basiert, Persistenz)
- Systemlog (Activity + Error Logs, Filter, Pagination, Export)
- Benutzer-Verwaltung (Admin, Operator, Viewer Rollen)
- Mail-Benachrichtigungen (SMTP + M365 Graph API, Alert-Regeln)
- SSL/Let's Encrypt Verwaltung (ACME, Auto-Renewal)
- DDNS-Konfiguration
- Custom Branding (Enterprise: Logo, Favicon, App-Name, Akzentfarbe)
- Lizenzmodell (Community, Pro, Enterprise mit Feature-Gates)
- Update-Check gegen release.floppyops.com
- PVE Agent v2.0.0 (Python, 22 Endpoints, TLS, Token-Auth)
- Dark + Light Theme (per-User Preference)
- Kompakt/Standard Layout (per-User Preference)
