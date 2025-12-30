# Project Cerberus v2.4 — Ghost Howl (Hardened Edition)

**A deniable, Tor-hardened, fully encrypted sentinel for monitoring global Central Bank Digital Currency (CBDC) developments**

**December 2025 — Strictly for research, journalistic, and civil liberties purposes**

## Overview

Cerberus Ghost Howl is a lightweight, single-file Python script designed to periodically archive publicly available information on CBDC initiatives from key open-source trackers and institutional sites. It operates with strong emphasis on privacy, deniability, and operational security:

- All persistent data (configuration and archived evidence) is encrypted using password-derived keys (Fernet symmetric encryption).
- Runs from a stealth directory mimicking system caches (`~/.cache/systemd/journald`).
- Supports routing through Tor for anonymity (with automatic fallback to clearnet if Tor is unavailable).
- Includes a "panic" trigger for immediate secure deletion of all traces.
- Best-effort process name cloaking on Linux.
- Robust error handling, retries, and rotating logs to minimize footprint.

The tool **does not** perform any active scanning, exploitation, or unauthorized access. It only fetches and parses publicly accessible HTML pages, storing clean text excerpts for offline analysis.

This tool is intended for researchers, journalists, privacy advocates, and civil libertarians monitoring the rapid evolution of CBDCs and potential implications for financial privacy, surveillance, and monetary freedom.

## Key Features

- **Full-disk encryption of data**: Uses PBKDF2-HMAC-SHA256 with 480,000 iterations for key derivation from user-provided password.
- **Deniable operation**: No plaintext passwords stored; password prompted at runtime. Invalid password results in decryption failure without revealing contents.
- **Tor integration**: Proxied requests via localhost:9050 if Tor is running.
- **Stealth storage**: Hidden in system-like paths; evidence stored as encrypted files hashed for deduplication.
- **Panic mode**: If `/tmp/.systemd-health` exists, the entire stealth directory is securely wiped on startup.
- **Configurable sources**: Encrypted config allows adding/removing monitored URLs.
- **HTML parsing**: Uses BeautifulSoup to extract clean text while preserving raw HTML.
- **Production-grade reliability**: Exponential backoff retries, session reuse, rotating logs (max 5MB each).

## Default Monitored Sources (as of December 2025)

These are major public CBDC tracking resources, confirmed active:

- https://www.atlanticcouncil.org/cbdctracker/ – Atlantic Council's GeoEconomics Center CBDC Tracker (interactive global overview).
- https://www.bis.org/about/bisih/projects.htm – Bank for International Settlements (BIS) Innovation Hub projects (includes CBDC-related experiments).
- https://cbdctracker.org/ – Independent CBDC Tracker database with country-by-country status updates.

Sources can be modified in the encrypted config after first run.

## Requirements

- Python 3.8+
- Dependencies (install via pip):

```bash
pip install requests cryptography beautifulsoup4
```

- Optional: Running Tor service for anonymized requests (`tor` package on most Linux distros; listens on 9050 by default).

## Installation & Usage

1. Save the script as `cerberus_ghost_howl.py` (or any innocuous name).
2. Make executable (optional): `chmod +x cerberus_ghost_howl.py`
3. Run:

```bash
python3 cerberus_ghost_howl.py
```

- You will be prompted for a strong password (used for encryption/decryption).
- On first run: Generates salt, creates encrypted default config.
- Subsequent runs: Decrypts config, fetches sources, encrypts and stores new/changed page content as evidence files in the stealth directory.

Run periodically (e.g., via cron) for ongoing monitoring.

## Panic Mode

Create the panic file to trigger immediate wipe on next execution:

```bash
touch /tmp/.systemd-health
```

Next run will delete `~/.cache/systemd/journald` entirely and exit.

## Disclaimer & Legal Notes

- This tool is for **lawful research and archival purposes only**.
- All fetched data is from public websites; respect robots.txt and terms of service.
- Users are solely responsible for compliance with local laws regarding data collection, encryption, and anonymity tools.
- No warranty; provided as-is under MIT-like open research terms.

For questions or contributions (research-focused only), discuss in privacy-preserving forums.

**Stay vigilant. Financial freedom depends on awareness.**