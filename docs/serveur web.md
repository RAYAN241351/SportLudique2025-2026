<img width="620" height="485" alt="image" src="https://github.com/user-attachments/assets/8cfce6b7-de11-4b7f-85bc-30f5cd990973" />

# Documentation Serveur Web Apache HTTPS

## 1. Objectif
Ce serveur web Apache est configuré pour fonctionner en **HTTPS** via un certificat signé par notre propre **autorité de certification (CA)** OpenSSL.  
Il héberge notre site interne sécurisé `www.bourges.sportludique.fr`.

---

## 2. Arborescence des fichiers liés au site web

```text
~/tpssl/
├── siteweb
│   ├── certificats
│   │   └── siteweb.crt         # Certificat signé par la CA
│   ├── keys
│   │   └── privatekey.key      # Clé privée du site web
│   └── requests_certificats
│       └── demande.csr         # CSR généré pour le site web
└── autorite
    ├── certificats
    │   └── ca.crt              # Certificat de la CA
    └── keys
        └── private_ca.key      # Clé privée de la CA
