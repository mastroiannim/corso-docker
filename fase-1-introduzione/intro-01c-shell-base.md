## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1c  |  **Tipo**: Teoria

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Modulo 1b — Filesystem](intro-01b-filesystem.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Spiegare cos'è una shell e perché è utile
- Eseguire comandi base: `ls`, `cd`, `mkdir`, `cat`, `cp`, `mv`, `rm`
- Navigare nel filesystem da terminale
- Creare, copiare, spostare ed eliminare file e directory

---

# Modulo 1c: Shell — Comandi Fondamentali

## Cos'è una Shell?

La shell è un'interfaccia testuale che permette di comunicare con il sistema operativo attraverso comandi testuali. Possiamo pensarla come una "conversazione" diretta con il computer.

### Terminologia:
- **Terminal (o terminale)**: Il programma che fornisce l'interfaccia per la shell
- **Shell**: L'interprete dei comandi (bash, zsh, sh, ecc.)
- **Prompt**: L'indicatore che la shell è pronta a ricevere comandi

### Tipi di Shell Comuni:
- **Bash** (Bourne Again SHell): La shell più comune nei sistemi Linux
- **Zsh**: Shell avanzata con funzionalità aggiuntive
- **Fish**: Shell user-friendly con suggerimenti colorati
- **PowerShell**: Shell di Windows con funzionalità avanzate

## Vantaggi dell'Interfaccia a Riga di Comando

Perché usare la shell quando esiste un'interfaccia grafica?

### Vantaggi della CLI (Command Line Interface):
- **Potenza**: Operazioni complesse con pochi comandi
- **Automazione**: Script per automatizzare attività ripetitive
- **Efficienza**: Meno risorse di sistema rispetto all'interfaccia grafica
- **Precisione**: Controllo dettagliato delle operazioni
- **Accesso remoto**: Gestione di sistemi senza interfaccia grafica

### Quando Usare CLI vs GUI:
- **CLI**: Automazione, operazioni batch, gestione remota, sviluppo
- **GUI**: Editing grafico, navigazione intuitiva, multitasking visivo

## Anatomia di un Comando

```
comando [opzioni] [argomenti]
```
- **Comando**: L'azione da eseguire
- **Opzioni**: Modificatori che cambiano il comportamento (preceduti da - o --)
- **Argomenti**: Su cosa agire (file, directory, testo, ecc.)

### Comandi Semplici per Iniziare:
- `echo "Ciao mondo"`: Visualizza il testo "Ciao mondo"
- `date`: Mostra la data e l'ora correnti
- `whoami`: Mostra il nome dell'utente corrente
- `clear`: Pulisce lo schermo del terminale

## Navigazione nel Filesystem

I comandi per muoversi nel filesystem sono tra i più utilizzati:

### Comandi Principali:
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

## Manipolazione di File e Directory

### Creazione:
- `mkdir` (Make Directory): Crea una nuova directory
  - `mkdir -p dir1/dir2/dir3`: Crea directory annidate
- `touch`: Crea un file vuoto o aggiorna la data di modifica

### Copia e Spostamento:
- `cp` (Copy): Copia file o directory
  - `cp origine destinazione`: Copia un file
  - `cp -r origine destinazione`: Copia ricorsivamente (directory)
- `mv` (Move): Sposta o rinomina file o directory
  - `mv vecchio_nome nuovo_nome`: Rinomina
  - `mv file directory/`: Sposta un file in una directory

### Eliminazione:
- `rm` (Remove): Elimina file
  - `rm -i file`: Chiede conferma prima di eliminare
  - `rm -r directory`: Elimina ricorsivamente una directory
- `rmdir`: Elimina directory vuote

## Visualizzazione di File

- `cat` (Concatenate): Mostra l'intero contenuto di un file
- `less`: Visualizza il contenuto pagina per pagina (q per uscire)
- `head`: Mostra le prime righe di un file (default: 10)
  - `head -n 5 file`: Mostra le prime 5 righe
- `tail`: Mostra le ultime righe di un file
  - `tail -f file`: Segue l'aggiornamento del file in tempo reale

## Modifica di File

- `nano`: Editor di testo semplice e intuitivo
  - Ctrl+O: Salva
  - Ctrl+X: Esci
- `vi`: Editor di testo disponibile su tutti i sistemi Unix/Linux
  - `i`: Entra in modalità inserimento
  - `Esc` → `:wq`: Salva ed esci

## Lab Rapido

**Obiettivo**: Applicare i comandi di navigazione e manipolazione del filesystem

Esegui in ordine su Alpine Linux:

```bash
# 1. Verifica dove ti trovi
pwd

# 2. Crea una struttura di directory
mkdir -p ~/progetto/{documenti,immagini,script}

# 3. Naviga nella struttura
cd ~/progetto
ls -la

# 4. Crea file di esempio
echo "Questo è un documento" > documenti/readme.txt
touch immagini/foto.jpg
echo '#!/bin/sh' > script/saluta.sh
echo 'echo "Ciao!"' >> script/saluta.sh

# 5. Copia e sposta file
cp documenti/readme.txt documenti/backup.txt
mv documenti/backup.txt ~/progetto/

# 6. Verifica il risultato
ls -R ~/progetto
cat documenti/readme.txt
```

### ✅ Checkpoint

```bash
ls -R ~/progetto
```

**Output atteso**:
```
/root/progetto:
backup.txt  documenti  immagini  script

/root/progetto/documenti:
readme.txt

/root/progetto/immagini:
foto.jpg

/root/progetto/script:
saluta.sh
```

---

## ➡️ Prossimi passi

- **Continua con**: [Modulo 1d — Shell Avanzata](intro-01d-shell-avanzata.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice della sessione](intro-01-teoria-os-filesystem-shell.md) · [Torna all'indice principale](../README.md)
