---
marp: true
theme: default
paginate: true
---

## 📍 Navigazione

**Fase**: Preliminare  |  **Modulo**: 2  |  **Tipo**: Slide

### ✅ Prerequisiti

Queste slide sono il supporto visivo alla teoria:
- [ ] [Sessione Introduttiva 2 — Teoria](intro-02-teoria-reti-servizi-vm.md) *(da consultare in parallelo)*

### 🎯 Obiettivi di questo modulo

Al termine saprai:
- Visualizzare i concetti chiave di reti, servizi e virtualizzazione
- Avere un riferimento rapido per diagrammi e schemi della Sessione 2

---

# SLIDE: SESSIONE INTRODUTTIVA 2
## RETI, APPLICAZIONI E SERVIZI

---

### Slide 1: Introduzione alla Sessione
- **Titolo:** Reti, Applicazioni e Servizi: Connettere il Mondo Digitale
- **Immagine:** Rete interconnessa di dispositivi con applicazioni e servizi che fluiscono tra loro
- **Obiettivi della sessione:**
  - Comprendere i concetti fondamentali delle reti informatiche
  - Esplorare il modello client-server e le sue applicazioni
  - Imparare a utilizzare strumenti di diagnostica di rete
  - Distinguere tra applicazioni e servizi
  - Introdurre i concetti di virtualizzazione
  - **Collegamento con Docker:** Preparare le basi per comprendere il networking in Docker

---

## PARTE 1: CONCETTI FONDAMENTALI DI RETI

### Slide 2: Cos'è una Rete Informatica?
- **Titolo:** Reti Informatiche: Il Sistema Circolatorio Digitale
- **Immagine:** Sistema di strade e autostrade che collegano città (computer)
- **Contenuto:**
  - Definizione: insieme di dispositivi interconnessi che comunicano tra loro
  - Scopo: condivisione di risorse e informazioni
  - Metafora: sistema postale digitale
  - **Collegamento con Docker:** I container comunicano attraverso reti virtuali

---

### Slide 3: Tipi di Reti
- **Titolo:** Dalle Piccole alle Grandi Reti
- **Immagine:** Diagramma che mostra i diversi tipi di reti con scale crescenti
- **Contenuto:**
  - **LAN (Local Area Network):**
    - Rete locale (casa, ufficio, scuola)
    - Alta velocità, bassa latenza
    - Esempio: rete Wi-Fi domestica
  - **WAN (Wide Area Network):**
    - Rete geografica estesa
    - Collega LAN distanti
    - Esempio: rete aziendale con sedi in diverse città
  - **Internet:**
    - La "rete delle reti"
    - Collega miliardi di dispositivi in tutto il mondo
- **Verifica:** Identifica a quale tipo di rete appartiene la connessione che stai utilizzando

---

### Slide 4: Indirizzi IP
- **Titolo:** Indirizzi IP: I Numeri Civici di Internet
- **Immagine:** Mappa di una città con indirizzi evidenziati
- **Contenuto:**
  - Definizione: numeri che identificano univocamente dispositivi in rete
  - **IPv4:** formato a 32 bit (es. 192.168.1.1)
    - Limitato a circa 4,3 miliardi di indirizzi
  - **IPv6:** formato a 128 bit (es. 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
    - Praticamente illimitato
  - **Indirizzi privati vs pubblici:**
    - Privati: usati nelle reti locali (192.168.x.x, 10.x.x.x)
    - Pubblici: univoci su Internet
- **Collegamento con Docker:** I container hanno i propri indirizzi IP all'interno di reti Docker

---

### Slide 5: DNS - Il Sistema dei Nomi di Dominio
- **Titolo:** DNS: L'Elenco Telefonico di Internet
- **Immagine:** Rubrica telefonica che traduce nomi in numeri
- **Contenuto:**
  - Funzione: traduce nomi di dominio in indirizzi IP
  - Esempio: www.example.com → 93.184.216.34
  - Gerarchia DNS:
    - Root servers
    - Top-level domain servers (.com, .org, .it)
    - Authoritative name servers
  - Metafora: chiedere informazioni a una catena di persone sempre più specializzate
- **Esercizio rapido:** Usa nslookup o dig per trovare l'indirizzo IP di un sito web famoso

---

### Slide 6: Porte e Protocolli
- **Titolo:** Porte e Protocolli: Le Regole della Comunicazione
- **Immagine:** Edificio con diverse porte numerate per servizi specifici
- **Contenuto:**
  - **Porte:** canali numerati (0-65535) per servizi specifici
    - 80: HTTP (web)
    - 443: HTTPS (web sicuro)
    - 22: SSH (accesso remoto)
    - 25: SMTP (email)
  - **Protocolli:** regole che governano la comunicazione
    - HTTP/HTTPS: trasferimento pagine web
    - TCP: trasmissione affidabile
    - UDP: trasmissione veloce
    - IP: instradamento pacchetti
- **Collegamento con Docker:** Le porte sono fondamentali per esporre servizi dai container

---

### Slide 7: Come Viaggiano i Dati in Rete
- **Titolo:** Il Viaggio di un Pacchetto di Dati
- **Immagine:** Animazione del percorso di un pacchetto attraverso router e switch
- **Contenuto:**
  - Suddivisione dei dati in pacchetti
  - Instradamento attraverso router
  - Riassemblaggio a destinazione
  - Metafora: sistema postale che scompone e ricompone un pacco grande
- **Verifica:** Quali dispositivi attraversa un pacchetto di dati per raggiungere un server web?

---

### Slide 8: Esercizio Pratico - Analisi della Propria Rete
- **Titolo:** Conosciamo la Nostra Rete
- **Contenuto:** Istruzioni passo-passo per:
  - Identificare il proprio indirizzo IP locale:
    - Windows: `ipconfig`
    - Linux/macOS: `ip addr`
  - Identificare il proprio indirizzo IP pubblico
  - Testare la connettività con `ping`
  - Risolvere nomi di dominio con `nslookup` o `dig`
- **Verifica:** Compila la "carta d'identità" della tua connessione di rete

---

## PARTE 2: MODELLO CLIENT-SERVER

### Slide 9: L'Architettura Client-Server
- **Titolo:** Client e Server: Una Relazione di Servizio
- **Immagine:** Ristorante con clienti (client) e camerieri/cuochi (server)
- **Contenuto:**
  - **Client:** dispositivo o software che richiede servizi
  - **Server:** dispositivo o software che fornisce servizi
  - Caratteristiche:
    - Separazione dei ruoli
    - Centralizzazione delle risorse
    - Scalabilità
  - **Collegamento con Docker:** I container spesso implementano servizi in un'architettura client-server

---

### Slide 10: Come Funziona la Comunicazione Client-Server
- **Titolo:** Il Dialogo Client-Server
- **Immagine:** Diagramma di sequenza di una comunicazione
- **Contenuto:**
  - 1. Il client formula una richiesta
  - 2. La richiesta viene inviata al server
  - 3. Il server processa la richiesta
  - 4. Il server invia la risposta
  - 5. Il client riceve e interpreta la risposta
- **Esercizio rapido:** Identifica client e server in scenari quotidiani (email, navigazione web, streaming video)

---

### Slide 11: Esempio Concreto: Navigazione Web
- **Titolo:** Anatomia di una Richiesta Web
- **Immagine:** Diagramma dettagliato del processo di caricamento di una pagina web
- **Contenuto:**
  - 1. Browser (client) richiede una pagina web
  - 2. Richiesta HTTP inviata al server web
  - 3. Server web elabora la richiesta
  - 4. Server invia la pagina HTML come risposta
  - 5. Browser riceve l'HTML e lo visualizza
  - 6. Browser richiede risorse aggiuntive (CSS, JavaScript, immagini)
- **Collegamento con Docker:** I container web funzionano come server in questo processo

---

### Slide 12: Esempi di Sistemi Client-Server
- **Titolo:** Client-Server Nella Vita Quotidiana
- **Contenuto:** Tabella con esempi:
  - **Web:**
    - Client: Browser (Chrome, Firefox)
    - Server: Web server (Apache, Nginx)
    - Protocollo: HTTP/HTTPS
  - **Email:**
    - Client: Client di posta (Outlook, Gmail)
    - Server: Mail server
    - Protocollo: SMTP, IMAP, POP3
  - **Database:**
    - Client: Applicazione
    - Server: DBMS (MySQL, PostgreSQL)
    - Protocollo: Specifico del database
- **Verifica:** Identifica altri esempi di sistemi client-server che utilizzi quotidianamente

---

### Slide 13-14: Esercizio Pratico - Analisi di Richieste Web
- **Titolo:** Vediamo il Client-Server in Azione
- **Contenuto:** Tutorial guidato per:
  - Aprire gli strumenti di sviluppo del browser (F12)
  - Selezionare la scheda "Network" (Rete)
  - Visitare un sito web
  - Analizzare le richieste generate:
    - Metodo (GET, POST)
    - URL richiesto
    - Codice di stato (200, 404, 500)
    - Tipo di contenuto
- **Verifica:** Crea un diagramma del flusso di richieste e risposte osservate

---

## PARTE 3: COMANDI DI RETE E DIAGNOSTICA

### Slide 15: Strumenti di Base per la Diagnostica
- **Titolo:** La Cassetta degli Attrezzi di Rete
- **Immagine:** Kit di strumenti per la riparazione di reti
- **Contenuto:**
  - `ping`: verifica la connettività con un host
  - `traceroute` (Windows: `tracert`): mostra il percorso dei pacchetti
  - `nslookup` / `dig`: interroga i server DNS
  - `curl` / `wget`: interagisce con server web
  - `ss`: mostra connessioni di rete attive
- **Collegamento con Docker:** Questi strumenti sono utili per diagnosticare problemi di rete nei container

---

### Slide 16: Ping e Traceroute
- **Titolo:** Ping e Traceroute: Testare la Connettività
- **Contenuto:**
  - **Ping:**
    - Funzione: verifica se un host è raggiungibile
    - Sintassi: `ping hostname` o `ping indirizzo_ip`
    - Output: tempo di risposta e pacchetti persi
    - Esempio: `ping google.com`
  - **Traceroute:**
    - Funzione: mostra il percorso verso una destinazione
    - Sintassi: `traceroute hostname` (Linux) o `tracert hostname` (Windows)
    - Output: lista di router attraversati
    - Esempio: `traceroute example.com`
- **Esercizio guidato:** Esegui ping e traceroute verso diversi siti e confronta i risultati

---

### Slide 17: DNS e Strumenti di Query
- **Titolo:** Interrogare il DNS: nslookup e dig
- **Contenuto:**
  - **nslookup:**
    - Funzione: risolve nomi di dominio in indirizzi IP
    - Sintassi: `nslookup dominio`
    - Esempio: `nslookup google.com`
  - **dig (Linux/macOS):**
    - Funzione: strumento avanzato per query DNS
    - Sintassi: `dig dominio [tipo]`
    - Esempio: `dig google.com MX` (per record di posta)
  - Interpretazione dell'output:
    - Record A: indirizzi IPv4
    - Record AAAA: indirizzi IPv6
    - Record MX: server di posta
    - Record CNAME: alias
- **Verifica:** Quali informazioni puoi ottenere da una query DNS completa?

---

### Slide 18: Curl e Wget
- **Titolo:** Interagire con Server Web: curl e wget
- **Contenuto:**
  - **curl:**
    - Funzione: trasferisce dati da/verso server
    - Sintassi base: `curl URL`
    - Opzioni comuni:
      - `-I`: solo header
      - `-o file`: salva output in file
      - `-X POST`: specifica metodo HTTP
    - Esempio: `curl -I https://example.com`
  - **wget:**
    - Funzione: scarica file da web
    - Sintassi base: `wget URL`
    - Opzioni comuni:
      - `-O file`: specifica nome file output
      - `-r`: download ricorsivo
    - Esempio: `wget https://example.com/file.zip`
- **Collegamento con Docker:** Utili per testare servizi web in container

---

### Slide 19: Interpretazione dei Risultati
- **Titolo:** Cosa ci Dicono i Risultati?
- **Contenuto:**
  - **Latenza (ping):**
    - < 20ms: Eccellente
    - 20-100ms: Buona
    - 100-300ms: Accettabile
    - > 300ms: Problematica
  - **Perdita di pacchetti:**
    - 0%: Ottimale
    - 1-2%: Accettabile
    - > 5%: Problematica
  - **Codici di stato HTTP:**
    - 2xx: Successo (es. 200 OK)
    - 3xx: Reindirizzamento (es. 301 Moved)
    - 4xx: Errore client (es. 404 Not Found)
    - 5xx: Errore server (es. 500 Internal Server Error)
- **Esercizio rapido:** Analizza i risultati ottenuti negli esercizi precedenti

---

### Slide 20: Esercizio Pratico - Diagnostica di Rete
- **Titolo:** Risolviamo Problemi di Rete
- **Contenuto:** Scenari simulati con problemi da risolvere:
  - **Scenario 1: "Il sito non risponde"**
    - Verificare connettività con `ping`
    - Controllare DNS con `nslookup`
    - Analizzare percorso con `traceroute`
  - **Scenario 2: "Connessione lenta"**
    - Misurare tempi di risposta
    - Identificare colli di bottiglia
  - **Scenario 3: "Accesso bloccato"**
    - Verificare servizio in ascolto
    - Controllare firewall
- **Verifica:** Compila una tabella di diagnosi per ciascuno scenario

---

## PARTE 4: APPLICAZIONI E SERVIZI

### Slide 21: Differenza tra Applicazioni e Servizi
- **Titolo:** Applicazioni vs Servizi: Qual è la Differenza?
- **Immagine:** Utensili (applicazioni) vs utilità sempre attive (servizi)
- **Contenuto:**
  - **Applicazioni:**
    - Programmi che eseguiamo attivamente
    - Interazione diretta con l'utente
    - Avvio e arresto espliciti
    - Esempi: browser, editor di testo, giochi
  - **Servizi:**
    - Programmi che funzionano in background
    - Poca o nessuna interazione diretta
    - Avvio automatico all'avvio del sistema
    - Esempi: web server, database, servizio di stampa
- **Collegamento con Docker:** I container spesso eseguono servizi piuttosto che applicazioni

---

### Slide 22: Processi e Demoni
- **Titolo:** Dietro le Quinte: Processi e Demoni
- **Immagine:** Teatro con attori sul palco (processi) e tecnici dietro le quinte (demoni)
- **Contenuto:**
  - **Processi:**
    - Istanza di un programma in esecuzione
    - Identificato da un PID (Process ID)
    - Allocazione di memoria e risorse
    - Visualizzazione: `ps aux` (Linux/macOS), Task Manager (Windows)
  - **Demoni (Linux) / Servizi (Windows):**
    - Processi in background
    - Avvio automatico
    - Nomi spesso terminanti con "d" in Linux (httpd, sshd)
    - Gestione: `rc-service` e `rc-status` (Alpine/OpenRC), Services Manager (Windows)
- **Esercizio rapido:** Identifica tre processi e un demone in esecuzione sul tuo sistema

---

### Slide 23: Gestione dei Servizi
- **Titolo:** Controllare i Servizi
- **Contenuto:**
  - **Linux (Alpine/OpenRC):**
    - `rc-service servizio start`: avvia un servizio
    - `rc-service servizio stop`: ferma un servizio
    - `rc-service servizio status`: verifica lo stato
    - `rc-update add servizio default`: abilita all'avvio
    - `rc-update del servizio default`: disabilita all'avvio
  - **Windows:**
    - Services Manager (interfaccia grafica)
    - `net start servizio`: avvia un servizio
    - `net stop servizio`: ferma un servizio
    - `sc query servizio`: verifica lo stato
- **Collegamento con Docker:** Docker stesso funziona come un servizio

---

### Slide 24: Dipendenze Software
- **Titolo:** L'Ecosistema del Software
- **Immagine:** Grafico di dipendenze tra componenti software
- **Contenuto:**
  - **Librerie condivise:**
    - Codice riutilizzabile tra programmi
    - Riduzione della duplicazione
    - Esempi: .so (Linux), .dll (Windows)
  - **Gestori di pacchetti:**
    - Installano e aggiornano software
    - Gestiscono le dipendenze
    - Esempi: apk (Alpine), yum/dnf (Red Hat/Fedora), pacman (Arch)
  - **Problemi comuni:**
    - Dipendenze mancanti
    - Conflitti di versione
    - Hell delle dipendenze
- **Collegamento con Docker:** I container risolvono molti problemi di dipendenze

---

### Slide 25-26: Esercizio Pratico - Esplorazione di Applicazioni e Servizi
- **Titolo:** Esploriamo Applicazioni e Servizi
- **Contenuto:** Tutorial guidato per:
  - Identificare servizi in esecuzione:
    - Linux: `rc-status`
    - Windows: Services Manager
  - Analizzare le dipendenze di un'applicazione:
    - Linux: `ldd /path/to/executable`
    - Windows: Process Explorer
  - Gestire un servizio semplice:
    - Avviare, fermare, verificare stato
    - Configurare avvio automatico
- **Verifica:** Crea una mappa delle relazioni tra applicazioni e servizi sul tuo sistema

---

## PARTE 5: INTRODUZIONE ALLA VIRTUALIZZAZIONE

### Slide 27: Cos'è la Virtualizzazione
- **Titolo:** Virtualizzazione: Computer dentro Computer
- **Immagine:** Matrioske (bambole russe)
- **Contenuto:**
  - **Definizione:** creazione di versioni virtuali di risorse fisiche
  - **Metafora:** appartamenti in un condominio vs case singole
  - **Tipi di virtualizzazione:**
    - Hardware/piattaforma: macchine virtuali
    - Sistema operativo: container
    - Rete: reti virtuali
    - Storage: dischi virtuali
- **Collegamento con Docker:** Docker è una forma di virtualizzazione a livello di sistema operativo

---

### Slide 28: Macchine Virtuali
- **Titolo:** Macchine Virtuali: Computer Completi Simulati
- **Immagine:** Diagramma di un computer fisico che ospita più VM
- **Contenuto:**
  - **Definizione:** ambiente isolato che emula un computer completo
  - **Componenti:**
    - Sistema operativo guest
    - Hardware virtualizzato (CPU, RAM, disco, rete)
    - File di configurazione
  - **Vantaggi:**
    - Isolamento completo
    - Diversi sistemi operativi su una macchina
    - Snapshot e backup
  - **Svantaggi:**
    - Overhead significativo
    - Avvio lento
    - Dimensioni grandi
- **Verifica:** Quali risorse hardware vengono virtualizzate in una VM?

---

### Slide 29: Hypervisor
- **Titolo:** Hypervisor: Il Gestore delle VM
- **Immagine:** Diagramma che mostra i due tipi di hypervisor
- **Contenuto:**
  - **Definizione:** software che crea e gestisce macchine virtuali
  - **Tipi:**
    - **Tipo 1 (bare metal):**
      - Esegue direttamente sull'hardware
      - Esempi: VMware ESXi, Microsoft Hyper-V, Xen
      - Uso: server, data center
    - **Tipo 2 (hosted):**
      - Esegue su un sistema operativo host
      - Esempi: VirtualBox, VMware Workstation, Parallels
      - Uso: desktop, sviluppo, test
  - **Funzionamento:** alloca risorse fisiche alle VM
- **Collegamento con Docker:** A differenza degli hypervisor, Docker condivide il kernel host

---

### Slide 30: Vantaggi della Virtualizzazione
- **Titolo:** Perché Virtualizzare?
- **Contenuto:**
  - **Isolamento:**
    - Separazione tra ambienti
    - Sicurezza migliorata
    - Problemi contenuti
  - **Efficienza delle risorse:**
    - Consolidamento di più server
    - Migliore utilizzo dell'hardware
    - Riduzione dei costi
  - **Portabilità:**
    - Indipendenza dall'hardware
    - Facile migrazione
    - Backup e ripristino
  - **Flessibilità:**
    - Diversi sistemi operativi
    - Creazione rapida di ambienti
    - Snapshot e clonazione
- **Esercizio rapido:** Identifica tre scenari in cui la virtualizzazione sarebbe vantaggiosa

---

### Slide 31-32: Da VM a Container
- **Titolo:** L'Evoluzione: Dai Computer Virtuali ai Container
- **Immagine:** Confronto visivo tra VM e container
- **Contenuto:**
  - **Differenze fondamentali:**
    - VM: virtualizzano l'hardware completo, includono un intero SO
    - Container: virtualizzano solo il sistema operativo, condividono il kernel host
  - **Confronto:**
    - **Dimensione:**
      - VM: Gigabyte
      - Container: Megabyte
    - **Avvio:**
      - VM: Minuti
      - Container: Secondi
    - **Isolamento:**
      - VM: Completo (hardware + SO)
      - Container: Parziale (processi + filesystem)
  - **Metafora:** VM come case indipendenti, container come appartamenti in un condominio
- **Collegamento con Docker:** Docker è la piattaforma di containerizzazione più popolare

---

### Slide 33: Docker come Piattaforma di Containerizzazione
- **Titolo:** Docker: Containerizzazione Semplificata
- **Immagine:** Logo Docker con i componenti principali
- **Contenuto:**
  - Standardizzazione del formato dei container
  - Strumenti per creare, distribuire e gestire container
  - Ecosistema di immagini pronte all'uso
  - Bridge tra sviluppo e produzione
  - Anteprima di ciò che vedremo nelle prossime esercitazioni
- **Verifica:** Quali problemi risolve Docker rispetto alle macchine virtuali tradizionali?

---

### Slide 34: Discussione Finale - Casi d'Uso
- **Titolo:** VM o Container? Scegliere lo Strumento Giusto
- **Contenuto:** Scenari di discussione:
  - **Quando usare VM:**
    - Esecuzione di sistemi operativi diversi
    - Isolamento completo per sicurezza
    - Test di compatibilità hardware
  - **Quando usare container:**
    - Microservizi
    - Sviluppo e test di applicazioni
    - Deployment scalabile
  - **Approcci ibridi:**
    - Container all'interno di VM
    - Orchestrazione di container su cluster di VM
- **Collegamento con le esercitazioni Docker:** Preparazione per il corso principale

---

## ESERCIZI PRATICI (RIMANDO OPERATIVO)

Queste slide restano un supporto visivo.
Per istruzioni complete, output attesi e checkpoint usa i materiali operativi:

- [Sessione Introduttiva 2 — Teoria](intro-02-teoria-reti-servizi-vm.md)
- [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)

### Traccia rapida in aula
1. Analisi della propria rete (IP locale/pubblico, DNS, ping)
2. Analisi delle richieste web dal browser (tab Network)
3. Diagnostica di rete su scenari guidati
4. Mappa dei servizi principali del sistema

---

## VERIFICHE DI COMPRENSIONE

### Quiz 1: Reti Informatiche
1. Qual è la differenza principale tra LAN e WAN?
2. Spiega la differenza tra indirizzo IP privato e pubblico.
3. A cosa serve il sistema DNS?
4. Cosa rappresenta il numero di porta in una connessione di rete?
5. Elenca tre protocolli di rete comuni e le loro funzioni.

### Quiz 2: Modello Client-Server
1. Definisci i ruoli di client e server in un'architettura client-server.
2. Descrivi il ciclo di vita di una richiesta web.
3. Quali sono i vantaggi dell'architettura client-server?
4. Fai un esempio di sistema client-server che non sia la navigazione web.
5. Cosa significano i codici di stato HTTP 200, 404 e 500?

### Quiz 3: Diagnostica di Rete
1. Quale comando useresti per verificare la connettività con un host?
2. Come puoi determinare il percorso che i pacchetti seguono verso una destinazione?
3. Quale strumento useresti per risolvere un nome di dominio in un indirizzo IP?
4. Come puoi verificare quali porte sono in ascolto sul tuo sistema?
5. Quali informazioni puoi ottenere dal comando `curl -I https://example.com`?

### Quiz 4: Applicazioni e Servizi
1. Qual è la differenza principale tra un'applicazione e un servizio?
2. Cosa sono i demoni in un sistema Linux?
3. Come gestiresti un servizio in Linux usando OpenRC (`rc-service` e `rc-update`)?
4. Cosa sono le librerie condivise e perché sono importanti?
5. Quali problemi possono sorgere con le dipendenze software?

### Quiz 5: Virtualizzazione
1. Definisci la virtualizzazione e i suoi principali vantaggi.
2. Qual è la differenza tra un hypervisor di tipo 1 e di tipo 2?
3. Elenca tre vantaggi delle macchine virtuali.
4. Quali sono le principali differenze tra macchine virtuali e container?
5. Perché Docker è importante nel contesto della containerizzazione?

---

## COLLEGAMENTI CON LE ALTRE SESSIONI

### Collegamenti con la Sessione Introduttiva 1
- I concetti di shell e comandi bash si applicano alla diagnostica di rete
- La struttura del filesystem è correlata all'organizzazione dei servizi
- I permessi dei file sono importanti per la sicurezza delle applicazioni e dei servizi

### Collegamenti con l'Esercitazione 1 su Docker
- Il modello client-server è alla base dell'architettura Docker (client Docker comunica con daemon Docker)
- I concetti di rete sono fondamentali per comprendere come i container comunicano
- La comprensione dei servizi prepara all'esecuzione di applicazioni in container

### Collegamenti con l'Esercitazione 2 su Docker
- I concetti di porte e protocolli sono essenziali per esporre servizi dai container
- Le dipendenze software sono gestite in modo diverso nei container
- La diagnostica di rete si applica alla risoluzione di problemi di networking in Docker

### Collegamenti con l'Esercitazione 3 su Docker
- La virtualizzazione è il concetto fondamentale su cui si basa Docker
- Il networking avanzato in Docker si basa sui concetti di rete appresi
- L'orchestrazione di container estende i concetti di gestione dei servizi

---

## ➡️ Prossimi passi

- **Configura la rete**: [Lab Modalità di Rete VirtualBox](../fase-0-prerequisiti/00-prerequisiti-rete-virtualbox.md)
- **Metti in pratica**: [Lab Sessione 2](intro-02-lab-reti-servizi-vm.md)
- **Rivedi la teoria**: [Sessione Introduttiva 2 — Teoria](intro-02-teoria-reti-servizi-vm.md)
- **Hai dubbi?** Consulta [CHEATSHEET.md](../CHEATSHEET.md)

↩ [Torna all'indice principale](../README.md)
