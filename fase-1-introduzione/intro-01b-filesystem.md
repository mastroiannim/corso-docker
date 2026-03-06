## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1b  |  **Tipo**: Teoria

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Modulo 1a — Sistemi Operativi](intro-01a-sistemi-operativi.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Descrivere la struttura gerarchica del filesystem Linux
- Distinguere tra percorsi assoluti e relativi
- Comprendere il sistema di permessi (rwx) e la proprietà dei file

---

# Modulo 1b: Filesystem e Organizzazione dei Dati

## Struttura Gerarchica del Filesystem

Il filesystem è il modo in cui il sistema operativo organizza e gestisce i file. Possiamo pensarlo come un sistema di classificazione di una biblioteca, con scaffali (directory) che contengono libri (file).

### Concetti Fondamentali:
- **File**: Unità base di archiviazione (documenti, immagini, programmi, ecc.)
- **Directory (o cartella)**: Contenitore che può contenere file e altre directory
- **Struttura ad albero**: Organizzazione gerarchica che parte da una radice (root)

### Struttura del Filesystem in Linux:
- **/** (root): La directory principale da cui parte tutto il filesystem
- **/bin**: Comandi essenziali del sistema
- **/etc**: File di configurazione
- **/home**: Directory personali degli utenti
- **/var**: File variabili (log, cache, ecc.)
- **/tmp**: File temporanei
- **/usr**: Programmi e librerie installati

## Percorsi Assoluti e Relativi

Per identificare la posizione di un file o una directory nel filesystem, utilizziamo i percorsi:

### Percorso Assoluto:
- Parte dalla directory root (/)
- Specifica l'intero percorso fino al file/directory
- Esempio: `/home/utente/documenti/file.txt`
- È come dare un indirizzo completo partendo dalla città

### Percorso Relativo:
- Parte dalla directory corrente
- Non inizia con /
- Esempio: `documenti/file.txt` (se ci troviamo in /home/utente)
- È come dare indicazioni partendo dalla posizione attuale

### Simboli Speciali:
- `.` (punto): Directory corrente
- `..` (doppio punto): Directory superiore (parent)
- `~` (tilde): Directory home dell'utente

## Permessi e Proprietà

In Linux, ogni file e directory ha associati permessi che determinano chi può fare cosa:

### Proprietà:
- **Proprietario**: L'utente che ha creato il file
- **Gruppo**: Un gruppo di utenti con permessi specifici
- **Altri**: Tutti gli altri utenti del sistema

### Tipi di Permessi:
- **r (read)**: Permesso di lettura
- **w (write)**: Permesso di scrittura
- **x (execute)**: Permesso di esecuzione (per file) o accesso (per directory)

### Rappresentazione dei Permessi:
- Simbolica: `rwxr-xr--` (proprietario: rwx, gruppo: r-x, altri: r--)
- Numerica: 754 (proprietario: 7=rwx, gruppo: 5=r-x, altri: 4=r--)

## Esercizio di Verifica

**Obiettivo**: Comprendere la struttura del filesystem e i concetti di percorsi

**Attività**:
1. Esplorare il filesystem usando l'interfaccia grafica
2. Identificare le principali directory di sistema
3. Creare una struttura di directory personale
4. Scrivere percorsi assoluti e relativi per diversi file
5. Verificare i permessi di alcuni file e directory

### ✅ Checkpoint

Esegui su Alpine Linux:

```bash
ls -la /
```

**Output atteso**: una lista delle directory principali (`bin`, `etc`, `home`, `var`, `tmp`, `usr`) con i relativi permessi.

```bash
stat /etc/hostname
```

**Output atteso**: informazioni dettagliate sul file, inclusi proprietario, gruppo e permessi.

---

## ➡️ Prossimi passi

- **Continua con**: [Modulo 1c — Shell Base](intro-01c-shell-base.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice della sessione](intro-01-teoria-os-filesystem-shell.md) · [Torna all'indice principale](../README.md)
