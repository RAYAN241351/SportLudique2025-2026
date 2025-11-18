## Documentation OPNsense
<img width="600" height="400" alt="opnsense-logo-vector-removebg-preview" src="https://github.com/user-attachments/assets/85dd3415-b10b-4fa0-9fb7-532366d5280c" />



Architecture – Interfaces – Routage – NAT – Notes importantes
## 1.Présentation générale

Ce système OPNsense joue le rôle de pare-feu et passerelle dans une architecture comprenant plusieurs réseaux :

Un réseau interne LAN

Un réseau DMZ (via WAN)

Un réseau d’interconnexion avec un switch cœur (OPT1)

Il assure :
✔ le routage entre réseaux
✔ certaines routes statiques vers un firewall Stormshield
✔ une règle NAT interne anti-lockout
✔ la gestion de plusieurs passerelles

## 2.Interfaces configurées
## 2.1 Interface LAN

Nom : LAN
Adresse IP : 192.168.110.98
Rôle : Interface de gestion (accès GUI OPNsense)

## 2.2 Interface WAN

Nom : WAN
Adresse IP : 192.168.18.2

Réseau : DMZ
Passerelle associée : Passerelle-Stormshield – 192.168.18.1
Rôle : Sortie vers le firewall Stormshield et vers d'autres réseaux via celui-ci.

## 2.3 Interface OPT1

Nom : OPT1
Adresse IP : 172.28.159.126 /25


## 3. Routage

Les routes configurées dans Système → Routes → Configuration sont les suivantes :

|Réseau destination|	Passerelle| 	Description|	Activée ou Désactivé|
|:-|:-|:-|:-|
|172.28.129.0/24|	Passerelle-Stormshield (192.168.18.1)|	Client|	Oui/Non (à préciser)|
|172.28.128.0/24|	Passerelle-Stormshield (192.168.18.1)|	Serveur|	Oui/Non (à préciser)|
|0.0.0.0/0|	Passerelle-Stormshield (192.168.18.1)|	Route par défaut|	✔ activée|


## Signification:
- La route par défaut envoie tout le trafic vers le Stormshield.

- Deux réseaux internes 172.28.128.0/24 et 172.28.129.0/24 sont atteints via cette même passerelle.



## Pare-feu: NAT: Redirection de port 

|Interface LAN | Proto | Adresse | Ports | Adresse | Ports | IP | Ports | Description|
|:-|:-|:-|:-|:-|:-|:-|:-|:-|
|LAN|TCP|*|*|LAN adresse| 80,443|*|*|Règle anti-Lockout|

- Cette règle empêche de te verrouiller en bloquant l’accès à l’interface Web (HTTP/HTTPS) depuis le LAN.
- HTTP (80)
- HTTPS (443)
→ vers l’IP LAN pour la GUI

## 5. Passerelles configurées
## 5.1 Passerelle Stormshield
IP : 192.168.18.1
Interface associée : WAN

Utilisée pour :
✔ route par défaut
✔ réseaux 172.28.129.0/24 et 172.28.128.0/24

## 5.2 Passerelle Switch Core
IP : 172.28.159.1
Interface : OPT1

## 6.Schéma Logique:
        +----------------------+
        |     Switch Core      |
        | 172.28.159.1         |
        +----------+-----------+
                   |
         OPT1: 172.28.159.126/25
                   |
        +----------v-----------+
        |       OPNsense       |
        |-----------------------|
        | 192.168.110.98 -> LAN |
        | 192.168.18.2   -> WAN |
        +----------+------------+
                   |
        +----------v-----------+
        |   Firewall Stormshield |
        |    192.168.18.1        |
        +------------------------+
