## installation de Unbound sous debian

<img width="1040" height="549" alt="image" src="https://assets.techrepublic.com/uploads/2022/06/how-to-install-unbound-dns.jpeg"/>

installer les paquet

```
sudo apt update
sudo apt install unbound
```
## Créez ou modifiez le fichier de configuration principal :

```
sudo nano /etc/unbound/unbound.conf.d/custom.conf
```
éffectuer la configuration dans ce fichier 

```
server:## installation de Unbound sous debian

<img width="1040" height="549" alt="image" src="https://assets.techrepublic.com/uploads/2022/06/how-to-install-unbound-dns.jpeg"/>

installer les paquet

```
sudo apt update
sudo apt install unbound
```
## Créez ou modifiez le fichier de configuration principal :

```
sudo nano /etc/unbound/unbound.conf.d/custom.conf
```
éffectuer la configuration dans ce fichier 

```
server:
        verbosity: 0
        use-syslog: yes
 
        interface: 0.0.0.0
        port: 53
 
        do-not-query-localhost: no
        access-control: 127.0.0.0/8 allow
        access-control: ::1 allow
        access-control: 192.168.110.0/24 allow
        access-control: 172.28.128.0/24 allow
        access-control: 192.168.18.0/24 allow
        access-control: 172.28.129.0/24 allow
        access-control: 172.28.159.0/25 allow
 
forward-zone:
        name: "sportludique.fr"
        forward-addr: 121.183.90.205
 
#forward-zone:
       # name: "brg.bourges.sportludique.fr"
       # forward-addr: 172.28.128.2
 
forward-zone:
        name: "bourge.sportludique.fr"
        forward-addr: 172.28.128.70
 ```
## redémarrer unbound après avoir modifier la configuration

```
sudo systemctl restart unbound
```
## Pour vérifier le fonctionnement de la configuration, tester une résolution depuis le serveur
```
dig @127.0.0.1 www.google.com
```
## Puis faire le test sur un poste externe 

**machine windows**
```
nslookup www.google.com 192.168.x.x
```

        verbosity: 0
        use-syslog: yes
 
        interface: 0.0.0.0
        port: 53
 
        do-not-query-localhost: no
        access-control: 127.0.0.0/8 allow
        access-control: ::1 allow
        access-control: 192.168.110.0/24 allow
        access-control: 172.28.128.0/24 allow
        access-control: 192.168.18.0/24 allow
        access-control: 172.28.129.0/24 allow
        access-control: 172.28.159.0/25 allow
 
forward-zone:
        name: "sportludique.fr"
        forward-addr: 121.183.90.205
 
#forward-zone:
       # name: "brg.bourges.sportludique.fr"
       # forward-addr: 172.28.128.2
 
forward-zone:
        name: "bourges.sportludique.fr"
        forward-addr: 172.28.128.70
 ```
## redémarrer unbound après avoir modifier la configuration

```
sudo systemctl restart unbound
```
## Pour vérifier le fonctionnement de la configuration, tester une résolution depuis le serveur
```
dig @127.0.0.1 www.google.com
```
## Puis faire le test sur un poste externe 

**machine windows**
```
nslookup www.google.com 192.168.x.x
```
## Dans l'active directory 

Aller dans le **gestionnaire DNS** puis mettre l'ip du résolveur en temps que redirecteur.
