# Reinitialisation du routeur CISCO
<img width="1100" height="600" alt="image" src="https://github.com/user-attachments/assets/bae53d48-ddf7-4060-bdc2-7d8e9a99dfb8" />



### 1. Appuyer simultanément sur la touche *CTRL* + *PAUSE*
- *confreg 0x2142*
- *reset*

### 2. Enregistrer la config-running en ayant fait des modifications et *reset* le routeur (ROMMON MODE)
- *hostname Routeur-bourges-fibre*
- *copy running-config startup-config*
- *reset*
  
  




### 3. De nouveau sur le mode ROMMON, on modifie le registre qui charge la configuration enregistrée en mémoire  
- *confreg 0x2102*
- ***Enlever le cordon d'alimentation et le remettre dedans***  
  



  

