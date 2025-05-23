# Esercitazione Pratica di Laboratorio - Sessione Introduttiva 1
## Sistemi Operativi, Filesystem e Shell

### Panoramica dell'Esercitazione
Questa esercitazione pratica di laboratorio è progettata per accompagnare la Sessione Introduttiva 1 sui Sistemi Operativi, Filesystem e Shell. Gli studenti lavoreranno sia in ambiente Windows 11 (con account senza privilegi di amministratore) sia in ambiente Linux Alpine tramite VirtualBox, per mettere in pratica i concetti teorici appresi durante la sessione.

### Prerequisiti
- Computer con Windows 11 (account senza privilegi di amministratore)
- VirtualBox installato
- Immagine ISO di Alpine Linux
- Conoscenze di base sull'uso del computer

### Durata
3 ore di laboratorio

### Obiettivi di Apprendimento
- Confrontare i terminali Windows e Linux
- Navigare nel filesystem in entrambi gli ambienti
- Eseguire comandi di base per la gestione di file e directory
- Comprendere le differenze tra i due sistemi operativi
- Preparare l'ambiente per le future esercitazioni su Docker

## Parte 1: Configurazione dell'Ambiente di Lavoro (45 minuti)

### 1.1 Esplorazione dell'Ambiente Windows
**Obiettivo**: Familiarizzare con gli strumenti disponibili in Windows 11 senza privilegi di amministratore.

**Attività**:
1. Aprire il Prompt dei Comandi di Windows:
   - Premere `Win + R`
   - Digitare `cmd` e premere Invio

2. Aprire Windows PowerShell:
   - Premere `Win + X`
   - Selezionare "Windows PowerShell"

3. Verificare le informazioni di sistema:
   ```
   systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
   ```

4. Verificare l'account utente e i gruppi:
   ```
   whoami
   net user %username%
   ```

5. Annotare le limitazioni dell'account senza privilegi di amministratore:
   - Tentare di installare un software
   - Tentare di modificare impostazioni di sistema
   - Osservare i messaggi di errore relativi ai privilegi

### 1.2 Configurazione di VirtualBox e Alpine Linux
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

7. Dopo il riavvio, accedere come root e installare alcuni pacchetti utili:
   ```
   apk update
   apk add bash vim nano less grep
   ```

8. Creare un utente non privilegiato per le esercitazioni:
   ```
   adduser studente
   ```
   (Seguire le istruzioni per impostare la password)

9. Aggiungere l'utente al gruppo wheel per consentire l'uso di sudo:
   ```
   apk add sudo
   echo '%wheel ALL=(ALL) ALL' > /etc/sudoers.d/wheel
   addgroup studente wheel
   ```

10. Verificare che tutto funzioni correttamente:
    - Disconnettersi: `exit`
    - Accedere come "studente"
    - Provare un comando sudo: `sudo ls /root`

## Parte 2: Confronto tra Terminali Windows e Linux (45 minuti)

### 2.1 Terminali Windows: CMD e PowerShell
**Obiettivo**: Comprendere le differenze tra i terminali disponibili in Windows.

**Attività**:
1. Esplorare il Prompt dei Comandi (CMD):
   - Aprire CMD
   - Eseguire i seguenti comandi e annotare l'output:
     ```
     ver
     help
     dir
     cd
     echo Hello World
     ipconfig
     ```

2. Esplorare PowerShell:
   - Aprire PowerShell
   - Eseguire i seguenti comandi e annotare l'output:
     ```
     $PSVersionTable
     Get-Command
     Get-ChildItem
     Set-Location
     Write-Host "Hello World"
     Get-NetIPConfiguration
     ```

3. Compilare una tabella di confronto:

   | Funzione | CMD | PowerShell |
   |----------|-----|------------|
   | Versione | `ver` | `$PSVersionTable` |
   | Aiuto | `help` | `Get-Help` |
   | Elenco file | `dir` | `Get-ChildItem` o `dir` |
   | Cambio directory | `cd` | `Set-Location` o `cd` |
   | Output testo | `echo` | `Write-Host` |
   | Info rete | `ipconfig` | `Get-NetIPConfiguration` |

4. Osservare le differenze principali:
   - CMD è più semplice ma meno potente
   - PowerShell è orientato agli oggetti, non solo al testo
   - PowerShell ha alias per i comandi CMD per compatibilità
   - PowerShell supporta script più avanzati

### 2.2 Terminale Linux (Alpine)
**Obiettivo**: Familiarizzare con il terminale Linux e confrontarlo con quelli di Windows.

**Attività**:
1. Accedere alla VM Alpine Linux come utente "studente"

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

3. Estendere la tabella di confronto:

   | Funzione | CMD | PowerShell | Bash (Linux) |
   |----------|-----|------------|--------------|
   | Versione | `ver` | `$PSVersionTable` | `uname -a` |
   | Aiuto | `help` | `Get-Help` | `man` |
   | Elenco file | `dir` | `Get-ChildItem` | `ls` |
   | Cambio directory | `cd` | `Set-Location` | `cd` |
   | Output testo | `echo` | `Write-Host` | `echo` |
   | Info rete | `ipconfig` | `Get-NetIPConfiguration` | `ip addr` |

4. Osservare le differenze principali:
   - Bash è case-sensitive, CMD e PowerShell no
   - Bash usa forward slash (/) per percorsi, Windows usa backslash (\)
   - Bash ha pipe (|) e redirection (>, >>) simili a CMD
   - Bash è lo standard de facto in ambienti Linux/Unix

5. Testare la compatibilità dei comandi:
   - Provare comandi Windows in Linux e viceversa
   - Notare quali comandi sono simili e quali sono completamente diversi

## Parte 3: Navigazione nel Filesystem (45 minuti)

### 3.1 Filesystem Windows
**Obiettivo**: Esplorare la struttura del filesystem Windows.

**Attività**:
1. In PowerShell, esplorare la struttura delle directory:
   ```
   Get-PSDrive -PSProvider FileSystem
   cd C:\
   dir
   cd Users
   dir
   cd $env:USERNAME
   dir
   ```

2. Comprendere i percorsi in Windows:
   - Percorsi assoluti:
     ```
     cd C:\Users\$env:USERNAME\Documents
     ```
   - Percorsi relativi:
     ```
     cd ..
     cd Documents
     ```

3. Visualizzare le variabili d'ambiente dei percorsi:
   ```
   echo $env:PATH
   echo $env:USERPROFILE
   echo $env:TEMP
   ```

4. Creare una struttura di directory per il laboratorio:
   ```
   mkdir C:\Users\$env:USERNAME\LabDocker
   cd C:\Users\$env:USERNAME\LabDocker
   mkdir Progetti Documenti Configurazioni
   dir
   ```

### 3.2 Filesystem Linux
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

### 3.3 Confronto tra Filesystem
**Obiettivo**: Comprendere le differenze tra i filesystem Windows e Linux.

**Attività**:
1. Compilare una tabella di confronto:

   | Caratteristica | Windows | Linux |
   |----------------|---------|-------|
   | Separatore percorsi | `\` | `/` |
   | Root | `C:\` | `/` |
   | Home utente | `C:\Users\username` | `/home/username` |
   | Case sensitivity | No | Sì |
   | File nascosti | Attributo "nascosto" | Iniziano con `.` |
   | Estensioni file | Importanti | Opzionali |

2. Discutere le implicazioni di queste differenze per lo sviluppo e la containerizzazione

## Parte 4: Gestione di File e Directory (45 minuti)

### 4.1 Gestione File in Windows
**Obiettivo**: Praticare i comandi di gestione file in Windows.

**Attività**:
1. Navigare nella directory del laboratorio:
   ```
   cd C:\Users\$env:USERNAME\LabDocker
   ```

2. Creare e manipolare file:
   ```
   echo "Questo è un file di test" > test.txt
   type test.txt
   copy test.txt Documenti\test_copia.txt
   move test.txt Progetti\test_spostato.txt
   dir Progetti
   dir Documenti
   ```

3. Creare e manipolare directory:
   ```
   mkdir Progetti\Progetto1
   mkdir Progetti\Progetto2
   dir Progetti
   move Progetti\Progetto1 Progetti\Progetto_Rinominato
   dir Progetti
   ```

4. Eliminare file e directory:
   ```
   del Documenti\test_copia.txt
   dir Documenti
   rmdir Progetti\Progetto2
   dir Progetti
   ```

### 4.2 Gestione File in Linux
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

### 4.3 Confronto dei Comandi di Gestione File
**Obiettivo**: Confrontare i comandi di gestione file tra Windows e Linux.

**Attività**:
1. Compilare una tabella di confronto:

   | Operazione | Windows CMD | Windows PowerShell | Linux Bash |
   |------------|-------------|-------------------|------------|
   | Creare file | `echo > file.txt` | `New-Item file.txt` | `touch file.txt` |
   | Visualizzare file | `type file.txt` | `Get-Content file.txt` | `cat file.txt` |
   | Copiare file | `copy file.txt dest` | `Copy-Item file.txt dest` | `cp file.txt dest` |
   | Spostare file | `move file.txt dest` | `Move-Item file.txt dest` | `mv file.txt dest` |
   | Eliminare file | `del file.txt` | `Remove-Item file.txt` | `rm file.txt` |
   | Creare directory | `mkdir dir` | `New-Item -Type Directory dir` | `mkdir dir` |
   | Eliminare directory | `rmdir dir` | `Remove-Item dir` | `rmdir dir` |

2. Discutere le implicazioni di queste differenze per lo sviluppo e la containerizzazione

## Parte 5: Esercizio Finale Integrato (45 minuti)

### 5.1 Progetto "Preparazione Ambiente Docker"
**Obiettivo**: Creare una struttura di directory e file che sarà utilizzata nelle future esercitazioni su Docker.

**Attività**:
1. In Windows, creare la seguente struttura:
   ```
   cd C:\Users\$env:USERNAME\LabDocker
   mkdir DockerLab
   cd DockerLab
   mkdir Immagini Volumi Reti Compose
   echo "# Appunti su Docker" > README.md
   echo "Immagini Docker" > Immagini\README.md
   echo "Volumi Docker" > Volumi\README.md
   echo "Reti Docker" > Reti\README.md
   echo "Docker Compose" > Compose\README.md
   ```

2. In Alpine Linux, creare la stessa struttura:
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

3. Creare un file di configurazione per un'applicazione di esempio:
   - In Windows:
     ```
     mkdir DockerLab\Compose\app-esempio
     echo "{`n  `"name`": `"app-esempio`",`n  `"version`": `"1.0.0`",`n  `"description`": `"Applicazione di esempio per Docker`"`n}" > DockerLab\Compose\app-esempio\config.json
     ```
   - In Linux:
     ```
     mkdir -p DockerLab/Compose/app-esempio
     echo '{
       "name": "app-esempio",
       "version": "1.0.0",
       "description": "Applicazione di esempio per Docker"
     }' > DockerLab/Compose/app-esempio/config.json
     ```

4. Verificare la struttura creata:
   - In Windows:
     ```
     dir /s DockerLab
     ```
   - In Linux:
     ```
     find DockerLab -type f | sort
     ```

### 5.2 Script di Automazione
**Obiettivo**: Creare semplici script per automatizzare operazioni in entrambi gli ambienti.

**Attività**:
1. In Windows, creare uno script PowerShell:
   ```
   notepad C:\Users\$env:USERNAME\LabDocker\setup_env.ps1
   ```
   Contenuto:
   ```powershell
   # Script di setup ambiente Windows
   $labDir = "$env:USERPROFILE\LabDocker"
   
   # Crea directory se non esistono
   if (-not (Test-Path $labDir)) {
       New-Item -ItemType Directory -Path $labDir
   }
   
   # Crea struttura per Docker
   $dockerDir = "$labDir\DockerLab"
   if (-not (Test-Path $dockerDir)) {
       New-Item -ItemType Directory -Path $dockerDir
       New-Item -ItemType Directory -Path "$dockerDir\Immagini"
       New-Item -ItemType Directory -Path "$dockerDir\Volumi"
       New-Item -ItemType Directory -Path "$dockerDir\Reti"
       New-Item -ItemType Directory -Path "$dockerDir\Compose"
   }
   
   # Crea file README
   Set-Content -Path "$dockerDir\README.md" -Value "# Laboratorio Docker`n`nCreato il $(Get-Date)"
   
   Write-Host "Ambiente di laboratorio configurato in $dockerDir"
   ```

2. In Linux, creare uno script Bash:
   ```
   nano ~/LabDocker/setup_env.sh
   ```
   Contenuto:
   ```bash
   #!/bin/bash
   # Script di setup ambiente Linux
   
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

3. Rendere lo script Linux eseguibile:
   ```
   chmod +x ~/LabDocker/setup_env.sh
   ```

4. Eseguire gli script:
   - In Windows:
     ```
     powershell -ExecutionPolicy Bypass -File C:\Users\$env:USERNAME\LabDocker\setup_env.ps1
     ```
   - In Linux:
     ```
     ~/LabDocker/setup_env.sh
     ```

5. Verificare i risultati:
   - In Windows:
     ```
     dir C:\Users\$env:USERNAME\LabDocker\DockerLab
     type C:\Users\$env:USERNAME\LabDocker\DockerLab\README.md
     ```
   - In Linux:
     ```
     ls -la ~/LabDocker/DockerLab
     cat ~/LabDocker/DockerLab/README.md
     ```

## Conclusione e Verifica dell'Apprendimento

### Riepilogo
- Abbiamo configurato un ambiente di lavoro con Windows 11 e Alpine Linux
- Abbiamo confrontato i terminali Windows (CMD e PowerShell) con il terminale Linux (Bash)
- Abbiamo esplorato e confrontato i filesystem Windows e Linux
- Abbiamo praticato i comandi di gestione file e directory in entrambi gli ambienti
- Abbiamo creato una struttura di directory e script per le future esercitazioni su Docker

### Verifica dell'Apprendimento
1. Quali sono le principali differenze tra i terminali Windows e Linux?
2. Come si naviga nel filesystem in Windows e Linux? Quali sono le differenze principali?
3. Come si gestiscono file e directory in entrambi gli ambienti?
4. Perché è importante comprendere entrambi gli ambienti per lavorare con Docker?
5. Come possono gli script di automazione semplificare il lavoro in entrambi gli ambienti?

### Preparazione per la Prossima Sessione
- Mantenere attiva la VM Alpine Linux
- Conservare la struttura di directory creata
- Rivedere i concetti di rete che saranno approfonditi nella prossima sessione

## Risorse Aggiuntive
- [Documentazione PowerShell](https://docs.microsoft.com/en-us/powershell/)
- [Guida Bash](https://www.gnu.org/software/bash/manual/bash.html)
- [Alpine Linux Wiki](https://wiki.alpinelinux.org/)
- [Confronto tra comandi Windows e Linux](https://www.lemoda.net/windows/windows2unix/windows2unix.html)
