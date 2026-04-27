## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1  |  **Tipo**: Lab

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] **[Setup Ambiente di Lavoro](../fase-0-prerequisiti/00-lab-setup-ambiente.md)** ← OBBLIGATORIO
- [ ] [Sessione Introduttiva 1 — Teoria](intro-01-teoria-os-filesystem-shell.md)

**Nota**: Il setup ambiente installa e configura VirtualBox, Alpine Linux, Docker e tutti gli strumenti necessari.

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Eseguire comandi base nel terminale Linux (Alpine)
- Navigare nel filesystem e gestire file e directory
- Preparare l'ambiente per le future esercitazioni su Docker

---

# Esercitazione Pratica di Laboratorio - Sessione Introduttiva 1
## Sistemi Operativi, Filesystem e Shell

### Panoramica dell'Esercitazione
Questa esercitazione pratica di laboratorio è progettata per accompagnare la Sessione Introduttiva 1 sui Sistemi Operativi, Filesystem e Shell. Gli studenti lavoreranno in ambiente **Linux Alpine tramite VirtualBox** (configurato nel modulo di setup), mettendo in pratica i concetti teorici appresi durante la sessione. **Il focus è esclusivamente su Linux/Alpine** — l'ambiente su cui opereremo anche con Docker.

### Prerequisiti
- VirtualBox installato (dal setup ambiente)
- VM Alpine Linux funzionante
- Conoscenze di base sull'uso del computer

### Durata
- **Durata consigliata**: 1h30-2h00
- **Approfondimenti opzionali**: +30min

### Obiettivi di Apprendimento
- Navigare nel filesystem Linux
- Eseguire comandi di base per la gestione di file e directory
- Comprendere la struttura del sistema operativo Linux
- Preparare l'ambiente per le future esercitazioni su Docker

## Parte 1: Configurazione dell'Ambiente di Lavoro (30 minuti)

### 1.1 Configurazione di VirtualBox e Alpine Linux
**Obiettivo**: Configurare una macchina virtuale con Alpine Linux.

**Attività**:
1. Avviare VirtualBox

2. Creare una nuova macchina virtuale:
   - Nome: "AlpineLinux-Lab"
   - Tipo: Linux
   - Versione: Other Linux (64-bit)
   - Memoria: 1024 MB
   - Disco rigido: Creare un disco virtuale da 8 GB

3. Configurare la rete:
   - Scheda 1: NAT (per accesso a Internet)

4. Montare l'immagine ISO di Alpine Linux:
   - Impostazioni > Storage
   - Controller IDE > Aggiungi unità ottica
   - Selezionare l'immagine ISO di Alpine Linux

5. Avviare la macchina virtuale e seguire la procedura di installazione di Alpine Linux:
   - Login come "root" (senza password)
   - Eseguire `setup-alpine`
   - Seguire le istruzioni per configurare:
     - Tastiera
     - Nome host (usare "alpine-lab")
     - Rete (DHCP)
     - Password root
     - Fuso orario
     - Repository (default)
     - Disco di installazione (sda)
     - Tipo di utilizzo (sys)

6. Riavviare la macchina virtuale:
   ```
   reboot
   ```

7. Dopo il riavvio, verificare che i repository Alpine usino HTTPS (obbligatorio in laboratorio):
   ```
   cat /etc/apk/repositories
   cp /etc/apk/repositories /etc/apk/repositories.bak
   sed -i 's|^http://|https://|g' /etc/apk/repositories
   grep -nE '^http://|^https://' /etc/apk/repositories
   ```
   Output atteso: tutte le righe dei repository iniziano con `https://`

8. Accedere come root e installare alcuni pacchetti utili:
   ```
   apk update
   apk add bash vim nano less grep
   ```

9. Creare un utente non privilegiato per le esercitazioni:
   ```
   adduser studente
   ```
   (Seguire le istruzioni per impostare la password)

10. Aggiungere l'utente al gruppo wheel per consentire l'uso di sudo:
   ```
   apk add sudo
   echo '%wheel ALL=(ALL) ALL' > /etc/sudoers.d/wheel
   addgroup studente wheel
   ```

11. Verificare che tutto funzioni correttamente:
    - Disconnettersi: `exit`
    - Accedere come "studente"
    - Provare un comando sudo: `sudo ls /root`

## Parte 2: Terminale Linux (Alpine) (30 minuti)

**Obiettivo**: Familiarizzare con il terminale Linux (Bash).

**Attività**:
1. Accedere alla VM Alpine Linux come utente "studente"
   - Mantieni questo utente per le attività del laboratorio: usa `sudo` solo quando il comando richiede privilegi amministrativi.

2. Esplorare la shell Bash:
   - Eseguire i seguenti comandi e annotare l'output:
     ```
     uname -a
     man ls
     ls -la
     pwd
     echo "Hello World"
     ip addr
     ```

3. Osservare le caratteristiche principali di Bash:
   - Bash è case-sensitive
   - Bash usa forward slash (/) per i percorsi
   - Bash ha pipe (|) e redirection (>, >>)
   - Bash è lo standard de facto in ambienti Linux/Unix

## Parte 3: Navigazione nel Filesystem Linux (30 minuti)

**Obiettivo**: Esplorare la struttura del filesystem Linux.

**Attività**:
1. In Alpine Linux, esplorare la struttura delle directory:
   ```
   df -h
   cd /
   ls -la
   cd /home
   ls -la
   cd /home/studente
   ls -la
   ```

2. Comprendere i percorsi in Linux:
   - Percorsi assoluti:
     ```
     cd /home/studente/Documents
     ```
   - Percorsi relativi:
     ```
     cd ..
     cd Documents
     ```

3. Visualizzare le variabili d'ambiente dei percorsi:
   ```
   echo $PATH
   echo $HOME
   echo $TMPDIR
   ```

4. Creare una struttura di directory per il laboratorio:
   ```
   mkdir ~/LabDocker
   cd ~/LabDocker
   mkdir Progetti Documenti Configurazioni
   ls -la
   ```

## Parte 4: Gestione di File e Directory in Linux (30 minuti)

**Obiettivo**: Praticare i comandi di gestione file in Linux.

**Attività**:
1. Navigare nella directory del laboratorio:
   ```
   cd ~/LabDocker
   ```

2. Creare e manipolare file:
   ```
   echo "Questo è un file di test" > test.txt
   cat test.txt
   cp test.txt Documenti/test_copia.txt
   mv test.txt Progetti/test_spostato.txt
   ls -la Progetti
   ls -la Documenti
   ```

3. Creare e manipolare directory:
   ```
   mkdir Progetti/Progetto1
   mkdir Progetti/Progetto2
   ls -la Progetti
   mv Progetti/Progetto1 Progetti/Progetto_Rinominato
   ls -la Progetti
   ```

4. Eliminare file e directory:
   ```
   rm Documenti/test_copia.txt
   ls -la Documenti
   rmdir Progetti/Progetto2
   ls -la Progetti
   ```

5. Gestire i permessi (specifico di Linux):
   ```
   touch file_permessi.txt
   ls -la file_permessi.txt
   chmod 755 file_permessi.txt
   ls -la file_permessi.txt
   chmod 644 file_permessi.txt
   ls -la file_permessi.txt
   ```

## Parte 5: Esercizio Finale (30 minuti)

### 5.1 Progetto "Preparazione Ambiente Docker"
**Obiettivo**: Creare una struttura di directory e file che sarà utilizzata nelle future esercitazioni su Docker.

**Attività** (tutto su Alpine Linux):
1. Creare la struttura di directory:
   ```
   cd ~/LabDocker
   mkdir DockerLab
   cd DockerLab
   mkdir Immagini Volumi Reti Compose
   echo "# Appunti su Docker" > README.md
   echo "Immagini Docker" > Immagini/README.md
   echo "Volumi Docker" > Volumi/README.md
   echo "Reti Docker" > Reti/README.md
   echo "Docker Compose" > Compose/README.md
   ```

2. Creare un file di configurazione per un'applicazione di esempio:
   ```
   mkdir -p DockerLab/Compose/app-esempio
   echo '{
     "name": "app-esempio",
     "version": "1.0.0",
     "description": "Applicazione di esempio per Docker"
   }' > DockerLab/Compose/app-esempio/config.json
   ```

3. Verificare la struttura creata:
   ```
   find DockerLab -type f | sort
   ```

### 5.2 Script di Automazione
**Obiettivo**: Creare un semplice script Bash per automatizzare operazioni in ambiente Linux.

**Attività**:
1. Creare uno script Bash:
   ```
   nano ~/LabDocker/setup_env.sh
   ```
   Contenuto:
   ```bash
   #!/bin/bash
   # Script di setup ambiente Linux per Docker
   
   LAB_DIR="$HOME/LabDocker"
   
   # Crea directory se non esistono
   if [ ! -d "$LAB_DIR" ]; then
       mkdir -p "$LAB_DIR"
   fi
   
   # Crea struttura per Docker
   DOCKER_DIR="$LAB_DIR/DockerLab"
   if [ ! -d "$DOCKER_DIR" ]; then
       mkdir -p "$DOCKER_DIR"
       mkdir -p "$DOCKER_DIR/Immagini"
       mkdir -p "$DOCKER_DIR/Volumi"
       mkdir -p "$DOCKER_DIR/Reti"
       mkdir -p "$DOCKER_DIR/Compose"
   fi
   
   # Crea file README
   echo "# Laboratorio Docker" > "$DOCKER_DIR/README.md"
   echo "" >> "$DOCKER_DIR/README.md"
   echo "Creato il $(date)" >> "$DOCKER_DIR/README.md"
   
   echo "Ambiente di laboratorio configurato in $DOCKER_DIR"
   ```

2. Rendere lo script eseguibile:
   ```
   chmod +x ~/LabDocker/setup_env.sh
   ```

3. Eseguire lo script:
   ```
   ~/LabDocker/setup_env.sh
   ```

4. Verificare i risultati:
   ```
   ls -la ~/LabDocker/DockerLab
   cat ~/LabDocker/DockerLab/README.md
   ```

## Conclusione e Verifica dell'Apprendimento

### Riepilogo
- Abbiamo configurato e testato l'ambiente Alpine Linux su VirtualBox
- Abbiamo esplorato il filesystem Linux e la shell Bash
- Abbiamo praticato i comandi di gestione file e directory
- Abbiamo creato una struttura di directory e script per le future esercitazioni su Docker

### Verifica dell'Apprendimento
1. Come si naviga nel filesystem Linux? Quali comandi si usano?
2. Come si gestiscono file e directory in Linux?
3. Quali sono i comandi equivalenti a quelli che useremo in Docker?
4. Perché è importante conoscere il filesystem Linux per lavorare con Docker?

### Preparazione per la Prossima Sessione
- Mantenere attiva la VM Alpine Linux
- Conservare la struttura di directory creata
- Rivedere i concetti di rete che saranno approfonditi nella prossima sessione

## Risorse Aggiuntive
- [Guida Bash](https://www.gnu.org/software/bash/manual/bash.html)
- [Alpine Linux Wiki](https://wiki.alpinelinux.org/)

---

## ➡️ Prossimi passi

- **Continua con**: [Sessione Introduttiva 2 — Teoria](../fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md)
- **Approfondisci**: [Lab Modalità di Rete VirtualBox](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice principale](../README.md)