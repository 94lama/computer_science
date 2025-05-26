# CLI
[Documentazione](https://openserverless.apache.org/docs/cli/)
Il comando che permette di utilizzare OpenServerless da cli è:
```sh
ops <entità> <comando> <parametri> <flags>
```

## Entità
Le entità disponibili sono:
- action
- invoke
- url

## Azioni
### action
```sh
ops action
```
	COMANDI:
	- list # ritorna la lista delle azioni disponibili
	- create
	- delete

### ai
Utilizza il plugin OpsAI
```sh
ops ai
```
	COMANDI:
	- chat <modello> # apre una chat con l'AI da CLI
	- cli # apre Python REPL
	- lesson # scarica le lezioni e le soluzioni
	- new # crea un nuovo servizio
	- user # aggiorna gli utenti

### ide
```sh
ops ide
```
	COMANDI:
	- clean # pulisce i file temporaei
	- deploy # impacchetta ed effettua il deploy delle actions (o di quella indicata)
	- devel # modalità di sviluppo incrementale
	- login # effettua il login ad un'istanza openserverless

### invoke
```sh
ops ide
```
	COMANDI:
	- reverse

## Esempi
#### Modifica password
```sh
ops ai user update <username>
```
	INFO: Ti chiederà di modificare la password dell'utente

#### CRUD di un'azione
Se si vuole
1. creare un'azione chiamata "reverse"
2. invocare utilizzando un input = "hello" e ricevendo il risultato
3. 
4. eliminare l'azione
```sh
ops action list ops action create reverse lessons/reverse.py ops invoke reverse ops invoke reverse input=hello ops url reverse curl https://openserverless.dev/api/v1/namespaces/msciab/actions/reverse ops action update reverse lessons/reverse.py --web true ops url reverse curl https://openserverless.dev/api/v1/web/msciab/default/reverse curl "https://openserverless.dev/api/v1/web/msciab/default/reverse?input=hello" ops action delete reverse ops action list
```