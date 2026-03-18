## Réseautage sous Windows

Ce cours présente les concepts essentiels pour configurer, connecter et faire communiquer des systèmes Windows dans divers environnements.

---

## I. Configuration de base du système

La préparation de l'identité de la machine est essentielle avant toute manipulation réseau.

### 1. Identification de l'ordinateur

- **Action** : Allez dans **Propriétés du système** > **Modifier les paramètres**.
- **Rôle** : Attribuez un nom unique à votre machine (ex : **PC-FORMATION-01**) pour l'identifier sur le réseau (Groupe de travail ou Domaine).

### 2. Protocole TCP/IP (IPv4)

La communication dépend de l'adresse IP, qui se compose d'une **partie Réseau** (fixée par le masque) et d'une **partie Hôte** (spécifique à chaque machine).

- **Mode Dynamique (DHCP)** : Le routeur attribue automatiquement une adresse IP.
- **Mode Statique** : Vous configurez manuellement l'adresse IP, utile pour des serveurs ou des tests spécifiques.

---

## II. Paramétrage des Adresses IP

### 1. Configurer une IP fixe

1. **Chemin** : Panneau de configuration > Centre Réseau et partage > Modifier les paramètres de la carte.
2. **Propriétés** : Clic droit sur la carte réseau > Propriétés > **Protocole Internet version 4 (TCP/IPv4)**.
3. **Configuration** :
   - **IP** : ex : `192.168.1.10`
   - **Masque** : ex : `255.255.255.0`
   - **Passerelle** : Adresse IP de votre routeur (ex : `192.168.1.1`).
   - **DNS** : ex : `8.8.8.8` (Google) pour la navigation Internet.

### 2. Configuration Alternative

Utile pour définir une IP de secours quand aucun serveur DHCP n'est détecté. 

- Dans les propriétés IPv4, accédez à l'onglet **Configuration alternative**.
- Cochez **Utilisateurs configurés** et entrez vos paramètres statiques.

---

## III. Virtualisation avec VirtualBox

Pour établir une communication entre une machine virtuelle (VM) et votre ordinateur physique (hôte).

### 1. Modes de connexion

- **Accès par pont (Bridged)** : La VM est connectée au même réseau que l'hôte.
- **Réseau privé hôte (Host-Only)** : Crée un réseau local fermé entre l'hôte et la VM.

### 2. Mise en place (Host-Only)

1. Dans VirtualBox, allez à **Fichier** > **Gestionnaire de réseau hôte** (vérifiez l'IP de l'adaptateur, souvent `192.168.56.1`).
2. Dans les paramètres de la VM : Dans l'onglet **Réseau**, sélectionnez "Réseau privé hôte".

---

## IV. Partage de fichiers (Protocole SMB)

Permet l'échange de dossiers entre deux machines sur le même réseau.

### 1. Préparation du système

- **Profil Réseau** : Réglez-le sur **Privé** (Paramètres > Réseau et Internet).
- **Partage avancé** : Dans le Centre Réseau et partage, activez la "Découverte de réseau" et le "Partage de fichiers".

### 2. Partager un dossier

1. Clic droit sur le dossier > **Propriétés** > Onglet **Partage**.
2. Cliquez sur **Partage avancé** > **Partager ce dossier**.
3. Configurez les **Autorisations** (ex : "Tout le monde" avec lecture/écriture).

Puis sur les autorisations de Sécurité (NTFS), toujours dans les Propriétés du dossier, allez sur l'onglet Sécurité.

4. Cliquez sur Modifier puis sur Ajouter.
5. Tapez Tout le monde (ou Everyone si votre Windows est en anglais) et validez.
6. Sélectionnez "Tout le monde" dans la liste et cochez les cases Lecture et exécution, Affichage du contenu du dossier et Lecture.
7. Cliquez sur Appliquer.

### 3. Accéder au partage

Depuis une autre machine, utilisez le raccourci `Windows + R` et tapez :
`\\ADRESSE_IP_DU_PC_DISTANT` (ex : `\\192.168.56.101`).

---

## V. Outils de Diagnostic (Invite de commande)

Utilisez ces commandes pour valider votre installation réseau :

| Commande              | Utilité                                        |
|----------------------|-----------------------------------------------|
| `ipconfig /all`      | Affiche la configuration réseau complète.    |
| `ping [IP]`          | Teste la réponse d'une autre machine.        |
| `tracert [IP]`       | Trace la route vers une destination.         |
| `netstat`            | Affiche les connexions actives.              |
| `net share`          | Affiche la liste des dossiers partagés.      |

> **Note importante** : Si le réseau semble bien configuré mais que le `ping` échoue,vérifiez toujours le **Pare-feu Windows**. Pour vos tests, il est souvent nécessaire d'autoriser le "Partage de fichiers et d'imprimantes" dans les règles du pare-feu.

---

## VI. Sécurité Réseau

Une bonne configuration de sécurité est cruciale pour protéger les données de votre réseau.

### 1. Configuration du Pare-feu Windows

- **Action** : Accédez à **Contrôle de compte d'utilisateur** > **Panneau de configuration** > **Pare-feu Windows**.
- **Règles entrantes** : Configurez les règles nécessaires pour le "Partage de fichiers et d'imprimantes", en assurant que les connexions réseau soient autorisées.

### 2. Gestion des utilisateurs et des autorisations

- **Création d'utilisateurs** : Assurez-vous que seuls les utilisateurs autorisés aient accès au réseau.
- **Configuration des permissions** : Définissez des autorisations spécifiques sur les dossiers partagés pour garantir la sécurité des données.

---

## VII. Administration à Distance

### 1. Utilisation de Remote Desktop

- **Configuration** : Activez le Bureau à distance dans les **Propriétés système**.
- **Connexion** : Utilisez le client de Bureau à distance pour vous connecter à d'autres machines à distance en utilisant les informations d'identification appropriées.

### 2. Outils de gestion à distance

- **PowerShell** : Utilisez des commandes à distance pour la gestion et le diagnostic des systèmes dans votre réseau.
- **Gestion de l'ordinateur** : Accédez à des fonctions de gestion avancées via la console de gestion de l’ordinateur et configurez des partages à distance.

---

## VIII. Résolution des Problèmes

### 1. Diagnostic des problèmes de connexion

- **Vérification de l'état de la carte réseau** : Utilisez `ipconfig` pour vérifier que l'adresse IP a été assignée correctement et que la connexion est active.
- **Test de ping** : Utilisez `ping` pour diagnostiquer la connectivité avec d'autres appareils sur le réseau.

### 2. Outils supplémentaires

- **Event Viewer** : Consultez les logs d'événements pour identifier les erreurs réseau ou les problèmes de configuration.
- **Network Troubleshooter** : Utilisez l'outil de dépannage réseau intégré à Windows pour identifier automatiquement et corriger certains problèmes.

 
