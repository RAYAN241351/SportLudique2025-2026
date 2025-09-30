## Connexion au firewall

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

### Image 1

## Configuration générale

Par défaut, le firewall déconnecte l'utilisateur après une période d'inactivité.  
Il est possible de modifier ce comportement en cliquant sur la **flèche à droite de l'icône représentant l'utilisateur connecté**, en haut à droite, puis sur **Préférences**.

### Image 2

Dans la zone **Paramètres de connexion**, sélectionnez dans la liste **« Déconnexion en cas d'inactivité »** la valeur :  
👉 **Toujours rester connecté**

---

### Image 3

Pour **renommer le firewall**, accédez au menu :  
**Configuration → Système → Configuration**

Le volet **Configuration générale** s’affiche.

💡 **Conseil :**  
Il est préférable de mettre les **logs en anglais** (option *Langue du pare-feu (traces)*) afin de faciliter les recherches de problèmes dans la documentation Stormshield ou sur les forums, en utilisant les bons mots-clés.

---

### Sécurité — Changement du mot de passe administrateur

Pour des raisons de sécurité, il est nécessaire de **changer le mot de passe du firewall**.

La modification du mot de passe administrateur se fait dans :  
**Configuration → Système → Administrateurs → Onglet "Compte ADMIN"**

### Image 5

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

### Image 6

---

### Configuration d'une interface virtuelle (VLAN)

Pour créer une interface virtuelle (VLAN), procédez comme suit :

1. Dans une interface physique, cliquez sur le bouton **Ajouter** (en haut de la liste).  
2. Choisissez **VLAN → "nom de l’interface"**.

### Image 7

Une page de configuration apparaît.  
Dans la section **Configuration générale**, remplissez les informations du VLAN :

- **Nom** : nom du VLAN  
- **Interface parente** : interface physique à laquelle le VLAN est rattaché  
- **Identifiant** : ID du VLAN  
- **Interface interne/externe** : à définir selon le rôle du réseau

### Image 8

Dans la section **Plan d’adressage**, la configuration est identique à celle d’une interface physique :  
- Sortez du mode *bridge*  
- Choisissez le mode **Dynamique** ou **Statique**

En sélectionnant **IP fixe (statique)**, il sera possible d’ajouter une adresse IP sous la forme :  
`IP/masque`

### Image 9
