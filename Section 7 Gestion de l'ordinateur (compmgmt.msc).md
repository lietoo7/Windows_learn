# Fiche Technique 7 : Gestion de l'ordinateur

La console **Gestion de l'ordinateur** est une interface MMC (Microsoft Management Console) qui regroupe plusieurs outils d'administration en une seule fenêtre. Elle est accessible via un clic droit sur le bouton Démarrer ou en tapant `compmgmt.msc`.

## 1. Outils système

Cette partie permet de surveiller l'état de santé et les événements du système.

* **Observateur d'événements :** Le journal de bord de Windows. Il enregistre tout : erreurs matérielles, tentatives de connexion échouées, mises à jour réussies. C'est le premier endroit où regarder en cas de panne (BSOD).
* **Dossiers partagés :** Permet de voir quels dossiers de votre machine sont accessibles sur le réseau, qui y est connecté actuellement et quels fichiers sont ouverts à distance.
* **Utilisateurs et groupes locaux :** Un outil puissant pour gérer les comptes (création, réinitialisation de mot de passe) et surtout pour gérer les **Groupes** (ex: ajouter un utilisateur au groupe "Administrateurs" ou "Utilisateurs du Bureau à distance").

## 2. Stockage (Gestion des disques)

C'est l'un des composants les plus utilisés de cette console.

* **Initialisation :** Préparer un nouveau disque dur pour Windows.
* **Partitionnement :** Réduire un volume pour en créer un nouveau, ou étendre une partition existante.
* **Formatage :** Choisir le système de fichiers (NTFS, exFAT) et effacer les données.
* **Lettres de lecteur :** Modifier la lettre attribuée à un disque (ex: changer D: en E:).

## 3. Services et applications

* **Services :** Permet de gérer les programmes qui tournent en arrière-plan sans interface utilisateur. On peut configurer leur mode de démarrage :
* **Automatique :** Se lance au démarrage de Windows.
* **Manuel :** Se lance uniquement si une application en a besoin.
* **Désactivé :** Le service ne peut pas être lancé, ce qui peut renforcer la sécurité ou économiser des ressources.


* **Contrôle WMI :** Outil avancé pour configurer les propriétés de l'infrastructure de gestion Windows.

## 4. Gestionnaire de périphériques

Bien qu'accessible séparément, il est intégré ici pour :

* Mettre à jour les pilotes (drivers).
* Identifier les composants non reconnus (marqués d'un point d'exclamation jaune).
* Désactiver un composant matériel défectueux.
 
-----------------------------------------
## Pratique : Gestion de l’Ordinateur (compmgmt.msc)

### Vue d’Ensemble

1. Ouvrir la console avec un raccourci clavier.
2. Gérer services, événements, et dossiers partagés rapidement.
3. Effectuer des opérations sur les disques en toute sécurité.

### Objectifs

- Accéder à compmgmt.msc et s’orienter rapidement.
- Gérer les services, consulter les journaux d’événements, et voir les fichiers ouverts.
- Effectuer des opérations sur les disques en sécurité (lettres et partitions).

#### Étape 1 : Ouvrir et Naviguer au Clavier

1. **Action Principale**
   - Raccourci : Win + R → taper `compmgmt.msc`
   - La console s'ouvre. Utilisez **Flèches, Tab, Entrée** pour naviguer.
   - *Repères clavier* :
     - **F6 ou Tab** : bascule entre l’arborescence (gauche) et le volet de contenu (droite).
     - **Alt + Entrée** : afficher les propriétés de l’élément.
     - **Alt + A** puis la lettre soulignée : accéder aux menus.

2. **Utilité**
   - Gain de temps et concentration sans souris.

3. **Explication**
   - L’arborescence comprend “Outils système”, “Stockage”, “Services et applications”.

#### Étape 2 : Services, Événements, Dossiers Partagés

1. **Services**
   - Chemin : Outils système → Services et applications → Services
   - Sélectionner un service avec les flèches, puis **Alt + Entrée** pour afficher ses propriétés. Définir le type de démarrage (Automatique/Manuel).
   - Raccourci : `services.msc` pour un accès direct.

2. **Observateur d’événements**
   - Chemin : Outils système → Observateur d’événements → Journaux Windows
   - Utilisez **Ctrl + F** pour rechercher une erreur précise.
   - Raccourci : `eventvwr.msc` pour un accès direct.

3. **Dossiers Partagés**
   - Chemin : Outils système → Dossiers partagés
   - Sélectionner une session et utiliser **Suppr** pour fermer si nécessaire.
   - Raccourci : `fsmgmt.msc` pour un accès direct.

#### Étape 3 : Gestion des Disques

1. **Gestion des disques**
   - Chemin : Stockage → Gestion des disques, attendre l'inventaire.
   - Sélectionner un volume, utiliser **Alt + Entrée** pour ses propriétés.
   - Raccourci : `diskmgmt.msc` pour un accès direct.

2. **Utilité**
   - Permet de changer les lettres de lecteur, d'étendre/réduire des partitions, et de marquer un volume comme actif si nécessaire.

3. **Explication**
   - Effectuer une sauvegarde avant toute modification.
     
### Astuce

Pour lancer **compmgmt.msc** avec des privilèges administratifs au clavier, utilisez la combinaison suivante :  
**Touche Win** + tapez **"compmgmt"**, puis **Ctrl + Maj + Entrée**. Cela est utile si votre session n'est pas déjà configurée en tant qu'administrateur.

---

### À Ne Pas Faire

- Ne désactivez pas des services Windows critiques sans comprendre leurs dépendances.
- Ne réduisez ni n’étendez un volume système sans avoir pris de sauvegarde et sans point de restauration.
- Évitez d’effacer les journaux d’événements pendant qu'un diagnostic est en cours.

 
 
 
