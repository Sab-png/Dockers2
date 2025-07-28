
# Guida Introduttiva a Docker 🐳

## ✅ Cos'è Docker?

Docker è una piattaforma che consente di **automatizzare il deployment di applicazioni** dentro **container**, cioè ambienti isolati, leggeri e riproducibili.

Un container include:
- Il codice dell'applicazione
- Le dipendenze
- Le configurazioni
- Il sistema di runtime

Grazie a Docker, possiamo garantire che **un'applicazione funzioni allo stesso modo ovunque**: sul computer di sviluppo, su un server remoto o su un cloud.

---

## 🧱 Differenza tra Container e Macchina Virtuale

| Macchina Virtuale         | Docker Container             |
|---------------------------|------------------------------|
| Sistema operativo completo | Condivide il kernel dell'host |
| Più pesante e lenta        | Più leggera e veloce          |
| Consuma molte risorse      | Utilizzo minimo delle risorse |
| Avvio lento                | Avvio quasi istantaneo        |

> 🔍 **Container ≠ Virtual Machine**, ma ottengono un risultato simile con un approccio più moderno e performante.

---

## 🧩 Componenti Fondamentali di Docker

### 📦 Immagine Docker (Image)
Un'immagine è un **template immutabile** che contiene tutto il necessario per far girare un'applicazione.  
È **read-only** e può essere condivisa facilmente (es. tramite Docker Hub).

Esempi di immagini:
- `node:18`
- `python:3.12`
- `nginx:alpine`

> Le immagini sono costruite da **layer**, che possono essere riutilizzati per più immagini.

---

### 🧪 Container
Un container è un'istanza **in esecuzione** di un'immagine.  
È isolato dal sistema host ma può essere collegato a reti, volumi o altri container.

Caratteristiche:
- Può leggere i dati dal layer immutabile (immagine)
- Ha un suo filesystem temporaneo
- Può essere avviato, fermato, distrutto e ricreato rapidamente

---

### 🛠️ Dockerfile
Il `Dockerfile` è un file di testo che definisce **le istruzioni per creare un'immagine**.

Esempio:

```Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "index.js"]
```

Ogni riga è **uno step di build**. Quando eseguiamo `docker build`, Docker esegue questi comandi per costruire l'immagine finale.

---

### 🗂️ Docker Hub
È il **registry ufficiale** di immagini pubbliche.  
Puoi scaricare immagini con `docker pull` o pubblicare le tue con `docker push`.

> Esempio: `docker pull ubuntu:20.04`

---

## 🚀 Esempio Completo - Hello World in Docker

1. **Crea `hello.py`**:

```python
print("Ciao dal container Docker!")
```

2. **Crea `Dockerfile`**:

```Dockerfile
FROM python:3.12-slim
COPY hello.py /app/hello.py
WORKDIR /app
CMD ["python", "hello.py"]
```

3. **Costruisci l'immagine**:

```bash
docker build -t hello-docker .
```

4. **Esegui il container**:

```bash
docker run hello-docker
```

---

## 🔧 Comandi Docker più usati

| Comando                            | Descrizione |
|-----------------------------------|-------------|
| `docker build -t nome .`          | Costruisce un'immagine dal Dockerfile |
| `docker run nome`                 | Avvia un container dall'immagine |
| `docker ps`                       | Elenca i container attivi |
| `docker ps -a`                    | Elenca tutti i container |
| `docker images`                   | Elenca le immagini |
| `docker stop <id>`                | Ferma un container |
| `docker rm <id>`                  | Rimuove un container |
| `docker rmi <nome>`               | Rimuove un'immagine |
| `docker exec -it <id> bash`       | Apre una shell nel container |
| `docker logs <id>`                | Mostra l'output di un container |

---

## 🧭 Architettura Docker in breve

1. **Docker Client**: riceve i comandi da terminale (`docker ...`)
2. **Docker Daemon**: gestisce i container, immagini, rete e volumi
3. **Docker Registries**: archivi di immagini (es. Docker Hub)
4. **Host Machine**: il sistema operativo su cui gira Docker

---

## 📦 Quando e Perché Usare Docker

- ✔️ Deployment coerente in ambienti diversi (dev, staging, prod)
- ✔️ Sviluppo basato su microservizi
- ✔️ CI/CD automatizzato e isolato
- ✔️ Testing in ambienti puliti
- ✔️ Evitare conflitti di dipendenze

---

## 🧠 Conclusione

Docker ha rivoluzionato il modo in cui sviluppiamo, testiamo e distribuiamo software.  
Grazie ai container possiamo:
- Standardizzare gli ambienti
- Isolare i servizi
- Ridurre problemi di configurazione
- Automatizzare il ciclo di vita delle applicazioni

> 💡 *"Se funziona nel tuo container, funzionerà ovunque."*
