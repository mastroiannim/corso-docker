# Documentazione del Corso: Docker e Containerizzazione

## Panoramica del Corso

### Obiettivi Formativi
1. Comprendere i principi di base della containerizzazione (Docker, immagini, volumi e networking)
2. Acquisire capacità operative nell'utilizzo di Docker per creare, gestire e distribuire container anche attraverso il DockerHub
3. Analizzare le differenze tra virtualizzazione tradizionale e containerizzazione, valutandone vantaggi e casi d'uso
4. Applicare le tecniche apprese in scenari reali, come il deployment di applicazioni web o servizi distribuiti

### Destinatari
Studenti di quarta superiore di un istituto tecnico industriale a indirizzo informatico con conoscenze pregresse di:
- JavaScript e Node.js
- Virtual Machine (modulo introduttivo)
- Linux Alpine

### Struttura del Corso
- Tre esercitazioni di 3 ore ciascuna
- Progetto finale: implementazione di un'infrastruttura ibrida (VM + container)
- Ambiente di lavoro: VM Linux Alpine con accesso root e connessione Internet via SSH

## Esercitazione 1: Introduzione a Docker e Concetti Base

### Obiettivi Specifici
- Comprendere i concetti fondamentali della containerizzazione
- Confrontare VM e container
- Installare e configurare Docker su Linux Alpine
- Eseguire i primi comandi Docker
- Utilizzare container esistenti
- Comprendere il concetto di immagine Docker

### Contenuti
1. **Teoria della containerizzazione**
   - Cos'è la containerizzazione
   - Storia ed evoluzione dei container
   - Differenze tra VM e container
   - Vantaggi e svantaggi dei container

2. **Architettura di Docker**
   - Componenti principali (Docker Engine, Docker CLI, Docker Registry)
   - Concetto di immagine e container
   - Ciclo di vita di un container
   - Docker Hub e repository di immagini

3. **Installazione e configurazione**
   - Installazione di Docker su Alpine Linux
   - Configurazione dell'ambiente Docker
   - Test di funzionamento
   - Risoluzione dei problemi comuni

4. **Primi passi con Docker**
   - Comandi Docker di base
   - Esecuzione di container
   - Gestione del ciclo di vita dei container
   - Esecuzione di un'applicazione Node.js in container

### Esercizi Pratici
1. Hello World Docker
2. Container interattivo
3. Container in background
4. Gestione del ciclo di vita dei container
5. Esplorazione di Docker Hub
6. Esecuzione di un'applicazione Node.js in container

## Esercitazione 2: Creazione e Gestione di Container Docker

### Obiettivi Specifici
- Creare immagini Docker personalizzate con Dockerfile
- Gestire il ciclo di vita dei container
- Utilizzare volumi per la persistenza dei dati
- Configurare il networking di base
- Pubblicare e utilizzare immagini su DockerHub

### Contenuti
1. **Creazione di immagini Docker**
   - Introduzione ai Dockerfile
   - Sintassi e istruzioni principali
   - Best practices nella scrittura di Dockerfile
   - Ottimizzazione delle immagini
   - Creazione di immagini personalizzate
   - Gestione dei tag e versionamento

2. **Persistenza dei dati e volumi**
   - Problema della persistenza nei container
   - Tipi di storage in Docker
   - Volumi Docker
   - Bind mount
   - Condivisione di dati tra container
   - Backup e ripristino dei dati

3. **Networking e distribuzione**
   - Networking in Docker
   - Reti Docker predefinite
   - Creazione di reti personalizzate
   - Comunicazione tra container
   - Esposizione di porte e port mapping
   - Distribuzione di immagini Docker
   - Docker Hub e registry

### Esercizi Pratici
1. Creazione di un'immagine per un'applicazione Node.js semplice
2. Ottimizzazione del Dockerfile
3. Gestione delle dipendenze
4. Multi-stage build per ridurre la dimensione dell'immagine
5. Creazione e gestione di volumi Docker
6. Utilizzo di bind mount
7. Condivisione di dati tra container
8. Backup e ripristino dei dati
9. Configurazione di una rete Docker personalizzata
10. Comunicazione tra container in rete
11. Pubblicazione di un'immagine su Docker Hub
12. Applicazione multi-container

## Esercitazione 3: Docker Avanzato e Applicazioni Distribuite

### Obiettivi Specifici
- Configurare reti Docker avanzate
- Implementare comunicazione tra container
- Gestire configurazioni con variabili d'ambiente
- Ottimizzare immagini Docker
- Introdurre concetti di orchestrazione semplice
- Preparare il terreno per il progetto finale

### Contenuti
1. **Networking avanzato in Docker**
   - Tipologie di reti in Docker
   - Configurazione avanzata delle reti
   - DNS e service discovery
   - Gestione degli indirizzi IP
   - Comunicazione tra container
   - Modelli di comunicazione tra container
   - Utilizzo di nomi DNS per la comunicazione
   - Implementazione di pattern client-server
   - Gestione delle dipendenze tra container

2. **Configurazione e ottimizzazione**
   - Gestione delle configurazioni
   - Utilizzo di variabili d'ambiente
   - File di configurazione e secrets
   - Gestione di configurazioni in ambienti diversi
   - Best practices per la configurazione di applicazioni containerizzate
   - Ottimizzazione delle immagini Docker
   - Tecniche avanzate di multi-stage build
   - Riduzione della dimensione delle immagini
   - Sicurezza delle immagini
   - Strategie di caching

3. **Introduzione all'orchestrazione e preparazione al progetto finale**
   - Concetti base di orchestrazione
   - Panoramica degli strumenti di orchestrazione
   - Docker Compose come strumento di base
   - Casi d'uso dell'orchestrazione
   - Introduzione a Docker Compose
   - Sintassi del file docker-compose.yml
   - Definizione di servizi, reti e volumi
   - Gestione del ciclo di vita delle applicazioni
   - Scaling dei servizi
   - Preparazione al progetto finale

### Esercizi Pratici
1. Creazione di reti personalizzate con configurazioni specifiche
2. Implementazione di comunicazione tra container in diverse reti
3. Risoluzione di problemi di networking comuni
4. Monitoraggio del traffico di rete tra container
5. Implementazione di configurazioni tramite variabili d'ambiente
6. Ottimizzazione di un'immagine Docker esistente
7. Analisi e miglioramento della sicurezza delle immagini
8. Benchmark delle prestazioni prima e dopo l'ottimizzazione
9. Creazione di un file docker-compose.yml per un'applicazione multi-container
10. Gestione del ciclo di vita dell'applicazione con Docker Compose
11. Scaling dei servizi e gestione del carico
12. Debugging di applicazioni multi-container

## Progetto Finale: Infrastruttura Ibrida VM + Container

### Obiettivi
- Progettare e implementare un'infrastruttura ibrida che combina VM e container
- Creare una semplice applicazione web distribuita dimostrativa
- Applicare le conoscenze acquisite durante le tre esercitazioni
- Comprendere i vantaggi e le sfide dell'approccio ibrido

### Architettura dell'Infrastruttura
- VM Alpine Linux come host per i container Docker
- Container Docker per i vari componenti dell'applicazione
- Networking tra VM e container
- Persistenza dei dati

### Componenti dell'Applicazione
- Frontend (Nginx + Vue.js)
- Backend API (Node.js)
- Database (MongoDB)
- Cache (Redis)
- Reverse Proxy (Nginx)

### Tutorial Guidato
1. Configurazione dell'ambiente
2. Esplorazione dell'architettura
3. Avvio dell'infrastruttura
4. Test dell'applicazione
5. Esplorazione avanzata

## Risorse Didattiche

### Materiali di Riferimento
- Documentazione ufficiale di Docker: [https://docs.docker.com/](https://docs.docker.com/)
- Docker Hub: [https://hub.docker.com/](https://hub.docker.com/)
- Documentazione di Alpine Linux: [https://docs.alpinelinux.org/](https://docs.alpinelinux.org/)
- Documentazione di Node.js: [https://nodejs.org/en/docs/](https://nodejs.org/en/docs/)

### Strumenti Necessari
- VM Linux Alpine con accesso root
- Connessione Internet
- Client SSH
- Browser web

## Metodologia Didattica
- Lezioni teoriche interattive
- Esercitazioni pratiche guidate
- Apprendimento basato su progetti
- Discussioni di gruppo
- Troubleshooting collaborativo

## Valutazione
- Partecipazione attiva durante le esercitazioni
- Completamento degli esercizi pratici
- Implementazione del progetto finale
- Comprensione dei concetti teorici
- Capacità di risolvere problemi

## Consigli per gli Studenti
- Sperimentare attivamente con i comandi Docker
- Consultare regolarmente la documentazione ufficiale
- Collaborare con i compagni per risolvere problemi
- Prendere appunti durante le esercitazioni
- Applicare le conoscenze acquisite in progetti personali

## Appendici

### Glossario dei Termini
- **Container**: Unità standard di software che impacchetta il codice e tutte le sue dipendenze
- **Immagine Docker**: Template di sola lettura con istruzioni per creare un container
- **Dockerfile**: File di testo che contiene istruzioni per costruire un'immagine Docker
- **Docker Hub**: Registry pubblico per le immagini Docker
- **Volume**: Meccanismo per la persistenza dei dati nei container
- **Networking**: Sistema di comunicazione tra container
- **Orchestrazione**: Automazione della gestione, coordinamento e organizzazione di container

### Comandi Docker Comuni
- `docker run`: Crea ed esegue un container
- `docker ps`: Elenca i container in esecuzione
- `docker images`: Elenca le immagini disponibili
- `docker build`: Costruisce un'immagine da un Dockerfile
- `docker pull`: Scarica un'immagine da un registry
- `docker push`: Pubblica un'immagine su un registry
- `docker volume`: Gestisce i volumi
- `docker network`: Gestisce le reti
- `docker-compose`: Gestisce applicazioni multi-container

### Troubleshooting Comune
- Problemi di installazione di Docker
- Errori nei Dockerfile
- Problemi di networking
- Persistenza dei dati
- Conflitti di porte
- Problemi di permessi
