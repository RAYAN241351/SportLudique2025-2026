# Installation complète de GLPI 11 sur Debian 12/13
<img width="500" height="275" alt="image" src="https://github.com/user-attachments/assets/294694fe-3481-4a86-a8fd-d04035acc118" />

Procédure d'installation de GLPI 11 avec Apache2, PHP-FPM et MariaDB.

---

## 1. Présentation

GLPI (Gestion Libre de Parc Informatique) est une solution web libre de gestion de parc, helpdesk et ITSM.

GLPI 11 repose sur :
- Un serveur web (Apache ou Nginx)
- PHP (PHP-FPM recommandé)
- Une base de données MariaDB ou MySQL

---

## 2. Prérequis

### 2.1 Système et réseau

- Debian 12 ou 13
- Accès root ou sudo
- IP fixe ou réservation DHCP
- Nom DNS pointant vers le serveur GLPI
- Accès réseau : SSH (administration) et HTTP/HTTPS (clients)

### 2.2 Logiciels requis

- Apache2
- MariaDB (≥ 10.2) ou MySQL (≥ 5.7)
- PHP compatible GLPI 11

### 2.3 Extensions PHP requises/recommandées

`curl` `gd` `intl` `mysqli` `session` `json` `xml` `mbstring` `zip` `bz2` `imap` `ldap` `apcu`

---

## 3. Mise à jour du système

sudo apt update && sudo apt upgrade -y



---

## 4. Installation de la stack LAMP

### 4.1 Apache2 et MariaDB

sudo apt install -y apache2 mariadb-server



### 4.2 PHP-FPM et extensions

Adapter la version PHP :
- Debian 12 → PHP 8.2
- Debian 13 → PHP 8.4

sudo apt install -y
php php-fpm php-mysql php-gd php-intl php-curl php-xml php-zip
php-mbstring php-bz2 php-imap php-apcu php-ldap php-common php-json



---

## 5. Configuration de MariaDB

### 5.1 Sécurisation

sudo mysql_secure_installation



### 5.2 Création de la base GLPI

sudo mysql


undefined

CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpiuser'@'localhost' IDENTIFIED BY 'MotDePasseFort';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpiuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;



---

## 6. Téléchargement de GLPI 11

### 6.1 Récupération

cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/11.0.0/glpi-11.0.0.tgz



### 6.2 Installation

sudo tar xvf glpi-11.0.0.tgz -C /var/www/
sudo mv /var/www/glpi /var/www/glpi-11
sudo ln -s /var/www/glpi-11 /var/www/glpi



---

## 7. Organisation et sécurisation des fichiers

### 7.1 Configuration GLPI

sudo mkdir -p /etc/glpi
sudo chown www-data:www-data /etc/glpi
sudo mv /var/www/glpi/config /etc/glpi



### 7.2 Données GLPI

sudo mkdir -p /var/lib/glpi
sudo mv /var/www/glpi/files /var/lib/glpi
sudo chown -R www-data:www-data /var/lib/glpi



### 7.3 Journaux

sudo mkdir -p /var/log/glpi
sudo chown www-data:www-data /var/log/glpi





## 8. Configuration des chemins

### 8.1 downstream.php

```php
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}
```

### 8.2 local_define.php

```bash
sudo nano /etc/glpi/local_define.php
```

Contenu :

```php
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi/files');
define('GLPI_LOG_DIR', '/var/log/glpi');
```

---

## 9. Permissions

```bash
sudo chown -R www-data:www-data /var/www/glpi-11
sudo chown -R www-data:www-data /etc/glpi /var/lib/glpi /var/log/glpi
sudo find /var/www/glpi-11 -type d -exec chmod 750 {} \;
sudo find /var/www/glpi-11 -type f -exec chmod 640 {} \;
```

---

## 10. Configuration Apache2

### 10.1 VirtualHost GLPI

```bash
sudo nano /etc/apache2/sites-available/glpi.conf
```

Contenu :

```apache
<VirtualHost *:80>
    ServerName glpi.mondomaine.local
    DocumentRoot /var/www/glpi/public

    <Directory /var/www/glpi/public>
        Require all granted
        AllowOverride All
        Options FollowSymlinks

        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/glpi-error.log
    CustomLog ${APACHE_LOG_DIR}/glpi-access.log combined

    <FilesMatch "\.php$">
        SetHandler "proxy:unix:/run/php/php8.2-fpm.sock|fcgi://localhost/"
        # Debian 13 / PHP 8.4 :
        # SetHandler "proxy:unix:/run/php/php8.4-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>
```

### 10.2 Activation

```bash
sudo a2enmod proxy_fcgi setenvif rewrite
sudo a2enconf php*-fpm
sudo a2ensite glpi.conf
sudo systemctl reload apache2
```

---

## 11. Paramètres PHP recommandés

```bash
sudo nano /etc/php/8.2/fpm/php.ini
```

Paramètres à ajuster :

```ini
memory_limit = 256M
upload_max_filesize = 20M
post_max_size = 20M
date.timezone = Europe/Paris
```

Redémarrer PHP-FPM :

```bash
sudo systemctl restart php8.2-fpm
```

---

## 12. Installation via l'interface web

Accéder à :

* [http://glpi.mondomaine.local](http://glpi.mondomaine.local)
* http://IP_DU_SERVEUR

### Étapes

1. Langue → Français
2. Licence GPL → Installer
3. Base de données :

   * Hôte : `localhost`
   * Utilisateur : `glpiuser`
   * Mot de passe : `MotDePasseFort`
   * Base : `glpi`

### Comptes par défaut (à modifier immédiatement)

| Rôle          | Login     | Mot de passe |
| ------------- | --------- | ------------ |
| Admin         | glpi      | glpi         |
| Technicien    | tech      | tech         |
| Utilisateur   | normal    | normal       |
| Lecture seule | post-only | post-only    |

---

## 13. Post-installation

### 13.1 Suppression de l'installateur

```bash
sudo find /var/www/glpi-11 -name "install.php" -delete
```

### 13.3 Sauvegardes

Sauvegarder régulièrement :

* Base de données `glpi`
* Dossiers :

  * `/etc/glpi`
  * `/var/lib/glpi`
  * `/var/log/glpi`
* Plugins

---

## 14. Récapitulatif rapide

```bash
# Mise à jour
sudo apt update && sudo apt upgrade -y

# Apache & MariaDB
sudo apt install -y apache2 mariadb-server

# PHP
sudo apt install -y \
  php php-fpm php-mysql php-gd php-intl php-curl php-xml php-zip \
  php-mbstring php-bz2 php-imap php-apcu php-ldap php-common php-json

# Sécurisation DB
sudo mysql_secure_installation

# Base GLPI
sudo mysql <<EOF
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpiuser'@'localhost' IDENTIFIED BY 'MotDePasseFort';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpiuser'@'localhost';
FLUSH PRIVILEGES;
EOF

# Téléchargement GLPI
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/11.0.0/glpi-11.0.0.tgz
sudo tar xvf glpi-11.0.0.tgz -C /var/www/
sudo mv /var/www/glpi /var/www/glpi-11
sudo ln -s /var/www/glpi-11 /var/www/glpi

# Organisation fichiers
sudo mkdir -p /etc/glpi /var/lib/glpi /var/log/glpi
sudo mv /var/www/glpi/config /etc/glpi
sudo mv /var/www/glpi/files /var/lib/glpi
sudo chown -R www-data:www-data /etc/glpi /var/lib/glpi /var/log/glpi /var/www/glpi-11

# downstream.php
cat <<'PHP' | sudo tee /var/www/glpi/inc/downstream.php
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}
PHP

# local_define.php
cat <<'PHP' | sudo tee /etc/glpi/local_define.php
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi/files');
define('GLPI_LOG_DIR', '/var/log/glpi');
PHP

# Permissions
sudo chown -R www-data:www-data /var/www/glpi-11 /etc/glpi /var/lib/glpi /var/log/glpi
sudo find /var/www/glpi-11 -type d -exec chmod 750 {} \;
sudo find /var/www/glpi-11 -type f -exec chmod 640 {} \;

# VirtualHost Apache (à configurer manuellement)
sudo nano /etc/apache2/sites-available/glpi.conf

# Activation
sudo a2enmod proxy_fcgi setenvif rewrite
sudo a2enconf php*-fpm
sudo a2ensite glpi.conf
sudo systemctl reload apache2
```

---

