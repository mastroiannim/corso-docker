# Esercitazione 2: Creazione e Gestione di Container Docker

## Panoramica dell'Esercitazione
Questa seconda esercitazione di 3 ore è progettata per approfondire la creazione di immagini Docker personalizzate, la gestione della persistenza dei dati e il networking di base. Questa esercitazione si basa sulle conoscenze acquisite nelle sessioni introduttive e nella prima esercitazione su Docker.

## Parte 1: Creazione di Immagini Docker

### Introduzione ai Dockerfile
Un Dockerfile è un file di testo che contiene una serie di istruzioni per costruire un'immagine Docker in modo automatizzato e ripetibile.

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i file di configurazione che abbiamo visto nella Sessione Introduttiva 1? Il Dockerfile è un file di configurazione che definisce come costruire un'immagine Docker.

#### Struttura di Base:
```dockerfile
# Commento
ISTRUZIONE argomenti
```

#### Istruzioni Principali:
- `FROM`: Immagine base da cui partire
- `RUN`: Esegue comandi durante la build
- `COPY`/`ADD`: Copia file nell'immagine
- `WORKDIR`: Imposta la directory di lavoro
- `ENV`: Definisce variabili d'ambiente
- `EXPOSE`: Dichiara le porte su cui il container ascolta
- `CMD`/`ENTRYPOINT`: Definisce il comando da eseguire all'avvio del container

### Sintassi e Istruzioni Principali

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate la sintassi dei comandi bash che abbiamo visto nella Sessione Introduttiva 1? Le istruzioni del Dockerfile seguono una sintassi simile ma più strutturata.

#### FROM:
```dockerfile
FROM <immagine>:<tag>
```
- Definisce l'immagine base
- Deve essere la prima istruzione (eccetto ARG)
- Esempi: `FROM ubuntu:20.04`, `FROM node:14-alpine`

#### RUN:
```dockerfile
RUN <comando>
# oppure
RUN ["eseguibile", "param1", "param2"]
```
- Esegue comandi durante la build
- Crea un nuovo layer nell'immagine
- Esempi: `RUN apt-get update && apt-get install -y nginx`, `RUN ["pip", "install", "flask"]`

#### COPY e ADD:
```dockerfile
COPY <src> <dest>
ADD <src> <dest>
```
- Copia file dal contesto di build all'immagine
- ADD supporta anche URL e decompressione automatica di archivi
- Esempi: `COPY app.js /app/`, `ADD https://example.com/file.tar.gz /tmp/`

#### WORKDIR:
```dockerfile
WORKDIR /path/to/directory
```
- Imposta la directory di lavoro per le istruzioni successive
- Crea la directory se non esiste
- Esempio: `WORKDIR /app`

#### ENV:
```dockerfile
ENV <chiave>=<valore>
```
- Definisce variabili d'ambiente
- Disponibili durante la build e nel container in esecuzione
- Esempio: `ENV NODE_ENV=production`

#### EXPOSE:
```dockerfile
EXPOSE <porta> [<porta>...]
```
- Dichiara le porte su cui il container ascolta
- Solo documentativo, non pubblica effettivamente le porte
- Esempio: `EXPOSE 80 443`

#### CMD e ENTRYPOINT:
```dockerfile
CMD ["eseguibile", "param1", "param2"]
ENTRYPOINT ["eseguibile", "param1", "param2"]
```
- Definiscono il comando da eseguire all'avvio del container
- CMD può essere sovrascritto dalla riga di comando
- ENTRYPOINT è più difficile da sovrascrivere
- Esempio: `CMD ["node", "app.js"]`, `ENTRYPOINT ["nginx", "-g", "daemon off;"]`

### Best Practices nella Scrittura di Dockerfile

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate i concetti di ottimizzazione delle applicazioni che abbiamo visto nella Sessione Introduttiva 2? Applicheremo principi simili per ottimizzare i Dockerfile.

#### Immagini Base:
- Usare immagini ufficiali quando possibile
- Preferire immagini Alpine per dimensioni ridotte
- Specificare sempre un tag preciso, non `latest`

#### Ottimizzazione dei Layer:
- Combinare comandi RUN correlati con `&&` e `\`
- Rimuovere file temporanei nello stesso layer in cui sono stati creati
- Ordinare le istruzioni dalla meno alla più frequentemente modificata

#### Sicurezza:
- Evitare di esporre informazioni sensibili
- Non eseguire container come root quando possibile
- Usare `--no-install-recommends` con apt-get
- Scansionare le immagini per vulnerabilità

#### Esempio di Dockerfile Ottimizzato:
```dockerfile
FROM node:14-alpine

# Impostare la directory di lavoro
WORKDIR /app

# Copiare solo i file necessari per npm install
COPY package*.json ./

# Installare le dipendenze in un unico layer
RUN npm ci --only=production && \
    npm cache clean --force

# Copiare il resto del codice
COPY . .

# Utente non-root per la sicurezza
USER node

# Esporre la porta
EXPOSE 3000

# Comando di avvio
CMD ["node", "app.js"]
```

### Ottimizzazione delle Immagini

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato dell'efficienza delle risorse. Qui vedremo come ottimizzare le immagini Docker per ridurre dimensioni e migliorare le prestazioni.

#### Riduzione della Dimensione:
- Usare immagini base leggere (Alpine, slim)
- Rimuovere file temporanei e cache
- Utilizzare `.dockerignore` per escludere file non necessari
- Implementare multi-stage builds

#### Multi-stage Builds:
```dockerfile
# Stage di build
FROM node:14 AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build

# Stage finale
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Vantaggi:
- Immagini finali più piccole
- Separazione tra ambiente di build e runtime
- Riduzione della superficie di attacco
- Migliore organizzazione del Dockerfile

### Creazione di Immagini Personalizzate

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i concetti di file e directory che abbiamo visto nella Sessione Introduttiva 1? Ora vedremo come organizzare i file per creare un'immagine Docker.

#### Preparazione del Contesto di Build:
1. Creare una directory per il progetto:
   ```bash
   mkdir -p docker-project
   cd docker-project
   ```

2. Creare un file `.dockerignore`:
   ```
   node_modules
   npm-debug.log
   .git
   .gitignore
   ```

3. Creare un Dockerfile:
   ```dockerfile
   FROM node:14-alpine
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 3000
   CMD ["node", "index.js"]
   ```

4. Creare i file dell'applicazione (es. `index.js`):
   ```javascript
   const express = require('express');
   const app = express();
   const port = 3000;

   app.get('/', (req, res) => {
     res.send('Hello from custom Docker image!');
   });

   app.listen(port, () => {
     console.log(`App listening at http://localhost:${port}`);
   });
   ```

#### Costruzione dell'Immagine:
```bash
docker build -t my-custom-app:1.0 .
```

#### Opzioni Comuni di Build:
- `-t <nome>:<tag>`: Assegna nome e tag all'immagine
- `--no-cache`: Ignora la cache durante la build
- `--build-arg <var>=<valore>`: Passa variabili di build
- `--file <file>`: Specifica un Dockerfile alternativo

#### Verifica dell'Immagine:
```bash
docker images
docker history my-custom-app:1.0
docker inspect my-custom-app:1.0
```

## Parte 2: Persistenza dei Dati e Volumi

### Problema della Persistenza nei Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate il concetto di persistenza dei dati che abbiamo visto nella Sessione Introduttiva 1? I container hanno un filesystem effimero, che scompare quando il container viene eliminato.

#### Caratteristiche del Filesystem dei Container:
- **Effimero:** I dati scompaiono quando il container viene eliminato
- **Layer scrivibile:** Ogni container ha un proprio layer scrivibile
- **Union filesystem:** Combina il layer scrivibile con i layer di sola lettura dell'immagine
- **Isolamento:** Modifiche al filesystem non influenzano altri container o l'host

#### Casi d'Uso per la Persistenza:
- Database
- File di configurazione
- Log
- Contenuti generati dagli utenti
- Certificati SSL
- Codice sorgente in sviluppo

### Tipi di Storage in Docker

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Nella Sessione Introduttiva 1 abbiamo visto diversi tipi di storage. Docker offre diverse soluzioni per la persistenza dei dati, ciascuna con caratteristiche specifiche.

#### Volumi:
- Gestiti completamente da Docker
- Memorizzati in un'area dedicata del filesystem dell'host
- Indipendenti dal ciclo di vita dei container
- Migliore opzione per la persistenza dei dati

#### Bind Mount:
- Mappano una directory dell'host nel container
- Percorso specificato dall'utente
- Utili per lo sviluppo e quando è necessario accedere ai file dall'host
- Dipendono dalla struttura delle directory dell'host

#### tmpfs Mount:
- Memorizzati solo nella memoria dell'host
- Non persistenti
- Utili per dati temporanei o sensibili
- Più veloci ma limitati dalla RAM disponibile

### Volumi Docker

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i concetti di gestione dei file che abbiamo visto nella Sessione Introduttiva 1? I volumi Docker sono un modo per gestire i dati persistenti in modo indipendente dai container.

#### Creazione e Gestione:
```bash
# Creazione di un volume
docker volume create my-volume

# Elenco dei volumi
docker volume ls

# Dettagli di un volume
docker volume inspect my-volume

# Eliminazione di un volume
docker volume rm my-volume

# Pulizia dei volumi inutilizzati
docker volume prune
```

#### Utilizzo con Container:
```bash
# Montare un volume in un container
docker run -d --name db -v my-volume:/var/lib/mysql mysql:5.7

# Creare e montare un volume in un unico comando
docker run -d --name web -v web-data:/app/data nginx
```

#### Vantaggi:
- Gestione semplificata
- Backup e ripristino facilitati
- Condivisione tra container
- Indipendenza dal filesystem dell'host
- Supporto per driver di storage

### Bind Mount

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i percorsi assoluti e relativi che abbiamo visto nella Sessione Introduttiva 1? I bind mount utilizzano percorsi del filesystem host per montare directory nei container.

#### Sintassi:
```bash
# Sintassi completa
docker run -v /path/on/host:/path/in/container[:options] ...

# Sintassi più recente
docker run --mount type=bind,source=/path/on/host,target=/path/in/container ...
```

#### Esempi:
```bash
# Montare una directory in sola lettura
docker run -v $(pwd)/config:/etc/app/config:ro my-app

# Montare per lo sviluppo
docker run -v $(pwd):/app node:14 npm start

# Con la sintassi --mount
docker run --mount type=bind,source="$(pwd)",target=/app node:14 npm start
```

#### Casi d'Uso:
- Sviluppo di applicazioni
- Configurazione da file esterni
- Condivisione di dati con l'host
- Log accessibili dall'host

### Condivisione di Dati tra Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate il concetto di condivisione delle risorse che abbiamo visto nella Sessione Introduttiva 2? Docker permette di condividere dati tra container in diversi modi.

#### Utilizzo di Volumi Condivisi:
```bash
# Creare un volume condiviso
docker volume create shared-data

# Primo container che utilizza il volume
docker run -d --name writer -v shared-data:/data alpine sh -c "while true; do date >> /data/dates.txt; sleep 10; done"

# Secondo container che legge dal volume
docker run -it --name reader -v shared-data:/data alpine sh -c "tail -f /data/dates.txt"
```

#### Volume da un Container:
```bash
# Creare un container con un volume
docker run -d --name source -v /data alpine touch /data/file.txt

# Montare i volumi da un altro container
docker run --volumes-from source alpine ls -la /data
```

#### Considerazioni:
- Permessi dei file
- Concorrenza e lock
- Coerenza dei dati
- Backup e ripristino

### Backup e Ripristino dei Dati

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Nella Sessione Introduttiva 1 abbiamo parlato dell'importanza del backup dei dati. Vediamo come applicare questo concetto ai volumi Docker.

#### Backup di un Volume:
```bash
# Creare un container temporaneo che monta il volume e lo archivia
docker run --rm -v my-volume:/source -v $(pwd):/backup alpine tar -czf /backup/my-volume-backup.tar.gz -C /source .
```

#### Ripristino di un Volume:
```bash
# Creare un nuovo volume
docker volume create my-new-volume

# Ripristinare i dati dal backup
docker run --rm -v my-new-volume:/target -v $(pwd):/backup alpine sh -c "tar -xzf /backup/my-volume-backup.tar.gz -C /target"
```

#### Strategie di Backup:
- Backup incrementali
- Automazione con script o cron
- Utilizzo di strumenti specializzati
- Backup remoti

## Parte 3: Networking e Distribuzione

### Networking in Docker

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate i concetti di rete che abbiamo visto nella Sessione Introduttiva 2? Docker implementa un sistema di networking che permette ai container di comunicare tra loro e con l'esterno.

#### Concetti Fondamentali:
- **Container Network Model (CNM):** Architettura di networking di Docker
- **Network Drivers:** Implementazioni specifiche del CNM
- **Endpoint:** Connessione di un container a una rete
- **Sandbox:** Isolamento della configurazione di rete di un container
- **Network:** Gruppo di endpoint che possono comunicare direttamente

#### Comandi Base:
```bash
# Elenco delle reti
docker network ls

# Creare una rete
docker network create my-network

# Ispezionare una rete
docker network inspect my-network

# Eliminare una rete
docker network rm my-network
```

### Reti Docker Predefinite

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato di diversi tipi di reti. Docker fornisce alcune reti predefinite con caratteristiche specifiche.

#### Bridge:
- Rete predefinita per i container
- Isolata dall'host ma con accesso tramite port mapping
- I container sulla stessa rete bridge possono comunicare tra loro
- Esempio: `docker run --network bridge nginx`

#### Host:
- Condivide lo stack di rete dell'host
- Nessun isolamento di rete
- Prestazioni migliori ma meno sicurezza
- Esempio: `docker run --network host nginx`

#### None:
- Nessuna connettività di rete
- Container completamente isolato
- Utile per elaborazioni batch o dove la rete non è necessaria
- Esempio: `docker run --network none alpine`

#### Overlay:
- Per comunicazione tra container su host diversi
- Utilizzata principalmente con Docker Swarm
- Esempio: `docker network create --driver overlay my-overlay-network`

### Creazione di Reti Personalizzate

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate i concetti di subnet e indirizzi IP che abbiamo visto nella Sessione Introduttiva 2? Possiamo applicarli per creare reti Docker personalizzate.

#### Creazione di una Rete Bridge Personalizzata:
```bash
docker network create --driver bridge \
  --subnet=172.18.0.0/16 \
  --gateway=172.18.0.1 \
  my-custom-network
```

#### Opzioni Comuni:
- `--driver`: Tipo di rete (bridge, overlay, macvlan, ...)
- `--subnet`: Range di indirizzi IP
- `--gateway`: Indirizzo IP del gateway
- `--ip-range`: Sottoinsieme di indirizzi per l'allocazione
- `--internal`: Rete senza accesso esterno

#### Collegare Container a una Rete:
```bash
# All'avvio
docker run --network my-custom-network --name container1 nginx

# Container esistente
docker network connect my-custom-network container2
```

### Comunicazione tra Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate il modello client-server che abbiamo visto nella Sessione Introduttiva 2? I container possono comunicare tra loro seguendo questo modello.

#### Risoluzione dei Nomi:
- Container sulla stessa rete possono comunicare usando i nomi
- Docker fornisce un DNS interno per la risoluzione dei nomi
- Esempio: `container1` può raggiungere `container2` usando il suo nome

#### Esempio Pratico:
```bash
# Creare
(Content truncated due to size limit. Use line ranges to read in chunks)