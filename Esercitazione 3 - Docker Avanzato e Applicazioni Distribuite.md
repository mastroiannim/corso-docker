# Esercitazione 3: Docker Avanzato e Applicazioni Distribuite

## Panoramica dell'Esercitazione
Questa terza esercitazione di 3 ore è progettata per approfondire aspetti avanzati di Docker, come il networking avanzato, l'ottimizzazione delle immagini, la gestione delle configurazioni e un'introduzione all'orchestrazione con Docker Compose. Questa esercitazione si basa sulle conoscenze acquisite nelle sessioni introduttive e nelle prime due esercitazioni su Docker.

## Parte 1: Networking Avanzato in Docker

### Approfondimento sui Driver di Rete

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate i diversi tipi di reti che abbiamo visto nella Sessione Introduttiva 2? Docker implementa questi concetti attraverso i driver di rete.

#### Driver Bridge:
- Driver predefinito
- Crea una rete isolata sull'host
- Permette la comunicazione tra container sulla stessa rete
- Supporta port mapping per l'accesso dall'esterno
- Esempio avanzato:
  ```bash
  docker network create --driver bridge \
    --subnet=172.28.0.0/16 \
    --ip-range=172.28.5.0/24 \
    --gateway=172.28.0.1 \
    custom-bridge
  ```

#### Driver Host:
- Usa direttamente lo stack di rete dell'host
- Nessun isolamento di rete
- Migliori prestazioni
- Potenziali conflitti di porte
- Esempio:
  ```bash
  docker run --network host nginx
  ```

#### Driver Overlay:
- Per comunicazione tra container su host diversi
- Utilizzato principalmente con Docker Swarm
- Incapsula i pacchetti in VXLAN
- Esempio:
  ```bash
  docker network create --driver overlay \
    --attachable \
    --subnet=10.0.9.0/24 \
    multi-host-network
  ```

#### Driver Macvlan:
- Assegna indirizzi MAC ai container
- Appare come dispositivo fisico sulla rete
- Utile per migrare applicazioni legacy
- Esempio:
  ```bash
  docker network create --driver macvlan \
    --subnet=192.168.32.0/24 \
    --gateway=192.168.32.1 \
    -o parent=eth0 \
    macvlan-network
  ```

#### Driver None:
- Nessuna connettività di rete
- Container completamente isolato
- Utile per elaborazioni batch
- Esempio:
  ```bash
  docker run --network none alpine
  ```

### Configurazione Avanzata del Networking

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato di configurazione avanzata delle reti. Vediamo come applicare questi concetti in Docker.

#### Indirizzi IP Statici:
```bash
docker run --network custom-bridge --ip 172.28.5.100 nginx
```

#### DNS Personalizzato:
```bash
docker run --dns 8.8.8.8 --dns 8.8.4.4 nginx
```

#### Hostname e Entry DNS:
```bash
docker run --hostname mycontainer --add-host db:192.168.1.10 nginx
```

#### Controllo della Connettività:
```bash
# Disabilitare la pubblicazione di porte
docker run --network-alias db --publish-all=false mysql

# Limitare le interfacce per il port mapping
docker run -p 127.0.0.1:80:80 nginx
```

### Comunicazione tra Container su Host Diversi

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Ricordate i concetti di routing e gateway che abbiamo visto nella Sessione Introduttiva 2? Questi concetti sono fondamentali per la comunicazione tra container su host diversi.

#### Opzioni per la Comunicazione Multi-Host:
1. **Docker Swarm:**
   - Modalità cluster nativa di Docker
   - Reti overlay gestite automaticamente
   - Esempio:
     ```bash
     # Inizializzare Swarm sul primo host
     docker swarm init --advertise-addr <IP>
     
     # Creare una rete overlay
     docker network create --driver overlay swarm-network
     
     # Eseguire servizi sulla rete
     docker service create --network swarm-network --name web nginx
     ```

2. **Overlay Network in Modalità Standalone:**
   - Richiede un key-value store esterno (es. Consul)
   - Configurazione più complessa
   - Maggiore flessibilità

3. **Macvlan su Più Host:**
   - Container appaiono direttamente sulla rete fisica
   - Richiede configurazione di rete appropriata
   - Esempio:
     ```bash
     # Su host1
     docker network create --driver macvlan \
       --subnet=192.168.1.0/24 \
       --gateway=192.168.1.1 \
       -o parent=eth0 \
       macvlan-net
     
     # Su host2 (stessa configurazione)
     docker network create --driver macvlan \
       --subnet=192.168.1.0/24 \
       --gateway=192.168.1.1 \
       -o parent=eth0 \
       macvlan-net
     ```

4. **Port Mapping e Proxy Inverso:**
   - Esporre servizi tramite port mapping
   - Utilizzare un proxy inverso (es. Nginx, Traefik)
   - Soluzione più semplice ma meno efficiente

### Sicurezza delle Reti Docker

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato dell'importanza della sicurezza nelle reti. Vediamo come applicare questi principi alle reti Docker.

#### Isolamento delle Reti:
- Creare reti separate per diversi ambienti o applicazioni
- Limitare la comunicazione tra reti diverse
- Esempio:
  ```bash
  docker network create frontend
  docker network create backend
  
  # Frontend può accedere solo alla rete frontend
  docker run --network frontend frontend-app
  
  # Backend può accedere a entrambe le reti
  docker run --network backend backend-app
  docker network connect frontend backend-app
  ```

#### Controllo degli Accessi:
- Limitare l'esposizione delle porte
- Utilizzare reti interne per servizi non pubblici
- Esempio:
  ```bash
  docker network create --internal private-network
  docker run --network private-network db
  ```

#### Crittografia del Traffico:
- Utilizzare TLS per la comunicazione tra container
- Implementare HTTPS per servizi esposti
- Esempio con Nginx e certificati SSL:
  ```bash
  docker run -v $(pwd)/certs:/etc/nginx/certs \
    -v $(pwd)/nginx.conf:/etc/nginx/conf.d/default.conf \
    -p 443:443 \
    nginx
  ```

#### Monitoraggio e Logging:
- Registrare il traffico di rete
- Implementare sistemi di rilevamento delle intrusioni
- Utilizzare strumenti come tcpdump o Wireshark

## Parte 2: Configurazione e Ottimizzazione

### Configurazione Avanzata di Docker

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i file di configurazione che abbiamo visto nella Sessione Introduttiva 1? Docker utilizza file di configurazione simili per personalizzare il suo comportamento.

#### File di Configurazione del Daemon:
- Percorso: `/etc/docker/daemon.json`
- Formato JSON
- Esempio:
  ```json
  {
    "storage-driver": "overlay2",
    "log-driver": "json-file",
    "log-opts": {
      "max-size": "10m",
      "max-file": "3"
    },
    "default-address-pools": [
      {"base": "172.30.0.0/16", "size": 24}
    ],
    "registry-mirrors": ["https://mirror.example.com"]
  }
  ```

#### Opzioni Comuni:
- `storage-driver`: Driver di storage (overlay2, devicemapper, ...)
- `log-driver`: Driver per i log (json-file, syslog, journald, ...)
- `default-ulimits`: Limiti di risorse predefiniti
- `insecure-registries`: Registry non sicuri
- `registry-mirrors`: Mirror per Docker Hub

#### Applicazione delle Modifiche:
```bash
sudo systemctl restart docker
```

### Gestione delle Risorse

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Nella Sessione Introduttiva 1 abbiamo parlato della gestione delle risorse del sistema. Docker permette di limitare e monitorare le risorse utilizzate dai container.

#### Limiti di CPU:
```bash
# Limitare a 0.5 CPU
docker run --cpus=0.5 nginx

# Assegnare peso relativo
docker run --cpu-shares=512 nginx

# Specificare i core CPU
docker run --cpuset-cpus="0,1" nginx
```

#### Limiti di Memoria:
```bash
# Limitare a 512MB
docker run --memory=512m nginx

# Limitare lo swap
docker run --memory=512m --memory-swap=1g nginx

# Riservare memoria
docker run --memory-reservation=256m nginx
```

#### Limiti di I/O:
```bash
# Limitare IOPS
docker run --device-read-iops=/dev/sda:1000 --device-write-iops=/dev/sda:1000 nginx

# Limitare velocità di trasferimento
docker run --device-read-bps=/dev/sda:10mb --device-write-bps=/dev/sda:10mb nginx
```

#### Monitoraggio delle Risorse:
```bash
# Statistiche in tempo reale
docker stats

# Statistiche di un container specifico
docker stats container_name

# Ispezionare i limiti configurati
docker inspect container_name
```

### Ottimizzazione delle Immagini

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato dell'ottimizzazione delle applicazioni. Vediamo come ottimizzare le immagini Docker per ridurre dimensioni e migliorare le prestazioni.

#### Tecniche di Ottimizzazione:
1. **Utilizzare Immagini Base Leggere:**
   - Alpine Linux invece di Ubuntu/Debian
   - Varianti "slim" delle immagini ufficiali
   - Immagini "scratch" per binari statici
   - Esempio:
     ```dockerfile
     FROM node:14-alpine
     # invece di
     # FROM node:14
     ```

2. **Multi-stage Builds:**
   - Separare build e runtime
   - Copiare solo i file necessari
   - Esempio:
     ```dockerfile
     # Stage di build
     FROM golang:1.16 AS builder
     WORKDIR /app
     COPY . .
     RUN go build -o myapp
     
     # Stage finale
     FROM alpine:3.14
     COPY --from=builder /app/myapp /usr/local/bin/
     CMD ["myapp"]
     ```

3. **Minimizzare i Layer:**
   - Combinare comandi RUN correlati
   - Pulire cache e file temporanei nello stesso layer
   - Esempio:
     ```dockerfile
     RUN apt-get update && \
         apt-get install -y --no-install-recommends package1 package2 && \
         apt-get clean && \
         rm -rf /var/lib/apt/lists/*
     ```

4. **Utilizzare .dockerignore:**
   - Escludere file non necessari dal contesto di build
   - Ridurre la dimensione del contesto
   - Esempio:
     ```
     node_modules
     npm-debug.log
     .git
     .env
     *.md
     ```

5. **Ottimizzare l'Ordine delle Istruzioni:**
   - Posizionare le istruzioni che cambiano meno frequentemente all'inizio
   - Sfruttare la cache di Docker
   - Esempio:
     ```dockerfile
     # Raramente cambia
     FROM node:14-alpine
     WORKDIR /app
     
     # Cambia solo quando cambiano le dipendenze
     COPY package*.json ./
     RUN npm ci
     
     # Cambia frequentemente
     COPY . .
     ```

### Logging e Monitoraggio

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato dell'importanza del monitoraggio dei servizi. Docker offre diverse opzioni per il logging e il monitoraggio dei container.

#### Driver di Logging:
- **json-file:** Driver predefinito, salva i log in file JSON
- **syslog:** Invia i log al syslog dell'host
- **journald:** Invia i log al journal di systemd
- **fluentd:** Invia i log a Fluentd
- **awslogs:** Invia i log ad Amazon CloudWatch
- **splunk:** Invia i log a Splunk

#### Configurazione del Logging:
```bash
# Specificare il driver di logging
docker run --log-driver=syslog nginx

# Configurare opzioni di logging
docker run --log-driver=json-file \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  nginx
```

#### Configurazione Globale:
```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

#### Strumenti di Monitoraggio:
- **Docker Stats:** Monitoraggio base delle risorse
- **cAdvisor:** Monitoraggio dettagliato dei container
- **Prometheus + Grafana:** Monitoraggio avanzato e visualizzazione
- **Datadog, New Relic, Dynatrace:** Soluzioni commerciali

#### Esempio di Implementazione con cAdvisor:
```bash
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

## Parte 3: Introduzione all'Orchestrazione con Docker Compose

### Approfondimento su Docker Compose

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Ricordate i concetti di automazione che abbiamo visto nella Sessione Introduttiva 1? Docker Compose automatizza la gestione di applicazioni multi-container.

#### Caratteristiche Avanzate:
- Gestione del ciclo di vita dell'applicazione
- Variabili d'ambiente e sostituzione
- Estensione e override di configurazioni
- Dipendenze e healthcheck
- Scaling dei servizi

#### Versioni del File Compose:
- Versione 2: Supporto per dipendenze, volumi denominati
- Versione 3: Supporto per Docker Swarm, deploy options
- Differenze principali tra le versioni

### Configurazione Avanzata di Docker Compose

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 1:** Nella Sessione Introduttiva 1 abbiamo parlato della configurazione avanzata dei servizi. Vediamo come applicare questi concetti in Docker Compose.

#### Dipendenze e Ordine di Avvio:
```yaml
services:
  web:
    image: nginx
    depends_on:
      - api
      - db
  
  api:
    image: api-service
    depends_on:
      - db
  
  db:
    image: postgres
```

#### Healthcheck:
```yaml
services:
  db:
    image: postgres
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
```

#### Variabili d'Ambiente:
```yaml
services:
  web:
    image: webapp
    environment:
      - NODE_ENV=production
      - API_URL=http://api:3000
    env_file:
      - ./config/web.env
```

#### Sostituzione di Variabili:
```yaml
services:
  web:
    image: webapp
    environment:
      - SERVICE_NAME=${SERVICE_PREFIX:-web}_service
      - API_URL=${API_URL:-http://localhost:3000}
```

#### Estensione e Override:
```yaml
# docker-compose.yml
services:
  web:
    image: webapp
    ports:
      - "80:80"

# docker-compose.override.yml
services:
  web:
    environment:
      - DEBUG=true
    volumes:
      - ./src:/app/src
```

### Scaling e Deployment

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato di scalabilità dei servizi. Docker Compose permette di scalare i servizi in modo semplice.

#### Scaling Manuale:
```bash
docker-compose up -d --scale service=3
```

#### Configurazione per lo Scaling:
```yaml
services:
  worker:
    image: worker-app
    deploy:
      replicas: 3
    configs:
      - source: worker_config
      - target: /etc/worker/config.yml

configs:
  worker_config:
    file: ./worker-config.yml
```

#### Considerazioni per lo Scaling:
- Gestione delle porte (evitare conflitti)
- Persistenza dei dati
- Bilanciamento del carico
- Stato condiviso

#### Esempio con Bilanciamento del Carico:
```yaml
services:
  lb:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - web
  
  web:
    image: webapp
    expose:
      - "8080"
    deploy:
      replicas: 3
```

### Gestione di Ambienti Multipli

#### Richiamo alle Sessioni Introduttive:
- **Collegamento con la Sessione 2:** Nella Sessione Introduttiva 2 abbiamo parlato della gestione di ambienti diversi. Docker Compose permette di gestire configurazioni per ambienti diversi.

#### File Compose per Ambienti Diversi:
- `docker-compose.yml`: Configurazione base
- `docker-compose.override.yml`: Override automatico (sviluppo)
- `docker-compose.prod.yml`: Configurazione di produzione
- `docker-compose.test.yml`: Configurazione di test

#### Esempio di Struttura:
```
project/
├── docker-compose.yml          # Base comune
├── docker-compose.override.yml # Sviluppo (automatico)
├── docker-compose.prod.yml     # Produzione
├── docker-compose.test.yml     # Test
└── .env                        # Variabili d'ambiente
```

#### Utilizzo:
```bash
# Sviluppo (usa automaticamente docker-compose.override.yml)
docker-compose up -d

# Produzione
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# Test
docker-compose -f docker-compose.yml -f docker-compose.test.yml up -d
```

#### Esempio di Configurazioni:
```yaml
# docker-compose.yml (base)
serv
(Content truncated due to size limit. Use line ranges to read in chunks)