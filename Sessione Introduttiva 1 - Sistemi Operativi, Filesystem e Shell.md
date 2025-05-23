# Sessione Introduttiva 1: Sistemi Operativi, Filesystem e Shell

## Panoramica della Sessione
Questa prima sessione introduttiva di 5 ore è progettata per fornire agli studenti le conoscenze di base sui sistemi operativi, il filesystem e l'interfaccia a riga di comando (shell). Questi concetti sono fondamentali per comprendere il funzionamento di Docker e della containerizzazione.

## Parte 1: Introduzione ai Sistemi Operativi (1 ora)

### Cos'è un Sistema Operativo?
Un sistema operativo è il software fondamentale che gestisce tutte le risorse hardware e software di un computer. Possiamo pensarlo come il "direttore d'orchestra" del computer, che coordina tutte le attività e assegna le risorse ai vari programmi.

#### Funzioni Principali di un Sistema Operativo:
- **Gestione dell'hardware**: Controlla e coordina i dispositivi fisici (CPU, memoria, dischi, periferiche)
- **Gestione della memoria**: Alloca e libera la memoria RAM per i programmi
- **Gestione dei processi**: Avvia, monitora e termina i programmi in esecuzione
- **Gestione del filesystem**: Organizza e gestisce l'accesso ai dati memorizzati
- **Interfaccia utente**: Fornisce un modo per interagire con il computer

### Principali Sistemi Operativi
Esistono diversi sistemi operativi, ciascuno con caratteristiche specifiche:

#### Windows
- Sviluppato da Microsoft
- Interfaccia grafica intuitiva
- Ampia compatibilità con software commerciale
- Predominante nei computer desktop aziendali e personali

#### macOS
- Sviluppato da Apple
- Interfaccia elegante e coerente
- Ottimizzato per l'hardware Apple
- Popolare tra professionisti creativi

#### Linux
- Open source e gratuito
- Altamente personalizzabile
- Disponibile in molte distribuzioni (Ubuntu, Fedora, Debian, Alpine, ecc.)
- Predominante nei server e nei sistemi embedded
- **Fondamentale per Docker**: Docker è basato sul kernel Linux

### Focus su Linux
Linux merita un'attenzione particolare perché:
- È la base per la maggior parte dei server web
- Il kernel Linux fornisce le funzionalità che rendono possibili i container
- Docker è nato su Linux e sfrutta le sue caratteristiche native
- Alpine Linux (che gli studenti utilizzeranno) è una distribuzione Linux leggera e sicura

### Esercizio Pratico: Esplorazione del Sistema Operativo
**Obiettivo**: Familiarizzare con i componenti visibili del sistema operativo

**Attività**:
1. Identificare il sistema operativo in uso
2. Esplorare il menu principale e le impostazioni di sistema
3. Trovare informazioni sul sistema (versione, memoria, processore)
4. Identificare dove si trovano i programmi installati
5. Discutere le differenze tra i sistemi operativi conosciuti

## Parte 2: Filesystem e Organizzazione dei Dati (1 ora)

### Struttura Gerarchica del Filesystem
Il filesystem è il modo in cui il sistema operativo organizza e gestisce i file. Possiamo pensarlo come un sistema di classificazione di una biblioteca, con scaffali (directory) che contengono libri (file).

#### Concetti Fondamentali:
- **File**: Unità base di archiviazione (documenti, immagini, programmi, ecc.)
- **Directory (o cartella)**: Contenitore che può contenere file e altre directory
- **Struttura ad albero**: Organizzazione gerarchica che parte da una radice (root)

#### Struttura del Filesystem in Linux:
- **/** (root): La directory principale da cui parte tutto il filesystem
- **/bin**: Comandi essenziali del sistema
- **/etc**: File di configurazione
- **/home**: Directory personali degli utenti
- **/var**: File variabili (log, cache, ecc.)
- **/tmp**: File temporanei
- **/usr**: Programmi e librerie installati

### Percorsi Assoluti e Relativi
Per identificare la posizione di un file o una directory nel filesystem, utilizziamo i percorsi:

#### Percorso Assoluto:
- Parte dalla directory root (/)
- Specifica l'intero percorso fino al file/directory
- Esempio: `/home/utente/documenti/file.txt`
- È come dare un indirizzo completo partendo dalla città

#### Percorso Relativo:
- Parte dalla directory corrente
- Non inizia con /
- Esempio: `documenti/file.txt` (se ci troviamo in /home/utente)
- È come dare indicazioni partendo dalla posizione attuale

#### Simboli Speciali:
- `.` (punto): Directory corrente
- `..` (doppio punto): Directory superiore (parent)
- `~` (tilde): Directory home dell'utente

### Permessi e Proprietà
In Linux, ogni file e directory ha associati permessi che determinano chi può fare cosa:

#### Proprietà:
- **Proprietario**: L'utente che ha creato il file
- **Gruppo**: Un gruppo di utenti con permessi specifici
- **Altri**: Tutti gli altri utenti del sistema

#### Tipi di Permessi:
- **r (read)**: Permesso di lettura
- **w (write)**: Permesso di scrittura
- **x (execute)**: Permesso di esecuzione (per file) o accesso (per directory)

#### Rappresentazione dei Permessi:
- Simbolica: `rwxr-xr--` (proprietario: rwx, gruppo: r-x, altri: r--)
- Numerica: 754 (proprietario: 7=rwx, gruppo: 5=r-x, altri: 4=r--)

### Esercizio Pratico: Esplorazione del Filesystem
**Obiettivo**: Comprendere la struttura del filesystem e i concetti di percorsi

**Attività**:
1. Esplorare il filesystem usando l'interfaccia grafica
2. Identificare le principali directory di sistema
3. Creare una struttura di directory personale
4. Scrivere percorsi assoluti e relativi per diversi file
5. Verificare i permessi di alcuni file e directory

## Parte 3: Introduzione alla Shell (1 ora)

### Cos'è una Shell?
La shell è un'interfaccia testuale che permette di comunicare con il sistema operativo attraverso comandi testuali. Possiamo pensarla come una "conversazione" diretta con il computer.

#### Terminologia:
- **Terminal (o terminale)**: Il programma che fornisce l'interfaccia per la shell
- **Shell**: L'interprete dei comandi (bash, zsh, sh, ecc.)
- **Prompt**: L'indicatore che la shell è pronta a ricevere comandi

#### Tipi di Shell Comuni:
- **Bash** (Bourne Again SHell): La shell più comune nei sistemi Linux
- **Zsh**: Shell avanzata con funzionalità aggiuntive
- **Fish**: Shell user-friendly con suggerimenti colorati
- **PowerShell**: Shell di Windows con funzionalità avanzate

### Vantaggi dell'Interfaccia a Riga di Comando
Perché usare la shell quando esiste un'interfaccia grafica?

#### Vantaggi della CLI (Command Line Interface):
- **Potenza**: Operazioni complesse con pochi comandi
- **Automazione**: Script per automatizzare attività ripetitive
- **Efficienza**: Meno risorse di sistema rispetto all'interfaccia grafica
- **Precisione**: Controllo dettagliato delle operazioni
- **Accesso remoto**: Gestione di sistemi senza interfaccia grafica

#### Quando Usare CLI vs GUI:
- **CLI**: Automazione, operazioni batch, gestione remota, sviluppo
- **GUI**: Editing grafico, navigazione intuitiva, multitasking visivo

### Primi Passi con la Shell
Iniziamo a familiarizzare con la shell:

#### Apertura del Terminale:
- **Linux**: Ctrl+Alt+T o cerca "Terminal" nel menu
- **macOS**: Cmd+Space, poi cerca "Terminal"
- **Windows**: PowerShell o Windows Subsystem for Linux (WSL)

#### Anatomia di un Comando:
```
comando [opzioni] [argomenti]
```
- **Comando**: L'azione da eseguire
- **Opzioni**: Modificatori che cambiano il comportamento (preceduti da - o --)
- **Argomenti**: Su cosa agire (file, directory, testo, ecc.)

#### Esempi di Comandi Semplici:
- `echo "Ciao mondo"`: Visualizza il testo "Ciao mondo"
- `date`: Mostra la data e l'ora correnti
- `whoami`: Mostra il nome dell'utente corrente
- `clear`: Pulisce lo schermo del terminale

### Esercizio Pratico: Primi Comandi
**Obiettivo**: Familiarizzare con l'uso della shell e i comandi di base

**Attività**:
1. Aprire il terminale
2. Eseguire i comandi base (echo, date, whoami, clear)
3. Provare diverse combinazioni di opzioni e argomenti
4. Osservare e interpretare l'output dei comandi

## Parte 4: Comandi Bash Fondamentali (1.5 ore)

### Navigazione nel Filesystem
I comandi per muoversi nel filesystem sono tra i più utilizzati:

#### Comandi Principali:
- `pwd` (Print Working Directory): Mostra la directory corrente
- `ls` (List): Elenca i contenuti della directory
  - `ls -l`: Formato lungo con dettagli
  - `ls -a`: Mostra anche i file nascosti (che iniziano con .)
  - `ls -h`: Dimensioni in formato leggibile (KB, MB, GB)
- `cd` (Change Directory): Cambia la directory corrente
  - `cd /percorso/assoluto`: Va a un percorso assoluto
  - `cd percorso/relativo`: Va a un percorso relativo
  - `cd ..`: Sale di un livello (directory superiore)
  - `cd ~`: Torna alla directory home
  - `cd -`: Torna alla directory precedente

### Manipolazione di File e Directory
Comandi per creare, copiare, spostare ed eliminare file e directory:

#### Creazione:
- `mkdir` (Make Directory): Crea una nuova directory
  - `mkdir -p dir1/dir2/dir3`: Crea directory annidate
- `touch`: Crea un file vuoto o aggiorna la data di modifica

#### Copia e Spostamento:
- `cp` (Copy): Copia file o directory
  - `cp origine destinazione`: Copia un file
  - `cp -r origine destinazione`: Copia ricorsivamente (directory)
- `mv` (Move): Sposta o rinomina file o directory
  - `mv vecchio_nome nuovo_nome`: Rinomina
  - `mv file directory/`: Sposta un file in una directory

#### Eliminazione:
- `rm` (Remove): Elimina file
  - `rm -i file`: Chiede conferma prima di eliminare
  - `rm -r directory`: Elimina ricorsivamente una directory
  - `rm -f file`: Forza l'eliminazione senza chiedere conferma
- `rmdir`: Elimina directory vuote

### Visualizzazione e Modifica di File
Comandi per visualizzare e modificare il contenuto dei file:

#### Visualizzazione:
- `cat` (Concatenate): Mostra l'intero contenuto di un file
- `less`: Visualizza il contenuto pagina per pagina (q per uscire)
- `head`: Mostra le prime righe di un file (default: 10)
  - `head -n 5 file`: Mostra le prime 5 righe
- `tail`: Mostra le ultime righe di un file
  - `tail -f file`: Segue l'aggiornamento del file in tempo reale

#### Modifica:
- `nano`: Editor di testo semplice e intuitivo
  - Ctrl+O: Salva
  - Ctrl+X: Esci
- `vim`: Editor di testo potente ma con curva di apprendimento ripida
- `gedit`: Editor grafico (se disponibile)

### Esercizio Pratico: Gestione di File e Directory
**Obiettivo**: Applicare i comandi di navigazione e manipolazione del filesystem

**Attività**:
1. Creare una struttura di directory per un progetto personale
2. Navigare tra le directory usando percorsi assoluti e relativi
3. Creare, copiare, spostare e rinominare file
4. Visualizzare il contenuto dei file creati
5. Modificare i file con un editor di testo

## Parte 5: Comandi Avanzati e Recap (0.5 ore)

### Reindirizzamento e Pipe
Questi meccanismi permettono di collegare comandi e file:

#### Reindirizzamento:
- `>`: Reindirizza l'output a un file (sovrascrive)
  - `echo "testo" > file.txt`: Scrive "testo" in file.txt
- `>>`: Aggiunge l'output a un file (append)
  - `echo "altra riga" >> file.txt`: Aggiunge "altra riga" a file.txt
- `<`: Reindirizza l'input da un file
  - `sort < file.txt`: Ordina il contenuto di file.txt

#### Pipe (|):
- Collega l'output di un comando all'input di un altro
- Esempio: `ls -l | grep "maggio"`: Elenca i file e filtra quelli che contengono "maggio"
- Esempio: `cat file.txt | sort | uniq`: Legge il file, ordina le righe e rimuove i duplicati

### Ricerca di File e Contenuti
Comandi per trovare file e cercare testo all'interno dei file:

#### Ricerca di File:
- `find`: Cerca file e directory
  - `find . -name "*.txt"`: Trova tutti i file .txt nella directory corrente e sottodirectory
  - `find /home -type d -name "progetti"`: Trova directory chiamate "progetti" in /home
  - `find . -size +10M`: Trova file più grandi di 10 MB

#### Ricerca di Contenuti:
- `grep`: Cerca testo all'interno dei file
  - `grep "parola" file.txt`: Trova "parola" in file.txt
  - `grep -i "parola" file.txt`: Ignora maiuscole/minuscole
  - `grep -r "parola" .`: Cerca ricorsivamente in tutti i file

### Riepilogo e Risorse
Facciamo un riepilogo dei concetti chiave appresi:

#### Concetti Fondamentali:
- Sistemi operativi e loro funzioni
- Struttura gerarchica del filesystem
- Shell come interfaccia a riga di comando
- Comandi base per navigazione e manipolazione
- Tecniche avanzate (reindirizzamento, pipe, ricerca)

#### Risorse per Approfondire:
- Documentazione ufficiale (man pages): `man comando`
- Tutorial online: Linux Journey, The Linux Command Line
- Cheat sheet di comandi bash
- Comunità e forum: Stack Overflow, Reddit r/linux4noobs

### Esercizio Finale: Mini-Progetto "Organizzatore"
**Obiettivo**: Combinare più comandi in un flusso di lavoro completo

**Attività**:
1. Creare una directory "Organizzatore" con sottodirectory per categorie (Documenti, Immagini, Script)
2. Creare file di esempio in ciascuna categoria
3. Scrivere uno script semplice che mostri il contenuto di tutte le categorie
4. Utilizzare reindirizzamento per salvare l'output in un file di riepilogo
5. Cercare parole specifiche nei file creati

## Conclusione
In questa sessione abbiamo esplorato i concetti fondamentali dei sistemi operativi, del filesystem e della shell. Queste conoscenze sono essenziali per comprendere come funzionano i container Docker, che vedremo nelle prossime esercitazioni.

Nella prossima sessione introduttiva, approfondiremo i concetti di reti, applicazioni e servizi, completando così la preparazione necessaria per il corso su Docker e containerizzazione.
