# Introduzione
Docker è un software che permette di creare un #container nel quale far girare applicazioni.
**![A screenshot of a video game Description automatically generated](https://lh7-us.googleusercontent.com/M8aRJW9e8qJBF7GlTi6OlcUWlsVX1iE5uR7A7BXjU0jEUb5QFgepZojSnscJEHVi28c8_lbrFx07TYwQbZwkaeLtfZQwNu_KCg-ypRJQCUL8xyMUVUffodEQlnCjRuNlizywqPJqpqUt2tJ25lh78A=s2048)**


Una problematica di Docker è l'assenza di meccanismi di gestione e controllo delle risorse utilizzate (a differenza dei meccanismi di [[Virtualizzazione]] convenzionali). Per questo, di solito Docker si accoppia con [[Kubernetes]]
## ALM
L' #alm, o Application Lifecycle Management rappresenta tutti i processi 
**![](https://lh7-us.googleusercontent.com/tB1J4EOxXgNBNC9eI7uwd-HVolNAZWlLhBDomq4jyZkd-dG7LzGCOygfZ4VqTlFrzOH9QkqeN3s4-_ApI8i2AP-kiqNZkK9OPYt-k9iBGQPX0RjPx2TDwKATdJjejsPkg_7-R9joYvPuy_yPRjOhQA=s2048)**
# Struttura
Docker viene gestito da file, che ne definiscono le caratteristiche della macchina da virtualizzare o dei software da eseguire al suo interno.
## Makefile
E' un file che gestisce le operazioni da effettuare all'apertura o a seguito di richiesta tramite bash
```Docker
MAKEFILE

operazione_1:
	@echo "inizia"
operazione_2:
	mkdir "cartella"
```
```SHELL
make operazione_1 #risultato: "inizia"

```
## Dockerfile
Dockerfile è un file che contiene le istruzioni da eseguire per eseguire un immagine Docker. Una guida dei principali comandi da utilizzare può essere trovata [qua](https://docs.docker.com/build/concepts/dockerfile/).

```DOCKERFILE
FROM <>
CMD [<comando_1>, <comando_2>, ..., <comando_n>]
```
### Inizializzazione
```bash
docker build <percorso>
docker run 
```

### Multi-stage
E' possibile creare delle build tramite stage diversi (es. installazione dei packages, build, deploy), in modo da facilitarne la lettura durante il loro sviluppo. Un esempio è il seguente:
```Dockerfile
FROM alpine:latest AS builder
RUN apk --no-cache add build-base

FROM builder AS build1
COPY source1.cpp source.cpp
RUN g++ -o /binary source.cpp

FROM builder AS build2
COPY source2.cpp source.cpp
RUN g++ -o /binary source.cpp
```
	FONTE: https://docs.docker.com/build/building/multi-stage/

Nel caso si utilizzi questo metodo, è possibile anche indicare il livello della build a cui fermarsi (utile nel caso di test), utilizzando l'opzione "--target" durante la build del container
```sh
docker build --target build -t hello .
```
## docker-compose.yml
Il [docker-compose.yaml](https://docs.docker.com/compose/) è un file che contiene i dati necessari (incluse #porte, localizzazioni di [database](../Database/Database), dati di accesso, ecc.) per creare l'ambiente su cui far partire il programma.
**NB. in environment non inserire le porte, perché renderebbe il container vulnerabile**.
## Container
## Image
## Volume
E' la componente correlata al [[Database]] e ne permette il collegamento con i [[#Container]]
# Comandi

#### Mostrare i processi in esecuzione
```Docker
docker ps
```

#### Arrestare il container selezionato
```Docker
docker stop <nome_container o id_container>
```

#### Eseguire in container selezionato nella porta selezionata
```Docker
docker run <nome_container> o <id_container> <comando>
```
	OPZIONI
	-i #
	-t #

#### Inizializzare un processo nel #kernel [[Linux]]
```Docker
docker run -it ubuntu bash
```

#### Collegare il terminale al #container già avviato 
```Docker
docker exec -it <id container> <comando>
```
	OPZIONI
	-i #
	-t #
	
	ESEMPI
	- Aprire il terminale del container
		docker exec -it <nome_container> 

# [[Cybersecurity]]