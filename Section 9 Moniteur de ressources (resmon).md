# Fiche Technique 9 : Moniteur de ressources (resmon)

Le **Moniteur de ressources** (`resmon.exe`) est un outil beaucoup plus précis que le Gestionnaire des tâches. Il permet de surveiller l'utilisation des ressources en temps réel avec une granularité chirurgicale, ce qui est idéal pour identifier quel processus précis ralentit un système.

## 1. Onglet Vue d'ensemble

Cet écran affiche quatre graphiques en temps réel (CPU, Disque, Réseau, Mémoire). C'est le tableau de bord principal pour repérer une anomalie globale.

## 2. Analyse du Processeur (CPU)

Au-delà de l'utilisation globale, cet onglet permet de voir :

* **Services :** Quels services système consomment du CPU au sein d'un processus.
* **Handles associés :** On peut rechercher quel programme utilise un fichier spécifique (pratique pour débloquer un fichier qu'on ne peut pas supprimer car "utilisé par un autre programme").
* **Modules associés :** Liste les fichiers DLL utilisés par un processus donné.

## 3. Analyse de la Mémoire (RAM)

C'est ici que l'on comprend réellement comment Windows utilise la mémoire vive grâce à une barre de couleur explicative :

* **Matériel réservé :** Mémoire utilisée par le BIOS ou les cartes graphiques.
* **Utilisée :** Mémoire activement occupée par les programmes et Windows.
* **Modifiée :** Données qui doivent être écrites sur le disque avant de pouvoir être libérées.
* **En veille :** Contient des données en cache pour accélérer le système, mais peut être libérée instantanément si besoin.
* **Libre :** Mémoire totalement vide.

## 4. Analyse du Disque

Cet onglet est crucial pour diagnostiquer les lenteurs système (PC qui "rame").

* **Processus avec activité disque :** Identifie quel logiciel lit ou écrit sur le disque.
* **Activité du disque :** Affiche précisément **quel fichier** est en train d'être lu ou écrit sur le disque physique.
* **Temps de réponse (ms) :** Si cette valeur dépasse 100 ms pour un fichier, cela indique souvent un disque dur en fin de vie ou saturé.

## 5. Analyse du Réseau

Permet de surveiller le trafic de données en direct :

* **Processus avec activité réseau :** Quels logiciels communiquent avec l'extérieur.
* **Connexions TCP :** Affiche l'adresse IP de destination et le port utilisé par chaque logiciel (très utile pour détecter un malware qui communique avec un serveur distant).
* **Ports d'écoute :** Liste tous les ports ouverts sur la machine qui attendent une connexion entrante.
 
