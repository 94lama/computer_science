[Documentazione](https://kubernetes.io/it/docs/concepts/overview/what-is-kubernetes/) 
# Introduzione
Kubernetes è un software per l'orchestrazione, la gestione e il coordinamento automatico dei sistemi informativi, di solito utilizzato come complementare di [Docker.
Funziona tramite [#Kubelet] e [#Pod]

## Installazione
### Linux
La lista dei comandi è disponibile [sul sito ufficiale](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management):

1. Aggiornamento dei packages
	```sh
	sudo apt-get update
	# apt-transport-https may be a dummy package; if so, you can skip that package
	sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
	```
2. Download della chiave pubblica per scaricare il pacchetto Kubernetes
	```sh
	# If the folder `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
	# sudo mkdir -p -m 755 /etc/apt/keyrings
	curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
	sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg # allow unprivileged APT programs to read this keyring
	```
3. Scaricare Kubernetes tramite [apt](../OS/Linux#apt)
	```shell
	# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
	echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
	sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list   # helps tools such as command-not-found to work correctly
	```
4. Aggiornare alla minor release più recente
	```shell
	sudo apt-get update
	sudo apt-get install -y kubectl
	```

# Kubelet
Rappresenta l'area operativa di Kubernetes (di solito è sovrapponibile all'intero progetto in [Docker](./Docker))
## Environment
### Green
Contiene tutti gli aggiornamenti da testare, rilasciare o in fase di sviluppo. L'accesso avviene solo tramite rete locale
### Blue
Contiene l'applicazione ufficiale, con accesso aperto al pubblico.
### Deploy
E' possibile effettuare sia il #rollout (la pubblicazione di una nuova versione del software), che il #rollback (ovvero la pubblicazione di una versione precedente del software)
### Self-healing
Kubernetes riavvia i container di [[Docker]] che si bloccano, sostituisce i container e termina quelli che non rispondono agli #health-check

# Entità
## Pod
Rappresenta l'unità più piccola in Kubernetes e costituisce un singolo nodo all'interno della [Kubelet](#Kubelet). Un **pod** può contenere uno o più container, a patto che tutti i container all'interno di un pod condividano le stesse risorse.

#### Eseguire un pod
```sh
kubectl run <nome> --image=<nome immagine docker>
```
#### Crea
```sh
kubectl create pod 
```
#### Elimina
```sh
kubectl delete pod <nome pod>
```

## Deployment
Entità che permette di gestire la quantità di [pod](#Pod) da utilizzare per deploy e il loro metodo di aggiornamento.

#### Crea
```sh
kubectl create deployment <nome> --image=<nome immagine docker>
```

#### Esponi alla rete esterna
```sh
kubectl expose deployment <nome> --type=NodePort --port=<porta>
```

#### Elimina
```sh
kubectl delete deployment <nome>
```

## Service
Gestisce la comunicazione tra i [pod](#Pod) stessi e tra i pod e la rete esterna. Funziona tramite raggruppamento di pod ed impostazione di regole di accesso per gruppo

#### Esposizione
```sh
minikube service <nome servizio>
```
	OPZIONI:
	--url #stampa solo il link con l'url di accesso

### Esempio
Esempio pratico di creazione di un servizio, attivazione ed esposizione alla rete esterna
1. Creazione di un file "simple-service.yaml"
	```simple-service.yaml
	apiVersion: v1
	kind: Service
	metadata:
	  name: my-service
	spec:
	  selector:
	    app.kubernetes.io/name: MyApp
	  ports:
	    - protocol: TCP
	      port: 80
	      targetPort: 9376
	```
2. Attivo il servizio
	```sh
	kubectl apply -f simple-service.yaml
	```
3. Visualizzo lo stato del service per conferma che il processo sia stato eseguito correttamente
	```sh
	kubectl get service simple-service
	```
	NOTE: La voce PORTS segnala la porta di accesso del servizio (scelta casualmente se non indicato altrimenti nel range 30000-32767)
4. Esposizione sulla rete esterna
	```sh
	kubectl port-forward service/simple-service 8080:<porta di accesso>
	```
## Controller
Entità che monitorano e gestiscono lo stato del cluster rispetto allo "stato desiderato" (ovvero quello specificato nel deployment). Nel caso ci sia una discrepanza tra stato desiderato e stato reale, il **controller** si occuperà, ad esempio, di riavviare il pod.


# kubectl
Kubectl (o **kubecontrol**) è il tool per linea di comando di Kubernetes e serve a:
- **Comunicare con il cluster:** Invia comandi all'API server di Kubernetes per creare, modificare, eliminare e visualizzare le risorse del cluster.
- **Gestire le risorse:** Puoi creare Pod, Deployments, Services, ConfigMaps, Secrets, Ingress e qualsiasi altra risorsa Kubernetes.
- **Controllare lo stato:** Ottenere informazioni sullo stato dei tuoi Pod, nodi, eventi, log dei container, ecc.
- **Debug:** Eseguire comandi all'interno dei container, inoltrare porte, copiare file da/verso i container.

![[schema-kubernetes.png]]
## Comandi
#### Port forwarding
```sh
kubectl port-forward <entità>/<nome nodo> <porta ingresso>:<porta uscita>
```

#### Visualizzazione
```sh
kubectl get <oggetto>
```
	OGGETTO:
	- all # Mostra tutti i nodi attivi
	- pod
	- deployment
	- service

# minikube
Minikube è uno strumento pensato per l'esecuzione di singoli nodi in locale, particolarmente utile per ambienti di sviluppo o testing di componenti del cluster reale del software
## Installazione
```shcurl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

# file
E' infine possibile configurare il ambiente Kubernetes tramite file [yaml]
I file .yaml possono essere utilizzati tramite comando
```sh
kubectl apply -f <nome file>.yaml
```
	
## Esempi
### pod
Cofigurazione di un pod [NGINX](./NGINX)
```nginx/pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

# Toubleshooting
## Docker
- Il pod non riesce e trovare l'immagine nell'ambiente locale
	```config-file.yaml
	imagePullPolicy: IfNotPresent
	```
- Le variabili di Docker non sono correttamente impostate su Kubernetes
	```sh
	eval ($minikube docker-env)
	```

# [[Cybersecurity]]
Kubernetes possiede più di 90 certificazioni .
Il software permette di
- Memorizzare e gestire informazioni sensibili (password, token OAuth, chiavi SSH)