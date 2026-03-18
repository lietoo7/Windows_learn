# Fiche Technique 5 : Paramètres et Panneau de configuration

Cette section explique la coexistence des deux interfaces de configuration de Windows et comment accéder aux réglages système avancés.

## 1. La dualité des interfaces

Depuis Windows 8 et 10, Windows propose deux environnements pour gérer le système :

* **L'application Paramètres (Settings) :** * Interface moderne, tactile et simplifiée.
* Conçue pour les réglages quotidiens (Wi-Fi, personnalisation, mises à jour via Windows Update).


* **Le Panneau de configuration (Control Panel) :** * Interface classique héritée des anciennes versions de Windows.
* Contient des outils plus granulaires et techniques qui n'ont pas encore été migrés vers l'application moderne.

---

## 2. System Configuration (msconfig)

L'utilitaire `msconfig` est un outil de dépannage essentiel pour modifier la manière dont Windows démarre.

### 1. Onglet "Général"

L'onglet **Général** est le point d'entrée de l'utilitaire. Il permet de définir le profil global de démarrage de l'ordinateur. C'est ici que l'on choisit si Windows doit démarrer avec tous ses composants ou dans un état restreint pour le dépannage.

### 1.1. Sélection du mode de démarrage

L'utilisateur peut choisir entre trois options mutuellement exclusives :

* **Démarrage normal :**
  * **Description :** Charge tous les pilotes de périphériques et tous les services Windows.
  * **Usage :** État par défaut pour une utilisation quotidienne de l'ordinateur.

* **Démarrage en mode diagnostic :**
  * **Description :** Charge uniquement les pilotes de périphériques et les services de base (essentiels au fonctionnement minimal).
  * **Usage :** Très utile pour déterminer si un problème (plantage, lenteur) est causé par un service tiers ou un pilote non essentiel.

* **Démarrage sélectif :**
  * **Description :** Permet une personnalisation précise de ce qui doit être chargé.
  * **Sous-options :**
    * *Charger les services système :* Active ou désactive globalement les services listés dans l'onglet "Services".
    * *Charger les éléments de démarrage :* Concerne les applications qui se lancent automatiquement à l'ouverture de session.
    * *Utiliser la configuration de démarrage d'origine :* Verrouille les paramètres sur les valeurs par défaut de Windows.

### 1.2. Flux de Diagnostic Recommandé

L'onglet Général est souvent utilisé selon cette procédure logique :

1. **Constat :** L'ordinateur présente une erreur au démarrage.
2. **Action :** Basculer en **Démarrage en mode diagnostic**.
3. **Analyse :** Si le problème disparaît, cela signifie qu'une application tierce ou un pilote secondaire est le coupable.
4. **Résolution :** Utiliser le **Démarrage sélectif** pour réactiver les éléments un par un jusqu'à trouver celui qui recrée l'erreur.

### 1.3. Résumé des commandes d'interface

| Bouton | Action |
| --- | --- |
| **OK** | Enregistre les modifications et propose de redémarrer pour appliquer le nouveau mode. |
| **Annuler** | Ferme l'utilitaire sans appliquer de changements. |
| **Appliquer** | Valide le choix sans fermer la fenêtre. |


### 2. Onglet "Démarrer" (Boot)

L'onglet **Démarrer** est particulièrement critique car il interagit directement avec les données de configuration de démarrage (BCD).

### 2.1. Liste des Systèmes d'Exploitation

Le cadre supérieur affiche les systèmes installés.

* **Identifiant :** `Windows 11 (C:\WINDOWS)`
* **Statut :** Système d'exploitation actuel et par défaut.
* **Utilité :** Si vous aviez plusieurs versions de Windows (Dual-boot), elles apparaîtraient ici pour définir celle qui démarre en premier.

### 2.2. Options de démarrage (Boot Options)

Ces cases permettent de forcer des modes de diagnostic lors du prochain redémarrage :

* **Démarrage sécurisé (Safe Boot) :** Redémarre Windows en mode sans échec.
  * *Minimal :* Charge uniquement les pilotes critiques (pas de réseau).
  * *Autre environnement (Alternate Shell) :* Ouvre l'invite de commande sans interface graphique.
  * *Réparer Active Directory :* Pour les serveurs/contrôleurs de domaine.
  * *Réseau :* Mode sans échec avec prise en charge d'Internet.

* **Ne pas démarrer la GUI :** Désactive l'animation de chargement de Windows (le logo tournant) au profit d'un écran noir.
* **Journaliser le démarrage :** Crée un fichier log (`ntbtlog.txt`) listant tous les pilotes chargés (ou non) pour identifier un plantage.
* **Vidéo de base :** Charge Windows avec des pilotes graphiques VGA standards au lieu de vos pilotes Nvidia/AMD (utile en cas de conflit d'écran).
* **Infos de démarrage du système d'exploitation :** Affiche le nom de chaque pilote à mesure qu'il se charge sur l'écran de démarrage.

### 2.3. Paramètres de Délai et Avancés

* **Délai (Timeout) :** Par défaut à **30 secondes**. C'est le temps pendant lequel le menu de démarrage s'affiche avant de lancer automatiquement le système par défaut.
* **Rendre permanents tous les paramètres de démarrage :** Si cette case est cochée, les modifications ne seront pas réinitialisées après le redémarrage (à utiliser avec prudence).
* **Options avancées... :** Permet de limiter le nombre de processeurs ou la quantité de mémoire RAM utilisée (très utile pour les développeurs testant des environnements limités).

### 2.4. Boutons d'Action

| Bouton | Action |
| --- | --- |
| **OK** | Applique les changements et ferme la fenêtre (propose généralement un redémarrage). |
| **Annuler** | Ferme sans enregistrer. |
| **Appliquer** | Enregistre les modifications sans fermer la fenêtre. |

### 3. Onglet "Services"

L'onglet **Services** dresse la liste exhaustive des programmes d'arrière-plan qui s'exécutent au démarrage de Windows, qu'ils appartiennent au système d'exploitation ou à des éditeurs tiers (comme Apple ou des fabricants de pilotes).

### 3.1. Interface de la liste des services

La liste est organisée en quatre colonnes principales pour faciliter l'audit du système :

* **Service :** Le nom complet du service (ex: Audio Windows, Heure cellulaire).
* **Fabricant :** L'entité responsable du service (ex: Microsoft Corporation, Apple Inc.).
* **État :** Indique si le service est actuellement en cours d'exécution ("Exécution...") ou s'il est à l'arrêt ("Arrêté").
* **Date de désactivation :** Indique la date à laquelle un service a été arrêté manuellement via cet outil (si applicable).

### 3.2. Fonctionnalités de filtrage et d'action

* **Masquer tous les services Microsoft :** 
  * **Usage :** C'est la fonction la plus critique pour le dépannage.
  * **Intérêt :** En cochant cette case, vous ne voyez plus que les services installés par vos logiciels (antivirus, navigateurs, utilitaires de jeux). Cela permet de désactiver les services tiers sans risquer de rendre Windows instable.

* **Désactiver tout :** Permet d'arrêter d'un coup tous les services affichés dans la liste actuelle.
* **Activer tout :** Réactive l'ensemble des services pour revenir à un démarrage complet.

### 3.3. Cas d'usage technique (Dépannage "Clean Boot")

Pour identifier un conflit logiciel, les techniciens procèdent souvent ainsi :

1. Cocher **Masquer tous les services Microsoft**.
2. Cliquer sur **Désactiver tout** pour les services restants.
3. Redémarrer l'ordinateur.
4. Si le problème disparaît, il suffit de réactiver les services tiers un par un pour identifier le coupable.

### 4. Onglet "Démarrage" (Startup)

L'onglet **Démarrage** sert à gérer les applications et logiciels qui s'exécutent automatiquement dès qu'un utilisateur ouvre sa session Windows.

### 4.1. Centralisation vers le Gestionnaire des tâches

Contrairement aux anciennes versions de Windows où la liste des programmes était affichée directement dans cette fenêtre, les versions modernes (Windows 10 et 11) ont déporté cette gestion pour plus de clarté :

* **Redirection :** L'onglet contient désormais un lien hypertexte intitulé "**Ouvrir le Gestionnaire des tâches**".
* **Instructions :** Un message explicatif indique explicitement d'utiliser la section "Démarrage" du Gestionnaire des tâches pour gérer les éléments.

### 4.2. Pourquoi ce changement ?

Le déplacement de cette fonctionnalité vers le Gestionnaire des tâches permet d'accéder à des informations techniques supplémentaires qui n'étaient pas disponibles dans l'ancien MSConfig, notamment :

* **L'impact au démarrage :** Le Gestionnaire des tâches indique si une application ralentit beaucoup, moyennement ou peu le chargement du bureau.
* **Le statut en temps réel :**
Permet d'activer ou de désactiver une application d'un simple clic droit.
* **L'utilisation des ressources :** Visualiser la consommation CPU et mémoire de ces programmes dès l'ouverture de session.

### 4.3. Impact sur la configuration système

Même si la gestion se fait ailleurs, toute modification effectuée dans le Gestionnaire des tâches aura une répercussion sur l'onglet **Général** de MSConfig :

* Si vous désactivez un programme de démarrage, MSConfig considérera que vous êtes en mode "**Démarrage sélectif**".

### 5. Onglet "Outils"

L'onglet **Outils** sert de tableau de bord centralisé regroupant les utilitaires de diagnostic, de configuration et de réparation les plus puissants de Windows. Plutôt que de chercher ces outils dans différents menus ou de retenir des commandes complexes, l'utilisateur peut les lancer directement d'ici.

### 5.1. Liste des outils disponibles

L'interface présente une liste structurée avec le **Nom de l'outil** et une **Description** de sa fonction :

* **Options Internet :** Permet d'afficher et de modifier les propriétés et paramètres liés à la navigation web.
* **Configuration du protocole Internet :** Utilitaire pour configurer ou dépanner les paramètres d'adresse réseau (IP).
* **Analyseur de performances :** Outil avancé pour analyser en détail la santé des ordinateurs locaux ou distants.
* **Moniteur de ressource :** Permet de surveiller en temps réel l'utilisation du processeur, de la mémoire, du disque et du réseau.
* **Gestionnaire des tâches :** Affiche les processus en cours et les performances globales du système.
* **Invite de commandes :** Ouvre une console (`cmd.exe`) pour exécuter des lignes de commandes textuelles.
* **Éditeur du Registre :** Permet de modifier la base de données de configuration critique de Windows (`regedit`).
* **Assistance à distance :** Fonction pour recevoir ou proposer de l'aide technique via Internet.
* **Restaurer le système :** Permet de ramener l'ordinateur à un état antérieur (point de restauration) en cas de problème majeur.

### 5.2. Fonctionnement de l'interface

* **Commande sélectionnée :** Lorsqu'un outil est cliqué dans la liste, le chemin d'accès technique ou la commande spécifique s'affiche dans ce champ (ex: `C:\Windows\System32\rstrui.exe` pour la restauration).
* **Options avancées :** Cette case permet d'ajouter des paramètres supplémentaires à la commande avant son exécution.
* **Bouton Exécuter :** Lance immédiatement l'utilitaire sélectionné.

### 5.3. Utilité pour l'administrateur

Cet onglet est particulièrement utile en **Mode sans échec**. Si l'interface habituelle de Windows est instable, l'onglet Outils de MSConfig reste souvent accessible, permettant de lancer une **Restauration du système** ou l'**Éditeur du Registre** pour réparer le système.

---

## 3. Configuration système avancée

Pour accéder aux réglages de bas niveau, on utilise souvent les "Propriétés système" (accessibles via `sysdm.cpl`).
-------------------------------------------
### 3.1. Nom de l'Ordinateur

#### Modifier le Nom de la Machine

1. **Accéder aux Propriétés Système :**
   - Appuyez sur `Win + R`, tapez `sysdm.cpl`, puis appuyez sur `Entrée`.

2. **Onglet "Nom de l'ordinateur" :**
   - Cliquez sur l'onglet **"Nom de l'ordinateur."**
  
3. **Changer le Nom :**
   - Cliquez sur le bouton **"Modifier..."**.
   - Dans le champ **"Nom de l’ordinateur,"** entrez le nouveau nom souhaité.
   
4. **Joindre un Domaine ou un Groupe de Travail :**
   - Sous **"Membre de,"** sélectionnez **"Domaine"** ou **"Groupe de travail"**.
   - Entrez le nom du domaine ou du groupe de travail, puis cliquez sur **OK**.
   
5. **Redémarrage :**
   - Un redémarrage du système sera nécessaire pour appliquer les changements.

### 3.2. Paramètres Système Avancés

#### 3.2.1. Performances

##### Configurer les Effets Visuels

1. **Accéder aux Propriétés Système :**
   - Appuyez sur `Win + R`, tapez `sysdm.cpl`, puis appuyez sur `Entrée`.

2. **Onglet "Avancé" :**
   - Allez dans l'onglet **"Avancé."**
   - Cliquez sur le bouton **"Paramètres..."** sous la section **"Performances."**

3. **Ajuster les Effets Visuels :**
   - Dans l'onglet **"Effets visuels,"** choisissez **"Personnalisé."**
   - Désélectionnez les effets visuels que vous ne souhaitez pas utiliser.
   - Cliquez sur **OK** pour appliquer les changements.

##### Gestion de la Mémoire Virtuelle

1. **Dans le même menu des "Paramètres de performance" :**
   - Cliquez sur l'onglet **"Avancé."**
   - Dans la section **"Mémoire virtuelle,"** cliquez sur le bouton **"Modifier..."**.

2. **Configurer le Fichier d'Échange :**
   - Décochez **"Gestion automatique du fichier d’échange pour tous les lecteurs."**
   - Choisissez le lecteur, puis définissez la taille du fichier d'échange (Taille initiale et Taille maximale).
   - Cliquez sur **"Définir,"** puis sur **OK** pour enregistrer et fermer.

#### 3.2.2. Profils Utilisateur

##### Gestion des Profils

1. **Accéder à la Gestion des Profils :**
   - Lancez **"Propriétés système"** via `sysdm.cpl` comme précédemment.
  
2. **Accéder aux Profils Utilisateurs :**
   - Sous l'onglet **"Avancé,"** recherchez la section **"Profils utilisateurs."**
   - Cliquez sur le bouton **"Paramètres..."** pour voir tous les profils.

3. **Supprimer un Profil :**
   - Sélectionnez le profil que vous souhaitez supprimer.
   - Cliquez sur **"Supprimer."**
   - Confirmez la suppression lorsque cela est demandé.

#### 3.2.3. Démarrage et Récupération

##### Configurer Comportement en cas d'Erreur

1. **Accéder aux Propriétés Système :**
   - Ouvrez **"Propriétés système"** via `sysdm.cpl`.

2. **Onglet "Avancé" :**
   - Dans cet onglet, trouvez la section **"Démarrage et récupération."**
   - Cliquez sur le bouton **"Paramètres..."**.

3. **Configurer les Options :**
   - Sous **"Échec du système,"** cochez ou décochez les options souhaitées, par exemple **"Redémarrer automatiquement."**
   - Définissez le type de fichier de vidage mémoire que vous souhaitez créer dans le menu déroulant.

4. **Appliquer les Changements :**
   - Cliquez sur **OK** pour enregistrer les modifications.

#### 3.2.4. Variables d'Environnement

##### Accéder et Modifier les Variables Système

1. **Accéder aux Propriétés Système :**
   - Ouvrez **"Propriétés système"** via `sysdm.cpl`.

2. **Onglet "Avancé" :
Voici un résumé détaillé du contenu de votre fichier concernant la commande **sysdm.cpl**, organisé en format Markdown :

---

# Guide de la commande sysdm.cpl : Accès aux propriétés système de Windows

Comment utiliser la commande intégrée **sysdm.cpl** pour accéder rapidement aux paramètres avancés de Windows ?  

## Qu'est-ce que la commande sysdm.cpl ?

* **Définition** : C'est un utilitaire Windows intégré qui ouvre directement la boîte de dialogue **Propriétés système**.
* **Signification** : L'extension ".cpl" signifie *Control Panel* (Panneau de configuration), indiquant qu'il s'agit d'un raccourci vers un applet spécifique du panneau de configuration.
* **Compatibilité** : Elle est disponible sur presque toutes les versions de Windows, incluant Windows 11, 10, 8, 7, Vista et XP .

## Comment exécuter la commande

Il existe trois méthodes principales pour lancer cet outil :

1. **Boîte de dialogue Exécuter** : Appuyez sur `Windows + R`, tapez `sysdm.cpl` et appuyez sur Entrée.
2. **Invite de commandes (CMD)** : Ouvrez le terminal, tapez `sysdm.cpl` et validez.
3. **Windows PowerShell** : Ouvrez PowerShell, saisissez `sysdm.cpl` et appuyez sur Entrée.

## Exploration des onglets des Propriétés Système

Une fois la fenêtre ouverte, vous trouverez cinq onglets principaux :

### 1. Nom de l'ordinateur

* Affiche le nom actuel de la machine et le groupe de travail (Workgroup).
* **Fonctionnalités** : Permet de renommer l'ordinateur ou de rejoindre un domaine/groupe de travail pour la mise en réseau.

### 2. Matériel

* Fournit un accès direct au **Gestionnaire de périphériques** pour gérer les composants matériels.
* Permet de configurer les options de signature des pilotes.

### 3. Paramètres système avancés

C'est la section la plus puissante pour optimiser le système:

* **Performances** : Ajustement des effets visuels et de la mémoire virtuelle (fichier d'échange).
* **Profils utilisateurs** : Gestion des comptes et des paramètres associés.
* **Démarrage et récupération** : Choix du système d'exploitation par défaut et configuration des actions en cas de défaillance (plantage).

### 4. Protection du système

* Gère les **points de restauration** du système.
* Permet de restaurer Windows à un état antérieur en cas de dysfonctionnement.

### 5. Utilisation à distance

* Permet d'activer ou de désactiver les connexions au **Bureau à distance**.
* **Conseil** : Utilisez des mots de passe robustes pour sécuriser cet accès.

### Astuces et Dépannage

* **Créer un raccourci bureau** : Faites un clic droit sur votre bureau > Nouveau > Raccourci. Tapez `sysdm.cpl` comme emplacement et nommez-le "Propriétés Système" .
* **Erreur "Fichier introuvable"** : Si la commande ne fonctionne pas, il est possible que le fichier système soit manquant ou corrompu.

---

### les fonctions avancées de **BCDEdit**.

### 1. Gestion du Délai d'Attente (Timeout)

Le timeout définit le temps (en secondes) pendant lequel le menu de démarrage s'affiche avant de lancer l'entrée par défaut.

* **Modifier le délai :**
```bash
bcdedit /timeout 15

```
*(Ici réglé sur 15 secondes)*.

### 2. Création d'une Entrée de Secours (Dual Boot de sécurité)

Il est possible de copier l'entrée actuelle pour créer un clone. Si vous cassez la configuration de l'entrée principale, la seconde restera fonctionnelle.

#### Étape A : Copier l'entrée actuelle

```bash
bcdedit /copy {current} /d "Démarrage de Secours"

```

* **Note :** Cette commande va générer un identifiant long (GUID) entre accolades, type `{896e...}`. **Notez-le bien.**

#### Étape B : Désactiver le mode sans échec sur cette copie (si nécessaire)

Si vous voulez être sûr que cette copie démarre normalement :

```bash
bcdedit /deletevalue {GUID_DE_LA_COPIE} safeboot

```

### 3. Résumé des Identifiants (Cheat Sheet)

| Identifiant | Rôle |
| --- | --- |
| **{bootmgr}** | Le programme qui affiche le menu au démarrage. |
| **{current}** | L'OS sur lequel vous avez ouvert l'invite de commande. |
| **{default}** | L'OS qui démarrera automatiquement après le timeout. |
| **{memdiag}** | L'outil de diagnostic de la mémoire Windows. |

### 4. Cas pratique : Supprimer une entrée inutile

Si vous avez trop d'entrées dans votre menu de démarrage (après un clone ou une mauvaise manipulation) :

```bash
bcdedit /delete {GUID_A_SUPPRIMER}

```

> [!WARNING]
> N'utilisez jamais `/delete` sur `{current}` ou `{default}` sans avoir une autre entrée valide, sinon le PC ne démarrera plus.

### 5. Schéma du flux de démarrage

Le BCD intervient très tôt. Voici l'ordre logique traité par le système :

1. **BIOS/UEFI** initialise le matériel.
2. **Boot Manager ({bootmgr})** lit le fichier BCD.
3. **Le Menu** s'affiche (pendant le temps défini par `/timeout`).
4. **Winload.exe** charge le noyau Windows correspondant à l'entrée choisie.

### les fonctions avancées de Taskkill

La commande `taskkill` permet de mettre fin à une ou plusieurs tâches ou processus. Elle est particulièrement utile lorsque l'interface graphique (le Gestionnaire des tâches) ne répond plus ou pour créer des scripts de maintenance.

### Syntaxe de base

La structure générale de la commande est la suivante :
`taskkill [/S système] [/U utilisateur [/P mot_de_passe]] { [/FI filtre] [/PID id_processus | /IM nom_image] } [/F] [/T]`

### Options et Arguments principaux

| Option | Description |
| --- | --- |
| **/IM** (Image Name) | Spécifie le nom du fichier exécutable du processus à arrêter (ex: `notepad.exe`). |
| **/PID** (Process ID) | Spécifie l'identifiant numérique du processus (visible dans le Gestionnaire des tâches). |
| **/F** (Force) | Force la fermeture du processus. À utiliser si le programme ne répond pas. |
| **/T** (Tree) | Arrête le processus spécifié **et tous les processus enfants** qu'il a générés. |
| **/S** (System) | Spécifie le nom ou l'adresse IP d'un ordinateur distant. |

### Cas d'utilisation courants

### 1. Forcer la fermeture d'un programme

C'est la commande que vous avez mentionnée. Elle est idéale pour un logiciel "planté".

* **Commande :** `taskkill /F /IM nom_du_processus.exe`
* **Exemple :** `taskkill /F /IM chrome.exe` (Ferme toutes les instances de Google Chrome).

### 2. Fermer un processus et ses dépendances

Certains logiciels ouvrent plusieurs sous-processus. Pour tout nettoyer d'un coup :

* **Commande :** `taskkill /F /T /IM excel.exe`

### 3. Utilisation de filtres avancés (`/FI`)

Vous pouvez cibler des processus selon des critères précis, comme ceux qui ne répondent plus.

* **Commande :** `taskkill /FI "STATUS eq NOT RESPONDING"`

### 4. Gestion de l'onglet "Démarrage" et Automatisation

Bien que `taskkill` serve à arrêter des processus **actifs**, il est souvent couplé à d'autres outils pour gérer le démarrage :

1. **Identification :** Utilisez `tasklist` pour voir ce qui tourne actuellement.
2. **Nettoyage au démarrage :** Si un programme se lance automatiquement et que vous voulez l'arrêter via un script (.bat) au démarrage de la session :
* Créez un fichier `clean_startup.bat`.
* Ajoutez vos lignes `taskkill /F /IM malin.exe`.

3. **Alternative (MSConfig/Task Manager) :** Pour empêcher un programme de se lancer, préférez l'onglet "Démarrage" du Gestionnaire des tâches ou la commande `msconfig`.

> [!CAUTION]
> **Attention :** Forcer la fermeture d'un processus avec `/F` ne permet pas au logiciel de sauvegarder les données en cours. Utilisez-le avec prudence sur des outils de travail comme Word ou des bases de données.

### les fonctions avancées de gestion des Services via `NET`

La commande `NET` est un utilitaire de ligne de commande hérité mais puissant, utilisé pour gérer les ressources réseau et les services système sous Windows. Bien que PowerShell propose désormais `Start-Service` et `Stop-Service`, la syntaxe `NET` reste la plus rapide et la plus compatible.

### 1. Syntaxe de base

| Action | Commande | Description |
| --- | --- | --- |
| **Démarrer** | `net start [NomDuService]` | Lance un service arrêté ou en pause. |
| **Arrêter** | `net stop [NomDuService]` | Arrête un service en cours d'exécution. |
| **Lister** | `net start` | Affiche la liste de tous les services **actifs**. |

### 2. Identification du "Nom du Service"

Il est crucial de distinguer le **Nom de l'affichage** (ex: *Spouleur d'impression*) du **Nom du service** réel (ex: *Spooler*). C'est ce dernier qu'il faut utiliser en ligne de commande.

**Comment trouver le nom exact :**

1. Ouvrez l'onglet **Services** (via `services.msc`).
2. Faites un clic droit sur un service > **Propriétés**.
3. Le "Nom du service" est celui affiché tout en haut de l'onglet Général.

### 3. Exemples d'utilisation courants

#### Gérer le service d'impression (Spooler)

Souvent utilisé pour débloquer une file d'attente d'impression :
* `net stop spooler`
* `net start spooler`

#### Gérer le service Windows Update

* `net stop wuauserv`
* `net start wuauserv`

### 4. Règles et Astuces

* **Guillemets :** Si le nom du service contient des espaces, vous devez impérativement utiliser des guillemets.
* *Exemple :* `net start "AdobeUpdateService"`


* **Privilèges :** Ces commandes nécessitent une **Invite de commande exécutée en tant qu'administrateur**. Sans cela, vous recevrez une "Erreur système 5 : Accès refusé".
* **Dépendances :** Si vous arrêtez un service dont d'autres services dépendent, Windows vous demandera confirmation avant de tous les arrêter.
* **Statut de retour :**
* Si le service est déjà lancé, `net start` renverra un message d'information.
* Si le service ne peut pas démarrer (erreur de configuration), un code d'erreur spécifique s'affichera.

### 5. Alternative moderne (SC)

Pour des actions plus complexes (comme changer le type de démarrage ou interroger l'état précis), on utilise souvent la commande `sc` (Service Control) :

* `sc query [NomDuService]` : Pour voir le statut détaillé.
* `sc config [NomDuService] start= disabled` : Pour désactiver un service.

