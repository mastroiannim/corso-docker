# Piano di Ristrutturazione — `mastroiannim/corso-docker`

> Analisi completa del repository · Mappa dei collegamenti · Piano operativo per agente AI
> _Marzo 2026_

---

## Indice

1. [Inventario Completo dei File](#1-inventario-completo-dei-file)
2. [Mappa dei Collegamenti tra File](#2-mappa-dei-collegamenti-tra-file)
3. [Analisi delle Criticità](#3-analisi-delle-criticità)
4. [Piano di Ristrutturazione — Istruzioni per l'Agente AI](#4-piano-di-ristrutturazione--istruzioni-per-lagente-ai)
5. [Struttura Repository Proposta](#5-struttura-repository-proposta)

---

## 1. Inventario Completo dei File

Il repository contiene **13 file Markdown** più **1 file di testo** (`copy-paste`). Di seguito la classificazione per tipologia e fase didattica.

| File | Tipo | Contenuto | Criticità | Azione |
|------|------|-----------|-----------|--------|
| `README.md` | Config | Indice navigabile del corso, struttura fasi, link a tutti i file | Struttura già buona ma link non verificati | Aggiornare link + aggiungere diagramma percorso |
| `Sessione Introduttiva 1 - Sistemi Operativi, Filesystem e Shell.md` | Teoria | Concetti OS, filesystem, shell bash, comandi fondamentali | Molto denso (5h di teoria in un unico file) | Dividere in moduli da 30-45min con checkpoint |
| `SLIDE_ SESSIONE INTRODUTTIVA 1.md` | Slide | Versione slide della Sessione 1 | Formato slide in Markdown non renderizza bene | Aggiungere frontmatter YAML per Marp |
| `Esercitazione Pratica di Laboratorio - Sessione Introduttiva 1.md` | Lab | Lab pratico: conf. ambiente, shell Windows vs Linux | Nessun collegamento esplicito alla teoria | Aggiungere sezione 'Prerequisiti' con link |
| `Sessione Introduttiva 2 - Reti, Applicazioni e Servizi.md` | Teoria | Reti, client-server, virtualizzazione, diagnostica | Molto denso, salto brusco da reti a virtualizzazione | Dividere + aggiungere bridge esplicito verso Docker |
| `SLIDE_ SESSIONE INTRODUTTIVA 2.md` | Slide | Versione slide della Sessione 2 | Stessa criticità SLIDE 1 | Uniformare struttura con SLIDE 1 rivista |
| `Esercitazione Pratica di Laboratorio - Sessione Introduttiva 2.md` | Lab | Lab reti: diagnostica, web server, processi, VM | Dipende da VirtualBox ma non cita il file rete | Aggiungere prerequisito esplicito + link |
| `Esercitazione di Laboratorio -  Modalità di Rete in VirtualBox con Alpine Linux.md` *(doppio spazio)* | Lab | Modalità di rete VirtualBox: NAT, Bridge, Host-Only | **DUPLICATO** — doppio spazio nel nome | Eliminare, mantenere solo la versione con spazio singolo |
| `Esercitazione di Laboratorio - Modalità di Rete in VirtualBox con Alpine Linux.md` | Lab | Identica al file precedente | File canonico da mantenere | Rinominare con nuova convenzione |
| `Esercitazione 1 - Introduzione a Docker e Concetti Base.md` | Lab | Teoria container, architettura Docker, installazione, primi comandi | Mix di teoria e lab non chiaramente separati | Separare sezione 'concetti' da 'hands-on' |
| `Esercitazione 2 - Creazione e Gestione di Container Docker.md` | Lab | Dockerfile, volumi, networking base, multi-container | Prerequisiti impliciti, nessun link a Esercitazione 1 | Aggiungere prerequisiti + scaffolding progressivo |
| `Esercitazione 3 - Docker Avanzato e Applicazioni Distribuite.md` | Lab | Networking avanzato, ottimizzazione, Docker Compose | Progetto finale citato ma file non esiste | Creare file progetto separato + checklist |
| `Verifica di Coerenza e Completezza del Corso Ristrutturato.md` | Config | Meta-documento di verifica interna | Non linkato chiaramente, confonde studenti | Spostare in `docs/` e marcare come INTERNAL |
| `copy-paste` | Config | File di appunti/comandi rapidi (testo grezzo) | Formato informale, non linkato a nulla | Trasformare in `CHEATSHEET.md` strutturato |

---

## 2. Mappa dei Collegamenti tra File

La mappa mostra le dipendenze didattiche (`→` = richiede come prerequisito), i riferimenti incrociati (`↔` = approfondisce lo stesso tema) e i file mancanti (`⚠️`).

### Fase Preliminare — Sessione 1 (OS, Filesystem, Shell)

| File Sorgente | Collega a | Tipo collegamento |
|---|---|---|
| Sessione Intro 1 (teoria) | SLIDE Sessione 1 | ↔ Versione presentazione degli stessi concetti |
| Sessione Intro 1 (teoria) | Esercitazione Lab 1 | → Prerequisito obbligatorio *(implicito, non linkato)* |
| SLIDE Sessione 1 | Sessione Intro 1 (teoria) | ↔ Mirror contenutistico |
| Esercitazione Lab 1 | Sessione Intro 1 (teoria) | → Dipende da (concetti OS, bash) |
| Esercitazione Lab 1 | Esercitazione Lab VirtualBox | → Dipende da (setup VM Alpine per SSH) |

### Fase Preliminare — Sessione 2 (Reti, Servizi, VM)

| File Sorgente | Collega a | Tipo collegamento |
|---|---|---|
| Sessione Intro 2 (teoria) | SLIDE Sessione 2 | ↔ Mirror presentazione |
| Sessione Intro 2 (teoria) | Esercitazione Lab 2 | → Prerequisito *(implicito)* |
| Sessione Intro 2 (teoria) | Esercitazione Lab VirtualBox | → Approfondisce virtualizzazione |
| Sessione Intro 2 (teoria) | Esercitazione Docker 1 | → Bridge concettuale (VM → Container) |
| Esercitazione Lab 2 | Esercitazione Lab VirtualBox | → Dipende da (VM Alpine funzionante) |
| Esercitazione Lab VirtualBox | Esercitazione Lab 2 | → Prerequisito tecnico |
| Esercitazione Lab VirtualBox | Esercitazione Docker 1 | → Prerequisito infrastrutturale |
| ⚠️ FILE DUPLICATO (doppio spazio) | Esercitazione Lab VirtualBox | ⚠️ Duplicato — stesso contenuto |

### Fase Principale — Esercitazioni Docker 1-2-3

| File Sorgente | Collega a | Tipo collegamento |
|---|---|---|
| Esercitazione Docker 1 | Sessione Intro 1 + 2 (teoria) | → Richiama concetti da entrambe |
| Esercitazione Docker 1 | Esercitazione Lab VirtualBox | → Prerequisito infrastrutturale (VM Alpine) |
| Esercitazione Docker 2 | Esercitazione Docker 1 | → Prerequisito *(implicito, non linkato)* |
| Esercitazione Docker 2 | Sessione Intro 2 (teoria) | → Richiama concetti di rete e filesystem |
| Esercitazione Docker 3 | Esercitazione Docker 2 | → Prerequisito *(implicito)* |
| Esercitazione Docker 3 | Sessione Intro 2 (teoria) | → Approfondisce networking avanzato |
| Esercitazione Docker 3 | ⚠️ `[MANCANTE]` Progetto Finale | → Cita progetto finale ma file non esiste |
| Verifica Coerenza.md | Tutti i file | ↔ Meta-documento (solo uso interno) |
| `copy-paste` | *(nessuno)* | ⚠️ File isolato, non linkato a nulla |

---

## 3. Analisi delle Criticità

### 🔴 CRITICO — File duplicato con nome quasi identico

Esistono due file con contenuto identico che differiscono solo per un doppio spazio nel nome:
- `Esercitazione di Laboratorio -  Modalità di Rete in VirtualBox con Alpine Linux.md` *(doppio spazio)*
- `Esercitazione di Laboratorio - Modalità di Rete in VirtualBox con Alpine Linux.md` *(spazio singolo)*

Questo crea confusione in studenti e agenti AI che navigano il repo.

### 🔴 CRITICO — File progetto finale mancante

Il README descrive una "Fase Finale: Progetto Applicativo — Infrastruttura Ibrida VM + Container" con contenuti dettagliati, ma il corrispondente file Markdown **non esiste** nel repository.

### 🟡 IMPORTANTE — Dipendenze implicite, non dichiarate

Nessuna delle esercitazioni dichiara esplicitamente i prerequisiti con link. Uno studente che apre "Esercitazione 2" senza aver completato l'Esercitazione 1 non riceve alcun avviso.

### 🟡 IMPORTANTE — File `copy-paste` non strutturato e non linkato

Il file contiene probabilmente comandi rapidi o snippet, ma non è collegato a nessun altro file e non ha un formato riutilizzabile.

### 🟠 MINORE — Slide in Markdown non ottimizzate

I due file `SLIDE_` sono Markdown piani, non compatibili con strumenti di presentazione (Marp, Reveal.js). Andrebbero convertiti con frontmatter YAML appropriato.

### 🟠 MINORE — Contenuto troppo denso per sessioni da 5 ore

Le due Sessioni Introduttive concentrano molti argomenti in un unico file. Per la fruizione laboratoriale è consigliabile dividere in moduli da 30-45 minuti con obiettivi espliciti e checkpoint.

### Problemi di navigazione

- Nessun file ha sezione "Prerequisiti" in cima con link cliccabili
- Nessun file ha sezione "Dove andare dopo" (next steps) alla fine
- I singoli file non si linkano tra loro
- Non esiste un indice visuale del percorso
- Il file "Verifica di Coerenza" non è chiaramente marcato come documento interno

---

## 4. Piano di Ristrutturazione — Istruzioni per l'Agente AI

> ⚠️ **Eseguire le fasi nell'ordine A → B → C → D → E.** Non saltare la Fase A: il file duplicato può causare errori in tutte le fasi successive.

---

### FASE A — Pulizia e Normalizzazione

#### A0 — Creare un nuovo branch per il corso ristrutturato

Prima di qualsiasi modifica, l'agente deve operare su un branch dedicato per preservare lo stato originale del corso.

```bash
git checkout -b corso-ristrutturato
```

Il branch `main` rimane intatto come riferimento storico. Tutte le modifiche delle Fasi A-E vengono eseguite su `corso-ristrutturato`. Al termine, aprire una Pull Request verso `main` per revisione del docente.

#### A1 — Eliminazione del file duplicato

1. Identificare i due file con nome quasi identico (doppio vs singolo spazio prima di "Modalità")
2. Confrontare il contenuto per confermare che sono identici
3. Eliminare il file con doppio spazio nel nome
4. Rinominare il file canonico in: `00-prerequisiti-rete-virtualbox.md`
5. Aggiornare tutti i link che puntavano al vecchio nome (README, Esercitazione Lab 2, Esercitazione Docker 1)

#### A2 — Trasformazione di `copy-paste` in `CHEATSHEET.md`

1. Leggere il contenuto del file `copy-paste`
2. Creare `CHEATSHEET.md` con struttura a sezioni: Shell Linux | Rete & Diagnostica | Docker Basics | Docker Compose | VirtualBox
3. Distribuire ogni comando/snippet nella sezione appropriata con breve descrizione
4. Aggiungere link a `CHEATSHEET.md` nel README nella sezione "Risorse Aggiuntive"
5. Eliminare il file `copy-paste` originale

#### A3 — Rinominazione file con convenzione consistente

Convenzione adottata: `[fase]-[numero]-[slug-descrittivo].md`

| Nome attuale | Nuovo nome |
|---|---|
| `Sessione Introduttiva 1 - Sistemi Operativi, Filesystem e Shell.md` | `intro-01-teoria-os-filesystem-shell.md` |
| `SLIDE_ SESSIONE INTRODUTTIVA 1.md` | `intro-01-slide-os-filesystem-shell.md` |
| `Esercitazione Pratica di Laboratorio - Sessione Introduttiva 1.md` | `intro-01-lab-os-filesystem-shell.md` |
| `Sessione Introduttiva 2 - Reti, Applicazioni e Servizi.md` | `intro-02-teoria-reti-servizi-vm.md` |
| `SLIDE_ SESSIONE INTRODUTTIVA 2.md` | `intro-02-slide-reti-servizi-vm.md` |
| `Esercitazione Pratica di Laboratorio - Sessione Introduttiva 2.md` | `intro-02-lab-reti-servizi-vm.md` |
| `Esercitazione di Laboratorio - Modalità di Rete in VirtualBox con Alpine Linux.md` | `00-prerequisiti-rete-virtualbox.md` |
| `Esercitazione 1 - Introduzione a Docker e Concetti Base.md` | `docker-01-intro-concetti-base.md` |
| `Esercitazione 2 - Creazione e Gestione di Container Docker.md` | `docker-02-container-volumi-networking.md` |
| `Esercitazione 3 - Docker Avanzato e Applicazioni Distribuite.md` | `docker-03-avanzato-compose-orchestrazione.md` |
| `Verifica di Coerenza e Completezza del Corso Ristrutturato.md` | `docs/INTERNAL-verifica-coerenza.md` |

---

### FASE B — Aggiunta Struttura di Navigazione

> L'agente deve applicare questo template a **tutti** i file del corso (teoria, slide, lab).

#### B1 — Header di navigazione (inserire in cima a ogni file)

```markdown
## 📍 Navigazione

**Fase**: [Preliminare / Principale / Finale]  |  **Modulo**: [numero]  |  **Tipo**: [Teoria / Slide / Lab]

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Nome file prerequisito 1](../path/file1.md)
- [ ] [Nome file prerequisito 2](../path/file2.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- ...
- ...

---
```

#### B2 — Footer di navigazione (inserire in fondo a ogni file)

```markdown
---

## ➡️ Prossimi passi

- **Continua con**: [Nome file successivo](../path/file-next.md)
- **Approfondisci**: [Link risorsa esterna](https://docs.docker.com)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice principale](../README.md)
```

---

### FASE C — Creazione File Mancanti

#### C1 — Creare `docker-04-progetto-finale.md`

Struttura minima richiesta:

- Sezione prerequisiti con link alle Esercitazioni 1, 2, 3
- Descrizione scenario: applicazione web distribuita (es. frontend + backend + db)
- Step 1 — Disegno architettura (VM Alpine + container Docker)
- Step 2 — Configurazione rete VirtualBox (bridge o host-only)
- Step 3 — Creazione Dockerfile per ogni servizio
- Step 4 — Scrittura `docker-compose.yml`
- Step 5 — Test di integrazione con comandi di verifica
- Step 6 — Ottimizzazione (multi-stage build, variabili di ambiente)
- Checklist finale di valutazione (10 criteri verificabili)
- Sezione "Consegna" con istruzioni per export/documentazione

#### C2 — Aggiornare `README.md` con diagramma di flusso

Aggiungere nella sezione "Come Utilizzare Questo Repository":

```
PERCORSO CONSIGLIATO:

[Sessione Teoria 1] → [Slide 1] → [Lab 1]
        ↓
[Sessione Teoria 2] → [Slide 2] → [Lab VirtualBox] → [Lab 2]
        ↓
[Docker 1: Intro] → [Docker 2: Container] → [Docker 3: Compose]
        ↓
[PROGETTO FINALE: Infrastruttura Ibrida]
```

---

### FASE D — Aggiornamento Contenuti

#### D1 — Sessione Introduttiva 1: Divisione in moduli

Dividere il file da 5 ore in 5 sotto-moduli, mantenendo il file originale come indice:

1. `intro-01a-sistemi-operativi.md` — OS e kernel (45 min) + quiz 3 domande
2. `intro-01b-filesystem.md` — struttura directory, permessi (45 min) + esercizio verifica
3. `intro-01c-shell-base.md` — comandi fondamentali `ls`, `cd`, `mkdir`, `cat` (60 min) + lab rapido
4. `intro-01d-shell-avanzata.md` — pipe, redirect, `grep`, `find` (45 min) + lab rapido
5. `intro-01e-recap-connessioni.md` — riepilogo + bridge esplicito verso Docker

#### D2 — Sessione Introduttiva 2: Aggiungere bridge Docker

Il file teoria reti deve concludersi con una sezione "Dalla VM al Container":

- Confronto VM vs Container: tabella risorse, avvio, isolamento
- Domanda riflessiva: "Cosa cambia se invece di una VM avessi un container?"
- Anteprima Docker: cosa vedremo nella Fase Principale

#### D3 — Slide: Aggiungere frontmatter YAML per Marp

Entrambi i file `SLIDE_` vanno aggiornati con:

```yaml
---
marp: true
theme: default
paginate: true
---
```

---

### FASE E — Verifica Finale

> L'agente deve verificare questi punti **dopo** aver implementato le Fasi A-D.

1. Nessun file duplicato nel repository — verificare con `git ls-files | sort | uniq -d`
2. Tutti i file seguono la convenzione di naming adottata
3. Ogni file ha sezione "Prerequisiti" con link funzionanti
4. Ogni file ha sezione "Prossimi passi" con link funzionanti
5. Il file `docker-04-progetto-finale.md` esiste ed è linkato dal README
6. Il file `CHEATSHEET.md` esiste ed è linkato dal README
7. Il file `copy-paste` è stato eliminato
8. Il README ha il diagramma di flusso del percorso
9. Tutti i link nel README puntano ai nuovi nomi file
10. Il file `INTERNAL-verifica-coerenza.md` è spostato in `docs/` e marcato chiaramente

---

## 5. Struttura Repository Proposta

La struttura attuale ha tutti i file flat nella root. Struttura proposta:

```
corso-docker/
├── README.md                          # Indice principale con diagramma percorso
├── CHEATSHEET.md                      # Comandi rapidi organizzati per fase
│
├── fase-0-prerequisiti/
│   └── 00-prerequisiti-rete-virtualbox.md
│
├── fase-1-introduzione/
│   ├── intro-01-teoria-os-filesystem-shell.md   # Indice moduli 1a-1e
│   ├── intro-01a-sistemi-operativi.md
│   ├── intro-01b-filesystem.md
│   ├── intro-01c-shell-base.md
│   ├── intro-01d-shell-avanzata.md
│   ├── intro-01e-recap-connessioni.md
│   ├── intro-01-slide-os-filesystem-shell.md
│   └── intro-01-lab-os-filesystem-shell.md
│
├── fase-2-reti-servizi/
│   ├── intro-02-teoria-reti-servizi-vm.md
│   ├── intro-02-slide-reti-servizi-vm.md
│   └── intro-02-lab-reti-servizi-vm.md
│
├── fase-3-docker/
│   ├── docker-01-intro-concetti-base.md
│   ├── docker-02-container-volumi-networking.md
│   ├── docker-03-avanzato-compose-orchestrazione.md
│   └── docker-04-progetto-finale.md
│
└── docs/
    └── INTERNAL-verifica-coerenza.md
```

---

## Note finali per l'agente

- **Mantenere lo stile laboratoriale**: ogni modulo deve avere almeno un comando da eseguire in terminale
- **Sistema di riferimento**: usare Alpine Linux per tutti i comandi (non Ubuntu o Debian)
- **Target**: studenti di quarta superiore — evitare jargon senza definizione inline
- **Checkpoint verificabili**: ogni checkpoint deve avere un comando con output atteso che lo studente può eseguire autonomamente
- **Non eliminare contenuti**: riorganizzare, non ridurre
- **Branch**: tutte le modifiche vanno sul branch `corso-ristrutturato`; aprire una Pull Request verso `main` al termine per revisione del docente

---

*Piano generato da analisi automatica del repository `mastroiannim/corso-docker` — Marzo 2026*
