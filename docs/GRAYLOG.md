## Installation et Configuration de Graylog sur Debian

Cette documentation décrit les étapes pour installer et configurer Graylog sur un serveur Debian, en incluant la configuration de l'utilisateur admin et la sécurisation des mots de passe.

## 1. Pré-requis

Un serveur Debian à jour

Accès root ou sudo

Serveur accessible sur le réseau

Ports à ouvrir : 9000 (web interface Graylog)

Mettre à jour le serveur :

```
sudo apt update && sudo apt upgrade -y
```

Installer les dépendances :

sudo apt install apt-transport-https openjdk-17-jdk pwgen curl gnupg -y
java -version