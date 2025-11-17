# Reverse Proxy – Nginx

Documentation de configuration – Projet HTTPS / Certificat CA interne

## Objectif du reverse proxy

Le reverse proxy Nginx a pour rôle de :

recevoir les connexions HTTP/HTTPS depuis Internet

rediriger toutes les requêtes HTTP vers HTTPS

présenter un certificat signé par la CA interne

transférer les requêtes HTTPS vers le serveur Apache situé dans le LAN
→ https://192.168.18.90

masquer l’adresse interne du serveur Web

appliquer des règles de sécurité (SSL, en-têtes, etc.)

## Architecture :
```
              Internet
                  │
                  ▼
          [Reverse Proxy Nginx]
       BRG-DEBIAN-REVERSE-PROXY
         - IP publique NAT/PAT
         - IP LAN interne (172.28.x.x)
                  │
                  ▼
        [Serveur Web Apache HTTPS]
              192.168.18.90
```