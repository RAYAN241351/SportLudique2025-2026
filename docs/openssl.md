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

## Création du certificat de la CA
```
openssl req -new -x509 -days 365 \
  -key ~/tpssl/autorite/keys/private_ca.key \
  -out ~/tpssl/autorite/certificats/ca.crt
```
-new -x509 : crée un certificat auto-signé pour la CA.

-days 365 : validité du certificat.

Informations à renseigner lors de l’invite :

Country Name (FR)

State or Province Name

Locality Name

Organization Name

Common Name (nom de la CA, ex: CA-Bourges)

Email Address

Note : Ici, le certificat de la CA est signé par lui-même, mais il servira à signer les certificats des serveurs.

## Création du certificat serveur via la CA
géneration de la clé privée du serveur:
```
openssl genrsa 2048 > ~/tpssl/siteweb/keys/siteweb.key
```
Création de la demande de certificat (CSR) :
```
openssl req -new -key ~/tpssl/siteweb/keys/siteweb.key \
  -out ~/tpssl/siteweb/requests_certificats/siteweb.csr \
  -config ~/tpssl/openssl-site.cnf
```
Remplir les informations demandées, le Common Name doit correspondre au domaine du site.

Assurez-vous que le fichier de config OpenSSL contient la section subjectAltName pour SAN.