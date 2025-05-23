# Sessione Introduttiva 2: Reti, Applicazioni e Servizi

## Panoramica della Sessione
Questa seconda sessione introduttiva di 5 ore è progettata per fornire agli studenti le conoscenze di base sulle reti informatiche, le applicazioni, i servizi e un'introduzione alla virtualizzazione. Questi concetti sono fondamentali per comprendere il funzionamento di Docker e della containerizzazione.

## Parte 1: Concetti Fondamentali di Reti (1 ora)

### Cos'è una Rete Informatica?
Una rete informatica è un insieme di dispositivi interconnessi che possono comunicare tra loro e condividere risorse. Possiamo pensarla come un sistema postale digitale, dove i messaggi vengono inviati da un punto all'altro seguendo regole precise.

#### Tipi di Reti:
- **LAN** (Local Area Network): Rete locale limitata a un'area geografica ristretta
  - Esempio: rete di un'aula, di un ufficio o di una casa
  - Caratteristiche: alta velocità, bassa latenza, gestione centralizzata
- **WAN** (Wide Area Network): Rete geografica che copre grandi distanze
  - Esempio: rete aziendale con sedi in diverse città
  - Caratteristiche: velocità variabile, latenza maggiore, connessioni dedicate
- **Internet**: La "rete delle reti", che collega miliardi di dispositivi in tutto il mondo
  - Caratteristiche: protocolli standardizzati, struttura decentralizzata

### Indirizzi IP e DNS
Per comunicare in una rete, ogni dispositivo deve avere un indirizzo univoco:

#### Indirizzi IP:
- **Definizione**: Numeri che identificano univocamente un dispositivo in una rete
- **IPv4**: Formato a 32 bit (es. 192.168.1.1)
  - Limitato a circa 4,3 miliardi di indirizzi
  - Esaurimento degli indirizzi disponibili
- **IPv6**: Formato a 128 bit (es. 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
  - Praticamente illimitato (340 undecilioni di indirizzi)
  - Adozione graduale per sostituire IPv4
- **Indirizzi privati vs pubblici**:
  - Privati: usati nelle reti locali (es. 192.168.x.x, 10.x.x.x)
  - Pubblici: univoci su Internet

#### DNS (Domain Name System):
- **Funzione**: Traduce nomi di dominio in indirizzi IP
- **Metafora**: Elenco telefonico che associa nomi a numeri
- **Esempio**: www.example.com → 93.184.216.34
- **Gerarchia DNS**:
  - Root servers
  - Top-level domain servers (.com, .org, .it)
  - Authoritative name servers

### Porte e Protocolli
Oltre agli indirizzi IP, la comunicazione di rete si basa su porte e protocolli:

#### Porte:
- **Definizione**: Canali numerati (0-65535) che identificano specifici servizi su un dispositivo
- **Metafora**: Sportelli specializzati di un ufficio
- **Porte comuni**:
  - 80: HTTP (web)
  - 443: HTTPS (web sicuro)
  - 22: SSH (accesso remoto sicuro)
  - 25: SMTP (email in uscita)
  - 53: DNS (risoluzione nomi)

#### Protocolli:
- **Definizione**: Insieme di regole che governano la comunicazione
- **Protocolli comuni**:
  - **HTTP/HTTPS**: Trasferimento di pagine web
  - **TCP**: Trasmissione affidabile con controllo degli errori
  - **UDP**: Trasmissione veloce senza garanzie
  - **IP**: Instradamento dei pacchetti
  - **SSH**: Connessione sicura a sistemi remoti

### Esercizio Pratico: Analisi della Propria Rete
**Obiettivo**: Comprendere la propria connessione di rete

**Attività**:
1. Identificare il proprio indirizzo IP locale:
   - Windows: `ipconfig`
   - Linux/macOS: `ifconfig` o `ip addr`
2. Identificare il proprio indirizzo IP pubblico:
   - Visitare un sito come whatismyip.com
3. Testare la connettività con il comando `ping`:
   - `ping google.com`
   - `ping 8.8.8.8` (DNS di Google)
4. Risolvere nomi di dominio con `nslookup` o `dig`:
   - `nslookup example.com`
   - `dig example.com`
5. Compilare una "carta d'identità" della propria connessione

## Parte 2: Modello Client-Server (1 ora)

### Architettura Client-Server
Il modello client-server è uno dei paradigmi fondamentali dell'informatica distribuita:

#### Definizioni:
- **Client**: Dispositivo o software che richiede servizi o risorse
- **Server**: Dispositivo o software che fornisce servizi o risorse
- **Metafora**: Il ristorante, dove i clienti (client) ordinano e i camerieri/cuochi (server) servono

#### Caratteristiche:
- **Separazione dei ruoli**: Chiara distinzione tra chi richiede e chi fornisce
- **Centralizzazione delle risorse**: Dati e servizi gestiti centralmente
- **Scalabilità**: Possibilità di aggiungere più client o potenziare i server
- **Specializzazione**: Server ottimizzati per compiti specifici

### Richieste e Risposte
La comunicazione client-server segue un modello di richiesta-risposta:

#### Ciclo di Vita di una Richiesta:
1. **Iniziazione**: Il client formula una richiesta
2. **Trasmissione**: La richiesta viene inviata al server
3. **Elaborazione**: Il server processa la richiesta
4. **Risposta**: Il server invia il risultato al client
5. **Ricezione**: Il client riceve e interpreta la risposta

#### Esempio: Ciclo di Vita di una Richiesta Web:
1. Browser (client) richiede una pagina web
2. La richiesta HTTP viene inviata al server web
3. Il server web elabora la richiesta (cerca file, esegue script)
4. Il server invia la pagina HTML come risposta
5. Il browser riceve l'HTML e lo visualizza

### Esempi di Sistemi Client-Server
Il modello client-server è alla base di molti servizi quotidiani:

#### Web:
- **Client**: Browser (Chrome, Firefox, Safari)
- **Server**: Web server (Apache, Nginx, IIS)
- **Protocollo**: HTTP/HTTPS

#### Email:
- **Client**: Client di posta (Outlook, Thunderbird, Gmail)
- **Server**: Mail server (SMTP per invio, IMAP/POP3 per ricezione)
- **Protocollo**: SMTP, IMAP, POP3

#### Database:
- **Client**: Applicazione o interfaccia di gestione
- **Server**: DBMS (MySQL, PostgreSQL, Oracle)
- **Protocollo**: Specifico del database

#### File Sharing:
- **Client**: File manager, client FTP
- **Server**: File server, server FTP
- **Protocollo**: SMB, FTP, NFS

### Esercizio Pratico: Analisi di Richieste Web
**Obiettivo**: Visualizzare l'interazione client-server in azione

**Attività**:
1. Aprire gli strumenti di sviluppo del browser (F12 o Ctrl+Shift+I)
2. Selezionare la scheda "Network" (Rete)
3. Visitare un sito web (es. example.com)
4. Analizzare le richieste generate:
   - Metodo (GET, POST)
   - URL richiesto
   - Codice di stato (200, 404, 500)
   - Tipo di contenuto (HTML, CSS, JavaScript, immagini)
5. Creare un diagramma del flusso di richieste e risposte

## Parte 3: Comandi di Rete e Diagnostica (1 ora)

### Strumenti di Base per la Diagnostica
Esistono diversi comandi per diagnosticare problemi di rete:

#### `ping`:
- **Funzione**: Verifica la connettività con un host
- **Utilizzo**: `ping hostname` o `ping indirizzo_ip`
- **Output**: Tempo di risposta (in ms) e percentuale di pacchetti persi
- **Esempio**: `ping google.com`

#### `traceroute` (Windows: `tracert`):
- **Funzione**: Mostra il percorso dei pacchetti verso una destinazione
- **Utilizzo**: `traceroute hostname` o `traceroute indirizzo_ip`
- **Output**: Lista di router attraversati e tempi di risposta
- **Esempio**: `traceroute example.com`

#### `nslookup` / `dig`:
- **Funzione**: Interroga i server DNS
- **Utilizzo**: `nslookup dominio` o `dig dominio`
- **Output**: Indirizzi IP associati al dominio e server DNS utilizzati
- **Esempio**: `nslookup google.com`

#### `curl` / `wget`:
- **Funzione**: Interagisce con server web e scarica contenuti
- **Utilizzo**: `curl URL` o `wget URL`
- **Output**: Contenuto della risposta o file scaricato
- **Esempio**: `curl -I https://example.com` (solo header)

#### `netstat`:
- **Funzione**: Mostra connessioni di rete attive
- **Utilizzo**: `netstat -tuln`
- **Output**: Lista di porte in ascolto e connessioni stabilite
- **Esempio**: `netstat -tuln | grep LISTEN`

### Monitoraggio del Traffico di Rete
Per analisi più approfondite, esistono strumenti specializzati:

#### Interpretazione dei Risultati:
- **Latenza**: Tempo di risposta (ping)
  - < 20ms: Eccellente
  - 20-100ms: Buona
  - 100-300ms: Accettabile
  - > 300ms: Problematica
- **Perdita di pacchetti**: Percentuale di pacchetti non consegnati
  - 0%: Ottimale
  - 1-2%: Accettabile
  - > 5%: Problematica
- **Codici di stato HTTP**:
  - 2xx: Successo (es. 200 OK)
  - 3xx: Reindirizzamento (es. 301 Moved Permanently)
  - 4xx: Errore client (es. 404 Not Found)
  - 5xx: Errore server (es. 500 Internal Server Error)

### Esercizio Pratico: Diagnostica di Rete
**Obiettivo**: Risolvere problemi di connettività simulati

**Attività**:
1. **Scenario 1: "Il sito non risponde"**
   - Verificare la connettività di base con `ping`
   - Controllare la risoluzione DNS con `nslookup`
   - Analizzare il percorso con `traceroute`
   - Identificare se è un problema DNS o di connettività

2. **Scenario 2: "Connessione lenta"**
   - Misurare i tempi di risposta con `ping`
   - Identificare colli di bottiglia con `traceroute`
   - Verificare la velocità di download con `curl -o /dev/null URL`

3. **Scenario 3: "Accesso bloccato"**
   - Verificare se il servizio è in ascolto con `netstat`
   - Controllare se il firewall blocca la connessione
   - Testare con diversi protocolli e porte

## Parte 4: Applicazioni e Servizi (1 ora)

### Differenza tra Applicazioni e Servizi
È importante distinguere tra applicazioni e servizi:

#### Applicazioni:
- **Definizione**: Programmi che eseguiamo attivamente per svolgere compiti specifici
- **Caratteristiche**:
  - Interazione diretta con l'utente
  - Avvio e arresto espliciti
  - Interfaccia utente (grafica o testuale)
- **Esempi**: Browser web, editor di testo, giochi

#### Servizi:
- **Definizione**: Programmi che funzionano in background per fornire funzionalità al sistema o ad altre applicazioni
- **Caratteristiche**:
  - Esecuzione continua o su richiesta
  - Nessuna o limitata interazione diretta con l'utente
  - Avvio automatico all'avvio del sistema
- **Esempi**: Web server, database server, servizio di stampa

#### Relazione:
- Le applicazioni spesso utilizzano i servizi
- Un programma può funzionare sia come applicazione che come servizio

### Processi e Demoni
Nel sistema operativo, applicazioni e servizi sono rappresentati come processi:

#### Processi:
- **Definizione**: Istanza di un programma in esecuzione
- **Caratteristiche**:
  - Identificato da un PID (Process ID)
  - Allocazione di memoria e risorse
  - Stato di esecuzione (running, sleeping, stopped)
- **Visualizzazione**: 
  - Linux/macOS: `ps aux`
  - Windows: Task Manager

#### Demoni (Linux/Unix) o Servizi (Windows):
- **Definizione**: Processi in background che forniscono servizi
- **Caratteristiche**:
  - Avvio automatico all'avvio del sistema
  - Nessun terminale di controllo
  - Nomi spesso terminanti con "d" in Linux (httpd, sshd)
- **Gestione**:
  - Linux (systemd): `systemctl start/stop/status servizio`
  - Windows: Services Manager

### Dipendenze Software
Le applicazioni e i servizi raramente funzionano in isolamento:

#### Librerie Condivise:
- **Definizione**: Codice riutilizzabile condiviso tra più programmi
- **Vantaggi**:
  - Riduzione della duplicazione del codice
  - Aggiornamenti centralizzati
  - Risparmio di memoria
- **Esempi**: 
  - Linux: .so (shared object)
  - Windows: .dll (dynamic link library)

#### Gestori di Pacchetti:
- **Funzione**: Installano, aggiornano e rimuovono software gestendo le dipendenze
- **Esempi**:
  - Linux: apt (Debian/Ubuntu), yum/dnf (Red Hat/Fedora), apk (Alpine)
  - macOS: Homebrew, MacPorts
  - Windows: Windows Store, Chocolatey
  - Linguaggi di programmazione: npm (Node.js), pip (Python)

#### Problemi Comuni:
- **Dipendenze mancanti**: "Library not found"
- **Conflitti di versione**: Diverse applicazioni richiedono versioni diverse
- **Dipendenze circolari**: A dipende da B che dipende da A
- **Hell delle dipendenze**: Catene complesse di dipendenze

### Esercizio Pratico: Esplorazione di Applicazioni e Servizi
**Obiettivo**: Comprendere le relazioni tra applicazioni, servizi e dipendenze

**Attività**:
1. Identificare i servizi in esecuzione sul sistema:
   - Linux: `systemctl list-units --type=service --state=running`
   - Windows: Services Manager
2. Analizzare le dipendenze di un'applicazione:
   - Linux: `ldd /path/to/executable`
   - Windows: Process Explorer
3. Gestire un servizio semplice:
   - Avviare, fermare e verificare lo stato
   - Configurare l'avvio automatico
4. Creare una mappa delle relazioni tra applicazioni e servizi

## Parte 5: Introduzione alla Virtualizzazione (1 ora)

### Cos'è la Virtualizzazione
La virtualizzazione è una tecnologia che permette di creare versioni virtuali di risorse fisiche:

#### Definizione:
- Creazione di una versione virtuale (non fisica) di qualcosa
- Astrazione delle risorse hardware
- Metafora: Appartamenti in un condominio vs case singole

#### Tipi di Virtualizzazione:
- **Virtualizzazione hardware/piattaforma**: Macchine virtuali complete
- **Virtualizzazione del sistema operativo**: Container
- **Virtualizzazione di rete**: Reti virtuali
- **Virtualizzazione dello storage**: Dischi virtuali

### Macchine Virtuali e Hypervisor
Le macchine virtuali (VM) sono computer completi simulati all'interno di un computer fisico:

#### Macchina Virtuale:
- **Definizione**: Ambiente isolato che emula un computer completo
- **Componenti**:
  - Sistema operativo guest
  - Hardware virtualizzato (CPU, RAM, disco, rete)
  - File di configurazione

#### Hypervisor:
- **Definizione**: Software che crea e gestisce macchine virtuali
- **Tipi**:
  - **Tipo 1 (bare metal)**: Esegue direttamente sull'hardware
    - Esempi: VMware ESXi, Microsoft Hyper-V, Xen
    - Uso: Server, data center
  - **Tipo 2 (hosted)**: Esegue su un sistema operativo host
    - Esempi: VirtualBox, VMware Workstation, Parallels
    - Uso: Desktop, sviluppo, test

#### Funzionamento:
1. L'hypervisor alloca risorse fisiche alle VM
2. Ogni VM funziona come un computer indipendente
3. Le VM sono isolate tra loro
4. L'hypervisor gestisce la comunicazione tra VM e hardware

### Vantaggi della Virtualizzazione
La virtualizzazione offre numerosi benefici:

#### Isolamento:
- Separazione completa tra ambienti
- Sicurezza migliorata
- Problemi in una VM non influenzano le altre

#### Efficienza delle Risorse:
- Consolidamento di più server su un singolo host
- Migliore utilizzo dell'hardware
- Riduzione dei costi energetici e di spazio

#### Portabilità:
- VM indipendenti dall'hardware sottostante
- Facile migrazione tra host diversi
- Backup e ripristino semplificati

#### Flessibilità:
- Esecuzione di diversi sistemi operativi contemporaneamente
- Creazione rapida di nuovi ambienti
- Snapshot e clonazione

### Da VM a Container: Un'Anteprima
I container rappresentano un'evoluzione della virtualizzazione:

#### Differenze Fondamentali:
- **VM**: Virtualizzano l'hardware completo, includono un intero SO
- **Container**: Virtualizzano solo il sistema operativo, condividono il kernel host

#### Confronto:
- **Dimensione**:
  - VM: Gigabyte
  - Container: Megabyte
- **Avvio**:
  - VM: Minuti
  - Container: Secondi
- **Isolamento**:
  - VM: Completo (hardware + SO)
  - Container: Parziale (processi + filesystem)

#### Docker come Piattaforma di Containerizzazione:
- Standardizzazione del formato dei container
- Strumenti per creare, distribuire e gestire container
- Ecosistema di immagini pronte all'uso
- Bridge tra sviluppo e produzione

### Esercizio Finale: Discussione sui Casi d'Uso
**Obiettivo**: Comprendere quando usare VM, quando usare container

**Attività**:
1. Discutere scenari adatti alle VM:
   - Esecuzione di sistemi operativi diversi
   - Isolamento completo per sicurezza
   - Test di compatibilità hardware
2. Discutere scenari adatti ai container:
   - Microservizi
   - Sviluppo e test di applicazioni
   - Deployment scalabile
3. Identificare casi ibridi:
   - Container all'interno di VM
   - Orchestrazione di container su cluster di VM

## Conclusione
In questa sessione abbiamo esplorato i concetti fondamentali di reti, applicazioni, servizi e virtualizzazione. Queste conoscenze sono es
(Content truncated due to size limit. Use line ranges to read in chunks)