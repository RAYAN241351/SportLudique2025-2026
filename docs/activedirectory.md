## Configuration du serveur ActiveDirectory et de ses services DHCP, DNS etc...
<img width="1100" height="800" alt="image" src="https://github.com/user-attachments/assets/77feaa4a-2653-4863-9dda-eaa975e9a701" />

## Installation des roles nécessaires
<img width="781" height="553" alt="image" src="https://github.com/user-attachments/assets/56af54e4-825e-44dd-b73c-ba475afd2528" />

- *Essentiel de prendre la première option car elle permet de créer un serveur en ajoutant des roles, des services de role et des fonctionnalités*

<img width="1023" height="718" alt="image" src="https://github.com/user-attachments/assets/7c51c0df-441f-4979-9fbc-0d11f4088d01" />

<img width="518" height="438" alt="image" src="https://github.com/user-attachments/assets/689c5c94-918b-4dc9-943e-6207fc5efd13" />

-* Mettre un nom et une description comme par exemple 


- *Nous installons les services essentiels pour Ad-Directory, DHCP, DNS, Controlleur de domaine*

## Configuration DHCP (exemple d'un réseau)

## 1.Création de l'étendue 
<img width="518" height="438" alt="image" src="https://github.com/user-attachments/assets/689c5c94-918b-4dc9-943e-6207fc5efd13" />
-* Mettre un nom et une description comme par exemple

<img width="515" height="428" alt="image" src="https://github.com/user-attachments/assets/f35a4e8c-67d0-434a-9bd3-5d9986dec978" />

-*Nous avons juste besoin de renseigner l'IP du début et l'ip de fin qui vont etre assigner aux futures ordinateurs du réseau et et ainsi le masque en décimale et en CIDR*


<img width="514" height="436" alt="image" src="https://github.com/user-attachments/assets/3ac833ab-244b-4df5-9282-b4402f50d0ec" />
-*Nous avons ici la possibilité **d'exclure les adresses IP** qui sont par exemple utiliser par un service quelconque ou un équipement comme un routeur, serveur etc..*

<img width="515" height="430" alt="image (1)" src="https://github.com/user-attachments/assets/c2b1d3f6-55fc-4866-a0f7-23938700416d" />
-*Nous pouvons mettre **la durée du bail** qui veut dire **la durée pendant laquelle un utilisateur peut utiliser une adresse IP dans cette etendue***

<img width="517" height="436" alt="image (3)" src="https://github.com/user-attachments/assets/08e973e5-efdf-40e1-84f3-db6aba373af3" />
-*Nous pouvons attribuer **la passerelle par défaut du routeur** donc l'adresse de celui-ci* 

<img width="519" height="437" alt="image (4)" src="https://github.com/user-attachments/assets/9f4d14b5-5087-4c74-905f-d22e7d997d21" />
-*Nous pouvons mettre le nom du serveur (nom de domaine) pour **tester la résolution DNS** en cliquant sur **Résoudre** pour voir si il arrive à trouver l'adresse IP du serveur ActiveDirectory avec son nom de domaine*

<img width="515" height="440" alt="image (2)" src="https://github.com/user-attachments/assets/e96cce6a-ff03-4b7b-a741-7ac6217d1d5a" />
-*On a la possibilité de soit **l'activer directement** ou sinon de **l'activer prochainement** pour une étendue futur*



