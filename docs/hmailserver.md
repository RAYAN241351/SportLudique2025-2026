
# Documentation Hmail server sur Windows 10

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRlhGIt-LJ0HHVtW9KLymtCv3Db32CJVLV5I_ywXU6FcKn5pWS97_LKoHS6wECqPF25IQ&usqp=CAU" width="800">

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

## configuration du routeur cisco

### Commandes a ajouter :

```
ip nat inside source static tcp 192.168.18.102 25 interface GigabitEthernet0/0 25
ip nat inside source static tcp 192.168.18.102 465 interface GigabitEthernet0/0 465
ip nat inside source static tcp 192.168.18.102 587 interface GigabitEthernet0/0 587
ip nat inside source static tcp 192.168.18.102 143 interface GigabitEthernet0/0 143
ip nat inside source static tcp 192.168.18.102 993 interface GigabitEthernet0/0 993
```

## Installation de Hmail sous windows

après avoir installer hmail sur le web il faut également installer framework sinon l'installation ne fonctionnera pas

## Configuration de hmail

### Lancer Hmail server et mettre le nom de domaine

capture1

### mettre les protocoles nécessaires (dans notre cas on utilise smtp pour envoyer des mails et imap pour en recevoir)

capture2

## Résolution du méssage "MX lookup failed: name not resolved" dans les logs

 Le DNS interne ne retournait pas correctement smtp.bourges.sportludique.fr
 problème corrigé en ajoutant dans le serveur d'autorité (interne):

 ```
 smtp IN CNAME mail
mail IN A 192.168.18.102
```
