# Corso Ristrutturato: Docker e Containerizzazione

## Panoramica del Corso

Questo corso completo è progettato per fornire agli studenti una comprensione approfondita di Docker e della containerizzazione, partendo dalle basi dei sistemi operativi e delle reti fino all'implementazione di applicazioni distribuite in container.

### Struttura del Corso

Il corso è strutturato in tre fasi principali:

1. [**Fase Preliminare**](#fase-preliminare-allineamento-delle-conoscenze): Due sessioni introduttive di 5 ore ciascuna per acquisire i prerequisiti necessari, ciascuna con una esercitazione pratica di laboratorio di 3 ore
2. [**Fase Principale**](#fase-principale-docker-e-containerizzazione): Tre esercitazioni pratiche di 3 ore ciascuna su Docker e containerizzazione
3. [**Fase Finale**](#fase-finale-progetto-applicativo): Un progetto applicativo che integra tutte le conoscenze acquisite

### Percorso Consigliato

```
[Setup Ambiente] ← INIZIA QUI
        ↓
[Sessione Teoria 1] → [Slide 1] → [Lab 1]
        ↓
[Sessione Teoria 2] → [Slide 2] → [Lab VirtualBox] → [Lab 2]
        ↓
[Docker 1: Intro] → [Docker 2: Container] → [Docker 3: Compose]
        ↓
[PROGETTO FINALE: Infrastruttura Ibrida]
```

### Target

Il corso è rivolto a studenti di quarta superiore di un istituto tecnico industriale a indirizzo informatico, senza prerequisiti specifici. La fase preliminare è progettata per fornire le conoscenze di base necessarie per affrontare con successo la fase principale.

### Ambiente di Lavoro

Gli studenti avranno a disposizione:
- Computer con Windows 11 (account senza privilegi di amministratore)
- VirtualBox installato
- Immagine ISO di Alpine Linux
- Accesso alla VM Alpine Linux tramite SSH

---

## Fase 0: Setup dell'Ambiente di Lavoro

### 🚀 Laboratorio: Setup Completo dell'Ambiente (90-120 minuti)

**⚠️ INIZIA QUI** — Questo è il primo passo obbligatorio del corso.

**Obiettivo**: Configurare passo-passo tutto l'ambiente di lavoro necessario per seguire il corso Docker, partendo da zero.

**Cosa configurerai**:
- ✅ VirtualBox sul tuo computer Windows
- ✅ Alpine Linux in una macchina virtuale
- ✅ Rete funzionante per comunicare con la VM
- ✅ Accesso SSH da Windows alla VM
- ✅ Docker e tutti gli strumenti necessari (editor, git, bash)

**Prerequisiti**: NESSUNO — questa è la base di partenza.

**Materiali**:
- **[📖 Laboratorio: Setup Ambiente](fase-0-prerequisiti/00-lab-setup-ambiente.md)** ← INIZIA DA QUI

**Nota**: Questo laboratorio non richiede conoscenze pregresse di Linux o virtualizzazione. Ogni passo è spiegato in dettaglio.

### Approfondimento: Configurazione Rete VirtualBox (opzionale)

Dopo aver completato il setup base, puoi approfondire le diverse modalità di rete disponibili in VirtualBox.

**Materiali**:
- [Modalità di Rete in VirtualBox](fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)

---

## Fase Preliminare: Allineamento delle Conoscenze

### Sessione Introduttiva 1: Sistemi Operativi, Filesystem e Shell (5 ore)

**Obiettivo**: Fornire le conoscenze di base sui sistemi operativi, la struttura del filesystem e l'utilizzo della shell.

**Contenuti**:
- Introduzione ai sistemi operativi
- Filesystem e organizzazione dei dati
- Introduzione alla shell
- Comandi bash fondamentali
- Comandi avanzati e recap

**Materiali**:
- [Slide Teoriche](fase-1-introduzione/intro-01-slide-os-filesystem-shell.md)
- [Contenuto Teorico](fase-1-introduzione/intro-01-teoria-os-filesystem-shell.md)

**Sotto-moduli** (per studio progressivo):
- [1a — Sistemi Operativi](fase-1-introduzione/intro-01a-sistemi-operativi.md)
- [1b — Filesystem](fase-1-introduzione/intro-01b-filesystem.md)
- [1c — Shell Base](fase-1-introduzione/intro-01c-shell-base.md)
- [1d — Shell Avanzata](fase-1-introduzione/intro-01d-shell-avanzata.md)
- [1e — Recap e Connessioni](fase-1-introduzione/intro-01e-recap-connessioni.md)

### Esercitazione Pratica 1: Sistemi Operativi, Filesystem e Shell (3 ore)

**Obiettivo**: Mettere in pratica i concetti teorici appresi nella Sessione Introduttiva 1.

**Contenuti**:
- Configurazione dell'ambiente di lavoro (Windows e Alpine Linux)
- Confronto tra terminali Windows e Linux
- Navigazione nel filesystem
- Gestione di file e directory
- Esercizio finale integrato

**Materiali**:
- [Esercitazione](fase-1-introduzione/intro-01-lab-os-filesystem-shell.md)

### Sessione Introduttiva 2: Reti, Applicazioni e Servizi (5 ore)

**Obiettivo**: Fornire le conoscenze di base sulle reti informatiche, le applicazioni, i servizi e un'introduzione alla virtualizzazione.

**Contenuti**:
- Concetti fondamentali di reti
- Modello client-server
- Comandi di rete e diagnostica
- Applicazioni e servizi
- Introduzione alla virtualizzazione

**Materiali**:
- [Slide Teoriche](fase-2-reti-servizi/intro-02-slide-reti-servizi-vm.md)
- [Contenuto Teorico](fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md)

### Esercitazione Pratica 2: Reti, Applicazioni e Servizi (3 ore)

**Obiettivo**: Mettere in pratica i concetti teorici appresi nella Sessione Introduttiva 2.

**Contenuti**:
- Analisi della configurazione di rete in Windows e Linux
- Utilizzo di strumenti di diagnostica di rete
- Implementazione di un semplice server web
- Analisi di processi e servizi
- Introduzione pratica alla virtualizzazione

**Materiali**:
- [Esercitazione](fase-2-reti-servizi/intro-02-lab-reti-servizi-vm.md)


## Fase Principale: Docker e Containerizzazione

**Prerequisiti**:
- [Modalità di Rete in VirtualBox](fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)

### Esercitazione 1: Introduzione a Docker e Concetti Base (3 ore)

**Obiettivo**: Introdurre i concetti fondamentali di Docker e della containerizzazione, dall'installazione all'esecuzione dei primi container.

**Contenuti**:
- Teoria della containerizzazione
- Architettura di Docker
- Installazione e configurazione
- Primi passi con Docker

**Materiali**:
- [Esercitazione 1](fase-3-docker/docker-01-intro-concetti-base.md)

**Collegamenti con le Sessioni Introduttive**:
- Utilizzo dei concetti di sistema operativo dalla Sessione 1
- Applicazione dei comandi bash appresi nella Sessione 1
- Riferimenti ai concetti di client-server dalla Sessione 2
- Confronto con la virtualizzazione tradizionale vista nella Sessione 2

### Esercitazione 2: Creazione e Gestione di Container Docker (3 ore)

**Obiettivo**: Approfondire la creazione di immagini Docker personalizzate, la gestione della persistenza dei dati e il networking di base.

**Contenuti**:
- Creazione di immagini Docker
- Persistenza dei dati e volumi
- Networking e distribuzione
- Applicazioni multi-container

**Materiali**:
- [Esercitazione 2](fase-3-docker/docker-02-container-volumi-networking.md)

**Collegamenti con le Sessioni Introduttive**:
- Utilizzo dei concetti di filesystem dalla Sessione 1
- Applicazione dei concetti di gestione dei file dalla Sessione 1
- Riferimenti ai concetti di rete dalla Sessione 2
- Applicazione dei concetti di applicazioni e servizi dalla Sessione 2

### Esercitazione 3: Docker Avanzato e Applicazioni Distribuite (3 ore)

**Obiettivo**: Approfondire aspetti avanzati di Docker, come il networking avanzato, l'ottimizzazione delle immagini, la gestione delle configurazioni e un'introduzione all'orchestrazione.

**Contenuti**:
- Networking avanzato in Docker
- Configurazione e ottimizzazione
- Introduzione all'orchestrazione con Docker Compose
- Preparazione al progetto finale

**Materiali**:
- [Esercitazione 3](fase-3-docker/docker-03-avanzato-compose-orchestrazione.md)

**Collegamenti con le Sessioni Introduttive**:
- Applicazione avanzata dei concetti di rete dalla Sessione 2
- Utilizzo dei concetti di ottimizzazione dalla Sessione 2
- Riferimenti ai concetti di gestione dei servizi dalla Sessione 2
- Integrazione dei concetti di virtualizzazione dalla Sessione 2

## Fase Finale: Progetto Applicativo

### Progetto Finale: Infrastruttura Ibrida VM + Container

**Obiettivo**: Progettare e implementare un'infrastruttura ibrida (VM + container) per una semplice applicazione web distribuita.

**Contenuti**:
- Definizione del progetto
- Configurazione della VM Alpine
- Implementazione dei container
- Configurazione del networking e persistenza
- Orchestrazione con Docker Compose
- Test e ottimizzazione

**Materiali**:
- [Progetto Finale](fase-3-docker/docker-04-progetto-finale.md)

**Collegamenti con le Sessioni Introduttive e le Esercitazioni**:
- Integrazione di tutti i concetti visti nelle sessioni introduttive
- Applicazione pratica delle tecniche apprese nelle esercitazioni Docker
- Implementazione di un'architettura completa che combina VM e container

## Risorse Aggiuntive

### Riferimenti Rapidi

- [📋 CHEATSHEET — Comandi Rapidi](CHEATSHEET.md)

### Documentazione Interna

- [Verifica di Coerenza e Completezza](docs/INTERNAL-verifica-coerenza.md) *(uso interno)*

### Risorse Online

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Docker Hub](https://hub.docker.com/)
- [Alpine Linux Wiki](https://wiki.alpinelinux.org/)

### Glossario

Un glossario completo dei termini tecnici è disponibile nella documentazione del corso.

## Come Utilizzare Questo Repository

1. Iniziare con le sessioni introduttive e le relative esercitazioni pratiche
2. Procedere con le esercitazioni Docker in ordine
3. Completare il corso con il progetto finale
4. Consultare la [CHEATSHEET](CHEATSHEET.md) per un riferimento rapido ai comandi

## Nota per i Docenti

Questo corso è progettato per essere flessibile e adattabile alle esigenze specifiche degli studenti. Le sessioni introduttive possono essere abbreviate o approfondite in base al livello di conoscenza pregresso degli studenti.

Per la prossima erogazione è disponibile una pianificazione operativa completa in 10 incontri da 3 ore:
- [Proposta Operativa — 10 incontri da 3 ore](docs/PIANO-EROGAZIONE-10-INCONTRI-3H.md)
