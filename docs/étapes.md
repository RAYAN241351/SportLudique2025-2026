# Mise en place du routeur 
D'abord nous avons installé Putty afin de nous connecter sur le switch connecté sur en DB9 sur le pc, mais cela n'a pas marché donc nous avons téléchargé screen grace à la commande
Renommer le 

# Installer putty et ses outils
*sudo apt install putty putty-tools* 
# Installer screen (essentiel pour se connecter au switch)
*sudo apt install screen*
# Lancer le screen du switch cisco connecté de DB9
*sudo screen VTTYS0*
# Installer SSH 
*hostname Bourges*
*ip domain-name bourges.local*
*crypto key generate rsa modulus 2048*
*ip ssh version 2*
*line vty 0 4*
*transport input ssh*
*login local*
*username operateur operateur 123456*

# Se connecter en SSH 
*ssh nom d'utilisateur@ip_du_routeur (à assigner plus tard)*
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