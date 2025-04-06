# Esercitazione 2: Creazione e Gestione di Container Docker

## Parte 1: Creazione di immagini Docker

### Introduzione ai Dockerfile

Un Dockerfile è un file di testo che contiene una serie di istruzioni per costruire un'immagine Docker in modo automatizzato. Rappresenta la "ricetta" per creare un'immagine e garantisce che questa possa essere ricreata in modo identico in qualsiasi ambiente.

#### Sintassi e istruzioni principali

**FROM**:
Definisce l'immagine base da cui partire. È sempre la prima istruzione non commentata in un Dockerfile.
```dockerfile
FROM node:18-alpine
```

**RUN**:
Esegue comandi all'interno del container durante la fase di build. Ogni istruzione RUN crea un nuovo layer nell'immagine.
```dockerfile
RUN apk add --no-cache python3 make g++
RUN npm install
```

**COPY e ADD**:
Copiano file e directory dal contesto di build all'immagine.
- COPY: Copia file locali nell'immagine
- ADD: Simile a COPY, ma può anche estrarre archivi e scaricare file da URL
```dockerfile
COPY package.json .
COPY src/ /app/src/
ADD https://example.com/file.tar.gz /tmp/
```

**WORKDIR**:
Imposta la directory di lavoro per le istruzioni successive (RUN, CMD, ENTRYPOINT, COPY, ADD).
```dockerfile
WORKDIR /app
```

**ENV**:
Imposta variabili d'ambiente che saranno disponibili durante la build e nel container in esecuzione.
```dockerfile
ENV NODE_ENV=production
ENV PORT=3000
```

**EXPOSE**:
Informa Docker che il container ascolterà su porte specifiche a runtime. Non pubblica effettivamente le porte.
```dockerfile
EXPOSE 3000
```

**CMD**:
Specifica il comando predefinito da eseguire quando il container viene avviato. Può essere sovrascritto dalla riga di comando.
```dockerfile
CMD ["node", "app.js"]
```

**ENTRYPOINT**:
Simile a CMD, ma più difficile da sovrascrivere. Spesso usato con CMD per creare container eseguibili.
```dockerfile
ENTRYPOINT ["node"]
CMD ["app.js"]
```

**VOLUME**:
Crea un punto di montaggio per volumi esterni.
```dockerfile
VOLUME /data
```

**USER**:
Imposta l'utente (o UID) per eseguire i comandi successivi e il container.
```dockerfile
USER node
```

#### Best practices nella scrittura di Dockerfile

1. **Utilizzare immagini base ufficiali e specifiche**:
   ```dockerfile
   # Meglio
   FROM node:18-alpine
   # Evitare
   FROM node:latest
   ```

2. **Combinare comandi RUN per ridurre i layer**:
   ```dockerfile
   # Meglio
   RUN apk add --no-cache python3 make g++ \
       && npm install \
       && npm cache clean --force
   # Evitare
   RUN apk add --no-cache python3
   RUN apk add --no-cache make g++
   RUN npm install
   RUN npm cache clean --force
   ```

3. **Utilizzare .dockerignore**:
   Creare un file `.dockerignore` per escludere file non necessari dal contesto di build:
   ```
   node_modules
   npm-debug.log
   .git
   .env
   ```

4. **Ordinare le istruzioni per ottimizzare la cache**:
   Posizionare le istruzioni che cambiano meno frequentemente all'inizio del Dockerfile.
   ```dockerfile
   FROM node:18-alpine
   WORKDIR /app
   
   # Prima copiare solo package.json per sfruttare la cache
   COPY package*.json ./
   RUN npm install
   
   # Poi copiare il resto del codice
   COPY . .
   ```

5. **Utilizzare utenti non root**:
   ```dockerfile
   RUN addgroup -g 1000 appuser && \
       adduser -u 1000 -G appuser -s /bin/sh -D appuser
   USER appuser
   ```

6. **Impostare WORKDIR invece di usare comandi RUN cd**:
   ```dockerfile
   # Meglio
   WORKDIR /app
   # Evitare
   RUN cd /app
   ```

7. **Utilizzare ENTRYPOINT con CMD per container eseguibili**:
   ```dockerfile
   ENTRYPOINT ["node"]
   CMD ["app.js"]
   ```

8. **Specificare tag espliciti per le immagini base**:
   ```dockerfile
   FROM node:18.15.0-alpine3.16
   ```

#### Ottimizzazione delle immagini

1. **Multi-stage build**:
   Utilizzare build multi-stage per ridurre la dimensione finale dell'immagine:
   ```dockerfile
   # Stage di build
   FROM node:18-alpine AS build
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   RUN npm run build
   
   # Stage di produzione
   FROM node:18-alpine
   WORKDIR /app
   COPY --from=build /app/dist ./dist
   COPY --from=build /app/package*.json ./
   RUN npm install --only=production
   CMD ["node", "dist/index.js"]
   ```

2. **Minimizzare il numero di layer**:
   Ogni istruzione nel Dockerfile crea un nuovo layer. Combinare comandi correlati.

3. **Rimuovere file temporanei e cache**:
   ```dockerfile
   RUN apt-get update && apt-get install -y \
       package1 \
       package2 \
       && rm -rf /var/lib/apt/lists/*
   ```

4. **Utilizzare immagini base leggere**:
   Preferire Alpine o Debian slim rispetto a immagini complete:
   ```dockerfile
   FROM node:18-alpine  # ~50MB
   # invece di
   FROM node:18  # ~900MB
   ```

### Creazione di immagini personalizzate

#### Scrittura di un Dockerfile per un'applicazione Node.js

Ecco un esempio di Dockerfile per un'applicazione Node.js:

```dockerfile
# Utilizziamo Node.js 18 su Alpine Linux come base
FROM node:18-alpine

# Creiamo e impostiamo la directory di lavoro
WORKDIR /app

# Copiamo package.json e package-lock.json (se presente)
COPY package*.json ./

# Installiamo le dipendenze
RUN npm install

# Copiamo il resto del codice sorgente
COPY . .

# Esponiamo la porta su cui l'applicazione ascolterà
EXPOSE 3000

# Comando per avviare l'applicazione
CMD ["node", "app.js"]
```

#### Comando docker build

Il comando `docker build` costruisce un'immagine Docker a partire da un Dockerfile e da un contesto di build:

```bash
# Sintassi base
docker build [opzioni] percorso_contesto

# Esempio: costruire un'immagine dalla directory corrente
docker build -t nome-immagine:tag .

# Esempio con opzioni comuni
docker build --no-cache -t mia-app:1.0 .
```

Opzioni principali:
- `-t, --tag`: Assegna un nome e un tag all'immagine
- `--no-cache`: Non utilizza la cache durante la build
- `-f, --file`: Specifica il percorso del Dockerfile (se diverso da ./Dockerfile)
- `--build-arg`: Passa variabili di build al Dockerfile

#### Gestione dei tag

I tag sono etichette assegnate alle immagini Docker per identificarne versioni o varianti:

```bash
# Taggare un'immagine durante la build
docker build -t mia-app:1.0 .

# Taggare un'immagine esistente
docker tag mia-app:1.0 mia-app:latest

# Taggare per un registry specifico
docker tag mia-app:1.0 username/mia-app:1.0
```

Best practices per i tag:
- Utilizzare `latest` per la versione più recente
- Utilizzare numeri di versione semantica (1.0.0)
- Includere informazioni sull'ambiente (prod, dev, staging)
- Utilizzare hash di commit per build di sviluppo

#### Strategie di versionamento delle immagini

1. **Versionamento semantico**:
   ```bash
   docker build -t mia-app:1.0.0 .
   docker tag mia-app:1.0.0 mia-app:1.0
   docker tag mia-app:1.0.0 mia-app:latest
   ```

2. **Tag basati su data**:
   ```bash
   docker build -t mia-app:$(date +%Y%m%d) .
   ```

3. **Tag basati su commit Git**:
   ```bash
   docker build -t mia-app:$(git rev-parse --short HEAD) .
   ```

4. **Tag per ambiente**:
   ```bash
   docker build -t mia-app:1.0.0-prod .
   docker build -t mia-app:1.0.0-dev .
   ```

### Esercizi pratici guidati

#### Esercizio 1: Creazione di un'immagine per un'applicazione Node.js semplice

**Passo 1**: Creare una directory per l'applicazione
```bash
mkdir -p /home/studente/node-docker-app
cd /home/studente/node-docker-app
```

**Passo 2**: Creare un file `package.json`
```bash
cat > package.json << 'EOF'
{
  "name": "node-docker-app",
  "version": "1.0.0",
  "description": "Semplice applicazione Node.js per Docker",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
EOF
```

**Passo 3**: Creare un file `app.js`
```bash
cat > app.js << 'EOF'
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Ciao dal container Docker! Questa è la mia prima immagine personalizzata.');
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione su http://0.0.0.0:${port}`);
});
EOF
```

**Passo 4**: Creare un file `.dockerignore`
```bash
cat > .dockerignore << 'EOF'
node_modules
npm-debug.log
.git
.gitignore
EOF
```

**Passo 5**: Creare un Dockerfile
```bash
cat > Dockerfile << 'EOF'
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
EOF
```

**Passo 6**: Costruire l'immagine Docker
```bash
docker build -t node-app:1.0 .
```

**Passo 7**: Verificare che l'immagine sia stata creata
```bash
docker images
```

**Passo 8**: Eseguire un container dall'immagine
```bash
docker run -d -p 3000:3000 --name my-node-app node-app:1.0
```

**Passo 9**: Verificare che l'applicazione funzioni
```bash
curl http://localhost:3000
```

**Passo 10**: Visualizzare i log del container
```bash
docker logs my-node-app
```

#### Esercizio 2: Ottimizzazione del Dockerfile

**Passo 1**: Modificare il Dockerfile per utilizzare multi-stage build
```bash
cat > Dockerfile.optimized << 'EOF'
# Stage di build
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# Stage di produzione
FROM node:18-alpine

WORKDIR /app

COPY --from=build /app/package*.json ./
COPY --from=build /app/app.js ./

RUN npm install --only=production

EXPOSE 3000

USER node

CMD ["node", "app.js"]
EOF
```

**Passo 2**: Costruire l'immagine ottimizzata
```bash
docker build -t node-app:1.0-optimized -f Dockerfile.optimized .
```

**Passo 3**: Confrontare le dimensioni delle immagini
```bash
docker images
```

**Passo 4**: Eseguire un container dall'immagine ottimizzata
```bash
docker run -d -p 3001:3000 --name my-node-app-optimized node-app:1.0-optimized
```

**Passo 5**: Verificare che l'applicazione funzioni
```bash
curl http://localhost:3001
```

#### Esercizio 3: Gestione delle dipendenze

**Passo 1**: Aggiungere una nuova dipendenza al `package.json`
```bash
cat > package.json << 'EOF'
{
  "name": "node-docker-app",
  "version": "1.0.0",
  "description": "Semplice applicazione Node.js per Docker",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "moment": "^2.29.4"
  }
}
EOF
```

**Passo 2**: Aggiornare il file `app.js` per utilizzare la nuova dipendenza
```bash
cat > app.js << 'EOF'
const express = require('express');
const moment = require('moment');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  const now = moment().format('YYYY-MM-DD HH:mm:ss');
  res.send(`Ciao dal container Docker! Ora sono le ${now}`);
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione su http://0.0.0.0:${port}`);
});
EOF
```

**Passo 3**: Ricostruire l'immagine
```bash
docker build -t node-app:1.1 .
```

**Passo 4**: Eseguire un container dalla nuova immagine
```bash
docker run -d -p 3002:3000 --name my-node-app-v1.1 node-app:1.1
```

**Passo 5**: Verificare che l'applicazione utilizzi la nuova dipendenza
```bash
curl http://localhost:3002
```

#### Esercizio 4: Multi-stage build per ridurre la dimensione dell'immagine

**Passo 1**: Creare un'applicazione più complessa con fase di build
```bash
mkdir -p /home/studente/node-typescript-app
cd /home/studente/node-typescript-app
```

**Passo 2**: Creare un file `package.json` per TypeScript
```bash
cat > package.json << 'EOF'
{
  "name": "node-typescript-app",
  "version": "1.0.0",
  "description": "Applicazione TypeScript per Docker",
  "main": "dist/index.js",
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "@types/express": "^4.17.17",
    "@types/node": "^18.15.11",
    "typescript": "^5.0.4"
  }
}
EOF
```

**Passo 3**: Creare un file `tsconfig.json`
```bash
cat > tsconfig.json << 'EOF'
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*"]
}
EOF
```

**Passo 4**: Creare la directory `src` e il file `index.ts`
```bash
mkdir -p src
cat > src/index.ts << 'EOF'
import express from 'express';

const app = express();
const port: number = parseInt(process.env.PORT || '3000', 10);

app.get('/', (req, res) => {
  res.send('Ciao dal container Docker con TypeScript!');
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione su http://0.0.0.0:${port}`);
});
EOF
```

**Passo 5**: Creare un Dockerfile con multi-stage build
```bash
cat > Dockerfile << 'EOF'
# Stage di build
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
COPY tsconfig.json ./

RUN npm install

COPY src/ ./src/

RUN npm run build

# Stage di produzione
FROM node:18-alpine

WORKDIR /app

COPY --from=build /app/package*.json ./
COPY --from=build /app/dist/ ./dist/

RUN npm install --only=production

EXPOSE 3000

USER node

CMD ["node", "dist/index.js"]
EOF
```

**Passo 6**: Costruire l'immagine
```bash
docker build -t typescript-app:1.0 .
```

**Passo 7**: Eseguire un container dall'immagine
```bash
docker run -d -p 3003:3000 --name my-typescript-app typescript-app:1.0
```

**Passo 8**: Verificare che l'applicazione funzioni
```bash
curl http://localhost:3003
```

**Passo 9**: Confrontare le dimensioni dell'immagine
```bash
docker images
```

## Parte 2: Persistenza dei dati e volumi

### Gestione dello stato nei container

#### Problema della persistenza nei container

I container Docker sono progettati per essere effimeri: quando un container viene eliminato, tutti i dati al suo interno vengono persi. Questo comportamento è intenzionale e segue il principio dell'immutabilità, ma presenta sfide quando si tratta di gestire dati che devono persistere oltre il ciclo di vita del container.

Scenari comuni che richiedono persistenza:
- Database
- File di configurazione
- File caricati dagli utenti
- Log
- Cache e dati temporanei

#### Tipi di storage in Docker

Docker offre diverse soluzioni per la persistenza dei dati:

1. **Volumi Docker**:
   - Gestiti direttamente da Docker
   - Indipendenti dal ciclo di vita del container
   - Isolati dal filesystem dell'host
   - Più facili da migrare e fare backup
   - Supportano driver di storage

2. **Bind Mount**:
   - Mappano direttamente una directory dell'host nel container
   - Utili per lo sviluppo e quando è necessario accedere ai file dall'host
   - Dipendono dalla struttura delle directory dell'host

3. **tmpfs Mount**:
   - Memorizzano dati solo in memoria
   - Utili per dati temporanei e sensibili
   - I dati vengono persi al riavvio del container o dell'host

#### Volumi Docker

I volumi Docker sono il meccanismo preferito per la persistenza dei dati:

```bash
# Creare un volume
docker volume create mio-volume

# Elencare i volumi
docker volume ls

# Ispezionare un volume
docker volume inspect mio-volume

# Utilizzare un volume in un container
docker run -d --name db -v mio-volume:/data/db mongo

# Rimuovere un volume
docker volume rm mio-volume

# Rimuovere volumi non utilizzati
docker volume prune
```

Vantaggi dei volumi:
- Backup e ripristino semplificati
- Condivisione tra container
- Gestione del ciclo di vita indipendente
- Prestazioni migliori (specialmente con driver ottimizzati)
- Funzionano su tutti i sistemi operativi

#### Bind mount

I bind mount collegano una directory dell'host a una directory nel container:

```bash
# Utilizzare un bind mount
docker run -d --name web -v /home/user/html:/usr/share/nginx/html nginx

# Utilizzare la sintassi più recente
docker run -d --name web --mount type=bind,source=/home/user/html,target=/usr/share/nginx/html nginx

# Bind mount in sola lettura
docker run -d --name web -v /home/user/html:/usr/share/nginx/html:ro nginx
```

Vantaggi dei bind mount:
- Accesso diretto ai file dall'host
- Modifiche immediate visibili nel container
- Utili per lo sviluppo
- Nessuna gestione aggiuntiva di volumi

Svantaggi:
- Dipendenza dalla struttura delle directory dell'host
- Potenziali problemi di permessi
- Meno portabili tra ambienti diversi

### Esercizi pratici guidati

#### Esercizio 1: Creazione e gestione di volumi Docker

**Passo 1**: Creare un volume Docker
```bash
docker volume create data-volume
```

**Passo 2**: Ispezionare il volume
```bash
docker volume inspect data-volume
```

**Passo 3**: Utilizzare il volume con un container
```bash
docker run -d --name nginx-with-volume -v data-volume:/usr/share/nginx/html nginx
```

**Passo 4**: Accedere al container e modificare i dati nel volume
```bash
docker exec -it nginx-with-volume sh
echo "<h1>Pagina servita da un volume Docker</h1>" > /usr/share/nginx/html/index.html
exit
```

**Passo 5**: Fermare e rimuovere il container
```bash
docker stop nginx-with-volume
docker rm nginx-with-volume
```

**Passo 6**: Creare un nuovo container che utilizza lo stesso volume
```bash
docker run -d --name nginx-new -p 8080:80 -v data-volume:/usr/share/nginx/html nginx
```

**Passo 7**: Verificare che i dati persistano
```bash
curl http://localhost:8080
```

#### Esercizio 2: Utilizzo di bind mount

**Passo 1**: Creare una directory sull'host
```bash
mkdir -p /home/studente/nginx-data
echo "<h1>Pagina servita da un bind mount</h1>" > /home/studente/nginx-data/index.html
```

**Passo 2**: Eseguire un container con bind mount
```bash
docker run -d --name nginx-bind -p 8081:80 -v /home/studente/nginx-data:/usr/share/nginx/html nginx
```

**Passo 3**: Verificare che la pagina sia accessibile
```bash
curl http://localhost:8081
```

**Passo 4**: Modificare il file sull'host
```bash
echo "<h1>Pagina modificata sull'host</h1>" > /home/studente/nginx-data/index.html
```

**Passo 5**: Verificare che le modifiche siano visibili nel container
```bash
curl http://localhost:8081
```

#### Esercizio 3: Condivisione di dati tra container

**Passo 1**: Creare un volume per la condivisione
```bash
docker volume create shared-data
```

**Passo 2**: Eseguire un container che scrive nel volume
```bash
docker run --name writer -v shared-data:/data alpine sh -c "echo 'Dati condivisi tra container' > /data/shared.txt"
```

**Passo 3**: Eseguire un container che legge dal volume
```bash
docker run --name reader -v shared-data:/data alpine cat /data/shared.txt
```

**Passo 4**: Verificare che i dati siano stati condivisi
```bash
docker logs reader
```

#### Esercizio 4: Backup e ripristino dei dati

**Passo 1**: Creare un volume con dati
```bash
docker volume create backup-demo
docker run --name data-creator -v backup-demo:/data alpine sh -c "echo 'Dati importanti' > /data/important.txt"
```

**Passo 2**: Creare un backup del volume
```bash
docker run --rm -v backup-demo:/source -v /home/studente:/backup alpine tar -czf /backup/volume-backup.tar.gz -C /source .
```

**Passo 3**: Verificare che il backup sia stato creato
```bash
ls -la /home/studente/volume-backup.tar.gz
```

**Passo 4**: Eliminare il volume originale
```bash
docker rm data-creator
docker volume rm backup-demo
```

**Passo 5**: Ripristinare il backup in un nuovo volume
```bash
docker volume create restored-volume
docker run --rm -v restored-volume:/target -v /home/studente:/backup alpine sh -c "tar -xzf /backup/volume-backup.tar.gz -C /target"
```

**Passo 6**: Verificare che i dati siano stati ripristinati
```bash
docker run --rm -v restored-volume:/data alpine cat /data/important.txt
```

## Parte 3: Networking e distribuzione

### Networking in Docker

#### Reti Docker predefinite

Docker crea automaticamente tre reti predefinite:

1. **bridge**: La rete predefinita per i container. I container su questa rete possono comunicare tra loro tramite IP.
2. **host**: I container condividono lo stack di rete dell'host. Non c'è isolamento di rete.
3. **none**: I container non hanno connettività di rete.

```bash
# Elencare le reti disponibili
docker network ls

# Ispezionare una rete
docker network inspect bridge
```

#### Creazione di reti personalizzate

Le reti personalizzate offrono migliore isolamento e funzionalità di risoluzione DNS:

```bash
# Creare una rete bridge personalizzata
docker network create my-network

# Creare una rete con opzioni specifiche
docker network create --driver bridge --subnet=172.18.0.0/16 --gateway=172.18.0.1 custom-network

# Collegare un container esistente a una rete
docker network connect my-network container-name

# Scollegare un container da una rete
docker network disconnect my-network container-name
```

#### Comunicazione tra container

I container nella stessa rete possono comunicare tra loro:

1. **Tramite IP**: Ogni container riceve un indirizzo IP all'interno della rete
2. **Tramite nome**: In reti personalizzate, i container possono risolvere i nomi degli altri container

```bash
# Eseguire container nella stessa rete
docker run -d --name web --network my-network nginx
docker run -d --name db --network my-network postgres

# Eseguire ping da un container all'altro
docker exec web ping db
```

#### Esposizione di porte e port mapping

Per accedere ai servizi in esecuzione nei container dall'esterno:

```bash
# Mappare una porta (host:container)
docker run -d -p 8080:80 nginx

# Mappare tutte le porte esposte su porte casuali dell'host
docker run -d -P nginx

# Mappare su un'interfaccia specifica
docker run -d -p 127.0.0.1:8080:80 nginx
```

### Distribuzione di immagini Docker

#### Introduzione a Docker Hub

Docker Hub è il registry pubblico ufficiale per le immagini Docker:
- Repository pubblici e privati
- Immagini ufficiali e verificate
- Integrazione con GitHub e Bitbucket
- Webhook e automazioni

#### Creazione di un account Docker Hub

1. Visitare [Docker Hub](https://hub.docker.com/) e registrarsi
2. Verificare l'email
3. Accedere da CLI:
   ```bash
   docker login
   ```

#### Push e pull di immagini

```bash
# Taggare un'immagine per Docker Hub
docker tag node-app:1.0 username/node-app:1.0

# Pubblicare l'immagine su Docker Hub
docker push username/node-app:1.0

# Scaricare un'immagine da Docker Hub
docker pull username/node-app:1.0
```

#### Gestione dei repository

- **Repository pubblici**: Accessibili a tutti
- **Repository privati**: Accessibili solo agli utenti autorizzati
- **Organizzazioni**: Gestione di team e permessi
- **Automazioni**: Build automatiche da repository Git

### Esercizi pratici guidati

#### Esercizio 1: Configurazione di una rete Docker personalizzata

**Passo 1**: Creare una rete Docker personalizzata
```bash
docker network create app-network
```

**Passo 2**: Ispezionare la rete creata
```bash
docker network inspect app-network
```

**Passo 3**: Eseguire container nella rete personalizzata
```bash
docker run -d --name web --network app-network -p 8080:80 nginx
docker run -d --name db --network app-network postgres:alpine
```

**Passo 4**: Verificare la comunicazione tra container
```bash
docker exec web ping db
```

**Passo 5**: Verificare la risoluzione DNS
```bash
docker exec web sh -c "wget -O- db:5432 || echo 'Connessione riuscita a DB'"
```

#### Esercizio 2: Comunicazione tra container in rete

**Passo 1**: Creare un'applicazione Node.js che si connette a Redis
```bash
mkdir -p /home/studente/node-redis-app
cd /home/studente/node-redis-app
```

**Passo 2**: Creare un file `package.json`
```bash
cat > package.json << 'EOF'
{
  "name": "node-redis-app",
  "version": "1.0.0",
  "description": "Applicazione Node.js con Redis",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2",
    "redis": "^4.6.6"
  }
}
EOF
```

**Passo 3**: Creare un file `app.js`
```bash
cat > app.js << 'EOF'
const express = require('express');
const redis = require('redis');
const app = express();
const port = 3000;

// Configurazione client Redis
const client = redis.createClient({
  url: 'redis://redis:6379'
});

// Connessione a Redis
(async () => {
  await client.connect();
})();

client.on('error', (err) => {
  console.log('Errore Redis:', err);
});

// Contatore visite
app.get('/', async (req, res) => {
  try {
    const count = await client.incr('visits');
    res.send(`Numero di visite: ${count}`);
  } catch (error) {
    res.send('Errore nella connessione a Redis: ' + error.message);
  }
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione su http://0.0.0.0:${port}`);
});
EOF
```

**Passo 4**: Creare un Dockerfile
```bash
cat > Dockerfile << 'EOF'
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
EOF
```

**Passo 5**: Costruire l'immagine
```bash
docker build -t node-redis-app:1.0 .
```

**Passo 6**: Creare una rete per l'applicazione
```bash
docker network create redis-network
```

**Passo 7**: Eseguire Redis nella rete
```bash
docker run -d --name redis --network redis-network redis:alpine
```

**Passo 8**: Eseguire l'applicazione Node.js nella stessa rete
```bash
docker run -d --name node-app --network redis-network -p 3000:3000 node-redis-app:1.0
```

**Passo 9**: Verificare che l'applicazione funzioni
```bash
curl http://localhost:3000
```

**Passo 10**: Verificare che il contatore aumenti ad ogni richiesta
```bash
curl http://localhost:3000
curl http://localhost:3000
```

#### Esercizio 3: Pubblicazione di un'immagine su Docker Hub

**Passo 1**: Accedere a Docker Hub
```bash
docker login
```

**Passo 2**: Taggare l'immagine con il proprio username
```bash
docker tag node-redis-app:1.0 username/node-redis-app:1.0
```

**Passo 3**: Pubblicare l'immagine su Docker Hub
```bash
docker push username/node-redis-app:1.0
```

**Passo 4**: Verificare che l'immagine sia stata pubblicata
```bash
docker search username/node-redis-app
```

**Passo 5**: Rimuovere l'immagine locale
```bash
docker rmi username/node-redis-app:1.0
docker rmi node-redis-app:1.0
```

**Passo 6**: Scaricare l'immagine da Docker Hub
```bash
docker pull username/node-redis-app:1.0
```

**Passo 7**: Eseguire l'immagine scaricata
```bash
docker run -d --name node-app-from-hub -p 3001:3000 --network redis-network username/node-redis-app:1.0
```

**Passo 8**: Verificare che l'applicazione funzioni
```bash
curl http://localhost:3001
```

#### Esercizio 4: Applicazione multi-container

**Passo 1**: Creare una directory per l'applicazione
```bash
mkdir -p /home/studente/multi-container-app
cd /home/studente/multi-container-app
```

**Passo 2**: Creare un'applicazione Node.js con MongoDB
```bash
mkdir -p api
cat > api/package.json << 'EOF'
{
  "name": "api",
  "version": "1.0.0",
  "description": "API per applicazione multi-container",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.3",
    "cors": "^2.8.5"
  }
}
EOF
```

**Passo 3**: Creare il file server.js
```bash
cat > api/server.js << 'EOF'
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const app = express();
const port = 3000;

app.use(cors());
app.use(express.json());

// Connessione a MongoDB
mongoose.connect('mongodb://db:27017/notesapp', {
  useNewUrlParser: true,
  useUnifiedTopology: true
}).then(() => {
  console.log('Connesso a MongoDB');
}).catch(err => {
  console.error('Errore di connessione a MongoDB:', err);
});

// Schema e modello
const noteSchema = new mongoose.Schema({
  text: String,
  createdAt: { type: Date, default: Date.now }
});

const Note = mongoose.model('Note', noteSchema);

// Routes
app.get('/api/notes', async (req, res) => {
  try {
    const notes = await Note.find().sort({ createdAt: -1 });
    res.json(notes);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.post('/api/notes', async (req, res) => {
  try {
    const note = new Note({ text: req.body.text });
    await note.save();
    res.status(201).json(note);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

app.listen(port, '0.0.0.0', () => {
  console.log(`API server in esecuzione su http://0.0.0.0:${port}`);
});
EOF
```

**Passo 4**: Creare il Dockerfile per l'API
```bash
cat > api/Dockerfile << 'EOF'
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
EOF
```

**Passo 5**: Creare un frontend semplice
```bash
mkdir -p frontend
cat > frontend/index.html << 'EOF'
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Note App</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 800px; margin: 0 auto; padding: 20px; }
    .note { background: #f9f9f9; padding: 10px; margin-bottom: 10px; border-radius: 5px; }
    form { margin-bottom: 20px; }
    input { padding: 8px; width: 70%; }
    button { padding: 8px 15px; background: #4CAF50; color: white; border: none; cursor: pointer; }
  </style>
</head>
<body>
  <h1>Note App</h1>
  
  <form id="note-form">
    <input type="text" id="note-text" placeholder="Scrivi una nota..." required>
    <button type="submit">Salva</button>
  </form>
  
  <h2>Note</h2>
  <div id="notes-container"></div>

  <script>
    const API_URL = 'http://localhost:3000/api/notes';
    
    // Carica le note
    async function loadNotes() {
      try {
        const response = await fetch(API_URL);
        const notes = await response.json();
        
        const container = document.getElementById('notes-container');
        container.innerHTML = '';
        
        notes.forEach(note => {
          const noteEl = document.createElement('div');
          noteEl.className = 'note';
          noteEl.textContent = note.text;
          container.appendChild(noteEl);
        });
      } catch (error) {
        console.error('Errore nel caricamento delle note:', error);
      }
    }
    
    // Gestisce l'invio del form
    document.getElementById('note-form').addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const textInput = document.getElementById('note-text');
      const text = textInput.value;
      
      try {
        await fetch(API_URL, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ text }),
        });
        
        textInput.value = '';
        loadNotes();
      } catch (error) {
        console.error('Errore nel salvataggio della nota:', error);
      }
    });
    
    // Carica le note all'avvio
    loadNotes();
  </script>
</body>
</html>
EOF
```

**Passo 6**: Creare il Dockerfile per il frontend
```bash
cat > frontend/Dockerfile << 'EOF'
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
EOF
```

**Passo 7**: Costruire le immagini
```bash
docker build -t notes-api:1.0 ./api
docker build -t notes-frontend:1.0 ./frontend
```

**Passo 8**: Creare una rete per l'applicazione
```bash
docker network create notes-app-network
```

**Passo 9**: Eseguire MongoDB
```bash
docker run -d --name db --network notes-app-network -v notes-data:/data/db mongo:4.4
```

**Passo 10**: Eseguire l'API
```bash
docker run -d --name api --network notes-app-network -p 3000:3000 notes-api:1.0
```

**Passo 11**: Eseguire il frontend
```bash
docker run -d --name frontend --network notes-app-network -p 8080:80 notes-frontend:1.0
```

**Passo 12**: Verificare che l'applicazione funzioni
```bash
echo "Apri http://localhost:8080 nel browser per utilizzare l'applicazione"
```

## Verifiche di apprendimento

### Quiz sui concetti di Dockerfile e volumi

1. Qual è lo scopo dell'istruzione WORKDIR in un Dockerfile?
2. Perché è consigliabile combinare più comandi RUN in un unico comando?
3. Qual è la differenza tra CMD e ENTRYPOINT?
4. Quali sono i vantaggi dei volumi Docker rispetto ai bind mount?
5. Come si può rendere un volume di sola lettura in un container?

### Revisione dei Dockerfile creati

Analizzare i Dockerfile creati durante l'esercitazione e discutere:
1. Punti di forza e debolezza
2. Possibili ottimizzazioni
3. Best practices applicate
4. Sicurezza delle immagini

### Verifica del funzionamento dell'applicazione multi-container

1. Verificare che tutti i container comunichino correttamente
2. Testare la persistenza dei dati
3. Verificare il comportamento in caso di riavvio dei container
4. Discutere possibili miglioramenti dell'architettura

## Conclusioni e preparazione per la prossima esercitazione

Nella seconda esercitazione abbiamo:
- Creato immagini Docker personalizzate con Dockerfile
- Ottimizzato le immagini con multi-stage build
- Gestito la persistenza dei dati con volumi Docker
- Configurato reti Docker per la comunicazione tra container
- Pubblicato e utilizzato immagini su Docker Hub
- Implementato un'applicazione multi-container

Nella prossima esercitazione approfondiremo:
- Configurazioni avanzate di networking
- Gestione delle configurazioni con variabili d'ambiente
- Introduzione all'orchestrazione con Docker Compose
- Preparazione per il progetto finale

**Compito per casa (facoltativo)**: Migliorare l'applicazione multi-container aggiungendo funzionalità e ottimizzando le immagini Docker.
