# Esercitazione 1: Introduzione a Docker e Concetti Base

## Parte 1: Teoria - Introduzione alla containerizzazione

### Cos'è la containerizzazione

La containerizzazione è una tecnologia di virtualizzazione a livello di sistema operativo che permette di impacchettare un'applicazione insieme alle sue dipendenze in un'unità standardizzata chiamata container. A differenza delle macchine virtuali tradizionali, i container condividono il kernel del sistema operativo host, risultando più leggeri ed efficienti.

I container sono ambienti isolati che contengono tutto ciò che serve per eseguire un'applicazione: codice, runtime, librerie, variabili d'ambiente e file di configurazione. Questo approccio garantisce che l'applicazione funzioni allo stesso modo indipendentemente dall'ambiente in cui viene eseguita, risolvendo il classico problema "funziona sul mio computer".

### Storia ed evoluzione dei container

La storia dei container ha radici profonde nei sistemi Unix:

- **1979**: La funzionalità chroot di Unix introduce un primo concetto di isolamento del filesystem
- **2000**: FreeBSD introduce le "jail", un'evoluzione di chroot con maggiore isolamento
- **2005**: Solaris Containers (Zones) offre un ambiente virtualizzato completo
- **2008**: LXC (Linux Containers) introduce i namespace e i cgroups per l'isolamento
- **2013**: Docker viene rilasciato, semplificando drasticamente l'utilizzo dei container
- **2015**: Nascita della Open Container Initiative (OCI) per standardizzare i formati dei container
- **Oggi**: Ecosistema maturo con strumenti di orchestrazione come Kubernetes

Docker ha rivoluzionato il settore rendendo i container accessibili a tutti gli sviluppatori, standardizzando il formato e fornendo strumenti semplici per la creazione, distribuzione ed esecuzione.

### Differenze tra VM e container

| Caratteristica | Macchine Virtuali | Container |
|----------------|-------------------|-----------|
| Virtualizzazione | Hardware (hypervisor) | Sistema operativo |
| Sistema operativo | Ogni VM ha il proprio OS completo | Condividono il kernel dell'host |
| Dimensione | Gigabyte | Megabyte |
| Avvio | Minuti | Secondi o millisecondi |
| Isolamento | Completo (più sicuro) | A livello di processo (meno isolato) |
| Overhead | Elevato | Basso |
| Densità | Bassa (poche VM per host) | Alta (molti container per host) |
| Portabilità | Limitata | Elevata |

**Riflessione per gli studenti**: Considerando la vostra esperienza con le VM Alpine, quali vantaggi immediati vedete nell'utilizzo dei container? Quali potrebbero essere gli svantaggi?

### Vantaggi e svantaggi dei container

**Vantaggi:**
- **Leggerezza**: Consumano meno risorse rispetto alle VM
- **Velocità**: Si avviano in pochi secondi
- **Portabilità**: Funzionano allo stesso modo in qualsiasi ambiente
- **Consistenza**: Eliminano le differenze tra ambienti di sviluppo, test e produzione
- **Scalabilità**: Facili da replicare per gestire carichi variabili
- **Isolamento**: Le applicazioni non interferiscono tra loro
- **Efficienza delle risorse**: Permettono una maggiore densità di applicazioni per server

**Svantaggi:**
- **Sicurezza**: Isolamento meno robusto rispetto alle VM (condivisione del kernel)
- **Limitazioni del sistema operativo**: Tutti i container su un host devono utilizzare lo stesso tipo di kernel
- **Persistenza dei dati**: I container sono effimeri per natura, richiedono soluzioni specifiche per i dati persistenti
- **Complessità di gestione**: In ambienti di produzione complessi, richiedono strumenti di orchestrazione

## Architettura di Docker

### Componenti principali

**Docker Engine**:
È il cuore della piattaforma Docker, un'applicazione client-server composta da:
- **dockerd**: Il demone che gestisce oggetti Docker come immagini, container, reti e volumi
- **REST API**: Interfaccia che programmi e comandi possono usare per comunicare con il demone
- **CLI**: Interfaccia a riga di comando per interagire con Docker

**Docker CLI**:
L'interfaccia a riga di comando che permette agli utenti di interagire con Docker attraverso comandi come `docker run`, `docker build`, ecc.

**Docker Registry**:
Un repository che memorizza le immagini Docker. Docker Hub è il registry pubblico ufficiale, ma è possibile utilizzare registry privati.

### Concetto di immagine e container

**Immagine Docker**:
- Template di sola lettura con istruzioni per creare un container
- Composta da layer sovrapposti (filesystem a strati)
- Ogni layer rappresenta un'istruzione nel Dockerfile
- Immutabile: una volta creata non può essere modificata
- Può essere basata su altre immagini (ereditarietà)

**Container Docker**:
- Istanza eseguibile di un'immagine
- Aggiunge un layer scrivibile sopra l'immagine di base
- Ha un ciclo di vita definito (creazione, esecuzione, pausa, stop, eliminazione)
- Isolato ma condivide risorse con l'host
- Può essere connesso a reti e avere volumi collegati

**Analogia per gli studenti**: Se l'immagine è come una classe in programmazione orientata agli oggetti, il container è come un'istanza di quella classe.

### Ciclo di vita di un container

1. **Creazione**: `docker create` - Prepara un container ma non lo avvia
2. **Avvio**: `docker start` - Avvia un container creato
3. **Esecuzione**: `docker run` - Combina creazione e avvio
4. **Pausa/Ripresa**: `docker pause`/`docker unpause` - Sospende/riprende i processi
5. **Stop**: `docker stop` - Invia SIGTERM e poi SIGKILL se necessario
6. **Kill**: `docker kill` - Invia direttamente SIGKILL
7. **Riavvio**: `docker restart` - Ferma e riavvia un container
8. **Eliminazione**: `docker rm` - Rimuove un container fermato

**Nota per gli studenti**: I container sono progettati per essere effimeri. Quando un container viene eliminato, tutte le modifiche non salvate in un volume vengono perse.

### Docker Hub e repository di immagini

**Docker Hub**:
- Registry pubblico ufficiale di Docker
- Contiene migliaia di immagini pronte all'uso
- Offre immagini ufficiali mantenute da Docker e dai fornitori di software
- Permette di pubblicare e condividere immagini personalizzate
- Supporta repository privati (con account)

**Altri registry**:
- Amazon Elastic Container Registry (ECR)
- Google Container Registry (GCR)
- Azure Container Registry
- Registry privati self-hosted

**Best practices per l'utilizzo delle immagini**:
- Preferire immagini ufficiali o verificate
- Utilizzare tag specifici invece di `latest`
- Verificare la provenienza delle immagini
- Scansionare le immagini per vulnerabilità
- Mantenere le immagini aggiornate

## Parte 2: Installazione e configurazione

### Installazione di Docker su Alpine Linux

**Requisiti di sistema**:
- Alpine Linux 3.13 o superiore
- Kernel Linux 3.10 o superiore
- Accesso root o sudo
- Connessione Internet

**Comandi di installazione**:

```bash
# Aggiornare i repository
apk update

# Installare Docker
apk add docker

# Abilitare e avviare il servizio Docker
rc-update add docker boot
service docker start

# Verificare che Docker sia in esecuzione
service docker status
```

**Verifica dell'installazione**:

```bash
# Verificare la versione di Docker
docker --version

# Verificare che Docker funzioni correttamente
docker run hello-world
```

**Possibili problemi e soluzioni**:
- Se il comando `docker` richiede privilegi root, aggiungere l'utente al gruppo docker:
  ```bash
  addgroup <username> docker
  ```
- Se il servizio non si avvia, verificare i log:
  ```bash
  cat /var/log/docker.log
  ```
- Se Alpine è in esecuzione in una VM, assicurarsi che la virtualizzazione nidificata sia abilitata

### Configurazione dell'ambiente Docker

**Gestione del servizio Docker**:
```bash
# Avviare il servizio
service docker start

# Fermare il servizio
service docker stop

# Riavviare il servizio
service docker restart

# Verificare lo stato
service docker status
```

**Configurazione dei permessi**:
```bash
# Creare il gruppo docker se non esiste
addgroup docker

# Aggiungere l'utente al gruppo docker
addgroup <username> docker

# Applicare le modifiche (richiede logout/login)
newgrp docker
```

**File di configurazione**:
- `/etc/docker/daemon.json`: Configurazione del demone Docker
- Esempio di configurazione per modificare la directory dei dati:
  ```json
  {
    "data-root": "/path/to/docker/data",
    "storage-driver": "overlay2"
  }
  ```

**Test di funzionamento**:
```bash
# Eseguire un container di test
docker run --rm alpine echo "Docker funziona correttamente!"
```

## Parte 3: Primi passi con Docker

### Comandi Docker di base

**Informazioni sul sistema**:
```bash
# Visualizzare la versione di Docker
docker version

# Visualizzare informazioni dettagliate sul sistema Docker
docker info

# Visualizzare l'utilizzo del disco
docker system df
```

**Gestione delle immagini**:
```bash
# Elencare le immagini disponibili localmente
docker images

# Scaricare un'immagine dal registry
docker pull alpine:latest

# Cercare immagini su Docker Hub
docker search nginx

# Rimuovere un'immagine
docker rmi alpine:latest
```

**Gestione dei container**:
```bash
# Creare ed eseguire un container
docker run alpine echo "Hello, Docker!"

# Eseguire un container in modalità interattiva
docker run -it alpine sh

# Eseguire un container in background (demone)
docker run -d nginx

# Elencare i container in esecuzione
docker ps

# Elencare tutti i container (anche quelli fermati)
docker ps -a

# Fermare un container
docker stop <container_id>

# Avviare un container fermato
docker start <container_id>

# Rimuovere un container
docker rm <container_id>
```

**Parametri principali di docker run**:
```bash
# Assegnare un nome al container
docker run --name my-container alpine

# Mappare una porta (host:container)
docker run -p 8080:80 nginx

# Montare un volume (host:container)
docker run -v /host/path:/container/path alpine

# Impostare variabili d'ambiente
docker run -e VAR_NAME=value alpine

# Limitare risorse (CPU, memoria)
docker run --memory=512m --cpus=0.5 alpine

# Rimuovere automaticamente il container dopo l'esecuzione
docker run --rm alpine
```

### Esercizi pratici guidati

#### Esercizio 1: Hello World Docker

```bash
# Eseguire il container hello-world
docker run hello-world
```

Questo comando:
1. Cerca l'immagine hello-world localmente
2. Se non la trova, la scarica da Docker Hub
3. Crea un container dall'immagine
4. Esegue il container, che stampa un messaggio e termina

**Analisi dell'output**: Discutere con gli studenti il messaggio visualizzato, che spiega il funzionamento di Docker.

#### Esercizio 2: Container interattivo

```bash
# Eseguire un container Alpine in modalità interattiva
docker run -it alpine sh
```

All'interno del container, esplorare l'ambiente:
```bash
# Verificare la versione di Alpine
cat /etc/os-release

# Elencare i processi
ps aux

# Installare un pacchetto
apk add curl

# Verificare l'indirizzo IP
ip addr

# Uscire dal container
exit
```

**Riflessione**: Dopo l'uscita, eseguire `docker ps -a` per vedere che il container esiste ancora ma è fermato.

#### Esercizio 3: Container in background

```bash
# Eseguire un server web NGINX in background
docker run -d -p 8080:80 --name my-nginx nginx
```

Verificare che il container sia in esecuzione:
```bash
# Elencare i container in esecuzione
docker ps

# Verificare l'accesso al server web
curl http://localhost:8080
```

**Interazione con il container**:
```bash
# Visualizzare i log del container
docker logs my-nginx

# Eseguire un comando all'interno del container in esecuzione
docker exec my-nginx ls -la /usr/share/nginx/html

# Accedere al container in modalità interattiva
docker exec -it my-nginx bash
```

#### Esercizio 4: Gestione del ciclo di vita dei container

```bash
# Fermare il container NGINX
docker stop my-nginx

# Verificare che il container sia fermato
docker ps -a

# Riavviare il container
docker start my-nginx

# Verificare che il container sia di nuovo in esecuzione
docker ps

# Fermare e rimuovere il container
docker rm -f my-nginx
```

#### Esercizio 5: Esplorazione di Docker Hub

1. Visitare [Docker Hub](https://hub.docker.com/) tramite browser
2. Cercare immagini ufficiali di Node.js
3. Leggere la documentazione dell'immagine
4. Identificare i tag disponibili e il loro significato

```bash
# Scaricare l'immagine Node.js
docker pull node:18-alpine

# Verificare che l'immagine sia stata scaricata
docker images
```

### Esercizio finale: Esecuzione di un'applicazione Node.js in container

#### Passo 1: Creare un semplice script JavaScript

Creare un file `app.js`:

```javascript
const http = require('http');
const hostname = '0.0.0.0';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Ciao dal container Docker!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server in esecuzione su http://${hostname}:${port}/`);
});
```

#### Passo 2: Eseguire l'applicazione in un container

```bash
# Creare una directory per l'applicazione
mkdir -p /home/studente/node-app
cd /home/studente/node-app

# Copiare il file app.js nella directory
# (il file app.js deve essere creato come indicato sopra)

# Eseguire l'applicazione in un container
docker run -d \
  --name node-app \
  -p 3000:3000 \
  -v "$(pwd):/app" \
  -w /app \
  node:18-alpine \
  node app.js
```

#### Passo 3: Verificare il funzionamento

```bash
# Verificare che il container sia in esecuzione
docker ps

# Verificare l'accesso all'applicazione
curl http://localhost:3000
```

#### Passo 4: Monitorare e gestire l'applicazione

```bash
# Visualizzare i log dell'applicazione
docker logs node-app

# Fermare l'applicazione
docker stop node-app

# Rimuovere il container
docker rm node-app
```

## Verifiche di apprendimento

### Quiz rapido sui concetti fondamentali

1. Qual è la differenza principale tra container e macchine virtuali?
2. Cosa rappresenta un'immagine Docker?
3. Quali sono i componenti principali dell'architettura Docker?
4. Perché i container sono considerati "effimeri"?
5. Quali sono i vantaggi principali della containerizzazione?

### Esercizi pratici di verifica

1. Eseguire un container Ubuntu in modalità interattiva e installare il pacchetto `nano`
2. Creare un container che mostri la data corrente e si chiuda automaticamente
3. Eseguire un container NGINX, modificare la pagina HTML predefinita e verificare le modifiche
4. Elencare tutte le immagini Docker presenti nel sistema e ordinare per dimensione
5. Eseguire un container in background, visualizzare i suoi log e poi fermarlo e rimuoverlo

### Discussione di gruppo

Discutere i seguenti punti:
1. In quali scenari i container offrono vantaggi significativi rispetto alle VM?
2. Quali sfide potrebbe presentare l'adozione di Docker in un ambiente di produzione?
3. Come potrebbe Docker migliorare il workflow di sviluppo per applicazioni Node.js?
4. Quali sono le implicazioni di sicurezza nell'utilizzo dei container?

## Conclusioni e preparazione per la prossima esercitazione

Nella prima esercitazione abbiamo:
- Compreso i concetti fondamentali della containerizzazione
- Confrontato VM e container
- Installato e configurato Docker su Alpine Linux
- Eseguito i primi comandi Docker
- Utilizzato container esistenti
- Eseguito un'applicazione Node.js in un container

Nella prossima esercitazione approfondiremo:
- La creazione di immagini Docker personalizzate con Dockerfile
- La gestione dei volumi per la persistenza dei dati
- La configurazione del networking
- La pubblicazione di immagini su Docker Hub

**Compito per casa (facoltativo)**: Sperimentare con diversi container disponibili su Docker Hub e provare a eseguire un'applicazione Node.js più complessa in un container.
