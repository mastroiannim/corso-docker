## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1e  |  **Tipo**: Teoria

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Modulo 1d — Shell Avanzata](intro-01d-shell-avanzata.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Riepilogare tutti i concetti fondamentali della Sessione 1
- Collegare i concetti di OS, filesystem e shell a Docker
- Comprendere perché queste basi sono necessarie per la containerizzazione

---

# Modulo 1e: Riepilogo e Connessioni verso Docker

## Riepilogo dei Concetti Chiave

### Sistemi Operativi
- Il sistema operativo gestisce hardware, memoria, processi e filesystem
- Linux è il sistema operativo su cui si basa Docker
- Alpine Linux è una distribuzione leggera, ideale per container

### Filesystem
- Organizzazione gerarchica ad albero con radice `/`
- Percorsi assoluti (da `/`) e relativi (dalla directory corrente)
- Permessi `rwx` per proprietario, gruppo e altri

### Shell
- Interfaccia testuale per comunicare con il sistema operativo
- Comandi per navigare (`cd`, `ls`, `pwd`), manipolare file (`cp`, `mv`, `rm`, `mkdir`) e cercare (`find`, `grep`)
- Pipe (`|`) e redirezione (`>`, `>>`) per combinare comandi

## Perché Tutto Questo è Importante per Docker?

Ogni concetto che hai imparato ha un collegamento diretto con Docker:

| Concetto | Collegamento con Docker |
|----------|------------------------|
| **Sistema operativo** | I container Docker condividono il kernel Linux dell'host |
| **Filesystem** | Ogni container ha il proprio filesystem isolato; i volumi Docker montano directory dell'host |
| **Permessi** | La sicurezza dei container si basa sui permessi Linux |
| **Shell** | Tutti i comandi Docker si eseguono da terminale |
| **Pipe e redirect** | Utili per filtrare log dei container e automatizzare operazioni |
| **Grep e find** | Indispensabili per debug e diagnostica dei container |

## Anteprima: Cosa Vedremo nelle Prossime Sessioni

```
Sessione 1 (completata ✅)     Sessione 2 (prossima)          Fase Docker
OS, Filesystem, Shell    →    Reti, Servizi, VM          →    Container!
                                                                │
Sai usare il terminale        Imparerai come le macchine       Docker usa tutto:
e gestire file               comunicano in rete               shell + rete + filesystem
```

### Nella Sessione 2 imparerai:
- Come funzionano le reti (IP, porte, protocolli)
- Il modello client-server
- Cos'è la virtualizzazione e come si confronta con i container
- Come configurare le modalità di rete in VirtualBox

### Nelle Esercitazioni Docker imparerai:
- A creare e gestire container
- A scrivere Dockerfile
- A usare Docker Compose per applicazioni multi-servizio

## Mini-Progetto Finale: "Organizzatore"

**Obiettivo**: Combinare più comandi in un flusso di lavoro completo

```bash
# 1. Crea la struttura
mkdir -p ~/Organizzatore/{Documenti,Immagini,Script}

# 2. Crea file di esempio
echo "Lezione di oggi: OS e filesystem" > ~/Organizzatore/Documenti/lezione1.txt
echo "Lezione di domani: reti e servizi" > ~/Organizzatore/Documenti/lezione2.txt
echo '#!/bin/sh' > ~/Organizzatore/Script/lista.sh
echo 'find ~/Organizzatore -type f' >> ~/Organizzatore/Script/lista.sh
chmod +x ~/Organizzatore/Script/lista.sh

# 3. Genera un report
echo "=== Report Organizzatore ===" > ~/Organizzatore/report.txt
echo "Data: $(date)" >> ~/Organizzatore/report.txt
echo "File presenti:" >> ~/Organizzatore/report.txt
find ~/Organizzatore -type f >> ~/Organizzatore/report.txt
echo "---" >> ~/Organizzatore/report.txt
echo "Contenuto documenti:" >> ~/Organizzatore/report.txt
cat ~/Organizzatore/Documenti/*.txt >> ~/Organizzatore/report.txt

# 4. Visualizza il report
cat ~/Organizzatore/report.txt
```

### ✅ Checkpoint

```bash
find ~/Organizzatore -type f | wc -l
```

**Output atteso**: `5` (lezione1.txt, lezione2.txt, lista.sh, report.txt + il report che include se stesso)

```bash
grep -r "Lezione" ~/Organizzatore/Documenti/
```

**Output atteso**: due righe, una per ogni file di lezione.

### Risorse per Approfondire:
- Documentazione ufficiale (man pages): `man comando`
- Tutorial online: Linux Journey, The Linux Command Line
- [CHEATSHEET del corso](../CHEATSHEET.md)

---

## ➡️ Prossimi passi

- **Continua con**: [Sessione Introduttiva 2 — Reti, Servizi e VM](../fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md)
- **Metti in pratica**: [Lab Sessione 1](intro-01-lab-os-filesystem-shell.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice della sessione](intro-01-teoria-os-filesystem-shell.md) · [Torna all'indice principale](../README.md)
