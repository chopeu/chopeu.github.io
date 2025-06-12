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

* Modernisation des interfaces (intégration TCP/IP, …)

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

---

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

### Exercices pratiques

* [ ] **Exercice 1 – Se connecter et lancer `ISPF`** :

**Objectif** : Comprendre comment accéder à l’environnement ISPF via TSO.

   * Connecte-toi à TSO avec ton identifiant.
   * Lance la commande `ISPF`.
   * Explore les menus de 0 à 6 (lecture uniquement).
   * Utilise la touche `F3` pour revenir en arrière.

* [ ] **Bonus** : Affiche ton `PROFILE` avec la commande `TSO PROFILE`.

---

* [ ] **Exercice 2 – Rechercher et afficher des datasets**

**Objectif** : Utiliser l’outil 3.4 pour naviguer dans les fichiers.

   * Va dans **ISPF option 3.4**.
   * Saisis ton préfixe (ex. : `USERID.*`) et liste les datasets existants.
   * Ouvre un dataset en mode **View**.

* [ ] **Bonus** : Trier par date ou par nom.

---

* [ ] **Exercice 3 – Créer un dataset partitionné (PDS)**

**Objectif** : Créer un PDS pour stocker du code source.

   * Menu ISPF → `3.2` (Allocate Dataset)
   * Nom : `USERID.SOURCE.COBOL`
   * Type : PDS
    LRECL=80, RECFM=FB, BLKSIZE=800, SPACE=(5,5)

* [ ] **Bonus** : Crée aussi `USERID.JCL.LIB` pour y placer tes JCL.

---

* [ ] **Exercice 4 – Éditer un programme COBOL simple**

**Objectif** : Éditer un membre source COBOL dans ton PDS.

   * Menu `2` → Éditer `USERID.SOURCE.COBOL(HELLOCOB)`

       `IDENTIFICATION DIVISION.
       PROGRAM-ID. HELLOCOB.
       PROCEDURE DIVISION.
           DISPLAY 'BONJOUR MVS !'.
           STOP RUN.`

* [ ] **Bonus** : Ajoute un `ACCEPT` pour lire un nom.

---

* [ ] **Exercice 5 – Copier des fichiers entre datasets**

**Objectif** : Utiliser ISPF 3.3 pour copier des membres.

   * Menu `3.3`
   * Source : `USERID.SOURCE.COBOL`
   * Destination : `USERID.SOURCE.COPIE`
   * Copie le membre `HELLOCOB`

* [ ] **Bonus** : Copier un membre JCL vers un autre PDS.

---

* [ ] **Exercice 6 – Supprimer et renommer un dataset**

**Objectif** : Manipuler des datasets dans ISPF 3.4.

   * Supprime un dataset test avec `D` dans la liste 3.4
   * Renomme un dataset avec `R` (ex. : `OLDNAME` → `NEWNAME`)

* [ ] **Bonus** : Vérifier que le dataset renommé conserve ses membres.

---

* [ ] **Exercice 7 – ISPF – Commandes de ligne**

* [ ] Exercice 7.1 – Insérer des lignes

**Objectif** : Apprendre à utiliser les commandes I et In

   * Ouvre un membre dans un PDS avec ISPF → Option 2
   * À n’importe quelle ligne, tape I → Appuie sur Entrée
   * Tape du texte sur la ligne insérée (ex: INSERTION 1)
   * Ensuite, tape I3 → Ajoute 3 lignes à la suite

      *  Bonus : insère 5 lignes en haut du fichier
      
* [ ] Exercice 7.2 – Supprimer des lignes

**Objectif** : Apprendre à utiliser D, Dn, DD…DD

   * Supprime une seule ligne avec D
   * Supprime 3 lignes d’un coup avec D3
   * Place deux commandes DD en début et fin d’un bloc → supprime tout

      *  Bonus : Expérimente avec D1, D2, D5 pour bien voir l’effet
      
* [ ] Exercice 7.3 – Répéter une ligne

**Objectif** : Utiliser R, Rn, RR…RR

   * Tape R sur une ligne → observe la duplication
   * Tape R2 → observe la répétition de cette ligne 2 fois
   * Place RR sur 2 lignes + RR plus bas → répète un bloc

      *  Bonus : Ajoute un commentaire à chaque ligne répétée pour les repérer
      
* [ ] Exercice 7.4 – Copier une ligne

**Objectif** : Apprendre C, Cn, CC…CC + A/B/O

   * Tape C à gauche d’une ligne → A sur une autre ligne → Entrée
   * Utilise Cn pour copier plusieurs fois la même ligne
   * Teste la copie d’un bloc avec CC…CC + A

      *  Bonus : Essaye l’option O (overwrite)
      
* [ ] Exercice 7.5 – Déplacer des lignes

**Objectif** : Utiliser M, Mn, MM…MM

   * Déplace une ligne avec M + B ou A
   * Déplace un bloc avec MM + MM puis A pour recoller
   * Utilise Mn pour déplacer plusieurs fois une ligne

      *  Bonus : Essaye de déplacer vers le haut (avant) et vers le bas (après)
      
* [ ] Exercice 7.6 – Afficher les colonnes

**Objectif** : Utiliser COLS + RESET

   * Tape COLS sur la ligne de commande
   * Observe l’échelle de colonnes (1-72)
   * Tape RESET pour la masquer

      *  Bonus : Tape à nouveau COLS, modifie une colonne précise (ex: col 10-20)
      
* [ ] Exercice 7.7 – Décaler à droite

**Objectif** : ) )n ))…)) ))n…))

   * Tape ) devant une ligne → observe le décalage (2 colonnes)
   * Tape )5 → observe le décalage de 5 colonnes
   * Utilise )) + )) pour décaler un bloc
   * Teste aussi ))3 + ))3 pour bloc + 3 colonnes

      *  Bonus : Combine ))n et CC pour voir le déplacement dans SDSF
      
* [ ] Exercice 7.8 – Décaler à gauche

**Objectif** : ( (n (( ((n

   * Tape ( sur une ligne → décale de 2 colonnes vers la gauche
   * Tape (6 → décale de 6 colonnes
   * Utilise ((…(( pour un bloc
   * Teste ((3 pour un bloc de 3 colonnes

      *  Bonus : Revenir en arrière avec ) (récupérer la mise en page)
      
* [ ] Exercice 7.9 – Combiner insertion + déplacement

**Objectif** : I, M, A

   * Insère une ligne en haut de fichier avec I
   * Écris "ENTÊTE"
   * Prends une ligne de texte et place M + A pour la mettre après l’entête

      *  Bonus : Déplace 3 lignes sous cette entête avec MM…MM
      
* [ ] Exercice 7.10 – Construire un tableau formaté

**Objectif** : Travailler avec les colonnes et l’alignement

   * Tape COLS
   * Crée un tableau structuré :
    | Nom       | Prénom    | Âge  |
    |-----------|-----------|------|
    | MARTIN    | Jean      | 34   |
    | DURAND    | Claire    | 28   |

   * Utilise les commandes de décalage horizontal pour aligner correctement les colonnes

      *  Bonus : Répète une ligne du tableau pour ajouter un nouvel enregistrement
      
* [ ] Exercice 7.11 – Nettoyage structuré

**Objectif** : Enchaîner D, C, R, M sur une même zone

   * Supprime un bloc avec DD…DD
   * Copie une ligne + colle avec C/A
   * Répète une ligne avec R3
   * Déplace ce mini-bloc avec MM/A

      *  Bonus : Fais une macro de test : créer → modifier → déplacer → supprimer
      
* [ ] Exercice 7.12 – Deviner la commande manquante

**Objectif** : Consolider les acquis

   * Je veux copier 3 lignes consécutives et les insérer juste après la ligne 10.
   * Quelle combinaison de commandes dois-je utiliser ?


---