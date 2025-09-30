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
