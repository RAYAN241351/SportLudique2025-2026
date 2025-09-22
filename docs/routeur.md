```bash
! -------------------------------------------------
! 🔧 Configuration de base du routeur Cisco
! -------------------------------------------------

! 📛 Renommer le routeur
hostname Bourges-routeur

! 👤 Créer un utilisateur avec privilèges admin
username user privilege 15 secret motdepasse

! 🔐 Activer le login via SSH sur les lignes VTY
line vty 0 4
    login local
    transport input ssh

! -------------------------------------------------
! 🌐 Configuration des interfaces réseau
! -------------------------------------------------

! 🌍 Interface vers le fournisseur d'accès
interface GigabitEthernet0/0
    ip address 192.x.x.x 255.x.x.x
    ip nat outside

! 🏠 Interface vers le réseau local (VLANs configurés)
interface GigabitEthernet0/1
    no ip address

! 💻 Interface VLAN 210 (ex : réseau utilisateur)
interface GigabitEthernet0/1.210
    encapsulation dot1Q 210
    ip address 172.x.x.x 255.x.x.x
    ip nat inside

! -------------------------------------------------
! 🛣️ Routage
! -------------------------------------------------

! 🌐 Route par défaut vers le FAI
ip route 0.0.0.0 0.0.0.0 183.x.x.x

! -------------------------------------------------
! 🔄 Configuration du NAT
! -------------------------------------------------

! 🎯 ACL pour NAT (autorise les hôtes LAN à sortir)
access-list 1 permit 172.x.x.x 0.0.0.255

! 🔁 Activer le NAT avec surcharge (PAT)
ip nat inside source list 1 interface GigabitEthernet0/0 overload

! -------------------------------------------------
! ✅ Fin de configuration
! -------------------------------------------------

