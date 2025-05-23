# Esercitazione Pratica di Laboratorio - Sessione Introduttiva 2
## Reti, Applicazioni e Servizi

### Panoramica dell'Esercitazione
Questa esercitazione pratica di laboratorio è progettata per accompagnare la Sessione Introduttiva 2 su Reti, Applicazioni e Servizi. Gli studenti lavoreranno sia in ambiente Windows 11 (con account senza privilegi di amministratore) sia in ambiente Linux Alpine tramite VirtualBox, per mettere in pratica i concetti teorici appresi durante la sessione.

### Prerequisiti
- Computer con Windows 11 (account senza privilegi di amministratore)
- VirtualBox con Alpine Linux configurato (dalla precedente esercitazione)
- Conoscenze acquisite nella Sessione Introduttiva 1 e relativa esercitazione

### Durata
3 ore di laboratorio

### Obiettivi di Apprendimento
- Comprendere i concetti fondamentali di reti informatiche
- Utilizzare strumenti di diagnostica di rete in Windows e Linux
- Esplorare il modello client-server attraverso esempi pratici
- Analizzare applicazioni e servizi in esecuzione
- Sperimentare con la virtualizzazione come ponte verso i container

## Parte 1: Concetti Fondamentali di Reti (45 minuti)

### 1.1 Analisi della Configurazione di Rete in Windows
**Obiettivo**: Esplorare e comprendere la configurazione di rete in Windows.

**Attività**:
1. Aprire PowerShell e visualizzare la configurazione di rete:
   ```powershell
   Get-NetIPConfiguration
   ```

2. Ottenere informazioni dettagliate sull'adattatore di rete:
   ```powershell
   Get-NetAdapter | Format-List Name, Status, LinkSpeed, MacAddress
   ```

3. Visualizzare la tabella di routing:
   ```powershell
   Get-NetRoute
   ```

4. Verificare le connessioni attive:
   ```powershell
   Get-NetTCPConnection | Where-Object State -eq 'Established' | Format-Table -Property LocalAddress, LocalPort, RemoteAddress, RemotePort, State
   ```

5. Esaminare la configurazione DNS:
   ```powershell
   Get-DnsClientServerAddress
   ```

6. Creare un file di testo con il riepilogo delle informazioni di rete:
   ```powershell
   $netInfo = "Configurazione di Rete Windows`n"
   $netInfo += "============================`n`n"
   $netInfo += "Indirizzo IP: " + (Get-NetIPAddress | Where-Object AddressFamily -eq 'IPv4' | Where-Object InterfaceAlias -notlike '*Loopback*' | Select-Object -ExpandProperty IPAddress) + "`n"
   $netInfo += "Subnet Mask: " + (Get-NetIPAddress | Where-Object AddressFamily -eq 'IPv4' | Where-Object InterfaceAlias -notlike '*Loopback*' | Select-Object -ExpandProperty PrefixLength) + "`n"
   $netInfo += "Gateway: " + (Get-NetRoute | Where-Object DestinationPrefix -eq '0.0.0.0/0' | Select-Object -ExpandProperty NextHop) + "`n"
   $netInfo += "Server DNS: " + (Get-DnsClientServerAddress | Where-Object InterfaceAlias -notlike '*Loopback*' | Where-Object AddressFamily -eq 2 | Select-Object -ExpandProperty ServerAddresses) + "`n"
   $netInfo | Out-File -FilePath "$env:USERPROFILE\LabDocker\network_windows.txt"
   ```

### 1.2 Analisi della Configurazione di Rete in Alpine Linux
**Obiettivo**: Esplorare e comprendere la configurazione di rete in Linux.

**Attività**:
1. Accedere alla VM Alpine Linux come utente "studente"

2. Visualizzare la configurazione di rete:
   ```bash
   ip addr show
   ```

3. Visualizzare la tabella di routing:
   ```bash
   ip route show
   ```

4. Verificare le connessioni attive:
   ```bash
   ss -tuln
   ```

5. Esaminare la configurazione DNS:
   ```bash
   cat /etc/resolv.conf
   ```

6. Creare un file di testo con il riepilogo delle informazioni di rete:
   ```bash
   echo "Configurazione di Rete Linux" > ~/LabDocker/network_linux.txt
   echo "===========================" >> ~/LabDocker/network_linux.txt
   echo "" >> ~/LabDocker/network_linux.txt
   echo "Indirizzo IP: $(ip -4 addr show | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | grep -v '127.0.0.1')" >> ~/LabDocker/network_linux.txt
   echo "Subnet Mask: $(ip -4 addr show | grep -oP '(?<=inet\s)\d+(\.\d+){3}/\d+' | grep -v '127.0.0.1' | cut -d'/' -f2)" >> ~/LabDocker/network_linux.txt
   echo "Gateway: $(ip route | grep default | awk '{print $3}')" >> ~/LabDocker/network_linux.txt
   echo "Server DNS: $(grep nameserver /etc/resolv.conf | awk '{print $2}')" >> ~/LabDocker/network_linux.txt
   ```

### 1.3 Confronto tra Configurazioni di Rete
**Obiettivo**: Confrontare le configurazioni di rete tra Windows e Linux.

**Attività**:
1. Creare una tabella di confronto:

   | Informazione | Windows | Linux |
   |--------------|---------|-------|
   | Comando per visualizzare IP | `Get-NetIPAddress` | `ip addr show` |
   | Comando per visualizzare routing | `Get-NetRoute` | `ip route show` |
   | Comando per visualizzare connessioni | `Get-NetTCPConnection` | `ss -tuln` |
   | File configurazione DNS | Registro di sistema | `/etc/resolv.conf` |
   | Interfaccia di loopback | 127.0.0.1 | 127.0.0.1 |

2. Discutere le differenze e le similitudini:
   - Entrambi i sistemi utilizzano IPv4/IPv6
   - I comandi sono diversi ma i concetti sono gli stessi
   - Linux tende a utilizzare file di configurazione, Windows il registro di sistema
   - Entrambi i sistemi utilizzano lo stesso indirizzo di loopback (127.0.0.1)

## Parte 2: Strumenti di Diagnostica di Rete (45 minuti)

### 2.1 Strumenti di Diagnostica in Windows
**Obiettivo**: Utilizzare gli strumenti di diagnostica di rete disponibili in Windows.

**Attività**:
1. Testare la connettività con ping:
   ```powershell
   ping www.google.com
   ping -n 5 8.8.8.8
   ```

2. Tracciare il percorso di rete:
   ```powershell
   tracert www.google.com
   ```

3. Risolvere nomi DNS:
   ```powershell
   Resolve-DnsName www.google.com
   nslookup www.google.com
   ```

4. Verificare le porte aperte:
   ```powershell
   Test-NetConnection www.google.com -Port 80
   Test-NetConnection www.google.com -Port 443
   ```

5. Analizzare le statistiche di rete:
   ```powershell
   netstat -ano
   ```

6. Creare un file di report con i risultati:
   ```powershell
   $report = "Report Diagnostica di Rete Windows`n"
   $report += "================================`n`n"
   $report += "Ping a Google:`n"
   $report += (ping -n 3 www.google.com | Out-String)
   $report += "`nRisoluzione DNS di Google:`n"
   $report += (Resolve-DnsName www.google.com | Format-Table -Property Name, Type, TTL, IPAddress | Out-String)
   $report += "`nConnessione alla porta 80 di Google:`n"
   $report += (Test-NetConnection www.google.com -Port 80 | Out-String)
   $report | Out-File -FilePath "$env:USERPROFILE\LabDocker\network_diagnostics_windows.txt"
   ```

### 2.2 Strumenti di Diagnostica in Alpine Linux
**Obiettivo**: Utilizzare gli strumenti di diagnostica di rete disponibili in Linux.

**Attività**:
1. Installare gli strumenti necessari:
   ```bash
   sudo apk update
   sudo apk add iputils bind-tools traceroute
   ```

2. Testare la connettività con ping:
   ```bash
   ping -c 4 www.google.com
   ping -c 4 8.8.8.8
   ```

3. Tracciare il percorso di rete:
   ```bash
   traceroute www.google.com
   ```

4. Risolvere nomi DNS:
   ```bash
   dig www.google.com
   nslookup www.google.com
   ```

5. Verificare le porte aperte:
   ```bash
   nc -zv www.google.com 80
   nc -zv www.google.com 443
   ```

6. Analizzare le statistiche di rete:
   ```bash
   netstat -tuln
   ```

7. Creare un file di report con i risultati:
   ```bash
   echo "Report Diagnostica di Rete Linux" > ~/LabDocker/network_diagnostics_linux.txt
   echo "===============================" >> ~/LabDocker/network_diagnostics_linux.txt
   echo "" >> ~/LabDocker/network_diagnostics_linux.txt
   echo "Ping a Google:" >> ~/LabDocker/network_diagnostics_linux.txt
   ping -c 3 www.google.com >> ~/LabDocker/network_diagnostics_linux.txt
   echo "" >> ~/LabDocker/network_diagnostics_linux.txt
   echo "Risoluzione DNS di Google:" >> ~/LabDocker/network_diagnostics_linux.txt
   dig +short www.google.com >> ~/LabDocker/network_diagnostics_linux.txt
   echo "" >> ~/LabDocker/network_diagnostics_linux.txt
   echo "Connessione alla porta 80 di Google:" >> ~/LabDocker/network_diagnostics_linux.txt
   nc -zv www.google.com 80 2>&1 >> ~/LabDocker/network_diagnostics_linux.txt
   ```

### 2.3 Confronto tra Strumenti di Diagnostica
**Obiettivo**: Confrontare gli strumenti di diagnostica di rete tra Windows e Linux.

**Attività**:
1. Creare una tabella di confronto:

   | Funzione | Windows | Linux |
   |----------|---------|-------|
   | Ping | `ping` | `ping` |
   | Traceroute | `tracert` | `traceroute` |
   | Risoluzione DNS | `nslookup`, `Resolve-DnsName` | `nslookup`, `dig` |
   | Test porte | `Test-NetConnection` | `nc` (netcat) |
   | Statistiche di rete | `netstat` | `netstat`, `ss` |

2. Discutere le differenze e le similitudini:
   - Molti comandi hanno nomi simili o identici
   - Le opzioni e i parametri possono variare
   - Linux offre strumenti più specializzati (dig vs nslookup)
   - PowerShell offre output strutturato (oggetti vs testo)

## Parte 3: Modello Client-Server (45 minuti)

### 3.1 Implementazione di un Server Web Semplice
**Obiettivo**: Creare e testare un semplice server web per comprendere il modello client-server.

**Attività**:
1. In Alpine Linux, installare Python:
   ```bash
   sudo apk add python3
   ```

2. Creare una directory per il server web:
   ```bash
   mkdir -p ~/LabDocker/webserver
   cd ~/LabDocker/webserver
   ```

3. Creare un file HTML semplice:
   ```bash
   echo '<!DOCTYPE html>
   <html>
   <head>
       <title>Server Web di Test</title>
   </head>
   <body>
       <h1>Benvenuto nel Server Web di Test</h1>
       <p>Questo è un semplice server web creato per comprendere il modello client-server.</p>
       <p>Data e ora del server: <span id="datetime"></span></p>
       <script>
           document.getElementById("datetime").textContent = new Date().toLocaleString();
       </script>
   </body>
   </html>' > index.html
   ```

4. Avviare un server web Python:
   ```bash
   python3 -m http.server 8080
   ```

5. Prendere nota dell'indirizzo IP della VM Alpine:
   ```bash
   ip addr show | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | grep -v '127.0.0.1'
   ```

### 3.2 Accesso al Server Web da Windows
**Obiettivo**: Utilizzare Windows come client per accedere al server web in Alpine Linux.

**Attività**:
1. In Windows, aprire PowerShell e testare la connessione al server:
   ```powershell
   Test-NetConnection -ComputerName [IP_ALPINE_VM] -Port 8080
   ```

2. Utilizzare un browser web per accedere al server:
   - Aprire Microsoft Edge o un altro browser
   - Navigare a `http://[IP_ALPINE_VM]:8080`

3. Utilizzare PowerShell come client HTTP:
   ```powershell
   Invoke-WebRequest -Uri "http://[IP_ALPINE_VM]:8080" -Method GET | Select-Object -ExpandProperty Content > "$env:USERPROFILE\LabDocker\webserver_response.html"
   ```

4. Analizzare la risposta HTTP:
   ```powershell
   $response = Invoke-WebRequest -Uri "http://[IP_ALPINE_VM]:8080" -Method GET
   $response.Headers | Out-File -FilePath "$env:USERPROFILE\LabDocker\http_headers.txt"
   ```

### 3.3 Analisi del Traffico Client-Server
**Obiettivo**: Comprendere il flusso di comunicazione tra client e server.

**Attività**:
1. In Alpine Linux, installare tcpdump:
   ```bash
   sudo apk add tcpdump
   ```

2. Avviare la cattura del traffico:
   ```bash
   sudo tcpdump -i any port 8080 -w ~/LabDocker/capture.pcap
   ```

3. Da Windows, effettuare alcune richieste al server web

4. Interrompere la cattura con Ctrl+C

5. Analizzare la cattura:
   ```bash
   sudo tcpdump -r ~/LabDocker/capture.pcap -A | head -50 > ~/LabDocker/packet_analysis.txt
   ```

6. Discutere il modello client-server:
   - Il server è in ascolto su una porta specifica
   - Il client inizia la comunicazione
   - Il server risponde alle richieste del client
   - La comunicazione segue un protocollo (HTTP)
   - I pacchetti contengono intestazioni e dati

## Parte 4: Applicazioni e Servizi (45 minuti)

### 4.1 Analisi dei Processi in Windows
**Obiettivo**: Esplorare i processi e i servizi in esecuzione in Windows.

**Attività**:
1. Visualizzare i processi in esecuzione:
   ```powershell
   Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 20 | Format-Table -Property Id, ProcessName, CPU, WorkingSet
   ```

2. Visualizzare i servizi:
   ```powershell
   Get-Service | Where-Object Status -eq 'Running' | Sort-Object -Property DisplayName | Format-Table -Property Name, DisplayName, Status
   ```

3. Esaminare le dipendenze di un servizio:
   ```powershell
   Get-Service -Name "BITS" | Format-List *
   ```

4. Visualizzare le applicazioni in ascolto sulle porte:
   ```powershell
   Get-NetTCPConnection -State Listen | Select-Object -Property LocalAddress, LocalPort, @{Name="Process";Expression={(Get-Process -Id $_.OwningProcess).ProcessName}} | Sort-Object -Property LocalPort
   ```

5. Creare un report sui processi e servizi:
   ```powershell
   $processReport = "Report Processi e Servizi Windows`n"
   $processReport += "================================`n`n"
   $processReport += "Top 10 Processi per Utilizzo CPU:`n"
   $processReport += (Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 10 | Format-Table -Property Id, ProcessName, CPU, WorkingSet | Out-String)
   $processReport += "`nServizi in Esecuzione (primi 10):`n"
   $processReport += (Get-Service | Where-Object Status -eq 'Running' | Sort-Object -Property DisplayName | Select-Object -First 10 | Format-Table -Property Name, DisplayName, Status | Out-String)
   $processReport | Out-File -FilePath "$env:USERPROFILE\LabDocker\process_report_windows.txt"
   ```

### 4.2 Analisi dei Processi in Alpine Linux
**Obiettivo**: Esplorare i processi e i servizi in esecuzione in Linux.

**Attività**:
1. Installare gli strumenti necessari:
   ```bash
   sudo apk add procps htop
   ```

2. Visualizzare i processi in esecuzione:
   ```bash
   ps aux | sort -nrk 3,3 | head -20
   ```

3. Utilizzare htop per una visualizzazione interattiva:
   ```bash
   htop
   ```
   (Premere Q per uscire)

4. Visualizzare i servizi gestiti da init:
   ```bash
   rc-status
   ```

5. Visualizzare le applicazioni in ascolto sulle porte:
   ```bash
   sudo netstat -tulpn
   ```

6. Creare un report sui processi e servizi:
   ```bash
   echo "Report Processi e Servizi Linux" > ~/LabDocker/process_report_linux.txt
   echo "=============================" >> ~/LabDocker/process_report_linux.txt
   echo "" >> ~/LabDocker/process_report_linux.txt
   echo "Top 10 Processi per Utilizzo CPU:" >> ~/LabDocker/process_report_linux.txt
   ps aux | sort -nrk 3,3 | head -10 >> ~/LabDocker/process_report_linux.txt
   echo "" >> ~/LabDocker/process_report_linux.txt
   echo "Servizi in Esecuzione:" >> ~/LabDocker/process_report_linux.txt
   rc-status >> ~/LabDocker/process_report_linux.txt
   echo "" >> ~/LabDocker/process_report_linux.txt
   echo "Applicazioni in Ascolto:" >> ~/LabDocker/process_report_linux.txt
   sudo netstat -tulpn >> ~/LabDocker/process_report_linux.txt
   ```

### 4.3 Confronto tra Gestione dei Processi
**Obiettivo**: Confrontare la gestione dei processi e servizi tra Windows e Linux.

**Attività**:
1. Creare una tabella di confronto:

   | Funzione | Windows | Linux |
   |----------|---------|-------|
   | Visualizzare processi | `Get-Process` | `ps`, `top`, `htop` |
   | Visualizzare servizi | `Get-Service` | `rc-status`, `systemctl` |
   | Terminare un processo | `Stop-Process` | `kill`, `pkill` |
   | Avviare un servizio | `Start-Service` | `rc-service`, `systemctl` |
   | Porte in ascolto | `Get-NetTCPConnection` | `netstat`, `ss` |

2. Discutere le differenze e le similitudini:
   - Windows utilizza un modello di servizi centralizzato (SCM)
   - Linux utilizza vari sistemi di init (SysV, systemd, OpenRC)
   - Entrambi i sistemi separano processi utente e di sistema
   - Linux offre più strumenti di monitoraggio a riga di comando
   - Windows offre più strumenti grafici

## Parte 5: Introduzione alla Virtualizzazione (45 minuti)

### 5.1 Analisi della VM Alpine Linux
**Obiettivo**: Comprendere i concetti di virtualizzazione attraverso l'analisi della VM Alpine Linux.

**Attività**:
1. In Windows, esaminare le impostazioni della VM in VirtualBox:
   - Aprire VirtualBox
   - Selezionare la VM Alpine Linux
   - Esaminare le impostazioni (CPU, RAM, storage, rete)

2. Comprendere il concetto di hypervisor:
   - VirtualBox è un hypervisor di tipo 2 (ospitato)
   - Esegue VM su un sistema operativo host
   - Emula hardware per il sistema operativo guest

3. In Alpine Linux, esaminare le risorse virtuali:
   ```bash
   # CPU virtuale
   cat /proc/cpuinfo | grep "model name" | uniq
   
   # Memoria virtuale
   free -h
   
   # Storage virtuale
   df -h
   
   # Interfacce di rete virtuali
   ip link show
   ```

4. Creare un report sulle risorse virtuali:
   ```bash
   echo "Report Risorse Virtuali Alpine Linux" > ~/LabDocker/vm_resources.txt
   echo "==================================" >> ~/LabDocker/vm_resources.txt
   echo "" >> ~/LabDocker/vm_resources.txt
   echo "CPU:" >> ~/LabDocker/vm_resources.txt
   cat /proc/cpuinfo | grep "model name" | uniq >> ~/LabDocker/vm_resources.txt
   echo "Numero di core: $(grep -c processor /proc/cpuinfo)" >> ~/LabDocker/vm_resources.txt
   echo "" >> ~/LabDocker/vm_resources.txt
   echo "Memoria:" >> ~/LabDocker/vm_resources.txt
   free -h >> ~/LabDocker/vm_resources.txt
   echo "" >> ~/LabDocker/vm_resources.txt
   echo "Storage:" >> ~/LabDocker/vm_resources.txt
   df -h >> ~/LabDocker/vm_resources.txt
   echo "" >> ~/LabDocker/vm_resources.txt
   echo "Rete:" >> ~/LabDocker/vm_resources.txt
   ip link show >> ~/LabDocker/vm_resources.txt
   ```

### 5.2 Da VM a Container: Un'Anteprima
**Obiettivo**: Comprendere le differenze tra VM e container e prepararsi per il corso Docker.

**Attività**:
1. Discutere le differenze tra VM e container:
   - VM: virtualizzazione hardware completa
   - Container: virtualizzazione a livello di sistema operativo
   - VM: kernel separato per ogni VM
   - Container: kernel condiviso con l'host

2. Creare un diagramma di confronto:
   ```
   +---------------------------+    +---------------------------+
   | Applicazione A | App. B   |    | Applicazione A | App. B   |
   +---------------------------+    +---------------------------+
   | Bin/Libs A     | Bin/Libs B|    | Bin/Libs A     | Bin/Libs B|
   +---------------------------+    +---------------------------+
   | Guest OS A     | Guest OS B|    | Container Engine         |
   +---------------------------+    +---------------------------+
   |       Hypervisor          |    |         Host OS           |
   +---------------------------+    +---------------------------+
   |         Host OS           |    |         Hardware          |
   +---------------------------+    +---------------------------+
   |         Hardware          |
   +---------------------------+
   
          Macchine Virtuali                  Container
   ```

3. Preparare l'ambiente per Docker:
   - In Alpine Linux, verificare i requisiti per Docker:
     ```bash
     uname -a
     cat /proc/sys/kernel/osrelease
     ```

   - Creare una directory per i futuri esercizi Docker:
     ```bash
     mkdir -p ~/LabDocker/docker-exercises
     ```

### 5.3 Esercizio Finale: Preparazione per Docker
**Obiettivo**: Creare un ambiente di test che simuli alcuni aspetti dei container.

**Attività**:
1. In Alpine Linux, creare un ambiente chroot semplice:
   ```bash
   # Creare directory per il chroot
   sudo mkdir -p /home/studente/LabDocker/chroot-demo
   
   # Installare busybox
   sudo apk add busybox
   
   # Copiare busybox nel chroot
   sudo cp -a /bin/busybox /home/studente/LabDocker/chroot-demo/
   
   # Creare directory essenziali
   sudo mkdir -p /home/studente/LabDocker/chroot-demo/{bin,lib,proc}
   
   # Creare link simbolici per i comandi busybox
   cd /home/studente/LabDocker/chroot-demo/bin
   sudo ln -s ../busybox sh
   sudo ln -s ../busybox ls
   sudo ln -s ../busybox cat
   sudo ln -s ../busybox echo
   
   # Creare un file di test
   echo "Hello from chroot environment" | sudo tee /home/studente/LabDocker/chroot-demo/hello.txt
   ```

2. Entrare nell'ambiente chroot:
   ```bash
   sudo chroot /home/studente/LabDocker/chroot-demo /bin/sh
   ```

3. Esplorare l'ambiente chroot:
   ```bash
   # All'interno del chroot
   ls
   cat hello.txt
   echo "I'm in a container-like environment"
   ```

4. Uscire dall'ambiente chroot:
   ```bash
   exit
   ```

5. Discutere le similitudini con i container:
   - Isolamento del filesystem
   - Ambiente di esecuzione limitato
   - Condivisione del kernel con l'host
   - Differenze rispetto ai container Docker (mancanza di isolamento di rete, processi, ecc.)

## Conclusione e Verifica dell'Apprendimento

### Riepilogo
- Abbiamo esplorato i concetti fondamentali di reti informatiche
- Abbiamo utilizzato strumenti di diagnostica di rete in Windows e Linux
- Abbiamo implementato un semplice server web per comprendere il modello client-server
- Abbiamo analizzato processi e servizi in entrambi gli ambienti
- Abbiamo esplorato i concetti di virtualizzazione e fatto un'anteprima dei container

### Verifica dell'Apprendimento
1. Quali sono le principali differenze tra gli strumenti di diagnostica di rete in Windows e Linux?
2. Come funziona il modello client-server? Fate un esempio pratico.
3. Quali sono le differenze tra processi e servizi?
4. Quali sono le principali differenze tra macchine virtuali e container?
5. Come può un ambiente chroot simulare alcuni aspetti dei container?

### Preparazione per il Corso Docker
- Mantenere attiva la VM Alpine Linux
- Conservare i report e i file creati durante l'esercitazione
- Rivedere i concetti di virtualizzazione e container
- Assicurarsi che la VM soddisfi i requisiti per l'installazione di Docker

## Risorse Aggiuntive
- [Documentazione PowerShell per la rete](https://docs.microsoft.com/en-us/powershell/module/nettcpip/)
- [Guida ai comandi di rete Linux](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-linux-networking-commands-you-should-know)
- [Introduzione a HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [Gestione dei processi in Linux](https://www.linuxjournal.com/content/managing-processes-linux)
- [Introduzione ai container](https://www.docker.com/resources/what-container/)
