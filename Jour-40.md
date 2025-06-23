# Objectifs journaliers MS COBOL-SQL P4

# Daily – Vendredi 20 Juin (Jour-40)


## Introduction à la gestion de projets

---

## Objectifs

* [ ] Un peu de détails en JCL.
* [ ] Comprendre les fondamentaux de la gestion de projets.
* [ ] Identifier les étapes d’un projet.
* [ ] Utiliser des outils simples pour planifier, suivre et clôturer un projet.
* [ ] Collaborer efficacement dans un projet (rôles, communication, risques).

---

## Un peu de détails en JCL.

### 1. Le paramètre SPACE dans un JCL

		//DDNAME   DD  DSN=MON.FICHIER,
		//             DISP=(NEW,CATLG,DELETE),
		//             SPACE=(unit,primary,secondary[,RLSE][,CONTIG][,ROUND])

Ce que signifie SPACE :

Ce paramètre indique **combien d'espace disque** réserver pour un nouveau fichier (dataset) et **en quelle unité** : Cylindres, Pistes (Tracks), Blocs...

Unités de mesure possibles :

| Code    | Signification                   |
| ------- | ------------------------------- |
| **TRK** | Track (piste)                   |
| **CYL** | Cylinder (cylindre)             |
| **BLK** | Bloc (taille de bloc en octets) |


#### Schéma explicatif 

Structure disque (vue logique)
+-------------------------------+
| Disque physique               |
|  ├── Cylindre 0               |
|  │     ├── Track 0            |
|  │     ├── Track 1            |
|  │     └── ...                |
|  ├── Cylindre 1               |
|  │     ├── Track 0            |
|  │     └── ...                |
|  └── ...                      |
+-------------------------------+

*  **1 cylindre** = plusieurs **pistes (tracks)** (généralement 15)

*  **1 track** = plusieurs **blocs** (en octets)

#### Exemple pédagogique :

	`SPACE=(CYL,5,2)`

**Interprétation :**

* Alloue **5 cylindres** au départ (allocation primaire)

* Si besoin d'espace supplémentaire, ajoute **2 cylindres à chaque fois** (allocation secondaire)

	Étape 1 : Allocation initiale
	[Cylindre 0] [Cylindre 1] [Cylindre 2] [Cylindre 3] [Cylindre 4]

	Étape 2 (si fichier grossit) :
	+ [Cylindre 5] [Cylindre 6]
	+ [Cylindre 7] [Cylindre 8] (si encore besoin)

On peut l’imager comme un tiroir à dossiers :

* 5 tiroirs sont ouverts d’office

* Puis 2 tiroirs s’ouvrent à la fois si besoin

### 2. Le paramètre `UNIT`

**Syntaxe :**

	//DDNAME   DD  DSN=MON.FICHIER,
	//             DISP=(NEW,CATLG,DELETE),
	//             UNIT=SYSDA

**Ce que signifie `UNIT` :**
Il précise **sur quel type d’unité physique de stockage** le dataset sera alloué.

**Exemples courants :**
| UNIT      | Description                         |
| --------- | ----------------------------------- |
| **SYSDA** | Disques standards (unités DASD)     |
| **TAPE**  | Bandes magnétiques                  |
| **3390**  | Type de disque IBM (plus technique) |


**SYSDA = "System Direct Access"**
C'est **l'unité disque classique**, la plus utilisée pour stocker des datasets.

**Exemple d’explication simple :**
Imagine que tu demandes une place dans un entrepôt :

* `UNIT=SYSDA` → tu demandes une étagère normale dans un hangar.

* `UNIT=TAPE` → tu demandes une boîte à stocker sur une étagère spéciale à bandes.

+--------------------+
|        UNIT        |
+--------------------+
| SYSDA  → 🖴 Disque dur |
| TAPE   → 📼 Bande     |
| 3390   → 🖴 Type disque IBM |
+--------------------+

---

## La gestion de projet  






	
	//STEP001 EXEC PGM=PROGNAME
	//DD1     DD   DSN=FICHIER.ENTREE,DISP=SHR
	//DD2     DD   SYSOUT=A
	

* **JOB** : début du traitement.

* **EXEC** : exécute un programme.

* **DD** : définit les fichiers utilisés.

---

### 2. Bonnes pratiques générales

#### 2.1 Lisibilité & clarté

* Commentaires utiles avec `//*` pour expliquer le job ou les étapes.

* Nommez les steps et datasets de façon explicite (`//ETPLIRE`, `//DDENTREE`, etc.)

* Indentez et alignez les mots-clés pour faciliter la lecture.

#### 2.2 Organisation

* Placez **les étapes critiques au début** ou regroupez-les logiquement.

* Utilisez une **nomenclature cohérente** (ex : JOBABC01, JOBABC02 pour une suite logique).

* Commentez les zones complexes ou conditionnelles.

---

### 3. Maîtriser les paramètres essentiels

#### 3.1 EXEC : lancement d'un programme

* `PGM=nom_du_programme`

* `COND=` pour éviter l'exécution d'étapes selon le code retour

	* Exemple : `COND=(4,LT)` => skip si RC < 4

#### 3.2 DD : gestion des fichiers

* `DSN=` : nom du dataset

* `DISP=` : disposition (NOUVEAU, EXISTANT, etc.)

        *  `(NEW,CATLG,DELETE)` : créer, cataloguer, supprimer si RC ≠ 0
 
* `UNIT=SYSDA`, `SPACE=`, `DCB=`, etc.


#### 3.3 SYSOUT

* Redirection vers la sortie système (ex: SYSOUT=A pour JES2)


### 4. Gestion des erreurs et retour-code

#### Bonnes pratiques :

* Testez systématiquement **le code retour** (`RC`) des steps importants.

* Utilisez `COND` pour éviter d’exécuter inutilement une étape si la précédente a échoué.

* Évitez les suppressions brutales de fichiers en cas d’erreur.

	* Exemple :
	```
	//ETP1 EXEC PGM=PROGA
	//ETP2 EXEC PGM=PROGB,COND=(0,EQ,ETP1)
	```

### 5. Gestion des fichiers

#### 5.1 DISP : dispositions à connaître

* `(NEW,CATLG,DELETE)` → si tout va bien, fichier catalogué, sinon supprimé.

* `(MOD,KEEP,KEEP)` → concaténation dans un fichier existant.

* `(SHR)` → partage du fichier (lecture uniquement).

#### 5.2 SPACE

* `SPACE=(TRK,(10,5),RLSE)` → 10 tracks initiaux, 5 supplémentaires, libération auto.

#### 5.3 DCB

* `DCB=(RECFM=FB,LRECL=80,BLKSIZE=800)` → format et structure du fichier

### 6. Conventions et standards recommandés

| Élément                  | Bonnes pratiques                                           |
| ------------------------ | ---------------------------------------------------------- |
| **Nom de job**           | Max 8 caractères, éviter les noms génériques               |
| **Nom de step**          | Significatif, 8 caractères max                             |
| **Commentaires**         | Commencez chaque job par un bloc de commentaire explicatif |
| **Datasets temporaires** | Utilisez `&&TEMP` si fichier temporaire                    |
| **Fichiers SYSIN**       | Centralisez les paramètres d’exécution                     |


### 7. Exemple complet et commenté
	
	
	//* Job de tri de fichier client
	//TRIFICH  JOB (ACCT),'TRI CLIENT',CLASS=A,MSGCLASS=X
	//* Étape de tri
	//TRI      EXEC PGM=SORT
	//SORTIN   DD DSN=CLIENT.ENTREE,DISP=SHR
	//SORTOUT  DD DSN=CLIENT.TRI,DISP=(NEW,CATLG,DELETE),
	//             SPACE=(CYL,(10,5),RLSE),UNIT=SYSDA
	//SYSIN    DD *
	  SORT FIELDS=(1,10,CH,A)
	/*
	//SYSOUT   DD SYSOUT=*
	//SYSPRINT DD SYSOUT=*
	//SYSUDUMP DD SYSOUT=*
	


### 8. À éviter absolument

* Écrire un JCL sans test de retour-code
* Ne pas libérer l’espace (RLSE)
* Utiliser des noms de steps ou fichiers ambigus
* Laisser un fichier DISP=NEW sans SPACE défini
* Ne pas documenter un traitement conditionnel

### Conclusion

Écrire un bon JCL, c’est :

* Être rigoureux (structure, commentaires, logique).

* Protéger les données (via DISP, COND…).

* Faciliter la maintenance et la relecture.


---

## sélection de programmes utilitaires en COBOL

### 1. **IEFBR14**
* Fonction :

Programme “neutre” (il ne fait rien) — utilisé pour **allouer ou supprimer un dataset** sans exécuter de traitement.

* Exemple :
	
	```
	//ALLOCJOB JOB (ACCT),'ALLOC DATASET'
	//STEP01   EXEC PGM=IEFBR14
	//DD1      DD  DSN=MON.FICHIER.TEST,
	//             DISP=(NEW,CATLG,DELETE),
	//             SPACE=(TRK,1),UNIT=SYSDA
	```

* Explication :

Ce job **crée un fichier** (`DISP=NEW`) mais **aucun traitement n'est effectué**, car `IEFBR14` ne fait rien. Il est souvent utilisé pour **préparer des fichiers en amont** d’un traitement COBOL.

### 2. **IEBGENER**
* Fonction :

Copie de fichiers séquentiels. Très utilisé pour **dupliquer, transférer ou modifier légèrement** un fichier.

* Exemple :

	```
	//COPYJOB JOB (ACCT),'COPY FILE'
	//STEP01   EXEC PGM=IEBGENER
	//SYSPRINT DD  SYSOUT=*
	//SYSUT1   DD  DSN=ENTREE.DONNEES,DISP=SHR
	//SYSUT2   DD  DSN=SORTIE.DONNEES,DISP=(NEW,CATLG,DELETE),
	//             SPACE=(TRK,(1,1)),UNIT=SYSDA
	//SYSIN    DD  DUMMY
	```

* Explication :

Ce job **copie les données** du fichier `ENTREE.DONNEES` vers un **nouveau fichier de sortie**. `SYSIN DD DUMMY` signifie qu’aucune instruction spécifique n’est fournie.

### 3. **SORT (PGM=SORT ou ICETOOL)**
* Fonction :

Utilitaire très puissant pour **trier, filtrer, fusionner, éliminer des doublons,** etc.

* Exemple :

	```
	//SORTJOB  JOB (ACCT),'TRI FICHIER'
	//STEP01   EXEC PGM=SORT
	//SORTIN   DD  DSN=CLIENTS.ENTREE,DISP=SHR
	//SORTOUT  DD  DSN=CLIENTS.TRI,DISP=(NEW,CATLG,DELETE),
	//             SPACE=(CYL,(10,5)),UNIT=SYSDA
	//SYSOUT   DD  SYSOUT=*
	//SYSIN    DD  *
	  SORT FIELDS=(1,10,CH,A)
	/*
	```

* Explication :

Tri d’un fichier sur les 10 premiers caractères de chaque ligne (ordre alphabétique croissant).

### 4. **IEHLIST**
* Fonction :

Liste le contenu d’un volume ou d’un PDS (Partitioned Dataset).

* Exemple :

	```
	//LISTJOB  JOB (ACCT),'LISTE PDS'
	//STEP01   EXEC PGM=IEHLIST
	//SYSPRINT DD  SYSOUT=*
	//SYSIN    DD  *
	  LISTDS DSNAME=MON.PDS.EXEMPLE, MEMBERS
	/*
	```

* Explication :

Affiche **la liste des membres** d’un dataset partitionné (`PDS`). Utile pour savoir ce qu’il contient avant de lancer un programme COBOL.

### 5. **IDCAMS**
* Fonction :

Gestion des fichiers **VSAM** (définition, suppression, impression, etc.)

* Exemple : Création d’un fichier KSDS

	```
	//IDCAMS JOB (ACCT),'CREER VSAM'
	//STEP01 EXEC PGM=IDCAMS
	//SYSPRINT DD SYSOUT=*
	//SYSIN    DD *
	  DEFINE CLUSTER (NAME=MON.FICHIER.VSAM -
	    INDEXED KEYS(10,0) RECORDSIZE(80 80) -
	    TRACKS(5 1) CISZ(4096)) -
	    DATA(NAME=MON.FICHIER.VSAM.DATA) -
	    INDEX(NAME=MON.FICHIER.VSAM.INDEX)
	/*
	```

* Explication :

Crée un fichier **VSAM de type KSDS** (`accès direct`) avec une clé de 10 caractères. IDCAMS est **incontournable** pour tout traitement VSAM en COBOL.

### 6. **IEBCOPY**

* Fonction :

Copie ou sauvegarde de membres dans un PDS — très utile pour **sauvegarder des programmes COBOL** ou **mettre à jour un PDS**.

* Exemple :

	```
	//COPYPDS JOB (ACCT),'COPY MEMBRES'
	//STEP01   EXEC PGM=IEBCOPY
	//SYSPRINT DD  SYSOUT=*
	//INPDS    DD  DSN=COBOL.SOURCE,DISP=SHR
	//OUTPDS   DD  DSN=COBOL.SOURCE.SAUVE,DISP=SHR
	//SYSIN    DD  *
	  COPY INDD=INPDS,OUTDD=OUTPDS
	  SELECT MEMBER=(PGM1,PGM2)
	/*
	```

* Explication :

Copie les membres `PGM1` et `PGM2` d’un PDS vers un autre. Très utilisé pour **faire des sauvegardes manuelles**.

---

### Récapitulatif synthétique


| Utilitaire   | Fonction principale             | Usage typique                     |
| ------------ | ------------------------------- | --------------------------------- |
| **IEFBR14**  | Création/suppression de fichier | Init/finalisation sans traitement |
| **IEBGENER** | Copie de fichiers               | Dupliquer un fichier séquentiel   |
| **SORT**     | Tri et filtrage                 | Trier un fichier clients          |
| **IEHLIST**  | Lister contenu PDS              | Vérifier les membres COBOL        |
| **IDCAMS**   | Gestion des fichiers VSAM       | Créer/supprimer un fichier KSDS   |
| **IEBCOPY**  | Copier membres d’un PDS         | Sauvegarder des programmes COBOL  |


---

## Introduction à la gestion de projets

### Objectif pédagogique

* Comprendre les fondamentaux de la gestion de projets.

* Identifier les étapes d’un projet.

* Utiliser des outils simples pour planifier, suivre et clôturer un projet.

* Collaborer efficacement dans un projet (rôles, communication, risques).