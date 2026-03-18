# Fiche Technique 2 : Dossiers Windows et System32

Cette section explore l'arborescence critique du système d'exploitation et les variables utilisées pour y naviguer efficacement.

## 1. Le dossier C:\Windows

Il s'agit du répertoire racine du système d'exploitation, souvent appelé "Home Directory" de Windows.

* **Rôle :** Il contient les fichiers nécessaires au fonctionnement de Windows, les polices de caractères (Fonts), les journaux d'installation et les fichiers de configuration globaux.
* **Sécurité :** L'accès à ce dossier nécessite généralement des privilèges d'administrateur, car toute modification peut rendre le système instable.

## 2. Le dossier System32

Situé dans `C:\Windows\System32`, c'est le répertoire le plus crucial de l'architecture Windows.

* **Contenu :** Il regroupe les fichiers exécutables système (.exe) et les bibliothèques de liens dynamiques (.dll).
* **Fichiers clés :**
* **kernel32.dll :** Gère la mémoire et les processus.
* **user32.dll :** Gère l'interface utilisateur.
* **cmd.exe :** L'invite de commandes.
* **drivers :** Contient les pilotes de périphériques (dans `System32\drivers`).


* **Confusion 64-bit :** Sur une version 64 bits de Windows, le dossier `System32` contient paradoxalement les fichiers 64 bits, tandis que les fichiers 32 bits sont stockés dans `C:\Windows\SysWOW64`.

## 3. Les variables d'environnement (%windir%, %SystemRoot%)

Les variables d'environnement sont des raccourcis dynamiques qui permettent d'accéder à des dossiers même si Windows est installé sur un lecteur différent de C:.

* **%windir% :** Cette variable renvoie au chemin où Windows est installé. Dans 99% des cas, il s'agit de `C:\Windows`.
* **%SystemRoot% :** Pratiquement identique à `%windir%`, elle est utilisée par le système pour localiser les fichiers essentiels au démarrage.
* **Avantages :** * Facilite l'automatisation par scripts (un script fonctionnera sur tous les PC quel que soit le disque d'installation).
* Permet de naviguer rapidement : taper `%windir%` dans la barre d'adresse de l'explorateur de fichiers vous mènera directement au dossier Windows.


 
