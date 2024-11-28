# INSTALL GUIDE SCRIPT CREATION OU "Create_OU.ps1"

## Prérequis

Avant d'exécuter le script, assurez-vous que votre environnement est correctement configuré et que les éléments suivants sont en place :

1. **PowerShell** : Le script est conçu pour être exécuté dans PowerShell, qui est installé par défaut sur les systèmes Windows.
2. **Module Active Directory** : Vous devez avoir installé le module `ActiveDirectory` pour interagir avec Active Directory via PowerShell. Ce module est disponible dans les **Outils d'Administration de Serveur (RSAT)**.
3. **Accès à Active Directory** : Vous devez disposer des droits d'administrateur pour créer des OUs et modifier les objets dans Active Directory.

## Étapes d'Installation du Script

### 1. Télécharger le Script

Téléchargez le script `Create-OU.ps1` et placez-le dans un répertoire accessible sur votre serveur ou votre machine Windows.

- Vous pouvez créer un dossier nommé `Scripts` dans **C:\** ou **D:\** pour organiser vos scripts.
  
### 2. Préparer le Fichier CSV

Le script nécessite un fichier CSV contenant les informations sur les départements ou services à créer. Le format du fichier CSV doit correspondre aux colonnes utilisées dans le script. Par défaut, les colonnes attendues sont :

- **Nom** (Nom du département ou service)
- **Service** (Nom du service ou département)
  
Exemple de fichier CSV :

```csv
Nom,Service
Département A,Développement
Département B,Marketing
```

### 3. Modifier les Paramètres du Script

Avant d'exécuter le script, vous devrez peut-être ajuster certains paramètres dans le fichier `Create-OU.ps1` pour qu'ils correspondent à votre environnement Active Directory. Les principaux paramètres à vérifier sont :

- **$csvPath** : Le chemin d'accès au fichier CSV.
  ```powershell
  $csvPath = "C:\chemin\vers\votre\fichier.csv"
  ```

- **$parentOU** : Le chemin d'accès à l'OU parent sous laquelle les nouvelles OUs seront créées.
  ```powershell
  $parentOU = "OU=Departments,DC=monDomaine,DC=com"
  ```

- **$ouProtection** : Si vous souhaitez désactiver la protection contre la suppression accidentelle, laissez cette option activée :
  ```powershell
  $ouProtection = $true
  ```

### 4. Exécuter le Script

Une fois les étapes précédentes complétées, ouvrez **PowerShell** en tant qu'administrateur et exécutez le script en utilisant la commande suivante :

```powershell
cd C:\cheminers\le\script
.\Create-OU.ps1
```

Cela exécutera le script, créera les OUs spécifiées dans le fichier CSV et supprimera la protection contre la suppression accidentelle des OUs créées.

### 5. Vérification des Résultats

Après l'exécution du script, vous pouvez vérifier si les OUs ont été correctement créées dans Active Directory en utilisant la console **Utilisateurs et ordinateurs Active Directory** (Active Directory Users and Computers). Les nouvelles OUs devraient apparaître sous l'OU parent spécifiée.

## Résolution des Problèmes

### Problème : **Accès refusé**

Si vous obtenez une erreur d'accès refusé, assurez-vous que vous exécutez PowerShell avec des droits d'administrateur et que votre compte dispose des autorisations nécessaires pour créer des OUs dans Active Directory.

### Problème : **Erreur de syntaxe dans le fichier CSV**

Si le script rencontre des erreurs liées au format du fichier CSV, assurez-vous que les colonnes sont bien nommées et qu'il n'y a pas de cellules vides ou de caractères invalides dans les données.

## Conclusion

Une fois ces étapes terminées, vous devriez être en mesure de créer et gérer des unités organisationnelles dans votre Active Directory de manière automatisée. Ce script est flexible et peut être facilement adapté pour répondre à vos besoins spécifiques en matière de gestion des OUs.

Si vous avez besoin d'aide supplémentaire ou souhaitez personnaliser davantage le script, n'hésitez pas à consulter la documentation ou à me contacter pour plus d'assistance.

---

## REDONDANCE
* ### 1) Windows 2022 + rôles
  * ### 1.1) Redondance AD-DS, DHCP, DNS
* ### 2) windows Core + rôles
* ### 3) ADDS 
  * ### 3.1) OU et Groupes
  * ### 3.2) Utilisateurs
* ### 4) Debian
---
---
---
### 1) Windows 2022 + rôles
---
---
### :one: Installation des roles 

### Le détails des instalations des différents role n'est pas détaillé, dans ce document INSTALL.md , mais ici ⬇️

* #### [DHCP](https://github.com/NALSED/R-vision/blob/main/Fichier%20de%20r%C3%A9vision.md#4422-windows-22) 
* #### [DNS](https://github.com/NALSED/R-vision/blob/main/Fichier%20de%20r%C3%A9vision.md#414-windows-1)
* #### [ADDS](https://github.com/NALSED/R-vision/blob/main/Fichier%20de%20r%C3%A9vision.md#368-cr%C3%A9er-un-adds-) 

   ### 1.1) Redondance AD-DS, DHCP, DNS
---
#### :pencil2:Prérequis 2 serveurs win 2022, un maitre et un secondaire :pencil2:
#### :pencil2:Premièrement créer les rôles sur le maitre, puis créer les rôles sur le second :pencil2:
# 2️⃣Redondance DHCP :
### Vérifier les serveurs autorisés => clic droit DHCP (bleu) => Manage authorized servers...(rouge)
![ad1](https://github.com/user-attachments/assets/2eb6d9e9-f246-4b1b-b7bb-fb9c22b64c2a)
### Ici le serveur à répliquer nest pas autorisé
### clic Authorize (bleu) => rentrer l'IP du serveur à autoriser(rouge)
![ad1](https://github.com/user-attachments/assets/ad23a564-f483-41e9-97de-b191793cb530)
### Le serveur est retouvé et demande de confirmation 
#### ⚠️(le faire pour les deux serveurs SUR les deux serveurs)⚠️
![ad1](https://github.com/user-attachments/assets/1c30ba9c-9673-4678-937b-9a1616712d3e)
### Résultat 
![ad1](https://github.com/user-attachments/assets/db10e800-51c4-4c4d-ba9d-530591e21bf0)
### Pour démarer le redondance :
### Clic droit sur Scope => Configure Failover...
![ad1](https://github.com/user-attachments/assets/f1cc67e0-36c4-44e0-9398-312127750935)
### Rentrer l'IP du serveur de secours
![ad1](https://github.com/user-attachments/assets/70adfc4e-3976-4783-8b9c-376575ac598a)
### Configuration ⬇️
### Lien entre serveurs (bleu)
### Choisir Hot standby(rouge), 
##### (l'autre option permet de partager la charge dans l'attribution des adresses IP)
![ad1](https://github.com/user-attachments/assets/ce0f9c0e-e9b7-45a9-8b89-7bb6f4291b1d)
### Pour la suite diminuer l'intervale de 60 min par defaut à 5 min
##### (c'est le temps qu mettre le serveur deux à prendre le relais)
### Et cocher le case Enable Message Authentification
#### (Cela permet de chiffrer le echange au niveau de la trame)
![ad1](https://github.com/user-attachments/assets/6afddf6b-a7f4-408f-974a-54706d8efd2a)
### Puis finish
### Vérifier d'être en Successful partout
![ad1](https://github.com/user-attachments/assets/c8f106f4-e136-4b07-b66b-7b0ecc3a7dab)


# Redondance DNS :
### :pencil2:Prérequis un serveur DNS :pencil2:

### clic droit sur le serveur DNS => DNS Manage
![ad1](https://github.com/user-attachments/assets/6d21eac5-8bcd-4820-9afc-30939426051e)
### Clic droit sur Forward Lookup Zones(bleu) => New Zone...(rouge)
![ad1](https://github.com/user-attachments/assets/8c68f4d2-2204-4a2b-84a9-8483b32beda1)
### Next jusqu'au choix des zones
### Renseigner le nom de la zone, sur laquelle on veux appliquer la redondance
![ad1](https://github.com/user-attachments/assets/0e6013d5-cc8c-425c-b2d1-ce135727372e)
### Renseigner l'adresse IP (de l'autre serveur), Résultat ⬇️
![ad1](https://github.com/user-attachments/assets/6d423ca6-8242-4ba4-82ca-6d2d11b9481b)
### Retourner sur le serveur principale (master)
### Créer un nouvel enregistrement de type A ou AAAA.
![ad1](https://github.com/user-attachments/assets/594eff4f-9494-4643-9e44-5a49ecac86e3)
### Remplir le nom du nouvel enregistrement(peux importe le nom) (bleu)
##### ⚠️Une Reverse Lookup Zone doit être présente sur le serveur maitre, dans le cas contraire,la créer!⚠️
### Ip du serveur secondaire (rouge)
### Cocher la case Create associated (PTR) record (vert)
![ad1](https://github.com/user-attachments/assets/882bb546-5b39-44d6-92ec-b23ad9dcfe5c)
###  Puis Add Host
### Sur le serveur maitre Action => Properties
![ad1](https://github.com/user-attachments/assets/6b701638-b0ad-4151-853f-7453dbc8052f)
### Name server (bleu)
### Add (rouge)
### renseigner le nom complet du serveur secondaire (vert) => Resolve
![ad1](https://github.com/user-attachments/assets/f13627d7-fb02-4c43-976d-deed7af11771)
### (En cas de doute sur le nom du serveur il se trouve ici) ⬇️⬇️⬇️⬇️
![ad1](https://github.com/user-attachments/assets/f10776d0-f11e-42b1-a214-24792b2f0a19)
### Le message d'erreur est normal le second serveur ne fait pas autorité.
![ad1](https://github.com/user-attachments/assets/867bc3c1-cd9e-41bf-80c7-bbd392120b08)
![ad1](https://github.com/user-attachments/assets/607f1543-995c-4bcd-87a3-a6c1b17559f3)
### Pour finir Zone Transfers (bleu) 
### Cocher Only to servers listed on the Name Server tab (rouge)
![ad1](https://github.com/user-attachments/assets/72f50913-50b5-4b9a-9423-72e43b30b55e)
# Redondance ADDS
#### ⚠️Les deux serveurs doivent être sur le même réseau, et le DNS du serveur maitre renseigné dans le serveur secondaire. 
### sur un second serveur créer un rôle ADDS
### Puis Promote ths server to a domain controller
### Cocher la case Add a domain contoller to an existing domain 
### Puis renseigner le nom du domaine maitre(bleu)
### Cliquer change sur puis renseigner le nom(comme présent sur l'écran de connection) et MDT du serveur maitre.(rouge)
![ad1](https://github.com/user-attachments/assets/883eb080-4c21-4cdf-a780-1b6627d273ba)
### Laisser par defaut (ou voir avec reda notament RODC), puis rentrer MDT 
![ad1](https://github.com/user-attachments/assets/90313b3b-ceda-450a-a00e-2cce26d6fd9e)
### Next
### Spécifier le domaine
![ad1](https://github.com/user-attachments/assets/baefd9d0-0382-4fd8-baf1-756ae837341e)
### Next jusqu'a Install.








### 2) windows Core + rôles
### 3) ADDS 
  ### 3.1) OU et Groupes
  ### 3.2) Utilisateurs
### 4) Debian
















