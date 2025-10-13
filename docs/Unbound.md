## installation de Unbound sous debian

installer les paquet

```
sudo apt update
sudo apt install unbound
```
### Créez ou modifiez le fichier de configuration principal :

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