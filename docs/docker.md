# Docker sur Debian – Documentation (avec erreurs rencontrées)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/be971729-52d1-441e-a8e6-1b86d8ac5687" />

## 1. Présentation rapide

Docker permet de lancer des applications dans des **conteneurs légers**, isolés du système hôte, tout en partageant le même noyau que la machine.  
Par opposition, la **virtualisation** avec un hyperviseur (Proxmox, VMware, etc.) fait tourner des VM complètes, chacune avec son OS, ce qui consomme plus de ressources **CPU/RAM/stockage**.

---

## 2. Installation de Docker sur Debian

### 2.1. Pré‑requis
- sudo apt update
- sudo apt install ca-certificates curl gnupg lsb-release -y


### 2.2. Ajout du dépôt officiel Docker

- sudo install -m 0755 -d /usr/share/keyrings
- curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg
- sudo chmod a+r /usr/share/keyrings/docker.gpg

-echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker.gpg] 
https://download.docker.com/linux/debian $(lsb_release -cs) stable"
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


### 2.3. Installation de Docker Engine + plugin Compose

- sudo apt update
- sudo apt install docker-ce docker-ce-cli containerd.io
- docker-buildx-plugin docker-compose-plugin -y

- docker --version
- docker compose version
- sudo systemctl enable --now docker


### 3.1. Structure de projet

- mkdir ~/node-docker-demo
- cd ~/node-docker-demo
- npm init -y
- npm install
**Créer `app.js` :**
const http = require('http');

const server = http.createServer((req, res) => {
res.statusCode = 200;
res.setHeader('Content-Type', 'text/plain');
res.end('Hello, Docker!\n');
});

const port = process.env.PORT || 3000;

server.listen(port, () => {
console.log(Server running on http://localhost:${port});
});

**Créer `Dockerfile` :**
FROM node:18-alpine

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]


### 3.2. Fichier `docker-compose.yml` minimal

version: "3"
services:
app:
build:
context: .
ports:
- "3000:3000"

### 3.3. Lancement

- docker compose up -d # lancement en arrière-plan
- docker compose ps # état des conteneurs
- docker compose down # arrêt + suppression des conteneurs

L’application est ensuite accessible sur [**http://IP_DEBIAN:3000**](http://IP_DEBIAN:3000).

## 4. Notions importantes

### 4.1. Mappage de ports

Dans `docker-compose.yml` :
ports: "3000:3000"

- 1er nombre : port **hôte**  
- 2e nombre : port **conteneur**

**Exemples :**  
- `8080:80` → http://IP:8080 pointe vers le port 80 du conteneur (classique pour Nginx/Apache).  
- `0:3000` ou `3000` → Docker choisit un port dispo sur l’hôte, consultable avec `docker compose ps`.

---

### 4.2. Volumes (persistance des données)

**Exemple simple en ligne de commande :**

## 5. Commandes Docker de base

- `docker pull <image>` : Télécharger une image (ex. `docker pull nginx`)
- `docker images` : Lister les images locales
- `docker ps` / `docker ps -a` : Lister conteneurs actifs / tous les conteneurs
- `docker run -d -p 8080:80 nginx` : Lancer Nginx en arrière‑plan (accessible sur http://IP:8080)
- `docker exec -it <container> /bin/bash` : Ouvrir un shell dans un conteneur
- `docker stop <container>` / `docker rm <container>` : Arrêter / supprimer un conteneur
- `docker rmi <image>` : Supprimer une image
- `docker network ls` / `docker volume ls` : Lister réseaux et volumes

## 6. Portainer (UI Web pour Docker)

### 6.1. Déploiement

- docker volume create portainer_data

``docker run -d
-p 8000:8000 -p 9000:9000
--name=portainer
--restart=always
-v /var/run/docker.sock:/var/run/docker.sock
-v portainer_data:/data
portainer/portainer-ce:latest
``
Interface accessible sur [**http://IP_DEBIAN:9000**](http://IP_DEBIAN:9000) (création d’un compte admin lors du premier accès).

---

### 6.2. Message “instance programmée à des fins de sécurité”

Portainer coupe l’assistant de première installation après un délai pour éviter une prise de contrôle non souhaitée.

**Pour le réactiver :**
- docker stop portainer
- docker start portainer


---

## 7. Erreurs rencontrées et corrections

### 7.1. Erreur Docker APT : Failed to parse keyring "/usr/share/keyrings/docker.gpg"

**Symptôme :**  
`apt update` affiche :Error: Failed to parse keyring "/usr/share/keyrings/docker.gpg" – No such file or directory
Le dépôt https://download.docker.com/linux/debian n’est pas signé.`


**Cause :**  
Le dépôt Docker est configuré avec `signed-by=/usr/share/keyrings/docker.gpg` mais ce fichier **n’existe pas** (clé GPG non ajoutée).

**Correction :**
- **sudo install -m 0755 -d /usr/share/keyrings**

- **curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg**

- **sudo chmod a+r /usr/share/keyrings/docker.gpg**

- **sudo apt update**
  Vérifier aussi que `/etc/apt/sources.list.d/docker.list` contient bien une ligne de ce type :
  deb [arch=amd64 signed-by=/usr/share/keyrings/docker.gpg] https://download.docker.com/linux/debian trixie stable
  

---

### 7.2. Message Portainer : “Votre instance Portainer est programmée à des fins de sécurité”

**Symptôme :**  
Après l’installation, Portainer affiche un message indiquant que l’instance a expiré pour des raisons de sécurité et qu’il faut la redémarrer.

**Cause :**  
L’assistant de configuration initial (création de l’utilisateur admin) **n’a pas été terminé dans le délai imparti**.

**Correction :**
- docker stop portainer
- docker start portainer
**puis se connecter immédiatement sur http://IP_machine:9000 et terminer l’assistant**
  





