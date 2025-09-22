
# <p align="center">Reinitialisation du routeur CISCO Configuration des interfaces réseau</p>
<img width="1100" height="600" alt="image" src="https://github.com/user-attachments/assets/bae53d48-ddf7-4060-bdc2-7d8e9a99dfb8" />



### 1. Appuyer simultanément sur la touche *CTRL* + *PAUSE*
- *confreg 0x2142*
- *reset*

### 2. Enregistrer la running-config en ayant fait des modifications et *reset* le routeur (ROMMON MODE)
- *hostname Routeur-bourges-fibre (exemple)*
- *copy running-config startup-config*
- *reset*
  
  ### 3. De nouveau sur le mode ROMMON, on modifie le registre qui charge la configuration enregistrée en mémoire  
- *confreg 0x2102*
- ***Enlever le cordon d'alimentation et le remettre dedans***  


# Configuration de base du routeur Cisco


### 1. Renommer le routeur
- *hostname Bourges-routeur*

### 2. Créer un utilisateur avec privilèges admin
- *username user privilege 15 secret motdepasse*

### 3. Activer le login via SSH sur les lignes VTY
- *line vty 0 4*
- *login local*
- *transport input ssh*

### <p align="center">Configuration des interfaces réseau</p>
<img width="900" height="500" alt="image" src="https://github.com/user-attachments/assets/0e3b59af-9c27-4530-a9e2-d913e3ee79c3" />

#### Interface vers le fournisseur d'accès
| interface GigabitEthernet0/0|
| ------------ |
| ip address 192.x.x.x 255.x.x.x|
| ip nat outside  |

#### Interface vers le réseau local (VLANs configurés)
| interface GigabitEthernet0/1|
| ------------ |
| no ip address|

#### Interface VLAN 210 (ex : réseau utilisateur)
| interface GigabitEthernet0/1.210|
| ------------ |
| encapsulation dot1Q 210|
| ip address 172.x.x.x 255.x.x.x|
| ip nat inside |


### Routage
<img width="1100" height="600" alt="image" src="https://github.com/user-attachments/assets/16fe2fab-ca7d-40e1-a5d5-53327da81ae8" />


 #### 1. Route par défaut vers le FAI
- *ip route 0.0.0.0 0.0.0.0 183.x.x.x*


### Configuration du NAT


#### 1. ACL pour NAT (autorise les hôtes LAN à sortir)
- *access-list 1 permit 172.x.x.x 0.0.0.255*

#### 2. Activer le NAT avec surcharge (PAT)
- *ip nat inside source list 1 interface GigabitEthernet0/0 overload*


