OpenServerless è un tool utilizzato per creare e gestire le comunicazioni con gli agenti AI in maniera serverless. Utilizza le API di [OpenAI](./OpenAI.md)
# CLI
[Documentazione](https://openserverless.apache.org/docs/cli/)
Il comando che permette di utilizzare OpenServerless da cli è:
```sh
ops <entità> <comando> <parametri> <flags>
```
	PARAMETRI
	--web
	--param <nome> <valore> # permette di utilizzare il valore indicato per il parametro (es. --param AUTH $AUTH)

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
	- loader # converte file .pdf in file .txt

### ide
```sh
ops ide
```
	COMANDI:
	- clean # pulisce i file temporaei
	- deploy # impacchetta ed effettua il deploy delle actions (o di quella indicata)
	- devel # modalità di sviluppo incrementale
	- login # effettua il login ad un'istanza openserverless
	

#### deploy
- Funziona attorno a **ops action** e **ops packages**
- Crea dei packages per le actions, includento i parametri inclusi nei file di configurazione
- Nel caso ci siano azioni da più file, verrà creato un singolo file **.zip**
- Risolve le dependencies **requirements.txt** e **package.json**
- Estrae gli **args** dai **parametri** immessi nella CLI 
- Al momento funziona solo con Python, PHP e Node

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

#### Visualizzare AUTH
```sh
!ops -config -dump | grep AUTH
```
	RISPOSTA: AUTH=<uid>:<secret>


# Auth
Le azioni avvengono tramite azioni web (deploy effettuato con flag web=True), quindi di default non sono autenticate.

# Form
OpenServerless permette anche di gestire i Form tramite l'uso di array degli input, rappresentati come oggetti:
```input
{
	"name": string,
	"label": string,
	"type": string,
	"required": "string"
}
```

Per indicare la presenza di un **Form** nella *request*, basta inserire il form come dictionary come input.
Tutti i campi verranno letti ed organizzati per comporre un prompt, da inviare all'*agent*.

# Display
La funzione display è modificabile tramite richiesta, inserendo nel campo **input** la visualizzazione desiderata.

# Chat
Per l'utilizzo di componenti chat, OpenServerless segue la struttura delle API di [OpenAI](./OpenAI) per interagire con i modelli di AI. I modelli a disposizione sono:
- Ollama

## endpoint
```chat
https://<url>/api/chat
```

## Immagini

### Invio
```python
 msg = {
	"model": 'llama3.2-vision:11b', #Esempio
	"messages": [
		{
			"role": "user",
			"content": "what is in this image?",
			"images": [img] # Inserire l'immagine
		}
	]
 }
```

#### Form
E' anche possibile inviare l'immagine tramite [Form](#Form):
```python
FORM = [
	{ 
		"label": "any pics?", 
		"name": "pic", 
		"required": "true", 
		"type": "file" 
	}
]
```

### Risposta
Per visualizzare solo la prima riga della risposta:
```python
# get vision answer 
streamed lines = req.post(url, json=msg, stream=True).iter_lines() next(lines) # showing one line
```

Altrimenti si usa lo [Stream]:
```python
for line in lines:
	chunk = json.loads(line.decode("UTF-8")) 
	print(chunk.get("message", {}).get("content", ""), end='')
```

#### Form
```python
if type(inp) is dict and "form" in inp: 
	img = inp
		.get("form", {})
		.get("pic", "")
```

Il risultato sarà un URL, visualizzabile all'interno della propria pagina web tramite tag ```
<img />```. [Pinocchio](./MastroGPT) permette di visualizzare l'immagine tramite la funzionalità Display


# Milvus
Database vettoriale [NoSQL], che permette di trasformare dati di tipo testuale in valori numerici (**embedding**) ed effettua ricerche tramite comparazione ed identificazione di similarità tra le rappresentazioni numeriche dei vari valori memorizzati.
Si basa su **database** multipli, ognuno dei quali ha più **collezioni** (ognuna delle quali a sua volta provvista di **schema** e **indici**).

Essendo i dati elaborati unicamente in forma numerica e che il Database permette di memorizzare anche dati di tipo file, è utile ricordare che, in tal caso, è necessario utilizzare librerie di terzi per la manipolazione dei dati e la loro trasformazione in formato numerico. Tra le libreri più utilizzate ci sono:
- [pymupdf](../../Linguaggi/Python.md#pymupdf) per la gestione di file di tipo PDF
- [nltk](../../Linguaggi/Python.md#nltk) per dividere le stringhe delle varie pagine in frasi

## Login
Per effettuare il login, OpenServerles ha bisogno di 3 parametri:
```.env
MILVUS_HOST=
MILVUS_TOKEN=
MILVUS_DB_NAME=
```

```milvus.py
import os from pymilvus
import MilvusClient

uri = f "http://{ os.getenv("MILVUS_HOST")}" 
token = os.getenv("MILVUS_TOKEN")
db_name = os.getenv("MILVUS_DB_NAME") 
client = MilvusClient(uri=uri, token=token, db_name=db_name)
```

## collection

### creazione
```collection.py
client.create_collection(
	collection_name=COLLECTION,
	schema=schema, 
	index_params=index_params
)
```


### eliminazione
```python
client.drop_collection("<nome collection>")
```

## schema
#### Creazione
```python
from pymilvus import DataType 

COLLECTION = "test"
DIMENSION=1 024
```

#### Definizione
```python
schema = client.create_schema() 
schema.add_field(
	field_name="id", 
	datatype=DataType.INT64, 
	is_primary=True, 
	auto_id=True
) 
schema.add_field(
	field_name="text", 
	datatype=DataType.VARCHAR, 
	max_length=DIMENSION
) 
schema.add_field(
	field_name="embeddings", 
	datatype=DataType.FLOAT_VECTOR, 
	dim=DIMENSION
)
```
	PROPRIETA' DEI CAMPI:
	- field_name*: string
	- datatype*: DataType

## index

```index.py
index_params = client.prepare_index_params() 
index_params.add_index("embeddings", index_type="AUTOINDEX", metric_type="IP")
```


## record
### Inserimento
Per inserire valori all'interno dello schema
```python
text = "Hello World" vec = [float(i) for i in range(0,DIMENSION)] # TO BE REPLACED with embedding 
client.insert(COLLECTION, {"text":text, "embeddings": vec})
```

### Ricerca
```Python
qit = client.query_iterator(
	collection_name=COLLECTION, 
	batchSize=2, 
	output_fields=["text"]
) 
res = qit.next()
print(res[0].get("text"))
```

### Embedding
L'embedding serve a manipolare i dati di tipo stringa e tramutarli in valori di tipo numerico. Per effettuare l'embedding dei dati, prima del loro inserimento nel database, si utilizza un modello di embedding
```Python
import sys, requests as req 
MODEL="mxbai-embed-large:latest" 
DIMENSION=1024
```

#### Invocazione delle API
```Python
inp = "Hello World" 
url = f"https://{os.getenv("AUTH")}@{os.getenv("OLLAMA_HOST")}/api/embeddings" 
msg = { "model": MODEL, "prompt": inp, "stream": False }
res = req.post(url, json=msg).json() 
out = res.get('embedding', [])
```


## VectorDB
E' la classe che permette di interagire con [Milvus](#Milvus).

```Python
db = vdb.VectorDB
db {
	__init__
	insert(string)
	embed(string)
}
```
### Inizializzazione
```Python
!code packages/vdb/load/vdb.py # da usare nel caso si operi tramite bash Pyhton
import sys ; sys.path.append("packages/vdb/load") 
import vdb

db = vdb.VectorDB({})
```

### Ricerca
Per instanziare una ricerca tramite client:
```Python
cur = client.search(collection_name=COLLECTION, # collection
search_params={"metric_type": "IP"}, # how to measure distance 
anns_field="embeddings", # where to search 
data=[vec], # what to search 
output_fields=["text"] # field to return
)
```

Verrà ritornato un array di vettori, ordinato in base alla distanza.
```Python
for item in cur[0]:
	dist = item.get('distance', 0) 
	text = item.get("entity", {}).get("text", "") 
	print(dist, text)
```
	RISPOSTA EEMPIO:
		271.28466796875 Testing 
		252.88241577148438 This is a test
		231.84872436523438 This is another test
		181.67494201660156 Hello World

## CLI
Milvus è utilizzabile anche da CLI (Python) tramite
```Python cli
!ops invoke vdb/load input="openserverless cli import" !ops invoke vdb/load input="*openserverless" | jq .body.output
```

## Esempi
```python
client.list_collections() # ritorna la lista degli elementi
client.drop_collection("test") # elimina la collection
```


# Storage
OpenServerless include delle API per connettersi ai servizi in cloud [S3](../../Server/AWS#S3), comprendenti funzionalità di connessione e CRUD. Vengono forniti 3 diversi endpoint:
- **Deployment interno**: gestito tramite variabili **http**, **S3_HOST** e **S3_PORT**, fornite al momento del login
- **Testing interno**:  gestito tramite variabili **http**, **S3_HOST** e **S3_PORT** presenti nel file **.env**
- **Deployment esterno**: gestito tramite variabile
	```sh
	API_S3_URL=https://s3.openserverless.dev
	```

## Python
### Conenssione
```python
import os 
args = {} # endpoint 
host = args.get(
	"S3_HOST", 
	os.getenv("S3_HOST")
)

port = args.get(
	"S3_PORT", 
	os.getenv("S3_PORT")
) 

url = f"http://{host}:{port}" # bucket 
bucket = args.get(
	"S3_BUCKET_DATA", 
	os.getenv("S3_BUCKET_DATA")
)

external_url = args.get("S3_API_URL") # not available in test
```

Connettersi al client
```python
import os, boto3, base64, pathlib
key = args.get("S3_ACCESS_KEY", os.getenv("S3_ACCESS_KEY")) 
sec = args.get("S3_SECRET_KEY", os.getenv("S3_SECRET_KEY")) 
client = boto3.client('s3',
	endpoint_url=url, 
	region_name='us-east-1', 
	aws_access_key_id=key, 
	aws_secret_access_key=sec
)
```

### Lista degli elementi
```python
res = client.list_objects_v2(Bucket=bucket) 
'Contents' in res
res['Contents']
```

### Lettura
```python
res = client.get_object(Bucket=bucket, Key='cat.jpg') 
data = res["Body"].read()
```

#### Creazione di un link temporaneo
```python
client.put_object(Bucket=bucket, Key="cat.jpg", Body=body) 
url = client.generate_presigned_url(
	'get_object',
	Params={'Bucket': bucket, 'Key': 'cat.jpg'}, 
	ExpiresIn=3600
)
```

#### Esporre l'URL all'esterno
```python
external_url = "https://openserverless.dev" 
from urllib.parse import urlparse, urlunparse
old = urlparse(url) ; 
pref = urlparse(external_url) 
url = urlunparse((
	pref.scheme, 
	pref.netloc,
	old.path, 
	old.params, 
	old.query, 
	old.fragment
))
```

### Scrittura
```python
body = pathlib.Path("tests/vision/cat.jpg").read_bytes()
client.put_object(Bucket=bucket, Key="cat.jpg", Body=body)
```

### Eliminazione
```python
client.delete_object(Bucket=bucket, Key='cat.jpg')
```

# Best practices
