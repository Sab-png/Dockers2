
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

# Docker vs Podman: Demone centrale e confronto

## 🔧 Docker: demone centrale
Docker funziona secondo un'architettura **client-server**:

- Il comando `docker` che usi nel terminale è il **client**.
- Il demone `dockerd` è il **server** che riceve i comandi dal client e li esegue (creare container, scaricare immagini, gestire reti, volumi, ecc).
- Questo demone gira **continuamente in background** e **richiede privilegi di root** per funzionare (tranne con configurazioni specifiche).

### Problemi associati:
- **Sicurezza**: un demone che gira con privilegi elevati può rappresentare un rischio.
- **Gestione risorse**: se il demone si blocca, tutti i container ne risentono.
- **Architettura monolitica**: tutto passa attraverso un unico punto.

---

## 🧱 Podman: no demone centrale
- Podman **non ha un demone in background**.
- Ogni comando (`podman run`, `podman build`, ecc.) viene eseguito come **un singolo processo**, gestito direttamente dall'utente (anche senza root).
- Questo lo rende **più sicuro** e più adatto a contesti dove si cerca la **massima separazione dei privilegi**.

### Vantaggi:
- Può essere usato da utenti **non-root** senza compromessi.
- Nessun processo centrale da monitorare o riavviare.
- Ogni container è **figlio diretto del processo utente** che lo ha lanciato.

---

## 🧱 Docker vs Podman: Tabella comparativa

| Caratteristica              | **Docker**                                       | **Podman**                                      |
|-----------------------------|--------------------------------------------------|--------------------------------------------------|
| 🔧 Architettura             | Client-Server (CLI ↔ `dockerd`)                 | Senza demone (no server centrale)               |
| 🔐 Sicurezza                | Richiede demone con privilegi elevati (root)    | Può essere usato **senza root** (rootless)      |
| 🧑‍💻 Gestione container     | Tutti i container sono gestiti dal demone Docker| Ogni container è gestito dal processo utente     |
| 🔁 Compatibilità CLI        | Comandi `docker` standard                        | Compatibile 1:1 con i comandi `docker`           |
| 📦 Immagini                | Usa formato OCI (via Docker Hub o registries)   | Usa lo stesso formato OCI, pienamente compatibile|
| 🔄 Build immagini          | Usa `docker build`, supporta BuildKit           | Usa `buildah` (internamente o esternamente)     |
| 👥 Supporto a pod           | ❌ No (solo container singoli o Compose)         | ✅ Sì (concetto nativo di **pod** come in Kubernetes) |
| ⚙️ Runtime container        | Usa `containerd` + `runc`                       | Usa `crun` o `runc`                              |
| 📄 Dockerfile              | Supportato nativamente                          | Supportato nativamente                          |
| 📁 Docker Compose          | ✅ Sì (strumento ufficiale)                      | 🟡 Compatibile via `podman-compose` (meno maturo) |
| ☁️ Registry integrato      | Docker Hub + supporto altri registry            | Nessun registry proprietario, usa OCI registries |
| 💻 Desktop GUI             | Docker Desktop (GUI + VM)                       | Podman Desktop (GUI alternativa)                |
| ⚙️ Orchestrazione          | Docker Swarm, o integrato con Kubernetes        | Nessun orchestratore nativo, lavora con Kubernetes |
| 🔍 Ispezione container     | `docker inspect`                                | `podman inspect` (molto simile)                 |
| 🧪 Progetto open source    | Open source (con componenti aziendali)          | Completamente open source (Red Hat, Fedora)     |

---

## ✅ Vantaggi principali

| Docker                                   | Podman                                      |
|------------------------------------------|----------------------------------------------|
| Maturità, ampia community e supporto     | Sicurezza senza root, architettura moderna   |
| Integrazione facile con Docker Compose   | Pod nativi come in Kubernetes                |
| Tooling solido (Docker Desktop, Hub, ecc)| Non richiede demone, ideale per sistemi minimal |
| Supporto ufficiale per Windows/macOS     | Ottimo per ambienti Linux e sviluppatori orientati a sicurezza |

---

# 🚀 Kubernetes vs Docker

## 🧭 Kubernetes vs Docker: Differenze di livello

- **Docker** è uno **strumento per creare ed eseguire container**.
- **Kubernetes** è un **sistema di orchestrazione** che gestisce **cluster** di container in ambienti distribuiti (più macchine).

In realtà, **non sono in competizione**, ma lavorano **a livelli diversi della pila tecnologica**.

---

## 📌 Quando usare Docker da solo

Usi **solo Docker** quando:
- Hai **una singola macchina** (server, VM o laptop).
- L’applicazione è **piccola o media**, con pochi container.
- Vuoi sviluppare, testare o fare deploy in modo **semplice e diretto**.
- L’infrastruttura non richiede alta disponibilità o scalabilità automatica.

**Esempi:**
- Un’app Flask o Node.js in container con un database.
- Un’app a microservizi locale gestita con `docker-compose`.
- Progetti personali, demo, prototipi.

---

## 📌 Quando usare Kubernetes

Usi **Kubernetes** quando:
- Hai bisogno di **scalare automaticamente** le tue applicazioni.
- Vuoi **alta disponibilità** e **failover** automatico.
- Gestisci **più container distribuiti** su più nodi (macchine).
- Hai bisogno di funzionalità come:
  - Load balancing
  - Rolling updates
  - Auto-healing (restart automatico dei container falliti)
  - Secrets/config maps
  - Gestione dello storage persistente
- Lavori in ambienti **cloud-native**, **CI/CD** avanzati, o **microservizi su larga scala**.

**Esempi:**
- Un sistema SaaS con decine di microservizi in cloud.
- Un e-commerce distribuito con molteplici componenti.
- Infrastrutture enterprise multi-cluster.

---

## 🧱 Schema visivo semplificato

| Caratteristica                | **Docker (da solo)**       | **Kubernetes**                    |
|-------------------------------|-----------------------------|------------------------------------|
| Ambito                        | Singola macchina            | Cluster distribuito               |
| Gestione container            | Manuale o con Compose       | Automatizzata e orchestrata       |
| Scalabilità                   | Manuale                     | Automatica                         |
| Tolleranza ai guasti          | Limitata                    | Alta (auto-restart, replica)      |
| Rolling updates               | ❌ No                        | ✅ Sì                              |
| Load balancing                | ❌ No                        | ✅ Sì                              |
| Complessità                   | Bassa                       | Alta                               |
| Curva di apprendimento        | Facile                      | Ripida                             |

---

## 🔄 Come lavorano insieme

Un tempo, **Docker veniva usato anche all’interno di Kubernetes**, ma oggi Kubernetes usa direttamente **containerd** come runtime (senza Docker Engine). Tuttavia, **Docker rimane utilissimo** per:
- Costruire immagini (`docker build`)
- Testare localmente
- Usare strumenti come Docker Compose in dev

---

## ✅ In sintesi

- **Usa Docker** se sei in locale, progetto piccolo, o vuoi semplicità.
- **Usa Kubernetes** se hai **bisogno di gestire in modo professionale e scalabile** molti container distribuiti, in produzione o ambienti complessi.


# ✅ Kubernetes: Pro e Contro


| Categoria                        | ✅ Pro (Vantaggi)                                                                 | ❌ Contro (Svantaggi)                                                             |
|----------------------------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Scalabilità                      | Auto-scaling orizzontale, gestione carichi variabili                             | Richiede configurazione avanzata per scalabilità efficace                       |
| Alta disponibilità               | Failover automatico, auto-restart container                                     | Maggiore complessità operativa                                                  |
| Gestione dichiarativa           | Configurazione via YAML, infrastruttura come codice                             | Difficile da leggere e mantenere per team non esperti                           |
| Aggiornamenti                   | Rolling updates e rollback automatici                                           | Possibili errori se i deployment non sono configurati correttamente             |
| Multi-tenancy                   | Namespaces per ambienti/team separati                                           | Gestione RBAC e sicurezza complessa                                             |
| Ecosistema e community          | Ampio supporto di estensioni e strumenti (Helm, Istio, ArgoCD, ecc.)            | Tooling frammentato, learning curve elevata                                     |
| Config e Segreti                | Gestione integrata di Secrets e ConfigMap                                       | Richiede attenzione per evitare esposizione accidentale                        |
| Storage persistente             | Supporto a volumi per applicazioni stateful                                     | Configurazione più complicata rispetto a Docker                                 |
| Portabilità                     | Funziona su cloud, bare-metal, on-prem, locale                                  | DevOps richiesto per adattarsi a contesti diversi                               |
| Complessità                     | Architettura potente e flessibile                                               | Alta complessità per piccoli progetti                                           |
| Costi                           | Adatto a grandi sistemi distribuiti                                             | Maggiore utilizzo risorse = costi più alti (cloud, gestione, manutenzione)      |
| Debug e monitoraggio            | Supporta tool avanzati (Prometheus, Grafana, ecc.)                              | Debug più difficile, servono skill avanzate                                     |
