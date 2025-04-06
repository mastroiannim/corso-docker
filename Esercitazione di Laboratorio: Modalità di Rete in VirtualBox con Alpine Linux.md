# Esercitazione di Laboratorio: Modalità di Rete in VirtualBox con Alpine Linux

## Introduzione

Questa esercitazione di laboratorio è progettata per aiutare gli studenti a comprendere il funzionamento delle diverse modalità di rete disponibili in VirtualBox. Attraverso una serie di scenari pratici, gli studenti impareranno a configurare, testare e risolvere problemi relativi alle diverse modalità di rete utilizzando Alpine Linux, una distribuzione leggera e ottimizzata per ambienti virtualizzati.

### Obiettivi Didattici

Al termine di questa esercitazione, gli studenti saranno in grado di:

1. Comprendere le differenze tra le diverse modalità di rete in VirtualBox (NAT, Bridge, Host-Only, Internal Network)
2. Configurare correttamente ciascuna modalità di rete in VirtualBox
3. Installare e configurare Alpine Linux in ambiente virtualizzato
4. Implementare scenari di rete realistici utilizzando le diverse modalità
5. Diagnosticare e risolvere problemi comuni di rete in ambiente virtualizzato
6. Valutare quale modalità di rete è più adatta per specifici casi d'uso

### Requisiti

#### Hardware
- Computer con processore multi-core (almeno 2 core)
- Minimo 8GB di RAM (consigliati 16GB)
- Almeno 20GB di spazio libero su disco
- Scheda di rete funzionante (per la modalità Bridge)

#### Software
- VirtualBox (versione 6.1 o superiore)
- Immagine ISO di Alpine Linux (versione virt)
- Software per la cattura di screenshot (opzionale)
- Editor di testo per prendere appunti

### Tempistiche

L'esercitazione è progettata per essere completata in circa 3 ore, suddivise come segue:

- Preparazione dell'ambiente: 30 minuti
- Scenario 1 (Modalità NAT): 30 minuti
- Scenario 2 (Modalità Bridge): 30 minuti
- Scenario 3 (Modalità Host-Only): 30 minuti
- Scenario 4 (Modalità Internal Network): 45 minuti
- Troubleshooting e riflessioni finali: 15 minuti

## Preparazione dell'Ambiente

### Download e Installazione di VirtualBox

1. Scaricare VirtualBox dal sito ufficiale: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. Installare VirtualBox seguendo le istruzioni per il proprio sistema operativo
3. Installare anche il "VirtualBox Extension Pack" per funzionalità aggiuntive

### Download di Alpine Linux

1. Scaricare l'immagine ISO di Alpine Linux ottimizzata per la virtualizzazione:
   [https://alpinelinux.org/downloads/](https://alpinelinux.org/downloads/)
2. Scegliere la versione "Virtual" per l'architettura appropriata (x86_64 per sistemi a 64 bit)
3. Salvare l'immagine ISO in una posizione facilmente accessibile

### Creazione della Prima Macchina Virtuale

1. Avviare VirtualBox e fare clic su "Nuova"
2. Configurare la VM con i seguenti parametri:
   - Nome: `Alpine-Base`
   - Tipo: Linux
   - Versione: Other Linux (64-bit)
   - Memoria: 512MB
   - Disco rigido: Creare un nuovo disco virtuale
   - Tipo di file: VDI (VirtualBox Disk Image)
   - Archiviazione: Dinamicamente allocato
   - Dimensione: 2GB

3. Selezionare la VM appena creata e fare clic su "Impostazioni"
4. Nella sezione "Archiviazione", selezionare il controller IDE vuoto e aggiungere l'immagine ISO di Alpine Linux
5. Fare clic su "OK" per salvare le impostazioni

### Installazione di Base di Alpine Linux

1. Avviare la VM facendo clic su "Avvia"
2. Al prompt di avvio, accedere come `root` (senza password)
3. Eseguire il comando di installazione:
   ```
   setup-alpine
   ```
4. Seguire la procedura guidata di installazione:
   - Selezionare la tastiera (es. `it` per italiano)
   - Impostare il nome host (es. `alpine-base`)
   - Configurare la rete:
     - Selezionare l'interfaccia `eth0`
     - Scegliere la configurazione DHCP
     - Non configurare IPv6 per semplicità
   - Impostare il fuso orario (es. `Europe/Rome`)
   - Configurare i repository (usare i mirror predefiniti)
   - Impostare una password per l'utente root
   - Scegliere il disco di installazione (solitamente `sda`)
   - Scegliere `sys` come modalità di installazione
   - Confermare la formattazione del disco

5. Al termine dell'installazione, riavviare la VM:
   ```
   reboot
   ```

6. Rimuovere l'immagine ISO dal controller IDE nelle impostazioni della VM prima di riavviare

### Creazione di un Template

Per risparmiare tempo durante l'esercitazione, è consigliabile creare un template della VM di base:

1. Spegnere la VM `Alpine-Base`
2. In VirtualBox, fare clic con il tasto destro sulla VM e selezionare "Clona"
3. Assegnare un nome significativo (es. `Alpine-Template`)
4. Selezionare "Clonazione completa"
5. Fare clic su "Clona"

Questo template servirà come base per tutte le VM che creeremo durante l'esercitazione.

## Scenario 1: Modalità NAT

### Introduzione alla modalità NAT

La modalità NAT (Network Address Translation) è la configurazione di rete predefinita in VirtualBox e rappresenta il modo più semplice per consentire a una macchina virtuale di accedere a reti esterne. In questa modalità, VirtualBox agisce come un router tra la VM e la rete esterna, utilizzando l'indirizzo IP dell'host per le comunicazioni verso l'esterno.

### Concetti chiave della modalità NAT

- **Funzionamento**: La VM ottiene un indirizzo IP privato da un server DHCP integrato in VirtualBox (tipicamente nella rete 10.0.2.0/24)
- **Mascheramento**: Tutto il traffico in uscita dalla VM viene mascherato con l'indirizzo IP dell'host
- **Isolamento**: Per impostazione predefinita, le VM configurate con NAT non sono raggiungibili dall'esterno né da altre VM
- **Port Forwarding**: È possibile configurare il port forwarding per rendere accessibili dall'esterno specifici servizi in esecuzione sulla VM

### Creazione della VM Alpine Linux per NAT

1. Clonare il template `Alpine-Template` creato in precedenza:
   - Nome: `Alpine-NAT`
   - Selezionare "Clonazione completa"

2. Selezionare la VM appena creata e fare clic su "Impostazioni"
3. Nella sezione "Rete", verificare che l'adattatore 1 sia impostato su "NAT" (questa è l'impostazione predefinita)
4. Fare clic su "OK" per salvare le impostazioni

### Verifica della configurazione di rete

1. Avviare la VM `Alpine-NAT`
2. Accedere come `root` con la password impostata durante l'installazione
3. Verificare la configurazione di rete con i seguenti comandi:
   ```
   ip addr show
   ```
   Dovresti vedere l'interfaccia `eth0` con un indirizzo IP nella rete 10.0.2.0/24

4. Verificare la tabella di routing:
   ```
   ip route show
   ```
   Dovresti vedere un gateway predefinito (10.0.2.2) che è il router NAT di VirtualBox

5. Verificare la connettività verso Internet:
   ```
   ping -c 4 8.8.8.8
   ```

6. Verificare la risoluzione DNS:
   ```
   ping -c 4 google.com
   ```

### Esercizio 1: Analisi del funzionamento NAT

#### Attività
1. Installare gli strumenti di rete in Alpine Linux:
   ```
   apk update
   apk add tcpdump iptables curl
   ```

2. Analizzare il traffico di rete durante una richiesta HTTP:
   ```
   tcpdump -i eth0 -n host 8.8.8.8 &
   ping -c 4 8.8.8.8
   fg
   # Premere Ctrl+C per terminare tcpdump
   ```

3. Verificare l'indirizzo IP pubblico della VM:
   ```
   curl ifconfig.me
   ```
   Nota: questo indirizzo dovrebbe corrispondere all'indirizzo IP pubblico dell'host

#### Domande di riflessione
- Perché l'indirizzo IP pubblico della VM corrisponde a quello dell'host?
- Quali sono i vantaggi di utilizzare il NAT in termini di sicurezza?
- Come viene gestito il traffico in entrata nella modalità NAT?
- Quali limitazioni presenta questa configurazione?

### Esercizio 2: Configurazione del Port Forwarding

#### Attività
1. Installare un server web sulla VM:
   ```
   apk add lighttpd
   rc-update add lighttpd default
   rc-service lighttpd start
   ```

2. Creare una semplice pagina web:
   ```
   echo "<html><body><h1>Alpine Linux NAT Test</h1><p>Questa pagina è servita da una VM Alpine Linux in modalità NAT</p></body></html>" > /var/www/localhost/htdocs/index.html
   ```

3. Verificare che il server web funzioni localmente:
   ```
   curl http://localhost
   ```

4. Spegnere la VM:
   ```
   poweroff
   ```

5. Configurare il port forwarding in VirtualBox:
   - Selezionare la VM `Alpine-NAT`
   - Fare clic su "Impostazioni"
   - Selezionare "Rete"
   - Espandere "Avanzate"
   - Fare clic su "Inoltro delle porte"
   - Aggiungere una nuova regola:
     - Nome: `HTTP`
     - Protocollo: `TCP`
     - IP host: lasciare vuoto (tutte le interfacce)
     - Porta host: `8080`
     - IP guest: lasciare vuoto (IP assegnato dal DHCP)
     - Porta guest: `80`
   - Fare clic su "OK" per salvare

6. Avviare la VM e verificare che il server web sia accessibile dall'host:
   - Aprire un browser sull'host e navigare a `http://localhost:8080`
   - Dovresti vedere la pagina web creata in precedenza

#### Domande di riflessione
- Come funziona il port forwarding in VirtualBox?
- Quali sono i rischi di sicurezza associati all'apertura di porte verso la VM?
- Come si potrebbe limitare l'accesso al servizio solo a specifici indirizzi IP?
- Quali altri servizi potrebbero essere utili esporre tramite port forwarding?

### Esercizio 3: Limitazioni della modalità NAT

#### Attività
1. Creare una seconda VM Alpine Linux con configurazione NAT:
   - Clonare il template `Alpine-Template`
   - Chiamarla `Alpine-NAT2`
   - Verificare che l'adattatore di rete sia impostato su NAT

2. Avviare entrambe le VM e verificare i loro indirizzi IP:
   ```
   # Su Alpine-NAT
   ip addr show eth0
   
   # Su Alpine-NAT2
   ip addr show eth0
   ```

3. Tentare di effettuare il ping tra le due VM:
   ```
   # Da Alpine-NAT a Alpine-NAT2
   ping -c 4 <indirizzo_ip_di_Alpine-NAT2>
   ```

4. Osservare che le VM non possono comunicare direttamente tra loro

#### Domande di riflessione
- Perché le VM in modalità NAT non possono comunicare direttamente tra loro?
- Quali modalità di rete permetterebbero la comunicazione diretta tra VM?
- Come si potrebbe configurare la comunicazione tra VM in modalità NAT?
- In quali scenari reali la modalità NAT è la scelta migliore?

### Vantaggi e limitazioni della modalità NAT

#### Vantaggi
- Configurazione semplice e predefinita
- La VM può accedere a Internet senza configurazioni aggiuntive
- Maggiore sicurezza: la VM è protetta da accessi esterni non autorizzati
- Non richiede indirizzi IP aggiuntivi nella rete fisica
- Funziona in qualsiasi ambiente di rete, anche con restrizioni

#### Limitazioni
- Le VM non sono direttamente accessibili dalla rete esterna senza port forwarding
- Le VM con NAT non possono comunicare direttamente tra loro
- Il port forwarding può diventare complesso con molti servizi
- Possibili problemi con protocolli che non gestiscono bene il NAT
- Non adatto per scenari che richiedono connettività diretta

## Scenario 2: Modalità Bridge

### Introduzione alla modalità Bridge

La modalità Bridge (Bridged Networking) è una configurazione di rete in VirtualBox che collega direttamente la scheda di rete virtuale della VM alla scheda di rete fisica dell'host. In questa modalità, la VM si comporta come un computer fisico indipendente collegato alla stessa rete dell'host, ottenendo un indirizzo IP dalla stessa rete fisica.

### Concetti chiave della modalità Bridge

- **Connessione diretta**: La VM è collegata direttamente alla rete fisica dell'host
- **Visibilità completa**: La VM è visibile e accessibile da tutti i dispositivi nella rete locale
- **Indirizzo IP**: La VM ottiene un indirizzo IP dalla stessa rete dell'host (tramite DHCP o configurazione statica)
- **Indipendenza**: La VM funziona come un computer fisico indipendente nella rete

### Creazione della VM Alpine Linux per Bridge

1. Clonare il template `Alpine-Template` creato in precedenza:
   - Nome: `Alpine-Bridge`
   - Selezionare "Clonazione completa"

2. Selezionare la VM appena creata e fare clic su "Impostazioni"
3. Nella sezione "Rete", modificare l'adattatore 1:
   - Abilitare la scheda di rete
   - Connessa a: "Scheda con bridge"
   - Nome: selezionare l'interfaccia di rete fisica dell'host (es. eth0, wlan0)
   - Modalità promiscua: "Consenti tutto"
   - Tipo di scheda: lasciare il valore predefinito
4. Fare clic su "OK" per salvare le impostazioni

> **Nota importante**: La modalità Bridge richiede che l'host sia connesso a una rete fisica. Se si utilizza un laptop con Wi-Fi, selezionare l'interfaccia wireless. Se l'host ha più interfacce di rete, scegliere quella attualmente connessa alla rete.

### Verifica della configurazione di rete

1. Avviare la VM `Alpine-Bridge`
2. Accedere come `root` con la password impostata durante l'installazione
3. Verificare la configurazione di rete con i seguenti comandi:
   ```
   ip addr show
   ```
   Dovresti vedere l'interfaccia `eth0` con un indirizzo IP nella stessa rete dell'host

4. Verificare la tabella di routing:
   ```
   ip route show
   ```
   Dovresti vedere un gateway predefinito che corrisponde al router della rete fisica

5. Verificare la connettività verso Internet:
   ```
   ping -c 4 8.8.8.8
   ```

6. Verificare la risoluzione DNS:
   ```
   ping -c 4 google.com
   ```

7. Verificare la connettività con l'host:
   ```
   ping -c 4 <indirizzo_ip_host>
   ```
   Sostituire `<indirizzo_ip_host>` con l'indirizzo IP del sistema host

### Esercizio 1: Configurazione IP statico

In questo esercizio, configureremo un indirizzo IP statico sulla VM, utile in scenari dove è necessario un indirizzo fisso e prevedibile.

#### Attività
1. Identificare i parametri di rete necessari:
   - Indirizzo IP disponibile nella rete locale (non assegnato ad altri dispositivi)
   - Maschera di rete (solitamente 255.255.255.0 o /24)
   - Gateway predefinito (indirizzo IP del router)
   - Server DNS (puoi usare 8.8.8.8 e 8.8.4.4 di Google)

2. Modificare il file di configurazione dell'interfaccia:
   ```
   nano /etc/network/interfaces
   ```

3. Modificare la configurazione dell'interfaccia eth0 da DHCP a statica:
   ```
   auto eth0
   iface eth0 inet static
       address 192.168.1.100/24
       gateway 192.168.1.1
   ```
   > **Nota**: Sostituire 192.168.1.100 con un indirizzo IP disponibile nella tua rete e 192.168.1.1 con l'indirizzo del gateway della tua rete

4. Configurare i server DNS:
   ```
   nano /etc/resolv.conf
   ```
   Aggiungere o modificare le seguenti righe:
   ```
   nameserver 8.8.8.8
   nameserver 8.8.4.4
   ```

5. Riavviare il servizio di rete:
   ```
   rc-service networking restart
   ```

6. Verificare la nuova configurazione:
   ```
   ip addr show
   ip route show
   ping -c 4 8.8.8.8
   ping -c 4 google.com
   ```

#### Domande di riflessione
- Quali sono i vantaggi di utilizzare un indirizzo IP statico rispetto a uno dinamico?
- In quali scenari è preferibile utilizzare indirizzi IP statici?
- Quali problemi potrebbero sorgere se si assegna un indirizzo IP già in uso nella rete?
- Come si potrebbe verificare se un indirizzo IP è già in uso prima di assegnarlo?

### Esercizio 2: Esposizione di servizi sulla rete locale

In questo esercizio, configureremo un server web sulla VM e lo renderemo accessibile a tutti i dispositivi nella rete locale.

#### Attività
1. Installare un server web sulla VM:
   ```
   apk update
   apk add lighttpd
   rc-update add lighttpd default
   rc-service lighttpd start
   ```

2. Creare una semplice pagina web:
   ```
   echo "<html><body><h1>Alpine Linux Bridge Test</h1><p>Questa pagina è servita da una VM Alpine Linux in modalità Bridge</p><p>Indirizzo IP: $(hostname -I)</p></body></html>" > /var/www/localhost/htdocs/index.html
   ```

3. Verificare che il server web funzioni localmente:
   ```
   curl http://localhost
   ```

4. Accedere al server web da altri dispositivi nella rete locale:
   - Da un browser su un altro computer nella stessa rete, navigare a `http://<indirizzo_ip_vm>`
   - Sostituire `<indirizzo_ip_vm>` con l'indirizzo IP della VM Alpine Linux

5. Installare un server SSH per l'accesso remoto:
   ```
   apk add openssh
   rc-update add sshd default
   rc-service sshd start
   ```

6. Accedere alla VM tramite SSH da un altro computer nella rete:
   ```
   ssh root@<indirizzo_ip_vm>
   ```

#### Domande di riflessione
- Quali sono le implicazioni di sicurezza nell'esporre servizi direttamente sulla rete locale?
- Come si potrebbe limitare l'accesso ai servizi esposti?
- Quali altri servizi potrebbero essere utili esporre in una rete locale?
- Quali vantaggi offre la modalità Bridge rispetto alla modalità NAT per l'hosting di servizi?

### Esercizio 3: Comunicazione tra VM in modalità Bridge

In questo esercizio, creeremo una seconda VM in modalità Bridge e testeremo la comunicazione diretta tra le due VM.

#### Attività
1. Creare una seconda VM Alpine Linux con configurazione Bridge:
   - Clonare il template `Alpine-Template`
   - Chiamarla `Alpine-Bridge2`
   - Configurare l'adattatore di rete in modalità Bridge

2. Avviare entrambe le VM e verificare i loro indirizzi IP:
   ```
   # Su Alpine-Bridge
   ip addr show eth0
   
   # Su Alpine-Bridge2
   ip addr show eth0
   ```

3. Effettuare il ping tra le due VM:
   ```
   # Da Alpine-Bridge a Alpine-Bridge2
   ping -c 4 <indirizzo_ip_di_Alpine-Bridge2>
   
   # Da Alpine-Bridge2 a Alpine-Bridge
   ping -c 4 <indirizzo_ip_di_Alpine-Bridge>
   ```

4. Installare un server web sulla seconda VM:
   ```
   # Su Alpine-Bridge2
   apk update
   apk add lighttpd
   rc-update add lighttpd default
   rc-service lighttpd start
   echo "<html><body><h1>Alpine Linux Bridge2 Test</h1><p>Questa è la seconda VM in modalità Bridge</p></body></html>" > /var/www/localhost/htdocs/index.html
   ```

5. Accedere al server web della seconda VM dalla prima VM:
   ```
   # Su Alpine-Bridge
   apk add curl
   curl http://<indirizzo_ip_di_Alpine-Bridge2>
   ```

#### Domande di riflessione
- Quali sono i vantaggi della comunicazione diretta tra VM in modalità Bridge?
- Come si comportano le VM in modalità Bridge rispetto ai computer fisici nella rete?
- In quali scenari è utile avere più VM che comunicano direttamente tra loro?
- Quali considerazioni di sicurezza bisogna tenere presenti quando si hanno più VM esposte sulla rete?

### Vantaggi e limitazioni della modalità Bridge

#### Vantaggi
- Accesso diretto alla rete fisica
- La VM è visibile e accessibile da tutti i dispositivi nella rete
- Supporto completo per tutti i protocolli di rete
- Comunicazione diretta tra VM in modalità Bridge
- Ideale per server e servizi che devono essere accessibili nella rete locale

#### Limitazioni
- Richiede una connessione di rete fisica funzionante sull'host
- Può richiedere configurazioni aggiuntive in reti con restrizioni
- Maggiore esposizione della VM a potenziali minacce di rete
- Può causare conflitti di indirizzi IP se non configurata correttamente
- Non funziona in alcune reti wireless con isolamento client

## Scenario 3: Modalità Host-Only

### Introduzione alla modalità Host-Only

La modalità Host-Only è una configurazione di rete in VirtualBox che crea una rete privata isolata tra il sistema host e le macchine virtuali. A differenza della modalità NAT o Bridge, la modalità Host-Only non fornisce accesso diretto a Internet o alla rete esterna, ma crea un ambiente di rete isolato e controllato.

### Concetti chiave della modalità Host-Only

- **Rete isolata**: Crea una rete virtuale privata accessibile solo dall'host e dalle VM configurate con lo stesso adattatore Host-Only
- **Interfaccia virtuale**: VirtualBox crea un'interfaccia di rete virtuale sul sistema host (es. vboxnet0)
- **Server DHCP integrato**: Opzionalmente, può essere attivato un server DHCP per assegnare automaticamente indirizzi IP alle VM
- **Nessun accesso esterno**: Per impostazione predefinita, le VM in modalità Host-Only non possono accedere a Internet o alla rete esterna

### Creazione e configurazione dell'adattatore Host-Only

Prima di creare la VM, è necessario configurare l'adattatore di rete Host-Only in VirtualBox:

1. Aprire VirtualBox e selezionare "File" > "Strumenti" > "Gestore della rete host"
2. Nella finestra "Gestore della rete host", fare clic su "Crea" per creare un nuovo adattatore Host-Only
3. Verrà creato un nuovo adattatore (es. vboxnet0) con le impostazioni predefinite
4. Selezionare l'adattatore creato e fare clic su "Proprietà" per configurarlo:
   - **Scheda "Adattatore"**:
     - Indirizzo IPv4: 192.168.56.1 (questo sarà l'indirizzo dell'host nella rete Host-Only)
     - Maschera di rete IPv4: 255.255.255.0 (/24)
     - Lasciare disabilitato IPv6 per semplicità
   - **Scheda "Server DHCP"**:
     - Abilitare il server DHCP
     - Indirizzo server: 192.168.56.100
     - Maschera server: 255.255.255.0
     - Limite inferiore indirizzo: 192.168.56.101
     - Limite superiore indirizzo: 192.168.56.254
5. Fare clic su "Applica" e poi su "Chiudi"

### Creazione della VM Alpine Linux per Host-Only

1. Clonare il template `Alpine-Template` creato in precedenza:
   - Nome: `Alpine-HostOnly`
   - Selezionare "Clonazione completa"

2. Selezionare la VM appena creata e fare clic su "Impostazioni"
3. Nella sezione "Rete", modificare l'adattatore 1:
   - Abilitare la scheda di rete
   - Connessa a: "Scheda solo host"
   - Nome: selezionare l'adattatore Host-Only creato in precedenza (es. vboxnet0)
   - Modalità promiscua: "Consenti tutto"
   - Tipo di scheda: lasciare il valore predefinito
4. Fare clic su "OK" per salvare le impostazioni

### Verifica della configurazione di rete

1. Avviare la VM `Alpine-HostOnly`
2. Accedere come `root` con la password impostata durante l'installazione
3. Verificare la configurazione di rete con i seguenti comandi:
   ```
   ip addr show
   ```
   Dovresti vedere l'interfaccia `eth0` con un indirizzo IP nella rete 192.168.56.0/24 (assegnato dal server DHCP Host-Only)

4. Verificare la tabella di routing:
   ```
   ip route show
   ```
   Nota che non c'è un gateway predefinito, poiché la rete Host-Only è isolata

5. Verificare la connettività con l'host:
   ```
   ping -c 4 192.168.56.1
   ```
   Questo dovrebbe funzionare, poiché 192.168.56.1 è l'indirizzo dell'host nella rete Host-Only

6. Tentare di accedere a Internet:
   ```
   ping -c 4 8.8.8.8
   ```
   Questo dovrebbe fallire, poiché la rete Host-Only non fornisce accesso a Internet

### Esercizio 1: Configurazione IP statico nella rete Host-Only

In questo esercizio, configureremo un indirizzo IP statico sulla VM, utile in scenari dove è necessario un indirizzo fisso e prevedibile.

#### Attività
1. Identificare i parametri di rete necessari:
   - Indirizzo IP nella rete Host-Only (es. 192.168.56.10)
   - Maschera di rete (255.255.255.0 o /24)
   - Non è necessario un gateway predefinito per la comunicazione solo con l'host

2. Modificare il file di configurazione dell'interfaccia:
   ```
   nano /etc/network/interfaces
   ```

3. Modificare la configurazione dell'interfaccia eth0 da DHCP a statica:
   ```
   auto eth0
   iface eth0 inet static
       address 192.168.56.10/24
   ```

4. Riavviare il servizio di rete:
   ```
   rc-service networking restart
   ```

5. Verificare la nuova configurazione:
   ```
   ip addr show
   ip route show
   ping -c 4 192.168.56.1
   ```

#### Domande di riflessione
- Perché non è necessario configurare un gateway predefinito in una rete Host-Only?
- Quali vantaggi offre l'utilizzo di indirizzi IP statici in una rete Host-Only?
- Come influisce la mancanza di accesso a Internet sulle applicazioni che potrebbero essere eseguite sulla VM?
- In quali scenari reali è utile avere una rete isolata come quella Host-Only?

### Esercizio 2: Comunicazione tra host e VM

In questo esercizio, configureremo un server web sulla VM e lo renderemo accessibile dal sistema host.

#### Attività
1. Installare un server web sulla VM:
   ```
   apk update
   apk add lighttpd
   rc-update add lighttpd default
   rc-service lighttpd start
   ```

2. Creare una semplice pagina web:
   ```
   echo "<html><body><h1>Alpine Linux Host-Only Test</h1><p>Questa pagina è servita da una VM Alpine Linux in modalità Host-Only</p><p>Indirizzo IP: $(hostname -I)</p></body></html>" > /var/www/localhost/htdocs/index.html
   ```

3. Verificare che il server web funzioni localmente:
   ```
   apk add curl
   curl http://localhost
   ```

4. Accedere al server web dal sistema host:
   - Aprire un browser sul sistema host e navigare a `http://<indirizzo_ip_vm>`
   - Sostituire `<indirizzo_ip_vm>` con l'indirizzo IP della VM Alpine Linux (es. 192.168.56.10)

5. Installare un server SSH per l'accesso remoto:
   ```
   apk add openssh
   rc-update add sshd default
   rc-service sshd start
   ```

6. Accedere alla VM tramite SSH dal sistema host:
   ```
   ssh root@<indirizzo_ip_vm>
   ```

#### Domande di riflessione
- Quali sono i vantaggi di utilizzare una rete Host-Only per l'hosting di servizi di sviluppo?
- Come si potrebbe migliorare la sicurezza dei servizi esposti in una rete Host-Only?
- Quali altri servizi potrebbero essere utili in un ambiente di sviluppo isolato?
- Come si potrebbe simulare un ambiente di produzione completo utilizzando solo reti Host-Only?

### Esercizio 3: Comunicazione tra più VM in modalità Host-Only

In questo esercizio, creeremo una seconda VM in modalità Host-Only e testeremo la comunicazione diretta tra le due VM.

#### Attività
1. Creare una seconda VM Alpine Linux con configurazione Host-Only:
   - Clonare il template `Alpine-Template`
   - Chiamarla `Alpine-HostOnly2`
   - Configurare l'adattatore di rete in modalità Host-Only, selezionando lo stesso adattatore Host-Only (es. vboxnet0)

2. Avviare la VM e configurare un indirizzo IP statico:
   ```
   # Su Alpine-HostOnly2
   nano /etc/network/interfaces
   ```
   Modificare la configurazione:
   ```
   auto eth0
   iface eth0 inet static
       address 192.168.56.11/24
   ```
   Riavviare il servizio di rete:
   ```
   rc-service networking restart
   ```

3. Avviare entrambe le VM e verificare i loro indirizzi IP:
   ```
   # Su Alpine-HostOnly
   ip addr show eth0
   
   # Su Alpine-HostOnly2
   ip addr show eth0
   ```

4. Effettuare il ping tra le due VM:
   ```
   # Da Alpine-HostOnly a Alpine-HostOnly2
   ping -c 4 192.168.56.11
   
   # Da Alpine-HostOnly2 a Alpine-HostOnly
   ping -c 4 192.168.56.10
   ```

5. Installare un server web sulla seconda VM:
   ```
   # Su Alpine-HostOnly2
   apk update
   apk add lighttpd
   rc-update add lighttpd default
   rc-service lighttpd start
   echo "<html><body><h1>Alpine Linux HostOnly2 Test</h1><p>Questa è la seconda VM in modalità Host-Only</p></body></html>" > /var/www/localhost/htdocs/index.html
   ```

6. Accedere al server web della seconda VM dalla prima VM:
   ```
   # Su Alpine-HostOnly
   apk add curl
   curl http://192.168.56.11
   ```

#### Domande di riflessione
- Quali sono i vantaggi della comunicazione diretta tra VM in modalità Host-Only?
- Come si potrebbe simulare una rete aziendale completa utilizzando più VM in modalità Host-Only?
- Quali considerazioni di sicurezza bisogna tenere presenti quando si hanno più VM che comunicano tra loro?
- Come si potrebbe implementare una segmentazione di rete utilizzando più adattatori Host-Only?

### Vantaggi e limitazioni della modalità Host-Only

#### Vantaggi
- Crea un ambiente di rete isolato e controllato
- Comunicazione diretta tra host e VM
- Comunicazione diretta tra VM configurate con lo stesso adattatore Host-Only
- Maggiore sicurezza grazie all'isolamento dalla rete esterna
- Ideale per ambienti di test e sviluppo
- Possibilità di creare più reti Host-Only separate

#### Limitazioni
- Nessun accesso diretto a Internet o alla rete esterna
- Richiede configurazioni aggiuntive per consentire l'accesso a Internet (es. configurazione multi-adattatore)
- Limitato alla comunicazione con l'host e altre VM sulla stessa rete Host-Only
- Richiede la creazione e configurazione manuale dell'adattatore Host-Only
- Può causare confusione con più adattatori Host-Only

## Scenario 4: Modalità Internal Network

### Introduzione alla modalità Internal Network

La modalità Internal Network è una configurazione di rete in VirtualBox che crea una rete completamente isolata tra macchine virtuali, senza alcun accesso al sistema host o alla rete esterna. Questa modalità è progettata per creare ambienti di rete virtuali completamente isolati e sicuri, ideali per simulare reti private o testare configurazioni di rete in un ambiente controllato.

### Concetti chiave della modalità Internal Network

- **Isolamento completo**: Crea una rete virtuale accessibile solo dalle VM configurate con lo stesso nome di rete interna
- **Nessun accesso all'host**: A differenza della modalità Host-Only, l'host non ha accesso alla rete interna
- **Nessun server DHCP integrato**: Per impostazione predefinita, non c'è un server DHCP, richiedendo configurazione manuale degli indirizzi IP
- **Comunicazione diretta**: Le VM nella stessa rete interna possono comunicare direttamente tra loro
- **Sicurezza elevata**: L'isolamento completo garantisce che il traffico di rete rimanga confinato all'interno della rete virtuale

### Creazione delle VM Alpine Linux per Internal Network

#### Prima VM
1. Clonare il template `Alpine-Template` creato in precedenza:
   - Nome: `Alpine-Internal1`
   - Selezionare "Clonazione completa"

2. Selezionare la VM appena creata e fare clic su "Impostazioni"
3. Nella sezione "Rete", modificare l'adattatore 1:
   - Abilitare la scheda di rete
   - Connessa a: "Rete interna"
   - Nome: `intnet1` (questo nome identifica la rete interna)
   - Modalità promiscua: "Consenti tutto"
   - Tipo di scheda: lasciare il valore predefinito
4. Fare clic su "OK" per salvare le impostazioni

#### Seconda VM
1. Clonare il template `Alpine-Template` creato in precedenza:
   - Nome: `Alpine-Internal2`
   - Selezionare "Clonazione completa"

2. Selezionare la VM appena creata e fare clic su "Impostazioni"
3. Nella sezione "Rete", modificare l'adattatore 1:
   - Abilitare la scheda di rete
   - Connessa a: "Rete interna"
   - Nome: `intnet1` (lo stesso nome della prima VM)
   - Modalità promiscua: "Consenti tutto"
   - Tipo di scheda: lasciare il valore predefinito
4. Fare clic su "OK" per salvare le impostazioni

### Configurazione manuale della rete

#### Prima VM
1. Avviare la VM `Alpine-Internal1`
2. Accedere come `root` con la password impostata durante l'installazione
3. Configurare manualmente l'indirizzo IP:
   ```
   nano /etc/network/interfaces
   ```
   Modificare la configurazione dell'interfaccia eth0:
   ```
   auto eth0
   iface eth0 inet static
       address 10.0.0.1/24
   ```

4. Riavviare il servizio di rete:
   ```
   rc-service networking restart
   ```

5. Verificare la configurazione:
   ```
   ip addr show
   ip route show
   ```

#### Seconda VM
1. Avviare la VM `Alpine-Internal2`
2. Accedere come `root` con la password impostata durante l'installazione
3. Configurare manualmente l'indirizzo IP:
   ```
   nano /etc/network/interfaces
   ```
   Modificare la configurazione dell'interfaccia eth0:
   ```
   auto eth0
   iface eth0 inet static
       address 10.0.0.2/24
   ```

4. Riavviare il servizio di rete:
   ```
   rc-service networking restart
   ```

5. Verificare la configurazione:
   ```
   ip addr show
   ip route show
   ```

### Verifica della comunicazione tra le VM

1. Dalla prima VM, effettuare il ping alla seconda VM:
   ```
   # Su Alpine-Internal1
   ping -c 4 10.0.0.2
   ```

2. Dalla seconda VM, effettuare il ping alla prima VM:
   ```
   # Su Alpine-Internal2
   ping -c 4 10.0.0.1
   ```

3. Tentare di effettuare il ping all'host o a Internet:
   ```
   # Su entrambe le VM
   ping -c 4 8.8.8.8
   ```
   Questo dovrebbe fallire, dimostrando l'isolamento completo della rete interna

### Esercizio 1: Configurazione di base dei servizi di rete

In questo esercizio, configureremo servizi di base per consentire la comunicazione tra le VM nella rete interna.

#### Attività
1. Sulla prima VM (`Alpine-Internal1`), installare e configurare un server SSH:
   ```
   apk update
   apk add openssh
   rc-update add sshd default
   rc-service sshd start
   ```

2. Sulla seconda VM (`Alpine-Internal2`), installare il client SSH:
   ```
   apk update
   apk add openssh-client
   ```

3. Testare la connessione SSH dalla seconda VM alla prima:
   ```
   # Da Alpine-Internal2
   ssh root@10.0.0.1
   ```
   Accettare la chiave host e inserire la password di root della prima VM

4. Sulla prima VM, creare un file di test:
   ```
   echo "Questo è un file di test nella rete interna" > /root/test.txt
   ```

5. Dalla seconda VM, utilizzare SCP per copiare il file:
   ```
   # Da Alpine-Internal2
   scp root@10.0.0.1:/root/test.txt /root/
   cat /root/test.txt
   ```

#### Domande di riflessione
- Quali sono i vantaggi di utilizzare una rete completamente isolata per i test?
- Come influisce l'assenza di un server DHCP sulla configurazione della rete?
- Quali problemi di sicurezza potrebbero sorgere in una rete interna e come affrontarli?
- In quali scenari reali è utile avere una rete completamente isolata come quella interna?

### Esercizio 2: Implementazione di un server DHCP nella rete interna

In questo esercizio, configureremo un server DHCP sulla prima VM per automatizzare l'assegnazione degli indirizzi IP nella rete interna.

#### Attività
1. Sulla prima VM (`Alpine-Internal1`), installare il server DHCP:
   ```
   apk add dhcp
   ```

2. Configurare il server DHCP:
   ```
   nano /etc/dhcp/dhcpd.conf
   ```
   Inserire la seguente configurazione:
   ```
   # Configurazione di base
   option domain-name-servers 10.0.0.1;
   default-lease-time 600;
   max-lease-time 7200;
   authoritative;
   
   # Configurazione della rete interna
   subnet 10.0.0.0 netmask 255.255.255.0 {
     range 10.0.0.100 10.0.0.200;
     option routers 10.0.0.1;
     option broadcast-address 10.0.0.255;
   }
   ```

3. Configurare l'interfaccia di rete per il server DHCP:
   ```
   nano /etc/conf.d/dhcpd
   ```
   Modificare la riga DHCPD_IFACE:
   ```
   DHCPD_IFACE="eth0"
   ```

4. Avviare il server DHCP e configurarlo per l'avvio automatico:
   ```
   rc-update add dhcpd default
   rc-service dhcpd start
   ```

5. Sulla seconda VM (`Alpine-Internal2`), modificare la configurazione di rete per utilizzare DHCP:
   ```
   nano /etc/network/interfaces
   ```
   Modificare la configurazione dell'interfaccia eth0:
   ```
   auto eth0
   iface eth0 inet dhcp
   ```

6. Riavviare il servizio di rete sulla seconda VM:
   ```
   rc-service networking restart
   ```

7. Verificare che la seconda VM abbia ottenuto un indirizzo IP dal server DHCP:
   ```
   ip addr show
   ```
   Dovresti vedere un indirizzo IP nel range 10.0.0.100-10.0.0.200

8. Sulla prima VM, verificare i lease DHCP:
   ```
   cat /var/lib/dhcp/dhcpd.leases
   ```

#### Domande di riflessione
- Quali sono i vantaggi di utilizzare un server DHCP in una rete interna?
- Come si potrebbe estendere la configurazione DHCP per fornire più informazioni ai client?
- Quali problemi potrebbero sorgere con l'utilizzo di DHCP in una rete di produzione?
- Come si potrebbe implementare la ridondanza del server DHCP in una rete reale?

### Esercizio 3: Creazione di una topologia di rete complessa

In questo esercizio, creeremo una topologia di rete più complessa utilizzando più reti interne e una VM che funge da router tra di esse.

#### Attività
1. Creare una terza VM (`Alpine-Router`) con due adattatori di rete:
   - Adattatore 1: Rete interna `intnet1` (connesso alla rete esistente)
   - Adattatore 2: Rete interna `intnet2` (una nuova rete interna)

2. Creare una quarta VM (`Alpine-Internal3`) con un adattatore di rete:
   - Adattatore 1: Rete interna `intnet2`

3. Configurare la VM `Alpine-Router`:
   - Configurare l'interfaccia eth0 con indirizzo IP 10.0.0.254/24 (nella prima rete interna)
   - Configurare l'interfaccia eth1 con indirizzo IP 10.0.1.1/24 (nella seconda rete interna)
   - Abilitare l'inoltro dei pacchetti:
     ```
     echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
     sysctl -p
     ```

4. Configurare la VM `Alpine-Internal3`:
   - Configurare l'interfaccia eth0 con indirizzo IP 10.0.1.2/24
   - Impostare il gateway predefinito a 10.0.1.1 (l'indirizzo del router nella seconda rete)

5. Sulla VM `Alpine-Internal1` (10.0.0.1), aggiungere una route per raggiungere la seconda rete interna:
   ```
   ip route add 10.0.1.0/24 via 10.0.0.254
   ```
   Per rendere permanente questa route:
   ```
   echo "10.0.1.0/24 via 10.0.0.254" >> /etc/network/routes
   ```

6. Sulla VM `Alpine-Internal2`, aggiungere la stessa route:
   ```
   ip route add 10.0.1.0/24 via 10.0.0.254
   ```

7. Verificare la connettività tra tutte le VM:
   ```
   # Da Alpine-Internal1 (10.0.0.1)
   ping -c 4 10.0.0.2  # Alpine-Internal2
   ping -c 4 10.0.0.254  # Alpine-Router (eth0)
   ping -c 4 10.0.1.1  # Alpine-Router (eth1)
   ping -c 4 10.0.1.2  # Alpine-Internal3
   
   # Da Alpine-Internal3 (10.0.1.2)
   ping -c 4 10.0.1.1  # Alpine-Router (eth1)
   ping -c 4 10.0.0.254  # Alpine-Router (eth0)
   ping -c 4 10.0.0.1  # Alpine-Internal1
   ping -c 4 10.0.0.2  # Alpine-Internal2
   ```

#### Domande di riflessione
- Come si potrebbe estendere questa topologia per simulare una rete aziendale più complessa?
- Quali sono i vantaggi di segmentare una rete in più sottoreti?
- Come si potrebbe implementare un firewall tra le due reti interne?
- Quali considerazioni di sicurezza bisogna tenere presenti quando si configurano router tra reti?

### Vantaggi e limitazioni della modalità Internal Network

#### Vantaggi
- Isolamento completo dalla rete esterna e dal sistema host
- Massima sicurezza per test e simulazioni
- Comunicazione diretta tra VM configurate con la stessa rete interna
- Possibilità di creare topologie di rete complesse con più reti interne
- Ideale per laboratori virtuali e ambienti di test

#### Limitazioni
- Nessun accesso diretto a Internet o alla rete esterna
- Nessun accesso dal sistema host alla rete interna
- Richiede configurazione manuale degli indirizzi IP (a meno che non si configuri un server DHCP)
- Necessità di configurare manualmente servizi come DNS e routing
- Richiede più VM per simulare scenari di rete complessi

## Troubleshooting delle Reti in VirtualBox

### Strumenti di Diagnostica di Rete in Alpine Linux

Prima di affrontare problemi specifici, è importante conoscere gli strumenti di diagnostica disponibili in Alpine Linux:

#### Strumenti di Base

1. **ip** - Strumento principale per la configurazione e visualizzazione delle interfacce di rete
   ```
   # Visualizzare le interfacce di rete
   ip addr show
   
   # Visualizzare la tabella di routing
   ip route show
   
   # Visualizzare le statistiche delle interfacce
   ip -s link show
   ```

2. **ping** - Verifica la connettività di base
   ```
   # Verificare la connettività con un altro host
   ping -c 4 192.168.1.1
   ```

3. **traceroute** - Mostra il percorso dei pacchetti verso una destinazione
   ```
   apk add iputils
   traceroute 8.8.8.8
   ```

4. **netstat** - Visualizza le connessioni di rete, le tabelle di routing e le statistiche delle interfacce
   ```
   apk add net-tools
   netstat -tuln  # Mostra le porte in ascolto
   netstat -r     # Mostra la tabella di routing
   ```

#### Strumenti Avanzati

1. **tcpdump** - Analizzatore di pacchetti di rete
   ```
   apk add tcpdump
   tcpdump -i eth0 -n  # Cattura il traffico sull'interfaccia eth0
   ```

2. **nslookup/dig** - Strumenti per la risoluzione DNS
   ```
   apk add bind-tools
   nslookup google.com
   dig google.com
   ```

3. **iptables** - Gestione del firewall
   ```
   apk add iptables
   iptables -L  # Elenca le regole del firewall
   ```

### Problemi Comuni e Soluzioni

#### Problemi di Connettività di Base

##### Sintomi:
- Impossibilità di comunicare con altri host
- Errori "Network is unreachable" o "No route to host"
- Timeout nei ping

##### Diagnosi:
1. Verificare che l'interfaccia di rete sia attiva:
   ```
   ip link show eth0
   ```
   L'output dovrebbe mostrare `state UP`

2. Verificare che l'interfaccia abbia un indirizzo IP:
   ```
   ip addr show eth0
   ```

3. Verificare la tabella di routing:
   ```
   ip route show
   ```

##### Soluzioni:
1. Attivare l'interfaccia se è down:
   ```
   ip link set eth0 up
   ```

2. Configurare un indirizzo IP se mancante:
   ```
   # Per configurazione temporanea
   ip addr add 192.168.1.10/24 dev eth0
   
   # Per configurazione permanente, modificare /etc/network/interfaces
   ```

3. Aggiungere una route se necessario:
   ```
   ip route add default via 192.168.1.1
   ```

4. Riavviare il servizio di rete:
   ```
   rc-service networking restart
   ```

#### Problemi di Risoluzione DNS

##### Sintomi:
- Impossibilità di risolvere nomi di dominio
- Errori "Unknown host" o "Temporary failure in name resolution"
- Ping a indirizzi IP funziona ma ping a nomi di dominio fallisce

##### Diagnosi:
1. Verificare la configurazione DNS:
   ```
   cat /etc/resolv.conf
   ```

2. Testare la risoluzione DNS:
   ```
   nslookup google.com
   ```

##### Soluzioni:
1. Configurare correttamente i server DNS:
   ```
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
   echo "nameserver 8.8.4.4" >> /etc/resolv.conf
   ```

2. Verificare la connettività con i server DNS:
   ```
   ping -c 4 8.8.8.8
   ```

#### Problemi Specifici per Modalità di Rete

##### Modalità NAT
- **Port Forwarding non funziona**: Verificare la configurazione in VirtualBox, controllare che il servizio sia in esecuzione sulla VM, verificare che non ci siano firewall che bloccano la porta sull'host
- **VM non accede a Internet**: Verificare che l'host abbia accesso a Internet, riavviare la VM, ripristinare le impostazioni di rete predefinite

##### Modalità Bridge
- **VM non ottiene un indirizzo IP**: Verificare che il server DHCP nella rete fisica sia attivo, configurare manualmente un indirizzo IP statico, verificare che non ci siano restrizioni MAC nella rete
- **VM non accessibile dalla rete locale**: Verificare che la VM abbia un indirizzo IP nella stessa subnet degli altri dispositivi, controllare il firewall sulla VM

##### Modalità Host-Only
- **Adattatore Host-Only non visibile**: Verificare che sia stato creato almeno un adattatore Host-Only nel Gestore della rete host, riavviare VirtualBox dopo la creazione dell'adattatore
- **VM non comunica con l'host**: Verificare gli indirizzi IP dell'host e della VM, controllare che entrambi siano nella stessa subnet

##### Modalità Internal Network
- **VM non comunicano tra loro**: Verificare che tutte le VM siano configurate per utilizzare la stessa rete interna (stesso nome), controllare gli indirizzi IP e le subnet mask
- **Routing tra reti interne diverse non funziona**: Verificare che l'inoltro dei pacchetti sia abilitato sul router, controllare le tabelle di routing su tutte le VM

### Tecniche di Prevenzione dei Problemi

1. **Documentazione della Configurazione**: Mantenere una documentazione dettagliata della configurazione di rete (diagramma della topologia, tabella degli indirizzi IP, configurazione di ciascuna VM)
2. **Backup delle Configurazioni**: Creare backup regolari delle configurazioni di rete
3. **Configurazione Incrementale**: Iniziare con una configurazione minima e verificare che funzioni, aggiungere una funzionalità alla volta e testare dopo ogni modifica
4. **Test Regolari**: Implementare test regolari per verificare la funzionalità della rete

## Conclusioni e Riflessioni Finali

### Riepilogo delle Modalità di Rete

| Modalità | Accesso a Internet | Accesso dall'Host | Accesso da Altri Dispositivi | Comunicazione tra VM | Uso Tipico |
|----------|-------------------|------------------|----------------------------|---------------------|-----------|
| NAT | Sì | Solo con Port Forwarding | Solo con Port Forwarding | No (diretto) | Navigazione web, download |
| Bridge | Sì | Sì | Sì | Sì | Server, servizi di rete |
| Host-Only | No (diretto) | Sì | No | Sì (stessa rete) | Ambienti di test, sviluppo |
| Internal Network | No | No | No | Sì (stessa rete) | Laboratori isolati, test di sicurezza |

### Scelta della Modalità Appropriata

La scelta della modalità di rete dipende dalle esigenze specifiche:

- **NAT**: Quando si desidera semplicemente accedere a Internet dalla VM senza esporre servizi
- **Bridge**: Quando la VM deve essere accessibile come un computer fisico nella rete
- **Host-Only**: Quando si desidera un ambiente isolato ma con accesso dall'host
- **Internal Network**: Quando si desidera un ambiente completamente isolato per test o simulazioni

### Considerazioni sulla Sicurezza

- Le VM in modalità Bridge sono esposte alle stesse minacce di sicurezza dei computer fisici nella rete
- Le modalità NAT e Host-Only offrono un livello aggiuntivo di protezione grazie all'isolamento
- La modalità Internal Network offre il massimo livello di isolamento e sicurezza
- Indipendentemente dalla modalità, è sempre consigliabile implementare misure di sicurezza appropriate (firewall, aggiornamenti, ecc.)

### Spunti per Ulteriori Esercitazioni

1. **Configurazione Multi-Adattatore**: Combinare diverse modalità di rete su una singola VM
2. **Implementazione di un Firewall**: Configurare iptables per proteggere le VM
3. **Simulazione di una DMZ**: Utilizzare più reti interne per creare una zona demilitarizzata
4. **Load Balancing**: Configurare un load balancer tra più VM server
5. **VPN**: Implementare una connessione VPN tra reti virtuali isolate

## Risorse Aggiuntive

- [Documentazione ufficiale di VirtualBox](https://www.virtualbox.org/manual/UserManual.html)
- [Wiki di Alpine Linux](https://wiki.alpinelinux.org/wiki/Main_Page)
- [Guida alla rete in Linux](https://wiki.archlinux.org/title/Network_configuration)
- [Guida al troubleshooting di rete in Linux](https://www.linuxjournal.com/content/linux-network-troubleshooting)
