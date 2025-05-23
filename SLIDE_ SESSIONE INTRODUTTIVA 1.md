# SLIDE: SESSIONE INTRODUTTIVA 1
## SISTEMI OPERATIVI, FILESYSTEM E SHELL

---

### Slide 1: Introduzione alla Sessione
- **Titolo:** Sistemi Operativi, Filesystem e Shell: Le Fondamenta dell'Informatica
- **Immagine:** Rappresentazione visiva di un edificio con fondamenta etichettate come "SO", "Filesystem" e "Shell"
- **Obiettivi della sessione:**
  - Comprendere il ruolo e le funzioni dei sistemi operativi
  - Esplorare la struttura del filesystem e come navigarlo
  - Imparare a utilizzare la shell e i comandi bash fondamentali
  - Acquisire competenze pratiche attraverso esercizi guidati

---

## PARTE 1: INTRODUZIONE AI SISTEMI OPERATIVI

### Slide 2: Cos'è un Sistema Operativo?
- **Titolo:** Il Sistema Operativo: Il Direttore d'Orchestra del Computer
- **Immagine:** Un direttore d'orchestra che coordina diversi musicisti (rappresentanti hardware e software)
- **Contenuto:**
  - Il software fondamentale che gestisce tutte le risorse del computer
  - Intermediario tra l'utente e l'hardware
  - Coordina e assegna risorse ai programmi
  - **Collegamento con Docker:** I container Docker condividono il kernel del sistema operativo host

---

### Slide 3: Le Funzioni Principali di un Sistema Operativo
- **Titolo:** Cosa fa un Sistema Operativo?
- **Contenuto con icone:**
  - 🖥️ **Gestione dell'hardware:** Controlla e coordina i dispositivi
  - 📊 **Gestione della memoria:** Alloca e libera la RAM
  - ⚙️ **Gestione dei processi:** Avvia, monitora e termina i programmi
  - 📁 **Gestione del filesystem:** Organizza e accede ai dati
  - 👤 **Interfaccia utente:** Permette l'interazione con il computer
- **Verifica:** Quali di queste funzioni sono cruciali per Docker?

---

### Slide 4: I Principali Sistemi Operativi
- **Titolo:** I Tre Grandi: Windows, macOS e Linux
- **Immagini:** Loghi dei tre sistemi operativi
- **Contenuto:** Tabella comparativa con:
  - Diffusione e utilizzo tipico
  - Interfaccia caratteristica
  - Modello di sviluppo (proprietario vs open source)
  - Punti di forza
- **Esercizio rapido:** Identifica quale sistema operativo stai utilizzando e due sue caratteristiche principali

---

### Slide 5: Focus su Linux
- **Titolo:** Perché Linux è Importante per Docker
- **Contenuto:**
  - Sistema open source e altamente personalizzabile
  - Base per la maggior parte dei server web
  - Kernel Linux: fondamento dei container Docker
  - Efficienza e leggerezza
  - Ampia comunità di sviluppatori
- **Collegamento con Docker:** Docker è nato su Linux e sfrutta le sue funzionalità native

---

### Slide 6: Metafora Visiva
- **Titolo:** Il Sistema Operativo come Ecosistema
- **Immagine:** Infografica di un ecosistema dove:
  - Il terreno rappresenta l'hardware
  - Il clima e l'acqua rappresentano il sistema operativo
  - Le piante rappresentano le applicazioni
  - Gli animali rappresentano i processi utente
- **Discussione:** Come cambierebbe questa metafora quando parliamo di container?

---

### Slide 7: Architettura di un Sistema Operativo
- **Titolo:** Dentro il Sistema Operativo
- **Immagine:** Diagramma a strati dell'architettura di un SO
- **Contenuto:**
  - Kernel: il cuore del sistema operativo
  - Driver: comunicazione con l'hardware
  - Servizi di sistema: funzionalità di base
  - Shell: interfaccia per l'utente
- **Collegamento con Docker:** I container condividono il kernel ma hanno servizi isolati

---

### Slide 8: Verifica della Comprensione
- **Titolo:** Mettiamoci alla Prova!
- **Contenuto:** Quiz interattivo con domande come:
  - Quale componente gestisce l'allocazione della memoria?
  - Perché Linux è importante per Docker?
  - Quale metafora descrive meglio il ruolo del sistema operativo?
  - Quali sono le differenze principali tra Windows e Linux?
- **Feedback:** Risposte commentate per ogni domanda

---

## PARTE 2: FILESYSTEM E ORGANIZZAZIONE DEI DATI

### Slide 9: Il Filesystem: La Biblioteca del Computer
- **Titolo:** Come il Computer Organizza i Dati
- **Immagine:** Biblioteca con scaffali, sezioni e libri
- **Contenuto:**
  - Definizione di filesystem
  - Organizzazione gerarchica
  - Metafora: scaffali (directory) e libri (file)
- **Collegamento con Docker:** I container hanno il proprio filesystem isolato

---

### Slide 10: Struttura ad Albero
- **Titolo:** L'Albero del Filesystem
- **Immagine:** Diagramma ad albero del filesystem
- **Contenuto:**
  - Directory root (/)
  - Directory principali e loro funzioni
  - Relazioni padre-figlio tra directory
- **Esercizio rapido:** Disegna la struttura delle directory della tua home

---

### Slide 11: Percorsi nel Filesystem
- **Titolo:** Come Trovare la Strada: Percorsi Assoluti e Relativi
- **Immagine:** Mappa di una città con indicazioni stradali
- **Contenuto:**
  - Percorsi assoluti: partono dalla root (/)
    - Esempio: `/home/utente/documenti/file.txt`
  - Percorsi relativi: partono dalla posizione attuale
    - Esempio: `documenti/file.txt` (se siamo in /home/utente)
  - Simboli speciali: `.` (directory corrente), `..` (directory superiore), `~` (home)
- **Verifica:** Converti questi percorsi da assoluti a relativi e viceversa

---

### Slide 12: Permessi e Proprietà
- **Titolo:** Chi Può Fare Cosa: Permessi dei File
- **Immagine:** Sistema di chiavi e serrature
- **Contenuto:**
  - Proprietario, gruppo e altri
  - Permessi di lettura, scrittura ed esecuzione
  - Rappresentazione numerica e simbolica
- **Collegamento con Docker:** I permessi nei container e la sicurezza

---

### Slide 13-14: Esercizio Pratico Guidato
- **Titolo:** Esploriamo il Filesystem
- **Contenuto:**
  - Istruzioni passo-passo per esplorare il filesystem
  - Screenshot dell'interfaccia grafica
  - Attività di identificazione di percorsi e permessi
- **Verifica:** Completa la tabella con i percorsi e i permessi dei file trovati

---

## PARTE 3: INTRODUZIONE ALLA SHELL

### Slide 15: La Shell: Parlare con il Computer
- **Titolo:** La Shell: Una Conversazione Diretta con il Computer
- **Immagine:** Fumetti di dialogo tra utente e computer
- **Contenuto:**
  - Cos'è una shell e a cosa serve
  - Terminal vs shell
  - Prompt dei comandi
- **Collegamento con Docker:** La shell è l'interfaccia principale per interagire con Docker

---

### Slide 16: Perché Usare la Riga di Comando?
- **Titolo:** GUI vs CLI: Quando e Perché?
- **Contenuto:** Tabella comparativa con vantaggi di entrambi:
  - GUI: intuitiva, visuale, facile per principianti
  - CLI: potente, automatizzabile, efficiente, precisa
- **Discussione:** In quali situazioni preferiresti usare la CLI rispetto alla GUI?

---

### Slide 17: Anatomia di un Comando
- **Titolo:** Come Dare Ordini al Computer
- **Immagine:** Diagramma di un comando scomposto
- **Contenuto:**
  - Comando: l'azione da eseguire
  - Opzioni: modificatori (con - o --)
  - Argomenti: su cosa agire
- **Esempio:** `ls -la /home/utente`
  - Comando: `ls`
  - Opzioni: `-la` (formato lungo, mostra file nascosti)
  - Argomento: `/home/utente` (directory da elencare)

---

### Slide 18-19: Primi Comandi
- **Titolo:** I Nostri Primi Comandi
- **Contenuto:**
  - `echo`: far ripetere qualcosa al computer
  - `date`: visualizzare data e ora
  - `whoami`: chi sono io nel sistema
  - `clear`: pulire lo schermo
  - Esempi di output per ciascun comando
- **Esercizio guidato:** Prova questi comandi e annota l'output

---

### Slide 20: Esercizio Pratico
- **Titolo:** Proviamo Insieme!
- **Contenuto:** Istruzioni passo-passo per:
  - Aprire il terminale
  - Eseguire i comandi base
  - Interpretare l'output
- **Verifica:** Completa la tabella con i comandi eseguiti e il loro output

---

## PARTE 4: COMANDI BASH FONDAMENTALI

### Slide 21-22: Navigazione nel Filesystem
- **Titolo:** Muoversi nel Filesystem
- **Immagine:** Mappa con percorsi e punti di riferimento
- **Contenuto:**
  - `pwd`: dove mi trovo? (Print Working Directory)
  - `ls`: cosa c'è qui? (List)
  - `cd`: andiamo altrove (Change Directory)
  - Esempi pratici di navigazione
- **Collegamento con Docker:** Navigazione all'interno di un container

---

### Slide 23-24: Manipolazione di File e Directory
- **Titolo:** Creare, Spostare, Copiare e Cancellare
- **Contenuto:**
  - `mkdir`: creare directory
  - `touch`: creare file vuoti
  - `cp`: copiare file
  - `mv`: spostare/rinominare file
  - `rm`: eliminare file
  - Esempi e sintassi per ciascun comando
- **Esercizio guidato:** Crea una struttura di directory e file seguendo lo schema fornito

---

### Slide 25-26: Visualizzazione e Modifica di File
- **Titolo:** Leggere e Modificare i File
- **Contenuto:**
  - `cat`: visualizzare contenuto completo
  - `less`: visualizzare pagina per pagina
  - `head`/`tail`: inizio/fine del file
  - `nano`: editor di testo semplice
  - Esempi di utilizzo
- **Collegamento con Docker:** Visualizzazione dei file di configurazione nei container

---

### Slide 27-28: Esercizio Pratico Guidato
- **Titolo:** Mettiamo in Pratica!
- **Contenuto:** Tutorial passo-passo per:
  - Creare una struttura di directory
  - Creare e manipolare file
  - Navigare tra le directory
  - Visualizzare contenuti
- **Verifica:** Completa l'esercizio e confronta il risultato con quello atteso

---

## PARTE 5: COMANDI AVANZATI E RECAP

### Slide 29-30: Reindirizzamento e Pipe
- **Titolo:** Collegare i Comandi: Tubature Digitali
- **Immagine:** Sistema di tubature che collegano diversi strumenti
- **Contenuto:**
  - `>`: reindirizzare output a file (sovrascrive)
  - `>>`: aggiungere output a file
  - `|`: pipe, collegare output di un comando all'input di un altro
  - Esempi pratici di combinazioni
- **Collegamento con Docker:** Reindirizzamento dell'output nei comandi Docker

---

### Slide 31: Ricerca di File e Contenuti
- **Titolo:** Trovare l'Ago nel Pagliaio
- **Contenuto:**
  - `find`: cercare file e directory
  - `grep`: cercare testo all'interno dei file
  - Esempi di ricerche comuni
- **Esercizio guidato:** Trova tutti i file con una certa estensione e cerca una parola specifica al loro interno

---

### Slide 32-33: Riepilogo e Risorse
- **Titolo:** Cosa Abbiamo Imparato
- **Contenuto:**
  - Mappa concettuale dei concetti chiave
  - Riferimenti rapidi per i comandi
  - Risorse per approfondire
- **Collegamento con la prossima sessione:** Come questi concetti si collegano a reti e servizi

---

### Slide 34: Esercizio Finale
- **Titolo:** Sfida Finale
- **Contenuto:** Mini-progetto che combina più comandi:
  - Creare una struttura di directory
  - Popolarla con file di testo
  - Cercare informazioni specifiche
  - Salvare i risultati in un nuovo file
- **Verifica finale:** Quiz di autovalutazione sui concetti chiave della sessione

---

## ESERCIZI PRATICI DETTAGLIATI

### Esercizio 1: Esplorazione del Sistema Operativo
**Obiettivo:** Familiarizzare con i componenti del sistema operativo
**Materiali:** Computer con sistema operativo installato
**Durata:** 15 minuti

**Istruzioni:**
1. Identifica il sistema operativo in uso
2. Trova e apri le impostazioni di sistema
3. Localizza le informazioni su:
   - Versione del sistema operativo
   - Tipo di processore
   - Quantità di RAM
   - Spazio su disco disponibile
4. Identifica almeno tre componenti visibili dell'interfaccia e descrivi la loro funzione
5. Compila la scheda di identificazione del sistema

**Verifica:**
- Hai identificato correttamente tutti i componenti richiesti?
- Sei in grado di spiegare la funzione di ciascun componente?
- Comprendi la differenza tra hardware e software?

---

### Esercizio 2: Navigazione nel Filesystem
**Obiettivo:** Praticare la navigazione tramite shell
**Materiali:** Terminale/shell
**Durata:** 20 minuti

**Istruzioni:**
1. Apri il terminale
2. Usa `pwd` per identificare la directory corrente
3. Usa `ls -la` per visualizzare tutti i file, inclusi quelli nascosti
4. Naviga nella directory home con `cd ~`
5. Crea una nuova directory chiamata "esercizio_shell" con `mkdir esercizio_shell`
6. Entra nella nuova directory con `cd esercizio_shell`
7. Crea tre sottodirectory: "documenti", "immagini" e "script"
8. Naviga in ciascuna sottodirectory e torna alla directory principale
9. Usa percorsi assoluti e relativi per muoverti tra le directory

**Verifica:**
- Sei in grado di navigare efficacemente usando sia percorsi assoluti che relativi?
- Puoi elencare i contenuti di una directory con diverse opzioni?
- Comprendi la struttura gerarchica del filesystem?

---

### Esercizio 3: Creazione e Manipolazione di File
**Obiettivo:** Applicare i comandi di gestione file
**Materiali:** Terminale/shell
**Durata:** 25 minuti

**Istruzioni:**
1. Nella directory "esercizio_shell" creata nell'esercizio precedente:
2. Crea un file vuoto chiamato "note.txt" nella directory "documenti" usando `touch`
3. Usa `echo "Questo è un test" > documenti/note.txt` per aggiungere testo al file
4. Visualizza il contenuto del file con `cat documenti/note.txt`
5. Copia il file nella directory "immagini" con `cp documenti/note.txt immagini/`
6. Rinomina il file nella directory "immagini" in "backup.txt" usando `mv`
7. Aggiungi una seconda riga al file originale con `echo "Seconda riga" >> documenti/note.txt`
8. Confronta i due file usando `cat`
9. Crea un nuovo file nella directory "script" chiamato "hello.sh" con il seguente contenuto:
   ```bash
   #!/bin/bash
   echo "Hello, World!"
   ```
10. Rendi il file eseguibile con `chmod +x script/hello.sh`
11. Esegui lo script con `./script/hello.sh`

**Verifica:**
- Hai creato, modificato e visualizzato correttamente i file?
- Comprendi la differenza tra `>` e `>>`?
- Sei in grado di spiegare i permessi di esecuzione?

---

### Esercizio 4: Mini-Progetto "Organizzatore"
**Obiettivo:** Combinare più comandi in un flusso di lavoro
**Materiali:** Terminale/shell
**Durata:** 30 minuti

**Istruzioni:**
1. Crea una directory chiamata "Organizzatore" con tre sottodirectory: "Testi", "Dati" e "Risultati"
2. In "Testi", crea tre file: "file1.txt", "file2.txt" e "file3.txt"
3. Aggiungi contenuto diverso a ciascun file, assicurandoti che la parola "importante" appaia in almeno due file
4. In "Dati", crea un file "numeri.txt" con una lista di numeri (uno per riga)
5. Usa il comando `sort` per ordinare i numeri e salvare il risultato in "Risultati/numeri_ordinati.txt"
6. Usa `grep` per trovare tutte le occorrenze della parola "importante" nei file di testo
7. Salva i risultati della ricerca in "Risultati/parole_trovate.txt"
8. Crea un file di riepilogo in "Risultati/riepilogo.txt" che contenga:
   - Il numero di file in ciascuna directory
   - Il numero totale di righe in tutti i file
   - La data e l'ora di completamento dell'esercizio

**Verifica:**
- Hai completato tutte le operazioni richieste?
- I file di risultato contengono le informazioni corrette?
- Sei in grado di spiegare il flusso di lavoro e i comandi utilizzati?

---

## VERIFICHE DI COMPRENSIONE

### Quiz 1: Sistemi Operativi
1. Qual è la funzione principale di un sistema operativo?
2. Elenca tre differenze tra Windows e Linux.
3. Perché Linux è importante per Docker?
4. Cosa si intende per "kernel" di un sistema operativo?
5. Quali risorse hardware gestisce il sistema operativo?

### Quiz 2: Filesystem
1. Cosa rappresenta la directory root (/) in un filesystem Linux?
2. Qual è la differenza tra percorso assoluto e relativo?
3. Spiega il significato dei simboli `.`, `..` e `~` nei percorsi.
4. Cosa indicano i permessi "rwxr-xr--" per un file?
5. Come si rappresentano numericamente i permessi "rwxr-xr--"?

### Quiz 3: Shell e Comandi Bash
1. Qual è la differenza tra shell e terminale?
2. Spiega la struttura di un comando bash (comando, opzioni, argomenti).
3. Quale comando useresti per:
   - Visualizzare la directory corrente?
   - Creare una nuova directory?
   - Copiare un file?
   - Visualizzare il contenuto di un file?
4. Cosa fa il comando `ls -la`?
5. Qual è la differenza tra `>` e `>>`?

### Quiz 4: Comandi Avanzati
1. Come funziona il pipe (`|`) e a cosa serve?
2. Come cerchi tutti i file .txt in una directory e sottodirectory?
3. Come cerchi la parola "errore" all'interno di un file?
4. Spiega come combinare `grep` e `|` per filtrare l'output di un comando.
5. Come rendi eseguibile uno script bash?

---

## COLLEGAMENTI CON LE ALTRE SESSIONI

### Collegamenti con la Sessione Introduttiva 2
- I concetti di filesystem si collegano alla gestione delle reti e dei servizi
- La shell sarà utilizzata per eseguire comandi di rete e diagnostica
- I permessi dei file sono correlati alla sicurezza delle applicazioni e dei servizi

### Collegamenti con l'Esercitazione 1 su Docker
- La comprensione del sistema operativo è fondamentale per capire come Docker interagisce con il kernel
- I concetti di filesystem si applicano ai volumi Docker e alla persistenza dei dati
- I comandi bash sono la base per i comandi Docker CLI

### Collegamenti con l'Esercitazione 2 su Docker
- La manipolazione di file e directory si applica alla creazione di Dockerfile
- I concetti di permessi sono cruciali per la sicurezza dei container
- Il reindirizzamento e le pipe sono utilizzati frequentemente con Docker

### Collegamenti con l'Esercitazione 3 su Docker
- La gestione avanzata dei file si applica alla configurazione di Docker Compose
- I concetti di processi sono correlati all'orchestrazione dei container
- Le tecniche di ricerca e filtro sono utili per il debugging di applicazioni containerizzate
