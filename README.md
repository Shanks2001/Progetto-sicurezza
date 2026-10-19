# 🔐 Penetration Testing su Piattaforma Gestionale Laravel
Penetration test white-box su un’app Laravel: analisi Cyber Kill Chain, RCE via file upload bypass (CVE-2025-27515), exfiltration e remediation.

**Analisi di sicurezza offensiva e difensiva su un'applicazione web Laravel 10**, condotta seguendo la metodologia **Cyber Kill Chain**, nell'ambito del corso di Programmazione per il Web — Università degli Studi di Bari Aldo Moro (A.A. 2025/2026).

> ⚠️ **Disclaimer**: il test è stato eseguito in modalità **White Box**, su un'applicazione sviluppata dal team stesso come progetto didattico, in un ambiente di laboratorio isolato (rete locale/NAT tra macchine virtuali). Nessun sistema di terze parti è stato coinvolto. Il contenuto ha scopo esclusivamente educativo.

## 👥 Team — "Gli Accucchiati"
- Gabriele Pulinas
- Giovanni Tamborrino
- Francesco Trani

## 🎯 Obiettivo del progetto

Valutare la resilienza di una piattaforma gestionale Laravel (autenticazione, gestione progetti di ricerca, gestione personale) tramite un ciclo completo di attacco e difesa:

1. **Red Team** — simulazione di un attacco reale seguendo le 7 fasi della Cyber Kill Chain
2. **Blue Team** — analisi della causa radice (Root Cause Analysis) e implementazione delle contromisure

## 🛠️ Metodologia e fasi dell'attacco

| Fase | Attività | Strumenti |
|---|---|---|
| 1. Reconnaissance | Fingerprinting dello stack, enumerazione endpoint | `nmap`, `gobuster`, `nikto`, `whatweb` |
| 2. Weaponization | Creazione di una Polyglot Image (JPEG + payload PHP nei metadati EXIF) | `exiftool` |
| 3. Delivery | Upload del file armato tramite endpoint `/profile`, bypass del Content-Type | `Burp Suite` |
| 4. Exploitation | Remote Code Execution (RCE) via file caricato | Browser / richieste GET |
| 5-6. Installation & C2 | Web shell come canale di comando non convenzionale su HTTP | — |
| 7. Action on Objectives | Esfiltrazione di `.env` e del database SQLite tramite reverse shell PowerShell/Netcat | `netcat`, PowerShell |

## 🐞 Vulnerabilità principali identificate

- **CVE-2025-27515** — Laravel File Validation Bypass (wildcard validation su `files.*`)
- **CWE-434** — Unrestricted Upload of File with Dangerous Type: il `ProfileController` usava `getClientOriginalName()` + `storeAs()`, permettendo di controllare nome ed estensione del file salvato
- Header di sicurezza mancanti (`X-Content-Type-Options`, MIME sniffing)
- Cookie `XSRF-TOKEN` senza flag `HttpOnly`
- Policy CORS permissiva (`Access-Control-Allow-Origin: *`)

## 🛡️ Mitigazioni implementate (Blue Team)

- Sostituito il metodo insicuro `storeAs()` con il metodo standard `store()` di Laravel, che genera nomi file casuali e sicuri invece di fidarsi dell'input utente
- Consigliato l'header `X-Content-Type-Options: nosniff` per prevenire il MIME sniffing lato browser

## 📄 Report completo

Il report integrale (PDF), con tutti gli output dei tool, gli screenshot e l'analisi dettagliata di ogni fase, è disponibile in [`/report/Documento_Sicurezza.pdf`](./report/Documento_Sicurezza.pdf).

## 📁 Struttura consigliata della repository

```
.
├── README.md
├── report/
│   └── Documento_Sicurezza.pdf
└── screenshots/          # eventuali immagini estratte dal report
```

## 🧰 Competenze dimostrate

`Penetration Testing` · `Cyber Kill Chain` · `Web Application Security` · `Laravel/PHP Security` · `Burp Suite` · `Nmap` · `Gobuster` · `Nikto` · `Reverse Shell` · `Vulnerability Remediation` · `CWE/CVE Analysis`
