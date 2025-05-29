# CLI

# API
Le API si basano sui seguenti parametri:
- MODEL (default: llama3.1:8b)
- url (default https://nomeutente)
- inp (input)

# Architettura
MastroGPT utilizza Python per gestire i vari modelli utilizzati.

```modello
|- __main__.py
|- modello.py
```

Su \_\_main\_\_.py è possibile aggiungere dei [parametri](OpenServerless.md#CLI) personalizzati, che verranno utilizzati di default

## .env
Il file include le variabili d'ambiente. E' possibile utilizzare più file .env (es. .env, .env.packages, .env.test) per

```.env
AUTH=
OLLAMA_HOST=
```

# hello
Set di servizi preimpostati da Nuvolaris

```esempio.py
#--kind python:default
#--web true
  
import stateless

def main(args):

  return { "body": stateless.stateless(args) }
```

In questo caso, una volta caricata l'action "esempio", essa verrà eseguita con parametro **args** uguale a:

```args
{
	kind: python:default
	web: True 
}
```

## Stream
servizi di risposta tramite stream (per gestire grandi quantità di dati). La funzione di stream utilizza un **socket**, identificato dai parametri
- **STREAM_HOST**, che di solito non varia
- **STREAM_PORT**, varia in continuazione

### streamock
Oggetto utilizzato per effettuare test in locale per comunicazioni stream con l'IA.
Ha un metodo di arresto automatico dopo i 5 secondi di inutilizzo.

## Redis
Permette di interagire con Redis

## S3
Permette di interagire con i servizi S3 di Nuvolaris

## Milvus
Database vettoriale per conservare file. Permette di ottenere risultati con l'indice di corrispondenza con la query indicata

- **\*** Permette di effettuare una ricerca


# Display
Funzionalità di supporto di MastroGPT, che permette di visualizzare i messaggi inviati e ricevuti con l'agent AI.

## Immagini
E' possibile visualizzare i file di tipo immagine tramite
```python
res['html'] = f'<img src="data:image/png;base64,{img}">'
```

Un esempio di lettura di un'immagine è:
```python
if type(inp) is dict and "form" in inp:
    img = inp.get("form", {}).get("pic", "")
    vis = vision.Vision(args)
    out = vis.decode(img)
    res['html'] = f'<img src="data:image/png;base64,{img}">'
```
# Auth
MastroGPT possiede un meccanismo predefinito di autenticazione tramite API_KEY.
Il login alla piattaforma crea nella directory principale il file "~/.wskprops" e inserisce la chiave al suo interno. Per caricare la chiave nella bash ed utilizzarla per inviare comandi tramite CLI tramite:
```sh
source ~/.wskprops # carico l'API_KEY
VAR=$(ops url <azione> | tail +2) # estraggo l'URL
FLAGS="blocking=true&result=true" # salvo i flag
curl -u $AUTH -X POST "$URL?$FLAGS" # Post di autenticazione
```

Le azioni vengono impostate aventi **--web=False** e quindi protette. Questo però non è possibile nel caso si utilizzi un'interfaccia web come Pinocchio.
Pinocchio è già predisposto con un meccanismo di login.

# Best practices
- Utilizza sempre i parametri per utilizzare variabili d'ambiente
	```
	host = args.get("OLLAMA_HOST", os.getenv("OLLAMA_HOST"))
	```
- Aggiungi i parametri su **\_\_main\_\_.py**
- Passa gli argomenti nel deploy dell'action tramite:
	```sh
	ops ide deploy [action] 
	```
