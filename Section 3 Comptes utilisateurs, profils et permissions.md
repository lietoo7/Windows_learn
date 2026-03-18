# Fiche Technique 3 : Comptes utilisateurs, profils et permissions

Cette section traite de la gestion de l'identité des utilisateurs sur une machine locale et de la manière dont leurs données et privilèges sont structurés.

## 1. Types de comptes : Administrateur vs Utilisateur standard

Windows distingue principalement deux types de comptes pour garantir la sécurité du système :

**Utilisateur Standard :** 
Peut utiliser la plupart des logiciels et modifier les paramètres qui n'affectent que son propre profil.
Ne peut pas installer de logiciels impactant le système global ni modifier les fichiers système ou les paramètres de sécurité.


**Administrateur :** 
Possède un contrôle total sur l'ordinateur.
Peut installer des applications, modifier le registre, gérer les autres comptes et accéder à tous les fichiers de la machine.


**Système (SYSTEM) :** 
Un compte invisible encore plus puissant que l'administrateur, utilisé par Windows lui-même pour gérer les processus de bas niveau.

## 2. Appartenance aux Groupes dans Windows

Windows emploie des groupes pour gérer les permissions des utilisateurs de manière granulaire, au-delà des simples rôles d'**Administrateur** et d'**Utilisateur Standard**. Cette approche permet une hiérarchisation des droits et des accès en fonction des besoins spécifiques des utilisateurs.

<hr>

### Héritage des Droits

Un utilisateur hérite des droits associés à tous les groupes auxquels il appartient. Cela signifie que les permissions peuvent s'accumuler, augmentant ainsi les capacités de l'utilisateur. Par conséquent, la gestion des groupes est essentielle pour maintenir la sécurité et l'intégrité du système.

### Outil de Gestion des Groupes : netplwiz

L'outil **netplwiz** permet d'administrer les appartenances des groupes. Dans l'onglet **Appartenance au groupe > Autre**, les administrateurs peuvent assigner des rôles spécifiques aux utilisateurs. Voici les rôles disponibles :

| **Groupe**                          | **Description**                                                                                 |
|-------------------------------------|-------------------------------------------------------------------------------------------------|
| **Utilisateurs (Users)**            | Groupe par défaut pour les utilisateurs standards, sans risque de compromettre le système.      |
| **Administrateurs**                 | Accès complet au système avec tous les privilèges et autorisations.                             |
| **Invités (Guests)**                | Autorisations très limitées, souvent utilisées pour des profils temporaires.                   |
| **Utilisateurs du Bureau à distance** | Autorise la connexion à la machine depuis un autre ordinateur, idéal pour le télétravail.       |
| **Opérateurs de sauvegarde**        | Permet la copie de fichiers pour sauvegarde, même si des permissions de lecture sont manquantes.|
| **Utilisateurs avec pouvoir (Power Users)** | Groupe d'ordre intermédiaire, moins utilisé aujourd'hui, mais offrant des droits plus élevés qu'un utilisateur standard. |

<hr>

### Détails des Groupes

#### Utilisateurs (Users)
Ce groupe est le niveau d'accès par défaut pour tous les utilisateurs standards. Ils disposent de permissions suffisantes pour effectuer les tâches quotidiennes, sans risquer de compromettre la sécurité ou l'intégrité du système. Ils ne peuvent pas installer des applications ou faire des modifications système majeures.

#### Administrateurs
Les membres de ce groupe bénéficient d'un **accès illimité** à toutes les fonctionnalités du système. Cela inclut la possibilité d'installer des logiciels, de modifier des paramètres système, et d'accéder à toutes les données.

#### Invités (Guests)
Ce groupe est conçu pour des utilisateurs temporaires ayant des permissions minimales. Ils peuvent, par exemple, accéder à Internet ou aux fichiers publics, mais n'ont pas de droits sur les paramètres du système.

#### Utilisateurs du Bureau à distance
Les membres de ce groupe peuvent se connecter à un ordinateur à distance. Ceci est particulièrement utile pour le télétravail. Les utilisateurs doivent souvent être configurés pour activer cette fonctionnalité sur l'ordinateur cible.

#### Opérateurs de sauvegarde
Ce groupe est essentiel pour les pratiques de sauvegarde. Il permet aux utilisateurs de copier des fichiers bien qu'ils n'aient pas la permission d'y accéder normalement, ce qui est crucial pour les opérations de récupération de données.

#### Utilisateurs avec pouvoir (Power Users)
Historique et moins courant aujourd'hui, ce groupe offrait un compromis entre les droits d'un utilisateur standard et ceux d'un administrateur. Les "Power Users" pouvaient modifier certains paramètres système sans avoir l'accès complet des administrateurs.

---
## 3. Création, Modification et Suppression

La gestion des comptes peut s'effectuer via l'application **Paramètres** ou la console **netplwiz** (plus avancée).

**Création :**
Lors de la création, Windows demande s'il s'agit d'un compte local ou d'un compte Microsoft (lié au cloud).

**Modification :** 
Permet de changer le mot de passe, de modifier le type de compte (passer de Standard à Administrateur) ou de changer l'image de profil.

**Suppression :** 
Lors de la suppression, Windows propose de conserver ou de supprimer les fichiers personnels de l'utilisateur (documents, bureau, etc.).

---
## 4. Profils Utilisateurs (C:\Users)

Lorsqu'un utilisateur se connecte pour la première fois, Windows crée un **Profil**.

* **Emplacement :** `C:\Users\[NomUtilisateur]`.
* **Structure du profil :** Contient les dossiers personnels (Documents, Images, Téléchargements) et le dossier caché `AppData` qui stocke les paramètres spécifiques aux applications.
* **NTUSER.DAT :** Un fichier caché crucial dans chaque profil. Il contient les paramètres de registre spécifiques à l'utilisateur (HKEY_CURRENT_USER). Si ce fichier est supprimé ou corrompu, le profil ne pourra pas se charger correctement.

---
## 5. Permissions sur les profils

Par défaut, Windows applique une isolation stricte :

* Un utilisateur standard ne peut pas ouvrir le dossier d'un autre utilisateur.
* Seul un Administrateur peut forcer l'accès au profil d'un tiers après avoir accepté un avertissement de sécurité.
 
