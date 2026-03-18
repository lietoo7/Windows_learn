# PowerShell :

## 1) Navigation et Manipulation
## Sommaire
1. [Vérification de la version]( )
2. [Les lecteurs Windows PowerShell]( )
3. [Les cmdlets de navigation]( )
* [Get-Location]( )
* [Set-Location]( )
* [Get-ChildItem]( )

4. [Les cmdlets de manipulation]( )
* [Get-Item]( )
* [Get-Content]( )
* [Set-Content]( )
* [New-Item]( )
* [Remove-Item]( )
* [Copy-Item]( )
* [Rename-Item]( )
* [Move-Item]( )
* [Invoke-Item]( )

5. [Exercices pratiques]( )
---
## 1. Connaître la version de PowerShell installée 

Pour cela, une fois l’interface de Windows PowerShell ouverte, vous pouvez exécuter la ligne de commande suivante, à savoir $PSVersionTable qui retourne alors le contenu de la variable : 

```powershell
PS C:\Windows\system32> $PSVersionTable

```
Lancer Windows PowerShell en mode compatibilité avec une version antérieure à la version actuellement installée. 

```powershell
PS C:\Users\Admin> $PSVersionTable.PSCompatibleVersions

```
Pour basculer Windows PowerShell sur une version antérieure, il faut appeler l’exécutable powershell.exe, spécifier le paramètre -version et mentionner le numéro de version souhaité : 

```powershell
PS C:\Windows\system32> powershell.exe -version 2.0

```
Voici la suite du document avec la section 2, reprenant mot pour mot le contenu de vos notes.

---

## 2. Les lecteurs Windows PowerShell

Connaître les lecteurs Windows PowerShell accessibles.

Les accès aux lecteurs Windows PowerShell peuvent varier d’un poste à l’autre. 
Il existe une cmdlet permettant de lister ces accès : **Get-PSDrive**. 
Voici une démonstration sur un poste de travail : 

```powershell
PS C:\Windows\system32> Get-PSDrive

```
* **Alias** : il s’agit ici des alias mis en place pour appeler des cmdlets PowerShell. Ainsi, la commande `del` appelle en réalité la cmdlet `Remove-Item`. Il est bien sûr possible de rajouter de nouveaux alias.
  
* **FileSystem** : il s’agit des lecteurs contenant un système de fichiers. Ces lecteurs peuvent être locaux ou réseau.
  
* **Certificate** : correspond au magasin de certificats du poste de travail. Ce magasin de certificats est accessible en mode graphique depuis la console Microsoft Management Console (MMC), puis en ajoutant le composant logiciel enfichable nommé Certificats.
  
* **Environment** : contient la liste des variables d’environnement. Ces variables dynamiques sont utilisées par les différents processus du système d’exploitation. Ainsi, la variable d’environnement `$env:COMPUTERNAME` désigne le nom de l’ordinateur proprement enregistré dans Windows.
  
* **Function** : contient l’ensemble des fonctions présentes dans Windows PowerShell. Ces fonctions contiennent du code PowerShell qui est exécuté lorsque vous les appelez. Par exemple, lorsque vous rentrez la commande `D:` dans Windows PowerShell, ce dernier exécute en réalité : `Set-Location D:`. Il est bien sûr possible d’enregistrer de nouvelles fonctions.
  
* **Registry** : désigne la base de registre Windows. Cette base de données contient l’ensemble de la configuration du système d’exploitation et des logiciels installés. La base de registre est accessible avec une interface graphique en lançant l’exécutable `regedit.exe`.
* **Variable** : contient l’ensemble des variables durant la durée de vie du processus Windows PowerShell. Certaines variables sont préconstruites, comme `$PSVersionTable` et `$Host` qui ont été vues au chapitre Présentation de Windows PowerShell, et d’autres peuvent être créées.
  
* **WSMan** : contient les paramètres WS-Management. Ces paramètres sont principalement utilisés lors de l’utilisation de la fonctionnalité PowerShell Remoting.

## 3. cmdlets de navigation 
### a. 
Get-Location 
**Get-Location** permet simplement d’obtenir le répertoire actif au moment de l’exécution de la commande : 
```powershell
PS C:\Windows\system32> Get-Location

```
---
### b. 
Set-Location 
Tout administrateur système connaît la commande **cd** (Change Directory) de l’invite de commandes. Dans Windows PowerShell, si **cd** fonctionne encore, c’est en réalité grâce à un alias qui pointe vers la cmdlet **Set-Location**. 
Ainsi, pour aller dans un dossier spécifique, écrivez : 
```powershell
PS C:\Windows\system32> Set-Location D:\
PS D:\> Set-Location 'C:\Program Files'
PS C:\Program Files>
```
---
### c. 
Get-ChildItem 
La cmdlet **Get-ChildItem** permet de lister le contenu d’un dossier, et vous indique également les attributs de chacun de ses éléments (fichiers et dossiers). Il s’agit de l’équivalent de la commande **dir** (Directory) dans l’invite de commandes. Voici une démonstration : 
```powershell
PS C:\Program Files> Get-ChildItem

```
Descriptif des informations retournées par la commande **Get-ChildItem** : 
| Colonne | Description |
| --- | --- |
| **Mode :** | <br>**d, a, r, h, s** Les attributs des objets : **D**irectory (répertoire), **A**rchive, **R**ead-Only (lecture seule), **H**idden (objet caché), **S**ystem (objet système). |
| **LastWriteTime** | La date de dernière modification. |
| **Length** | La taille du fichier (en octets). |
| **Name** | Le nom de l’objet. |

Ensuite les paramètres les plus importants de **Get-ChildItem** : 

| Paramètre | Description |
| --- | --- |
| **-Attributes <String[]>** | Permet de sélectionner uniquement les objets avec un ou plusieurs attributs (voir tableau précédent). |
| **-Recurse** | Demande récursive (affiche le contenu des sous-dossiers). |
| **-Filter <String>** | Permet de filtrer les objets. |
| **-Path <String[]>** | Spécifie un ou plusieurs chemins d’accès. Les caractères génériques sont autorisés. L’emplacement par défaut est le répertoire actif. |
| **-Hidden** | Affiche uniquement les fichiers et dossiers cachés. |

**Exemple :** 

```powershell
PS C:\Users\Admin> Get-ChildItem -Path . -Hidden -Filter *.dat

```
---
4. cmdlets de Manipulation 

### a. 
Get-Item 

**Get-Item** permet de récupérer l’objet désigné dans la ligne de commande, grâce au paramètre **-Path**. Cette cmdlet est surtout très utile combinée à un tube, ou pipe en anglais (barre verticale, correspondant au caractère `|`), qui permet de passer le flux d’information de l’objet récupéré vers une autre cmdlet.
Voici un aperçu des paramètres disponibles : 

| Paramètre | Description |
| --- | --- |
| **-Exclude <String[]>** | Permet d’exclure des objets notifiés par le paramètre -Path. L’utilisation du caractère wildcard (*) est autorisée. |
| **-Force** | Autorise la cmdlet à récupérer l’objet qui est normalement non accessible, tels que les fichiers cachés. |
| **-Include <String[]>** | Récupère seulement les objets spécifiés. Le caractère wildcard (*) est autorisé.  |
| **-Path <String[]>** | Spécifie le chemin d’accès du ou des fichiers et/ou des dossiers. |

**Exemples :**
* L’exemple suivant permet de récupérer l’ensemble des fichiers avec l’extension *.txt présents dans un dossier spécifiquement ciblé : `PS C:\Windows\system32> Get-Item -Path C:\Temp\*.txt` 

* Utilisation avec une copie : 
`PS C:\Windows\system32> Get-Item -Path C:\Temp\*.txt | [cite_start]Copy-Item -Destination C:\Temp2` 

* Récupérer les propriétés associées à l’objet (exemple avec une image) : 
`PS C:\Temp> Get-Item .\IMGP3532.JPG | [cite_start]Format-List *` 
* Calcul de la taille en mégaoctets : `PS C:\Temp> (Get-Item .\IMGP3532.JPG).Length / 1MB` (Résultat : 11,07... soit environ 11 Mo ).
* Récupérer la date de dernière modification : `(Get-Item .\IMGP3532.JPG).LastWriteTime` 



---
### b. 
Get-Content 
**Get-Content** permet de récupérer le contenu d’un objet. Même si ce dernier est un fichier binaire, Get-Content s’adapte et retourne un résultat.

| Paramètre | Description |
| --- | --- |
| **-TotalCount <Int64>** | Retourne le nombre de lignes depuis le début du fichier.  |
| **-Tail <Int32>** | Retourne le nombre de lignes depuis la fin du fichier. |

**Exemple avec filtrage :**

```powershell
PS C:\Windows\system32> Get-Content C:\Windows\System32\drivers\etc\hosts | Where-Object {$_ -match "acme.com"}

```



---
### c. 
Set-Content 
À l’inverse de Get-Content, **Set-Content** permet d’écrire ou de remplacer des données dans un objet.

| Paramètre | Description |
| --- | --- |
| **-Path <String[]>** | Chemin d’accès à l’objet (fichier, registre, etc.). |
| **-Value <Object[]>** | Valeur à définir dans l’objet ciblé (chaîne de caractères ou objet).|

> **Note :** Si le fichier existe déjà, tout son contenu est écrasé. Si le dossier spécifié n’existe pas, une erreur est retournée.

---

### d. 
New-Item 
Permet de créer des fichiers, mais aussi des dossiers.
| Paramètre | Description |
| --- | --- |
| **-Force** | Autorise la cmdlet à écrire sur un objet existant, même si celui-ci est en lecture seule. |
| **-ItemType <String>** | Type d’objet à créer (ex: File, Directory). |
| **-Path <String[]>** | Chemin d’accès de l’objet à créer. |

**Exemples :**
* Création d'un fichier : `New-Item -Path C:\Temp\Test.txt -ItemType File` 
* Création d'un dossier : `New-Item C:\Temp\RepTest -ItemType Directory` 



---
### e. 
Remove-Item.
Supprime des fichiers et/ou dossiers. Attention : cette action est irréversible car elle ne passe pas par la Corbeille.

| Paramètre | Description |
| --- | --- |
| **-Force** | Autorise la suppression des objets cachés ou en lecture seule. |
| **-Path <String[]>** | Chemin d’accès de l’objet à supprimer. |
| **-Recurse** | Supprime les objets et les sous-objets présents. |
| **-WhatIf** | Retourne un aperçu du résultat sans exécuter la commande. |

---

### f. 
Copy-Item 
Permet de faire une copie d’un ou de plusieurs objets.
| Paramètre | Description |
| --- | --- |
| **-Destination <String>** | Chemin de destination. |
| **-Path <String[]>** | Objet à copier. |
| **-Recurse** | Spécifie une copie récursive. |

---

### g. 
Rename-Item 
Permet de renommer un objet.
`PS C:\Temp> Rename-Item -Path .\ScriptPowerShell.ps1 -NewName Migration.ps1` 

---

### h. 

Move-Item 

Permet de déplacer un fichier ou un dossier vers un autre emplacement.
`PS C:\Windows\system32> Move-Item -Path .\Migration.ps1 -Destination C:\Temp` 

---

### i. 

Invoke-Item 

Cette cmdlet permet d’ouvrir un fichier et d’effectuer son action par défaut (lancement de l’application associée). Elle permet notamment d'ouvrir plusieurs fichiers en une seule fois via des caractères génériques.
`PS C:\Windows\system32> Invoke-Item C:\Temp\*.txt` 
 
## 5. Exercices

### Exercice 1 : Prise en main et navigation
**Objectif :** Vérifier votre version de PowerShell, explorer les disques disponibles, et vous déplacer dans le système de fichiers.
**Ouvrez votre interface PowerShell**.
**Affichez la version de PowerShell installée sur votre machine**.
* `$psversiontable` 
**Listez tous les "lecteurs" (drives) accessibles depuis PowerShell sur votre système**.
* `get-psdrive` 

**Que remarquez-vous par rapport aux lecteurs traditionnels (comme C: ou D:)?** 
* PowerShell expose d'autres "lecteurs" virtuels comme Alias, Function, Variable ou Registry, ce qui permet d'interagir avec d'autres parties du système de la même manière qu'avec des fichiers.
 
**Affichez le répertoire de travail actuel**.
* `get-location` 

**Créez un dossier appelé "MonProjet" dans le répertoire C:\Temp (si C:\Temp n'existe pas, créez-le d'abord)**.
* `new-item .\Temp -type directory`
   
**Déplacez-vous dans ce nouveau répertoire "MonProjet"**.
* `set-location .\Temp`
   
**Affichez de nouveau le répertoire de travail pour confirmer que vous vous y trouvez bien**.
* `get-childitem` 
---

### Exercice 2 : Création et manipulation d'objets
**Objectif :** Créer des fichiers et des dossiers, puis les renommer et les supprimer.

**Assurez-vous d'être dans le répertoire C:\Temp\MonProjet**.
`set-location c:\Temp\MonProjet` 
* `get-location` 
 
**Créez trois fichiers texte vides à l'intérieur de ce dossier : "fichier1.txt", "fichier2.txt" et "rapport.log"**.
* `new-item -type file fichier1.txt, fichier2.txt, rapport.log` 

**Listez le contenu du dossier pour vérifier que les fichiers ont bien été créés**.
* `get-childitem` 

**Renommez le fichier "fichier2.txt" en "fichier_renomme.txt"**.
* `rename-item fichier2.txt fichier_renomme.txt` 

**Créez un sous-dossier nommé "AnciensFichiers" dans votre répertoire MonProjet**.
* `new-item AnciensFichiers -type directory` 


**Déplacez le fichier "fichier_renomme.txt" dans le sous-dossier "AnciensFichiers"**.
* `move-item -path fichier_renomme.txt -destination .\AncienFichiers` 

---

### Exercice 3 : Écriture et lecture de contenu
**Objectif :** Écrire du texte dans des fichiers et lire leur contenu.

**Dans le fichier "fichier1.txt", ajoutez le texte suivant : "Ceci est la première ligne de mon fichier."** 
* `set-content -path .fichier1.txt -value “ceci est la premiere ligne du fihcier”` 
  
**Ajoutez une nouvelle ligne au fichier "fichier1.txt" avec le texte : "Ceci est la deuxième ligne."** 
* `add-content -path .fichier1.txt -value “ceci est la deuxieme ligne du fihcier”`

**Lisez et affichez le contenu complet du fichier "fichier1.txt" dans la console**.
* `get-content -path .\ fichier1.txt` 

**Créez un nouveau fichier "notes.txt" et écrivez-y directement le contenu suivant : "Projet terminé le 21/08/2025."** 
* `new-item note.txtx -value “projet termine le 21/08/2025”` 


**Listez les fichiers du répertoire MonProjet qui se terminent par .txt et qui ont été modifiés dans les 24 dernières heures**.
* (Indice : Pour cet exercice, la colonne LastWriteTime de Get-ChildItem sera utile. Vous pouvez aussi utiliser l'opérateur -Filter) .
* `Get-ChildItem -Path . -Filter '*.txt' | [cite_start]Where-Object { $_.LastWriteTime -ge (Get-Date).AddDays(-1) }` 

---

### Exercice 4 : Suppression sécurisée

**Objectif :** Utiliser les options de sécurité avant de supprimer des fichiers ou des dossiers.
* Simulez la suppression du fichier "rapport.log" sans le supprimer réellement. Quelle commande utilisez-vous pour cela ? 
* `remove-item rapport.log -whatif` 

**Supprimez ensuite définitivement le fichier "rapport.log"**.
* `remove-item rapport.log` 

**Simulez la suppression du dossier "AnciensFichiers" et de tout ce qu'il contient**.
* `remove-item AncienFichiers -recurse -whatif` 


**Maintenant, supprimez définitivement le dossier "AnciensFichiers" et son contenu, sans que PowerShell vous demande une confirmation**.

* (Indice : Le paramètre -Recurse est utile pour les dossiers) .
* `remove-item AncienFichiers -recurse -force` 



