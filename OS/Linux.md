Linux è un sistema operativo Open Source, scritto in [C](../Linguaggi/C) basato su Unix, disponibile in varie versioni (chiamate Distro). La lista e il download delle varie versioni delle distro di Linux è disponibile [qua]([https://distrowatch.com/](https://distrowatch.com/ "https://distrowatch.com/")).

Linux è un sistema basato su file, organizzati tramite sistema a [cartelle](#File), ovvero file utilizzati per raccogliere altri file al loro interno.

Linux gestisce la memoria tramite la suddivisione in blocchi di dimensione uguale tra loro:
- Il [kernel](#Kernel) è il blocco con privilegi più alti ad è isolato dagli altri blocchi.
- Ogni utente ha degli appositi indirizzi di memoria, che sono accessibili agli altri utenti e alle applicazioni. Può comunicare con il kernel unicamente tramite apposite chiamate API di sistema che fungono da intermediario.

# Kernel

# Shell
## Comandi
I comandi sono stringhe a cui è collegato un determinato file che esegue operazioni. Essi possono avere **opzioni** per specificare il tipo di comportamento richiesto e un **argomento** per specificare l'oggetto su cui effettuare l'operazione

### Lista comandi

#### Introduzione
Inserire un commento  
```Linux
# commento 
```

Prevenire l'interpretazione di uno specifico carattere (di solito speciale)
```sh
\<carattere>
```
	ESEMPIO:
	echo \$PATH # risultato: $PATH
	echo $PATH # ritultato: /usr/local/sbin/...

Inserire un testo
```sh
"<testo>"
```
	Le virgolette doppie permettono di utilizzare caratteri speciali, ma di leggere le variabili
	ALTERNATIVE:
	'<testo>' # Non permettono di utlizzare le variabili
	`<testo>` # Permettono di utilizzare un comando all'interno di un altro comando
		ESEMPIO: echo Today is `date`

Utilizzare un comando all'interno di un altro comando
```sh
<comando> <inizio argomento> $(<secondo comando>) <fine argomento>
```
	NOTE:
	- Questa operazione è realizzazbile anche tramite backtick (``)

Accedere ad una rete come cliente
```shell
<nome_macchina>-cli -h localhost
```

Filtro un comando in base ad una determinata stringa
```shell
<comando> | grep <stringa>
```

Utilizzare un comando presente nello storico della bash
```sh
!<numero nello storico>
```
	NOTE:
		Nel caso voglia utilizzare l'ultimo comando inviato, posso anche scrivere:
			!!
	
	ES: Nel caso ho inviato precedetnemente i comandi:
		1. cd /
		2. ls -a
		3. cd home
	Posso chiedere una lista degli elementi presenti in cartella anche tramite
		!2

Visualizzare l'ultima interazione con un comando specifico (mostra sia il comando inserito che il risultato ottenuto)
```sh
!<comando>
```

Creare una funzione
```sh
<nome_funzione> () {
	<comando 1>
	<comando 2>
}
```

##### jobs
Eseguire il processo in background e mostrare la linea di comando (job)
```sh
<comando> &
```

#### alias
Riassumere più comandi in uno solo (creare un alias)
```sh
alias <nome_alias>='<comando> <opzioni> <argomento>'
```
	ESEMPIO:
	alias la='ls -a' # una volta utilizzato la, il sistema utilizzerà il comando impostato

#### apropos
Fornisce una breve lista dei contenuti delle pagine del manuale del comando
```sh
apropos <comando>
```
	NOTE:
	- Da gli stessi risultati di man -k

#### apt-get
Modificare i software installati all’interno della macchina
```Sh
apt-get
```
	OPZIONI: 
	install <nome app> # installa il software
	upgrade # aggiorna tutti i software installati
	dist-upgrade # aggiorna la distro
	
	N.B. Il comando necessita dei permessi di root.

#### arch
Visualizzare l'architettura della [CPU](../Tecnologia/Macchina#CPU)
```sh
arch
```

#### arp
Visualizzare la [tabella ARP](./Tecnologie/Protocolli#ARP) del dispositivo
```sh
arp
```

#### awk
Utilizzare il [linguaggio AWK](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/) per generare dei #report
```sh
awk
```

#### bash
Eseguire uno [shell script](#Script) tramite bash
```sh
bash <file>
```

#### bg
Spostare un processo in fondo alla cosa dei jobs
```sh
bg %<posizione processo>
```

#### bunzip
Decomprimere un file tramite algoritmo Burrows-Wheeler
```sh
bunzip2 <file>
```

#### bzip
Comprimere un file con algoritmo Burrows-Wheeler (file più piccolo, ma con utilizzo maggiore di CPU, estensione .bz)
```sh
bzip2 <file>
```

#### cat
Leggere il contenuto di un file
```sh
cat <nome_file>
```

#### chage
Modificare le informazioni relative l'invecchiamento (aging) delle password, memorizzato su [/etc/shadow](#etc#shadow)
```sh
chage <utente>
```
	OPZIONI:
	-d <valore> # modifica il valore di LAST_DAY
	-l # fornisce informazioni sull'aging dell'account
	-m <valore> # modifica il valore di MIN_DAYS
	-E <valore> # modifica il valore di EXPIRE_DATE
	-I <valore> # modifica il valore di INACTIVE (come trattare l'account dopo la scadenza della password)
	-M <valore> # modifica il valore di MAX_DAYS
	-W <valore> # modific ail valore di WARN_DAYS
	

#### chgrp
Modifica il gruppo proprietario dell'elemento
```sh
chgrp <nome_gruppo> <nome_file>
```
	OPZIONI:
	-R # modifica tutti i file selezionati in maniera ricorsiva

#### chmod
Modifica i permessi granulari di un file. Tramite questo comando è possibile impostare valori di [suid](#suid), [sgid](#sgid) e [sticky bit](<#sticky bit>) per permettere l'accesso o impedire la modifica di file ad utenti che di norma non avrebbero i permessi necessari. Il comando è utilizzabile dal **creatore** del file e da **root**
```sh
chmod <permessi_utente><permessi_gruppo><permessi_utente_generico> <nome_file>
```
	ESEMPI:
		chmod u-x, o-r <nome_file>
	NOTE:
	- Nel caso si vogliano utilizzare i numeri, nel caso di combinazione di permessi, si possono sommare i numeri (6 per lettura e scrittura, 7 per tutti e 3)
	- Se le cartelle non hanno almeno permesso 4, verrà negato l'accesso
	s: ovvero Set owner (user o group). Permette di far eseguire il file ad un utente (indipendentemente dai permessi assegnati) come fosse root
	t: (Sticky bit) Imposta gli utenti proprietari come unici utenti con permessi di cancellazione file (a parte il super user)

E' possibile definire i permessi tramite [valore numerico](#Permessi) o tramite l'utilizzo dei seguenti simboli:

| Simbolo | Descrizione           |
| ------- | --------------------- |
| u       | Proprietario          |
| g       | Gruppo                |
| o       | Altri utenti (others) |
| +       | Aggiungi permesso     |
| -       | Rimuovi permesso      |
| =       | Uguale                |
| r       | Lettura               |
| w       | Scrittura             |
| x       | Esecuzione            |

I simboli possono essere utilizzati per semplificare la modifica dei permessi. Alcuni esempi sono:
- chmod g+w abc.txt
- chmod u=r+x,o-r abc.txt

#### chown
Modifica il proprietario di un file (eseguibile solo con permessi di root)
```sh
chown <nome_utente> <nome_file>
```

Ci sono 3 modi per utilizzare il comando chown:
- ```chown <nome> <file>``` modifica l'utente proprietario
- ```chown <utente>:<gruppo> <file>``` modifica sia utente che gruppo proprietari del file
- ```chown .<gruppo> <file>``` modifica il gruppo proprietario del file

#### clear
Ripulire lo schermo
```sh
clear
```

#### cp
Copiare un file
```sh
cp <file> <percorso in cui copiare>
```
	OPZIONI:
	-i # aggiunge interattività al comando (ad esempio chiede se si voglia sostituire un eventuale file preesistente avente lo stesso nome)
	-n # no-overwrite, ovvero impedisce la sovrascrittura di un eventuale file preesistente
	-p # mantiene le proprietà iniziali del file
	-r # esegue l'azione in maniera ricorsiva per le cartelle
	-v # verbose (stampa il processo su console)
	-R # esegue l'opzione in maniera ricorsiva

#### cut
Estrare determinate righe da un file
```sh
cut <file>
```
	OPZIONI:
	-d # delimitatore
	-f<numero> # sezioni da copiare\
	
	NOTE:
	- le sezioni indicate tramite opzione "-f" possono essere separate tra loro tramite virgola, o tramite range, utilizzando il trattino
		-f1, 5-7 # prende la prima e le sezioni dalla 5 alla 7

#### dd
Copia un dato da un input ad un output 
```Shell
dd
````

#### dig
Cercare un Fully Qualified Domain Name nel server DNS di default (può essere anche la [cache](./Macchina#Cache) del dispositivo stesso) tramite [DNS](./Protocolli#DNS)
```sh
dig <sito web (opzionale)>
```
	OPZIONI:
	-x # 
	
	N.B. Se non viene inserito nessun indirizzo, la shell entrerà in modelità lookup, dove potrai effettuare le ricerche inserendo il nome del sito desiderato (inserisci il comando exit per terminare il processo).

#### df
Controllare lo spazio disponibile nelle varie partizioni del volume
```shell
df -hT
```

#### dmesg
Visualizza i log presenti su [/var/log/dmesg](#dmesg).
```sh
dmesg
```

**N.B.**: Il messaggio stampato ha una grandezza massima di 512 KB ,quindi spesso il comando viene utilizzato assieme al [grep](#grep) per filtrarne i contenuti.

#### echo
Stampare un testo. Il comando può essere utilizzato con le [ridirezioni](#Ridirezioni)
```sh
echo <argomento>
```
	OPZIONI:
	-e # abilita l'utilizzo dei caratteri di fuga che usano "\" (es. \n per andare a capo)
	-n # non andare a capo dopo aver ritornato il valore
	-E # disabilita l'utilizzo dei caratteri di fuga che usano "\" (utilizzato di default)
	
	N.B.
	- Legge le directory (es. echo D* ritorna tutti i file presenti su D)
	- Non legge altri comandi (es. echo cal ritorna la scritta cal)
	- echo $? stampa il risultato di uscita dell'ultima operazione (0 per operazione eeseguita con succeso, 1 per errore)

#### env
Ottenere una lista della variabili d'ambiente
```sh
env
```

#### eval
Fare robe
```sh
eval "<comando>"
```
	OPZIONI:
	- Avviare un ssh-agent: eval "$(ssh-agent)"

#### export
Salvare la variabile di ambiente sul file **env**
```sh
export $<nome variabile>
```
	NOTA: E' possibile comprimere i comandi, dichiarando la variabile direttamente dopo export:
		export <NOME_VARIABILE>=<valore>

#### fallocate
Preservare una porzione di spazio della #memoria ad un file. [Link](https://man7.org/linux/man-pages/man1/fallocate.1.html)
```sh
fallocate <directory_file>
```
	ES. sudo fallocate -l 4G /swapfile
	OPZIONI:
	-d # dig holes, ovvero frammenta lo spazio preservato
	-l <dimensione> # es. KiB, MiB, GiB, PiB
	-n <nome_file>

#### fdisk
Visualizza e gestisce i [dischi di memoria](../Tecnologie/Macchina#Memoria) di tipo [MBR](../Tecnologie/Macchina#MBR) ([guida all'utilizzo](https://learning.lpi.org/it/learning-materials/101-500/104/104.1/104.1_01/))
```sh
fdisk <disco>
```
	OPZIONI:
	-b <dimensione> # cambia la dimensione della partizione in uso del disco (512, 1024, 2048, 4096)
	-c # Modalità compatibliità
	-l # mostra la lista dei dispositivi collegati al computer
	-u # mostra le unitàç cilindri o settori
	-v # stampa la versione del programma
	-C <numero> # indica il numero di cilindri
	-H <numero> # indica il numero di teste
	-S <numero> # indica il numero di settori per traccia

#### fg
Spostare un processo a primo della lista dei [jobs](#jobs) (foreground)
```sh
fg <processo>
```
	N.B. Il simbolo "%" permette di 

#### file
Verificare il tipo di file. Alcuni file, come [/ver/log/wtmp] e [/var/log/stmp], hanno dei comandi dedicati per la visualizzazione (rispettivamente [last](#last) e [lastb](#lastb))
```sh
file <file>
```
	ESEMPIO:
	file /var/log/wtmp
		RISULTATO:
		/var/log/wtmp: data

#### find
Controllare lo spazio disponibile nelle varie partizioni
```shell
find <directory> <nome file>
```
	OPZIONI:
	-name <valore> # identifica solo i file con il nome indicato
	-nogroup # trova tutti i file non collegati ad alcun GID nella cartella
	
	ESEMPIO:
	find / -perm -4000
	-perm #cerca i file che hanno i permessi specificati
	permessi: 1=read, 2=write, 3=execute, 4=SUID

#### free
Controllare lo spazio disponibile nella [RAM](../Tecnologia/Macchina#RAM)
```sh
free
```
	OPZIONI:
	-g # arrotonda il risultato al GB più vicino
	-m # arrotonda l'output al MB più vicino
	-s <secondi> # modifica la frequenza dell'aggiornamento automatico

Un esempio di ritulato è la tabella eguente:

|                    | total    | used     | free     | shared | buffers | cached   |
| ------------------ | -------- | -------- | -------- | ------ | ------- | -------- |
| Mem:               | 32953528 | 26171772 | 6781756  | 0      | 4136    | 22660364 |
| -/+ buffers/cache: |          | 3507272  | 29446256 |        |         |          |
| Swap:              | 0        | 0        | 0        |        |         |          |
- -/+ buffers/cache: rappresenta la quantità di memoria fisica utilizzata dal kernel per buffers o cache (che quindi potrebbe essere liberata)
- [Swap](<#Swap file system>)

#### gdisk
Permette di gestire partizioni per dischi [GPT](../Tecnologie/Macchina#GPT), in modo equivalente al comando [fdisk](#fdisk).

#### gfdisk

#### getent
Ottenere informazioni riguardo un dato
```sh
getent <database> <dato>
```
	- database: si riferisce al file (di /env) nel quale il dato è salvato
	- dato: ad es. un nome utente
	
	ESEMPIO:
		getent passwd sysadmin
	NOTE:
	- Il comando da risultati simili a "grep <utente> /etc/passwd"

#### getfacl
Visualizzare le [ACL](#ACL) di un file
```sh
getfacl <nome_file>
```

#### gparted
Creare o gestire le [partizioni](../Tecnologie/Macchina#Partizione) della memoria tramite [gparted](#gparted)
```sh
gparted
```

#### gpasswd
Rimuovere un account
```Shell
gpasswd -d <nome_utente> <nome_ambiente>
```

#### grep
Filtrare il risultato di un [output](#Output) tramite comparazione con un'espressione in [RegEx](../Linguaggi/RegEx) delimitata da virgolette singole (' ')
```sh
grep <espressione> <files>
```
	OPZIONI:
	-c # conta quante linee corrispondono alla ricerca
	-i # rende i controlli non case-sensitive (non fanno differenza tra caratteri in minuscolo e maiuscolo)
	-n # riporta il numero della riga trovata
	-v # ritorna tutti i risultati che non corrispondano alla ricerca (inverso)
	-w # ritorna solo le corrispondenze di intere parole (e non nel caso di corrispondenza all'interno di una parte di parola)
	-E # permette di utilizzare i caratteri speciali all'interno dell'espressione
	--color # evidenzia la sezione che combacia con il valore utilizzato

#### groupadd
Creo un nuovo gruppo
```sh
groupadd <nome_gruppo*>
```
	OPZIONI:
	-g <GID> # assegna l'id di gruppo (GID) specificato al gruppo
	-r # Permette di assegnare un GID riservato all'utilizzo di sistema**
	
	ATTENZIONE:
	Richiede l'accesso come root

\* Il nome deve rispettare i seguenti criteri:
	- Il primo carattere deve essere minuscolo o il simbolo "\_"
	- Deve essere composto da massimo 32 caratteri (16 per alcune distro)
	- Tutti i caratteri dopo il primo devono essere alfanumerici, "\_" o "\-"
	- L'ultimo carattere non può essere "\-"

\*\* Per Debian tutti gli id minori di 1000, per RedHat tutti gli id minori di 500
#### groupdel
Elimino un gruppo
```sh
groupdel <nome_gruppo>
```
	Nel caso si voglia eliminare un gruppo che sia primario per un utente, il sistema lancerà un'avviso 
	OPZIONI:
	-f # forza la rimozione
	-r # 

#### groupmod
Modificare le caratteristiche di un gruppo
```sh
groupmod <gruppo>
```
	OPZIONI:
	-g <GID> # modifica il GID (id) del gruppo
	-n <nome> # cambia il nome del gruppo

**N.B.** Il comando modifica i dati relativi al gruppo sia agli utenti appartenenti che a tutti i file ad esso correlati, a meno che non venga modificato il GID (id del gruppo), nel qual caso il collegamento al gruppo viene rimosso dal file. I file senza nessun GID collegato, prendono il nome di *orphaned files*.

#### groups
Visualizzare i gruppi a cui appartiene un utente
```shell
groups <utente>
```
	Se non si inserisce l'utente, ritornerà tutti i gruppi del sistema
	OPZIONI:

#### gunzip
Decomprimere un file
```sh
gunzip
```

#### gzip
Comprimere un file tramite algoritmo Lempel-Ziv
```sh
gzip <file>
```
	OPZIONI:
	-u # decomprime il file

#### head
Visualizzare le prime 10 righe di un file
```sh
head <file>
```
	OPZIONI:
	-<numero di righe> # permette di visualizzare il numero di righe impostato
	-n <valore> # indica il numero di rhge da visualizzare
	-n -<valore> # mostra tutte le righe tranne le ultime 3
	-n +<riga> # mostra le righe partendo dalla riga indicata

#### history
Visualizzare lo storico dei comandi utilizzati
```sh
history
```
	ARGOMENTI:
	- Numero degli ultimi comandi da visualizzare

#### host
Verifica l'indirizzo IP di un nome di dominio ([DNS](../Tecnologie/Protocolli#DNS))
```host
host https://google.com
```
	OPZIONI:
	-a # accettta ogni tipo di richiesta (ANY)
	-m # attiva i lflag di debugging
	-t <nome> # specifica il tipo di richiesta
	-w # rimane in attesa fin quando non riceve una risposta (wait)
	-4 # usa solo il protocollo IPv4
	-6 # usa solo il protocollo IPv6
	
	RISULTATO: 8.8.8.8
	N.B. host può anche essere utilizzato per trovare il nameserver partendo da un indirizzo IP

I parametri accettati dall'opzione "-t" sono:
- **ANY**: accetta qualunque valore
- **SOA**: Start Of Authorioty. Accetta solo i valori del server primario
- **CNAME**: Canonical Name -alias
#### hping
Assembla e analizza i pacchetti utilizzati per port scanning, path discovery, OS fingerprinting e testing del firewall
```Terminal
hping
```

#### id
Verificare le informazioni di un utente
```sh
id <user>
```
	OPZIONI:
	-g # stampa solo il gruppo principale
	-G # stampa gli id dei gruppi a cui appartiene l'utente
	
	RISULTATO:
		uid=1001(sysadmimn) gid=1001(sysadmin) groups=1001(sysadmin),4(adm),27(sudo)

#### ifconfig
Mostrare i parametri delle interfacce di connessione (tipo IPv4, Ethernet)
```sh
ifconfig
```

```Esempio
eth0:     Link encap:Ethernet  HWaddr b6:84:ab:e9:8f:0a    
          inet addr:192.168.1.2  Bcast:0.0.0.0  Mask:255.255.255.0  
          inet6 addr: fe80::b484:abff:fee9:8f0a/64 Scope:Link       
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1        
          RX packets:95 errors:0 dropped:4 overruns:0 frame:0       
          TX packets:9 errors:0 dropped:0 overruns:0 carrier:0      
          collisions:0 txqueuelen:1000                              
          RX bytes:25306 (25.3 KB)  TX bytes:690 (690.0 B)
  
lo:       Link encap:Local Loopback                               
          inet addr:127.0.0.1  Mask:255.0.0.0                       
          inet6 addr: ::1/128 Scope:Host                           
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                  
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0        
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0      
          collisions:0 txqueuelen:0                                 
          RX bytes:460 (460.0 B)  TX bytes:460 (460.0 B)
```
	- eth0: porta analizzata
	- lo: loopback, ovvero dispositivo speciale, che la macchina utilizza per effettuare chiamate di rete a se stessa

#### ifdown
Disattivare un'interfaccia di connessione
```sh
ifdown <porta>
```

#### ifup
Attivare un'interfaccia di connessione
```sh
ifup<porta>
```

#### info
Visualizzare la documentazione relativa il sistema operativo o le feature
```sh
info <comando>
```
	NOTE:
	- nel caso non si inserisca nessun comando, ritornerà le informazioni di sistema

#### ip
Visualizzare le configurazioni IP
```sh
ip
```
	VALORI:
	- route: permette di interagire con la tabella di routing
	
	AZIONI:
	- show: mostra informazioni riguardo il valore selezionato
	
	ESEMPI:
		ip route show

#### iptables
Visualizzare le regole di accesso alla macchina ( #firewall)
```shell
iptables -L -v -n
```
	iptables -P INPUT DROP # cancella tutte le configurazioni  

#### iwconfig
Mostrare i parametri delle interfacce di connessione wireless
```sh
iwconfig 
```

#### jobs
Visualizzare i comandi attualmente in processo nel terminale (es. quelli lanciati seguiti da "&")
```sh
jobs
```

#### journalctl
Gestire i logging basati su **journald**
```sh
journalctl
```

#### kill
Terminare (metterlo in fase di arresto) di un processo (job)
```Sh
kill <processo>
```
	N.B. Se al numero del processo viene anteposto "%", il numero simboleggia l'ordine della lista nei jobs

#### killall
Terminare tutti i processi correlati ad uno specifico comando
```sh
killall <comando>
```

#### last
Tradurre in formato leggibile e visualizzare i log relativi gli accessi alla macchina, contenuti in [/var/log/wtmp](#wtmp) (richiede permessi di root)
```sh
last
```
	RISULTATO:
	sysadmin console Tue Sep 18 02:31   still logged in
	sysadmin console                    Tue Sep 18 02:31 - 02:31  (00:00)
	wtmp begins Tue Sep 18 02:31:57 2018

#### lastb
Tradurre in formato leggibile e visualizzare i log contenuti in [/var/log/btmp] (richiede permessi di root)
```sh
lastb
```

#### less
Visualizzare un elemento testuale (evoluzione di **more**)
```sh
less <file>
```
	NOTE:
	- ?: permette di effettuare una ricerca sul testo già letto
	- Shift+N: passa al successivo match con i criteri di ricerca 

**COMANDI**

| Comando                   | Descrizione                                               |
| ------------------------- | --------------------------------------------------------- |
| h, H                      | Apre il menu con la lista comandi                         |
| q, :q, Q, :Q, ZZ          | Esce dal lettore                                          |
| e, Ctrl+E, j, Ctrl+N, CR  | Andare avanti di una (o N) righe                          |
| y, Ctrl+Y                 | Torna indietro di una (o N) righe                         |
| f, Ctrl+F, Ctrl+V, Spazio | Avanti di una pagina o N righe                            |
| b, Ctrl+B, ESC+V          | Torna indietro di una pagina o N righe                    |
| z                         | Avanti di una pagina                                      |
| w                         | Indietro di una pagina                                    |
| ESC + Spazio              | Va avanti di una pagina, senza fermarsi a fine file (EOF) |
| d, Ctrl+D                 | Va avanti di mezza pagina                                 |
| u, Ctrl+U                 | Va indietro di mezza pagina                               |
| ESC+)                     | Va a destra di mezzo schermo                              |
**N.B.** per pagina si intende l'insieme di righe visualizzati in una schermata

#### locate
Localizzare un file all'interno del sistema tramite corrispondenza con i dati contenuti in un apposito database.
```sh
locate <nome file>
```
	NOTE:
	- la ricerca viene effettuata tramite database
	
	OPZIONI:
	-c # conta il numero di file trovati
	-b # limita la ricerca ai soli file e non alle directory nelle quali si trovano

**N.B.** Nel caso si voglia aggiornare il database contenente gli indirizzi dei file, si deve utilizzare [updatedb](#updatedb)


#### logrotate
Comando che permette di gestire la creazione dei log e ne permette la rotazione, compressione, rimozione e condivisione.

#### ln
Creare un [link](#link)
```Sh
ln <nome file> <nome link>
```
	OPZIONI:
	-s # crea un link simbolico

#### ls
Visualizzare tutti i [file e cartelle](#File) all'interno della [directory](#Navigazione) coecho ~/HOMErrente
```shell
ls
```
	OPZIONI:
	-a # mostra anche i file nascosti
	-all # mostra tutte le directory e i file contenuti, in una vista tabellare che comprende permessi, 
	-d # previene la visualizzazione dei file presenti nelle directory (utile nel caso si tuilzzion caratteri speciali per la ricerca)
	-h # mostra i risultati in un formato più facile da leggere
	-i # mostra l'inode* del file
	-l # mostra i dati in tabella
		tabella: directory_1 directory_2 directory_3|permessi|utente|gruppo utente|dimensione|data di creazione|nome del file 
		categorie utenti: proprietario, gruppo del proprietario, altri
		directory può essere "d" se directory o "-"
		permessi: r=read, w=write, x=execute, -=nessun perm
		dimensione: espressa in bytes
	-r # inverte l'ordine di visualizzazione dei file
	-t # ordina gli elementi in base al timestamp (data di modifica)
	-R # esegue il comando in maniera ricorsiva anche nelle cartelle figlie
	-S # ordina gli elementi in base alle loro dimensioni (dal più grande al più piccolo)
	--full-time # mostra maggiori dettagli dei timestamp dei file
	
	WILDCARDS:
	*: qualunque valore o combinazione di caratteri/simboli
	?: qualunque carattere/simbolo (1 carattere) 
	[]: match con un carattere all'interno di un preciso set di caratteri (indicato all'interno delle parentesi)
	!: qualunque carattere tranne quello successivo al simbolo

**N.B.** I file ritornati saranno colorati in base al loto tipo:
- bianco/nero: file normali
- blu: cartelle
- ciano: link simbolici
- verde: file eseguibili (programma)

**Link**
\* [inode](#inode)
#### lsblk
Controllare lo spazio utilizzato dalle varie partizioni
```shell
lsblk
```

#### lscpu
Controllare le caratteristiche della [CPU](../Tecnologie/Macchina#CPU)
```sh
lscpu
```

#### lsmod
Visualizza i moduli di memoria caricati
```sh
lsmod
```

#### lspci
Visualizzare la liste delle [periferiche](../Tecnologie/Macchina#Periferiche) collegate (List Peripheral Control Interface)
```sh
lspci
```

#### lsusb
Visualizzare l'elenco dei dispositivi collegati tramite [USB](../Tecnoloige/Macchina#USB)
```sh
lsusb
```

#### man
Aprire il manuale dei comandi del terminale per verificare le modalità d'uso di un comando (deriva da UNIX) tramite il lettore di file [less](#less)
```sh
man <comando>
```
	NOTE:
	- I documenti ritornati da man sono divisi in sezioni
	- Le pagine di man rientrano in una di 9 sezioni:
		1. Comandi generali
		2. Chiamate al sistema
		3. Chiamate alla libreria
		4. File speciali
		5. Formati di file e convenzioni
		6. GIochi
		7. Miscellanei
		8. Comandi di amministratore di sistema
		9. Routine del kernel
	- Shift+H permette di visualizzare le shortcut per navigare sulle pagine
	- utilizzando il simbolo "/" è possibile effettuare una ricerca
	
	SEZIONI:
	- 	NAME: fornisce il nome del comando e una breve descrizione
	- SYNOPSIS: Fornisce esmepi di esecuzione del comando
	- DESCRIPTION: fornisce una descrizione dettagliata del comando
	- OPTIONS: fornisce una lista delle opzioni disponibili per il comando
	- FILES: lista dei file associati al comando
	- REPORTING BUGS: modalità di report dei bug
	- COPYRIGHT: fornisce informazioni base sul copyright del file
	- SEE ALSO: link a fonti esterne per ulteriori informazioni
	
	OPZIONI:
	-f # filtra le pagine in base al nome (argomento inserito)
	-k # fornisce una breve descrizione dei contenuti delle varie pagine del manuale

#### more
Visualizzare un elemento di testo
```sh
more
```

#### mv
Spostare un file (può essere utilizzato anche per rinominare il file)
```sh
mv <file> <destinazione>
```
	OPZIONI:
	-i # chiede se si debba sovrascrivere un eventuale file già presente
	-n # impedisce la sovrascrittura
	-v # verbose, ovvero mostra il risultato in console

#### nano
Modificare il contenuto di un file
```sh
nano <nome_file>
```
	COMANDI:
	- Ctrl + O: salva il file
	- Ctrl + W: Apre il menu di ricerca
	- Ctrl + G: schermata di aiuto

#### ncat
Verifica la connessione ad un servizio tramite specifica porta
```Sh
ncat <indirizzo> <porta>
```
	N.B. È possibile utilizzare la forma abbreviata “nc”
	OPZIONI:
	-z | 
	-V |

#### netstat
Mostra le porte aperte al momento
```sh
netstat
```
	OPZIONI:
	-i # mostra informazioni sul traffico di rete
	-l # mostra solo le porte in ascolto (listening)
	-n # mostra i nomi degli endpoint
	-r # mostra informaizoni di routing
	-t # mostra il traffico tramite protocollo TCP

**ESEMPI**: 
- netstat -i
	```Risultato
	Kernel Interface table                                                        
	Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
	eth0       1500 0       137      0      4 0        12      0      0      0 BMRU
	lo        65536 0        18      0      0 0        18      0      0      0 LRU
	```
	- TX-OK segnala che la connessione è funzionante
	- TX-ERR segnala la presenza di un errore di connessione

**N.B.**
- Il comando ```netstat -g``` da lo stesso risultato di "[maddr](#maddr)"
- Il comando ```netstat -r``` da lo stesso risultato di "[route](#route)" o del più recente "[ip route show](#ip)"
- Il comando ```netstat -i``` da lo stesso risultato di "[ip -s link](#ip)"
- Il comando netstat è stato sostituito dai più recente [ss](#ss)

#### newgrp
Creare un nuovo gruppo e lo imposta come gruppo primario dell'utente che utilizza il comando.
```sh
newgrp <nome>
```

#### nl
Aggiunge un elenco numerato alla lista di valori
```sh
nl
```

#### nmap
Verificare se ci sono porte aperte verso l'esterno in una macchina #port-scanning
```bash
nmap <nome_macchina>
```

#### npm
Installare i pacchetti contenuti nel file package.json tramite npm
```Bash
npm install
```

#### node
Eseguire un file con [[Node.js]]
```Bash
node <nome file>.js 
```

#### nohup
Cancellare l'invio di segnali HUP (utilizzato per permettere la prosecuzione del processo anche a seguito di logout dell'utente che lo ha lanciato)
```sh
nohup <comando> <argomento>
```
	NOTE:
	- E' possibile aggiungere alla fine del comando il carattere "&" per indicare che il processo è un job (esecuzione in background)

#### passwd
Aggiungere una password di un utente
```sh
passwd <user>
```
	N.B. Verrà chiesto alllàutente di inserire la password. Nel caso la stessa no rispetitii criteri di sistema, verrà stampato un messaggio di avviso (impostato dall'amministratore) e verrà richiesto di utilizzarne una diversa

#### php
Aprire una #shell in [[Php]]
```Shell
php
```
	OPZIONI:
	-a # Apre la shell in maniera interattiva

#### ping
Richiedere un messaggio di risposta da un altro dispositivo
```sh
ping <indirizzo_ip>
```
	OPZIONI:
	-c <numero> # indica il numero di iterazioni da eseguire
	
	N.B. 
	- I seguenti indirizzi IPv4 sono utilizzati spesso durante i processi di troubleshooting:
		  127.0.0.1 - Indirizzo di Loopback, che permette di verificare se l'interfaccia TCP/IP del dispositivo è configurata correttamente  
	-I seguenti indirizzi IPv6 sono utilizzati spesso durante i processi di troubleshooting:
		  ::1 - Indirizzo di Loopback, che permette di verificare se l'interfaccia TCP/IP del dispositivo è configurata correttamente

#### pkill
Inviare un segnale a tutti i [processi](#Processo) attivati da un comando
```sh
pkill <comando>
```
	OPZIONI:
	-<numero> # numero del segnale da inviare al comando
		1. -
		2. -
		3. -
		4. -
		5. -
		6. -
		7. -
		8. -
		9. -
		10. -
		11. -
		12. -
		13. -
		14. -
		15. Terminare il processo
	--signal <numero> # simile a "-<numero>"
#### ps
Mostrare la lista dei processi in corso (di default mostra solo i processi in corso nella shell in uso)
```shell
ps
```
	 OPZIONI:
	 -e # mostra tutti i processi attivi (anceh quelli nascosti)
	 -f # mostra tutte le colonne dei dettagli per ogni processo:
		 - PID (identificativo del processo)
		 - %CPU (CPU utilizzata in %)
		 - %MEMORY (memoria utilizzata in %)
		 - VSZ
		 - RSS
		 - TTY (teletipo)
		 - STAT
		 - START (ora di avvio)
		 - TIME ()
		 - COMMAND (comando che ha fatto partire il processo)
	 -ef # mostra tutti i processi attivi con le colonne
	 -o <colonna1>,<colonna2> # mostra solo le colonne indicate
	 -u <utente># visualizza i processi attivi per uno specifico utente
	 --forest # crea una lista con visualizzazione ed albero simile a pstree
	 --sort %<colonna> # ordina i processi in base alla colonna indicata
	 
	 VALORI: (es. ps aux)
	- aux # mostra l’albero dei processi

\*[teletipo](https://www.reddit.com/r/linuxquestions/comments/oikda1/what_the_hell_is_tty/)

#### pstree
Visualizzare l'albero dei processi attivi
```sh
pstree
```
	ESEMPIO:
	init-+-cron
	|-login---bash---pstree # bashh è il genitore di pstree
	|-named---18*[{named}]
	|-rsyslogd___2*[{rsyslogd}]
	`-sshd

#### pwd
Stampo il percorso della cartella in cui mi trovo
```shell
pwd
```

#### python
Apro un terminale [[Python 1]]
```shell
python
```
	Il file ha permessi -rwsr-xr-x

#### reboot
Riviste il sistema
```Sh
reboot
```
	N.B. Il comando ha bisogno dei permessi di root

#### redis-sli
Interagire con un database remoto
```shell
redis-sli -h <nome_server>
```

#### rm
Rimuovere uno o più elementi
```sh
rm <nome_file>
```
	OPZIONI:
	-f # forza la rimozione
	-i # Interattivo, chiede se il file debba essere eliminato o meno (utile nel caso di uso di wildcard)
	-r # Effettua la rimozione in maniera ricorsiva (da usare ad es. per eliminare cartelle non vuote)
	-rf # Forza la rimozione ricorsiva

#### router
Visualizzare la tabella di routing. In alcune distro, il comando è deprecato e sostituito da [ip route show](#ip)
```sh
router
```
	OPZIONI:
	-n # mostra le informazioni in formato solo numerico
#### scp
Secure Copy (scp) permette di copiare file da una macchina in locale ad una in remoto.
```sh
scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2
```
	OPZIONI:
	-P # Specifica la porta di destinazione
	-p # Mantiene le moiìdifiche e i timestamp di accesso
	-q # Sopprime le metriche di progresso e i messaggi che non siano di errore
	-C # Forza la compressione dei file durante il trasferimento
	-r # Copia le directory in maniera ricorsiva

#### service
Gestire un servizio
```sh
service <servizio> <operazione>
```
	SERVIZI:
	- network
	
	OPERAZIONI:
	- restart

#### setfacl
Impostare una [ACL](#ACL) di un file
```sh
setfacl u:<user>:<permessi> <file>
```
	OPZIONI:
	-m # modifica la #ACL del file
	-R # applica la regola in maniera ricorsiva
	-x # riomuove la #ACL impostata

#### sfdisk

#### sh
Eseguire uno [shell script](#Script) tramite shell
```sh
sh <file>
```

#### sort
Ordinare le righe di un file alfabeticamente o numericamente
```sh
sort <file>
```
	OPZIONI:
	-k<numero> # specifica il delimitatore per la suddivisione delle righe in sezioni
	-n # ordine numerico
	-t<carattere> # specifica il numero della sezione da ordinare per prima

#### speedtest
Verificare la velocità di connessione ad internet
```sh
speedtest
```

#### ss
Abbreviazione di *socket statistics*. Permette di visualizzare le metriche di utilizzo dei [socket](../Tecnologie/Macchina#Socket) (supporta la maggior parte delle porte e dei pacchetti). E' pensato per sostituire il comando [netstat](#netstat).

```sh
ss
```
	OPZIONI:
	-s # mostra i protocolli di livello 4 - Trasporto (righe) e il numero di porte che lo utilizzano e i protocolli di livello 3 - Rete (colonne)

```Risultato
Netid  State      Recv-Q Send-Q   	Local Address:Port       	   Peer Address:Port
u_str  ESTAB      0      0                    * 104741                     * 104740 
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 14623      * 14606  
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 13582      * 13581  
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 16243      * 16242  
u_str  ESTAB      0      0                    * 16009                      * 16010  
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 10910      * 10909  
u_str  ESTAB      0      0      @/tmp/dbus-LoJW0hGFkV 15706                * 15705  
u_str  ESTAB      0      0                    * 24997                 	   * 24998  
u_str  ESTAB      0      0                    * 16242                      * 16243  
u_str  ESTAB      0      0      	@/tmp/dbus-opsTQoGE 15471              * 15470
```
	COLONNE:
	- Netid: Tipo di socket e protocollo di trasporto (livello 4)
	- State: Connesso o disconnesso
	- Recv-Q: Quantità di dati ricevuti
	- Send-Q: Quantità di dati inviati
	- Local Address:Port: indirizzo locale : porta utilizzata
	- Peer Address:Port: Indirizzo del destinatario : porta in ascolto
#### ssh
Connettersi ad un'altra macchina da remoto tramite protocollo [SSH](../Tecnologie/Protocolli#SSH)
```sh
ssh <utente>@><indirizzo_ip>
```
	NOTE:
	- Per chiudere la connessione, prenere Ctrl+D o utilizare in comando "exit"
	- Nel caso si voglia accedere come uno specifico utente:
		ssh <nome_utente>@<indirizzo_ip>
	
	OPZIONI:
	-i <chiave> # identificativo, ovvero chiave ssh da utilizzare per autenticarsi
	-p # porta

**N.B.** Nel caso si tenti una connessione ad una macchina già conosciuta (ovvero con la quale è già stata stabilita una connessione in passato, e la cui chiave di autenticazione è memorizzata all'interno di [~/.ssh/known_hosts](#known_hosts)), ma che ha cambiato la sua chiave [RSA](Algoritmi.md#RSA) (ad esempio a seguito di un ripristino), può essere necessario rimuovere il link dal file **~/.ssh/known/hosts**.:
	Esempio di messaggio d'errore:
	@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
	@   WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!   @
	@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
	IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
	Someone could be eavesdropping on you right now (man-in-the-middle attack)!
	It is also possible that the RSA host key has just been changed.
	The fingerprint for the RSA key sent by the remote host is
	c2:0d:ff:27:4c:f8:69:a9:c6:3e:13:da:2f:47:e4:c9.
	Please contact your system administrator.
	Add correct host key in /home/sysadmin/.ssh/known_hosts to get rid of this message.
	Offending key in /home/sysadmin/.ssh/known_hosts:1
	RSA host key for test has changed and you have requested strict checking.
	Host key verification failed.

#### ssh-add
Aggiungere una chiave ssh all'agent
```ssh
ssh-add <percorso>
```

#### ssh-agent
Impostare un ssh-agent
```sh
ssh-agent
```
	N.B. Nel caso il comando non vada a buon fine, lo sui può usare tramite eval

#### ssh-keygen
Per generare una #ssh-key 
```sh
ssh-keygen
```
	-t # Definire la tipologia di criptazione [dsa, ecdsa, ecdsa-k, ed25519, ed25519-sk, rsa]
	-C <nome utente>

#### start_webserver

```sh
start_webserver
```

#### stat
Visualizzare le proprietà di un file 
```sh
stat <file>
```
	RISULTATO:
	  File: `/tmp/filetest1'
	  Size: 0         	Blocks: 0          IO Block: 4096   regular empty file
	Device: fd00h/64768d	Inode: 31477       Links: 1
	Access: (0664/-rw-rw-r--)  Uid: ( 1001/sysadmin)   Gid: ( 1001/sysadmin) 
	Access: 2013-10-21 10:18:02.809118163 -0700
	Modify: 2013-10-21 10:18:02.809118163 -0700
	Change: 2013-10-21 10:18:02.809118163 -0700

#### su
Cambiare utente (chiede la password)
```shell
su <utente>
```
	OPZIONI:
	- #come "-l"
	-l # cambia user tramite login shell
	-m # preserva le variabili d'ambiente
	-p # come "-m"
	-s # usa la SHELL invece che quella di default
	--login # come "-l"
	
	N.B. Se non viene specificato nessun utente, viene effettuato un tentativo di accesso come root

#### sudo
Effettuare un comando come #superuser (SUperuser DO). Effettuabile solo se l'utente fa parte dei **sudoers**
```Shell
sudo <comando>
```

#### sysctl
Esegue le funzionalità #sysctl
```shell
sysctl
```
	-p # riavvia il SO

#### tail
Visualizzare le ultime 10 righe di un file
```sh
tail  <file>
```
	OPZIONI:
	-<numero di righe> # permette di visualizzare il numero di righe impostato
	-n # indica il numero di rhge da visualizzare

#### tar
Archiviare (compressione di un gruppo di file) elementi
```sh
tar [OPZIONI] <file di destinazione>.tgz <file originale> 
```
	OPZIONI:
	-c # Crea un archivio
	-cf # crea e usa archivio (inserisci file)
	EXTRACT
	-j # comprime o decomprime tramite bzip2
	-t # mostra lista di elementi inclusi nellarchivio
	-V # stampa i file inclusi nell’archivio mentre li processa
	-x # comprime o decomprime un file tramite gzip

##### Esempi
```sh
tar -cvzf file.tgz /Documents/file.txt
```
	DESCRIZIONE: comprimere il file file.txt, situato nella cartella Documents dentro un nuovo file file.tgz nella cartella corrente

```sh
tar -xvzf file.tgz
```
	DESCRIZIONE: decomprimere il file file.tgz, situato nella cartella corrente in nuovo file dal nome "file" nella cartella corrente

#### test
Effettuare test su un comando
```sh
test 
```

#### top
Mostrare la lista dei [processi](#Processo) attivi in maniera dinamica (di default in base alla percentuale di utilizzo di CPU)
```sh
top
```
	AZIONI:
	K # Interrompe il processo tramite invio di un numero di segnale 9 (interruzione)
	Q # interrompe il comando
	R # Riassegna un valore di priorità (da -20 a 19) al processo, utilizzabile solo da root

#### tr
Trasformare caratteri tramite [RegEx](../Linguaggi/RegEx)
```sh
tr <caratteri da trasformare> <nuovi caratteri>
```

#### traceroute
Tracciare il percorso dei pacchetti fino alla destinazione
```sh
traceroute
```

#### type
Ottenere informazioni riguardo il tipo di comando
```sh
type <comando>
```
	OPZIONI:
	-a # mostra tutte le informazioni (se è interno e percorso assoluto)
	NOTE:
	- Se il comando è interno, si otterrà:
		<nome_comando> is a shell builtin
	- Se il comando è esterno, si otterrà il percorso assoluto dove è situato il file che attiva il comando

#### umask
Gestire i [permessi](#Permessi) di default per la creazione di una directory (cartella). I permessi non possono superare quelli di lettura + scrittura (666)
```sh
umask <valore>
```
	N.B. Se non si inserisce alcun valore, il comando mostrerà i permessi di default attualmente in uso

Il **metodo di calcolo** utilizzato per determinare il valore da utilizzare è la sottrazione del valore inserito da quello di default:

Nel caso il valore di default sia 666:
```Esempio
umask 027
```
	RISULTATO: 640
		666 -
		027 =
		640

#### umount
Smontare una [periferica](../Tecnologie/Macchina#Periferiche) di memoria
```sh
umount
```
	OPZIONI:
	-a # smonta tutti i file dal sistema
	-A # smonta tutti i punti di montaggio per il dispositivo indicato
	-c # non standardizzare (canonicalize) i percorsi

#### unalias
Rimuovere un alias
```sh
unalias <nome alias>
```

#### uname
Ottenere il nome del sistema
```sh
uname
```
	OPZIONI:
	-n # ritorna il nome del nodo in cui ci si trova (es. localhost)

#### unset
Cancellare una variabile
```sh
unset <nome_variabile>
```
	NOTE:
	- Non è necessario anteporre $ alla variabile in questo caso

#### unxyz
Decomprimere un file tramite algoritmo Lempel-Ziv-Markov
```sh
unxz <file>
```

#### unzip
Decomprimere uno zip
```sh
unzip
```
	OPZIONI:
	-l # mostra la lista dei file all’interno dell’archivio zip

#### updatedb
Aggiornare il database dei nomi di tutti i file del sistema
```sh
updatedb
```

#### uptime
Visualizzare dati relativi l'utilizzo di risorse negli ultimi 1, 5 o 15 minuti (da [/proc/loadavg](#loadavg))
```sh
uptime
```

#### useradd
Creare un nuovo utente (richiede permessi root)
```sh
useradd <nome_utente>
```
	L'utente viene registrato in 3 file: /etc/passwd, /etc/shadow e /etc/group
	OPZIONI:
	-b <direcrtory> # indica di creare una directory diversa da quella di default (/home/<utente>)
	-c # aggiunge valori al campo COMMENT dell'utente
	-d # crea una home directory
	-e # inserisce una data di "scadenza" all'utente
	-f <valore> # modifica il valore INACTIVE
	-g <nome_gruppo> # aggiunge l'utente al gruppo come gruppo primario
	-m # crea una home directory dedicata e si posta nella stessa
	-p <password> # imposta la password. Sconsigliato perchè rende la password visibile nella lista dei processi
	-r # crea un account di sistema
	-s <shell> # indica di collegare l'utente ad una particolare shell (di default si usa quella indicata in /etc/default/useradd)
		-s /sbin/nologin # non imposta una shell di accesso per l'utente
	-u <UID> # specifica l'UID da assgnare all'utente
	-D # permette di visualizzare e/o modificare alcuni parametri* dell'utente
	-G <nomi_gruppo> # aggiunge l'utente ai gruppi come gruppo secondario (i nomi dei gruppi separati da una virgola)
	-M # indica al sistema di non creare una home directory per l'utente

\* GROUP, HOME, INACTIVE, EXPIRE, SHELL, SKEL, CREATE_MAIL_SPOOL (?)
#### userdel
Eliminare un utente
```sh
userdel <nome_utente>
```
	ATTENZIONE:
	Nel caso si cancellino più utenti, è possibile che Debian cancelli anche le password di root
	OPZIONI:
	-r # cancella utente e relativi mail spool e home directory (eventuali file esterni alla home directory diventano orfani, ovvero senza utente proprietario)
	

#### usermod
Modificare le caratteristiche di un utente
```sh
usermod
```
	Di solito si usa con sudo
	OPZIONI:
	-a # aggiunge dati (append) invece che sovrascriverli
		-aG <gruppo> <user> # Aggiunge l'user al gruppo
	-c # permette di aggiungere un commento nel campo GECOS o dei commenti
	-d <HOME_DIR> # cambia il valore di HOME_DIR nella nuova directory indicata
	-e <valore> # cambia il valore di EXPIRY_DATE
	-f <valore> # cambia il valore di INACTIVE
	-g <gruppo> # imposta il gruppo come primario
	-l <nome> # cambia il nome di login dell'utente
	-s <shell> # specifica la shell di login da utilizzare
	-u <id> # modifica l'id dell'user (UID)
	-G <gruppi> # imposta i gruppi secondari dell'utente
	-L # blocca l'utente
	-U # sblocca l'utente

#### vim
Modificare il contenuto di un file
```sh
vim <nome_file>
```

#### wc
Visualizzare le statistiche di un file
```sh
wc <file>
```
	OPZIONI:
	-l # mostra solo il numero di righe
	
	RISULTATO:
	1. numero di linee
	2. numero di parole
	3. numero di bytes
	4. nome del file

#### whatis
Filtra i contenuti del man in base al nome della pagina
```sh
whatis <nome>
```

#### whereis
Localizzare il file che gestisce un determinato comando
```sh
whereis <comando>
```

#### which
Visualizzare il percorso assoluto del file di un comando
```sh
which <nome_comando>
```

#### w
Visualizzare informazioni dettagliate sugli utenti
```sh
w
```
	RISULTATO:
	 10:44:03 up 50 min,  4 users,  load average: 0.78, 0.44, 0.19
	USER     	TTY     FROM	    LOGIN@   IDLE  	JCPU   	PCPU    WHAT
	root     	tty2    -           10:00    43:44 	0.01s  	0.01s   -bash
	sysadmin 	tty1    :0          09:58    50:02	5.68s 	0.16s   pam: gdm-password
	sysadmin	pts/0   :0.0        09:59    0.00s      0.14s  	0.13s   ssh 192.168.1.2
	sysadmin 	pts/1   example.com 10:00    0.00s  	0.03s  	0.01s   w

| colonna | Descrizione                                     |
| ------- | ----------------------------------------------- |
| USER    | Nome utente                                     |
| TTY     | Terminale utilizzato per il login               |
| FROM    | Dove è l'utente                                 |
| LOGIN@  | Quando ha effettuato l'accesso                  |
| IDLE    | Da quanto tempo non viene eseguito un comando   |
| JCPU    | CPU utilizzata per qualunque processo dal login |
| PCPU    | CPU utilizzata per il processo in corso         |
| WHAT    | Il processo in corso per l'utente               |
#### who
Visualizzare informazioni riguardo gli utenti del sistema, memorizzate all'interno di [/var/log/utmp](#utmp)
```sh
who
```
	OPZIONI:
	-b # mostra l'ultima data di avvio del sistema (boot)
	-r # mostra quando il sistema ha raggiunto l'attuale runlevel
	
	RISULTATO:
		root      tty2       2013-10-11 10:00
		sysadmin  tty1       2013-10-11 09:58 (:0)
		--------------------------------------------
		utente    terminale  ultimo login     login tramite terminale grafico

#### whoami
Verificare l'utente con cui si sta operando
```Shell
whoami
```

#### xz
Comprimere un file tramite algoritmo Lempel-Ziv-Markov (buona compressione e uso limitato di CPU, estensione .xz)
```sh
xz <file>
```

#### zip
Copiare i file selezionati in un archivio, compresso tramite algoritmo zip
```sh
zip <file>
```

Comandi da inserire
```shell
chroot
sed
```

### Operazioni tra comandi
Indicare la fine di un comando
```sh
;
```
	Questo permette di utilizzare più comandi alla volta

Concatenare comandi
```Linux
<comando_1> && <comando_2>
```

Eseguire un comando nel caso quello precedente non venga eseguito correttamente (doppia pipe)
```Linux
<comando_1> || <comando_2> 
```

Piping di due comandi
```sh
<comando 1> | <comando 2>
```
	Il piping serve a filtrare i risultati del primo comando attraverso il secondo
	ES. ls -l | grep file

### Ridirezioni
Le ridirezioni possono essere fatte sia per **input** che per **output**.
- **STDIN**: L'input standard è un input tipicamente inserito dall'utente.
- **STDOUT**: L'output standard è il risultato (senza errori) di un'operazione. E' anche chiamato *channel#1*
- **STDERR**: L'errore standard è un messaggio di errore generato da comandi e mostrato di solito nella stessa console dalla quale si è lanciato il comando. E' anche noto come *channel#2*

#### Input
Non tutti i comandi interagiscono con gli input. Alcuni dei comandi che lo fanno sono:
- cat
- tr

Esempio di utilizzo di input e output in una singola riga di comando
```Esempio
tr 'a-z' 'A-Z' < file.txt > new_file.txt
```
	RISULTATO: Prende il testo contenuto su "file.txt", modifica tutti i caratteri minuscoli in maiuscoli e li salva su file "new_file.txt" (se "new_file.txt" non esiste lo crea)

#### Output
Per reindirizzare un output verso un altro comando si usa il simbolo **>** (nel caso di errore, l'output verrà solo stampato in console)
```Esempio
echo "Ciao" > file.txt
```
	RISULTATO:
	Verrà creato un file dal nome "file.txt" con contenuto "Ciao"

Per aggiungere un testo in una nuova riga di un file di testo, si utilizza il comando **>>**
```Esempio
echo "Seconda riga" >> file.xtx
```
	file.txt:
	Ciao
	Seconda riga

Per registrare gli errori risultanti dai comandi, è possibile catturarli e salvarli in un file apposito
```Esempio
ls /fake 2> error.txt
```
	error.txt:
	ls: cannot access '/fake': No such file or directory

Per registrare un qualunque output (sia esso risultato o errore), si utilizza il simbolo "**&**".
```Esempio
ls /fake &> output.txt
```

### Info utili
- Premendo Tab, il terminale completa automaticamente il comando (o il file). Nel caso ci siano più risultati possibili, ritorna sotto la linea di comando la lista delle possibilità
- I comandi presenti di default nella shell vengono chiamati **comandi interni**
- I comandi creati dall'utente (o da altri utenti) vengono chiamati **comandi esterni**
- Ogni comando è scritto ed evocato tramite relativo file

## Variabili
La shell utilizza due tipi di variabile:
- Locali
- Di ambiente

### Locali
Le variabili locali sono valid solo per la sessione di bash nella quale sono state impostate.

Le **variabili locali** sono impostabili tramite dichiarazione del nome della variabile e contenuto:
```sh
<nome_variabile>=<valore>
```
	NOTE:
	Il contenuto può essere di tipo:
		- stringa (es. 'ciao')
	
	ESEMPIO:
	variabile1='ciao'

E sono utilizzabili anteponendo il simbolo "**$**" al nome della variabile
```sh
$<nome_variabile>
```
	ESEMPIO:
		echo $variabile1 # la shell ritornerà
			ciao

### Ambiente
Le variabili d'ambiente vengono utilizzate dall'intero sistema e vengono create nella shell al suo avvio. Questo tipo di variabili è salvato all'interno del sistema.

La creazione e l'utlilizzo delle **variabili d'ambiente** è simile a quello delle variabili locali, ma il loro nome è sempre scritto con caratteri maiuscoli

```ESEMPIO
variabile_locale=1
VARIABILE_DI_AMBIENTE='valore'
```

#### PATH
Contiene tutte le directory dove sono memorizzati i comandi.
DI default il valore è importato uguale a
```PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

## Scripting
Uno script shell è un file che contiene un'istruzione eseguibile su shell tramite comando **sh**. Il sistema operativo contiene alcuni [script precompilati](<#Script precompilati>).

### Sintassi
Alcune combinazioni di caratteri permette di ottenere effetti particolari durante la creazione di script.

| Sintassi     | Descrizione                                                                                                     |
| ------------ | --------------------------------------------------------------------------------------------------------------- |
| #! (shebang) | Permette di utilizzare altri linguaggi tramite reindirizzamento al percorso assoluto del rispettivo compilatore |

### Operatori
#### for
#### if
```Esempio
if [$? = 'Hello']; then
	echo "Hello, there"
fi
```
#### while

### Script precompilati
#### hello.sh
Contiene un **echo** con un semplice "hello world"

# Distro

## Kali

## Security Onion
Distro specializzata nell’analisi della sicurezza delle reti, molto utilizzata negli ambienti [SOC](../cybersecurity#soc) (Security Operation Center, o C’è tri Operativi di Sicurezza).

### Tools


## Ubuntu
### Software
#### [UFW](../Software/UFW)
Il Firewall non Complicato di Ubuntu è un semplice [Firewall basato sull'host](../Cybersecurity#HBF), installato di default, che si basa su [iptables](#iptables).

##### Gufw
E' l'interfaccia grafica con cui è possibile utilizzare UFW.

# GUI

```unix
top # mostra i processi attivi nella macchina
htop # come top, ma con una barra orizzontale per l'utilizzo della CPU
```
## [kali-linux](https://www.kali.org/get-kali/#kali-installer-images)
E' una versione di linux molto utilizzata per i [penetration test] e per l’attività forense. La distribuzione contiene oltre 600 tool (soprattutto da riga di comando), alcune delle quali utili per effettuare attacchi informatici. L'utente di base si chiama **kali** e i comandi sono in base #unix (utilizzato, ad esempio, anche in #git-bash) .
```sh
DEFAULT SETTINGS
user: kali
password: kali
```
### Struttura del terminale
```bash
|- (<utente>@<macchina>) -[<localizzazione>]
|- $
```
	Nel caso ci si trovi nella cartella principale (di default /home/kali), la localizzazione sarà ~
	Nel caso ci si trovi nella cartella root, la localizzazione sarà /
### [Struttura](#Struttura)
```root
├ 0
├ bin #contiene file e applicazioni binari
├ boot
├ etc # contiene file di configurazione del sistema
|	├ sysctl.conf
|	├ host.allow # non presente di default
|	├ host.deny # non presente di default
|	├ ssh
|	|	└ sshd_config 
|	└  passwd
├ home #contiene tutti gli utenti locali
├ dev #contiene i file necessari per far funzionare i dispositivi
├ lib #contiene i file di sistema
└ media #contiene i supporti esterni
```
	- ssh_config gestisce i permessi per effettuare chiamate in SSH

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
### [Code scanning](Cybersecurity.md#SCA)

# File
## Introduzione

Le proprietà di un file sono visualizzabili tramite comando [ls](#ls), assegnando l'opzione "-l"
```Esempio
d rwxr-x--x 2 syslog root 4096 Jul 19 06:51 journal
```
	d: tipo di file
		-: file normale
		d: cartella
		l: link simbolico
		b: blocco
		c: file carattere (correlato ad un hardware)
		p: file pipe (che invia la comunicazione ad un altro file)
		s: file socket (permette la comunicazione tra due processi)
	rwx: permessi del creatore deil file
	r-x: permessi del gruppo del creatore
	--x: permessi degli altri utenti
	2: conteggio degli Hard Link
	syslog: utente che ha creato il file
	root: gruppo dell'utente
	4096: dimensione del file
	Jul 19 06:51: data di ultima modifica del file
	journal: nome del file

I file possono essere:
- **Visibili**
- **Nascosti**: iniziano sempre con il simbolo "**.**" e non vengono mostrati dal comando ```ls```

Alcune proprietà  del file sono assegnabili tramite cartella di appartenenza, come:
- **Condivisibile**: Può essere condivisa con altre macchine all'interno della rete
- **Non condivisibile**: Non può essere condivisa con altre macchine
- **Variabile**: ovvero il contenuto del file può essere modificato
- **Statico**: Il contenuto non può essere modificato dall'utente

Le convenzioni per la nomenclatura dei file e delle cartelle in base a queste caratteristiche è presente [qui](http://www.pathname.com/fhs/).

### Pseudo filesystem
Gli pseudo file di sistema sono file che non vengono memorizzati in un dispositivo di memoria, bensì vengono salvati solo nella memoria temporanea. Di solito vengono utilizzati per contenere informazioni relative a [processi](#Processo) attivi.

## Permessi
- r = read (1): è possibile aprire, visualizzare e copiare il file (possibile effettuare un "ls" nel caso sia una cartella)
- w = write (2): è possibile modificare e salvare il file (di solito funziona correttamente solo se accoppiato con permessi di lettura)
- x = execute (4): è possibile eseguire il file (o entrarci dentro nel caso di directory) ed eliminarlo (nel caso ci siano anche i permessi di scrittura)

I permessi vengono mostrati tramite una stringa suddivisa in 3 parti, ciascuna di 3 caratteri
```permessi
- rwx-w-rw-
```
	- o d = specifica se si tratta di un file o di una directory
	1 terzetto = utente proprietario
	2 terzetto = gruppo
	3 terzetto = altri utenti

**N.B.** Il significato del primo simbolo (-) è indicato [qua](<#Tipi di File>)

È possibile utilizzare i numeri per definire i permessi anche come combinazioni:
0. Nessun accesso
1. Esecuzione
2. Scrittura
3. Esecuzione e scrittura
4. Lettura
5. Lettura ed esecuzione
6. Lettura e scrittura
7. Lettura, scrittura ed esecuzione

Il comando per modificare i permessi è [chmod](#chmod)

### ACL
Le Access Control List (Liste di Controlli di Accesso) permettono di dare permessi extra ad utenti specifici per uno specifico file o directory.

### Tipi di file
Linux supporta 7 tipi di file (la tipologia di un file è visualizzabile nel primo simbolo di descrizione del file a tramite comando ```ls -l```)

| Simbolo | Tipologia               | Descrizione                                               |
| ------- | ----------------------- | --------------------------------------------------------- |
| d       | Directory               | File utilizzato per racchiudere altri file (cartella)     |
| -       | File regolare           | Include file leggibili, immagini, binari e file compressi |
| l       | [Link simbolico](#Link) | Punta ad un altro file                                    |
| s       | Socket                  | Permette la comunicazione tra processi                    |
| p       | Pipe                    | Permette la comunicazione tra processi                    |
| b       | Blocco file             | Utilizzato per comunicazioni con l'hardware               |
| c       | Carattere               | Utilizzato per comunicazioni con l'hardware               |


### setuid
Permette di avviare un file eseguibile binario (programma) come se si fosse l'utente che ha creato il file stesso. Per abilitare il setuid si possono utilizzare vari metodi:
- Aggiungere 4000 ai permessi (es. 4770)
- tramite comando ```chmod u+s <file>```

Per rimuovere il setuid, basta eseguire il processo inverso.

A seguito dell'invio del comando ```ls -l```, il risultato per un file con setuid sarà
```RISULTATO
-rwsrw-r--. 1 root root ... <file>
```
	N.B. Se il file ha anceh setgid attivato, ma il file non ha i permessi di esecuzione per il gruppo, il carattere "S" sarà maiuscolo (s = S + x)

### setgid
Tipologia di permesso simile al [setuid](#setuid), ma dedicato ai gruppi invece che agli utenti. A differenza del primo, questo può essere utilizzato sia per impostare i permessi nei file, che per le cartelle. Nel secondo caso permetterà di impostare automaticamente il gruppo utenti dei file che verranno creati all'interno uguale al GID del proprietario della cartella stessa (utile nel caso di condivisione cartelle, in quanto permette di utilizzare un gruppo secondario invece che il principale, come avverrebbe di default).

Si abilita con:
- Inserimento del valore 2000 ai permessi (es. 2770)
- tramite comando ```chmod g+s <file>```

A seguito dell'invio del comando ```ls -l```, il risultato per un file con setgid sarà
```RISULTATO
-rw-rwsr--. 1 root root ... <file>
```
	N.B. Se il file ha anceh setgid attivato, ma il file non ha i permessi di esecuzione per il gruppo, il carattere "S" sarà maiuscolo (s = S + x)

### sticky bit
Lo sticky bit viene utilizzato per impedire agli utenti di cancellare i file che non possiedono all'interno di una cartella condivisa (in cui hanno permessi di scrittura). Un esempio di cartelle di questo tipo sono:
- [/tmp](#tmp)
- [/var/tmp](#var#tmp)

Lo sticky bit si attiva tramite:
- Comando ```chmod o+t <directory>```
- Assegnazione del valore 1000, sommato ai permessi della cartella su cui si vuole applicare lo sticky bit 

## Link
Un link è un collegamento tra un file ed un altro. Può essere di due tipi:
- Hard link: i file sono collegati e le modifiche ad uno verranno effettuate anche sul secondo (cancellazione del file inclusa)
- Soft link: le modifiche non vengono propagate

Il percorso dei soft link viene segnalato dal comando `ls -l`, mentre gli hard link sono più difficili da localizzare.
Gli hard link inoltre possono essere creati solo per file localizzato sullo stesso File System (e non possono essere collegati direttamente ad una directory, perché il sistema stesso usa gli hard link per gestire le cartelle).

Il comando per gestire i link è [ln](#ln), mentre di solito è comodo utilizzare in comando "find" per cercare su tutta la macchina file con lo stesso inode del file originale: 
```
find <cartella> -inum <inode del file originale>
```
	RISULTATO: il comando ritornaerà una lista dei file trovati


### Hard
Sono file copia dell'originale persistenti (cancellare l'originale non cancella le copie), collegati tramite identico [inode](#inode).

Sono identificabili tramite comandi ```ls -l <hard link>```
```RISULTATO
l<permessi>. 1 <utente> <gruppo> <timestamp> <hard link> -> <file originale> 
```

### Symbolic
Sono copie volatili di un file (rimuovendo l'originale vengono automaticamente cancellate anche le copie), collegate all'originale tramite identico [inode](#inode) e identificabili anche tramite comando 
```
ls -l <symbolic link>
```
	RISULTATO
	l<permessi>. 1 <utente> <gruppo> <timestamp> <symbolic link> -> <file originale> 

Alcuni benefici dei symbolic links (anche noti come soft links) sono:
- E' possibile creare link di cartelle
- E' possibile creare un soft link di qualsiasi file
- Sono più facili da trovare

## Navigazione
La navigazione avviene tramite inserimento dei percorso del bersaglio. Il percorso può essere inserito sia in maniera **assoluta** che **relativa**:
- **Percorso assoluto** rappresenta la localizzazione del file rispetto al root di sistema e si indica tramite simbolo **/** all'inizio del percorso
- **Percorso relativo** rappresenta la posizione del percorso rispetto alla cartella utilizzata dall'utente al momento del lancio del comando. Per indicare l'uso di un percorso relativo, si inserisce "**./**" all'inizio del percorso, o in alternativa si inserisce direttamente il percorso.

| Simbolo | Descrizione             |
| ------- | ----------------------- |
| ./      | Inzio percorso relativo |
| /       | Root directory          |
| ..      | Cartella genitore       |

### inode
Un [inode](https://www.ionos.it/digitalguide/server/know-how/inode/) (index node) è una struttura dati contenente informazioni sui file presenti in un file system di Linux. Solo un inode è presente per ogni file, ed il file stesso viene identificato in modo unico dal file system sul quale risiede, e dal proprio numero inode presente sul sistema in questione.

## Struttura
![[linux_struttura_cartelle.png]]
### bin/
Il nome è il diminuitivo di **binario**. La cartella di solito viene utilizzata (anche all'interno di altre cartelle) per raccogliere file binari utilizzabili dagli utenti (al contrario della cartella [sbin/](#sbin)). E' possibile inserire anche file binari di software terzi.

#### systemd
File che contiene informazioni relative l'avvio del kernel Linux.

### boot/
### dev/
Contiene i file relativi alle [periferiche esterne](../Tecnologie/Macchina#Periferiche). Ogni dispositivo avrà una propria cartella in cui saranno memorizzati i file necessari per l'uso.

#### Nomenclatura
La nomenclatura dei dispositivi si basa su un sistema numerale incrementale con prefisso che indica la tipologia di dispositivo (ide0, ide1, ide2, ...). Alcune tipologie utilizzano lettere al posto dei numeri (segnalati in tabella). Questa metodologia prende il nome di IDE Drive Mapping

| Tipologia | Prefisso     |
| --------- | ------------ |
| IDE       | ide          |
| CDROM     | hd           |
| SCSI      | sd\<lettera> |
|           |              |

### etc/
#### group
Contiene la lista dei gruppi
```group
<nome_gruppo>:x:a:b
```
	x = password del gruppo (se 'x' non c'è password)
	a = Group ID
	b = Utenti appartenenti al guppo
#### hosts
File che gestisce i nomi degli host. Può essere utilizzato come supporto di [resolv.conf](#resolv.conf) per la gestione del server [DNS](../Tecnologie/Protocolli#DNS).
```hosts
nameserver 127.0.0.1
```

#### login.defs
File che contiene informazioni e valori da applicare di default ai nuovi utenti creati tramite [useradd](#useradd). A differenza del simile [/etc/default/useradd](#etc#useradd), questo file è gestio direttamente dall'amministratore di sistema.

Un esempio di file (ottenuto tramite ```grep -Ev '^#|^$' /etc/login.defs```, ovvero senza commenti e righe vuote) è il seguente:
```login.defs
MAIL_DIR	/var/mail/spool
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_MIN_LEN	5
PASS_WARN_AGE	7
UID_MIN			  500
UID_MAX			60000
GID_MIN			  500
GID_MAX			60000
CREATE_HOME	yes
UMASK           077
USERGROUPS_ENAB yes
ENCRYPT_METHOD SHA512
MD5_CRYPT_ENAB no
```

#### journald.conf
File di configurazione per i log tramite [journalctl](#journalctl).

#### nsswitch.conf
File che contiene direttive per l'ordine di consultazione delle risorse di name server ([resolv.conf](#resolv.conf) e [hosts](#hosts)).
```nsswitch.conf
#                                                                               
# Example configuration of GNU Name Service Switch functionality.               
# If you have the `glibc-doc-reference' and `info' packages installed, try:     
# `info libc "Name Service Switch"' for information about this file.            

passwd:         compat systemd                                                  
group:          compat systemd                                                  
shadow:         compat                                                          
gshadow:        files                                                           
hosts:          files dns                                                       
networks:       files                                                           
protocols:      db files                                                        
services:       db files                                                        
ethers:         db files                                                        
rpc:            db files                                                        
netgroup:       nis
```

#### passwd
Il file contiene la lista di utenti registrati e i dati ad essi relativi. La password verrà inserita nel file solo nel caso l'utente venga creato non di default (es. root non avrà la password segnata nel file)
```passwd
root:0:0:
<nome_utente>:a:b:c:<commento>:<home_directory>:<shell_directory_dell'utente>
```
	a: password (se salvata in un altro file sarà x, mentre gli account di sistema hanno "*")
	b: User ID
	c: Group ID principale
	comemnto: di solito nome utente e altre informazioni
	shell_directory: gli account di sistema di solito hanno una shell non abilitata al login su altri utenti (segnalata ad esempio come "/usr/sbin/nologin")
	
	: = separatore di campi
	:: = campo vuoto
Il file ha permessi **731**, con permessi [setuid](#setuid).

#### resolv.conf
File di configurazione utilizzato per gestire il Server DNS del dispositivo.
```resolv.conf
nameserver 127.0.0.1
domain google.com # opzionale
search dominio1, dominio2 # opzionale
```

- **domain**: Permette di utilizzare il dominio indicato come destinatario nel caso di insuccesso di risoluzione del nameserver. Per es., nel caso nameserver non dia risposta, il computer tenterà di stabilire una connessione con il dominio google.com e sottodominio nameserver (**nameserver.google.com**)
- **search**: Come domain, ma accetta una serie di domini su cui effettuare tentativi di connessione a seguito di fallimento di connessione con l'IP del nameserver.
#### rsyslog.conf
Variante di [/etc/syslog.conf](#syslog.conf) per alcune distro di Linux.

#### shadow
File contenente le password.
```shadow
<username>:<password_cifrata>:a:MIN_DAYS:MAX_DAYS:WARN_DAYS:e:f:g
```
	password cifrata = viene cifrata ad una sola via, senza meccanismo di decriptazione a supporto
	a = Data dell'ultimo cambio password
	MIN_DAYS = Numero minimo di giorni di attesa tra due cambi password
	MAX_DAYS = Numero massimo di giorni tra due cambi password
	WARN_DAYS = Giorni di preavviso prima della scadenza della password
	e = Periodo di grazia per cambiare la passowrd prima della cancellazione dell'account
	f = Data di scadenza dell'account
	g = campo riservato (attualmente non utilizzato)
	
	N.B. Tutte le date sono in formato epoch (giorni passati dal 1 Gennaio 1970)
#### ssh/
Contiene i file delle chiavi pubbliche e provate del sistema

##### sshd_config
Gestire gli accessi al gruppo #root
```sshd_config
# PermitRootLogin = 0 # permettere l'accesso all'utente root

# MaxAuthhTries 6 # numero massimo di tentativi di accesso

# PermitRootLogin prohibit-password # gestire l'accesso all'utente root

# ClientAliveInterval 0 # intervallo di inattività di un utente loggato prima del logout automatico

# ClientAliveCountMax 3 # numero massimo di utenti collegati cotemporaneamente alla macchina

```
#### sysconfig/
Directory presente solo per i sistemi [CentOs], che contiene informazioni riguardo le connessioni alla rete.

##### network-scripts/
##### ifcfg-eth0
File presente per ogni periferica connessa alla rete del dispositivo (l'ultima parte del nome descrive la periferica che gestisce), che contiene una serie di variabili che ne riassumono le caratteristiche

Vari esempi di configurazione sono
```IPv4
DEVICE="eth-0"
BOOTPROTO=none # valore attivo nel caso il device sia impostato come client di un particolare protocollo (es. DHCP client)
NM_CONTROLLED="yes"
ONBOT=yes
TYPE="Ethernet"
UUID="98cf38bf-d91c-49b3-bb1b-f48ae7f2d3b5"
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
NAME="System eth0"
IPADDR=192.168.1.1
PREFIX=24
GATEWAY=192.168.1.1
DNS1=192.168.1.2
HWADDR=00:50:56:90:18:18
LAST_CONNECT=1376319928
```

```IPv6
DEVICE="eth-0"
BOOTPROTO=none # valore attivo nel caso il device sia impostato come client di un particolare protocollo (es. DHCP client)
NM_CONTROLLED="yes"
ONBOT=yes
TYPE="Ethernet"
UUID="98cf38bf-d91c-49b3-bb1b-f48ae7f2d3b5"
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=yes
IPV6ADDR=<IPv6 IP Address>
IPV6_DEFAULTGW=<IPv6 IP Gateway Address>
NAME="System eth0"
PREFIX=24
DNS1=192.168.1.2
HWADDR=00:50:56:90:18:18
LAST_CONNECT=1376319928
```

```DHCP
DEVICE="eth-0"
BOOTPROTO=DHCP
NM_CONTROLLED="yes"
ONBOT=yes
TYPE="Ethernet"
UUID="98cf38bf-d91c-49b3-bb1b-f48ae7f2d3b5"
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
DHCPV6INIT=yes
NAME="System eth0"
IPADDR=192.168.1.1
PREFIX=24
GATEWAY=192.168.1.1
DNS1=192.168.1.2
HWADDR=00:50:56:90:18:18
LAST_CONNECT=1376319928
```

#### sysctl.conf
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


#### syslog.conf
In alcune distro nominato **rsyslog.conf**, permette di gestire i log da memorizzare all'interno di [/var/log/dmesg](#dmesg).

#### ssh_config
#### ssh_config.d
#### sshd_config
#### sshd_config.d

#### wall
File con permessi **741** e [setgid](#setgid) abilitato.

### home/
Contiene i file e le impostazioni di ogni utente registrato nella macchina (ogni utente avrà una cartella personale).

#### user
Cartella dedicata allo specifico utente. Ogni utente ha una cartella (nominata con il nome dell'utente stesso) a lui dedicata.
##### .ssh
Cartella che contiene dati e informazioni utili (o necessarie) per l'utilizzo del protocollo [SSH](../Tecnologie/Protocolli#SSH). E' buona prassi memorizzare in questa cartella le chiavi RSA per l'autenticazione.

###### known_hosts
File che contiene la lista di tutti gli endpoint con cui è già stata effettuata con successo una connessione tramite CLI (known hosts, ovvero host conosciuti) e di cui è stata richiesta la memorizzazione (digitando "Y" durante la richiesta effettuata durante una connessione tramite comando [ssh](#ssh))

### lib/
#### system
##### systemd

### lib64/
### mnt/
Contiene i file che gestiscono i [Dischi ottici](<../Tecnologie/Macchina#Dischi ottici>). A volte può anche chiamarsi "**media/**"

### opt/
#### application
Contiene i file di applicazioni o software terzi.

### proc/
Contiene file che gestiscono l'accesso alle informazioni sui [processi](#Processo) in corso nella macchina, informazioni relative i supporti hardware installati e la configurazione del kernel in uso, tramite uno [*pseudo filesystem*](<#Pseudo filesystem>).

Alcuni comandi che fanno uso delle informazioni contenute in questa directory sono, ad esempio:
- top
- free
- cpuinfo
- mount
- umount

#### numeri/
Le cartelle nominate numericamente indicano i processi in corso all'interno della macchina. 

#### cmdline
File contenente le informazioni che vengono passate al kernel al suo avvio (come parametri della CLI e istruzioni speciali).

#### cpuinfo
#### loadavg
File che contiene informazioni relative all'utilizzo di risorse da parte del sistema negli ultimi 1, 5 e 15 minuti. Può essere consultato tramite comando ```uptime```
```Esempio
0.12 0.46 0.25 1/254 3052
```
	- 0.12: carico medio nell'ultimo minuto
	- 0.46: carico medio negli ultimi 5 minuti
	- 0.25: carico medio negli ultimi 15 minuti
	- 1: numero di processi attualmente in computazione nella CPU
	- 254: numero di processi attivi
	- 3052: ultimo PID eseguito dalla CPU

#### meminfo
File contenente informazioni relative l'uso della memoria del kernel.

#### modules
File contenente una lista di moduli attualmente caricati all'interno del kernel per fornire funzionalità aggiuntive.

### root/
### run/
### sbin/
Contiene file binari utilizzabili solo con permessi di accesso root. A parte questo dettaglio, ha metodologie di utilizzo simili a [bin/](#bin)

#### init
Contiene informazioni relative l'avvio del kernel Linux per [macchine virtuali](./Virtualizzazione).

### srv/
### sys/
Contiene informazioni (raccolte in [*pseudo filesystem*](<#Pseudo filesystem>)) relative ai dispositivi collegati alla macchina, anche noti come **file di sistema**.

#### Tipi di file di sistema
##### ext2
Era il sistema di default delle maggiori distro di Linux. Utile per dispositivi con memoria flash a causa dell’assenza di **journaling** (in quanto effettuare operazioni di scrittura riduce la vita della memoria), anche se lo supporta.

##### ext3
Evoluzione dell’ext2, aggiunge funzionalità di **journaling** per minimizzare il rischio di corruzione dei file in caso di arresto improvviso della macchina. La dimensione massima dei file ext3 è 32TB.

##### ext4
Evoluzione della ext3, implementa una serie di estensioni che ne migliorano le performance e aumentano la dimensione supportata dei file.

##### NFS
Il File di Sistema della Rete permette l’accesso ai file attraverso la rete. Dal punto di vista dell’utente non c’è differenza tra accedere ad informazioni ali terno di file memorizzato all’interno della macchina o all’esterno (purché siano dentro la stessa rete).

##### CDFS
Il File di Sistema per Dischi Compatti è stato creato appositamente per supportare l’utilizzo di dischi ottici

##### Swap file system
Lo Swap file system è utilizzato da Linux nel caso termini la RAM a disposizione. Permette di utilizzare una partizione momentanea per allocare i dati inutilizzati della RAM (essendo molto più lento della RAM, non sono da considerare come una soluzione definitiva)

##### HFS+
Linux include un module lo per la lettura e la scrittura su questo tipo di File System

##### APFS
File System sviluppato da Apple come diretta evoluzione dell’HFS+. Supporta funzionalità di cifratura ed è ottimizzato per memorie flash e dischi allo stato solido (SSD)

##### MBR
Il Registro Master di Avvio (Master Boot Record) è localizzato nella prima partizione del computer e contiene tutte le informazioni sull’organizzazione del File System. Una volta utilizzato, richiama una funzione di caricamento, che carica il sistema operativo.


#### Kernel
##### pid_max
Contiene informazioni relative il valore massimo accettato per l'identificativo di un [processo](#Processo) (PID) 

### tmp/
### usr/
#### doc/
Include file con documentazione ulteriore (utile per imparare o per gli amministratori di sistema  nel caso debbano imparare come impostare servizi software complessi)

#### lib/
Contiene i file di applicazioni o software terzi.

#### share/
Contiene i file di applicazioni o software terzi.

### var/
Il nome è un accorciamento di **variabile** e include file di tipo variabile (che possono essere modificati nel tempo, come i log). La cartella ha permessi ampi (777), rendendola potenzialmente dannosa nel caso di eliminazione accidentale (o volontaria) di file preesistenti.

#### log/
Contiene i file di [log](../cybersecurity#log). Genericamente Linux gestisce il logging con due metodologie diverse:
- **syslogd** + **klogd**: Metodo più vecchio, che utlizza questi due Daemon per gestire i log
- **rsyslogd**: Daemon più evoluto, che assolve i compiti dei due precedenti
- **journald**: Daemon recente (utilizzabile tramite [journalctl](#journalctl)) che permette di creare log come file testuali o in binario

I file in questa cartella vengono sovrascritti quotidianamente (rotazione) e il file precedente viene salvato aggiungendo la data del giorno nel quale sono stati registrati al nome.
	Prima di salvare il nuovo file messages, il precedente viene rinominato:
	messages -> messages-20181103

Prima di visualizzare un file di log, è sempre buona pratica [verificare il formato del file](#file), per assicurarsi che sia di tipo testuale e non binario (come avviene per i file [wtmp] e [btmp]) per evitare comportamenti strani in console.

E' spesso consigliabile memorizzare la directory dei log in una partizione separata da quella di sistema, per evitare il riempimento di spazio e il conseguente crash del sistema dovuto ad una quantità eccessiva di log.

##### auth.log
Contiene i log degli accessi al dispositivo per distro Debian e Ubuntu, con tutte le informazioni relative all’accesso.

##### boot.log
Contiene informazioni relative al boot e messaggi di log creati durante la fase di startup del dispositivo.

##### cron
File utilizzato per salvare i dati relativi le automazioni su Linux, come le loro pianificazioni, stati di esecuzione e messaggi di errore.

##### dmesg
Contiene messaggi del ring buffer del kernel, contenenti informazioni riguardanti i dispositivi hardware e i relativi drivers. È un file molto importante data la natura di basso livello delle informazioni (gli eventi avvengono prima che altri file di log entrino in azione).

Sebbene Linux non preveda un meccanismo di log per il kernel di default, è possibile configurarne uno tramite [/etc/syslog.conf](#syslog.conf)

##### kern.log
Contiene log relativi al kernel

##### journal
File che contiene i messaggi di log di [systemd-journald.service](#systemd-journald.service), configurabili tramite fil [/etc/journald.conf](#journald.conf)

##### maillog
File gestito dal Daemon dei messaggi mail, che contiene i log delle mail inviate e ricevute.

##### messages
Contiene log di attività generiche, utilizzate prevalentemente per il salvataggio di messaggi informativi non critici di sistema (nelle distro Debian è chiamato **syslog**)

##### mysql.log
Memorizzato come mysqld.log nelle distro RedHat, CentOS e Fedora, contiene i log del database MySQL, con informazioni relative I debug, funzionalità, messaggi di successo, il processo mysqld, e il daemon (un processo che viene eseguito senza bisogno di interazione da parte dell’utente) mysqld_safe.

##### secure
Variante di [auth.log](#auth.log) utilizzata da RedHat e CentOS. Tiene traccia anche di eventi che coinvolgono l’uso di sudo, login tramite SSH, e altri errori provenienti da SSSD.


##### Xorg.0.log
FIle che contiene i log del server [X Windows](<#X Window System>)

##### wtmp
File che contiene informazioni riguardo gli accessi e le uscite (logout) alla macchina dal suo ultimo avvio.

#### lib/
Contiene file di software o applicazioni terze.

### Altro
#### doc/
Cartella utilizzata per indicare la presenza di file di documentazione al suo interno.

#### info/
Cartella utilizzata per indicare la presenza di file di documentazione al suo interno.

#### man/
Cartella utilizzata per indicare la presenza di file di documentazione al suo interno.

### da smistare ---------------------------------------
#### nginx/
#### nginx.conf
Configura Nginx, un web server leggero per Linux.


#### ntp.conf
File di configurazione per il protocollo [NTP](../tecnologie/protocolli#ntp) (Network time protocol)

#### ppp
#### ip-down.d
#### ip-up.d

#### snort.conf
File di configurazione per l'[IDS](../Cybersecurity#IDS) (Software di Identificazione delle Intrusioni) Snort

# Software
L’estensione dei file di applicazioni su Linux dipende dalla distro:
- Arch Linux: pacman
- Debian: dpkg, apt
- Ubuntu: apt

Dove presente apt, il comando `apt-get` permette di scaricare app.

I software eseguibili su Linux sono divisi in 3 categorie, in base a determinate caratteristiche:
- **Applicazioni server**: software che non hanno bisogno di un monitor e di una tastiera per funzionare. Il loro scopo è interagire con altri dispositivi (chiamati **client**)
- **Applicazioni Desktop**: Utilizzano monitor e tastiere per operare. Esempi sono i Browser e editor di testo
- **Tool***: Software utilizzati per agevolare la gestione del sistema del computer. Di solito vengono utilizzati tramite **CLI**

## Processo
Un processo rappresenta una o più comandi di azioni che il kernel deve effettuare. Tutti i processi sono identificati tramite un identificativo numerico univoco (l'id 1 è assegnato al processo [*sbin/init*](#init) nel caso di sistemi virtualizzati, o [*bin/systemd*](#systemd) per i sistemi fisici, utilizzato per avviare il kernel), denominato PID. nel caso venga raggiunto il [limite massimo del PID](#pid_max), il sistema riparte il conteggio da 1, saltando i numeri dei processi attivi.

Nel caso un processo avvii un secondo processo, il primo prenderà il nome di processo genitore (parent process) ed il secondo il nome di processo figlio (child process). Se un processo è genitore, il suo identificativo verrà indicato come PPID, altrimenti semplicemente PID.


## Tool
### shell
Linux utilizza due tipologie di [shell](../Tecnologie/Macchina#Shell):
- Bash
- tcsh

#### Bash
E' la shell predefinita dei sistemi Linux.

#### tcsh
Shell che utilizza un linguaggio simile al C.

### Editor di testo
#### Vi
**NOTA**: Per i seguenti comandi ricordati che il tasto Shift rende un carattere maiuscolo, quindi:
- O = Shift + o

##### Navigazione

| Comando     | Descrizione                                |
| ----------- | ------------------------------------------ |
| j           | Sposta il cursore in alto di una riga      |
| k           | Sposta il cursore in basso di una riga     |
| l           | Sposta il cursore a destra di uno spazio   |
| h           | Sposta il cursore a sinistra di uno spazio |
| w           |                                            |
| e           |                                            |
| b           |                                            |
| $           |                                            |
| 0           | Muove il cursore a inizio linea            |
| \<numero\>G | Muove il cursore alla \<numero\> linea     |
| Shift+G     | Muove il cursore all'ultima linea          |

##### Modifica
Le operazioni di modifica possono essere effettuate solo se in **modalità comando**

| Comando      | Descrizione                                                     |
| ------------ | --------------------------------------------------------------- |
| J            | Unisce la riga selezionata con quella successiva                |
| ~            | Trasforma una lettera in minuscolo                              |
| dw           | Cancella la parola selezionata dal cursore                      |
| 2dw          | Cancella due parole                                             |
| u            | Cancella l'ultima operazione effettuata                         |
| \<numero\>u  | Cancella le ultime \<numero\> operazioni effettuate             |
| x            | Cancella il carattere dopo il cursore                           |
| \<numero\>x  | Cancella \<numero\> caratteri dopo il cursore                   |
| p            | Incolla l'ultimo elemento eliminato                             |
| P            | Incolla l'ultima parola copiata                                 |
| dd           | Cancella il rigo selezionato                                    |
| D            | Cancella la parte di riga a destra del cursore                  |
| \<numero\>dd | Cancella \<numero\> righe                                       |
| yk           | Copia la parola selezionata                                     |
| o            | Aggiunge una riga vuota dopo quella selezionata dal cursore     |
| O            | Aggiunge una riga vuota prima di quella selezionata dal cursore |
##### Operazioni
| Coma<br>ndo    | Descrizione                                             |
| -------------- | ------------------------------------------------------- |
| a              | Entra in modalità scrittura                             |
| i              | Entra in modalità scrittura                             |
| Esc            | Esci dalla modalità attuale e torna in modalità comando |
| :%s/\<testo\>/ | Effettua una ricerca del testo inserito nel documento   |
| \/\<testo\>    | Effettua una ricerca del testo inserito nel documento   |
| :e!            | Cancella tutte le modifiche                             |
| :q             | Esci                                                    |
| :q!            | Esci senza salvare                                      |
| :w             | Salva                                                   |
| :w!            | Scrive in sola lettura (se possibile)                   |
| :wq            | Salva ed esce                                           |
| :wq!           | Salva il file in sola lettura (se possibile) ed esce    |
| ZZ             | Salva il file ed esce                                   |
##### Ricerca
La modalità ricerca si attiva tramite ```/<testo>``` e permette di utilizzare i seguenti comandi:

| Comando     | Descrizione                                    |
| ----------- | ---------------------------------------------- |
| n           | Sposta il cursore verso il match successivo    |
| ?\<parola\> | Cerca la parola tra le parole sopra il cursore |
| cw          | Entra in modalità scrittura                    |

#### Vim
Versione avanzata di [Vi](#Vi) che permette l'utilizzo di funzionalità avanzate, come il key-binding.

| Comando            | Descrizione                                                  |
| ------------------ | ------------------------------------------------------------ |
| Esc                | Esce dalla modalità scrittura e permette di eseguire comandi |
| :x                 | Salva ed esci                                                |
| :wq                | Salva ed esci                                                |
| :q                 | Esci (se il file non è stato modificato)                     |
| :q!                | Esci senza salvare                                           |

##### Movimenti

| Comando            | Movimento                  |
| ------------------ | -------------------------- |
| j                  | In basso di una linea      |
| freccia in basso   | In basso di una linea      |
| Invio              | In basso di una linea      |
| k                  | In alto di una linea       |
| freccia in alto    | In alto di una linea       |
| h                  | A sinistra di un carattere |
| Backspace          | A sinistra di un carattere |
| freccia a sinistra | A sinistra di un carattere |
| l                  | A destra di un carattere   |
| Spazio             | A destra di un carattere   |
| freccia a destra   | A destra di un carattere   |
| 0                  | Ad inizio linea            |
| $                  | A fine linea               |
| w                  | A inizio parola successiva |
| b                  | A inizio parola precedente |
| :0 + Invio         | Alla prima riga del file   |

#### Emacs
#### Nano
Software Open Source che utilizza come base di partenza [Pico](#Pico).

#### Pico
Software proprietario.

### Package manager
Il software predefinito dal sistema per gestire i pacchetti dipende dalla distro che si sta utilizzando.

#### dpkg
E' il tool di più basso livello per la gestione dei pacchetti. [link](https://phoenixnap.com/kb/fix-sub-process-usr-bin-dpkg-returned-error-code-1)
##### Riconfigurare il database dpkg
```sh
dpkg --configure -a
```

#### apt-get
Software front-end di supporto per il [dpkg](#dpkg) che ne semplifica l'utilizzo.

#### RPM
E' un software che incarna il set di standard definito dalla Linux Foundation per permettere una più agevole compatibilità tra le varie distro. Utilizza il formato ```.rpm``` per definire un pacchetto e l'omologo comando di backend
```sh
rpm
```
per la gestione dei pacchetti. e delle relative dependency (pacchetti esterni che ne permettono il funzionamento).

E' possibile utilizzarlo tramite comandi lato front-end che ne automatizzano il processo come
```sh
yum
```
```sh
up2date
```

#### libzypp
#### Yumex
#### Gnome PackageKit

### Password manager
#### KeePassX

### chrootkit
Software basato su Linux che permette di controllare la presenza di noti [rootkit](../cybersecurity#rootkit) all’interno della macchina. Utilizza comandi shell (prevalentemente strings, grep e ps)

### gparted
Gestisce le [schede di memoria](<../Tecnologie/Macchina#Hard drive>) memorizzate all'interno della cartella [dev/](#dev/) e le relative partizioni.

### iptables
[Firewall](../cybersecurity#firewall) disponibile per Linux, che consente all'amministratore di sistema di configurare le regole di accesso, incorporandole nei moduli Netfilter del kernel di Linux.

### inetsim
Simulatore di ambienti (virtualizzatore), con opzioni di verifica per i protocolli:

### nftables
Diretta evoluzione di [iptables](#iptables), che utilizza una semplice macchine virtuale nel kernel di Linux. Utilizza la macchina virtuale per analizzare i pacchetti di rete ed implementare regole decisionali di accettazione e inoltro dei pacchetti.

### steghide
Tool utilizzabile per effettuare la steganografia di dati, ovvero l'inserimento dei dati all'interno di un altro file, per renderli difficilmente raggiungibili.

### TCP Wrappers
Tool di [controllo degli accessi](#acl) basato su regole e sistema di logging. Opera attraverso il filtraggio dei pacchetti basato sugli indirizzi IP e i servizi di rete.

### ufw
Firewall semplificato. [Guida](https://help.ubuntu.com/community/UFW)

## Probe
Un *probe* (sonda) è un elemento utilizzato per raccogliere dati di un'infrastruttura hardware e restituirli in forma digitale.

### Linux::inodes

## Package-manager
### apt
E' il gestore dei pacchetti di default di Linux

## Applicazioni Server
### [Apache](Apache.md)
### [NGINX](../Software/NGINX)
### Private Cloud Server
#### ownCloud
Progetto lanciato nel 2016 per fornire un software che permetta di memorizzare, sincronizzare e condividere dati con un [Cloud Server Privato](<../Server/Cloud#Private Cloud>). Il progetto è disponibile in Open Source con licenza GNU AGPLv3 o in versione Enterprise.

#### NextCloud
Progetto parallelo a [ownCloud](#ownCloud) (creato dallo steso sviluppatore, partendo dallo stesso software), punta ad un processo di sviluppo totalmente aperto e trasparente

### Database Server
I database servono per velocizzare la memorizzazione e la manipolazione dei dati tramite l'utilizzo di linguaggio a richieste strutturate (Structured Query Language, **SQL**). I database server più utilizzati sono

#### [MariaDB](../Database/MariaDB)
#### [MySQL](../Database/MySQL)
#### [PostgreSQL](../Database/Postgre)

### Mail Server
Sono composti dall'unione di 3 servizi:

#### MTA
Agent di trasferimento delle mail.

##### Sendmail
##### Postfix
Software più semplice e sicuro di [Sendmail](#Sendmail)

#### MDA
L'Agente di consegna locale (Mail Delivery Agent, MDA) si occupa di salvare le mail nella mailbox dell'utente. Di solito è il nodo finale del [MTA](#MTA)

#### POP/IMAP Server
Permette la comunicazione tramite protocolli [POP](../Tecnologie/Protocolli#POP3) e [IMAP](../Tecnololgie/Protocolli#IMAP)

##### Dovecot
##### Cyrus IMAP
##### Microsoft Exchange
Software proprietario di Microsoft.

### Condivisione di File
Esistono software specifici per condividere file tra ambienti diversi. In particolare è da considerare il protocollo di condivisione utilizzato dall'altro computer. I più utilizzati sono i l[DNS](../Tecnologie/Protocolli#DNS) e il [LDAP](../Tecnologie/Protocolli#LDAP). In entrambi i casi, il dispositivo Linux deve avere abilitato al protocollo [DHCP](../Tecnologie/Protocolli#DHCP), in quanto entrambi i protocolli precedentemente nominati utilizzano l'identificazione dei dispositivi tramite [IP](../Tecnologie/Protocolli#IP).

#### Samba
E' il software più utilizzato per permettere la condivisione file all'interno del dominio [Windows](./Windows)

#### Netatalk
Permette di comunicare all'interno del dominio [Apple](<./MAC OS X>).

## Applicazioni Desktop
### Desktop
#### X Window System
Interfaccia grafica basica per Sistemi operativi Linux. Da accoppiare ad altri software per la gestione dele finestre, come:
- KDE Windows manager
- Gnome Windows Manager (usata da Ubuntu)

### Mail
#### Thunderbird
Software di email client della Mozilla Foundation. Si collega ad un [server POP/IMAP](<#POP/IMAP Server>) e permette di visualizzare i dati in esso contenuti e di inviare mail tramite un server SMTP.

#### Evolution
#### KMail

### Creatività
#### Blender
Permette di manipolare elementi 3D

#### GIMP
Permette di manipolare elementi grafici in 2D

#### Audacity
Permette di manipolare file audio

### Produttività
#### LibreOffice
Suite di software per la creazione, modifica di gestione di elaborati testuali, presentazioni, fogli di calcolo, creata partendo da [OpenOffice](#OpenOffice).

#### OpenOffice
Suite di software per la creazione, modifica di gestione di elaborati testuali, presentazioni, fogli di calcolo.

### Browser
#### Firefox
#### Chrome

## Suite
### [Security Onion](https://securityonionsolutions.com/)
È una suite open source pure il monitoraggio della sicurezza della rete (NSM), formata da 3 funzioni alla base:
- Cattura dell’intero pacchetto e della tipologia di dati
- [[#IDS]] (sia network che host-based)
- Analizzatore degli [alert](../Cybersecurity#Alert)

![[security_onion.png]]

#### Data
##### PCAPS
##### Contenuti
##### Transazioni
##### Sessione
##### Log degli host
##### Alert
##### Syslog
Sistema di Daemon per la gestione dei file di [log](#log). Generalmente i sistemi Linux possono avere diversi metodi di logging.

##### Metadata

#### Identificazione
##### CapME
Web app che permette di visualizzare facilmente tutte le comunicazioni che avvengono nella rete al [livello 4](../tecnologie/protocolli#trasporto). I dati sono ottenuti tramite [[#Zeek]] o [tcpflow].
Il tool può essere utilizzato come plugin di [[#ELSA]] (Enterprise Log Search and Archive). In questo caso, i file possono essere visualizzati tramite [[#Wireshark]]

##### Snort
E' un [NIDS](../Cybersecurity#NIDS) (sistema di identificazione delle intrusioni di rete) utilizzato per creare dati di tipo [alert](../cybersecurity#alert) tramite l'implementazione di regole e firme. E' anche possibile scaricare automaticamente nuove regole tramite [[#PulledPork]] (entrambi sponsorizzati da Cisco).

Le regole sono strutturate in due componenti: header e opzioni.

| Componente     | Esempio                                                                                                        | Spiegazione                                                                                                            |
| -------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Header         | alert ip any any -> any any<br><azione> <protocollo> <IP_mitt> <porta_mitt> <direzione> <IP_dest> <porta_dest> | Azione da eseguire, indirizzi e porte di mittente e destinatario e direzione del traffico                              |
| Opzioni        | (msg:"GPL ATTACK_RESPONSE ID CHECK RETURNED ROOT";...)                                                         | Messaggio da mostrare con dettagli relativi il contenuto del pacchetto, tipo di alert, ID della fonte e altri dettagli |
| Localizzazione | /nsm/server_data/securityonion/rules/...                                                                       | Localizzazione del file della regola                                                                                   |

###### Header
Ogni dato è configurabile anche tramite l'utilizzo di variabili, configurabili sul file **snort.conf**:

| Variabile     | Utilizzo        |
| ------------- | --------------- |
| $HOME_NET     | Rete analizzata |
| $EXTERNAL_NET | Rete esterna    |

###### Opzioni
I messaggi inviati da Snort possono essere creati in base a regole. Le regole più comunemente utilizzate sono:
- **GPL**: Regola datata, creata da SourceFire
- **ET**: Emerging Threats (collezione di regole categorizzate, raccolte da risorse multiple Open Source sotto licenza BSD)
- **VRT**: Set di regole mantenute da Cisco Talos

| Componente | Spiegazione                                                |
| ---------- | ---------------------------------------------------------- |
| msg:       | Descrizione dell'alert                                     |
| content:   | Contenuto del pacchetto.                                   |
| reference: | URL che porta a maggior informazioni riguardanti la regola |
| classtype: | Categoria dell'attacco (Snort fornice un pool di default)  |
| sid:       | ID della regola                                            |
| rev:       | Revisione della regola rappresentata dal SID               |

##### Suricata
E' un [NIDS](../Cybersecurity#NIDS) signature-based (basato sulla firma) utilizzato anche per prevenzione di intrusioni inline. Sebbene sia simile a [[#Zeek]], Suricata utilizza il [Multithread] nativo, che permette l'utilizzo contemporaneo di più core del processore e include funzioni di blocco in base alla reputazione e supporto al multithread tramite GPU per aumentarne le performance.

##### Zeek
Precedentemente chiamato Bro, Zeek è un [NIDS](../Cybersecurity#NIDS) che utilizza un [approccio comportamentale](../Cybersecurity#Behavior-based) basato sulle policy (sotto forma di script che determinano quali dati avviano i log, quali gli alert, ecc.). Zeek permette anche di memorizzare file per una successiva analisi di malware, bloccare l'accesso a siti malevoli e spegnere in computer nel caso stia violando delle policy.

##### OSSEC
E' un [HIDS](../Cybersecurity#HIDS) che permette di monitorare le attività dell'host e effettuare analisi di integrità, monitoraggio (di log e [processi](#Processo) del sistema) e identificazione di [rotkit](../Cybersecurity#Rootkit)

##### Wazuh
E' un [HIDS](../Cybersecurity#HIDS) che aggiorna il precedentemente tool ([[#OSSEC]]), grazie all'integrazione di meccanismi di protezione dell'endpoint, incluso un sistema di analisi dei logfile dell'host, identificazione delle vulnerabilità, analisi delle configurazioni e incident response.
Richiede l'esecuzione di [agent] nella rete dell'host.

#### Analisi

##### Wireshark
Tool che permette di monitorare i pacchetti inviati e ricevuti da un host.

##### Sguil
Fornisce una console di alto livello per monitorare gli alert provenienti da varie fonti. Viene utilizzato come punto di partenza per l'analisi degli alert di sicurezza tramite l'appoggio di Sguil (che fornisce i dati) ad altri tool.

##### Kibana
Dashboard interattiva di [Elasticsearch](https://www.elastic.co/) data. Permette l'interrogazione dei dai [NSM] (Monitoraggio della Sicurezza della Rete) e la visualizzazione dei dati flessibile. E' possibile ottenere i dati da interrogare tramite [[#Sguil]] e visualizzarli, ad esempio, filtrandoli per IP associato ad un alert.


## Red team
### John the ripper
[John the ripper]( https://www.kali.org/tools/john/) è un tool utilizzato per effettuare un brute force delle password di un computer.

# Annotazioni
- Le distribuzioni di Linux, di solito, vengono provviste nativamente di [[Python]]

## Script utili
##### Copiare un file da locale a remoto tramite ssh
```sh
tar -c dir/ | gzip | gpg -c | ssh user@remote 'dd of=dir.tar.gz.gpg'
```
```sh
scp [-P <SERVER_PORT>] <PATH_TO_LOCAL_FILE> <SERVER_USER@SERVER_IP_ADDRESS>:<PATH_TO_DESTINATION_FOLDER>
```