Le varie versioni di Linux sono disponibili [qua]([https://distrowatch.com/](https://distrowatch.com/ "https://distrowatch.com/")).
# Comandi
Concatenare comandi
```Linux
<comando_1> && <comando_2>
```
Eseguire un comando nel caso quello precedente non venga eseguito correttamente 
```Linux
<comando_1> || <comando_2> 
```
Inserire un commento  
```Linux
# commento 
```
Accedere ad una rete come cliente
```shell
<nome_macchina>-cli -h localhost
```
Filtro un comando in base ad una determinata stringa
```shell
<comando> | grep <stringa>
```
Utilizzare il [linguaggio AWK](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/) per generare dei #report
```sh
awk
```
Ripulire lo schermo
```sh
clear
```
Modifica i permessi granulari di un file. #SUID
```sh
chmod <nome_file>
```
	chmod u-x, o-r <nome_file>
	u: proprietario
	g: gruppo
	o: altri utenti (other)
	aggiungere un permesso: +
	rimuovere un permesso: -
	r: read - 4
	w: write - 2
	x: execute - 1
	s: SUID, ovvero Set owner User ID. Permette di far eseguire il file ad un utente (indipendentemente dai permessi assegnati) come fosse root
	Nel caso si vogliano utilizzare i numeri, nel caso di combinazione di permessi, si possono sommare i numeri (6 per lettura e scrittura, 7 per tutti e 3)
Modifica il proprietario di un file
```sh
chown <nome_utente>:<nome_gruppo> <nome_file>
```
Controllare lo spazio disponibile nelle varie partizioni
```shell
df -hT
```
Controllare lo spazio disponibile nelle varie partizioni
```shell
find <directory>
```
	find / -perm -4000
	-perm #cerca i file che hanno i permessi specificati
	permessi: 1=read, 2=write, 3=execute, 4=SUID
Rimuovere un account
```Shell
gpasswd -d <nome_utente> <nome_ambiente>
```
Visualizzare i gruppi a cui appartiene un utente
```shell
groups <utente>
```
	Se non si inserisce l'utente, ritornerà tutti i gruppi del sistema
Visualizzare le regole di accesso alla macchina ( #firewall)
```shell
iptables -L -v -n
```
	iptables -P INPUT DROP # cancella tutte le configurazioni  
Visualizzare tutti i file e cartelle all'interno della directory corrente
```shell
ls
```
	-a # mostra anche i file nascosti
	-l # mostra i dati in tabella
	tabella: directory_1 directory_2 directory_3|permessi|utente|gruppo utente|dimensione|data di creazione|nome del file 
	categorie utenti: proprietario, gruppo del proprietario, altri
	directory può essere "d" se directory o "-"
	permessi: r=read, w=write, x=execute, -=nessun perm
Controllare lo spazio utilizzato dalle varie partizioni
```shell
lsblk
```
Verificare se ci sono porte aperte verso l'esterno in una macchina #port-scanning
```bash
nmap <nome_macchina>
```
Installare npm
```Bash
npm install
```
Eseguire un file con [[Node.js]]
```Bash
node <nome file>.js 
```
Aprire una #shell in [[Php]]
```Shell
php
```
	OPZIONI:
	-a # Apre la shell in maniera interattiva
Vedere l'albero dei processi
```shell
ps aux
```
Stampo il percorso della cartella in cui mi trovo
```shell
pwd
```
Apro un terminale [[Python]]
```shell
python
```
	Il file ha permessi -rwsr-xr-x
Interagire con un database remoto
```shell
redis-sli -h <nome_server>
```
Rimuovere uno o più elementi
```sh
rm <nome_file>
```
	OPZIONI:
	-rf # Forza la rimozione
Connettersi ad un'altra macchina da remoto tramite protocollo #ssh 
```sh
ssh <indirizzo_ip> -i <chiave>
```
Accedere come utente root (chiede la password)
```shell
su
```
Effettuare un comando come #superuser (SUperuser DO). Effettuabile solo se l'utente fa parte dei **sudores**
```Shell
sudo <comando>
```
	OPZIONI:
	-aG <gruppop> <user> # Aggiunge l'utente al gruppo-
Esegue le funzionalità #sysctl
```shell
sysctl
```
	-p # riavvia il SO
Aggiungo un nuovo utente (richiede permessi root)
```sh
useradd <nome_utente>
```
	-c #
	-d # assegna una cartella specifica
	-e # inserisce una data di "scadenza" all'utente
	-p # 
	useradd <utente> -p # inserisce l'utente su /etc/passwd
Verificare l'utente con cui si sta operando
```Shell
whoami
```
Comandi da inserire
```shell
chroot
getent
env
sh #shell
man <comando> # Ritorna i manuale del comando selezionato
```
## Opzioni
```Bash
-A # append
-aG #
-d # detach
-e # edit
-it # interattivo
-l # mostra una versione tabellare del risultato, includendo permessi, proprietario, gruppo del proprietario, ultima modifica e dimensione se utilizzato con ls
-L
-n
-p # protocollo di trasporto
-s # indirizzo ip
-v # version
--dport <numero> # porta di destinazione 
--help # lista dei comandi disponibili
```
	Permessi: 
## Info utili
- Premendo Tab, il terminale completa automaticamente il comando (o il file). Nel caso ci siano più risultati possibili, ritorna sotto la linea di comando la lista delle possibilità
# UNIX

```unix
top # mostra i processi attivi nella macchina
htop # come top, ma con una barra orizzontale per l'utilizzo della CPU
```
## [kali-linux](https://www.kali.org/get-kali/#kali-installer-images)
E' una versione di #linux molto utilizzata per #penetration-test e #forensic. La distribuzione contiene oltre 600 tool (soprattutto da riga di comando), alcune delle quali utili per effettuare attacchi informatici. L'utente di base si chiama **kali** e i comandi sono in base #unix (utilizzato, ad esempio, anche in #git-bash) .
```sh
DEFAULT SETTINGS
user: kali
password: kali
```
### Struttura del terminale
```bash
|- (<utente>@<macchina>) -[<locallizzazione>]
|- $
```
	Nel caso ci si trovi nella cartella principale (di default /home/kali), la localizzazione sarà ~
	Nel caso ci si trovi nella cartella root, la localizzazione sarà /
### Struttura
```root
|-0
|-bin #contiene file e applicazioni binari
|-boot
|-etc # contiene file di configurazione del sistema
|--sysctl.conf
|--host.allow # non presente di default
|--host.deny # non presente di default
|--ssh
|---sshd_config # gestisce i permessi per effettuare chiamate in #ssh
|--passwd
|-home #contiene tutti gli utenti locali
|-dev #contiene i file necessari per far funzionare i dispositivi
|-lib #contiene i file di sistema
|-media #contiene i supporti esterni
```
#### etc
	##### ssh/sshd_config
Gestire gli accessi al gruppo #root
```sshd_config
# PermitRootLogin = 0 # permettere l'accesso all'utente root

# MaxAuthhTries 6 # numero massimo di tentativi di accesso

# PermitRootLogin prohibit-password # gestire l'accesso all'utente root

# ClientAliveInterval 0 # intervallo di inattività di un utente loggato prima del logout automatico

# ClientAliveCountMax 3 # numero massimo di utenti collegati cotemporaneamente alla macchina

```
##### sysctl.conf
Il file ha tutti i parametri commentati. Nel caso si vogliano attivare alcuni comandi, basta rimuovere il \# ed impostare i parametri con il valore desiderato.
Connettersi alla rete pubblica dal dispositivo
```systctl.conf
# Uncomment the next line to enable packet forwarding for IPv4
# net.ipv4.ip_forward=1
```
	1 se si vuole utilizzare la macchina come firewall o come kernel
	0 se lo si vuole impostare come non attivo
Accettare messaggi con protocollo #IPCM, scoprendo le tabelle di routing della macchina
```systcl.conf
# Do not accept IPCM redirects (prevent MITM attacks)
# net.ipv4.conf.all.accept_redirects = 0
# net.ipv4.conf.default.accept_redirects = 0
```
source_route è un parametro che indica il percorso per raggiungere il dispositivo (può essere utilizzato per bypassare il #firewall)
```systctl.conf
# net.ipv4.conf.default.accept_source_route = 0
```
	0 - non accetta eventuali pacchetti che includono il parametro source_route
	1 - accetta pacchetti ch einclusono il parametro source_route

Permette di attivare i syncookies
```systctl.conf
# net.ipv4.tcp_syncookies = 1
```
Permette di indicare il numero di volte per le quali deve essere ripetuta la richiesta di risposta per l'handshake
```systctl.conf
# net.ipv4.tcp_sysnack_retries = 3
```
Permette di impostare i tasti per attivare le combinazioni [sysrq](https://linuxaria.com/howto/sysrq-linux-ubuntu?lang=it), ovvero comandi che permettono di effettuare delle operazioni predeterminate nel caso crashi il sistema 
```systctl.conf
# kernel.sysrq = 438
```
	kernel.sysrq = 0 => si disabilita la funzionalità
	kernel.sysrq = 1 => si abilitano tutte le funzionalità
	b - Riavvia il sistema
	o - Spegne il sistema
	s - Cerca di fare il sync di tutti i filesystem montati
	...

##### passwd
```passwd
root:0:0:
<nome_utente>:a:b:c:<nome>,,,:<percorso>:<percorso>
```
	a: permessi_root (0 si, 1 no) #rivedere
### Comandi extra
```sh
netstat -rn # fornisce #gateway
cd /usr/share/wordlists # cartella in cui è presente il file Rockyou.txt
cd ~ # Riporta nella cartella iniziale
gzip -d <nome file> # unzippa il file
tail <nome file> stampa le ultime righe del file
```

### WPScan
E' un tool nativo di #kali #linux per operare con siti #wordpress
```Kali
wpscan --url <url> # fornisce informazioni relative al sito, ad l'autore del sito
```
```Kali
wpscan --url <url> --password-attack xmlrpc -t 20 -U <nome utente> -P /usr/share/wordlists/rockyou.txt #effettua un #bruteforce sulla password dell'account selezionato
```
### Hydra
Software utilizzato per effettuare #brute-force su siti web.
## Hardening
### #DDos 
Ci sono dei software che permettono di inviare dei [Three-way-handshake](<Protocolli di comunicazione#Three-way-handshake) incompleti ad una macchina bersaglio. La macchina risponderà con il secondo passaggio del processo. Nel caso le richieste vengano effettuate in grande quantità, si effettua un attacco chiamato #syn-flow ed il risultato è il down del sistema e la conseguente interruzione del servizio.
#### Soluzione
Impostare il parametro net.ipv4.tcp_syncookies = 1 nel file [[#sysctl.conf]]
### #privilege-escalation
#### Accesso a file sensibili per leggere dati di accesso
Durante la creazione di server in Linux (così come tutti gli altri #OS), si deve porre particolare attenzione alla creazione e gestione degli utenti.
Particolarmente sensibile è il file **/etc/passwd**, poiché contiene, ad esempio, i dati di tutti gli utenti che hanno accesso alla macchina.
```sh
cat /etc/passwd |awk -F: '($3 = 0) {print $1}'
```
	Visualizzare i nomi di tutti gli utenti con permessi root
#### Tramite Python
Apro il terminale di Python
```sh
python
```
Importo e accedo ad un nuovo #OS 
```Python
import os
os.execl("/bin/bash", "bash", "-p")
```
Nel terminale sono nella shell. Nel terminale sono l'utente **root**
```sh
whoami
```
	Risultato: root
##### Soluzione
Il terminale Python ha il #SUID preimpostato. Quindi, lo devo quindi rimuovere.
```sh
chmod u-s /usr/bin/python3.11
```
	La versione di python installata può essere vista tramite python --version
In questo modo l'utente potrà accedere al terminale Python, ma con i permessi prestabiliti dall'amministratore.
### [Code scanning](Cybersecurity#SCA)

# Annotazioni
- Le distribuzioni di Linux, di solito, vengono provviste nativamente di [[Python]]
