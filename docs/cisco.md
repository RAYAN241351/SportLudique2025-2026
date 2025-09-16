nom du domain Bourges.local
# Créer le VLAN 110 et passer le port en mode access et l'assigner à ce VLAN (qui sera utilisé pour configurer le switch)
*config*
*vlan database* 
*vlan 110* 
*end*
*config*
*int*
*switchport mode access*
*switchport access vlan*
*no shut*
# Assigner le nom Management au vlan 110
*configure terminal*
*vlan 110*
*name Management*
# Assigner l'adresse ip au VLAN 110 
*ip address ip_cisco masque* 
# Enregistrer la configuration
*write memory*
# Configuration côté Switch Cisco 9200L
interface range gigabitEthernet 1/0/23 - 24
 channel-group 1 mode active
 no shutdown

 interface Port-channel1
 description LACP vers Routeur 1900
 switchport mode trunk 
 switchport trunk allowed vlan all
 no shutdown
 
 # Mettre en place un relais dhcp sur un switch
 ip routing

    Crée la SVI pour le VLAN 211 (si ce n’est pas déjà fait) :

interface Vlan211
 ip address 192.168.211.1 255.255.255.0
 no shutdown

    Ajoute la configuration du relais DHCP sur la SVI :

interface Vlan211
 ip helper-address 192.168.210.10
