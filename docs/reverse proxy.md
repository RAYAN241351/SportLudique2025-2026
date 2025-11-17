# Reverse Proxy – Nginx

<img src="https://thumbnail.imgbin.com/19/14/14/nginx-logo-GZFpkBSz_t.jpg" width="700">


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

## Certificats utilisés sur le reverse proxy

Les certificats ont été copiés depuis la VM CA vers le reverse proxy dans :

```
/etc/ssl/monsite/
├── siteweb.crt   → Certificat du site signé par la CA
├── siteweb.key   → Clé privée du site
└── ca.crt        → Certificat de l'autorité (CA)
```

## Voici le fichier de configurations dedié au site internet de l'infrastructure : 
```
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name www.bourges.sportludique.fr;

    # Certificats SSL signés par la CA interne
    ssl_certificate /etc/ssl/monsite/siteweb.crt;
    ssl_certificate_key /etc/ssl/monsite/siteweb.key;
    ssl_trusted_certificate /etc/ssl/monsite/ca.crt;

    # Sécurité SSL
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    location / {
        # Backend Apache sécurisé
        proxy_pass https://192.168.18.90;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Le backend utilise un certificat signé par ta CA → tu peux vérifier si tu veux
        proxy_ssl_verify off;
        # proxy_ssl_trusted_certificate /etc/ssl/monsite/ca.crt;
    }
}
```

## Redirection HTTP → HTTPS
```
server {
    listen 80;
    listen [::]:80;

    server_name www.bourges.sportludique.fr bourges.sportludique.fr;

    return 301 https://$host$request_uri;
}
```
## Activation du site
```
sudo ln -s /etc/nginx/sites-available/Bourges /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

## Vérifier Nginx

```
sudo nginx -t
sudo systemctl status nginx
```

## Vérifier HTTPS depuis l’extérieur
```
curl -I https://www.bourges.sportludique.fr
```

Rappel : configuration NAT/PAT sur le routeur
Ports à transférer vers le reverse proxy :

| port  | protocole          | destination interne |
|:-|:-:|-:|
| 80  |   TCP        |  reverse proxy |
| 443  | TCP            |   reverse proxy |``` 

