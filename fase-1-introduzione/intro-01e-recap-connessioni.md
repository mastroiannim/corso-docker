## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 1e  |  **Tipo**: Teoria

### ✅ Prerequisiti

Prima di iniziare questo modulo devi aver completato:
- [ ] [Modulo 1d — Shell Avanzata](intro-01d-shell-avanzata.md)

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Riepilogare in modo operativo i prerequisiti della Sessione 1
- Collegare OS, filesystem e shell alle attività Docker successive
- Verificare la tua prontezza prima della Sessione 2 e della Fase Docker

---

# Modulo 1e: Riepilogo e Connessioni verso Docker (essenziale)

## Checklist di uscita dalla Sessione 1

Se sai fare questi passaggi, sei pronto a proseguire:

- navigare nel filesystem (`pwd`, `cd`, `ls`)
- creare/spostare/eliminare file e directory (`mkdir`, `cp`, `mv`, `rm`)
- leggere e filtrare contenuti (`cat`, `grep`, `find`)
- combinare comandi con pipe e redirect (`|`, `>`, `>>`)

## Collegamento diretto con Docker

| Base Sessione 1 | Uso immediato in Docker |
|---|---|
| Shell | esecuzione comandi `docker` |
| Filesystem | volumi, bind mount, path host/container |
| Processi | ciclo di vita dei container |
| Pipe/grep/find | debug log e diagnostica |

## Checkpoint finale rapido

Esegui su Alpine Linux:

```bash
mkdir -p ~/check-docker/base
echo "ok-sessione-1" > ~/check-docker/base/stato.txt
cat ~/check-docker/base/stato.txt | grep ok
find ~/check-docker -name "stato.txt"
```

**Output atteso**:
- `grep` restituisce `ok-sessione-1`
- `find` restituisce il percorso completo di `stato.txt`

## Prossimo ponte didattico

1. Sessione 2: rete, servizi, VM in versione essenziale
2. Lab Sessione 2: applicazione pratica completa
3. Fase Docker: avvio container e networking

## Approfondimento opzionale (archivio)

Versione estesa originale del modulo:
- [Modulo 1e esteso (archivio)](../docs/archivio/intro-01e-recap-connessioni-esteso.md)

---

## ➡️ Prossimi passi

- **Continua con**: [Sessione Introduttiva 2 — Reti, Servizi e VM](../fase-2-reti-servizi/intro-02-teoria-reti-servizi-vm.md)
- **Metti in pratica**: [Lab Sessione 1](intro-01-lab-os-filesystem-shell.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice della sessione](intro-01-teoria-os-filesystem-shell.md) · [Torna all'indice principale](../README.md)
