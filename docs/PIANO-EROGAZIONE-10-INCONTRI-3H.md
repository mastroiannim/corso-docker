# Piano di Erogazione Studenti — 10 lezioni da 3 ore

> Corso: Docker e Containerizzazione  
> Target: classe quarta ITI informatica (17-18 anni)  
> Ambiente: Windows 11 + VM Alpine Linux su VirtualBox

Questo documento è il percorso guidato in ordine cronologico: per ogni lezione trovi cosa fare, quali comandi provare in laboratorio e il link diretto al modulo del corso.

## Indice cronologico (10 lezioni x 3h)

1. [Lezione 1 — Setup ambiente](#lezione-1-3h--setup-ambiente)
2. [Lezione 2 — Fondamenti OS, filesystem e shell](#lezione-2-3h--fondamenti-os-filesystem-e-shell)
3. [Lezione 3 — Shell avanzata + Lab 1](#lezione-3-3h--shell-avanzata--lab-1)
4. [Lezione 4 — Reti essenziali + diagnostica](#lezione-4-3h--reti-essenziali--diagnostica)
5. [Lezione 5 — VirtualBox networking applicato](#lezione-5-3h--virtualbox-networking-applicato)
6. [Lezione 6 — Docker 1: basi operative](#lezione-6-3h--docker-1-basi-operative)
7. [Lezione 7 — Docker 2: immagini e persistenza](#lezione-7-3h--docker-2-immagini-e-persistenza)
8. [Lezione 8 — Docker 3: Compose e orchestrazione base](#lezione-8-3h--docker-3-compose-e-orchestrazione-base)
9. [Lezione 9 — Progetto finale: implementazione](#lezione-9-3h--progetto-finale-implementazione)
10. [Lezione 10 — Progetto finale: test e consegna](#lezione-10-3h--progetto-finale-test-e-consegna)

---

<a id="lezione-1-3h--setup-ambiente"></a>
## Lezione 1 (3h) — Setup ambiente

**Modulo di riferimento**: [Setup completo ambiente](../fase-0-prerequisiti/00-lab-setup-ambiente.md)

**Scansione (3h)**
- 0:00–0:30 → VirtualBox + ISO Alpine
- 0:30–1:30 → creazione VM + installazione Alpine
- 1:30–2:15 → rete + SSH
- 2:15–3:00 → strumenti minimi + test finale

**Nota metodologica**: SSH viene introdotto subito perché, nella configurazione base di VirtualBox, il copia/incolla host↔guest non è disponibile senza Guest Additions.

### Tutorial guidato (laboratorio)
- `hostname` / `ip a` → verifica stato iniziale VM (rif. setup rete e host)
- `setup-alpine` → installazione guidata Alpine
- `rc-service sshd status` → verifica servizio SSH
- `apk update && apk add docker docker-compose openssh git bash` → tool base corso
- `docker --version` e `docker run hello-world` → checkpoint finale

**Link rapidi**
- [Parte 4: Installazione Alpine](../fase-0-prerequisiti/00-lab-setup-ambiente.md)
- [Parte 5: Accesso SSH e rete](../fase-0-prerequisiti/00-lab-setup-ambiente.md)

**Navigazione lezioni**: [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 2 →](#lezione-2-3h--fondamenti-os-filesystem-e-shell)

---

<a id="lezione-2-3h--fondamenti-os-filesystem-e-shell"></a>
## Lezione 2 (3h) — Fondamenti OS, filesystem e shell

**Moduli di riferimento**
- [Teoria sessione 1](../fase-1-introduzione/intro-01-teoria-os-filesystem-shell.md)
- [1a — Sistemi operativi](../fase-1-introduzione/intro-01a-sistemi-operativi.md)
- [1b — Filesystem](../fase-1-introduzione/intro-01b-filesystem.md)
- [1c — Shell base](../fase-1-introduzione/intro-01c-shell-base.md)

**Scansione (3h)**
- 0:00–1:15 → moduli 1a + 1b
- 1:15–2:30 → modulo 1c (comandi base)
- 2:30–3:00 → checkpoint finale

### Tutorial guidato (laboratorio)
- `pwd` / `ls -la` / `cd` → orientamento nel filesystem
- `mkdir laboratorio && cd laboratorio` → creazione area di lavoro
- `touch note.txt && echo "ciao" > note.txt` → file base e redirect
- `cat note.txt` / `cp note.txt copia.txt` / `mv copia.txt archivio.txt` → gestione file
- `rm archivio.txt` / `rmdir ..` (solo dove consentito) → pulizia controllata

**Link rapidi**
- [Filesystem: percorsi assoluti/relativi](../fase-1-introduzione/intro-01b-filesystem.md)
- [Shell base: navigazione e manipolazione file](../fase-1-introduzione/intro-01c-shell-base.md)

**Navigazione lezioni**: [← Lezione 1](#lezione-1-3h--setup-ambiente) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 3 →](#lezione-3-3h--shell-avanzata--lab-1)

---

<a id="lezione-3-3h--shell-avanzata--lab-1"></a>
## Lezione 3 (3h) — Shell avanzata + Lab 1

**Moduli di riferimento**
- [1d — Shell avanzata](../fase-1-introduzione/intro-01d-shell-avanzata.md)
- [1e — Recap e connessioni](../fase-1-introduzione/intro-01e-recap-connessioni.md)
- [Lab 1 — OS/Filesystem/Shell](../fase-1-introduzione/intro-01-lab-os-filesystem-shell.md)

**Scansione (3h)**
- 0:00–0:45 → shell avanzata
- 0:45–1:15 → recap verso Docker
- 1:15–3:00 → lab 1 (parti 2, 3, 4)

### Tutorial guidato (laboratorio)
- `echo "uno due tre" | tr ' ' '\n'` → prima pipeline
- `ls -la > elenco.txt` / `cat elenco.txt` → redirezione output
- `find . -name "*.txt"` → ricerca file
- `grep -R "docker" .` → ricerca contenuti
- `cat elenco.txt | grep "^d"` → combinazione comandi

**Link rapidi**
- [Shell avanzata: pipe, find, grep](../fase-1-introduzione/intro-01d-shell-avanzata.md)
- [Lab 1: parti operative](../fase-1-introduzione/intro-01-lab-os-filesystem-shell.md)

**Navigazione lezioni**: [← Lezione 2](#lezione-2-3h--fondamenti-os-filesystem-e-shell) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 4 →](#lezione-4-3h--reti-essenziali--diagnostica)

---

<a id="lezione-4-3h--reti-essenziali--diagnostica"></a>
## Lezione 4 (3h) — Reti essenziali + diagnostica

**Moduli di riferimento**
- [Teoria reti e servizi](../fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md)
- [Lab 2 — reti, applicazioni e servizi](../fase-2-reti-servizi/intro-02-lab-reti-servizi-vm.md)

**Scansione (3h)**
- 0:00–0:45 → teoria essenziale rete/client-server
- 0:45–2:30 → lab 2 (parti 1, 2, 3)
- 2:30–3:00 → checkpoint rete

### Tutorial guidato (laboratorio)
- `ip a` / `ip route` → lettura configurazione rete
- `ping -c 4 8.8.8.8` → test raggiungibilità
- `curl http://example.com` → test HTTP client
- `ss -tuln` → porte e servizi in ascolto
- `nc -l -p 8080` (server) + `nc <IP_VM> 8080` (client) → prova client-server

**Link rapidi**
- [Teoria: rete essenziale e diagnostica](../fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md)
- [Lab 2: parti 1-3](../fase-2-reti-servizi/intro-02-lab-reti-servizi-vm.md)

**Navigazione lezioni**: [← Lezione 3](#lezione-3-3h--shell-avanzata--lab-1) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 5 →](#lezione-5-3h--virtualbox-networking-applicato)

---

<a id="lezione-5-3h--virtualbox-networking-applicato"></a>
## Lezione 5 (3h) — VirtualBox networking applicato

**Moduli di riferimento**
- [Rete VirtualBox (NAT/Bridge/Host-Only)](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)
- [Lab 2 — parte 5](../fase-2-reti-servizi/intro-02-lab-reti-servizi-vm.md)

**Scansione (3h)**
- 0:00–1:00 → NAT vs Bridge vs Host-Only
- 1:00–2:15 → connettività e port forwarding
- 2:15–3:00 → troubleshooting guidato

### Tutorial guidato (laboratorio)
- `ip a` / `ip route` → verifica rete dopo cambio modalità VirtualBox
- `ping -c 4 <gateway>` → controllo gateway
- `python3 -m http.server 8080` → test servizio locale
- `ss -tuln | grep 8080` → verifica ascolto porta
- `curl http://<IP_VM>:8080` (da host) → verifica esposizione servizio

**Link rapidi**
- [Scenario NAT](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)
- [Scenario Bridge](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)
- [Scenario Host-Only](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)

**Navigazione lezioni**: [← Lezione 4](#lezione-4-3h--reti-essenziali--diagnostica) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 6 →](#lezione-6-3h--docker-1-basi-operative)

---

<a id="lezione-6-3h--docker-1-basi-operative"></a>
## Lezione 6 (3h) — Docker 1: basi operative

**Modulo di riferimento**: [Docker 01 — Introduzione e concetti base](../fase-3-docker/docker-01-intro-concetti-base.md)

**Scansione (3h)**
- 0:00–1:00 → concetti container e architettura Docker
- 1:00–2:30 → esercizi pratici 1–4
- 2:30–3:00 → esercizio 5 + debrief

### Tutorial guidato (laboratorio)
- `docker --version` / `docker info` → verifica motore Docker
- `docker run hello-world` → primo container
- `docker ps -a` → stato container
- `docker images` / `docker pull nginx:alpine` → gestione immagini
- `docker run -d --name web1 -p 8080:80 nginx:alpine` + `docker logs web1` → container in background

**Link rapidi**
- [Installazione e configurazione Docker](../fase-3-docker/docker-01-intro-concetti-base.md)
- [Primi passi con Docker](../fase-3-docker/docker-01-intro-concetti-base.md)

**Navigazione lezioni**: [← Lezione 5](#lezione-5-3h--virtualbox-networking-applicato) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 7 →](#lezione-7-3h--docker-2-immagini-e-persistenza)

---

<a id="lezione-7-3h--docker-2-immagini-e-persistenza"></a>
## Lezione 7 (3h) — Docker 2: immagini e persistenza

**Modulo di riferimento**: [Docker 02 — container, volumi, networking](../fase-3-docker/docker-02-container-volumi-networking.md)

**Scansione (3h)**
- 0:00–1:15 → Dockerfile e build
- 1:15–2:15 → volumi e bind mount
- 2:15–3:00 → networking base tra container

### Tutorial guidato (laboratorio)
- `docker build -t app-demo:1.0 .` → build immagine custom
- `docker run --rm app-demo:1.0` → esecuzione rapida
- `docker volume create dati_app` → creazione volume
- `docker run -d --name dbtest -v dati_app:/var/lib/postgresql/data postgres:16-alpine` → persistenza
- `docker network create rete_lab` + `docker network ls` → rete personalizzata

**Link rapidi**
- [Creazione immagini Docker](../fase-3-docker/docker-02-container-volumi-networking.md)
- [Persistenza dati e volumi](../fase-3-docker/docker-02-container-volumi-networking.md)
- [Networking e distribuzione](../fase-3-docker/docker-02-container-volumi-networking.md)

**Navigazione lezioni**: [← Lezione 6](#lezione-6-3h--docker-1-basi-operative) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 8 →](#lezione-8-3h--docker-3-compose-e-orchestrazione-base)

---

<a id="lezione-8-3h--docker-3-compose-e-orchestrazione-base"></a>
## Lezione 8 (3h) — Docker 3: Compose e orchestrazione base

**Modulo di riferimento**: [Docker 03 — avanzato, compose, orchestrazione](../fase-3-docker/docker-03-avanzato-compose-orchestrazione.md)

**Scansione (3h)**
- 0:00–0:45 → networking avanzato essenziale
- 0:45–2:15 → Docker Compose (definizione stack)
- 2:15–3:00 → preparazione progetto finale

### Tutorial guidato (laboratorio)
- `docker compose version` → verifica plugin Compose
- `docker compose up -d` → avvio stack multi-servizio
- `docker compose ps` / `docker compose logs --tail=50` → stato e log
- `docker compose exec <servizio> sh` → ispezione container
- `docker compose down` → stop e cleanup stack

**Link rapidi**
- [Networking avanzato](../fase-3-docker/docker-03-avanzato-compose-orchestrazione.md)
- [Introduzione all'orchestrazione con Docker Compose](../fase-3-docker/docker-03-avanzato-compose-orchestrazione.md)

**Navigazione lezioni**: [← Lezione 7](#lezione-7-3h--docker-2-immagini-e-persistenza) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 9 →](#lezione-9-3h--progetto-finale-implementazione)

---

<a id="lezione-9-3h--progetto-finale-implementazione"></a>
## Lezione 9 (3h) — Progetto finale: implementazione

**Modulo di riferimento**: [Docker 04 — progetto finale](../fase-3-docker/docker-04-progetto-finale.md)

**Scansione (3h)**
- 0:00–0:30 → Step 1–2 (architettura + rete)
- 0:30–2:15 → Step 3–4 (Dockerfile + compose)
- 2:15–3:00 → checkpoint tecnico intermedio

### Tutorial guidato (laboratorio)
- `mkdir progetto-finale && cd progetto-finale` → avvio workspace progetto
- `docker compose config` → validazione sintassi compose
- `docker compose up -d --build` → build + avvio applicazione
- `docker compose ps` / `docker images` → verifica stato risorse
- `curl http://localhost` e `curl http://localhost:3000/health` → verifica frontend/backend

**Link rapidi**
- [Step 1-2: architettura e rete](../fase-3-docker/docker-04-progetto-finale.md)
- [Step 3-4: Dockerfile e docker-compose](../fase-3-docker/docker-04-progetto-finale.md)

**Navigazione lezioni**: [← Lezione 8](#lezione-8-3h--docker-3-compose-e-orchestrazione-base) | [Indice](#indice-cronologico-10-lezioni-x-3h) | [Lezione 10 →](#lezione-10-3h--progetto-finale-test-e-consegna)

---

<a id="lezione-10-3h--progetto-finale-test-e-consegna"></a>
## Lezione 10 (3h) — Progetto finale: test e consegna

**Moduli di riferimento**
- [Docker 04 — progetto finale](../fase-3-docker/docker-04-progetto-finale.md)
- [Cheatsheet comandi](../CHEATSHEET.md)

**Scansione (3h)**
- 0:00–1:15 → Step 5 (test integrazione)
- 1:15–2:15 → Step 6 (ottimizzazione minima)
- 2:15–3:00 → checklist finale + consegna

### Tutorial guidato (laboratorio)
- `docker compose ps` / `docker compose logs --tail=100` → verifica operativa completa
- `docker exec -it <container_db> psql -U <utente> -d <db>` → test database
- `docker image ls` / `docker history <immagine>` → controllo immagini
- `docker system df` → verifica utilizzo spazio
- `docker compose down` e pacchetto consegna (repo + file richiesti)

**Link rapidi**
- [Step 5: test di integrazione](../fase-3-docker/docker-04-progetto-finale.md)
- [Step 6: ottimizzazione](../fase-3-docker/docker-04-progetto-finale.md)
- [Checklist finale e consegna](../fase-3-docker/docker-04-progetto-finale.md)

**Navigazione lezioni**: [← Lezione 9](#lezione-9-3h--progetto-finale-implementazione) | [Indice](#indice-cronologico-10-lezioni-x-3h)

---

## Uso consigliato in classe

- Apri una sola lezione alla volta e segui la scansione oraria.
- Esegui tutti i comandi del tutorial nell'ordine proposto.
- Alla fine di ogni lezione, completa il checkpoint nel modulo collegato.
- In caso di assenza, riparti dalla lezione successiva usando i link cronologici.
