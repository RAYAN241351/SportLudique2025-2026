# Configuration et mise en place de l'access point DWL-3200AP 
<img width="1664" height="936" alt="image" src="https://github.com/user-attachments/assets/9bc470c7-2d3b-447b-a42e-e333d9eaeb3d" />

## Reinitialisation du DWL-3200AP
<img width="813" height="721" alt="image" src="https://github.com/user-attachments/assets/ec5c3d80-b1d7-4342-8491-0bc0fc25a981" />
Appuyer sur le **bouton RESET** à l'aide d'un **trombone** afin de reinitialiser l'Access Point 

## Desactiver le broadcast du SSID 
<img width="1433" height="714" alt="image" src="https://github.com/user-attachments/assets/f19319f3-1bf2-407f-b25d-d5ff28469f1a" />
Dans **Basic Settings** > **Wireless** > **SSID Broadcast**, changez le SSID Broadcast de Enable en Disabled.
**Explication:** quand le **SSID n'est plus diffusé**, il **disparaît des listes WiFi automatiques**, mais on peut **encore s'y connecter manuellement en tapant exactement son nom + mot de passe**. C'est juste moins visible, pas sécurisé.

## Configuration ip 
Dans **Basic Settings > LAN**, il est possible de mettre l'ip de son choix.

**Attention**: en changeant l'adresse ip vous ne serait plus sur la meme plage ip que l'équipement, vous serez donc déconnecter.

# Configuration sur le stormshield

Maintenant que la borne wifi est prete, le reste de la configuration se fera sur le pare feu (en haut de la DMZ), dans notre cas c'est un stormshield.

## Portail captif

Un portail captif est une page web de contrôle qui s’affiche automatiquement lorsqu’on se connecte à un réseau. Tant que l’utilisateur ne passe pas par cette page, il n’aura aucun accès à Internet.
Pour des raisons légales, le portail captif sert à informer l’utilisateur que ses données seront conservées pour des raisons de traçabilité.

<img width="1170" height="863" alt="image" src="https://github.com/user-attachments/assets/d43e3265-bce7-4ace-935f-2c495920d206" />

## Authentification

**Il y a 3 mode d'authentification via un portail captif**

Mode **Guest** : l'utilisateur renseigne ses information sur le portail.

Mode **Vouchers** : L’accès est basé sur des codes d’accès temporaires (vouchers) générés par un administrateur, chaque code peut être limité en durée et en nombre d’utilisations.

Mode **Parrainage** : Un utilisateur déjà authentifié (employé, responsable, etc.) doit valider la demande d’accès d’un invité.

**Nous utiliserons la méthode Guest (invité)**

Aller dans **utilisateurs** > **Authentification** > **Méthode disponible**

activons la méthode `invité`

La fréquence d'affichage des conditions d'utilisation de l'accès à Internet correspond a la durée d'acces a internet de l'utilisateur.

<img width="871" height="554" alt="image" src="https://github.com/user-attachments/assets/c0f897f3-a0f9-4ab2-8051-d9f2cd388f73" />

Dans l'onglet **politique d'authentification** Ajoutons une règle en sélectionnant Règle Invités.​

<img width="663" height="193" alt="image" src="https://github.com/user-attachments/assets/f2fc8fb8-9c8e-437e-93bd-848f7d7314a8" />

Allons ensuite dans l'onglet **profil du partail captif**

sélectionnons le profil `Guest`,  Ce profil est préconfiguré avec les paramètres nécessaires, tels que la méthode d'authentification par défaut et l'activation des conditions d'utilisation de l'accès à Internet.​
nous pouvons aussi personnaliser les champs supplémentaires tels que e-mail ou prénom.

<img width="899" height="879" alt="image" src="https://github.com/user-attachments/assets/f1934c2d-f272-4da0-8aa1-b2c388d18e70" />

Pour finir dans l'onglet **portail captif** il faut associer le profil du portail captif aux interfaces réseau concernées 
