# Introduzione

## Classe
La classe è un elemento base per la programmazione ad oggetti in Python.
### costruttore
Contiene i metodi e l'assegnazione di variabili che devono essere attivati durante la creazione dell'oggetto
```__init__.py

```

# Framework
## [[Django]]
Django è un #framework #back-end 

# Librerie
## LangChain

## nltk
Utile per gestire grandi moli di dati di tipo stringa (ad esempio una pagina di un libro), per segmentare il dato in parti di dimensione ridotta

### Dividere un testo in frasi
```Python
import nltk.data nltk.download('punkt') # load tokenizer model 
from nltk.tokenize import sent_tokenize

sentences = sent_tokenize(text) # extract sentences from text 
enum = enumerate(sentences, 1) # enumerate
sent = next(enum) # extract one sentence
```

## numpy
## pandas

## pymupdf
Libreria che permette di gestire i file PDF.

### Estrarre testo da una pagina
```Python
import pymupdf doc = pymupdf.open("lessons/bitcoin.pdf") # load a file 
text = doc[0].get_text() # extract text from page
```

## tkinter
Libreria per realizzare GUI (Interfacce Utente Grafiche) con Python.

## pynput

## redis
redis è la libreria ufficiale per interagire con [Redis](../Software/Redis.md).