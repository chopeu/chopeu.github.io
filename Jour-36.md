# Objectifs journaliers MS COBOL-SQL P4

# Daily – Lundi 16 Juin (Jour-36)


##  MVS / TSO / ISPF

---

## Définitions clés

### Concepts fondamentaux

* [ ] **MVS (Multiple Virtual Storage)** : le système d'exploitation des gros ordinateurs (« mainframes ») d'IBM. MVS a été lancé en 1974 par IBM.
* [ ] **TSO (Time Sharing Option)** : Environnement de travail **interactif** en ligne de commande sur IBM z/OS. 
* [ ] **ISPF (Interactive System Productivity Facility)** : interface utilisateur en mode texte sur les systèmes IBM z/OS.

---

## Objectifs

* [ ] Survoler MVS.
* [ ] Appréhender TSO et ISPF.

---

## MVS (Multiple Virtual Storage)

### 1. La puissance d’un système informatique

La puissance d’un système informatique dépend de nombreux paramètres :
*	Rapidité de calcul du processeur central
*	Nombre de canaux
*	Taille de la mémoire centrale
*	Performance des périphériques
*	Type de système d’exploitation
*	Réglage (optimisation) du système d’exploitation.

En règle générale, la puissance d’une machine se donne en **MIPS (Million d’Instructions Par Seconde)**.



### 2. Qu’est-ce que MVS ?


* MVS = Multiple Virtual Storage

* Système d’exploitation développé par IBM dans les années 1970

* Ancêtre direct de z/OS, encore utilisé dans les environnements COBOL actuels

* Permet l’exécution simultanée de nombreux jobs, avec gestion fine des ressources


### 3. Composants clés de MVS

| Composant      | Rôle                                        |
| -------------- | ------------------------------------------- |
| **JES2/JES3**  | Gestion des jobs batch                      |
| **TSO**        | Interface en ligne de commande              |
| **ISPF**       | Interface utilisateur structurée            |
| **Dataset**    | Fichier de données (source, résultat, etc.) |
| **Catalogues** | Référencement des datasets                  |


### 4. Les fichiers MVS (Datasets)
* Datasets = fichiers structurés

* Deux grandes familles :

    * **Séquentiels (PS)** : lus ligne à ligne

    * **Partitionnés (PDS)** : équivalent des répertoires

* Identifiés par leur **DSN (DataSet Name)**, souvent catalogués


### 5. Types de datasets fréquents 

| Type                | Utilisation                                  | 
| ------------------- | -------------------------------------------- | 
| `SYSOUT`            | Sortie imprimante                            | 
| `SYSIN`             | Données entrées dans le JCL                  | 
| `LOADLIB`           | Bibliothèque de programmes compilés          | 
| `WORK`              | Fichiers temporaires                         | 
| `VSAM`              | Accès direct indexé (bases de données)       | 


### 6. Outils associés à MVS 

* ISPF : Interface d’édition et gestion des fichiers

* SDSF : Suivi des jobs batch

* IDCAMS : Utilitaire pour la gestion des fichiers VSAM

* IEFBR14 : Programme "vide" pour tester un JCL

### 7. Évolution de MVS 

* MVS ➜ OS/390 ➜ z/OS

* Modernisation des interfaces (intégration TCP/IP, sécurité RACF…)

* Compatible avec les anciens programmes COBOL/JCL

### 8. À retenir 

* MVS est la **colonne vertébrale** des environnements COBOL/JCL.

* Il offre **stabilité, performance, multitraitement** et une architecture éprouvée.

* Comprendre MVS, c’est maîtriser le **contexte d’exécution** de vos programmes.

---


## Introduction à `TSO` – Time Sharing Option

### 1. Qu’est-ce que TSO ?

* TSO = **Time Sharing Option**

* Environnement de travail **interactif** en ligne de commande sur IBM z/OS

* Permet à **plusieurs utilisateurs** de se connecter et travailler **simultanément**

* Souvent utilisé avec **ISPF** pour une navigation plus conviviale


### 2. Que permet de faire TSO ?

* Soumettre des **jobs JCL**

* Créer, éditer et gérer des **fichiers (datasets)**

* Accéder à des **programmes utilitaires**

* Exécuter des **commandes système (TSO/E)**


### 3. Connexion à TSO

* Généralement via un **émulateur 3270** (ex : IBM Personal Communications, x3270)

* Saisie de **login** et **mot de passe**

* Accès au menu ISPF via la commande `ISPF`

* Sinon, on reste en mode TSO natif


### 4. Exemples de commandes TSO

| Commande TSO | Fonction                            |
| ------------ | ----------------------------------- |
| `SUBMIT`     | Soumettre un fichier JCL            |
| `LISTCAT`    | Lister un dataset catalogué         |
| `DELETE`     | Supprimer un dataset                |
| `ALLOCATE`   | Créer un nouveau dataset            |
| `RENAME`     | Renommer un fichier                 |
| `SEND`       | Envoyer un message à un utilisateur |

Exemple :

`SUBMIT 'USER01.JCL.MAJSTOCK'`


### 5. Messages système et erreurs

* TSO affiche les messages issus de :

    * JES (envoi/lecture de job)

    * Retour de programmes (codes retour)

    * Problèmes d’allocation, de syntaxe, etc.

**Astuce** : Toujours lire attentivement le message retourné après une commande
[Liste des ABEND](https://www.lacommunauteducobol.com/abend/)


### 6. Astuces pour bien utiliser TSO

* `ISPF` : bascule dans l’interface graphique

* `=X` ou `LOGOFF` : quitter la session

* `HELP` : documentation intégrée

* `PROFILE` : voir ou modifier les paramètres utilisateur

* `TSO %nom_programme` : exécuter un programme


### 7. À retenir

* TSO est l’interface de **base et incontournable** des mainframes IBM

* Maîtriser TSO = savoir interagir efficacement avec le système

* ISPF est souvent préféré pour l’édition, mais **TSO reste indispensable** pour les automatisations et la compréhension système

---

##  Introduction à `ISPF` - Naviguer et travailler efficacement sur un système IBM `z/OS`

### 1. Qu’est-ce que l’ISPF ?

* **ISPF (Interactive System Productivity Facility)** est une interface utilisateur en mode texte sur les systèmes IBM z/OS.

* Elle permet d’interagir avec le système, d’éditer des fichiers, de soumettre des jobs, etc.

* Interface composée de **menus, panels et raccourcis clavier**.

### 2. Ce qu’on peut faire avec ISPF

* Éditer des datasets (fichiers) source (COBOL, JCL…)

* Parcourir les catalogues et organiser ses datasets

* Soumettre et suivre des jobs

* Gérer les compilations

* Naviguer dans les logs d’exécution (SDSF)

### 3. Accès et structure

* On accède à l’ISPF via une **connexion TSO (Time Sharing Option)**.

* ISPF est organisé en **menus numérotés** : on navigue par chiffre ou commande rapide.

* L’écran principal est appelé **ISPF Primary Option Menu**.

### 4. Naviguer dans ISPF

* Saisie directe de **numéros de menu** ou de **commandes rapides** (ex: `2` pour Edit).

* Utilisation de **PF keys (F3 pour sortir, F7/F8 pour naviguer)**.

* Raccourcis utiles :

    * `TSO` pour revenir au mode ligne

    * `=x` pour sortir d’un menu

### 5. Éditeur de fichier (ISPF Edit)

* Mode texte, en lignes numérotées.

* Commandes ligne (en début de ligne) : `I`, `D`, `C`, `R`, `M`, `A`, etc.

* Commandes globales (haut de l’écran) : `CHANGE`, `FIND`, `SAVE`, `SUBMIT`

### 6. Fonctions utiles ISPF

* **ISPF 3.4** : Rechercher et lister les datasets

* **ISPF 6** : Entrée de commandes TSO (ex : `SUBMIT`, `RENAME`, `DELETE`)

* **SDSF** (souvent via `=SD` ou menu 5) : Voir les jobs, messages d’erreur, logs

* **ISPF 3.3** : Copier des fichiers

* **ISPF 3.2** : Allouer un nouveau dataset

### 7. SDSF (Spool Display and Search Facility)

* Permet de **suivre l’exécution des jobs** :

    * `DA` : affichage des jobs

    * `ST` : statut des jobs

    * `S` : sélection d’un job

* Analyse des retours (RC), messages, ABEND éventuels

### 8. Astuces de navigation

* `=3.4` : accès direct à l'explorateur de fichiers

* `=6` puis `SUBMIT` : soumission d’un job en ligne de commande

* `=SD;ST` : SDSF en mode statut des jobs

### 9. À retenir

* ISPF est un **outil indispensable** pour tout développeur mainframe.

* Sa maîtrise permet d’être **autonome dans l’édition, la soumission et le débogage** des jobs.

* C’est l’interface principale pour tous les travaux en COBOL/JCL.

##  Pour aller plus `loin`

[Les commandes de TSO/ISPF](https://www.lacommunauteducobol.com/les-commandes-de-tso-ispf/)

### LES COMMANDES DE LIGNES

| Commande      | Fonction                                  |
| ------------- | ----------------------------------------- |
| `I`           | Insertion d’une ligne                     |
| `In`          | Insertion de n lignes                     |
| `D`           | Suppression d’une ligne                   |
| `Dn`          | Suppression de n lignes                   |
| `DD`          | Suppression du bloc de lignes             |
| `:`           | comprise entre les 2 commandes            |
| `:`           |                                           |
| `DD`          |                                           |
| `R, Rn, RR…RR`| Répétition de lignes                      |
| `C`           | Copie d’une ligne                         |
| `+`           | +                                         |
| `A ou B`      | après (After) un avant (Before) une ligne |
| `Ou O`        | sur (Overide) une ligne existante         |
| `Cn, CC…CC`   | Copie de plusieurs lignes                 |
| `M,Mn, MM…MM` | Déplacement de lignes                     |
| `+`           | +                                         |
| `A ou B`      | après (After) un avant (Before) une ligne |
| `Ou O`        | sur (Overide) une ligne existante         |
| `)`           | Déplacement (décalage) horizontal de      |
|               |  2 colonnes à droite                      |
| `)n`          | Déplacement de n colonnes                 |
| `))…))`       | Déplacement d’un bloc de lignes de 2      | 
|               | colonnes                                  |
| `))n…))`      | Déplacement d’un bloc de lignes de n      | 
|               | colonnes                                  |
| `(, (n,((…((` | Déplacement à gauche                      |
| `COLS`        | Visualise une ligne indiquant le          | 
|               | colonnage (échelle)                       |
|               | Cette ligne s’efface grâce à la commande  | 
|               | RESET                                     |

---
