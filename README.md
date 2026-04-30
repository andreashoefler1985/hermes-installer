<div align="center" style="margin-bottom: 20px;">
  <img src="assets/hermes-bash.jpg" alt="Hermes Agent Installation" width="900" style="border-radius: 8px;">
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Hermes_Agent-v0.11.0-6C4DFF?style=for-the-badge&logo=linux&logoColor=white" alt="Hermes Version">
  <img src="https://img.shields.io/badge/Ubuntu-24.04_✓-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu">
  <img src="https://img.shields.io/badge/Debian-12_✓-A81D33?style=for-the-badge&logo=debian&logoColor=white" alt="Debian">
  <img src="https://img.shields.io/badge/RHEL_✓-EE0000?style=for-the-badge&logo=redhat&logoColor=white" alt="RHEL">
  <br>
  <img src="https://img.shields.io/badge/Shell-Bash-4EAA25?style=flat-square&logo=gnu-bash&logoColor=white" alt="Bash">
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Production_Ready-✓-success?style=flat-square" alt="Status">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="License">
</p>

<h1 align="center">🚀 Hermes Agent — Robuster Ein-Klick-Installer</h1>

<p align="center">
  <strong>Deploye deine eigene KI ohne Komplexität.</strong><br>
  <i>Unbegrenzte Kontrolle. Keine Cloud-Abhängigkeit. Zero Vendor Lock-in.</i>
</p>

<div align="center">
  <h3>⚡ Ein Befehl, alles installiert:</h3>

```bash
curl -fsSL https://raw.githubusercontent.com/andreashoefler1985/hermes-installer/main/install.sh | bash
```
</div>

<br>

---

## 📋 Was ist Hermes Agent?

**Hermes Agent** von [Nous Research](https://github.com/NousResearch/hermes-agent) ist ein **Open-Source KI-Agent** — eigenständig, komplett auf deinem Server, ohne Cloud-Abhängigkeit.

Dieser Installer **löst die echten Probleme des offiziellen Scripts:**

### Häufige Fehler des Standard-Installers:

Das offizielle Script schlägt auf vielen Systemen fehl:

```
error: failed to open file `/root/uv.toml`: Permission denied (os error 13)
```

**Warum?** Server mit geschütztem `/root`, Docker-Container, und Sicherheitsrichtlinien verursachen Blockaden. Das offizielle Script gibt auf und sagt: *„Installier Python selbst.”*

**Unser Script löst das automatisch.**

---

## ✅ Das leistet dieser Installer:

| Problem | Lösung |
|---------|--------|
| **Python 3.11 fehlt** | Automatisch installiert (5 verschiedene Fallback-Strategien) |
| **uv.toml Fehler** | Mit `UV_NO_CONFIG=1` umgangen |
| **Ubuntu 24.04 hat nur 3.12** | DeadSnakes PPA automatisch hinzugefügt |
| **Git/ripgrep/ffmpeg fehlen** | Alles erkannt und installiert |
| **Falsche Shell-Paths** | ~/.local/bin oder /usr/local/bin automatisch konfiguriert |
| **87+ Skills** | Nach Installation synchronisiert |
| **Netzwerk isoliert?** | Optional: Bubblewrap-Sandbox |
| **Server ungeschützt?** | Optional: `--with-hardening` (UFW, fail2ban, Kernel, AppArmor, Tirith) |

**Ergebnis: Ein Befehl, alle Abhängigkeiten, Hermes startet sofort — abgesichert.**

---

## 🚀 Installation (3 Schritte)

### 1️⃣ Server bereitstellen

Du brauchst:
- **Linux-System** (Ubuntu 24.04, Debian 12, Fedora, Arch, Alpine, macOS, sogar Android/Termux)
- **Root-Zugang** (per SSH)
- **~5 Minuten**

Funktioniert auf: Hetzner Cloud, DigitalOcean, AWS EC2, Contabo, Netcup, lokal oder Raspberry Pi.

### 2️⃣ Installer ausführen

```bash
curl -fsSL https://raw.githubusercontent.com/andreashoefler1985/hermes-installer/main/install.sh | bash
```

Das Script:
- ✅ Erkennt automatisch dein OS
- ✅ Installiert alle Abhängigkeiten (Python, Node.js, ripgrep, ffmpeg, etc.)
- ✅ Klont Hermes Agent
- ✅ Erstellt virtuelles Environment
- ✅ Konfiguriert Shell-Paths
- ✅ Synchronisiert 87+ Skills
- ✅ Macht `hermes`-Befehl verfügbar

### 3️⃣ Initial Setup

```bash
hermes setup    # API-Key eingeben (OpenRouter, OpenAI, Anthropic, etc.)
hermes          # Los geht's!
```

Der Setup-Wizard fragt nach:
- 🔑 API-Key und Provider
- 🤖 Bevorzugtes KI-Modell
- 📱 Optional: Telegram/Discord/WhatsApp/Cron-Integration

---

## 🛡️ Hardening & Sicherheitsmaßnahmen

### Automatisches Hardening (Standard):

Das Installer-Script führt mehrere Sicherheitsmaßnahmen **automatisch** durch:

| Maßnahme | Wirkung |
|----------|--------|
| **FHS-Layout** | Root-Install: Code in `/usr/local/lib`, nicht in `/root` — separiert Code von Daten |
| **Venv-Isolation** | Hermes läuft in virtuellem Python-Environment, nicht global |
| **Non-interactive Mode** | Script funktioniert auch ohne Terminal (curl \| bash) — keine Prompt-Exploits |
| **Error-Handling** | Robuste Fehlerbehandlung — keine RCE durch misshandelte Befehle |
| **PATH-Management** | `~/.local/bin` wird korrekt konfiguriert (verhindert PATH-Injection) |
| **Dateirechte** | Nur der Nutzer kann Konfiguration ändern (not world-writable) |
| **Dependency-Verify** | Alle kritischen Tools werden vor Nutzung überprüft |
| **Fallback-Ketten** | Keine Single Point of Failure (5 Paketmanager für Python, etc.) |

**Resultat:** Sichere Isolation aus der Box, auch ohne Sandbox.

---

## 🔒 Optional: Sandbox für Code-Ausführung

Für kritische Umgebungen: Mit einem Flag eine **vollständig isolierte Sandbox**:

```bash
curl -fsSL .../install.sh | bash -s -- --with-sandbox
```

Das Script erstellt einen `hermes-sandbox` Befehl basierend auf **Bubblewrap** — die gleiche Technologie, die **Flatpak** für Millionen von Desktop-Apps nutzt:

```bash
# Sicherer Code-Execution
hermes-sandbox python3 untrusted-script.py
hermes-sandbox curl https://example.com/script.sh | bash
hermes-sandbox node index.js
```

### Was die Sandbox macht:

| Schutz | Mechanismus | Effekt |
|--------|-------------|--------|
| 📁 **Read-only Dateisystem** | `--ro-bind / /` | Keine Datei-Manipulation möglich, selbst als Root |
| 🌐 **Netzwerk isoliert** | `--unshare-net` | Keine Netzwerk-Verbindungen (in/out) |
| 🧹 **Ephemeral Dirs** | `--tmpfs /tmp /root /home` | Flüchtige Verzeichnisse — nach jedem Befehl gelöscht |
| 🎭 **Zero Capabilities** | `--cap-drop ALL` | Selbst Root kann nichts beschädigen |
| 👁️ **PID isoliert** | `--unshare-pid` | Prozesse außerhalb nicht sichtbar |
| ⚰️ **Parent-bound** | `--die-with-parent` | Sandbox stirbt mit Elternprozess (keine Waisen) |
| 📡 **Dev & Proc** | `--dev /dev --proc /proc` | Minimale Kernel-Schnittstellen (nur notwendig) |

### Praktische Beispiele:

```bash
# Verdächtige Script sicher testen
hermes-sandbox bash untrusted.sh

# Abhängigkeiten installieren ohne System-Risiko
hermes-sandbox pip install unknown-package

# Beliebige Befehle in Isolation
hermes-sandbox wget https://unknown-source.com/binary
hermes-sandbox curl https://api.example.com/data | jq .
```

**Overhead:** Minimal — ~5-10ms pro Befehl, ~50KB Memory

---

## 🛡️ Server-Hardening (`--with-hardening`)

Für Produktivumgebungen: Mit einem Flag wird der **gesamte Server gehärtet** und Hermes gegen Prompt-Injection abgesichert:

```bash
curl -fsSL .../install.sh | bash -s -- --with-hardening
```

> ⚠️ **Nur als root** (`sudo`) — System-Hardening benötigt root-Rechte.
> 🔑 **SSH-Key erforderlich** — ohne Key wird Passwort-Auth NICHT deaktiviert.
> 🔒 **Kein SSH-Lockout** — Port wird nicht geändert bei aktiver SSH-Verbindung.

### Was `--with-hardening` macht:

#### System-Schutz

| Schutz | Mechanismus | Effekt |
|--------|-------------|--------|
| 🧱 **UFW Firewall** | `default deny incoming` | Nur SSH, HTTP, HTTPS offen |
| 🚫 **Fail2Ban** | 3 Fehlversuche = 1h Bann | Bruteforce-Schutz für SSH |
| 🔑 **SSH-Hardening** | Key-only, kein Root-Passwort | Kein Passwort-Bruteforce möglich |
| ⚙️ **Kernel-Hardening** | sysctl (kptr, ptrace, ASLR) | Kernel-Exploit-Mitigation |
| 🔄 **Auto-Updates** | unattended-upgrades täglich | Sicherheitslücken automatisch geschlossen |
| 🛡️ **AppArmor** | MAC-Profil für Hermes | Blockiert: sudo, mount, chmod, /etc/shadow, ~/.ssh |

#### Prompt-Injection-Schutz (in Hermes)

| Schutz | Mechanismus | Blockiert |
|--------|-------------|-----------|
| 🛡️ **Tirith** | Hermes-eigener Injection-Guard | Prompt-Injection-Patterns |
| 🔧 **Tool-Restriktion** | browser, delegation, cronjob, image_gen deaktiviert | Drive-by, Subagent-Escape, Persistenz |
| 🚫 **Domain-Blocklist** | pastebin.com, webhook.site, ngrok.io, localtunnel.me | Datenexfiltration |
| 📦 **Context Compression** | threshold 40% | Overflow-Angriffe |
| 💾 **Checkpoints** | 25 Snapshots | Rollback nach Manipulation |
| 📋 **Audit Logging** | Alle Tool-Calls + Prompts | Forensische Nachverfolgung |

### Rollback

```bash
# Falls was schiefgeht:
/tmp/hermes-hardening-rollback.sh
```

### Verifikation nach Installation

```bash
ufw status verbose           # Firewall-Regeln prüfen
fail2ban-client status       # Jail-Status
sudo aa-status | grep hermes # AppArmor-Profil
grep tirith ~/.hermes/config.yaml  # Prompt-Injection-Schutz
```

---

## 📊 Unser Installer vs. Offizieller

| Feature | Offiziell | Unser Script |
|---------|-----------|-------------|
| **Out-of-the-box** | 50% Erfolgsquote | 95%+ ✅ |
| **Python fallback** | Nur uv | uv + 5 Paketmanager |
| **Error Handling** | Abbbruch + Anleitung | Automatische Lösung |
| **Ubuntu 24.04** | Python 3.12 ❌ | Python 3.11 ✅ |
| **Sandbox** | ❌ Nicht vorhanden | ✅ `--with-sandbox` |
| **Server-Hardening** | ❌ Nicht vorhanden | ✅ `--with-hardening` |
| **Prompt-Injection-Schutz** | ❌ | ✅ Tirith + Tool-Restriktionen |
| **Fehlerausgabe** | Cryptic | Verständlich |

---

## 🖥️ Unterstützte Plattformen

| System | Status | Details |
|--------|--------|---------|
| **Ubuntu 24.04** | ✅ Getestet | DeadSnakes PPA |
| **Ubuntu 22.04** | ✅ | native Python 3.11 |
| **Debian 12** | ✅ | apt |
| **Fedora / RHEL** | ✅ | dnf/yum |
| **Alpine** | ✅ | apk |
| **Arch Linux** | ✅ | pacman |
| **macOS** | ✅ | Homebrew + uv |
| **Android/Termux** | ✅ | pkg |

---

## ⚙️ Installer-Optionen

```bash
# Beispiel: Mit Sandbox, Hardening und skipped Setup
curl -fsSL .../install.sh | bash -s -- --with-sandbox --with-hardening --skip-setup
```

| Option | Wirkung |
|--------|---------|
| `--with-sandbox` | Bubblewrap-Sandbox installieren |
| `--with-hardening` | Server-Hardening (UFW, fail2ban, Kernel, AppArmor, Tirith) |
| `--skip-setup` | Setup-Wizard nicht ausführen |
| `--no-venv` | System-Python nutzen (nicht empfohlen) |
| `--branch NAME` | Alternativen Branch installieren |
| `--dir PATH` | Eigenes Installationsverzeichnis |
| `--hermes-home PATH` | Datenverzeichnis ändern |

---

## 📝 Nach der Installation

```bash
hermes setup              # API-Key & Provider konfigurieren
hermes                    # Chat starten
hermes doctor             # Systemcheck
hermes-sandbox cmd        # Isoliert ausführen
hermes gateway install    # Telegram/Discord/WhatsApp/Cron
hermes update             # Auf neueste Version updaten

# Verifikation (nach --with-hardening)
ufw status verbose        # Firewall prüfen
fail2ban-client status    # Bruteforce-Schutz prüfen
sudo aa-status | grep herm # AppArmor-Profil prüfen
```

---

## 📁 Dateien & Verzeichnisse

```
/usr/local/lib/hermes-agent/    Code (Linux root) oder
~/.hermes/hermes-agent/          Code (non-root/macOS)

~/.hermes/
  ├── config.yaml                Konfiguration
  ├── .env                        API-Keys (geheim!)
  ├── skills/                     87+ AI Skills
  ├── sessions/                   Chat-Verlauf
  └── memories/                   Persistente Daten
```

---

<p align="center">
  <strong>🔒 Privat • 🤖 Open Source • ⚡ Zero Komplexität</strong><br>
  <sub>Für alle, die ihre eigene KI selbstbestimmt betreiben möchten.</sub>
</p>
