## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1d  |  **Tipo**: Teoria

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Modulo 1c — Shell Base](intro-01c-shell-base.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Usare la redirezione dell'output (`>`, `>>`, `<`)
- Collegare comandi con le pipe (`|`)
- Cercare file con `find` e testo con `grep`
- Combinare più comandi in sequenze utili

---

# Modulo 1d: Shell Avanzata — Pipe, Redirect, Grep, Find

## Reindirizzamento

Questi meccanismi permettono di collegare comandi e file:

### Reindirizzamento dell'Output:
- `>`: Reindirizza l'output a un file (sovrascrive)
  - `echo "testo" > file.txt`: Scrive "testo" in file.txt
- `>>`: Aggiunge l'output a un file (append)
  - `echo "altra riga" >> file.txt`: Aggiunge "altra riga" a file.txt

### Reindirizzamento dell'Input:
- `<`: Reindirizza l'input da un file
  - `sort < file.txt`: Ordina il contenuto di file.txt

## Pipe (`|`)

La pipe collega l'output di un comando all'input di un altro. È uno dei meccanismi più potenti della shell.

### Esempi:
```bash
# Elenca i file e filtra quelli che contengono "maggio"
ls -l | grep "maggio"

# Legge il file, ordina le righe e rimuove i duplicati
cat file.txt | sort | uniq

# Conta il numero di file in una directory
ls | wc -l

# Mostra i 5 processi che usano più memoria
ps aux | sort -k4 -rn | head -5
```

## Ricerca di File con `find`

Il comando `find` cerca file e directory nel filesystem:

```bash
# Trova tutti i file .txt nella directory corrente e sottodirectory
find . -name "*.txt"

# Trova directory chiamate "progetti" in /home
find /home -type d -name "progetti"

# Trova file più grandi di 10 MB
find . -size +10M

# Trova file modificati negli ultimi 7 giorni
find . -mtime -7
```

## Ricerca di Contenuti con `grep`

Il comando `grep` cerca testo all'interno dei file:

```bash
# Cerca "parola" in un file
grep "parola" file.txt

# Ignora maiuscole/minuscole
grep -i "parola" file.txt

# Cerca ricorsivamente in tutti i file
grep -r "parola" .

# Mostra il numero di riga dove si trova la corrispondenza
grep -n "parola" file.txt

# Cerca e conta le occorrenze
grep -c "parola" file.txt
```

## Combinare i Comandi

La vera potenza della shell sta nel combinare questi strumenti:

```bash
# Trova tutti i file .conf e cerca quelli che contengono "server"
find /etc -name "*.conf" | xargs grep "server"

# Elenca tutti i processi attivi che contengono "docker"
ps aux | grep docker

# Salva la lista dei file grandi in un report
find / -size +100M 2>/dev/null > ~/report-file-grandi.txt
```

## Lab Rapido

Esegui in ordine su Alpine Linux:

```bash
# 1. Crea file di test
cd ~/progetto
echo "mela" > frutta.txt
echo "banana" >> frutta.txt
echo "arancia" >> frutta.txt
echo "mela" >> frutta.txt
echo "kiwi" >> frutta.txt

# 2. Ordina e rimuovi duplicati
sort frutta.txt | uniq

# 3. Conta le righe
wc -l frutta.txt

# 4. Cerca un frutto specifico
grep "mela" frutta.txt

# 5. Trova file creati di recente
find ~/progetto -name "*.txt"

# 6. Combinazione: trova e conta i file .txt
find ~/progetto -name "*.txt" | wc -l

# 7. Salva un report
echo "=== Report Progetto ===" > ~/report.txt
echo "File nella directory:" >> ~/report.txt
ls -la ~/progetto >> ~/report.txt
echo "---" >> ~/report.txt
echo "File .txt trovati:" >> ~/report.txt
find ~/progetto -name "*.txt" >> ~/report.txt
cat ~/report.txt
```

### ✅ Checkpoint

```bash
sort frutta.txt | uniq | wc -l
```

**Output atteso**: `4` (mela, banana, arancia, kiwi — senza duplicati)

```bash
grep -c "mela" frutta.txt
```

**Output atteso**: `2` (mela appare due volte nel file)

---

## ➡️ Prossimi passi

- **Continua con**: [Modulo 1e — Recap e Connessioni](intro-01e-recap-connessioni.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice della sessione](intro-01-teoria-os-filesystem-shell.md) · [Torna all'indice principale](../README.md)
