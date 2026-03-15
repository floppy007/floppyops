# Changelog

Alle Änderungen am Projekt.
Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.1.0/).

## [Unreleased]

## [0.9.9-beta1] - 2026-03-14

### Fixed
- **Health-Alerts kamen zu spaet/gar nicht**: Cron nutzte nur `cached_stats` aus DB, die nur aktualisiert wurden wenn jemand die UI offen hatte. Jetzt werden **Live-Daten via PVE API** geholt
- **Settings Save-Button blieb im Spinner haengen**: `apiFetch` gab bei Fehler `null` zurueck, Button wurde nie zurueckgesetzt. Jetzt mit try/catch + `.catch()` abgesichert
- **Health-Check Dropdown zeigte gespeicherten Wert nicht an**: PHP numerische Array-Keys wurden zu int gecastet, strict-Vergleich mit DB-String schlug fehl. Fix: `(string)` Cast
- **Dashboard PBS Warning-Spam**: `Undefined array key "host"` bei PBS-Server ohne Host-Feld spammte Apache error.log. Fix: Null-Coalescing `$pbs['host'] ?? ''`
- **USV doppelte Offline-Mails**: `cached_status` wurde bei Unerreichbarkeit nicht aktualisiert — jede Minute neue Offline-Mail statt nur einmal. Jetzt wird `cached_status` auf `OFFLINE` gesetzt nach erstem Alert
- **USV "wieder erreichbar"-Mail fehlte**: Nach Unerreichbarkeit (nicht Batterie) wurde keine back_online-Mail gesendet, da nur OB→OL geprueft wurde. Jetzt wird auch OFFLINE→OL erkannt
- **Integritaets-Banner blieb nach Update stehen**: `runtimeGuard()` setzte `integrity_runtime_alert` bei Erkennung, raeumte das Flag aber nie auf wenn alle Dateien wieder OK waren. Jetzt werden stale Flags automatisch geloescht

### Added
- **Storage-Schwellwert-Alerts**: Cron prueft jetzt alle aktiven Storages gegen `disk_warning_threshold` und `disk_critical_threshold` aus Einstellungen — nicht mehr nur Root-Partition
- **Server-Offline-Erkennung im Cron**: Wenn PVE API nicht erreichbar, wird `server_offline` Alert ausgeloest und Status in DB aktualisiert
- **HTML Mail-Template `health-alert.php`**: Professionelles Dark-Theme Template mit Host-Warnungen (CPU/RAM/Disk) und Storage-Warnungen Tabelle, Farbkodierung nach Schwere
- **Health-Check Intervall einstellbar**: Neues Dropdown unter Settings > Allgemein (2/3/4/5/10/15/20/30 Min, Default 3 Min). Wird in `app_settings.health_check_interval` gespeichert und vom Cron gelesen
- 15 neue i18n-Keys (`mail.health_*`, `settings.health_*`) in EN + DE

### Changed
- **Health-Alert Cron aktualisiert cached_stats**: Beim Live-Polling werden die Stats gleich in die DB geschrieben — Dashboard profitiert auch ohne offene Detail-Seite
- **Crontab auf `* * * * *`**: Cron laeuft jede Minute, Health-Check Intervall wird ueber `app_settings` gesteuert (Default 3 Min)
- **Settings-Formular mit `autocomplete="off"`**: Verhindert Bitwarden/Autofill-Crashes beim Speichern
- **USV Mail-Betreff mit Uhrzeit**: Datum + Uhrzeit wird an den Subject angehaengt fuer bessere Zuordnung

## [0.9.8-beta3] - 2026-03-12

### Fixed
- **Self-Update ueberschreibt neue Dateien mit altem Hotfix**: `autoApplyLatestPatch()` nutzte `APP_VERSION` (alte Version zur Laufzeit) statt die neue Zielversion — lud Hotfix der alten Version und ueberschrieb damit die gerade installierten neuen Dateien
- **SSE Update-Stream Timeout**: `set_time_limit(0)` und `ignore_user_abort(true)` fehlten im SSE-Handler — bei langen Updates konnte PHP den Prozess nach 30s abbrechen

## [0.9.8-beta2] - 2026-03-12

### Fixed
- **Integritaets-Alarm nach Update**: Guard-Cache wurde VOR Hotfix-Apply regeneriert — nach Update erschien faelschlicherweise "Code-Integritaetsverletzung". Guard-Cache und Alert-Flags werden jetzt NACH Patches zurueckgesetzt
- **"Update verfuegbar" nach Update**: Update-Cache (`update_available`) wurde nach erfolgreichem Update nicht geleert — UI zeigte weiter "Update verfuegbar" fuer die gerade installierte Version

## [0.9.8-beta1] - 2026-03-12

### Added
- **USV Mail-Alerts**: HTML-Mail-Template (`ups-alert.php`) mit Batterie/Last-Balken, Runtime, Input/Output V, Temperatur — nutzt globales Template-System (`sendTemplate()`)
- **USV Alert-Regeln** (Schema v34): 4 Standard-Regeln (`ups_on_battery`, `ups_low_battery`, `ups_offline`, `ups_back_online`) mit konfigurierbarem Empfaenger
- **USV Karten erweitert**: Groessere Gauges (110px), Runtime-Fortschrittsbalken, Stats-Grid (Temp, Bat.Spg., Frequenz, Leistung), groesserer Spark-Chart (48px)
- 16 neue i18n-Keys (`ups.mail_*`, `ups.runtime`, `ups.frequency`, `ups.power_va`, `ups.battery_voltage_short`) in EN + DE

### Fixed
- **USV Cron SQL-Fehler**: `s.node_name` Spalte existiert nicht in `servers` — crashte Shutdown-Regeln nach Batterie-Alert
- **USV Cron Mail**: Pruefte `smtp_host` statt `host` — Mail wurde nie gesendet
- **Dashboard Disk-Picker**: `clusterSelected.length >= 0` immer true — Cluster-Auswahl wurde ueberschrieben
- **ZFS Replikation**: GUID-Validierung beim Inkremental-Check — erkennt Snapshot-Mismatch (gleicher Name, unterschiedliche GUID) und faellt automatisch auf Full-Send zurueck statt zu scheitern
- **ZFS Replikation**: Mehrzeilige ZFS-Fehlermeldungen crashten den Status-Updater (Python SyntaxError) — Fehler werden jetzt via Temp-Datei uebergeben statt Shell-String-Interpolation
- **PVE Agent 2.5.0**: Beide Replication-Fixes im Agent-Script
- **ZFS Replikation**: Config-Sync nutzt jetzt Base64-Encoding statt Shell-echo (verhindert Command-Injection ueber VM-Config-Inhalte)
- **ZFS Replikation**: Agent-Auto-Update wird jetzt erst nach Source-Validierung ausgefuehrt (vermeidet unnoetige Restarts)
- Community-Ping wertet jetzt Server-Antwort aus — bei Admin-Deaktivierung wird lokale Registrierung entfernt
- Verification-Code-E-Mail nutzt jetzt App-Sprache statt englischem Fallback
- Server-Ping ueberschreibt `is_active` nicht mehr (Admin-Deaktivierung bleibt bestehen)

## [0.9.7-beta7] - 2026-03-06

## [0.9.7-beta6] - 2026-03-06

## [0.9.7-beta5] - 2026-03-05

### Added
- **Community-Aktivierungspflicht**: 30-Tage-Frist nach Erstinstallation, danach Registrierung oder Lizenzschluessel erforderlich
  - Standalone Aktivierungsseite (`/activation`) sperrt alle Routen nach Ablauf
  - API-Routes bleiben erreichbar (Aktivierungsformular funktioniert weiterhin)
  - Countdown-Banner in Settings > Lizenz zeigt verbleibende Tage
- **E-Mail-Verifizierung**: Community-Registrierung erfordert 6-stelligen Code per E-Mail
  - Code-Anforderung via `floppyops.com/api/community/request-code`
  - Verifizierung via `floppyops.com/api/community/verify-code`
  - Rate-Limit: max 3 Codes pro E-Mail/Stunde, 15 Minuten Gueltigkeit
- Schema v27: `first_installed_at` Tracking in `app_settings`
- 26 neue i18n-Keys (`activation.*`, `status.sending`) in EN + DE

### Changed
- **Lizenz-Tab vereinheitlicht**: Community-Registrierung und Lizenzschluessel-Eingabe in einer Karte mit Tab-Umschaltung
  - Registrierte Community-User sehen Upgrade-Option direkt
  - Aktive Lizenz-Info und Deaktivierung in gleicher Karte
- Community-Registrierung nutzt jetzt E-Mail-Verifizierung statt direkter Registrierung

## [0.9.7-beta4] - 2026-03-05

### Added
- **Inline Integrity Guard**: Unabhaengiger Hash-Check von LicenseService.php direkt in index.php
  - SHA256-Hash wird beim Build injiziert (GUARD_HASH_LICENSE Konstante)
  - Erkennt LicenseService-Manipulation auch wenn IntegrityService gehackt wurde
  - Im Dev-Modus inaktiv (leerer Hash = kein Check)
- **Page Transition Loader**: Spinner-Overlay beim Seitenwechsel und Refresh
  - Aktiviert bei internen Link-Klicks und Formular-Submits
  - bfcache-kompatibel (pageshow Event)
- **Umfangreiche Test-Suite**: 224 Unit-Tests + 19 Integrations-Tests
  - IntegrityService: 28 Tests (check, runtimeGuard, guardCache, verifyDownload, systemCheck)
  - UpdateService: 33 Tests (Lock-Management, Maintenance, Protected Paths, Backups)
  - UpdateCheckService: 12 Tests (Cache, Channel-Erkennung)
  - PatchService: 20 Tests (State-Management, Rollback, Hash-Verifikation)
  - Integrations-Test: 19 Tests gegen Live-Test-Server (Login, Manipulation, Detection, Exclusion)
- **Tests im Release-Workflow**: PHPUnit wird automatisch vor dem Build ausgefuehrt, Abbruch bei Fehlern

### Changed
- Update-Backups: Limit von 5 auf 10 erhoeht
- Update-Backups Card zeigt max. 5 Eintraege, Link zu voller Backup-Uebersicht
- Neue eigene Seite fuer alle Backups (Settings > Backups) mit Tabellen-Layout
- Build-Script: Neuer Step 6b fuer Inline-Guard Hash-Injection

## [0.9.7-beta2] - 2026-03-05

## [0.9.7-beta1] - 2026-03-05

### Added
- PVE Failed Tasks: Fehlgeschlagene PVE-Tasks (qmigrate, vzdump, etc.) automatisch im Systemlog erfassen
  - Neue Methode `getFailedTasks()` in ProxmoxAPIService (PVE API: `/nodes/{node}/tasks?errors=1`)
  - UPID-basierte Deduplizierung in `persistFailedTasks()` (keine Duplikate bei wiederholtem Abruf)
  - SSE-Zyklus: Alle 600 Zyklen (Offset 450) automatisch abgerufen
  - Source `pve-task` in `server_error_logs`, automatisch im Systemlog sichtbar via UNION-Query
- **Runtime Integrity Guard**: Automatische Erkennung von Code-Manipulation bei jedem Auth-Request
  - Prüft 4 kritische Dateien (index.php, AuthMiddleware, IntegrityService, LicenseService)
  - mtime-first Strategie: Fast-Path ~0.3ms, Worst-Case ~3ms
  - 60-Sekunden-Throttle für Performance
  - Admin-Banner bei Manipulation erkannt (mit Dismiss-Button)
- **License-bound Phone-Home**: Integritäts-Status an Release-Server melden
  - Periodischer Report als Piggyback im Update-Check (alle 24h)
  - Emergency Report bei akuter Manipulation (max 1x/Stunde, fire-and-forget)
  - Release-Server kann Force-Check und Admin-Nachrichten zurückgeben
- **Automatisches Lizenz-Downgrade**: Bei Code-Tampering wird Lizenz auf Community herabgestuft
  - Release-Server prüft Fingerprint gegen Integrity-Reports
  - Downgrade wirkt bei nächster Online-Lizenz-Validierung
  - Admin-Reset im Release-Server Admin-Panel für false positives
  - Warning-Banner in Settings > Lizenz bei aktivem Downgrade
- Release-Server: Integrity-Reports in Lizenzen-Admin-Seite integriert (3. Tab mit Filtern + Downgrade-Reset)

### Changed
- Auto-Update UI: Cron-Dropdowns (Tag + Uhrzeit) statt Freitext-Eingabe für App- und Agent-Auto-Update
- Auto-Update Auto-Save: Toggle und Dropdown-Änderungen werden sofort gespeichert (kein Save-Button)
- Agent Auto-Update Default: Standard auf AN geändert (war AUS)

### Fixed
- `tools/update.php`: APP_VERSION wird jetzt korrekt aus index.php geladen (CLI-Bug behoben)
- Error-Log Timestamp-Regex: Unterstützt jetzt `+01:00` Format zusätzlich zu `+0100` (verhinderte Duplikate)
- UpdateController: Ungenutzten PbsServer Import entfernt

## [0.9.5-beta2] - 2026-03-03

### Added
- Sicherheits- & Einrichtungswarnungen :12 System-Checks in Settings > Allgemein
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
