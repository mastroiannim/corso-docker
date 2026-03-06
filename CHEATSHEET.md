# 📋 Cheatsheet — Comandi Rapidi del Corso

> Riferimento veloce per tutti i comandi usati nel corso. Sistema di riferimento: **Alpine Linux**.

---

## � Setup Iniziale dell'Ambiente

### Installazione Alpine Linux

| Comando / Azione | Descrizione |
|-------------------|-------------|
| `setup-alpine` | Avvia la procedura guidata di installazione di Alpine |
| `setup-keymap` | Configura il layout della tastiera |
| `passwd root` | Cambia la password di root |

### Comandi di base per il setup

| Comando | Descrizione |
|---------|-------------|
| `apk update` | Aggiorna la lista dei pacchetti disponibili |
| `apk add pacchetto` | Installa un pacchetto |
| `apk upgrade` | Aggiorna tutti i pacchetti installati |
| `ip addr show eth0` | Mostra l'indirizzo IP dell'interfaccia eth0 |
| `ssh root@192.168.X.X` | Connessione SSH alla VM (da Windows) |
| `ping -c 4 google.com` | Testa la connettività Internet |
| `hostname` | Mostra il nome del sistema |
| `whoami` | Mostra l'utente corrente |
| `uname -a` | Mostra informazioni sul sistema |
| `df -h` | Mostra lo spazio su disco |

### 🔐 Laboratorio sicuro: repository APK solo HTTPS

Se `apk update` fallisce perché in laboratorio è bloccato `http`, eseguire:

```bash
cp /etc/apk/repositories /etc/apk/repositories.bak
sed -i 's|^http://|https://|g' /etc/apk/repositories
grep -nE '^http://|^https://' /etc/apk/repositories
apk update
```

Output atteso: solo repository `https://...` e messaggio finale `OK: ... distinct packages available`.

### Servizi essenziali

| Comando | Descrizione |
|---------|-------------|
| `rc-service sshd start` | Avvia il servizio SSH |
| `rc-service sshd status` | Verifica lo stato del servizio SSH |
| `rc-update add sshd default` | Abilita SSH all'avvio automatico |
| `rc-service docker start` | Avvia il servizio Docker |
| `rc-update add docker default` | Abilita Docker all'avvio automatico |

### Strumenti di sviluppo

| Comando | Descrizione |
|---------|-------------|
| `apk add nano` | Installa l'editor di testo nano |
| `apk add git` | Installa Git |
| `apk add bash bash-completion` | Installa Bash e il completamento automatico |
| `apk add docker docker-cli-compose` | Installa Docker e Docker Compose |
| `apk add curl wget bind-tools htop` | Installa strumenti di rete e monitoring |
| `chsh -s /bin/bash` | Cambia la shell predefinita a Bash |

### Configurazione Bash

| Comando | Descrizione |
|---------|-------------|
| `echo 'export EDITOR=nano' >> ~/.bashrc` | Imposta nano come editor predefinito |
| `source ~/.bashrc` | Ricarica la configurazione Bash |
| `git config --global user.name "Nome"` | Configura il nome utente per Git |
| `git config --global user.email "email"` | Configura l'email per Git |

### Gestione VM (VirtualBox)

| Comando / Azione | Descrizione |
|-------------------|-------------|
| `poweroff` | Spegne la VM Alpine |
| `reboot` | Riavvia la VM Alpine |
| **Istantanee** → **Crea** | Crea uno snapshot della VM (da VirtualBox) |
| **Dispositivi** → **Unità ottiche** → **Rimuovi disco** | Rimuove l'ISO dal lettore virtuale |

---

## �🐧 Shell Linux

| Comando | Descrizione |
|---------|-------------|
| `ls -la` | Mostra file e directory con dettagli e file nascosti |
| `cd /percorso` | Cambia directory |
| `mkdir -p cartella` | Crea una directory (e le directory padre se necessario) |
| `cat file.txt` | Mostra il contenuto di un file |
| `cp sorgente destinazione` | Copia file o directory |
| `mv sorgente destinazione` | Sposta o rinomina file |
| `rm file.txt` | Elimina un file |
| `rm -rf cartella/` | Elimina una directory e tutto il suo contenuto |
| `chmod 755 file` | Cambia i permessi di un file |
| `grep "testo" file` | Cerca testo dentro un file |
| `find / -name "file"` | Cerca un file nel filesystem |
| `pipe \|` | Passa l'output di un comando come input del successivo |
| `>` | Redirige l'output verso un file (sovrascrive) |
| `>>` | Redirige l'output verso un file (aggiunge in coda) |

---

## 🌐 Rete & Diagnostica

| Comando | Descrizione |
|---------|-------------|
| `ip addr show` | Mostra gli indirizzi IP delle interfacce di rete |
| `ip route show` | Mostra la tabella di routing |
| `ping -c 4 host` | Verifica la raggiungibilità di un host (4 pacchetti) |
| `curl https://host` | Invia una richiesta HTTPS e mostra la risposta |
| `wget https://url` | Scarica un file da un URL HTTPS |
| `ss -tlnp` | Mostra le porte TCP in ascolto |
| `nslookup dominio` | Risolve un nome di dominio in indirizzo IP |
| `traceroute host` | Mostra il percorso dei pacchetti verso un host |

---

## 🖥️ VirtualBox

| Comando / Azione | Descrizione |
|-------------------|-------------|
| `apk add virtualbox-guest-additions` | Installa le guest additions su Alpine |
| `mkdir /mnt/html` | Crea il punto di mount per la cartella condivisa |
| `modprobe vboxsf` | Carica il modulo kernel per le cartelle condivise |
| `mount -t vboxsf shared /mnt/html` | Monta la cartella condivisa manualmente |
| `echo "html /mnt/html vboxsf defaults 0 0" \| tee -a /etc/fstab > /dev/null` | Monta la cartella condivisa automaticamente all'avvio |

### Modalità di Rete VirtualBox

| Modalità | Accesso Internet | Accesso da Host | Accesso tra VM |
|----------|-----------------|-----------------|----------------|
| NAT | ✅ | ❌ (serve port forwarding) | ❌ |
| Bridge | ✅ | ✅ | ✅ |
| Host-Only | ❌ | ✅ | ✅ |
| NAT Network | ✅ | ❌ (serve port forwarding) | ✅ |

---

## 🐳 Docker Basics

| Comando | Descrizione |
|---------|-------------|
| `apk add docker` | Installa Docker su Alpine Linux |
| `rc-service docker start` | Avvia il servizio Docker |
| `rc-update add docker default` | Abilita Docker all'avvio automatico |
| `docker --version` | Verifica la versione di Docker installata |
| `docker pull immagine` | Scarica un'immagine dal Docker Hub |
| `docker images` | Elenca le immagini scaricate |
| `docker run immagine` | Esegue un container da un'immagine |
| `docker run -d --name nome immagine` | Esegue un container in background con un nome |
| `docker run -it immagine sh` | Esegue un container in modalità interattiva con shell |
| `docker run -p 8080:80 immagine` | Espone la porta 80 del container sulla porta 8080 dell'host |
| `docker ps` | Mostra i container in esecuzione |
| `docker ps -a` | Mostra tutti i container (anche fermi) |
| `docker stop nome` | Ferma un container |
| `docker start nome` | Avvia un container fermato |
| `docker rm nome` | Rimuove un container |
| `docker rmi immagine` | Rimuove un'immagine |
| `docker logs nome` | Mostra i log di un container |
| `docker exec -it nome sh` | Apre una shell in un container in esecuzione |
| `docker inspect nome` | Mostra i dettagli di un container |

### Dockerfile

```dockerfile
FROM alpine:latest
RUN apk add --no-cache pacchetto
COPY ./sorgente /destinazione
EXPOSE 80
CMD ["comando"]
```

### Volumi

| Comando | Descrizione |
|---------|-------------|
| `docker volume create nome` | Crea un volume |
| `docker volume ls` | Elenca i volumi |
| `docker run -v nome:/percorso immagine` | Monta un volume in un container |
| `docker run -v /host:/container immagine` | Monta una directory dell'host (bind mount) |

### Networking

| Comando | Descrizione |
|---------|-------------|
| `docker network create nome` | Crea una rete Docker |
| `docker network ls` | Elenca le reti Docker |
| `docker run --network nome immagine` | Collega un container a una rete |
| `docker network inspect nome` | Mostra i dettagli di una rete |

---

## 🐳 Docker Compose

| Comando | Descrizione |
|---------|-------------|
| `docker compose up -d` | Avvia tutti i servizi in background |
| `docker compose down` | Ferma e rimuove tutti i servizi |
| `docker compose ps` | Mostra lo stato dei servizi |
| `docker compose logs` | Mostra i log di tutti i servizi |
| `docker compose build` | Ricostruisce le immagini |
| `docker compose exec servizio sh` | Apre una shell in un servizio |

### Esempio `docker-compose.yml`

```yaml
services:
  web:
    build: ./web
    ports:
      - "8080:80"
    depends_on:
      - db
  db:
    image: postgres:alpine
    environment:
      POSTGRES_PASSWORD: password
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

---

## 🔧 Alpine Linux — Gestione Pacchetti

| Comando | Descrizione |
|---------|-------------|
| `apk update` | Aggiorna l'indice dei pacchetti |
| `apk add pacchetto` | Installa un pacchetto |
| `apk del pacchetto` | Rimuove un pacchetto |
| `apk search parola` | Cerca un pacchetto |
| `rc-service servizio start` | Avvia un servizio |
| `rc-service servizio stop` | Ferma un servizio |
| `rc-update add servizio default` | Abilita un servizio all'avvio |

---

↩ [Torna all'indice principale](README.md)
