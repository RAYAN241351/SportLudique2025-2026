# NXLog CE – Collecte de logs Windows vers Graylog

## 1. Présentation
NXLog CE est un agent léger de collecte de logs pour Windows et Linux.  
Il permet :
- La collecte d’EventLogs Windows.
- La conversion en formats standard (GELF, Syslog, JSON).
- L’envoi vers un serveur de log centralisé (Graylog, ELK, etc.).

**Version utilisée** : NXLog CE 3.2.2329 (Windows)  
**Objectif** : Envoyer les EventLogs Windows en GELF UDP vers Graylog.

---

## 2. Installation

### Étapes sur Windows
1. Télécharger NXLog CE depuis le site officiel :  
   [https://nxlog.co/products/nxlog-community-edition/download](https://nxlog.co/products/nxlog-community-edition/download)
2. Exécuter l’installateur `.msi` et suivre les étapes.
3. Choisir **C:\Program Files\nxlog** comme dossier d’installation.
4. Vérifier le service NXLog :
powershell
```
Get-Service nxlog
```

## 3. Arborescence principale

C:\Program Files\nxlog\conf : **fichiers de configuration principaux.**

C:\Program Files\nxlog\conf\nxlog.d : **fichiers de configuration modulaires.**

C:\Program Files\nxlog\data : **logs et spool.**

C:\Program Files\nxlog\modules : **modules NXLog (xm_syslog, xm_gelf, etc.)**

## 4. Configuration NXLog

Fichier principal : `nxlog.conf`

Fichier principal : nxlog.conf

```
define ROOT     C:\Program Files\nxlog
define CONFDIR  %ROOT%\conf\nxlog.d
define LOGDIR   %ROOT%\data

LogFile %LOGDIR%\nxlog.log
Moduledir %ROOT%\modules
CacheDir  %LOGDIR%
Pidfile   %LOGDIR%\nxlog.pid
SpoolDir  %LOGDIR%

# Extensions nécessaires
<Extension gelf>
    Module xm_gelf
</Extension>

<Extension _charconv>
    Module xm_charconv
    AutodetectCharsets iso8859-2, utf-8, utf-16, utf-32
</Extension>

# Input : EventLogs Windows
<Input in>
    Module im_msvistalog
</Input>

# Output : GELF UDP vers Graylog
<Output out>
    Module om_udp
    Host 192.168.110.110        # IP du serveur Graylog
    Port 12201                  # Port du input GELF UDP sur Graylog
    Exec to_gelf();             # Conversion en GELF
</Output>

# Route
<Route r1>
    Path in => out
</Route>
```