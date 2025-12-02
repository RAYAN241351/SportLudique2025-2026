
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
- [Hmail](https://www.hmailserver.com/download)

- [Framework](https://www.microsoft.com/fr-fr/download/details.aspx?id=6041)

## Configuration de hmail

### Lancement Hmailserver et renseigner le nom de domaine correspondant au votre 

<img width="783" height="592" alt="image" src="https://github.com/user-attachments/assets/be1530b4-2cf1-45ff-a3c0-fc7830e1d53a" />


### Protocoles nécessaires au bon fonctionnement de hMailServer (SMTP: Envoyer des mails, IMAP: Recevoir des mails)

<img width="218" height="80" alt="image" src="https://github.com/user-attachments/assets/9b145f9b-eb48-4576-b1c9-9ddce05a4a01" />
<img width="612" height="325" alt="image" src="https://github.com/user-attachments/assets/12fe3dcd-ccff-4153-90d9-3bea2617adc3" />
<img width="635" height="337" alt="image" src="https://github.com/user-attachments/assets/866b7b1a-76bc-40cf-a5a9-4e328c2af01e" />


## Résolution du méssage "MX lookup failed: name not resolved" dans les logs

 Le DNS interne ne retournait pas correctement smtp.bourges.sportludique.fr
 problème corrigé en ajoutant dans le serveur d'autorité (interne):

 ```
 smtp IN CNAME mail
 mail IN A 192.168.18.102
```







## Partie Thunderbird

    - Tout d’abord, installer Thunderbird puis cliquer sur « Configurer une adresse de messagerie » avec une adresse créée sur hMailServer sous Windows.

    - Sélectionner votre compte, puis appuyer sur Continuer.

<img width="862" height="392" alt="image" src="https://github.com/user-attachments/assets/7e8fc8c1-5d1e-4720-9af2-a93c731939be" />

**Si cela ne marche pas :**

    Cliquer sur Configuration manuelle.
    
<img width="520" height="381" alt="image" src="https://github.com/user-attachments/assets/80b8fcf6-0ed4-4e7a-a2fe-3572e338b1b1" />

### Serveur entrant
<img width="445" height="342" alt="image" src="https://github.com/user-attachments/assets/119c0780-a196-4fc1-bebe-bbb777d2487e" />

    Protocole : IMAP

    Nom d’hôte : .domain.tld (votre propre domaine avec un « . » au début)
Capture d’écran du 2025-12-02 11-50-58
    Port : 143

    Nom d’utilisateur : nom@nomdedomaine.fr

### Serveur sortant

<img width="419" height="266" alt="image" src="https://github.com/user-attachments/assets/aa2f69f1-70d8-404f-8d88-9ef6311e28dc" />

    Nom d’hôte : .domain.tld (votre propre domaine avec un « . » au début)

    Port : 25 (SMTP)

    Nom d’utilisateur : nom@nomdedomaine.fr (créé précédemment sur hMailServer)

Ensuite, cliquer sur **Terminer** : la connexion devrait fonctionner si la configuration du serveur est correcte.

### Interface graphique après connexion réussie
<img width="909" height="961" alt="image" src="https://github.com/user-attachments/assets/cef76d6f-4143-4d2b-b954-711db68f3ab7" />

## Problèmes rencontrés:
Mauvaise R
