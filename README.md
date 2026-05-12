# Writeup-Metasploitable

> **📦 Archived (read-only).** No longer maintained — kept for reference only.


**A penetration-test write-up of Metasploitable 2** — recon, scanning, and exploitation of
several services, with the supporting nmap output and notes. Written during cybersecurity
training (Simplon).

[![Type](https://img.shields.io/badge/type-writeup-blueviolet)]()
[![Target](https://img.shields.io/badge/target-Metasploitable%202%20(lab%20VM)-informational)]()
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

> All work was done against the deliberately-vulnerable **Metasploitable 2** training VM in
> an isolated lab. Standard rule: only test what you're allowed to test.

## Contents

- **[SCAN/NMAP.md](SCAN/NMAP.md)** — initial nmap scans (`ping.nmap`, `scan.nmap` outputs included)
- **PENTEST/** — per-service write-ups:
  - [FTP.md](PENTEST/FTP.md)
  - [Telnet.md](PENTEST/Telnet.md)
  - [SMB.md](PENTEST/SMB.md)
  - [IRCD.md](PENTEST/IRCD.md)
  - [WebDAV.md](PENTEST/WebDAV.md)
  - `Images/` — screenshots referenced by the write-ups

Non-exhaustive — a set of attack-vector notes, not a full report. Useful as a worked
example of going from "open port" to "shell" on a known-vulnerable box.

## See also

- [`debian-hardening-writeup`](https://github.com/ZZ0R0/debian-hardening-writeup) — the defensive counterpart (auditing/hardening a Debian box).
- [`road2exploit`](https://github.com/ZZ0R0/road2exploit) — a broader learning roadmap.

## License

[MIT](LICENSE)
