---
marp: true
theme: default
paginate: true
---

## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 2  |  **Tipo**: Slide

### ✅ Prerequisiti

Queste slide sono il supporto visivo alla teoria:
- [ ] [Sessione Introduttiva 2 — Teoria](intro-02-teoria-reti-servizi-vm.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Ripassare i concetti minimi di rete necessari per Docker
- Visualizzare il ponte VM → container
- Sapere quali verifiche eseguire subito nel lab

---

# SLIDE: SESSIONE INTRODUTTIVA 2
## RETI, SERVIZI, VM (ESSENZIALE)

---

## Percorso didattico minimo

1. Teoria essenziale
2. Lab operativo completo
3. Avvio fase Docker

👉 Documento pratico di riferimento: [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)

---

## Slide 1 — Le 4 parole chiave

- **IP**: identifica host
- **Porta**: identifica servizio
- **DNS**: nome → IP
- **Routing**: percorso verso la destinazione

In Docker userai gli stessi concetti per esporre servizi.

---

## Slide 2 — Client/Server in 20 secondi

- Client invia richiesta
- Server risponde su porta
- Esempio: browser → web server

In Docker: browser host → container web (`-p host:container`).

---

## Slide 3 — Diagnostica minima (tool)

- `ip addr show`
- `ip route show`
- `cat /etc/resolv.conf`
- `ping -c 3 8.8.8.8`
- `ss -tuln`
- `curl -I https://example.com`

---

## Slide 4 — Checkpoint rete (atteso)

- presenza di IP nella VM
- route `default` presente
- nameserver configurato
- risposta ICMP con `ping`
- almeno una porta/listener con `ss`

---

## Slide 5 — Applicazione vs Servizio

- Applicazione: avvio manuale, uso diretto
- Servizio: processo in background
- Processo: istanza in esecuzione

Per Docker conta soprattutto: **servizio + porta + log**.

---

## Slide 6 — VM vs Container (confronto utile)

| Aspetto | VM | Container |
|---|---|---|
| Isolamento | SO completo | processi isolati |
| Avvio | più lento | più rapido |
| Peso | maggiore | minore |
| Nel corso | base ambiente | servizi applicativi |

---

## Slide 7 — Modello ibrido del corso

**VirtualBox + Alpine (VM)**
↓
**Docker nella VM**
↓
**Container per i servizi**

Questo è il ponte didattico verso la fase principale.

---

## Slide 8 — Rinvio operativo al Lab

Nel lab fai davvero:
- analisi rete Windows + Alpine
- test client/server
- diagnostica guidata
- gestione servizi
- ponte pratico verso virtualizzazione/container

📌 [Apri il Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)

---

## Slide 9 — Verifica finale rapida

Se sai fare questi 4 passaggi, sei pronto per Docker:
1. leggere IP/route/DNS della VM
2. testare raggiungibilità e HTTP
3. identificare una porta in ascolto
4. spiegare quando usare VM e quando container

---

## Approfondimento opzionale (archivio)

Versione estesa originale delle slide:
- [Slide Sessione 2 estese (archivio)](../docs/archivio/intro-02-slide-reti-servizi-vm-estese.md)

---

## ➡️ Prossimi passi

- [Teoria Sessione 2](intro-02-teoria-reti-servizi-vm.md)
- [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)
- [Docker 1 — Introduzione](../fase-3-docker/docker-01-intro-concetti-base.md)

↩ [Torna all'indice principale](../README.md)
