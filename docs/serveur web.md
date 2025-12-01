<img width="620" height="485" alt="image" src="https://github.com/user-attachments/assets/8cfce6b7-de11-4b7f-85bc-30f5cd990973" />

# Documentation Serveur Web Apache HTTPS

## 1. Objectif
Ce serveur web Apache est configuré pour fonctionner en **HTTPS** via un certificat signé par notre propre **autorité de certification (CA)** OpenSSL.  
Il héberge notre site interne sécurisé `www.bourges.sportludique.fr`.

---

## 2. Arborescence des fichiers liés au site web

Le site est hébergé dans le répertoire Apache par défaut :
```
/var/www/html/
├── index.html
└── images/
```
## Configuration VirtualHost du site (HTTPS)

### Le VirtualHost HTTPS du site se trouve dans :

```
/etc/apache2/sites-available/bourges.conf
```
Configuration du site de Bourges : 
```

```

## Activation du site

### Activer le module SSL
```
sudo a2enmod ssl
```
### Activer le VirtualHost
```
sudo a2ensite bourges.conf
sudo systemctl restart apache2
```