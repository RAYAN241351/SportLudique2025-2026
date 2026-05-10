## Reinitialisation du routeur CISCO et Configuration du routeur CISCO
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/87d30af4-26a9-4365-9322-a27d3f34a118" />




## 1. Appuyer simultanément sur la touche *CTRL* + *PAUSE*
- *confreg 0x2142*
- *reset*

## 2. Enregistrer la running-config en ayant fait des modifications et *reset* le routeur (ROMMON MODE)
- *hostname Routeur-bourges-fibre (exemple)*
- *copy running-config startup-config*
- *reset*
  
## 3. De nouveau sur le mode ROMMON, on modifie le registre qui charge la configuration enregistrée en mémoire  
- *confreg 0x2102*
- *Enlever le cordon d'alimentation et le remettre dedans*  


## Configuration de base du routeur Cisco

## 4.Renommer le routeur
- *hostname Bourges-routeur*

## 5.Créer un utilisateur avec privilèges admin
- *username user privilege 15 secret motdepasse*

## 6.Activer le login via SSH sur les lignes VTY
- *line vty 0 4*
- *login local*
- *transport input ssh*

## <p align="center">Configuration des interfaces réseau</p>
<img width="900" height="700" alt="image" src="https://github.com/user-attachments/assets/0e3b59af-9c27-4530-a9e2-d913e3ee79c3" />

## Interface vers le fournisseur d'accès
| interface GigabitEthernet0/0|
| ------------ |
| ip address 192.x.x.x 255.x.x.x|
| ip nat outside  |

## Interface vers le réseau local (VLANs configurés)
| interface GigabitEthernet0/1|
| ------------ |
| no ip address|

## Interface VLAN 210 (ex : réseau utilisateur)
| interface GigabitEthernet0/1.210|
| ------------ |
| encapsulation dot1Q 210|
| ip address 172.x.x.x 255.x.x.x|
| ip nat inside |


## Routage (Route par défaut, NAT-PAT, ACL)
<img width="1100" height="600" alt="image" src="https://github.com/user-attachments/assets/16fe2fab-ca7d-40e1-a5d5-53327da81ae8" />


## Route par défaut vers le FAI
- *ip route 0.0.0.0 0.0.0.0 183.x.x.x*


## Configuration du NAT


## ACL pour NAT (autorise les hôtes LAN à sortir)
- *access-list 1 permit 172.x.x.x 0.0.0.255*

## Activer le NAT avec surcharge (PAT)
- *ip nat inside source list 1 interface GigabitEthernet0/0 overload*

## Mise en place de HSRP

### Étape 1 : Vérifier les prérequis

- Vérifier que les deux routeurs supportent HSRP (modèle + version IOS).
- Vérifier que les interfaces reliées au VLAN HSRP sont opérationnelles.

```bash
show version
show ip interface brief
```

---

### Étape 2 : Configurer les adresses IP des interfaces

Sur **chaque routeur**, configurer l’interface (ou sous‑interface) qui porte le VLAN où HSRP sera actif (ex. VLAN 219) :

```bash
configure terminal
interface GigabitEthernet0/1.219
 encapsulation dot1Q 219
 ip address <IP_routeur> <masque>
 no shutdown
exit
```

> Objectif : chaque routeur a sa propre IP dans le même réseau (par exemple `.252` et `.253`).

---

### Étape 3 : Activer HSRP et définir l’IP virtuelle

Sur **les deux routeurs**, sur l’interface du VLAN HSRP :

```bash
interface GigabitEthernet0/1.219
 standby 1 ip <IP_virtuelle>
```

- `1` = numéro de groupe HSRP.
- `<IP_virtuelle>` = IP de passerelle commune utilisée par les clients (ex. `.254`).

---

### Étape 4 : Définir les priorités et la préemption

Sur le **routeur qui doit être actif** par défaut (ex. Fibre) :

```bash
interface GigabitEthernet0/1.219
 standby 1 priority 110
 standby 1 preempt
```

Sur le **routeur standby** (ex. ADSL) :

```bash
interface GigabitEthernet0/1.219
 standby 1 priority 100
 standby 1 preempt
```

- La priorité la plus haute devient **Active**.
- La commande `preempt` permet au routeur prioritaire de **reprendre** le rôle actif lorsqu’il redevient disponible.

---

### Étape 5 : Vérifier l’état HSRP

Sur chaque routeur :

```bash
show standby brief
show standby interface GigabitEthernet0/1.219
```

Vérifier que :

- un routeur est en état **Active** ;
- l’autre en état **Standby** ;
- l’IP virtuelle est correcte.

---

### Étape 6 : Vérifier la route par défaut et le NAT

Sur chaque routeur, vérifier que la sortie vers Internet est fonctionnelle :

```bash
show run | include ip route 0.0.0.0
```

Exemple de configuration (si besoin) :

```bash
ip route 0.0.0.0 0.0.0.0 <next-hop_FAI>
```

NAT (exemple simple) :

```bash
access-list 1 permit <réseau_interne> <wildcard>
ip nat inside source list 1 interface <interface_WAN> overload
```

Marquer les interfaces :

```bash
interface <LAN_HSRP>
 ip nat inside

interface <WAN>
 ip nat outside
```

---

### Étape 7 : Tester le fonctionnement normal

Depuis un poste client (ou un routeur interne) dont la passerelle est **l’IP virtuelle** :

- Ping vers la passerelle virtuelle (`ping <IP_virtuelle>`).
- Ping vers une adresse Internet / site externe.
- Vérifier qu’en fonctionnement nominal, c’est bien le routeur prioritaire qui est **Active** (`show standby brief`).

---

### Étape 8 : Tester la bascule HSRP

1. Sur le routeur **actif**, simuler une panne de l’interface HSRP :

```bash
interface GigabitEthernet0/1.219
 shutdown
```

2. Vérifier :

   - que l’autre routeur passe en **Active** (`show standby brief`) ;
   - que les pings / la navigation Internet continuent à fonctionner depuis les clients.

3. Rétablir l’interface sur le routeur prioritaire :

```bash
interface GigabitEthernet0/1.219
 no shutdown
show standby brief
```

Vérifier qu’il redevient **Active** grâce à la préemption.

---

### Étape 9 : Sauvegarder la configuration

Sur chaque routeur :

```bash
write memory
# ou
copy running-config startup-config
```
