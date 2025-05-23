# Esercitazione 1: Introduzione a Docker e Concetti Base

## Panoramica dell'Esercitazione
Questa prima esercitazione di 3 ore è progettata per introdurre i concetti fondamentali di Docker e della containerizzazione, partendo dalle basi teoriche fino all'esecuzione dei primi container. Questa esercitazione si basa sulle conoscenze acquisite nelle sessioni introduttive sui sistemi operativi, filesystem, shell, reti e servizi.

## Parte 1: Teoria della Containerizzazione

### Cos'è la Containerizzazione?
La containerizzazione è una tecnologia di virtualizzazione a livello di sistema operativo che permette di impacchettare un'applicazione e le sue dipendenze in un'unità standardizzata chiamata container.

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate la differenza tra macchine virtuali e container che abbiamo discusso nella Sessione Introduttiva 2? I container condividono il kernel del sistema operativo host, mentre le VM virtualizzano l'hardware completo.

#### Caratteristiche Principali:
- **Isolamento:** I container sono isolati l'uno dall'altro e dalla macchina host
- **Portabilità:** Funzionano allo stesso modo in qualsiasi ambiente
- **Leggerezza:** Condividono il kernel del sistema operativo host
- **Velocità:** Si avviano in pochi secondi
- **Efficienza:** Utilizzano meno risorse rispetto alle macchine virtuali

### Storia ed Evoluzione dei Container
- **Origini:** Concetti di isolamento presenti in Unix (chroot) dagli anni '70
- **LXC (Linux Containers):** Primo sistema di containerizzazione completo
- **Docker:** Lanciato nel 2013, ha reso i container accessibili e facili da usare
- **Standardizzazione:** Open Container Initiative (OCI) per standardizzare il formato

### Differenze tra VM e Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo visto come le VM virtualizzano l'hardware completo. Ora vedremo come questo si confronta con l'approccio dei container.

#### Macchine Virtuali:
- Virtualizzano l'hardware completo
- Ogni VM include un intero sistema operativo
- Maggiore isolamento
- Avvio più lento (minuti)
- Dimensioni maggiori (GB)
- Overhead significativo

#### Container:
- Virtualizzano solo il sistema operativo
- Condividono il kernel dell'host
- Isolamento più leggero
- Avvio rapido (secondi)
- Dimensioni ridotte (MB)
- Overhead minimo

### Vantaggi e Svantaggi dei Container

#### Vantaggi:
- **Efficienza:** Utilizzo ottimale delle risorse hardware
- **Velocità:** Avvio e arresto rapidi
- **Consistenza:** Ambiente identico in sviluppo e produzione
- **Scalabilità:** Facile distribuzione e replica
- **Isolamento:** Applicazioni separate senza interferenze
- **Versioning:** Controllo delle versioni delle immagini

#### Svantaggi:
- **Sicurezza:** Isolamento meno robusto rispetto alle VM
- **Limitazioni:** Tutti i container condividono lo stesso kernel
- **Complessità:** Curva di apprendimento per orchestrazione avanzata
- **Persistenza:** Gestione dei dati persistenti più complessa

## Parte 2: Architettura di Docker

### Componenti Principali di Docker

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate il concetto di client-server che abbiamo visto nella Sessione Introduttiva 1? Docker utilizza proprio questa architettura per il suo funzionamento.

#### Docker Engine:
- **Docker Daemon (dockerd):** Servizio in background che gestisce container
- **REST API:** Interfaccia per comunicare con il daemon
- **Docker CLI:** Interfaccia a riga di comando per interagire con l'API

#### Docker Registry:
- Repository per le immagini Docker
- Docker Hub: registry pubblico ufficiale
- Possibilità di registry privati

#### Docker Objects:
- **Immagini:** Template di sola lettura per creare container
- **Container:** Istanze in esecuzione di un'immagine
- **Volumi:** Meccanismo per la persistenza dei dati
- **Network:** Sistema per la comunicazione tra container

### Concetto di Immagine e Container

#### Immagini Docker:
- Template di sola lettura
- Composte da layer sovrapposti
- Ogni layer rappresenta un'istruzione nel Dockerfile
- Sistema di caching per ottimizzare build e download
- Immutabili: una volta create non cambiano

#### Container Docker:
- Istanza in esecuzione di un'immagine
- Aggiunge un layer scrivibile sopra l'immagine
- Ha un ciclo di vita (creazione, esecuzione, pausa, arresto, eliminazione)
- Isolato ma può comunicare con altri container e con l'host

### Ciclo di Vita di un Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate il concetto di processo che abbiamo visto nella Sessione Introduttiva 1? I container hanno un ciclo di vita simile ai processi, ma con caratteristiche aggiuntive.

#### Stati di un Container:
- **Created:** Container creato ma non avviato
- **Running:** Container in esecuzione
- **Paused:** Esecuzione sospesa
- **Stopped:** Container fermato
- **Deleted:** Container eliminato

#### Comandi del Ciclo di Vita:
- `docker create`: Crea un container
- `docker start`: Avvia un container
- `docker pause`: Sospende un container
- `docker stop`: Ferma un container
- `docker rm`: Elimina un container

### Docker Hub e Repository di Immagini

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato dei gestori di pacchetti. Docker Hub funziona in modo simile, ma per le immagini Docker.

#### Docker Hub:
- Registry pubblico ufficiale per le immagini Docker
- Migliaia di immagini pronte all'uso
- Immagini ufficiali e verificate
- Possibilità di pubblicare immagini personalizzate

#### Altre Opzioni:
- Registry privati (Docker Trusted Registry)
- Registry cloud (Amazon ECR, Google Container Registry, Azure Container Registry)
- Registry self-hosted (Harbor, Nexus)

## Parte 3: Installazione e Configurazione

### Installazione di Docker su Alpine Linux

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i comandi di gestione pacchetti che abbiamo visto nella Sessione Introduttiva 1? Useremo `apk`, il gestore pacchetti di Alpine Linux.

#### Prerequisiti:
- VM Alpine Linux con accesso root
- Connessione Internet
- Conoscenza base dei comandi Linux

#### Passi per l'Installazione:
1. Aggiornare i repository:
   ```bash
   apk update
   ```

2. Installare Docker:
   ```bash
   apk add docker
   ```

3. Abilitare e avviare il servizio Docker:
   ```bash
   rc-update add docker boot
   service docker start
   ```

4. Verificare l'installazione:
   ```bash
   docker --version
   docker info
   ```

### Configurazione dell'Ambiente Docker

#### Configurazione di Base:
- Aggiungere l'utente al gruppo docker per eseguire comandi senza sudo:
  ```bash
  addgroup <username> docker
  ```

- Configurare il daemon Docker (opzionale):
  ```bash
  mkdir -p /etc/docker
  echo '{"storage-driver": "overlay2"}' > /etc/docker/daemon.json
  ```

- Riavviare il servizio Docker:
  ```bash
  service docker restart
  ```

#### Configurazione Avanzata:
- Impostazione di registry alternativi
- Configurazione di rete
- Limiti di risorse
- Logging

### Test di Funzionamento

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate i test di connettività che abbiamo fatto nella Sessione Introduttiva 2? Ora faremo un test simile per verificare che Docker funzioni correttamente.

#### Esecuzione del Container "Hello World":
```bash
docker run hello-world
```

Questo comando:
1. Cerca l'immagine "hello-world" localmente
2. Se non la trova, la scarica da Docker Hub
3. Crea un container dall'immagine
4. Esegue il container, che stampa un messaggio e termina

#### Verifica dei Componenti:
- Docker daemon in esecuzione
- Connettività a Docker Hub
- Creazione ed esecuzione di container
- Gestione delle immagini

### Risoluzione dei Problemi Comuni

#### Problemi di Installazione:
- Dipendenze mancanti
- Versioni incompatibili
- Permessi insufficienti

#### Problemi di Connettività:
- Firewall che blocca le connessioni
- Proxy non configurato
- DNS non funzionante

#### Problemi di Avvio:
- Conflitti di porte
- Spazio su disco insufficiente
- Problemi di driver di storage

#### Comandi Utili per il Troubleshooting:
- `docker info`: Informazioni sul sistema Docker
- `docker system df`: Utilizzo dello spazio
- `docker logs`: Log di un container
- `journalctl -u docker`: Log del servizio Docker

## Parte 4: Primi Passi con Docker

### Comandi Docker di Base

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate la struttura dei comandi bash che abbiamo visto nella Sessione Introduttiva 1? I comandi Docker seguono una struttura simile: `docker <comando> [opzioni] [argomenti]`.

#### Gestione delle Immagini:
- `docker images`: Elenco delle immagini disponibili
- `docker pull <immagine>`: Scarica un'immagine
- `docker rmi <immagine>`: Rimuove un'immagine
- `docker build`: Costruisce un'immagine da un Dockerfile

#### Gestione dei Container:
- `docker ps`: Elenco dei container in esecuzione
- `docker ps -a`: Elenco di tutti i container
- `docker run <immagine>`: Crea ed esegue un container
- `docker start/stop/restart <container>`: Gestisce lo stato di un container
- `docker rm <container>`: Rimuove un container

#### Informazioni e Monitoraggio:
- `docker inspect <oggetto>`: Dettagli su un oggetto Docker
- `docker logs <container>`: Visualizza i log di un container
- `docker stats <container>`: Statistiche di utilizzo risorse
- `docker top <container>`: Processi in esecuzione in un container

### Esecuzione di Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate il concetto di porte di rete che abbiamo visto nella Sessione Introduttiva 2? Useremo il port mapping per esporre servizi dai container.

#### Modalità di Esecuzione:
- **Interattiva:** `docker run -it <immagine> <comando>`
- **Background (detached):** `docker run -d <immagine>`
- **Con nome specifico:** `docker run --name <nome> <immagine>`

#### Opzioni Comuni:
- `-p <host_port>:<container_port>`: Mapping delle porte
- `-v <host_path>:<container_path>`: Mounting di volumi
- `-e <variabile>=<valore>`: Variabili d'ambiente
- `--rm`: Rimuove il container dopo l'arresto
- `--network <rete>`: Specifica la rete da utilizzare

#### Esempi:
- Eseguire un container Nginx e mappare la porta 80:
  ```bash
  docker run -d -p 8080:80 --name webserver nginx
  ```

- Eseguire un container interattivo con Alpine Linux:
  ```bash
  docker run -it --name alpine_test alpine /bin/sh
  ```

### Gestione del Ciclo di Vita dei Container

#### Comandi di Gestione:
- `docker start <container>`: Avvia un container fermato
- `docker stop <container>`: Ferma un container in esecuzione
- `docker restart <container>`: Riavvia un container
- `docker pause/unpause <container>`: Sospende/riprende l'esecuzione
- `docker kill <container>`: Termina forzatamente un container

#### Monitoraggio:
- `docker logs <container>`: Visualizza i log
- `docker logs -f <container>`: Segue i log in tempo reale
- `docker stats <container>`: Monitora l'utilizzo delle risorse
- `docker inspect <container>`: Visualizza informazioni dettagliate

#### Pulizia:
- `docker rm <container>`: Rimuove un container fermato
- `docker rm -f <container>`: Rimuove forzatamente un container
- `docker container prune`: Rimuove tutti i container fermati
- `docker system prune`: Pulizia completa di risorse inutilizzate

### Esecuzione di un'Applicazione Node.js in Container

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate il concetto di applicazioni e servizi che abbiamo visto nella Sessione Introduttiva 2? Ora containerizzeremo un'applicazione Node.js.

#### Preparazione dell'Applicazione:
1. Creare una directory per l'applicazione:
   ```bash
   mkdir -p nodejs-app
   cd nodejs-app
   ```

2. Creare un file `package.json`:
   ```json
   {
     "name": "docker-nodejs-example",
     "version": "1.0.0",
     "description": "Esempio di applicazione Node.js in Docker",
     "main": "app.js",
     "scripts": {
       "start": "node app.js"
     },
     "dependencies": {
       "express": "^4.17.1"
     }
   }
   ```

3. Creare un file `app.js`:
   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
     res.send('Hello from Docker!');
   });

   app.listen(PORT, () => {
     console.log(`Server running on port ${PORT}`);
   });
   ```

4. Creare un `Dockerfile`:
   ```dockerfile
   FROM node:14-alpine
   WORKDIR /app
   COPY package.json .
   RUN npm install
   COPY app.js .
   EXPOSE 3000
   CMD ["npm", "start"]
   ```

#### Costruzione ed Esecuzione:
1. Costruire l'immagine:
   ```bash
   docker build -t nodejs-app .
   ```

2. Eseguire il container:
   ```bash
   docker run -d -p 3000:3000 --name nodejs-example nodejs-app
   ```

3. Verificare il funzionamento:
   ```bash
   curl http://localhost:3000
   ```

## Esercizi Pratici

### Esercizio 1: Hello World Docker
**Obiettivo:** Familiarizzare con i comandi base di Docker

**Istruzioni:**
1. Eseguire il container "hello-world":
   ```bash
   docker run hello-world
   ```
2. Elencare tutti i container:
   ```bash
   docker ps -a
   ```
3. Elencare le immagini disponibili:
   ```bash
   docker images
   ```
4. Rimuovere il container "hello-world":
   ```bash
   docker rm <container_id>
   ```

### Esercizio 2: Container Interattivo
**Obiettivo:** Utilizzare un container in modalità interattiva

**Istruzioni:**
1. Eseguire un container Alpine in modalità interattiva:
   ```bash
   docker run -it --name alpine_interactive alpine /bin/sh
   ```
2. All'interno del container, eseguire alcuni comandi:
   ```bash
   ls -la
   echo "Hello from Alpine container" > test.txt
   cat test.txt
   ```
3. Uscire dal container:
   ```bash
   exit
   ```
4. Riavviare e ricollegarsi al container:
   ```bash
   docker start alpine_interactive
   docker attach alpine_interactive
   ```
5. Verificare che il file creato sia ancora presente:
   ```bash
   cat test.txt
   ```

### Esercizio 3: Container in Background
**Obiettivo:** Eseguire e gestire container in modalità detached

**Istruzioni:**
1. Eseguire un container Nginx in background:
   ```bash
   docker run -d -p 8080:80 --name webserver nginx
   ```
2. Verificare che il container sia in esecuzione:
   ```bash
   docker ps
   ```
3. Accedere alla pagina web servita da Nginx:
   ```bash
   curl http://localhost:8080
   ```
4. Visualizzare i log del container:
   ```bash
   docker logs webserver
   ```
5. Fermare e rimuovere il container:
   ```bash
   docker stop webserver
   docker rm webserver
   ```

### Esercizio 4: Gestione del Ciclo di Vita dei Container
**Obiettivo:** Praticare i comandi di gestione del ciclo di vita

**Istruzioni:**
1. Creare un container senza avviarlo:
   ```bash
   docker create --name lifecycle_test alpine ping localhost
   ```
2. Verificare lo stato del container:
   ```bash
   docker ps -a
   ```
3. Avviare il container:
   ```bash
   docker start lifecycle_test
   ```
4. Verificare che sia in esecuzione:
   ```bash
   docker ps
   ```
5. Mettere in pausa il container:
   ```bash
   docker pause lifecycle_test
   ```
6. Riprendere l'esecuzione:
   ```bash
   docker unpause lifecycle_test
   ```
7. Fermare il container:
   ```bash
   docker stop lifecycle_test
   ```
8. Rimuovere il container:
   ```bash
   docker rm lifecycle_test
   ```

### Esercizio 5: Esplorazione di Docker Hub
**Obiettivo:** Familiarizzare con Docker Hub e le immagini pubbliche

**Istruzioni:**
1. Cercare immagini di PostgreSQL su Docker Hub:
   ```bash
   docker search postgres
   ```
2. Scaricare l'immagine ufficiale di PostgreSQL:
   ```bash
   docker pull postgres:13-alpine
   ```
3. Visualizzare i dettagli dell'immagine:
   ```bash
   docker inspect postgres:1
(Content truncated due to size limit. Use line ranges to read in chunks)