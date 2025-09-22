
# Résumé du RAID
| RAID | Min. Disques | Redondance | Capacité utile    | Performance     | Tolérance aux pannes         |
|------|--------------|------------|-------------------|-----------------|------------------------------|
| RAID 0 | 2          | ❌ Non     | 100%              | 🔥 Excellente   | ❌ Aucune (perte totale)     |
| RAID 1 | 2          | ✅ Oui     | 50%               | 👍 Bonne (lecture) | ✅ 1 disque                  |
| RAID 5 | 3          | ✅ Oui     | (N-1)/N           | 👍 Bonne        | ✅ 1 disque                  |
| RAID 6 | 4          | ✅✅ Oui   | (N-2)/N           | 👌 Correcte     | ✅ 2 disques                 |
| RAID 10 | 4 (paire) | ✅ Oui     | 50%               | 🔥 Excellente   | ✅ 1 disque/paire (min 2)    |


# Configuration SWITCH-SALLE-SERVEUR 
### Configuration de LACP (EtherChannel) sur 2 ports:
| Interface GigabitEthernet1/0/23-24|
|-----------------------------------|
| switchport mode trunk             |
| channel-group 1 mode active       |
| no shutdown                        |

|interface Port-channel1|
|-----------------------| 
| description LACP vers Switch Coeur|
| switchport mode trunk             |
| switchport trunk allowed vlan all |
| no shutdown                       |
