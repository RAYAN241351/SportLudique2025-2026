<img src="https://data.ictjournal.ch/styles/np8_full/s3/media/2018/04/13/gmail-1162901_960_720.png?itok=WOPojcG3" width="800">

## Infrastructure des équipement important

| Service  | Adresse IP         | Logiciel |
|:-|:-:|-:|
| DNS autorité  |   192.168.18.70        |  BIND9 |
| reverse proxy  | 192.168.18.50             |   NGINX |
| routeur cisco  | 172.28.159.254             |   cisco 1921 |
| serveur mail  | 192.168.18.102          |    hmail server | ``` 

## Configuration DNS interne et externe sur le serveur d'autorité

### fichier de zone : `/etc/bind/db.interne`
```
$TTL 86400
@   IN SOA ns1.bourges.sportludique.fr. admin.bourges.sportludique.fr. (
       2025112519
       3600
       1800
       1209600
       86400 )

; Serveur DNS
@    IN NS ns1.bourges.sportludique.fr.
ns1  IN A 172.28.128.70

; Domaine principal
@    IN A 172.28.128.70
www  IN A 192.168.18.50

; Serveur de messagerie
mail IN A 192.168.18.102
smtp IN CNAME mail
imap IN CNAME mail

; MX
@    IN MX 10 smtp.bourges.sportludique.fr.

```
## Configuration DNS interne sur le serveur d'autorité

### fichier de zone : `/etc/bind/db.externe`

```
$TTL 86400

@   IN SOA ns1.bourges.sportludique.fr. admin.bourges.sportludique.fr. (

       2025112516 ; Serial (AAAAMMJJNN)

       3600       ; Refresh

       1800       ; Retry

       1209600    ; Expire

       86400 )    ; Minimum TTL
 
; Serveur DNS

@    IN NS ns1.bourges.sportludique.fr.

ns1  IN A 183.44.18.1
 
; Domaine principal et services web

@    IN A 183.44.18.1

www  IN A 183.44.18.1
 
; Serveur de messagerie unique

smtp IN A 183.44.18.1
 
 
; Enregistrement MX pointant vers le serveur mail

@    IN MX 10 smtp.bourges.sportludique.fr.

 
```