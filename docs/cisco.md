```bash
! -------------------------------------------------
!  Création du VLAN 110 pour la gestion
! -------------------------------------------------

vlan 110
    name Management
exit

interface range GigabitEthernet1/0/1
    switchport mode access
    switchport access vlan 110
    no shutdown
exit

! Donner une IP à la SVI du VLAN 110 (interface virtuelle)
interface Vlan110
    ip address [IP_CISCO] [MASQUE]
    no shutdown
exit

! -------------------------------------------------
!  Enregistrement de la configuration
! -------------------------------------------------

write memory

! -------------------------------------------------
!  Configuration d’un EtherChannel en LACP (Cisco 9200L)
! -------------------------------------------------

interface range GigabitEthernet1/0/23 - 24
    channel-group 1 mode active
    no shutdown
exit

interface Port-channel1
    description LACP vers Routeur 1900
    switchport mode trunk
    switchport trunk allowed vlan all
    no shutdown
exit

! -------------------------------------------------
!  Activer le routage IP sur le switch
! -------------------------------------------------

ip routing

! -------------------------------------------------
!  DHCP Relay (relais DHCP) sur VLAN 211
! -------------------------------------------------

! Créer la SVI pour le VLAN 211 (si non existante)
interface Vlan211
    ip address 192.168.211.1 255.255.255.0
    ip helper-address 192.168.210.10
    no shutdown
exit


! -------------------------------------------------
!  Configuration de LACP (EtherChannel) sur 2 ports
! -------------------------------------------------

interface range GigabitEthernet1/0/23 - 24
 switchport mode trunk
 channel-group 1 mode active
 no shutdown
exit

interface Port-channel1
 description LACP vers Routeur 1900
 switchport mode trunk
 switchport trunk allowed vlan all
 no shutdown
exit

! -------------------------------------------------
! 🛣️ Ajouter une route par défaut
! -------------------------------------------------

ip route 0.0.0.0 0.0.0.0 172.x.x.x

! -------------------------------------------------
! 🔐 Comptes utilisateurs
! -------------------------------------------------

username operateur privilege 15 secret [motdepasse]
username Bourges privilege 15 secret [motdepasse]

! -------------------------------------------------
! 🔐 Activer SSH et accès distant
! -------------------------------------------------

ip ssh version 2

line vty 0 4
 login local
 transport input ssh
!
line vty 5 15
 login local
 transport input ssh
!
line con 0
 login local