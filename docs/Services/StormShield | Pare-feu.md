

# StormShield | Pare-feu
<img width="1040" height="549" alt="image" src="https://github.com/user-attachments/assets/b4efca76-f72f-4a8c-aef4-7db5fde31154" />

Pour accéder à l'interface d'administration du pare-feu **SNS**, il est indispensable de connecter la machine cliente à une **interface interne (port LAN)**.

### Configuration d'usine

- Le firewall est en **mode bridge** par défaut.  
- Son adresse IP est : `10.0.0.254/8`  
- Un service **DHCP** fournit automatiquement des adresses IP sur les ports LAN.

### Accès à l’interface d’administration

1. Obtenez une adresse IP via le **DHCP**.  
2. Ouvrez un navigateur web et accédez à l’interface :  
   👉 [https://ip_firewall/admin](https://ip_firewall/admin)

### Identifiants par défaut

- **Identifiant :** `admin`  
- **Mot de passe :** `admin`

<img width="582" height="751" alt="image" src="https://github.com/user-attachments/assets/7bf7d6cf-2927-449e-aa4a-a9e01f855610" />


## Configuration générale

Par défaut, le firewall déconnecte l'utilisateur après une période d'inactivité.  
Il est possible de modifier ce comportement en cliquant sur la **flèche à droite de l'icône représentant l'utilisateur connecté**, en haut à droite, puis sur **Préférences**.

<img width="558" height="125" alt="image" src="https://github.com/user-attachments/assets/b3c6d271-e185-40e1-9b68-c19a0b6b8247" />


Dans la zone **Paramètres de connexion**, sélectionnez dans la liste **« Déconnexion en cas d'inactivité »** la valeur :  
👉 **Toujours rester connecté**

---

<img width="800" height="96" alt="image" src="https://github.com/user-attachments/assets/cd189fe8-c341-4b22-a666-e1f387ec6f7f" />


Pour **renommer le firewall**, accédez au menu :  
**Configuration → Système → Configuration**

Le volet **Configuration générale** s’affiche.

💡 **Conseil :**  
Il est préférable de mettre les **logs en anglais** (option *Langue du pare-feu (traces)*) afin de faciliter les recherches de problèmes dans la documentation Stormshield ou sur les forums, en utilisant les bons mots-clés.

<img width="800" height="229" alt="image" src="https://github.com/user-attachments/assets/1357a4e0-7421-4776-b2a1-93d64c0e09c3" />


### Sécurité — Changement du mot de passe administrateur

Pour des raisons de sécurité, il est nécessaire de **changer le mot de passe du firewall**.

La modification du mot de passe administrateur se fait dans :  
**Configuration → Système → Administrateurs → Onglet "Compte ADMIN"**

<img width="800" height="267" alt="image" src="https://github.com/user-attachments/assets/f705d880-db1d-45e2-98b8-6fb86ad05fd7" />


## Création d'interfaces

### Configuration d'une interface physique

Pour configurer une interface physique, rendez-vous dans :  
**Configuration → Réseau → Interfaces**

Sélectionnez ensuite l’interface de votre choix :

- **in** → Interface LAN (interne au réseau)  
- **out** → Interface WAN (externe, vers le routeur ou Internet)  
- **DMZ** → Zone démilitarisée

Après avoir sélectionné une interface, il faut **la sortir du mode bridge** en cochant la case **Adressage dynamique/statique**.  
Choisissez ensuite un mode d’adressage :
- **Statique** : vous entrez manuellement l’adresse IP.  
- **Dynamique (DHCP)** : l’adresse IP est attribuée automatiquement.

<img width="800" height="409" alt="image" src="https://github.com/user-attachments/assets/27d5bf11-8d4c-4f2d-a8f6-690c6cea7487" />


---

### Configuration d'une interface virtuelle (VLAN)

Pour créer une interface virtuelle (VLAN), procédez comme suit :

1. Dans une interface physique, cliquez sur le bouton **Ajouter** (en haut de la liste).  
2. Choisissez **VLAN → "nom de l’interface"**.

<img width="800" height="158" alt="image" src="https://github.com/user-attachments/assets/f6a7a8f9-76e2-498f-a304-5f5ece9f5e42" />


Une page de configuration apparaît.  
Dans la section **Configuration générale**, remplissez les informations du VLAN :

- **Nom** : nom du VLAN  
- **Interface parente** : interface physique à laquelle le VLAN est rattaché  
- **Identifiant** : ID du VLAN  
- **Interface interne/externe** : à définir selon le rôle du réseau

<img width="980" height="492" alt="image" src="https://github.com/user-attachments/assets/e9677ba9-f480-4396-80ba-6b553cae45e1" />


Dans la section **Plan d’adressage**, la configuration est identique à celle d’une interface physique :  
- Sortez du mode *bridge*  
- Choisissez le mode **Dynamique** ou **Statique**

En sélectionnant **IP fixe (statique)**, il sera possible d’ajouter une adresse IP sous la forme :  
`IP/masque`

<img width="916" height="320" alt="image" src="https://github.com/user-attachments/assets/a240981d-f3db-4e72-be87-f7d521363c5c" />


## Règles de filtrage et NAT (Politique de sécurité)
<img width="1647" height="171" alt="image" src="https://github.com/user-attachments/assets/e49048f5-c38b-4392-8c8d-16e7da3576d2" />
