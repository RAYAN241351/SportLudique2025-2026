#  Apache2 | Serveur Web.md
<img width="620" height="485" alt="image" src="https://github.com/user-attachments/assets/8cfce6b7-de11-4b7f-85bc-30f5cd990973" />

## Introduction à Apache2





## Installation d'Apache2 
- ```sudo apt install apache2```


## Commandes utiles
- Pour arrêter apache2 : ```sudo systemctl stop apache2```
- Pour lancer apache2 : ```sudo systemctl start apache2```
- Pour relancer apache2 : ```sudo systemctl restart apache2```
- Pour recharger la configuration d'apache2 : ```sudo systemctl reload apache2```
- Pour voir la version d'Apache utilisée : ```sudo apache2ctl -v ou a2query -v```
- Pour tester l'ensemble de la configuration d'Apache : ```sudo apache2ctl -t```
- Pour lister les hôtes virtuels chargés : ```a2query -s```
- Pour lister les hôtes virtuels chargés et leurs configurations : ```sudo apache2ctl -S```
- Pour tester la configuration des hôtes virtuels : ```sudo apache2ctl -t -D DUMP_VHOSTS```
- Pour voir les modules d'Apache chargés : ```sudo apache2ctl -M ou a2query -m```
