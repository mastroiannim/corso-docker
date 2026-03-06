# Proposta Operativa — 10 incontri da 3 ore (30 ore totali)

> Corso: Docker e Containerizzazione  
> Target: classe quarta ITI informatica (17-18 anni)  
> Ambiente: Windows 11 + VM Alpine Linux su VirtualBox

## Criterio di pianificazione

Per rispettare il vincolo delle 30 ore:
- in aula si svolgono i task pratici obbligatori e i checkpoint;
- i contenuti di approfondimento restano come studio guidato o recupero;
- i due lab introduttivi restano disponibili in versione estesa, ma in questa proposta sono erogati in versione ridotta da 3h.

---

## Lezione 1 (3h) — Setup ambiente

**File principali**:
- `fase-0-prerequisiti/00-lab-setup-ambiente.md`

**Scansione operativa**:
- 0:00–0:30 → VirtualBox + ISO Alpine
- 0:30–1:30 → creazione VM + installazione Alpine
- 1:30–2:15 → rete + SSH
- 2:15–3:00 → installazione strumenti minimi + test finale

**Output lezione**:
- VM avviabile
- accesso SSH funzionante
- Docker disponibile nella VM

---

## Lezione 2 (3h) — Fondamenti OS/filesystem/shell

**File principali**:
- `fase-1-introduzione/intro-01-teoria-os-filesystem-shell.md`
- `fase-1-introduzione/intro-01a-sistemi-operativi.md`
- `fase-1-introduzione/intro-01b-filesystem.md`
- `fase-1-introduzione/intro-01c-shell-base.md`

**Scansione operativa**:
- 0:00–1:15 → moduli 1a + 1b
- 1:15–2:30 → modulo 1c (comandi base)
- 2:30–3:00 → checkpoint sessione + consegna breve

**Output lezione**:
- studenti autonomi con `ls`, `cd`, `mkdir`, `cat`, percorsi assoluti/relativi

---

## Lezione 3 (3h) — Shell avanzata + Lab 1 ridotto

**File principali**:
- `fase-1-introduzione/intro-01d-shell-avanzata.md`
- `fase-1-introduzione/intro-01e-recap-connessioni.md`
- `fase-1-introduzione/intro-01-lab-os-filesystem-shell.md`

**Scansione operativa**:
- 0:00–0:45 → modulo 1d
- 0:45–1:15 → modulo 1e (ponte verso Docker)
- 1:15–3:00 → Lab 1 (selezione parti obbligatorie: Parte 2, 3, 4)

**Output lezione**:
- uso base di pipe/redirezioni e gestione file pronta per Docker

---

## Lezione 4 (3h) — Reti essenziali + diagnostica

**File principali**:
- `fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md`
- `fase-2-reti-servizi/intro-02-slide-reti-servizi-vm.md`
- `fase-2-reti-servizi/intro-02-lab-reti-servizi-vm.md`

**Scansione operativa**:
- 0:00–0:45 → teoria essenziale rete/client-server
- 0:45–2:30 → Lab 2 (parti obbligatorie: Parte 1, 2, 3)
- 2:30–3:00 → checkpoint rete + Q&A

**Output lezione**:
- diagnostica base (`ip`, `ping`, `curl`, `ss`) e modello client-server operativo

---

## Lezione 5 (3h) — VirtualBox networking (approfondimento applicato)

**File principali**:
- `fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md`
- `fase-2-reti-servizi/intro-02-lab-reti-servizi-vm.md` (Parte 5 come raccordo)

**Scansione operativa**:
- 0:00–1:00 → NAT vs Bridge vs Host-Only (scenario guidato)
- 1:00–2:15 → test connettività e port forwarding
- 2:15–3:00 → troubleshooting guidato

**Output lezione**:
- classe in grado di scegliere e configurare la modalità rete adatta

---

## Lezione 6 (3h) — Docker 1: basi operative

**File principali**:
- `fase-3-docker/docker-01-intro-concetti-base.md`

**Scansione operativa**:
- 0:00–1:00 → concetti container, architettura Docker
- 1:00–2:30 → esercizi 1–4
- 2:30–3:00 → esercizio 5 + debrief

**Output lezione**:
- gestione ciclo di vita container e immagini

---

## Lezione 7 (3h) — Docker 2: immagini e persistenza

**File principali**:
- `fase-3-docker/docker-02-container-volumi-networking.md`

**Scansione operativa**:
- 0:00–1:15 → Dockerfile e build
- 1:15–2:15 → volumi e bind mount
- 2:15–3:00 → networking base tra container

**Output lezione**:
- build di immagine custom e persistenza dati funzionante

---

## Lezione 8 (3h) — Docker 3: Compose e orchestrazione base

**File principali**:
- `fase-3-docker/docker-03-avanzato-compose-orchestrazione.md`

**Scansione operativa**:
- 0:00–0:45 → networking avanzato (focus essenziale)
- 0:45–2:15 → Docker Compose (file, avvio, log, stop)
- 2:15–3:00 → preparazione progetto finale

**Output lezione**:
- stack multi-servizio avviato con Compose

---

## Lezione 9 (3h) — Progetto finale (implementazione)

**File principali**:
- `fase-3-docker/docker-04-progetto-finale.md`

**Scansione operativa**:
- 0:00–0:30 → Step 1–2 (architettura + rete)
- 0:30–2:15 → Step 3–4 (Dockerfile + compose)
- 2:15–3:00 → checkpoint tecnico intermedio

**Output lezione**:
- progetto avviabile con frontend, backend, db

---

## Lezione 10 (3h) — Progetto finale (test, ottimizzazione, consegna)

**File principali**:
- `fase-3-docker/docker-04-progetto-finale.md`
- `CHEATSHEET.md`

**Scansione operativa**:
- 0:00–1:15 → Step 5 (test integrazione)
- 1:15–2:15 → Step 6 (ottimizzazione minima)
- 2:15–3:00 → checklist finale + consegna

**Output lezione**:
- progetto verificato, documentato e consegnato

---

## Riepilogo copertura file

- Fase 0: setup + networking VirtualBox
- Fase 1: teoria modulare + lab operativo ridotto
- Fase 2: teoria essenziale + lab operativo ridotto
- Fase 3: Docker 1-2-3 + progetto finale completo
- Supporto trasversale: `CHEATSHEET.md`

## Nota per erogazione

In questa pianificazione le attività non svolte in aula dei lab introduttivi restano utilizzabili come:
- recupero per studenti assenti;
- esercitazione autonoma;
- materiale di consolidamento prima delle verifiche pratiche.
