# renommer le routeur
Hostname Bourges-routeur

# Creer un login et un mot de passe
username "user" privilege 15 secret "motdepasse"

# mettre une ip a une interface (vers le fournisseur d'acces)
interface gigabitethernet0/0
    ip address 192.x.x.x 255.x.x.x
    ip nat outside

# mettre une ip a une interface (vers le réseau local)
interface gigabitethernet0/1
    no ip address (car les ip sont définis sur les vlan)

# mettre une ip sur les interface virtuel (vlan)
interface gigabitethernet0/1.210
    encapsulation dot1Q 210
    ip address 172.x.x.x 255.x.x.x

# ajouter une route par défaut
ip route 0.0.0.0 0.0.0.0 183.x.x.x(ip du fournisseur d'acces)

# mettre en place le NAT (Networking Address Translation)
ip nat inside source list 1 interface gigabitethernet0/1 overload (partie lan de notre réseau)

# mettre en place les ACL (regle pour pemmetre au interface d'aller vers l'ip du fournisseur d'acces) 
access-list 1 permit 172.x.x.x x.x.x.255

