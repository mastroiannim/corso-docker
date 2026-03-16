## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 2  |  **Tipo**: Teoria

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Sessione Introduttiva 1 — Teoria](../fase-1-introduzione/intro-01-teoria-os-filesystem-shell.md)
- [ ] [Lab Sessione 1](../fase-1-introduzione/intro-01-lab-os-filesystem-shell.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Spiegare i concetti essenziali di rete utili a Docker (IP, porte, DNS)
- Descrivere il modello client-server in modo operativo
- Usare una diagnostica minima di rete su Alpine Linux
- Comprendere la differenza tra VM e container

---

# Sessione Introduttiva 2: Reti, Applicazioni e Servizi (versione essenziale)

## Panoramica della Sessione
Questa versione è volutamente snella: solo teoria necessaria per affrontare la Fase Docker.
La pratica dettagliata è nel laboratorio associato.

## Percorso minimo consigliato

1. Leggi questa teoria essenziale (30-40 minuti)
2. Esegui il [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md) (centrale)
3. Ripassa le [Slide Sessione 2](intro-02-slide-reti-servizi-vm.md) come promemoria visivo

---

## Parte 1: Rete essenziale per Docker

### Cosa ti serve davvero sapere
- **IP**: identifica un host in rete
- **Porta**: identifica un servizio su quell'host (es. `80`, `443`, `22`)
- **DNS**: traduce nomi (es. `example.com`) in IP
- **Privato vs pubblico**: nella VM userai soprattutto IP privati

### Checkpoint rapido (Alpine)

```bash
ip addr show
ip route show
cat /etc/resolv.conf
```

**Output atteso**:
- almeno un indirizzo IP della VM
- una route `default` verso il gateway
- almeno un `nameserver` nel file DNS

**Installa iproute2 (servirà tra poco per usare ss)**
```bash
apk add iproute2
```

---

## Parte 2: Modello client-server (operativo)

### Idea chiave
- Il **client** invia una richiesta
- Il **server** risponde su una porta
- Docker userà esattamente questo schema tra browser, API, database, container

### Checkpoint rapido (Alpine)

```bash
curl -I https://example.com
```

**Output atteso**: intestazioni HTTP con una riga di stato (es. `HTTP/2 200`).

---

## Parte 3: Diagnostica minima di rete

### Tool minimi che userai anche in Docker
- `ping`: connettività di base
- `ss -tuln`: porte in ascolto
- `curl`: test di un endpoint HTTP
- `ip addr` / `ip route`: stato rete locale

### Checkpoint rapido (Alpine)

```bash
ping -c 3 8.8.8.8
ss -tuln
```

**Output atteso**:
- `ping`: pacchetti inviati/ricevuti con latenza
- `ss`: elenco socket in ascolto (colonna `Local Address:Port`)

👉 Procedura completa e guidata: [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)

---

## Parte 4: Applicazioni, processi, servizi (solo ciò che serve)

### Distinzione pratica
- **Applicazione**: la avvii quando ti serve
- **Servizio**: resta in esecuzione in background
- **Processo**: istanza in esecuzione di un programma

In Docker ragionerai quasi sempre in termini di servizi esposti su porte.

### Checkpoint rapido (Alpine)

```bash
ps aux | head
rc-service --list
```

**Output atteso**:
- lista processi attivi
- elenco servizi disponibili nel sistema

---

## Parte 5: VM vs Container (ponte verso Docker)

| Aspetto | VM | Container |
|---|---|---|
| Cosa virtualizza | Hardware completo | Sistema operativo (kernel condiviso) |
| Avvio | Più lento | Più rapido |
| Dimensione | Maggiore | Minore |
| Uso tipico nel corso | Ambiente host (VirtualBox + Alpine) | Esecuzione servizi applicativi |

### Messaggio chiave
Nel corso userai un modello ibrido: **VM Alpine come base** + **container Docker per i servizi**.

---

## Approfondimento opzionale (contenuto esteso)

La versione estesa della teoria è stata mantenuta in archivio:
- [Versione estesa Sessione 2 (archivio)](../docs/archivio/intro-02-teoria-reti-servizi-vm-estesa.md)

---

## ➡️ Prossimi passi

- **Slide di supporto**: [Slide Sessione 2](intro-02-slide-reti-servizi-vm.md)
- **Configura la rete**: [Lab Modalità di Rete VirtualBox](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)
- **Metti in pratica**: [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice principale](../README.md)
