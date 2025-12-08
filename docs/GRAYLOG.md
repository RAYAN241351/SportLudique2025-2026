## Installation et Configuration de Graylog sur Debian

Cette documentation décrit les étapes pour installer et configurer Graylog sur un serveur Debian, en incluant la configuration de l'utilisateur admin et la sécurisation des mots de passe.

## 1. Pré-requis

Un serveur Debian à jour

Accès root ou sudo

Serveur accessible sur le réseau

Ports à ouvrir : 9000 (web interface Graylog)

Mettre à jour le serveur :

```Installer les dépendances :

sudo apt install apt-transport-https openjdk-17-jdk pwgen curl gnupg -y
java -version
sudo apt update && sudo apt upgrade -y
```

## 2. Installation des composants Graylog

Graylog nécessite MongoDB et Elasticsearch/OpenSearch. Pour cette installation locale, Graylog fonctionne avec une instance OpenSearch ou Elasticsearch. Cependant nous installerons opensearch sur un **serveur dédié**

Graylog nécessite **MongoDB** et **OpenJDK** pour fonctionner.

### Installer Java
```
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
```

### Installer MongoDB
```
sudo apt install mongodb -y
sudo systemctl enable mongodb
sudo systemctl start mongodb
```
## 3.Installation de Graylog

### Ajouter le dépôt Graylog

```
wget https://packages.graylog2.org/repo/packages/graylog-6.3-repository_latest.deb
sudo dpkg -i graylog-6.3-repository_latest.deb
sudo apt update
```

### Installer Graylog
```
sudo apt install graylog-server -y
```
## 4.Configuration de Graylog

Le fichier principal est /etc/graylog/server/server.conf

### Définir le mot de passe secret

Le mot de passe secret est utilisé pour sécuriser les mots de passe stockés.

```
pwgen -N 1 -s 96
```

### Copier la valeur générée dans :

```
password_secret = <valeur_generee>
```
### Définir le mot de passe root en faisant un hash

```
echo -n "votre_mdp" | sha256sum
```

### Copier le hash dans le fichier de conf:

```
root_password_sha2 = <hash_genere>
root_username = admin
```