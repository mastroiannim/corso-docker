## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 0  |  **Tipo**: Lab

### ✅ Prerequisiti

Prima di iniziare questo modulo devi avere:
- [ ] Un computer con Windows 11 (account utente standard, **non serve** essere amministratore)
- [ ] Connessione a Internet attiva
- [ ] Almeno 20GB di spazio libero su disco
- [ ] Almeno 8GB di RAM (16GB consigliati)

**Nota importante**: Questo è il **primo passo** del corso. Non hai bisogno di conoscenze pregresse di Linux, virtualizzazione o Docker.

### 🎯 Obiettivi di questo modulo

Al termine di questa sessione avrai:
- ✅ VirtualBox installato e funzionante sul tuo computer
- ✅ Alpine Linux installato in una macchina virtuale
- ✅ Configurazione di rete funzionante per comunicare con la VM
- ✅ SSH configurato per collegarsi alla VM da Windows
- ✅ Tutti gli strumenti necessari installati (editor di testo, git, docker)
- ✅ Un ambiente di lavoro pronto per seguire l'intero corso

---

# Laboratorio: Setup Completo dell'Ambiente di Lavoro

## Introduzione

Benvenuto! Questa è la prima sessione pratica del corso. In questo laboratorio configurerai **passo per passo** tutto l'ambiente di lavoro necessario per seguire il corso Docker.

### Cosa costruirai

Alla fine di questa sessione avrai:
1. **VirtualBox**: un software che permette di eseguire Linux sul tuo computer Windows
2. **Alpine Linux**: una versione leggera di Linux ottimizzata per container e virtualizzazione
3. **Accesso SSH**: un modo comodo per controllare Linux dal terminale Windows
4. **Strumenti di sviluppo**: tutto il necessario per scrivere codice e usare Docker

### Perché Alpine Linux?

Alpine Linux è una distribuzione Linux:
- **Leggera**: occupa pochissimo spazio (circa 130MB contro i 4-5GB di Ubuntu)
- **Veloce**: si avvia in pochi secondi
- **Sicura**: progettata con la sicurezza in mente
- **Ideale per Docker**: è la base usata dalla maggior parte delle immagini Docker ufficiali

### Tempistiche

Questa sessione richiede circa **90-120 minuti**, suddivisi in:
- Download e installazione VirtualBox: 15 minuti
- Download Alpine Linux: 5 minuti
- Creazione e configurazione della VM: 15 minuti
- Installazione Alpine Linux: 20 minuti
- Configurazione rete e SSH: 20 minuti
- Installazione strumenti di lavoro: 30 minuti
- Test finale e troubleshooting: 15 minuti

---

## Parte 1: Installazione di VirtualBox

### Cos'è VirtualBox?

VirtualBox è un software gratuito che permette di eseguire sistemi operativi "ospiti" (guest) all'interno del tuo sistema operativo principale "ospitante" (host). Nel nostro caso:
- **Host**: Windows 11 (il tuo computer)
- **Guest**: Alpine Linux (la macchina virtuale che creeremo)

### Step 1.1 — Download di VirtualBox

1. Apri il browser (Edge, Chrome, Firefox)
2. Vai all'indirizzo: **https://www.virtualbox.org/wiki/Downloads**
3. Clicca su **"Windows hosts"** sotto la sezione "VirtualBox platform packages"
4. Il file scaricato si chiamerà qualcosa come `VirtualBox-7.0.XX-Win.exe` (dove XX è la versione specifica)

[Screenshot: Pagina di download VirtualBox con freccia che indica il link "Windows hosts"]

5. Salva il file nella cartella **Download**

**Dimensione attesa**: circa 100-120 MB  
**Tempo di download**: 1-5 minuti (dipende dalla tua connessione)

### Step 1.2 — Installazione di VirtualBox

1. Apri la cartella **Download** (premi `Win + E` → clicca su "Download" nella barra laterale)
2. Fai doppio clic sul file `VirtualBox-7.0.XX-Win.exe`
3. Se appare "Controllo dell'account utente" che chiede l'autorizzazione:
   - **Se hai diritti amministratore**: clicca "Sì"
   - **Se NON hai diritti amministratore**: chiedi al tecnico di laboratorio di inserire la password amministratore

4. Nella finestra di installazione:
   - **Welcome screen**: clicca "Next"
   - **Custom Setup**: lascia tutto selezionato, clicca "Next"
   - **Warning Network Interfaces**: clicca "Yes" (è normale, disconnetterà brevemente la rete)
   - **Missing Dependencies**: se appare, clicca "Yes" per installare
   - **Ready to Install**: clicca "Install"

[Screenshot: Finestra di installazione VirtualBox in fase "Ready to Install"]

5. Attendi il completamento (circa 2-3 minuti)
6. Alla fine, **deseleziona** "Start Oracle VM VirtualBox after installation"
7. Clicca "Finish"

### Step 1.3 — Verifica installazione VirtualBox

1. Premi il tasto **Windows** sulla tastiera
2. Digita: `virtualbox`
3. Clicca su "Oracle VM VirtualBox"
4. Si apre la finestra principale di VirtualBox

[Screenshot: Finestra principale di VirtualBox appena installato, area macchine vuota]

✅ **Checkpoint 1**: Se vedi la finestra di VirtualBox con la barra laterale sinistra vuota e i pulsanti "Nuova", "Impostazioni", ecc. in alto, **l'installazione è riuscita**.

---

## Parte 2: Download di Alpine Linux

### Cos'è Alpine Linux?

Alpine Linux è il sistema operativo che installeremo nella macchina virtuale. È molto diverso da Windows:
- Non ha interfaccia grafica (lavorerai solo con la riga di comando)
- È molto compatto e veloce
- È lo stesso sistema usato nella maggior parte dei container Docker

### Step 2.1 — Download dell'immagine Alpine Linux Virt

1. Apri il browser
2. Vai all'indirizzo: **https://alpinelinux.org/downloads/**
3. Scorri la pagina fino alla sezione **"VIRTUAL"**
4. Clicca sul link **"x86_64"** nella riga "VIRTUAL"

[Screenshot: Pagina download Alpine con evidenziata la sezione VIRTUAL e il link x86_64]

5. Vieni reindirizzato a una pagina di mirror
6. Clicca su uno dei link disponibili (es. il primo della lista)
7. Si scarica un file chiamato: `alpine-virt-3.XX.X-x86_64.iso` (dove 3.XX.X è la versione corrente)

**Link diretto** (valido a marzo 2026):  
https://dl-cdn.alpinelinux.org/alpine/v3.19/releases/x86_64/alpine-virt-3.19.1-x86_64.iso

Se il link non funziona, usa la procedura manuale sopra.

**Dimensione attesa**: circa 50-60 MB  
**Tempo di download**: 30 secondi - 2 minuti

8. Salva il file nella cartella **Download**

✅ **Checkpoint 2**: Nella cartella Download hai ora due file:
- `VirtualBox-7.0.XX-Win.exe` (installatore VirtualBox)
- `alpine-virt-3.XX.X-x86_64.iso` (immagine Alpine Linux)

---

## Parte 3: Creazione della Macchina Virtuale

### Cos'è una macchina virtuale?

Una macchina virtuale (VM) è come un "computer dentro il computer". VirtualBox simula un computer completo (processore, RAM, disco, scheda di rete) all'interno di Windows. Alpine Linux penserà di essere installato su un computer fisico vero.

### Step 3.1 — Creazione della nuova VM

1. Apri VirtualBox (se l'hai chiuso: tasto Windows → digita "virtualbox" → Invio)
2. Clicca sul pulsante **"Nuova"** (icona blu con stella, in alto a sinistra)

[Screenshot: Pulsante "Nuova" evidenziato in VirtualBox]

3. Si apre la finestra "Crea macchina virtuale"

### Step 3.2 — Configurazione base della VM

Compila i campi come segue:

**Nome e sistema operativo:**
- **Nome**: `Alpine-Docker` (puoi scegliere un nome diverso, ma ricordatelo!)
- **Cartella**: lascia quella predefinita
- **Immagine ISO**: clicca sull'icona della cartella (▼) → "Altro..." → vai in Download → seleziona il file `alpine-virt-3.XX.X-x86_64.iso`
- **Tipo**: Linux
- **Versione**: Other Linux (64-bit)

[Screenshot: Finestra configurazione VM con campi compilati]

4. Clicca **"Avanti"**

### Step 3.3 — Configurazione memoria e processori

**Hardware:**
- **Memoria di base**: `1024 MB` (1GB)
  
  *Nota: Se il tuo PC ha 16GB o più di RAM, puoi impostare 2048 MB (2GB)*

- **Processori**: `2`

[Screenshot: Slider memoria e processori]

5. Clicca **"Avanti"**

### Step 3.4 — Configurazione disco virtuale

**Disco rigido:**
- Seleziona: **"Crea un nuovo disco rigido virtuale adesso"**
- **Dimensione disco**: `8.00 GB`

  *Nota: Il disco viene creato "dinamicamente allocato", quindi non occuperà subito 8GB sul tuo PC, ma crescerà man mano che lo usi*

[Screenshot: Configurazione disco con 8GB selezionati]

6. Clicca **"Avanti"**

### Step 3.5 — Riepilogo e creazione

7. Controlla il riepilogo delle impostazioni
8. Clicca **"Termina"**

✅ **Checkpoint 3**: Nella barra laterale di sinistra di VirtualBox vedi ora la tua VM "Alpine-Docker" con stato "Spenta".

### Step 3.6 — Configurazione rete (IMPORTANTE)

Prima di avviare la VM, dobbiamo configurare la rete per poterci collegare in SSH.

1. Seleziona la VM "Alpine-Docker" nella lista
2. Clicca sul pulsante **"Impostazioni"** (icona ingranaggio)
3. Nel menu laterale clicca su **"Rete"**

**Scheda 1 (Adattatore 1):**
- **Abilita scheda di rete**: ✅ (checkbox attivo)
- **Connessa a**: seleziona **"NAT"**

[Screenshot: Configurazione rete con "NAT" selezionato]

**Perché NAT in laboratorio scolastico?**
- Evita conflitti di indirizzi IP sulla LAN scolastica
- Riduce i problemi dovuti a regole DHCP/firewall della rete d'istituto
- Garantisce accesso Internet stabile alla VM

Per collegarti in SSH da Windows con NAT, aggiungi una regola di inoltro porta:

1. In VirtualBox vai su **Impostazioni → Rete → Avanzate → Inoltro porte**
2. Clicca sull'icona **+** e inserisci:
   - **Nome**: `ssh`
   - **Protocollo**: `TCP`
   - **IP host**: `127.0.0.1`
   - **Porta host**: `2222`
   - **IP guest**: (lascia vuoto)
   - **Porta guest**: `22`

4. Clicca **"OK"**

---

## Parte 4: Installazione di Alpine Linux nella VM

### Cosa faremo

Ora avvieremo la VM e installeremo Alpine Linux. All'inizio la VM partirà da un "Live CD" (l'immagine ISO), poi installeremo Alpine sul disco virtuale. È come installare Windows, ma molto più veloce.

### Step 4.1 — Primo avvio della VM

1. Seleziona la VM "Alpine-Docker"
2. Clicca sul pulsante verde **"Avvia"** (freccia verde in alto)

[Screenshot: Pulsante Avvia evidenziato]

3. Si apre una finestra nera (la console della VM)
4. Dopo pochi secondi appare il prompt di login di Alpine:

```
Welcome to Alpine Linux 3.XX
Kernel 6.X.XX on an x86_64

localhost login:
```

[Screenshot: Schermata di login Alpine]

### Step 4.2 — Login iniziale

1. Digita: `root` e premi **Invio**

   *Nota: "root" è l'utente amministratore di Linux. Nella live non ha password.*

2. Dovresti vedere il prompt:

```
localhost:~#
```

✅ **Checkpoint 4**: Sei loggato come root in Alpine Linux. Il cursore lampeggia dopo il simbolo `#`.

### Step 4.3 — Avvio del setup di Alpine

Prima di avviare `setup-alpine`, esegui questi passaggi obbligatori.

> **Consigliato: imposta subito la tastiera italiana**
>
> Appena hai fatto login come `root`, esegui:
>
> ```bash
> setup-keymap
> ```
>
> Scegli `it` come layout. Il default è US: se hai tastiera italiana, farlo subito evita errori nei caratteri speciali (es. @, è, ò) durante i prossimi comandi.
>
> Dopo aver impostato la keymap, prosegui con la configurazione di rete.

### Step 4.3a — `setup-interfaces` (obbligatorio)

```bash
setup-interfaces
```

Quando richiesto, usa:
- Interfaccia: `eth0`
- IP: `dhcp`
- Configurazione manuale: `n`

**Perché questo passaggio è obbligatorio?**
- Inizializza in modo esplicito la rete prima dell'installazione
- Evita che `setup-alpine` fallisca quando prova a contattare i mirror

### Step 4.3b — `rc-service networking restart` (obbligatorio)

```bash
rc-service networking restart
```

Verifica rapida della connettivita:

```bash
ping -c 3 8.8.8.8
```

**Perché questo passaggio è obbligatorio?**
- Applica subito la configurazione scritta da `setup-interfaces`
- Conferma che il livello IP funziona prima di avviare il setup guidato

### Step 4.3c — `setup-apkrepos` (obbligatorio)

Usa lo strumento ufficiale Alpine e allinea i repository alla policy di laboratorio (community + http) con modifica interattiva:

```bash
setup-apkrepos
```

Nel menu di `setup-apkrepos`, scegli `e` (edit repositories): si apre `vi` su `/etc/apk/repositories`.

> **Nota importante:**
> Se il file `/etc/apk/repositories` è vuoto, prima scegli `r` (random) per aggiungere un mirror casuale. Poi torna su `e` per modificare il file come indicato sotto.

Flusso rapido:

In `vi`, applica queste modifiche:
- Decommenta la riga `community` (rimuovi `#` iniziale)
- Sostituisci `https://` con `http://` nelle righe dei mirror

Comandi base `vi` (promemoria rapido):
- `ESC`: torna alla modalita comando
- `i`: entra in modalita inserimento/modifica
- `:w` + `Invio`: salva
- `:q` + `Invio`: esci (solo se non ci sono modifiche)
- `:wq` + `Invio`: salva ed esci
- `:q!` + `Invio`: esci senza salvare

Dopo il salvataggio, aggiorna l'indice pacchetti:

```bash
apk update
```

**Motivazioni teoriche**:
- `main` include i pacchetti base del sistema, `community` contiene molti pacchetti applicativi del corso (incluso Docker nelle release correnti)
- In ambienti scolastici filtrati, usare mirror `http` nel bootstrap riduce errori legati a certificati/intercettazione TLS non ancora configurati nella VM

Verifica se Docker e disponibile nei repository attivi:

```bash
apk policy docker
```

Se compare una versione candidata, Docker e installabile con la configurazione attuale.

Alpine include un programma interattivo che guida nell'installazione. Si chiama `setup-alpine`.

1. Digita: `setup-alpine` e premi **Invio**

```
localhost:~# setup-alpine
```

2. Inizia una serie di domande. Rispondi come indicato sotto.

### Step 4.4 — Configurazione tastiera

```
Select keyboard layout: [none]
```

1. Digita: `it` (per tastiera italiana) e premi **Invio**

```
Select variant (or 'abort'):
```

2. Premi semplicemente **Invio** (accetta la variante predefinita)

**Verifica**: Prova a digitare alcuni caratteri speciali (@, è, ò) per confermare che la tastiera funziona correttamente.

### Step 4.5 — Configurazione hostname

```
Enter system hostname (fully qualified form, e.g. 'foo.example.org') [localhost]
```

1. Digita: `alpine-docker` e premi **Invio**

   *L'hostname è il nome del computer nella rete*

### Step 4.6 — Configurazione interfaccia di rete

```
Available interfaces are: eth0.
Which one do you want to initialize? (or '?' or 'done') [eth0]
```

1. Premi **Invio** (accetta `eth0`, che è la scheda di rete virtuale)

```
Ip address for eth0? (or 'dhcp', 'none', '?') [dhcp]
```

2. Premi **Invio** (accetta DHCP)

   *DHCP significa che la VM riceverà automaticamente un indirizzo IP dal router*

```
Do you want to do any manual network configuration? (y/n) [n]
```

3. Premi **Invio** (nessuna configurazione manuale)


### Step 4.7 — Configurazione password di root

```
Changing password for root
New password:
```

1. **Importante**: Scegli una password e **ricordala**! Servirà per connetterti in SSH.
   
   *Suggerimento: per il laboratorio puoi usare una password semplice come "alpine" o "password123"*

2. Digita la password (non vedrai nulla sullo schermo, è normale) e premi **Invio**

```
Retype password:
```

3. Digita di nuovo la stessa password e premi **Invio**

### Step 4.8 — Configurazione fuso orario

```
Which timezone are you in? ('?' for list) [UTC]
```

1. Digita: `Europe/Rome` e premi **Invio**

   *Così l'orologio della VM sarà sincronizzato con quello italiano*

### Step 4.9 — Configurazione proxy

```
HTTP/FTP proxy URL? (e.g. 'http://proxy:8080', or 'none') [none]
```

1. Premi **Invio** (nessun proxy)

   *A scuola/casa normalmente non serve un proxy. Se sei in una rete aziendale con proxy, chiedi all'amministratore di rete*

### Step 4.10 — Configurazione mirror APK

```
Enter mirror number (1-XX) or URL to add (or r/f/e/done) [1]
```

1. Premi **Invio** (accetta il mirror predefinito)

   *I mirror sono server da cui Alpine scarica i pacchetti software*

```
Which SSH server? ('openssh', 'dropbear', or 'none') [openssh]
```

### Step 4.11 — Configurazione SSH

2. Premi **Invio** (installa OpenSSH)

   *SSH ti permetterà di connetterti alla VM dal terminale Windows*

```
Allow root SSH login? ('?' for help) [prohibit-password]
```

3. Digita: `yes` e premi **Invio**

   **Attenzione**: In produzione non si dovrebbe mai permettere il login SSH di root. Lo facciamo qui solo per semplicità didattica.

### Step 4.12 — Configurazione disco

```
Which disk(s) would you like to use? (or '?' for help or 'none') [none]
```

1. Digita: `sda` e premi **Invio**

   *`sda` è il disco virtuale da 8GB che abbiamo creato*

```
How would you like to use it? ('sys', 'data', 'lvm', or '?' for help) [?]
```

2. Digita: `sys` e premi **Invio**

   *`sys` installa Alpine completamente sul disco (come un'installazione normale)*

```
WARNING: Erase the above disk(s) and continue? (y/n) [n]
```

3. Digita: `y` e premi **Invio**

   *Confermi di formattare il disco virtuale e installarci Alpine*

### Step 4.13 — Attesa installazione

Ora il setup copia tutti i file sul disco. Vedrai un output simile a:

```
Installing system on /dev/sda:
-----> Installing packages for alpine-base
(1/XX) Installing ...
...
Installation is complete. Please reboot.
```

**Tempo richiesto**: 1-3 minuti

✅ **Checkpoint 5**: L'installazione è completa quando vedi "Installation is complete. Please reboot."

### Step 4.14 — Riavvio e rimozione ISO

1. Digita: `reboot` e premi **Invio**

```
localhost:~# reboot
```

2. La VM si riavvia, ma si bloccherà con un messaggio tipo:

```
Please remove the installation medium, then press ENTER:
```

3. **IMPORTANTE**: Prima di premere Invio in Alpine, rimuovi l'ISO dalla VM:
   - Nella finestra di VirtualBox, in alto, clicca su **"Dispositivi"** → **"Unità ottiche"** → **"Rimuovi disco dal lettore virtuale"**

[Screenshot: Menu Dispositivi → Unità ottiche → Rimuovi disco]

4. Torna alla finestra della VM e premi **Invio**

5. La VM si riavvia di nuovo, questa volta dal disco. Dopo pochi secondi vedi:

```
alpine-docker login:
```

### Step 4.15 — Test login locale

1. Digita: `root` e premi **Invio**
2. Digita la password che hai scelto prima
3. Dovresti vedere:

```
alpine-docker:~#
```

✅ **Checkpoint 6**: Sei loggato come root su Alpine installato. L'hostname è `alpine-docker` invece di `localhost`.

---

## Parte 5: Configurazione SSH e Connessione da Windows

### Perché SSH?

Lavorare direttamente nella finestra della console di VirtualBox è meno pratico:
- Nella configurazione base non c'è copia/incolla host↔guest (serve installare Guest Additions)
- Lo schermo è piccolo
- Non puoi aprire più finestre/tab

Nel corso useremo SSH anche per un motivo pratico: permette di copiare/incollare rapidamente i comandi delle esercitazioni dal terminale Windows.

Con SSH ti colleghi alla VM da un terminale Windows (es. Windows Terminal, PowerShell, o il nuovo prompt di Windows) e lavori comodamente.

### Step 5.1 — Prima connessione SSH da Windows (NAT)

1. Sul **tuo computer Windows** (non nella VM), apri il PowerShell:
   - Premi `Win + X` → clicca "Windows PowerShell" o "Terminale Windows"
   - Oppure: premi `Win + R` → digita `powershell` → Invio

2. Nel terminale Windows, digita:

```powershell
ssh -p 2222 root@127.0.0.1
```

3. La prima volta vedrai un messaggio simile:

```
The authenticity of host '[127.0.0.1]:2222 ([127.0.0.1]:2222)' can't be established.
ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

4. Digita: `yes` e premi **Invio**

5. Inserisci la password di root (quella che hai scelto durante l'installazione)

6. Dovresti vedere:

```
Welcome to Alpine!
...
alpine-docker:~#
```

🎉 **Congratulazioni!** Sei connesso via SSH alla tua VM Alpine Linux da Windows!

[Screenshot: Connessione SSH riuscita con prompt alpine-docker]

✅ **Checkpoint 8**: Sei connesso in SSH, il terminale mostra `alpine-docker:~#`.

### Step 5.4 — Test comandi base

Ora che sei connesso, prova alcuni comandi:

1. Mostra il nome del sistema:

```bash
hostname
```

**Output atteso**:
```
alpine-docker
```

2. Mostra l'utente corrente:

```bash
whoami
```

**Output atteso**:
```
root
```

3. Mostra informazioni sul sistema:

```bash
uname -a
```

**Output atteso** (simile a):
```
Linux alpine-docker 6.1.XX-X-virt #1-Alpine SMP ... x86_64 Linux
```

4. Mostra lo spazio su disco:

```bash
df -h
```

**Output atteso** (simile a):
```
Filesystem                Size      Used Available Use% Mounted on
/dev/sda3                 7.0G    200M      6.8G   3% /
...
```

---

## Parte 6: Installazione Strumenti di Sviluppo

### Cosa installeremo

Per seguire il corso hai bisogno di:
- **nano** o **vim**: editor di testo (nano è più semplice per chi inizia)
- **git**: per scaricare esempi e gestire il codice
- **docker** e **docker-compose**: il cuore del corso
- **bash**: una shell più avanzata rispetto a `ash` (quella predefinita di Alpine)
- **sudo**: per eseguire comandi come amministratore (utile per il futuro)

### Step 6.0 — Verifica repository Alpine su HTTPS (obbligatorio in laboratorio)

In alcuni laboratori scolastici il traffico `http` è bloccato per sicurezza. Prima di aggiornare i pacchetti, verifica che Alpine usi solo repository `https`.

1. Controlla il contenuto attuale del file repository:

```bash
cat /etc/apk/repositories
```

2. Se vedi righe che iniziano con `http://`, fai prima una copia di sicurezza:

```bash
cp /etc/apk/repositories /etc/apk/repositories.bak
```

3. Sostituisci automaticamente `http://` con `https://`:

```bash
sed -i 's|^http://|https://|g' /etc/apk/repositories
```

4. Verifica che non ci siano più URL in `http`:

```bash
grep -nE '^http://|^https://' /etc/apk/repositories
```

**Output atteso** (esempio):
```
1:https://dl-cdn.alpinelinux.org/alpine/v3.XX/main
2:https://dl-cdn.alpinelinux.org/alpine/v3.XX/community
```

✅ **Checkpoint 6.0**: Se tutte le righe repository iniziano con `https://`, puoi proseguire.

---

### Step 6.1 — Aggiornamento lista pacchetti

Prima di installare qualsiasi software, aggiorna la lista dei pacchetti disponibili:

```bash
apk update
```

**Output atteso**:
```
fetch http://dl-cdn.alpinelinux.org/alpine/v3.XX/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.XX/community/x86_64/APKINDEX.tar.gz
v3.XX.X-XX-xxxxx [http://dl-cdn.alpinelinux.org/alpine/v3.XX/main]
v3.XX.X-XX-xxxxx [http://dl-cdn.alpinelinux.org/alpine/v3.XX/community]
OK: XXXXX distinct packages available
```

✅ Se vedi "OK: XXXXX distinct packages available", l'aggiornamento è riuscito.

### Step 6.2 — Installazione editor di testo (nano)

```bash
apk add nano
```

**Output atteso**:
```
(1/1) Installing nano (X.XX-rX)
Executing busybox-X.XX.X-rX.trigger
OK: XXX MiB in XX packages
```

**Test**:
```bash
nano --version
```

**Output atteso**:
```
 GNU nano, version 7.X
```

### Step 6.3 — Installazione Git

```bash
apk add git
```

**Output atteso**:
```
(1/X) Installing ...
...
OK: XXX MiB in XX packages
```

**Test**:
```bash
git --version
```

**Output atteso**:
```
git version 2.XX.X
```

### Step 6.4 — Installazione Bash

Alpine usa di default `ash` (una shell minimale). Bash è più comodo per lavorare.

```bash
apk add bash bash-completion
```

**Output atteso**:
```
(1/X) Installing ...
...
OK: XXX MiB in XX packages
```

**Test**:
```bash
bash --version
```

**Output atteso**:
```
GNU bash, version 5.X.XX(X)-release (x86_64-alpine-linux-musl)
```

**Cambia shell predefinita**:
```bash
chsh -s /bin/bash
```

**Nota**: La shell cambierà al prossimo login. Per ora resta `ash`.

### Step 6.5 — Installazione Docker e Docker Compose

Questo è il passo più importante! Docker è il cuore del corso.

**Installa Docker**:
```bash
apk add docker docker-cli-compose
```

**Output atteso**:
```
(1/XX) Installing ...
...
OK: XXX MiB in XX packages
```

**Avvia il servizio Docker**:
```bash
rc-update add docker default
rc-service docker start
```

**Output atteso**:
```
 * service docker added to runlevel default
 * Caching service dependencies ...
 * Starting docker ...
```

**Test Docker**:
```bash
docker --version
```

**Output atteso**:
```
Docker version 24.X.X, build XXXXXXX
```

**Test Docker Compose**:
```bash
docker compose version
```

**Output atteso**:
```
Docker Compose version v2.XX.X
```

### Step 6.6 — Test completo Docker

Esegui il classico "Hello World" di Docker:

```bash
docker run hello-world
```

**Cosa succede**:
1. Docker cerca l'immagine `hello-world` localmente (non la trova)
2. La scarica da Docker Hub
3. Crea un container e lo esegue
4. Il container stampa un messaggio e si ferma

**Output atteso**:
```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
...
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

✅ **Checkpoint 9**: Se vedi il messaggio "Hello from Docker!", Docker funziona perfettamente!

### Step 6.7 — Installazione strumenti di rete (utili per debug)

```bash
apk add curl wget bind-tools htop
```

**Spiegazione**:
- **curl** e **wget**: scaricare file da Internet
- **bind-tools**: include `dig` e `nslookup` per test DNS
- **htop**: monitor dei processi (più comodo di `top`)

**Test curl**:
```bash
curl -I https://www.google.com
```

**Output atteso**:
```
HTTP/2 200
content-type: text/html; charset=ISO-8859-1
...
```

### Step 6.8 — Installazione sudo (opzionale ma consigliato)

`sudo` permette di eseguire comandi singoli come root senza dover fare login come root.
In questo modulo stai già operando come `root`, quindi i comandi restano senza `sudo`.

```bash
apk add sudo
```

**Configurazione** (per il futuro, quando creerai un utente normale):
```bash
echo '%wheel ALL=(ALL) ALL' > /etc/sudoers.d/wheel
```

---

## Parte 7: Configurazione Finale e Ottimizzazioni

### Step 7.1 — Impostare un editor predefinito

```bash
export EDITOR=nano
echo 'export EDITOR=nano' >> /root/.bashrc
```

### Step 7.2 — Abilitare il completamento automatico Bash

```bash
echo 'source /etc/profile.d/bash_completion.sh' >> /root/.bashrc
```

### Step 7.3 — Configurare alias utili

Crea alcuni alias per velocizzare i comandi più comuni:

```bash
cat >> /root/.bashrc << 'EOF'

# Alias utili
alias ll='ls -lah'
alias la='ls -A'
alias l='ls -CF'
alias cls='clear'
alias update='apk update && apk upgrade'

# Alias Docker
alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias dex='docker exec -it'
alias dlog='docker logs'

EOF
```

**Ricarica la configurazione**:
```bash
source /root/.bashrc
```

**Test**:
```bash
ll
```

Dovresti vedere l'elenco dettagliato dei file nella directory corrente.

### Step 7.4 — Configurazione Git (identità)

Configura il tuo nome e email per Git:

```bash
git config --global user.name "Il Tuo Nome"
git config --global user.email "tua.email@esempio.com"
```

**Verifica**:
```bash
git config --list
```

**Output atteso** (include):
```
user.name=Il Tuo Nome
user.email=tua.email@esempio.com
```

### Step 7.5 — Cartella condivisa Windows ↔ Alpine (VirtualBox)

Questa sezione configura una cartella condivisa tra Windows (host) e Alpine (guest).

> Usa lo **stesso nome** della cartella condivisa in tutti i comandi. In questo corso useremo `html`.

1. In VirtualBox, con VM spenta:
   - Apri **Impostazioni** della VM → **Cartelle condivise**
   - Aggiungi la cartella di Windows da condividere
   - Imposta **Nome cartella**: `html`

2. Avvia la VM Alpine e installa il supporto guest additions:

```bash
apk add virtualbox-guest-additions
mkdir -p /mnt/html
```

3. Montaggio manuale (immediato):

```bash
modprobe vboxsf
mount -t vboxsf html /mnt/html
```

4. Verifica mount manuale:

```bash
mount | grep /mnt/html
ls -la /mnt/html
```

**Output atteso**:
- una riga con `type vboxsf` e `/mnt/html`
- il contenuto della cartella condivisa (oppure directory vuota se non ci sono file)

5. Montaggio automatico all'avvio:

```bash
echo "html /mnt/html vboxsf defaults 0 0" | tee -a /etc/fstab > /dev/null
mount -a
```

6. Verifica configurazione automatica:

```bash
grep -n "^html /mnt/html vboxsf" /etc/fstab
mount | grep /mnt/html
```

**Output atteso**:
- una riga in `/etc/fstab` con `html /mnt/html vboxsf defaults 0 0`
- mount attivo su `/mnt/html`

### Step 7.6 — Snapshot della VM (raccomandato)

Ora che hai tutto configurato, è il momento perfetto per creare uno **snapshot** (istantanea) della VM. Se qualcosa va storto in futuro, potrai ripristinare la VM a questo punto.

1. **Spegni** la VM:

```bash
poweroff
```

2. In VirtualBox:
   - Seleziona la VM "Alpine-Docker"
   - Clicca su "Istantanee" nel menu in alto
   - Clicca sull'icona "Crea" (fotocamera)
   - Nome snapshot: "Setup Base Completo"
   - Descrizione: "Alpine + Docker + strumenti di sviluppo installati"
   - Clicca "OK"

[Screenshot: Creazione snapshot in VirtualBox]

3. Riavvia la VM e riconnettiti in SSH

---

## Parte 8: Test Finale dell'Ambiente

### Checklist completa

Verifichiamo che tutto funzioni. Riconnettiti in SSH se hai spento la VM, poi esegui:

**1. Test sistema**:
```bash
hostname && uname -r && uptime
```

**2. Test rete**:
```bash
ping -c 3 google.com
```

**3. Test editor**:
```bash
nano --version
```

**4. Test Git**:
```bash
git --version && git config user.name
```

**5. Test Docker**:
```bash
docker --version && docker compose version
```

**6. Test Docker funzionante**:
```bash
docker run --rm alpine:latest echo "Docker funziona!"
```

**Output atteso**: "Docker funziona!"

**7. Test strumenti di rete**:
```bash
curl -s http://httpbin.org/ip | head -5
```

**8. Spazio disco disponibile**:
```bash
df -h / | tail -1
```

Dovresti avere ancora almeno 5-6 GB liberi.

### ✅ Riepilogo finale

Se tutti i test sopra funzionano, hai un ambiente completo e pronto per il corso! Il tuo setup include:

| Componente | Versione | Stato |
|------------|----------|-------|
| VirtualBox | 7.0.X | ✅ Installato |
| Alpine Linux | 3.XX | ✅ Installato e configurato |
| Rete | NAT + Port Forwarding (2222→22) | ✅ Funzionante per SSH da Windows |
| SSH | OpenSSH | ✅ Accessibile da Windows |
| Editor | nano | ✅ Installato |
| Git | 2.XX | ✅ Configurato |
| Docker | 24.X | ✅ Operativo |
| Docker Compose | v2.XX | ✅ Operativo |
| Bash | 5.X | ✅ Disponibile |

---

## Troubleshooting — Problemi Comuni

### Problema 1: VirtualBox non si installa (errore privilegi)

**Sintomo**: Durante l'installazione appare un errore di permessi.

**Soluzione**:
- Devi avere privilegi di amministratore su Windows
- Chiedi al tecnico di laboratorio o all'insegnante di inserire la password amministratore
- In alternativa, chiedi che VirtualBox sia pre-installato sui computer del laboratorio

### Problema 2: Alpine non trova la rete durante setup

**Sintomo**: Durante `setup-alpine`, il ping a 8.8.8.8 fallisce.

**Soluzione 1** (prima prova):
```bash
# Interrompi setup con Ctrl+C
# Configura manualmente la rete
setup-interfaces
# Riavvia la rete
rc-service networking restart
# Riprova setup
setup-alpine
```

**Soluzione 2** (se Soluzione 1 non funziona):
- Spegni la VM
- In VirtualBox, Impostazioni → Rete
- Verifica che "Connessa a" sia impostato su "NAT"
- Riavvia la VM e riprova

### Problema 2-bis: `apk update` fallisce perché i repository usano HTTP

**Sintomo**: `apk update` mostra errori di fetch oppure timeout quando prova URL `http://...`

**Causa tipica in laboratorio**: il firewall consente solo traffico `https`.

**Soluzione passo passo**:

```bash
# 1) Controlla repository attuali
cat /etc/apk/repositories

# 2) Backup file
cp /etc/apk/repositories /etc/apk/repositories.bak

# 3) Converti http -> https
sed -i 's|^http://|https://|g' /etc/apk/repositories

# 4) Verifica e aggiorna indice
grep -nE '^http://|^https://' /etc/apk/repositories
apk update
```

**Output atteso**: righe `fetch https://...` e messaggio finale `OK: ... distinct packages available`.

Se anche con `https` non funziona, segnala al tecnico di laboratorio un possibile blocco verso `dl-cdn.alpinelinux.org`.

### Problema 3: SSH da Windows alla VM fallisce (NAT)

**Sintomo**: `ssh -p 2222 root@127.0.0.1` non si connette.

**Diagnosi**:
1. Verifica che la VM sia accesa
2. Verifica che il port forwarding di VirtualBox sia `2222` (host) → `22` (guest)
3. Nella VM, verifica che SSH sia attivo

**Soluzioni**:

**A. Se la VM non ha indirizzo IP**:
```bash
# Nella VM
dhclient eth0
# Oppure
udhcpc -i eth0
```

**B. Verifica il servizio SSH nella VM**:
```bash
service sshd status
netstat -tlnp | grep :22
```

**C. Ripristina configurazione di rete**:
```bash
# Nella VM
rc-service networking restart
```

### Problema 4: SSH connection refused

**Sintomo**: `ssh -p 2222 root@127.0.0.1` risponde "Connection refused".

**Soluzione**:

1. Verifica che SSH sia in esecuzione nella VM:
```bash
# Nella console VM (non SSH)
rc-service sshd status
```

Se non è attivo:
```bash
rc-service sshd start
rc-update add sshd default
```

2. Verifica che SSH ascolti sulla porta 22:
```bash
netstat -tlnp | grep :22
```

### Problema 5: Docker non si avvia

**Sintomo**: `rc-service docker start` fallisce.

**Soluzione**:

1. Guarda i log di errore:
```bash
dmesg | tail -20
```

2. Verifica i moduli kernel necessari:
```bash
modprobe overlay
modprobe br_netfilter
```

3. Riavvia il servizio:
```bash
rc-service docker restart
```

### Problema 6: "docker run hello-world" dà errore di permessi

**Sintomo**: Errore "permission denied while trying to connect to the Docker daemon socket"

**Soluzione**:
Assicurati che il servizio Docker sia in esecuzione:
```bash
rc-service docker status
rc-service docker start  # se non è attivo
```

Se il problema persiste:
```bash
# Aggiungi root al gruppo docker (anche se sei già root, a volte serve)
addgroup root docker
# Riloggati
exit
# Riconnettiti in SSH
ssh -p 2222 root@127.0.0.1
```

### Problema 7: Tastiera non funziona correttamente

**Sintomo**: I caratteri digitati sono diversi da quelli attesi.

**Soluzione**:
```bash
setup-keymap
# Seleziona: it
# Seleziona variante: (default)
```

### Problema 8: VM troppo lenta

**Sintomo**: La VM impiega molto tempo a rispondere ai comandi.

**Soluzioni**:

1. **Aumenta RAM della VM**:
   - Spegni la VM
   - VirtualBox → Impostazioni → Sistema → Memoria di base → 2048 MB
   - Riavvia

2. **Aumenta CPU**:
   - Spegni la VM
   - VirtualBox → Sistema → Processore → 2 CPU
   - Riavvia

3. **Abilita accelerazione hardware** (se disponibile):
   - VirtualBox → Sistema → Accelerazione → Abilita VT-x/AMD-V

### Problema 9: Non riesco a fare copia-incolla tra Windows e VM

**Soluzione**:
Non è necessario. Una volta collegato in SSH dal terminale Windows, puoi fare copia-incolla normalmente nella finestra del terminale (non nella console di VirtualBox).

Se vuoi comunque abilitarlo:
- Spegni la VM
- VirtualBox → Impostazioni → Generali → Avanzate
- "Appunti condivisi": Bidirezionale
- "Drag'n'Drop": Bidirezionale
- Richiede l'installazione di Guest Additions (non coperto in questa guida)

### Problema 10: Ho dimenticato la password di root

**Soluzione** (recovery mode):

1. Riavvia la VM
2. Quando appare il menu GRUB (boot loader), premi `E`
3. Trova la riga che inizia con `linux`
4. Alla fine della riga aggiungi: `init=/bin/sh`
5. Premi `Ctrl+X` per avviare
6. Quando appare il prompt, digita:
```bash
mount -o remount,rw /
passwd root
# Inserisci nuova password
sync
reboot -f
```

---

## 📚 Materiale di Riferimento

### Comandi principali usati in questo lab

| Comando | Descrizione | Esempio |
|---------|-------------|---------|
| `apk update` | Aggiorna la lista dei pacchetti | `apk update` |
| `apk add <pacchetto>` | Installa un pacchetto | `apk add nano` |
| `ip addr show` | Mostra indirizzi IP | `ip addr show eth0` |
| `ping <host>` | Testa connettività | `ping google.com` |
| `ssh -p <porta> <user>@<host>` | Connessione SSH con NAT | `ssh -p 2222 root@127.0.0.1` |
| `docker --version` | Mostra versione Docker | `docker --version` |
| `docker run <immagine>` | Esegue un container | `docker run hello-world` |
| `rc-service <servizio> start` | Avvia un servizio | `rc-service docker start` |
| `rc-update add <srv> default` | Abilita avvio automatico | `rc-update add docker default` |
| `poweroff` | Spegne il sistema | `poweroff` |
| `reboot` | Riavvia il sistema | `reboot` |

### Link utili

- **VirtualBox**: https://www.virtualbox.org/
- **Alpine Linux**: https://alpinelinux.org/
- **Alpine Linux Wiki**: https://wiki.alpinelinux.org/
- **Docker Documentation**: https://docs.docker.com/
- **Alpine Package Search**: https://pkgs.alpinelinux.org/

### Risorse di approfondimento

- [Alpine Linux Handbook](https://wiki.alpinelinux.org/wiki/Alpine_Linux:FAQ)
- [Docker Getting Started](https://docs.docker.com/get-started/)
- [SSH Tutorial](https://www.ssh.com/academy/ssh)

---

## ➡️ Prossimi passi

Ora che hai configurato l'ambiente, sei pronto per iniziare il corso vero e proprio!

**Prossimo modulo**:  
→ [Sessione Introduttiva 1 - Teoria: OS, Filesystem e Shell](../fase-1-introduzione/intro-01-teoria-os-filesystem-shell.md)

### Cosa imparerai nei prossimi moduli

1. **Fase 1 - Sistemi Operativi e Shell**: Capirai come funziona Linux, cos'è un filesystem, come usare la shell bash
2. **Fase 2 - Reti e Servizi**: Apprenderai i concetti di rete, client-server, virtualizzazione
3. **Fase 3 - Docker**: Inizierai a creare e gestire container Docker

### ✅ Mantenimento dell'ambiente

**Best practices per le prossime sessioni**:

1. **Prima di ogni sessione**:
   - Avvia VirtualBox
   - Avvia la VM "Alpine-Docker"
   - Connettiti in SSH da Windows

2. **Alla fine di ogni sessione**:
   - Disconnetti SSH: `exit`
   - Spegni la VM: nella console digita `poweroff` oppure in VirtualBox: Macchina → Chiudi → Spegni

3. **Aggiornamenti periodici** (1 volta a settimana):
```bash
apk update
apk upgrade
```

4. **Backup/Snapshot** (dopo modifiche importanti):
   - Spegni la VM
   - VirtualBox → Istantanee → Crea

---

## 🎓 Hai completato il setup!

**Congratulazioni!** 🎉

Hai configurato con successo tutto l'ambiente di lavoro necessario per il corso Docker. Questo è stato il passo più tecnico e complesso: da qui in avanti il focus sarà sull'apprendimento di Docker e dei container.

**Cosa hai imparato in questo laboratorio**:
- ✅ Cos'è una macchina virtuale e come crearla
- ✅ Come installare e configurare un sistema Linux (Alpine)
- ✅ Come funziona la rete in un ambiente virtualizzato
- ✅ Come connettersi in SSH da Windows a una VM Linux
- ✅ Come installare software in Alpine Linux con `apk`
- ✅ Come installare e testare Docker
- ✅ Come creare snapshot per backup

**Stima del tempo totale**: 90-120 minuti

**Prossima sessione**: Teoria su sistemi operativi e shell Linux

---

↩ [Torna all'indice principale](../README.md)
