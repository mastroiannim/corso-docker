# Esercitazione 3: Docker Avanzato e Applicazioni Distribuite

## Parte 1: Networking avanzato in Docker

### Reti Docker avanzate

#### Tipologie di reti in Docker

Docker supporta diversi tipi di reti, ciascuna con caratteristiche specifiche:

1. **Bridge**:
   - Tipo di rete predefinito
   - Crea un bridge software isolato sull'host
   - I container sulla stessa rete bridge possono comunicare tra loro
   - Esempio di creazione:
     ```bash
     docker network create --driver bridge my-bridge-network
     ```

2. **Host**:
   - Rimuove l'isolamento di rete tra container e host
   - I container condividono lo stack di rete dell'host
   - Migliori prestazioni ma minore isolamento
   - Esempio di utilizzo:
     ```bash
     docker run --network host nginx
     ```

3. **Overlay**:
   - Permette la comunicazione tra container su host diversi
   - Utilizzata principalmente in ambienti multi-host (Swarm)
   - Esempio di creazione:
     ```bash
     docker network create --driver overlay my-overlay-network
     ```

4. **Macvlan**:
   - Assegna un indirizzo MAC ai container
   - Permette ai container di apparire come dispositivi fisici sulla rete
   - Utile per applicazioni che richiedono connessione diretta alla rete fisica
   - Esempio di creazione:
     ```bash
     docker network create --driver macvlan \
       --subnet=192.168.0.0/24 \
       --gateway=192.168.0.1 \
       -o parent=eth0 my-macvlan-network
     ```

5. **None**:
   - Disabilita completamente la rete per il container
   - Il container ha solo l'interfaccia di loopback
   - Esempio di utilizzo:
     ```bash
     docker run --network none alpine
     ```

#### Configurazione avanzata delle reti

**Subnet e Gateway**:
```bash
docker network create --driver bridge \
  --subnet=172.20.0.0/16 \
  --gateway=172.20.0.1 \
  custom-network
```

**IP Range**:
```bash
docker network create --driver bridge \
  --subnet=172.20.0.0/16 \
  --ip-range=172.20.5.0/24 \
  --gateway=172.20.0.1 \
  custom-network
```

**Opzioni avanzate**:
```bash
docker network create --driver bridge \
  --opt com.docker.network.bridge.name=docker-bridge \
  --opt com.docker.network.bridge.enable_icc=true \
  custom-network
```

**Assegnazione di IP statici**:
```bash
docker run --network custom-network --ip 172.20.5.10 nginx
```

#### DNS e service discovery

Docker fornisce un server DNS integrato che permette ai container di risolvere i nomi degli altri container nella stessa rete:

1. **Risoluzione automatica dei nomi**:
   - I container possono comunicare usando i nomi degli altri container
   - Funziona solo in reti personalizzate (non nella rete bridge predefinita)
   - Esempio:
     ```bash
     # Creare una rete
     docker network create app-network
     
     # Eseguire container nella rete
     docker run -d --name web --network app-network nginx
     docker run -d --name db --network app-network postgres
     
     # Accedere a db da web usando il nome
     docker exec web ping db
     ```

2. **Alias DNS**:
   - Permette di assegnare nomi aggiuntivi ai container
   - Utile per implementare pattern come blue/green deployment
   - Esempio:
     ```bash
     docker run -d --name web1 --network app-network --network-alias webserver nginx
     docker run -d --name web2 --network app-network --network-alias webserver nginx
     ```

3. **Configurazione DNS personalizzata**:
   - È possibile specificare server DNS esterni
   - Esempio:
     ```bash
     docker run --dns 8.8.8.8 --dns 8.8.4.4 nginx
     ```

#### Gestione degli indirizzi IP

**Visualizzare gli indirizzi IP dei container**:
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name
```

**Visualizzare tutti i container in una rete**:
```bash
docker network inspect -f '{{range .Containers}}{{.Name}}: {{.IPv4Address}}{{println}}{{end}}' network_name
```

**Disconnettere e riconnettere container a una rete**:
```bash
docker network disconnect network_name container_name
docker network connect network_name container_name
```

### Comunicazione tra container

#### Modelli di comunicazione tra container

1. **Comunicazione diretta tramite rete**:
   - Container nella stessa rete possono comunicare direttamente
   - Utilizzo di nomi DNS per la risoluzione
   - Esempio: applicazione web che comunica con un database

2. **Comunicazione tramite porte esposte**:
   - Container espongono porte che altri container possono utilizzare
   - Esempio: server web che espone la porta 80

3. **Comunicazione tramite volumi condivisi**:
   - Container condividono dati attraverso volumi
   - Esempio: container di elaborazione che legge/scrive su un volume condiviso

4. **Comunicazione tramite ambiente host**:
   - Container comunicano attraverso l'host (socket, file, ecc.)
   - Esempio: container che utilizzano socket Unix

#### Utilizzo di nomi DNS per la comunicazione

In una rete Docker personalizzata, i container possono comunicare utilizzando i nomi degli altri container:

```bash
# Creare una rete
docker network create microservices

# Eseguire container nella rete
docker run -d --name api --network microservices my-api
docker run -d --name db --network microservices postgres

# Nel codice dell'API, connettersi al database usando il nome "db"
# Esempio in Node.js:
# const client = new Client({ host: 'db', port: 5432, ... });
```

Vantaggi dell'utilizzo dei nomi DNS:
- Indipendenza dagli indirizzi IP (che possono cambiare)
- Facilità di configurazione
- Supporto per la scalabilità (con alias DNS)

#### Implementazione di pattern client-server

Esempio di pattern client-server con Node.js e Redis:

**Server (Redis)**:
```bash
docker run -d --name redis --network app-network redis:alpine
```

**Client (Node.js)**:
```javascript
// app.js
const express = require('express');
const redis = require('redis');
const app = express();

// Connessione a Redis usando il nome del container
const client = redis.createClient({
  url: 'redis://redis:6379'
});

client.connect();

app.get('/counter', async (req, res) => {
  const count = await client.incr('visits');
  res.send(`Visite: ${count}`);
});

app.listen(3000, '0.0.0.0');
```

```bash
docker run -d --name api --network app-network -p 3000:3000 my-node-app
```

#### Gestione delle dipendenze tra container

1. **Attesa per i servizi dipendenti**:
   - Problema: i container possono avviarsi prima che i servizi dipendenti siano pronti
   - Soluzione: implementare meccanismi di retry o utilizzare script di attesa

   Esempio di script di attesa per database:
   ```bash
   #!/bin/sh
   # wait-for-db.sh
   
   set -e
   
   host="$1"
   shift
   cmd="$@"
   
   until nc -z "$host" 5432; do
     echo "Attesa per il database su $host:5432..."
     sleep 1
   done
   
   echo "Database disponibile, esecuzione del comando: $cmd"
   exec $cmd
   ```

   Utilizzo nello Dockerfile:
   ```dockerfile
   COPY wait-for-db.sh /usr/local/bin/
   RUN chmod +x /usr/local/bin/wait-for-db.sh
   CMD ["wait-for-db.sh", "db", "node", "app.js"]
   ```

2. **Health check**:
   - Implementare endpoint di health check nelle applicazioni
   - Utilizzare i health check di Docker

   Esempio nel Dockerfile:
   ```dockerfile
   HEALTHCHECK --interval=30s --timeout=3s \
     CMD curl -f http://localhost:3000/health || exit 1
   ```

### Esercizi pratici guidati

#### Esercizio 1: Creazione di reti personalizzate con configurazioni specifiche

**Passo 1**: Creare una rete bridge personalizzata con subnet specifica
```bash
docker network create --driver bridge \
  --subnet=172.30.0.0/16 \
  --gateway=172.30.0.1 \
  custom-bridge
```

**Passo 2**: Ispezionare la rete creata
```bash
docker network inspect custom-bridge
```

**Passo 3**: Eseguire container con IP statici
```bash
docker run -d --name web1 --network custom-bridge --ip 172.30.0.10 nginx
docker run -d --name web2 --network custom-bridge --ip 172.30.0.11 nginx
```

**Passo 4**: Verificare gli indirizzi IP assegnati
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web1
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web2
```

**Passo 5**: Testare la comunicazione tra i container
```bash
docker exec web1 ping -c 3 web2
docker exec web2 ping -c 3 web1
```

#### Esercizio 2: Implementazione di comunicazione tra container in diverse reti

**Passo 1**: Creare due reti separate
```bash
docker network create frontend-network
docker network create backend-network
```

**Passo 2**: Eseguire container in reti diverse
```bash
# Container nella rete frontend
docker run -d --name web --network frontend-network -p 8080:80 nginx

# Container nella rete backend
docker run -d --name db --network backend-network postgres:alpine
```

**Passo 3**: Verificare che i container non possano comunicare
```bash
docker exec web ping -c 2 db || echo "Comunicazione non possibile"
```

**Passo 4**: Collegare un container a entrambe le reti
```bash
docker run -d --name api --network frontend-network my-api-image
docker network connect backend-network api
```

**Passo 5**: Verificare che il container api possa comunicare con entrambi
```bash
docker exec api ping -c 2 web
docker exec api ping -c 2 db
```

**Passo 6**: Implementare un proxy per la comunicazione tra reti
```bash
# Creare un file di configurazione per Nginx
mkdir -p /home/studente/proxy-config
cat > /home/studente/proxy-config/nginx.conf << 'EOF'
events {
    worker_connections 1024;
}

http {
    server {
        listen 80;
        
        location /api/ {
            proxy_pass http://api:3000/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
        
        location / {
            proxy_pass http://web:80;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
EOF

# Eseguire un container Nginx come proxy
docker run -d --name proxy \
  --network frontend-network \
  -p 80:80 \
  -v /home/studente/proxy-config/nginx.conf:/etc/nginx/nginx.conf:ro \
  nginx

# Collegare il proxy alla rete backend
docker network connect backend-network proxy
```

#### Esercizio 3: Risoluzione di problemi di networking comuni

**Passo 1**: Creare una situazione di conflitto di porte
```bash
# Eseguire due container che utilizzano la stessa porta sull'host
docker run -d --name web1 -p 8080:80 nginx
docker run -d --name web2 -p 8080:80 nginx || echo "Errore: porta già in uso"
```

**Passo 2**: Risolvere il conflitto utilizzando porte diverse
```bash
docker run -d --name web2 -p 8081:80 nginx
```

**Passo 3**: Simulare un problema di DNS
```bash
# Eseguire un container con DNS non valido
docker run --dns 1.1.1.123 alpine ping -c 2 google.com || echo "Errore DNS"
```

**Passo 4**: Risolvere il problema DNS
```bash
docker run --dns 8.8.8.8 alpine ping -c 2 google.com
```

**Passo 5**: Diagnosticare problemi di connettività
```bash
# Creare una rete isolata
docker network create isolated-network

# Eseguire un container nella rete isolata
docker run -d --name isolated --network isolated-network alpine sleep 1000

# Tentare di accedere a Internet
docker exec isolated ping -c 2 google.com || echo "Nessuna connettività Internet"

# Verificare la configurazione di rete
docker exec isolated ip addr
docker exec isolated route -n
```

#### Esercizio 4: Monitoraggio del traffico di rete tra container

**Passo 1**: Installare strumenti di monitoraggio
```bash
docker run -d --name monitoring --network host alpine sleep 1000
docker exec monitoring apk add --no-cache tcpdump iptables
```

**Passo 2**: Creare una rete per il test
```bash
docker network create monitoring-network
```

**Passo 3**: Eseguire container di test
```bash
docker run -d --name sender --network monitoring-network alpine sleep 1000
docker run -d --name receiver --network monitoring-network alpine sleep 1000
```

**Passo 4**: Generare traffico tra i container
```bash
# Ottenere l'IP del receiver
RECEIVER_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' receiver)

# Generare traffico
docker exec sender ping -c 10 $RECEIVER_IP
```

**Passo 5**: Monitorare il traffico
```bash
# Identificare l'interfaccia bridge
BRIDGE=$(docker network inspect -f '{{.Id}}' monitoring-network | cut -c1-12)
BRIDGE="br-$BRIDGE"

# Monitorare il traffico
docker exec monitoring tcpdump -i $BRIDGE -n
```

## Parte 2: Configurazione e ottimizzazione

### Gestione delle configurazioni

#### Utilizzo di variabili d'ambiente

Le variabili d'ambiente sono il metodo principale per configurare le applicazioni containerizzate:

1. **Definizione nel Dockerfile**:
   ```dockerfile
   ENV NODE_ENV=production
   ENV PORT=3000
   ```

2. **Passaggio durante l'esecuzione**:
   ```bash
   docker run -e NODE_ENV=development -e PORT=4000 my-app
   ```

3. **Utilizzo di file .env**:
   ```bash
   # .env file
   NODE_ENV=staging
   PORT=5000
   ```
   
   ```bash
   docker run --env-file .env my-app
   ```

4. **Accesso alle variabili d'ambiente nell'applicazione**:
   ```javascript
   // Node.js
   const port = process.env.PORT || 3000;
   ```

   ```python
   # Python
   import os
   port = os.environ.get('PORT', '3000')
   ```

#### File di configurazione e secrets

1. **Montaggio di file di configurazione**:
   ```bash
   docker run -v /path/to/config.json:/app/config.json my-app
   ```

2. **Utilizzo di Docker secrets** (in modalità Swarm):
   ```bash
   # Creare un secret
   echo "password123" | docker secret create db_password -
   
   # Utilizzare il secret
   docker service create \
     --name db \
     --secret db_password \
     postgres
   ```

   I secrets sono montati in `/run/secrets/` all'interno del container.

3. **Gestione di configurazioni sensibili in ambiente non-Swarm**:
   ```bash
   # Creare un file temporaneo
   echo "password123" > /tmp/password.txt
   
   # Montare il file come volume
   docker run -v /tmp/password.txt:/run/secrets/password:ro my-app
   
   # Rimuovere il file dopo l'uso
   rm /tmp/password.txt
   ```

#### Gestione di configurazioni in ambienti diversi

1. **Utilizzo di immagini diverse per ambienti diversi**:
   ```bash
   docker build -t my-app:dev -f Dockerfile.dev .
   docker build -t my-app:prod -f Dockerfile.prod .
   ```

2. **Utilizzo di variabili d'ambiente per distinguere gli ambienti**:
   ```bash
   # Sviluppo
   docker run -e NODE_ENV=development my-app
   
   # Produzione
   docker run -e NODE_ENV=production my-app
   ```

3. **Utilizzo di file di configurazione specifici per ambiente**:
   ```bash
   # Sviluppo
   docker run -v /path/to/config.dev.json:/app/config.json my-app
   
   # Produzione
   docker run -v /path/to/config.prod.json:/app/config.json my-app
   ```

#### Best practices per la configurazione di applicazioni containerizzate

1. **Seguire il principio "12-factor app"**:
   - Memorizzare la configurazione nell'ambiente
   - Separare configurazione dal codice
   - Utilizzare variabili d'ambiente per le configurazioni

2. **Evitare credenziali hardcoded**:
   - Non includere password o chiavi API nel codice o nel Dockerfile
   - Utilizzare variabili d'ambiente o secrets

3. **Fornire valori predefiniti sensati**:
   - Implementare fallback per le configurazioni mancanti
   - Esempio: `const port = process.env.PORT || 3000;`

4. **Validare le configurazioni all'avvio**:
   - Verificare che tutte le configurazioni necessarie siano presenti
   - Terminare con un errore chiaro se mancano configurazioni critiche

5. **Utilizzare file di configurazione per configurazioni complesse**:
   - Preferire JSON o YAML per configurazioni strutturate
   - Montare i file come volumi

6. **Centralizzare la gestione delle configurazioni**:
   - Utilizzare un modulo dedicato per caricare e validare le configurazioni
   - Esempio in Node.js:
     ```javascript
     // config.js
     module.exports = {
       port: parseInt(process.env.PORT || '3000', 10),
       dbUrl: process.env.DB_URL || 'mongodb://localhost:27017/myapp',
       logLevel: process.env.LOG_LEVEL || 'info',
       
       validate() {
         if (!this.dbUrl) throw new Error('DB_URL is required');
       }
     };
     ```

### Ottimizzazione delle immagini Docker

#### Tecniche avanzate di multi-stage build

1. **Separazione di dipendenze di build e runtime**:
   ```dockerfile
   # Stage di build
   FROM node:18 AS build
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

2. **Utilizzo di stage intermedi per la compilazione**:
   ```dockerfile
   # Stage di build delle dipendenze
   FROM node:18 AS deps
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   
   # Stage di build dell'applicazione
   FROM node:18 AS builder
   WORKDIR /app
   COPY --from=deps /app/node_modules ./node_modules
   COPY . .
   RUN npm run build
   
   # Stage di produzione
   FROM node:18-alpine
   WORKDIR /app
   COPY --from=builder /app/dist ./dist
   COPY --from=deps /app/node_modules ./node_modules
   CMD ["node", "dist/index.js"]
   ```

3. **Riutilizzo di stage per il caching**:
   ```dockerfile
   # Stage di base
   FROM node:18-alpine AS base
   WORKDIR /app
   
   # Stage di dipendenze
   FROM base AS dependencies
   COPY package*.json ./
   RUN npm install
   
   # Stage di build
   FROM dependencies AS build
   COPY . .
   RUN npm run build
   
   # Stage di produzione
   FROM base AS production
   COPY --from=dependencies /app/node_modules ./node_modules
   COPY --from=build /app/dist ./dist
   CMD ["node", "dist/index.js"]
   ```

#### Riduzione della dimensione delle immagini

1. **Utilizzare immagini base leggere**:
   - Alpine Linux: `node:18-alpine` (~50MB) vs `node:18` (~900MB)
   - Distroless: `gcr.io/distroless/nodejs` (solo runtime, senza shell)

2. **Rimuovere file non necessari**:
   ```dockerfile
   RUN npm install && \
       npm cache clean --force && \
       rm -rf /tmp/*
   ```

3. **Utilizzare .dockerignore**:
   ```
   node_modules
   npm-debug.log
   .git
   .env
   *.md
   tests/
   docs/
   ```

4. **Minimizzare il numero di layer**:
   - Combinare comandi correlati
   - Utilizzare `&&` per concatenare comandi

5. **Comprimere i file statici**:
   ```dockerfile
   RUN find /app/dist -type f -name "*.js" -exec gzip -k {} \;
   ```

6. **Utilizzare strumenti di analisi delle immagini**:
   ```bash
   docker images --format "{{.Repository}}:{{.Tag}} {{.Size}}"
   docker history my-image:latest
   ```

#### Sicurezza delle immagini

1. **Utilizzare utenti non root**:
   ```dockerfile
   # Creare un utente non privilegiato
   RUN addgroup -g 1000 appuser && \
       adduser -u 1000 -G appuser -s /bin/sh -D appuser
   
   # Cambiare proprietario dei file
   RUN chown -R appuser:appuser /app
   
   # Passare all'utente non privilegiato
   USER appuser
   ```

2. **Scansionare le immagini per vulnerabilità**:
   ```bash
   # Utilizzare Docker Scout
   docker scout cves my-image:latest
   ```

3. **Mantenere le immagini aggiornate**:
   - Aggiornare regolarmente le immagini base
   - Ricostruire le immagini per includere le patch di sicurezza

4. **Minimizzare la superficie di attacco**:
   - Installare solo i pacchetti necessari
   - Rimuovere strumenti di sviluppo e debug
   - Utilizzare immagini minimaliste

5. **Firmare le immagini**:
   ```bash
   # Firmare un'immagine
   docker trust sign my-image:latest
   ```

#### Strategie di caching

1. **Ordinare le istruzioni nel Dockerfile**:
   - Posizionare le istruzioni che cambiano meno frequentemente all'inizio
   - Esempio: installare dipendenze prima di copiare il codice sorgente

2. **Utilizzare la cache in modo selettivo**:
   ```bash
   # Disabilitare la cache per una build specifica
   docker build --no-cache .
   
   # Disabilitare la cache per un'istruzione specifica
   RUN --no-cache apt-get update && apt-get install -y package
   ```

3. **Utilizzare BuildKit**:
   ```bash
   # Abilitare BuildKit
   export DOCKER_BUILDKIT=1
   
   # Utilizzare la sintassi mount per la cache
   RUN --mount=type=cache,target=/var/cache/apt \
       apt-get update && apt-get install -y package
   ```

4. **Utilizzare la cache del registry**:
   ```bash
   # Utilizzare la cache da un'immagine precedente
   docker build --cache-from my-image:previous .
   ```

### Esercizi pratici guidati

#### Esercizio 1: Implementazione di configurazioni tramite variabili d'ambiente

**Passo 1**: Creare un'applicazione Node.js configurabile
```bash
mkdir -p /home/studente/env-config-app
cd /home/studente/env-config-app
```

**Passo 2**: Creare un file `package.json`
```bash
cat > package.json << 'EOF'
{
  "name": "env-config-app",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}
EOF
```

**Passo 3**: Creare un file `app.js` che utilizza variabili d'ambiente
```bash
cat > app.js << 'EOF'
const express = require('express');
const app = express();

// Configurazione tramite variabili d'ambiente
const config = {
  port: parseInt(process.env.PORT || '3000', 10),
  environment: process.env.NODE_ENV || 'development',
  logLevel: process.env.LOG_LEVEL || 'info',
  greeting: process.env.GREETING || 'Hello, World!'
};

// Validazione della configurazione
if (!['development', 'production', 'test'].includes(config.environment)) {
  console.error(`Errore: NODE_ENV non valido: ${config.environment}`);
  process.exit(1);
}

// Endpoint che mostra la configurazione
app.get('/', (req, res) => {
  res.json({
    message: config.greeting,
    environment: config.environment,
    logLevel: config.logLevel
  });
});

// Endpoint di health check
app.get('/health', (req, res) => {
  res.status(200).send('OK');
});

// Avvio del server
app.listen(config.port, '0.0.0.0', () => {
  console.log(`Server in esecuzione in ambiente ${config.environment} sulla porta ${config.port}`);
  console.log(`Livello di log: ${config.logLevel}`);
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

# Configurazione predefinita
ENV NODE_ENV=production
ENV PORT=3000
ENV LOG_LEVEL=info
ENV GREETING="Hello from Docker!"

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -q -O- http://localhost:3000/health || exit 1

CMD ["node", "app.js"]
EOF
```

**Passo 5**: Costruire l'immagine
```bash
docker build -t env-config-app:1.0 .
```

**Passo 6**: Eseguire il container con configurazione predefinita
```bash
docker run -d --name app-default -p 3000:3000 env-config-app:1.0
```

**Passo 7**: Verificare la configurazione predefinita
```bash
curl http://localhost:3000
```

**Passo 8**: Eseguire il container con configurazione personalizzata
```bash
docker run -d --name app-custom \
  -p 3001:3000 \
  -e NODE_ENV=development \
  -e LOG_LEVEL=debug \
  -e GREETING="Ciao dal container personalizzato!" \
  env-config-app:1.0
```

**Passo 9**: Verificare la configurazione personalizzata
```bash
curl http://localhost:3001
```

**Passo 10**: Utilizzare un file .env
```bash
cat > .env << 'EOF'
NODE_ENV=test
PORT=3000
LOG_LEVEL=warn
GREETING=Configurazione da file .env
EOF

docker run -d --name app-env-file \
  -p 3002:3000 \
  --env-file .env \
  env-config-app:1.0
```

**Passo 11**: Verificare la configurazione da file .env
```bash
curl http://localhost:3002
```

#### Esercizio 2: Ottimizzazione di un'immagine Docker esistente

**Passo 1**: Creare un'applicazione web con dipendenze
```bash
mkdir -p /home/studente/optimize-app
cd /home/studente/optimize-app
```

**Passo 2**: Creare un file `package.json` con molte dipendenze
```bash
cat > package.json << 'EOF'
{
  "name": "optimize-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "start": "node dist/server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "compression": "^1.7.4",
    "cors": "^2.8.5",
    "helmet": "^6.0.1"
  },
  "devDependencies": {
    "webpack": "^5.76.0",
    "webpack-cli": "^5.0.1",
    "typescript": "^5.0.2",
    "@types/express": "^4.17.17",
    "@types/compression": "^1.7.2",
    "@types/cors": "^2.8.13"
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

**Passo 4**: Creare un file `webpack.config.js`
```bash
cat > webpack.config.js << 'EOF'
const path = require('path');

module.exports = {
  entry: './src/server.ts',
  target: 'node',
  mode: 'production',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'server.js'
  },
  resolve: {
    extensions: ['.ts', '.js']
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  }
};
EOF
```

**Passo 5**: Creare la directory `src` e il file `server.ts`
```bash
mkdir -p src
cat > src/server.ts << 'EOF'
import express from 'express';
import compression from 'compression';
import cors from 'cors';
import helmet from 'helmet';

const app = express();
const port = process.env.PORT || 3000;

// Middleware
app.use(helmet());
app.use(compression());
app.use(cors());
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.json({ message: 'Server ottimizzato!' });
});

app.get('/health', (req, res) => {
  res.status(200).send('OK');
});

// Start server
app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione sulla porta ${port}`);
});
EOF
```

**Passo 6**: Creare un Dockerfile non ottimizzato
```bash
cat > Dockerfile.original << 'EOF'
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
EOF
```

**Passo 7**: Costruire l'immagine non ottimizzata
```bash
docker build -t optimize-app:original -f Dockerfile.original .
```

**Passo 8**: Creare un Dockerfile ottimizzato con multi-stage build
```bash
cat > Dockerfile.optimized << 'EOF'
# Stage di build
FROM node:18-alpine AS builder

WORKDIR /app

# Installare solo le dipendenze necessarie per il build
COPY package*.json ./
RUN npm install

# Copiare e compilare il codice sorgente
COPY . .
RUN npm run build

# Stage di produzione
FROM node:18-alpine

WORKDIR /app

# Copiare solo i file necessari per l'esecuzione
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/dist ./dist

# Installare solo le dipendenze di produzione
RUN npm install --only=production && \
    npm cache clean --force

# Utilizzare un utente non root
RUN addgroup -g 1000 appuser && \
    adduser -u 1000 -G appuser -s /bin/sh -D appuser && \
    chown -R appuser:appuser /app

USER appuser

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -q -O- http://localhost:3000/health || exit 1

CMD ["node", "dist/server.js"]
EOF
```

**Passo 9**: Costruire l'immagine ottimizzata
```bash
docker build -t optimize-app:optimized -f Dockerfile.optimized .
```

**Passo 10**: Confrontare le dimensioni delle immagini
```bash
docker images optimize-app
```

**Passo 11**: Eseguire entrambe le immagini
```bash
docker run -d --name app-original -p 3000:3000 optimize-app:original
docker run -d --name app-optimized -p 3001:3000 optimize-app:optimized
```

**Passo 12**: Verificare il funzionamento
```bash
curl http://localhost:3000
curl http://localhost:3001
```

#### Esercizio 3: Analisi e miglioramento della sicurezza delle immagini

**Passo 1**: Creare un'applicazione con potenziali problemi di sicurezza
```bash
mkdir -p /home/studente/secure-app
cd /home/studente/secure-app
```

**Passo 2**: Creare un file `app.js` con credenziali hardcoded
```bash
cat > app.js << 'EOF'
const express = require('express');
const fs = require('fs');
const app = express();
const port = 3000;

// Credenziali hardcoded (problema di sicurezza)
const dbCredentials = {
  username: 'admin',
  password: 'password123',
  host: 'localhost',
  database: 'myapp'
};

app.get('/', (req, res) => {
  res.send('Applicazione con problemi di sicurezza');
});

// Endpoint vulnerabile (esecuzione di comandi)
app.get('/exec', (req, res) => {
  const cmd = req.query.cmd || 'echo "No command specified"';
  require('child_process').exec(cmd, (error, stdout, stderr) => {
    res.send(stdout);
  });
});

// Lettura di file senza validazione (path traversal)
app.get('/file', (req, res) => {
  const filename = req.query.name || 'default.txt';
  const content = fs.readFileSync(filename, 'utf8');
  res.send(content);
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione sulla porta ${port}`);
  console.log(`Credenziali DB: ${JSON.stringify(dbCredentials)}`);
});
EOF
```

**Passo 3**: Creare un Dockerfile non sicuro
```bash
cat > Dockerfile.insecure << 'EOF'
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
EOF
```

**Passo 4**: Creare un file `package.json`
```bash
cat > package.json << 'EOF'
{
  "name": "secure-app",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}
EOF
```

**Passo 5**: Costruire l'immagine non sicura
```bash
docker build -t secure-app:insecure -f Dockerfile.insecure .
```

**Passo 6**: Identificare i problemi di sicurezza
```bash
# Eseguire l'immagine non sicura
docker run -d --name app-insecure -p 3000:3000 secure-app:insecure

# Verificare i problemi
curl "http://localhost:3000/exec?cmd=ls%20-la"
curl "http://localhost:3000/file?name=app.js"
docker logs app-insecure
```

**Passo 7**: Creare una versione sicura dell'applicazione
```bash
cat > app.secure.js << 'EOF'
const express = require('express');
const path = require('path');
const app = express();
const port = process.env.PORT || 3000;

// Configurazione tramite variabili d'ambiente
const dbCredentials = {
  username: process.env.DB_USERNAME || 'default_user',
  password: process.env.DB_PASSWORD || 'default_password',
  host: process.env.DB_HOST || 'localhost',
  database: process.env.DB_DATABASE || 'myapp'
};

app.get('/', (req, res) => {
  res.send('Applicazione sicura');
});

// Endpoint sicuro (nessuna esecuzione di comandi)
app.get('/exec', (req, res) => {
  res.status(403).send('Operazione non consentita per motivi di sicurezza');
});

// Lettura di file con validazione
app.get('/file', (req, res) => {
  const filename = req.query.name;
  if (!filename) {
    return res.status(400).send('Nome file richiesto');
  }
  
  // Validazione del percorso
  const safePath = path.join(__dirname, 'public', filename);
  if (!safePath.startsWith(path.join(__dirname, 'public'))) {
    return res.status(403).send('Accesso negato');
  }
  
  try {
    const fs = require('fs');
    const content = fs.readFileSync(safePath, 'utf8');
    res.send(content);
  } catch (error) {
    res.status(404).send('File non trovato');
  }
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione sulla porta ${port}`);
  // Non loggare le credenziali
  console.log('Database configurato');
});
EOF
```

**Passo 8**: Creare una directory per i file pubblici
```bash
mkdir -p public
echo "Questo è un file pubblico" > public/test.txt
```

**Passo 9**: Creare un Dockerfile sicuro
```bash
cat > Dockerfile.secure << 'EOF'
# Stage di build
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Stage di produzione
FROM node:18-alpine

# Installare solo i pacchetti necessari
RUN apk add --no-cache dumb-init

WORKDIR /app

COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/app.secure.js ./app.js
COPY --from=builder /app/public ./public

# Creare un utente non privilegiato
RUN addgroup -g 1000 appuser && \
    adduser -u 1000 -G appuser -s /bin/sh -D appuser && \
    chown -R appuser:appuser /app

USER appuser

EXPOSE 3000

# Utilizzare dumb-init come process manager
ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "app.js"]
EOF
```

**Passo 10**: Costruire l'immagine sicura
```bash
docker build -t secure-app:secure -f Dockerfile.secure .
```

**Passo 11**: Eseguire l'immagine sicura con variabili d'ambiente
```bash
docker run -d --name app-secure \
  -p 3001:3000 \
  -e DB_USERNAME=secure_user \
  -e DB_PASSWORD=secure_password \
  -e DB_HOST=db.example.com \
  -e DB_DATABASE=securedb \
  secure-app:secure
```

**Passo 12**: Verificare la sicurezza
```bash
curl "http://localhost:3001/exec?cmd=ls%20-la"
curl "http://localhost:3001/file?name=test.txt"
curl "http://localhost:3001/file?name=../app.js"
docker logs app-secure
```

#### Esercizio 4: Benchmark delle prestazioni prima e dopo l'ottimizzazione

**Passo 1**: Creare un'applicazione per il benchmark
```bash
mkdir -p /home/studente/benchmark-app
cd /home/studente/benchmark-app
```

**Passo 2**: Creare un file `app.js` con operazioni intensive
```bash
cat > app.js << 'EOF'
const express = require('express');
const crypto = require('crypto');
const app = express();
const port = process.env.PORT || 3000;

// Funzione CPU-intensive
function hashPassword(password, iterations = 10000) {
  let hash = password;
  for (let i = 0; i < iterations; i++) {
    hash = crypto.createHash('sha256').update(hash).digest('hex');
  }
  return hash;
}

// Endpoint leggero
app.get('/', (req, res) => {
  res.json({ status: 'ok' });
});

// Endpoint CPU-intensive
app.get('/hash', (req, res) => {
  const password = req.query.password || 'default';
  const iterations = parseInt(req.query.iterations || '10000', 10);
  
  const start = process.hrtime();
  const hash = hashPassword(password, iterations);
  const end = process.hrtime(start);
  const duration = (end[0] * 1000 + end[1] / 1000000).toFixed(2);
  
  res.json({
    hash: hash.substring(0, 10) + '...',
    iterations,
    duration: `${duration} ms`
  });
});

// Endpoint memory-intensive
app.get('/memory', (req, res) => {
  const size = parseInt(req.query.size || '100', 10);
  const start = process.hrtime();
  
  // Allocare memoria
  const buffers = [];
  for (let i = 0; i < size; i++) {
    buffers.push(Buffer.alloc(1024 * 1024)); // 1MB per buffer
  }
  
  const end = process.hrtime(start);
  const duration = (end[0] * 1000 + end[1] / 1000000).toFixed(2);
  
  res.json({
    allocated: `${size} MB`,
    duration: `${duration} ms`
  });
  
  // Liberare memoria
  buffers.length = 0;
});

app.listen(port, '0.0.0.0', () => {
  console.log(`Server in esecuzione sulla porta ${port}`);
});
EOF
```

**Passo 3**: Creare un file `package.json`
```bash
cat > package.json << 'EOF'
{
  "name": "benchmark-app",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}
EOF
```

**Passo 4**: Creare un Dockerfile non ottimizzato
```bash
cat > Dockerfile.original << 'EOF'
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
EOF
```

**Passo 5**: Creare un Dockerfile ottimizzato
```bash
cat > Dockerfile.optimized << 'EOF'
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --only=production && \
    npm cache clean --force

COPY . .

# Ottimizzazioni per Node.js
ENV NODE_ENV=production
ENV NODE_OPTIONS="--max-old-space-size=256"

EXPOSE 3000

CMD ["node", "--optimize_for_size", "--gc_interval=100", "app.js"]
EOF
```

**Passo 6**: Costruire entrambe le immagini
```bash
docker build -t benchmark-app:original -f Dockerfile.original .
docker build -t benchmark-app:optimized -f Dockerfile.optimized .
```

**Passo 7**: Eseguire i container con limiti di risorse
```bash
# Container originale
docker run -d --name benchmark-original \
  -p 3000:3000 \
  --memory=512m \
  --cpus=0.5 \
  benchmark-app:original

# Container ottimizzato
docker run -d --name benchmark-optimized \
  -p 3001:3000 \
  --memory=512m \
  --cpus=0.5 \
  benchmark-app:optimized
```

**Passo 8**: Creare uno script per il benchmark
```bash
cat > benchmark.sh << 'EOF'
#!/bin/bash

# Funzione per eseguire il benchmark
run_benchmark() {
  local url=$1
  local endpoint=$2
  local name=$3
  
  echo "Benchmark $name - $endpoint"
  
  # Warm-up
  curl -s "$url$endpoint" > /dev/null
  
  # Test
  start=$(date +%s.%N)
  for i in {1..10}; do
    curl -s "$url$endpoint" > /dev/null
  done
  end=$(date +%s.%N)
  
  # Calcolo durata
  duration=$(echo "$end - $start" | bc)
  avg=$(echo "$duration / 10" | bc -l)
  
  echo "  Durata totale: $duration secondi"
  echo "  Media per richiesta: $avg secondi"
  echo ""
}

# Benchmark container originale
echo "=== Container Originale ==="
run_benchmark "http://localhost:3000" "/" "Originale - Endpoint leggero"
run_benchmark "http://localhost:3000" "/hash?iterations=5000" "Originale - Endpoint CPU-intensive"
run_benchmark "http://localhost:3000" "/memory?size=50" "Originale - Endpoint memory-intensive"

# Benchmark container ottimizzato
echo "=== Container Ottimizzato ==="
run_benchmark "http://localhost:3001" "/" "Ottimizzato - Endpoint leggero"
run_benchmark "http://localhost:3001" "/hash?iterations=5000" "Ottimizzato - Endpoint CPU-intensive"
run_benchmark "http://localhost:3001" "/memory?size=50" "Ottimizzato - Endpoint memory-intensive"
EOF

chmod +x benchmark.sh
```

**Passo 9**: Eseguire il benchmark
```bash
./benchmark.sh
```

**Passo 10**: Monitorare l'utilizzo delle risorse
```bash
docker stats benchmark-original benchmark-optimized --no-stream
```

**Passo 11**: Confrontare le dimensioni delle immagini
```bash
docker images benchmark-app
```

## Parte 3: Introduzione all'orchestrazione e preparazione al progetto finale

### Concetti base di orchestrazione

#### Cos'è l'orchestrazione di container

L'orchestrazione di container è il processo automatizzato di gestione, coordinamento e organizzazione di container in un ambiente distribuito. Include:

- Deployment di applicazioni multi-container
- Scaling automatico
- Load balancing
- Service discovery
- Gestione della configurazione
- Monitoraggio e health check
- Rollout e rollback di aggiornamenti
- Auto-healing (riavvio automatico di container falliti)

#### Panoramica degli strumenti di orchestrazione

1. **Docker Compose**:
   - Strumento semplice per definire e gestire applicazioni multi-container
   - Ideale per ambienti di sviluppo e test
   - Limitato a un singolo host

2. **Docker Swarm**:
   - Soluzione di orchestrazione nativa di Docker
   - Cluster di host Docker
   - Più semplice di Kubernetes ma meno potente
   - Utilizza la stessa API di Docker

3. **Kubernetes**:
   - Sistema di orchestrazione enterprise
   - Standard de facto per l'orchestrazione di container
   - Altamente scalabile e flessibile
   - Ecosistema ricco di strumenti e estensioni

4. **Altri strumenti**:
   - Amazon ECS (Elastic Container Service)
   - Azure Container Instances
   - Google Cloud Run
   - Nomad (HashiCorp)

#### Docker Compose come strumento di base

Docker Compose è uno strumento per definire e gestire applicazioni multi-container. Caratteristiche principali:

- Definizione dell'intera applicazione in un singolo file YAML
- Gestione del ciclo di vita di tutti i container come un'unica entità
- Creazione di reti e volumi isolati
- Gestione delle dipendenze tra servizi
- Variabili d'ambiente e sostituzione di valori
- Scaling di servizi individuali

#### Casi d'uso dell'orchestrazione

1. **Applicazioni microservizi**:
   - Gestione di molti servizi indipendenti
   - Comunicazione tra servizi
   - Deployment indipendente

2. **Applicazioni stateful**:
   - Database distribuiti
   - Sistemi di messaggistica
   - Cache distribuite

3. **Applicazioni ad alta disponibilità**:
   - Replica di servizi
   - Failover automatico
   - Load balancing

4. **Ambienti di sviluppo e test**:
   - Creazione di ambienti identici
   - Isolamento tra ambienti
   - Integrazione continua

5. **Deployment blue/green e canary**:
   - Aggiornamenti senza downtime
   - Test di nuove versioni su un sottoinsieme di utenti
   - Rollback rapido in caso di problemi

### Introduzione a Docker Compose

#### Sintassi del file docker-compose.yml

Il file `docker-compose.yml` è un file YAML che definisce i servizi, le reti e i volumi di un'applicazione:

```yaml
version: '3'  # Versione della sintassi

services:      # Definizione dei servizi (container)
  web:         # Nome del servizio
    image: nginx:alpine  # Immagine da utilizzare
    ports:     # Mapping delle porte
      - "80:80"
    volumes:   # Volumi
      - ./html:/usr/share/nginx/html
    environment:  # Variabili d'ambiente
      - NGINX_HOST=example.com
    depends_on:   # Dipendenze
      - api
    networks:     # Reti
      - frontend

  api:
    build:        # Build da Dockerfile
      context: ./api
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    networks:
      - frontend
      - backend

  db:
    image: postgres:13-alpine
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=secret
    networks:
      - backend

networks:       # Definizione delle reti
  frontend:
  backend:

volumes:        # Definizione dei volumi
  db-data:
```

#### Definizione di servizi, reti e volumi

**Servizi**:
- Rappresentano i container dell'applicazione
- Possono essere basati su immagini esistenti o build da Dockerfile
- Supportano tutte le opzioni di `docker run`

```yaml
services:
  web:
    image: nginx:alpine
    container_name: my-web  # Nome del container
    restart: always         # Politica di riavvio
    ports:
      - "80:80"             # HOST:CONTAINER
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      - NGINX_HOST=example.com
    env_file:               # File di variabili d'ambiente
      - ./web.env
    command: nginx -g 'daemon off;'  # Comando di avvio
    entrypoint: /docker-entrypoint.sh  # Entrypoint
```

**Reti**:
- Permettono la comunicazione tra container
- Possono essere configurate con driver e opzioni specifiche

```yaml
networks:
  frontend:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.28.0.0/16
          gateway: 172.28.0.1
  backend:
    driver: bridge
```

**Volumi**:
- Forniscono storage persistente per i container
- Possono essere volumi Docker o bind mount

```yaml
volumes:
  db-data:
    driver: local
  cache:
    driver_opts:
      type: tmpfs
      device: tmpfs
```

#### Gestione del ciclo di vita delle applicazioni

Comandi principali di Docker Compose:

```bash
# Avviare tutti i servizi
docker-compose up

# Avviare in background
docker-compose up -d

# Visualizzare i log
docker-compose logs

# Visualizzare lo stato dei servizi
docker-compose ps

# Fermare i servizi
docker-compose stop

# Fermare e rimuovere i container
docker-compose down

# Fermare, rimuovere container e volumi
docker-compose down -v

# Ricostruire le immagini
docker-compose build

# Eseguire un comando in un servizio
docker-compose exec web sh

# Riavviare un servizio
docker-compose restart web
```

#### Scaling dei servizi

Docker Compose permette di scalare i servizi orizzontalmente:

```bash
# Avviare 3 istanze del servizio web
docker-compose up -d --scale web=3
```

Considerazioni per lo scaling:
- Le porte pubblicate devono essere uniche o non specificate
- I nomi dei container non devono essere personalizzati
- I volumi devono essere condivisibili tra istanze

### Esercizi pratici guidati

#### Esercizio 1: Creazione di un file docker-compose.yml per un'applicazione multi-container

**Passo 1**: Creare una directory per il progetto
```bash
mkdir -p /home/studente/compose-app
cd /home/studente/compose-app
```

**Passo 2**: Creare una directory per l'API
```bash
mkdir -p api
cat > api/app.js << 'EOF'
const express = require('express');
const { MongoClient } = require('mongodb');
const redis = require('redis');
const app = express();
const port = process.env.PORT || 3000;

// Middleware
app.use(express.json());

// Configurazione MongoDB
const mongoUrl = process.env.MONGO_URL || 'mongodb://mongo:27017';
const mongoClient = new MongoClient(mongoUrl);
let db;

// Configurazione Redis
const redisClient = redis.createClient({
  url: process.env.REDIS_URL || 'redis://redis:6379'
});

// Connessione ai database
async function connectDB() {
  try {
    await mongoClient.connect();
    console.log('Connesso a MongoDB');
    db = mongoClient.db('compose-app');
    
    await redisClient.connect();
    console.log('Connesso a Redis');
  } catch (err) {
    console.error('Errore di connessione ai database:', err);
    setTimeout(connectDB, 5000);
  }
}

connectDB();

// Routes
app.get('/', (req, res) => {
  res.json({ message: 'API funzionante' });
});

// Endpoint per i contatori
app.get('/counter/:id', async (req, res) => {
  try {
    const id = req.params.id;
    const count = await redisClient.incr(`counter:${id}`);
    res.json({ id, count });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Endpoint per i messaggi
app.post('/messages', async (req, res) => {
  try {
    const message = {
      text: req.body.text,
      createdAt: new Date()
    };
    
    const result = await db.collection('messages').insertOne(message);
    res.status(201).json({ id: result.insertedId, ...message });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.get('/messages', async (req, res) => {
  try {
    const messages = await db.collection('messages').find().sort({ createdAt: -1 }).limit(10).toArray();
    res.json(messages);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Health check
app.get('/health', (req, res) => {
  res.status(200).send('OK');
});

// Avvio del server
app.listen(port, '0.0.0.0', () => {
  console.log(`API in esecuzione sulla porta ${port}`);
});
EOF
```

**Passo 3**: Creare un file `package.json` per l'API
```bash
cat > api/package.json << 'EOF'
{
  "name": "compose-api",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2",
    "mongodb": "^5.1.0",
    "redis": "^4.6.5"
  }
}
EOF
```

**Passo 4**: Creare un Dockerfile per l'API
```bash
cat > api/Dockerfile << 'EOF'
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -q -O- http://localhost:3000/health || exit 1

CMD ["node", "app.js"]
EOF
```

**Passo 5**: Creare una directory per il frontend
```bash
mkdir -p frontend
cat > frontend/index.html << 'EOF'
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Compose App</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 800px; margin: 0 auto; padding: 20px; }
    .message { background: #f9f9f9; padding: 10px; margin-bottom: 10px; border-radius: 5px; }
    .counter { background: #e9f7ef; padding: 10px; margin-bottom: 20px; border-radius: 5px; }
    form { margin-bottom: 20px; }
    input { padding: 8px; width: 70%; }
    button { padding: 8px 15px; background: #4CAF50; color: white; border: none; cursor: pointer; }
  </style>
</head>
<body>
  <h1>Compose App</h1>
  
  <div class="counter" id="counter">
    Contatore: <span id="count">0</span>
    <button id="increment">Incrementa</button>
  </div>
  
  <h2>Messaggi</h2>
  <form id="message-form">
    <input type="text" id="message-text" placeholder="Scrivi un messaggio..." required>
    <button type="submit">Invia</button>
  </form>
  
  <div id="messages-container"></div>

  <script>
    const API_URL = '/api';
    
    // Incrementa contatore
    document.getElementById('increment').addEventListener('click', async () => {
      try {
        const response = await fetch(`${API_URL}/counter/main`);
        const data = await response.json();
        document.getElementById('count').textContent = data.count;
      } catch (error) {
        console.error('Errore:', error);
      }
    });
    
    // Carica messaggi
    async function loadMessages() {
      try {
        const response = await fetch(`${API_URL}/messages`);
        const messages = await response.json();
        
        const container = document.getElementById('messages-container');
        container.innerHTML = '';
        
        messages.forEach(message => {
          const messageEl = document.createElement('div');
          messageEl.className = 'message';
          messageEl.textContent = message.text;
          container.appendChild(messageEl);
        });
      } catch (error) {
        console.error('Errore nel caricamento dei messaggi:', error);
      }
    }
    
    // Invia messaggio
    document.getElementById('message-form').addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const textInput = document.getElementById('message-text');
      const text = textInput.value;
      
      try {
        await fetch(`${API_URL}/messages`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ text }),
        });
        
        textInput.value = '';
        loadMessages();
      } catch (error) {
        console.error('Errore nell\'invio del messaggio:', error);
      }
    });
    
    // Carica contatore iniziale
    fetch(`${API_URL}/counter/main`)
      .then(response => response.json())
      .then(data => {
        document.getElementById('count').textContent = data.count;
      })
      .catch(error => console.error('Errore:', error));
    
    // Carica messaggi all'avvio
    loadMessages();
  </script>
</body>
</html>
EOF
```

**Passo 6**: Creare un Dockerfile per il frontend
```bash
cat > frontend/Dockerfile << 'EOF'
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -q -O- http://localhost/health || exit 1

CMD ["nginx", "-g", "daemon off;"]
EOF
```

**Passo 7**: Creare un file di configurazione Nginx per il frontend
```bash
cat > frontend/nginx.conf << 'EOF'
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://api:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /health {
        return 200 'OK';
        add_header Content-Type text/plain;
    }
}
EOF
```

**Passo 8**: Creare un file docker-compose.yml
```bash
cat > docker-compose.yml << 'EOF'
version: '3'

services:
  # Frontend service
  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - api
    networks:
      - frontend-network
    restart: unless-stopped

  # API service
  api:
    build: ./api
    environment:
      - MONGO_URL=mongodb://mongo:27017
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongo
      - redis
    networks:
      - frontend-network
      - backend-network
    restart: unless-stopped

  # MongoDB service
  mongo:
    image: mongo:5.0
    volumes:
      - mongo-data:/data/db
    networks:
      - backend-network
    restart: unless-stopped

  # Redis service
  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data
    networks:
      - backend-network
    restart: unless-stopped

networks:
  frontend-network:
  backend-network:

volumes:
  mongo-data:
  redis-data:
EOF
```

**Passo 9**: Avviare l'applicazione con Docker Compose
```bash
docker-compose up -d
```

**Passo 10**: Verificare che tutti i servizi siano in esecuzione
```bash
docker-compose ps
```

**Passo 11**: Verificare i log dei servizi
```bash
docker-compose logs
```

**Passo 12**: Accedere all'applicazione
```bash
echo "Apri http://localhost nel browser per utilizzare l'applicazione"
```

#### Esercizio 2: Gestione del ciclo di vita dell'applicazione con Docker Compose

**Passo 1**: Visualizzare i container in esecuzione
```bash
docker-compose ps
```

**Passo 2**: Visualizzare i log di un servizio specifico
```bash
docker-compose logs api
```

**Passo 3**: Seguire i log in tempo reale
```bash
docker-compose logs -f
```

**Passo 4**: Eseguire un comando in un container
```bash
docker-compose exec mongo mongo --eval "db.stats()"
```

**Passo 5**: Fermare un servizio specifico
```bash
docker-compose stop api
```

**Passo 6**: Verificare lo stato dei servizi
```bash
docker-compose ps
```

**Passo 7**: Avviare il servizio fermato
```bash
docker-compose start api
```

**Passo 8**: Riavviare tutti i servizi
```bash
docker-compose restart
```

**Passo 9**: Fermare l'applicazione senza rimuovere i container
```bash
docker-compose stop
```

**Passo 10**: Avviare l'applicazione
```bash
docker-compose start
```

**Passo 11**: Fermare e rimuovere i container
```bash
docker-compose down
```

**Passo 12**: Avviare l'applicazione preservando i dati
```bash
docker-compose up -d
```

**Passo 13**: Fermare e rimuovere i container e i volumi
```bash
docker-compose down -v
```

#### Esercizio 3: Scaling dei servizi e gestione del carico

**Passo 1**: Modificare il file docker-compose.yml per supportare lo scaling
```bash
cat > docker-compose.scale.yml << 'EOF'
version: '3'

services:
  # Load Balancer
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - frontend
    networks:
      - frontend-network
    restart: unless-stopped

  # Frontend service
  frontend:
    build: ./frontend
    expose:
      - "80"
    depends_on:
      - api
    networks:
      - frontend-network
    restart: unless-stopped

  # API service
  api:
    build: ./api
    expose:
      - "3000"
    environment:
      - MONGO_URL=mongodb://mongo:27017
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongo
      - redis
    networks:
      - frontend-network
      - backend-network
    restart: unless-stopped
    deploy:
      replicas: 1

  # MongoDB service
  mongo:
    image: mongo:5.0
    volumes:
      - mongo-data:/data/db
    networks:
      - backend-network
    restart: unless-stopped

  # Redis service
  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data
    networks:
      - backend-network
    restart: unless-stopped

networks:
  frontend-network:
  backend-network:

volumes:
  mongo-data:
  redis-data:
EOF
```

**Passo 2**: Creare un file di configurazione per Nginx come load balancer
```bash
cat > nginx.conf << 'EOF'
upstream frontend {
    server frontend:80;
}

upstream api {
    server api:3000;
}

server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://frontend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /api/ {
        proxy_pass http://api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /health {
        return 200 'OK';
        add_header Content-Type text/plain;
    }
}
EOF
```

**Passo 3**: Avviare l'applicazione con il nuovo file di configurazione
```bash
docker-compose -f docker-compose.scale.yml up -d
```

**Passo 4**: Scalare il servizio API
```bash
docker-compose -f docker-compose.scale.yml up -d --scale api=3
```

**Passo 5**: Verificare che le istanze siano in esecuzione
```bash
docker-compose -f docker-compose.scale.yml ps
```

**Passo 6**: Modificare il file di configurazione Nginx per il round-robin
```bash
cat > nginx.conf << 'EOF'
upstream frontend {
    server frontend:80;
}

upstream api {
    server api:3000;
    # Le istanze aggiuntive vengono rilevate automaticamente
}

server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://frontend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /api/ {
        proxy_pass http://api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /health {
        return 200 'OK';
        add_header Content-Type text/plain;
    }
}
EOF
```

**Passo 7**: Ricaricare la configurazione di Nginx
```bash
docker-compose -f docker-compose.scale.yml exec nginx nginx -s reload
```

**Passo 8**: Testare il load balancing
```bash
for i in {1..10}; do
  curl -s http://localhost/api | grep -o "API funzionante"
done
```

**Passo 9**: Scalare anche il frontend
```bash
docker-compose -f docker-compose.scale.yml up -d --scale frontend=2
```

**Passo 10**: Verificare che tutte le istanze siano in esecuzione
```bash
docker-compose -f docker-compose.scale.yml ps
```

#### Esercizio 4: Debugging di applicazioni multi-container

**Passo 1**: Creare un file docker-compose con un errore intenzionale
```bash
cat > docker-compose.debug.yml << 'EOF'
version: '3'

services:
  # Frontend service con errore nella porta
  frontend:
    build: ./frontend
    ports:
      - "8080:8080"  # Errore: la porta interna è 80, non 8080
    depends_on:
      - api
    networks:
      - app-network
    restart: unless-stopped

  # API service con errore nell'URL di MongoDB
  api:
    build: ./api
    ports:
      - "3000:3000"
    environment:
      - MONGO_URL=mongodb://mongodb:27017  # Errore: il nome del servizio è 'mongo', non 'mongodb'
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongo
      - redis
    networks:
      - app-network
    restart: unless-stopped

  # MongoDB service
  mongo:
    image: mongo:5.0
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network
    restart: unless-stopped

  # Redis service
  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:

volumes:
  mongo-data:
  redis-data:
EOF
```

**Passo 2**: Avviare l'applicazione con errori
```bash
docker-compose -f docker-compose.debug.yml up -d
```

**Passo 3**: Verificare lo stato dei servizi
```bash
docker-compose -f docker-compose.debug.yml ps
```

**Passo 4**: Esaminare i log per identificare gli errori
```bash
docker-compose -f docker-compose.debug.yml logs
```

**Passo 5**: Correggere l'errore della porta del frontend
```bash
cat > docker-compose.debug.fixed.yml << 'EOF'
version: '3'

services:
  # Frontend service con porta corretta
  frontend:
    build: ./frontend
    ports:
      - "8080:80"  # Corretto: la porta interna è 80
    depends_on:
      - api
    networks:
      - app-network
    restart: unless-stopped

  # API service con errore nell'URL di MongoDB
  api:
    build: ./api
    ports:
      - "3000:3000"
    environment:
      - MONGO_URL=mongodb://mongodb:27017  # Ancora errato
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongo
      - redis
    networks:
      - app-network
    restart: unless-stopped

  # MongoDB service
  mongo:
    image: mongo:5.0
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network
    restart: unless-stopped

  # Redis service
  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:

volumes:
  mongo-data:
  redis-data:
EOF
```

**Passo 6**: Aggiornare l'applicazione con la prima correzione
```bash
docker-compose -f docker-compose.debug.yml down
docker-compose -f docker-compose.debug.fixed.yml up -d
```

**Passo 7**: Verificare lo stato e i log
```bash
docker-compose -f docker-compose.debug.fixed.yml ps
docker-compose -f docker-compose.debug.fixed.yml logs api
```

**Passo 8**: Utilizzare exec per debuggare l'API
```bash
docker-compose -f docker-compose.debug.fixed.yml exec api sh
# All'interno del container
ping mongo
ping mongodb
exit
```

**Passo 9**: Correggere l'errore dell'URL di MongoDB
```bash
cat > docker-compose.debug.fixed2.yml << 'EOF'
version: '3'

services:
  # Frontend service
  frontend:
    build: ./frontend
    ports:
      - "8080:80"
    depends_on:
      - api
    networks:
      - app-network
    restart: unless-stopped

  # API service con URL corretto
  api:
    build: ./api
    ports:
      - "3000:3000"
    environment:
      - MONGO_URL=mongodb://mongo:27017  # Corretto: il nome del servizio è 'mongo'
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongo
      - redis
    networks:
      - app-network
    restart: unless-stopped

  # MongoDB service
  mongo:
    image: mongo:5.0
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network
    restart: unless-stopped

  # Redis service
  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:

volumes:
  mongo-data:
  redis-data:
EOF
```

**Passo 10**: Aggiornare l'applicazione con la seconda correzione
```bash
docker-compose -f docker-compose.debug.fixed.yml down
docker-compose -f docker-compose.debug.fixed2.yml up -d
```

**Passo 11**: Verificare che l'applicazione funzioni correttamente
```bash
docker-compose -f docker-compose.debug.fixed2.yml ps
curl http://localhost:3000
curl http://localhost:8080
```

**Passo 12**: Utilizzare docker-compose config per validare il file
```bash
docker-compose -f docker-compose.debug.fixed2.yml config
```

### Preparazione al progetto finale

#### Discussione dell'architettura del progetto finale

Il progetto finale consisterà in un'infrastruttura ibrida che combina VM e container per implementare una semplice applicazione web distribuita. L'architettura sarà composta da:

1. **VM Alpine Linux**:
   - Host per i container Docker
   - Gestione della rete e della sicurezza
   - Punto di accesso SSH

2. **Container Docker**:
   - Frontend: Interfaccia utente dell'applicazione
   - Backend: API per la logica di business
   - Database: Persistenza dei dati
   - Cache: Miglioramento delle prestazioni

3. **Componenti dell'applicazione**:
   - Applicazione web Node.js
   - API RESTful
   - Database MongoDB
   - Cache Redis

4. **Networking**:
   - Rete interna per la comunicazione tra container
   - Esposizione selettiva di servizi all'esterno

#### Identificazione dei componenti necessari

1. **Componenti software**:
   - Docker e Docker Compose sulla VM Alpine
   - Node.js per l'applicazione web e l'API
   - MongoDB per la persistenza dei dati
   - Redis per la cache
   - Nginx come reverse proxy e load balancer

2. **Configurazione della VM**:
   - Rete NAT per l'accesso a Internet
   - Port forwarding per l'accesso ai servizi
   - Configurazione SSH per l'accesso remoto

3. **Configurazione Docker**:
   - Reti Docker per l'isolamento dei servizi
   - Volumi per la persistenza dei dati
   - Politiche di riavvio per la resilienza

4. **Strumenti di monitoraggio e gestione**:
   - Log centralizzati
   - Health check per i servizi
   - Script di backup e ripristino

#### Pianificazione dell'implementazione

1. **Fase 1: Configurazione dell'ambiente**
   - Preparazione della VM Alpine
   - Installazione di Docker e Docker Compose
   - Configurazione della rete e della sicurezza

2. **Fase 2: Sviluppo dei componenti**
   - Creazione dell'applicazione frontend
   - Implementazione dell'API backend
   - Configurazione del database e della cache
   - Sviluppo dei Dockerfile per ogni componente

3. **Fase 3: Orchestrazione con Docker Compose**
   - Definizione del file docker-compose.yml
   - Configurazione delle reti e dei volumi
   - Impostazione delle dipendenze tra servizi
   - Configurazione delle variabili d'ambiente

4. **Fase 4: Deployment e test**
   - Avvio dell'infrastruttura completa
   - Test di funzionalità e prestazioni
   - Verifica della resilienza e del failover
   - Ottimizzazione della configurazione

5. **Fase 5: Documentazione**
   - Documentazione dell'architettura
   - Guida all'installazione e configurazione
   - Procedure operative standard
   - Troubleshooting e FAQ

#### Considerazioni su VM e container nell'infrastruttura ibrida

1. **Vantaggi dell'approccio ibrido**:
   - Isolamento a livello di VM per la sicurezza
   - Flessibilità e scalabilità dei container
   - Gestione semplificata dell'infrastruttura
   - Ottimizzazione delle risorse

2. **Sfide dell'approccio ibrido**:
   - Complessità della gestione
   - Overhead di risorse della VM
   - Configurazione della rete tra VM e container
   - Backup e ripristino dell'intero sistema

3. **Best practices**:
   - Mantenere la VM leggera e focalizzata
   - Utilizzare Docker Compose per l'orchestrazione
   - Implementare monitoraggio e logging centralizzati
   - Automatizzare il deployment e la configurazione
   - Implementare strategie di backup per dati critici

4. **Considerazioni di sicurezza**:
   - Isolamento dei container in reti separate
   - Limitazione dei privilegi dei container
   - Aggiornamenti regolari delle immagini
   - Scansione delle vulnerabilità
   - Hardening della VM host

## Verifiche di apprendimento

### Quiz sui concetti di networking avanzato e orchestrazione

1. Quali sono i tipi di reti disponibili in Docker e quando è appropriato utilizzare ciascuno?
2. Come funziona la risoluzione DNS tra container in una rete Docker personalizzata?
3. Quali sono i vantaggi dell'utilizzo di Docker Compose rispetto ai comandi Docker individuali?
4. Come si gestiscono le dipendenze tra servizi in Docker Compose?
5. Quali sono le strategie per implementare la persistenza dei dati in un'applicazione containerizzata?

### Revisione delle configurazioni create

Analizzare le configurazioni create durante l'esercitazione e discutere:
1. Efficacia della struttura dei file docker-compose.yml
2. Gestione delle dipendenze tra servizi
3. Configurazione delle reti e dei volumi
4. Gestione delle variabili d'ambiente e dei secrets
5. Strategie di scaling e load balancing

### Valutazione delle ottimizzazioni applicate alle immagini

Discutere le ottimizzazioni applicate alle immagini Docker:
1. Efficacia delle tecniche di multi-stage build
2. Riduzione della dimensione delle immagini
3. Miglioramento della sicurezza
4. Ottimizzazione delle prestazioni
5. Strategie di caching

### Discussione di gruppo sull'architettura del progetto finale

Discutere in gruppo i seguenti aspetti del progetto finale:
1. Architettura complessiva dell'infrastruttura ibrida
2. Scelta dei componenti e delle tecnologie
3. Strategie di deployment e scaling
4. Gestione della persistenza dei dati
5. Monitoraggio e troubleshooting

## Conclusioni e preparazione per il progetto finale

Nella terza esercitazione abbiamo:
- Configurato reti Docker avanzate
- Implementato comunicazione tra container
- Gestito configurazioni con variabili d'ambiente
- Ottimizzato immagini Docker
- Introdotto concetti di orchestrazione con Docker Compose
- Preparato il terreno per il progetto finale

Nel progetto finale applicheremo tutte le conoscenze acquisite per:
- Progettare un'infrastruttura ibrida (VM + container)
- Implementare una semplice applicazione web distribuita
- Configurare la comunicazione tra componenti
- Documentare l'architettura e le scelte implementative

**Compito per casa (facoltativo)**: Iniziare a progettare l'architettura del progetto finale, identificando i componenti necessari e le loro interazioni.
