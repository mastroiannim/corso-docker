## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1a  |  **Tipo**: Teoria

### ✅ Prerequisiti

Nessun prerequisito — questo è il primo sotto-modulo del corso.

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Spiegare il ruolo operativo di un sistema operativo
- Collegare kernel, processi e filesystem al funzionamento dei container
- Comprendere perché Linux è la base tecnica di Docker

---

# Modulo 1a: Sistemi Operativi e Kernel (essenziale)

## Cosa serve sapere per il percorso Docker

Un sistema operativo gestisce risorse hardware e software. Per il corso ci interessano soprattutto tre aspetti:

- **Kernel**: coordina CPU, memoria, processi e I/O
- **Filesystem**: organizza i dati in directory/file
- **Processi**: esecuzione dei programmi e dei servizi

Docker si appoggia a questi meccanismi Linux per isolare i container.

## Focus didattico su Linux

- I container Docker usano funzionalità del kernel Linux
- Alpine Linux è l'ambiente base del corso
- Sapere usare shell + filesystem Linux è prerequisito diretto per Docker

## Checkpoint rapido

Esegui su Alpine Linux:

```bash
uname -s
uname -r
ps aux | head
```

**Output atteso**:
- `uname -s` restituisce `Linux`
- `uname -r` mostra la versione del kernel
- `ps aux | head` mostra processi attivi (inclusi processi di sistema)

## Rinvio operativo

Per la pratica completa di shell/filesystem:
- [Lab Sessione 1](intro-01-lab-os-filesystem-shell.md)

## Approfondimento opzionale (archivio)

Versione estesa originale del modulo:
- [Modulo 1a esteso (archivio)](../docs/archivio/intro-01a-sistemi-operativi-esteso.md)

---

## ➡️ Prossimi passi

- **Continua con**: [Modulo 1b — Filesystem](intro-01b-filesystem.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice della sessione](intro-01-teoria-os-filesystem-shell.md) · [Torna all'indice principale](../README.md)
