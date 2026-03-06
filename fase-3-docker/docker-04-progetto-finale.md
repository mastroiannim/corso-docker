## 📍 Navigazione

**Fase**: Finale  |  **Modulo**: 4  |  **Tipo**: Progetto

### ✅ Prerequisiti

Prima di iniziare questo progetto devi aver completato:
- [ ] [Esercitazione 1 — Introduzione a Docker](docker-01-intro-concetti-base.md)
- [ ] [Esercitazione 2 — Container, Volumi e Networking](docker-02-container-volumi-networking.md)
- [ ] [Esercitazione 3 — Docker Avanzato e Compose](docker-03-avanzato-compose-orchestrazione.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Progettare un'architettura ibrida VM + container
- Creare Dockerfile personalizzati per ogni servizio
- Orchestrare un'applicazione multi-servizio con Docker Compose
- Testare e verificare il funzionamento dell'intera infrastruttura

---

# Progetto Finale: Infrastruttura Ibrida VM + Container

## Panoramica del Progetto

In questo progetto finale progetterai e implementerai un'**applicazione web distribuita** composta da tre servizi (frontend, backend, database) eseguiti come container Docker su una VM Alpine Linux in VirtualBox. Questo progetto integra tutte le conoscenze acquisite durante il corso.

### Scenario

Dovrai realizzare un'applicazione web composta da:

| Servizio | Tecnologia | Ruolo |
|----------|-----------|-------|
| **Frontend** | Nginx (Alpine) | Serve pagine HTML statiche e fa da reverse proxy verso il backend |
| **Backend** | Node.js (Alpine) o Python (Alpine) | API REST che gestisce la logica applicativa |
| **Database** | PostgreSQL (Alpine) | Archivia i dati dell'applicazione |

### Architettura

```
┌──────────────────────────────────────────────────┐
│               VM Alpine Linux (VirtualBox)        │
│                                                    │
│  ┌─────────┐    ┌─────────┐    ┌──────────────┐  │
│  │ Frontend │───▶│ Backend │───▶│   Database    │  │
│  │  Nginx   │    │ Node.js │    │  PostgreSQL   │  │
│  │ :8080    │    │ :3000   │    │  :5432        │  │
│  └─────────┘    └─────────┘    └──────────────┘  │
│       │                              │            │
│       │         Docker Network       │            │
│       └──────────────────────────────┘            │
│                                                    │
│  Volume: db-data (persistenza PostgreSQL)          │
└──────────────────────────────────────────────────┘
         │
    Host (Windows 11)
    Browser → http://localhost:8080
```

---

## Step 1 — Disegno dell'Architettura (30 min)

Prima di scrivere codice, disegna su carta o su un editor l'architettura della tua applicazione.

### Cosa definire

1. **Servizi**: quali container servono e quale immagine base usano
2. **Rete**: come comunicano i container tra loro
3. **Porte**: quali porte esponi verso l'host
4. **Volumi**: dove servono dati persistenti
5. **Dipendenze**: in che ordine devono avviarsi i servizi

### ✅ Checkpoint

Hai completato questo step se puoi rispondere a queste domande:
- Quanti container servono?
- Quale rete Docker userai?
- Quali porte saranno esposte sull'host?
- Dove verranno salvati i dati persistenti?

---

## Step 2 — Configurazione Rete VirtualBox (15 min)

Configura la VM Alpine Linux per essere raggiungibile dall'host.

### Opzione A: Port Forwarding con NAT (consigliato)

```
VirtualBox → Impostazioni → Rete → Avanzate → Port Forwarding
```

| Protocollo | Porta Host | Porta Guest |
|-----------|-----------|------------|
| TCP | 8080 | 8080 |

### Opzione B: Bridge Adapter

Seleziona "Bridge Adapter" nelle impostazioni di rete della VM. La VM otterrà un IP sulla stessa rete dell'host.

### ✅ Checkpoint

Verifica la connettività dalla VM:

```bash
ip addr show
ping -c 2 8.8.8.8
```

**Output atteso**: la VM ha un indirizzo IP e può raggiungere Internet.

---

## Step 3 — Creazione dei Dockerfile (45 min)

Crea una directory di progetto e i Dockerfile per ogni servizio.

### Struttura del progetto

```bash
mkdir -p ~/progetto-finale/{frontend,backend,db}
cd ~/progetto-finale
```

### 3.1 — Frontend (Nginx)

Crea il file `frontend/index.html`:

```html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Progetto Finale - Corso Docker</title>
</head>
<body>
    <h1>Progetto Finale</h1>
    <p>Applicazione web distribuita con Docker</p>
    <div id="result"></div>
    <script>
        fetch('/api/health')
            .then(r => r.json())
            .then(data => {
                document.getElementById('result').textContent =
                    'Backend: ' + data.status + ' | DB: ' + data.db;
            })
            .catch(e => {
                document.getElementById('result').textContent = 'Errore: ' + e;
            });
    </script>
</body>
</html>
```

Crea il file `frontend/nginx.conf`:

```nginx
server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }

    location /api/ {
        proxy_pass http://backend:3000/;
    }
}
```

Crea il file `frontend/Dockerfile`:

```dockerfile
FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

### 3.2 — Backend (Node.js)

Crea il file `backend/package.json`:

```json
{
  "name": "backend",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.0",
    "pg": "^8.11.0"
  }
}
```

Crea il file `backend/server.js`:

```javascript
const express = require('express');
const { Pool } = require('pg');

const app = express();
const port = 3000;

const pool = new Pool({
    host: process.env.DB_HOST || 'db',
    port: process.env.DB_PORT || 5432,
    database: process.env.DB_NAME || 'appdb',
    user: process.env.DB_USER || 'appuser',
    password: process.env.DB_PASSWORD || 'apppassword'
});

app.get('/health', async (req, res) => {
    try {
        const result = await pool.query('SELECT NOW()');
        res.json({
            status: 'ok',
            db: 'connesso',
            timestamp: result.rows[0].now
        });
    } catch (err) {
        res.status(500).json({
            status: 'ok',
            db: 'errore: ' + err.message
        });
    }
});

app.listen(port, () => {
    console.log(`Backend in ascolto sulla porta ${port}`);
});
```

Crea il file `backend/Dockerfile`:

```dockerfile
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install --production
COPY server.js .
EXPOSE 3000
CMD ["node", "server.js"]
```

### 3.3 — Database (PostgreSQL)

Crea il file `db/init.sql`:

```sql
CREATE TABLE IF NOT EXISTS visite (
    id SERIAL PRIMARY KEY,
    timestamp TIMESTAMPTZ DEFAULT NOW()
);
```

### ✅ Checkpoint

Verifica che tutti i file siano stati creati:

```bash
find ~/progetto-finale -type f | sort
```

**Output atteso**:
```
/root/progetto-finale/backend/Dockerfile
/root/progetto-finale/backend/package.json
/root/progetto-finale/backend/server.js
/root/progetto-finale/db/init.sql
/root/progetto-finale/frontend/Dockerfile
/root/progetto-finale/frontend/index.html
/root/progetto-finale/frontend/nginx.conf
```

---

## Step 4 — Scrittura del `docker-compose.yml` (30 min)

Crea il file `docker-compose.yml` nella directory `~/progetto-finale`:

```yaml
services:
  frontend:
    build: ./frontend
    ports:
      - "8080:80"
    depends_on:
      - backend
    networks:
      - app-network

  backend:
    build: ./backend
    environment:
      - DB_HOST=db
      - DB_PORT=5432
      - DB_NAME=appdb
      - DB_USER=appuser
      - DB_PASSWORD=apppassword
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: postgres:alpine
    environment:
      - POSTGRES_DB=appdb
      - POSTGRES_USER=appuser
      - POSTGRES_PASSWORD=apppassword
    volumes:
      - db-data:/var/lib/postgresql/data
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  db-data:
```

### ✅ Checkpoint

Verifica la sintassi del file:

```bash
docker compose config
```

**Output atteso**: il file viene visualizzato senza errori, con tutti i servizi espansi.

---

## Step 5 — Test di Integrazione (30 min)

### 5.1 — Avvio dell'applicazione

```bash
cd ~/progetto-finale
docker compose up -d --build
```

### 5.2 — Verifica stato container

```bash
docker compose ps
```

**Output atteso**: tutti e tre i servizi devono essere `Up` (running).

### 5.3 — Test connettività frontend

```bash
curl http://localhost:8080
```

**Output atteso**: il contenuto HTML della pagina principale.

### 5.4 — Test API backend

```bash
curl http://localhost:8080/api/health
```

**Output atteso**: un JSON con `"status": "ok"` e `"db": "connesso"`.

### 5.5 — Test database

```bash
docker compose exec db psql -U appuser -d appdb -c "SELECT NOW();"
```

**Output atteso**: la data e ora corrente, confermando che il database è attivo.

### 5.6 — Verifica persistenza dati

```bash
# Ferma e riavvia
docker compose down
docker compose up -d

# Verifica che i dati del database siano ancora presenti
docker compose exec db psql -U appuser -d appdb -c "\dt"
```

**Output atteso**: la tabella `visite` è ancora presente dopo il riavvio.

### 5.7 — Verifica log

```bash
docker compose logs --tail=10
```

**Output atteso**: nessun errore critico nei log dei tre servizi.

---

## Step 6 — Ottimizzazione (30 min)

### 6.1 — Multi-stage build per il backend

Modifica `backend/Dockerfile` per ridurre la dimensione dell'immagine:

```dockerfile
# Stage 1: installazione dipendenze
FROM node:alpine AS builder
WORKDIR /app
COPY package.json .
RUN npm install --production

# Stage 2: immagine finale
FROM node:alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY server.js .
EXPOSE 3000
CMD ["node", "server.js"]
```

### 6.2 — Variabili di ambiente con file `.env`

Crea il file `.env` nella directory del progetto:

```env
DB_NAME=appdb
DB_USER=appuser
DB_PASSWORD=apppassword
DB_PORT=5432
```

Aggiorna `docker-compose.yml` per usare il file `.env`:

```yaml
  backend:
    build: ./backend
    environment:
      - DB_HOST=db
      - DB_PORT=${DB_PORT}
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
```

### 6.3 — Verifica dimensione immagini

```bash
docker images | grep progetto
```

### ✅ Checkpoint

```bash
# Rebuild e test
docker compose up -d --build
curl http://localhost:8080/api/health
```

**Output atteso**: `{"status":"ok","db":"connesso",...}` — l'applicazione funziona ancora dopo l'ottimizzazione.

---

## Checklist Finale di Valutazione

Prima di consegnare, verifica ogni punto:

| # | Criterio | Comando di Verifica | ✅ |
|---|----------|--------------------|----|
| 1 | La VM Alpine è configurata e raggiungibile | `ping -c 2 <IP-VM>` | ☐ |
| 2 | Docker e Docker Compose sono installati | `docker --version && docker compose version` | ☐ |
| 3 | Tutti e 3 i container si avviano senza errori | `docker compose ps` (tutti `Up`) | ☐ |
| 4 | Il frontend è raggiungibile dal browser | `curl http://localhost:8080` | ☐ |
| 5 | Il backend risponde alle richieste API | `curl http://localhost:8080/api/health` | ☐ |
| 6 | Il database è connesso e funzionante | `docker compose exec db psql -U appuser -d appdb -c "SELECT 1;"` | ☐ |
| 7 | I dati del database persistono dopo un riavvio | `docker compose down && docker compose up -d` + query | ☐ |
| 8 | La rete Docker è configurata correttamente | `docker network inspect progetto-finale_app-network` | ☐ |
| 9 | I Dockerfile usano immagini Alpine | `docker images \| grep progetto` | ☐ |
| 10 | Il `docker-compose.yml` è valido | `docker compose config` (nessun errore) | ☐ |

---

## Consegna

### Cosa consegnare

1. **Directory del progetto** con tutti i file (`docker-compose.yml`, Dockerfile, codice sorgente)
2. **Screenshot** dei test superati (output dei comandi della checklist)
3. **Breve relazione** (max 1 pagina) che descriva:
   - L'architettura scelta e le motivazioni
   - Le difficoltà incontrate e come le hai risolte
   - Cosa cambieresti se dovessi rifare il progetto

### Come consegnare

```bash
# Crea un archivio del progetto
cd ~
tar -czf progetto-finale.tar.gz progetto-finale/
```

Consegna il file `progetto-finale.tar.gz` e la relazione al docente.

---

## ➡️ Prossimi passi

- **Approfondisci**: [Docker Compose Documentation](https://docs.docker.com/compose/)
- **Esplora**: [Docker Hub](https://hub.docker.com/) per immagini alpine di altri servizi
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice principale](../README.md)
