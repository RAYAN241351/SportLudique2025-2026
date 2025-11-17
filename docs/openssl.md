<img src="https://www.linuxadictos.com/wp-content/uploads/OpenSSL_logo.svg.png" width="800">

# Partie serveur d'autorité de certification (openssl)

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

## Création du 

    La clé privée de la CA reste sur la VM CA.

    Tous les certificats serveurs doivent être signés par cette CA pour être reconnus comme valides par le reverse proxy ou les clients internes.

    Cette méthode évite les certificats auto-signés côté serveur, le serveur web utilise un certificat signé par la CA interne.
certificat serveur via la CA
### géneration de la clé privée du serveur:
```
openssl genrsa 2048 > ~/tpssl/siteweb/keys/siteweb.key
```
### Création de la demande de certificat (CSR) :
```
openssl req -new -key ~/tpssl/siteweb/keys/siteweb.key \
  -out ~/tpssl/siteweb/requests_certificats/siteweb.csr \
  -config ~/tpssl/openssl-site.cnf
```
Remplir les informations demandées, le Common Name doit correspondre au domaine du site.

Assurez-vous que le fichier de config OpenSSL contient la section subjectAltName pour SAN.
### Signature de la demande par la CA :
```
openssl x509 -req \
  -in ~/tpssl/siteweb/requests_certificats/siteweb.csr \
  -CA ~/tpssl/autorite/certificats/ca.crt \
  -CAkey ~/tpssl/autorite/keys/private_ca.key \
  -CAcreateserial \
  -out ~/tpssl/siteweb/certificats/siteweb.crt \
  -days 365 \
  -extfile ~/tpssl/openssl-site.cnf -extensions v3_req
```
Le certificat serveur est maintenant signé par la CA.

### Vérification: 
```
openssl x509 -in ~/tpssl/siteweb/certificats/siteweb.crt -text -noout
```
## Partage des certificats sur le serveur web
```
scp ~/tpssl/siteweb/certificats/siteweb.crt operateur@IP_SERVEUR:/etc/ssl/monsite/
scp ~/tpssl/siteweb/keys/siteweb.key operateur@IP_SERVEUR:/etc/ssl/monsite/
scp ~/tpssl/autorite/certificats/ca.crt operateur@IP_SERVEUR:/etc/ssl/monsite/
```

`IP_SERVEUR` : IP de la VM Apache2 ou Nginx.
Ces fichiers seront utilisés par le serveur pour sécuriser le site via TLS.

## Notes importantes
La clé privée de la CA **reste sur la VM CA**.

Tous les certificats serveurs doivent être signés par cette CA pour être reconnus comme valides par le reverse proxy ou les clients internes.

Cette méthode **évite les certificats auto-signés côté serveur**, le serveur web utilise un certificat signé par la CA interne.

# Partie serveur web (Apache2)

## installation
```
sudo a2enmod ssl
```
/etc/ssl/monsite/
├── siteweb.crt       # Certificat du serveur signé par la CA
├── siteweb.key       # Clé privée du serveur
└── ca.crt            # Certificat de la CA

## Configuration du site sécurisé

### Copier le fichier de configuration SSL par défaut :
```
sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/monsite.conf
```
### Modifier le fichier /etc/apache2/sites-available/monsite.conf :
```
<IfModule mod_ssl.c>
    <VirtualHost *:443>
        ServerAdmin admin@monsite.local
        ServerName www.bourges.sportludique.fr
        DocumentRoot /var/www/html

        SSLEngine on
        SSLCertificateFile      /etc/ssl/monsite/siteweb.crt
        SSLCertificateKeyFile   /etc/ssl/monsite/siteweb.key
        SSLCertificateChainFile /etc/ssl/monsite/ca.crt

        <Directory /var/www/html>
            Options Indexes FollowSymLinks
            AllowOverride All
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
</IfModule>
```
### Remarques :

`SSLCertificateChainFile` contient le certificat de la CA.

`ServerName` doit correspondre exactement au CN du certificat.

## Test du site HTTPS

### Depuis le serveur ou un client du réseau interne :
```
curl -vk https://www.bourges.sportludique.fr
```