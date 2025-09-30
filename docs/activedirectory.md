## Configuration du serveur ActiveDirectory et de ses services DHCP, DNS etc...
<img width="1100" height="800" alt="image" src="https://github.com/user-attachments/assets/77feaa4a-2653-4863-9dda-eaa975e9a701" />

## Installation des roles nécessaires
<img width="781" height="553" alt="image" src="https://github.com/user-attachments/assets/56af54e4-825e-44dd-b73c-ba475afd2528" />


<img width="1023" height="718" alt="image" src="https://github.com/user-attachments/assets/7c51c0df-441f-4979-9fbc-0d11f4088d01" />
*Lors de l’installation du serveur, il est essentiel de sélectionner l’option permettant l’ajout de rôles, services de rôle et fonctionnalités.*
*Ceci permet d’installer les services fondamentaux :*

-**Active Directory Domain Services (AD DS)**

-**DHCP Server**

-**DNS Server**

-**Contrôleur de domaine**

## Configuration DHCP (exemple d'un réseau)
*le DHCP permet **d’attribuer automatiquement** des adresses IP aux ordinateurs du réseau.*
## 1.Création de l'étendue 



<img width="518" height="438" alt="image" src="https://github.com/user-attachments/assets/689c5c94-918b-4dc9-943e-6207fc5efd13" />
*Une étendue DHCP **définit la plage d’adresses IP** qui pourra être attribuée aux clients du réseau.*
-*Donner un **nom** et une **description** à l’étendue (ex. : "DHCP_Réseau_Local")*




<img width="515" height="428" alt="image" src="https://github.com/user-attachments/assets/f35a4e8c-67d0-434a-9bd3-5d9986dec978" />

-*Renseigner **l’adresse IP de début** et **l’adresse IP de fin**.*
-*Définir le **masque de sous-réseau** (**notation décimale** ou **CIDR**).*




<img width="514" height="436" alt="image" src="https://github.com/user-attachments/assets/3ac833ab-244b-4df5-9282-b4402f50d0ec" />
-*Possibilité **d’exclure certaines adresses IP** (ex. : celles d’un routeur, d’un serveur ou d’un autre équipement fixe).*






<img width="515" height="430" alt="image (1)" src="https://github.com/user-attachments/assets/c2b1d3f6-55fc-4866-a0f7-23938700416d" />
-*Définir la **durée du bail DHCP**, c’est-à-dire **le temps pendant lequel un client peut utiliser une adresse IP attribuée.***







<img width="517" height="436" alt="image (3)" src="https://github.com/user-attachments/assets/08e973e5-efdf-40e1-84f3-db6aba373af3" />
-*Ajouter **l’adresse IP de la passerelle par défaut** (souvent l’IP du routeur).* 





<img width="519" height="437" alt="image (4)" src="https://github.com/user-attachments/assets/9f4d14b5-5087-4c74-905f-d22e7d997d21" />
-*Indiquer le **nom du serveur DNS** (ex. : domaine.local).**
-*Tester la **résolution DNS** avec l’option **“Résoudre”** afin de vérifier que **l’Active Directory est bien joignable***.





<img width="515" height="440" alt="image (2)" src="https://github.com/user-attachments/assets/e96cce6a-ff03-4b7b-a741-7ac6217d1d5a" />
-*Possibilité **d’activer immédiatement l’étendue**, ou de la **laisser inactive pour une utilisation future.***



