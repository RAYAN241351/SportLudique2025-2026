## Mise a jour du systeme

Nous ferons toute les manipulation sur un machine virtuel sous debian

```
sudo apt update && sudo apt upgrade -y

```
##  Installation d’OpenSSL

```
sudo apt install openssl -y
```
## Arborescence pour les certificats

```
mkdir -p ~/tpssl/autorite/certificats
mkdir -p ~/tpssl/autorite/keys
mkdir -p ~/tpssl/siteweb/{certificats,keys,requests_certificats}
```

## Création de la clé privée de la CA
```
openssl genrsa -des3 2048 > ~/tpssl/autorite/keys/private_ca.key
```