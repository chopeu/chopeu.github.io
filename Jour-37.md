# Objectifs journaliers MS COBOL-SQL P4

# Daily – Mardi 17 Juin (Jour-37)


##  Formation JCL – Introduction et Pratique
##  Comprendre et utiliser le JCL dans un environnement MVS

---

## Objectifs

* [ ] Comprendre le rôle du **JCL** dans un environnement **Mainframe MVS**.
* [ ] Maîtriser les **principes de structure** d’un JOB (JOB/EXEC/DD).
* [ ] Savoir **écrire, lire et modifier** des scripts JCL.

---

## le JCL --> KESAKO 

### 1. QU’EST-CE QUE LE JCL ?

* Langage de **commande système**, utilisé pour soumettre des travaux (jobs).

* Permet de décrire **quels programmes exécuter**, avec **quelles ressources**.

* Utilisé dans les environnements IBM MVS, en particulier avec COBOL.

* Composé de **cartes** (instructions), autrefois sur cartes perforées.


### 2. HISTORIQUE ET CONTEXTE

* Anciennement, les instructions étaient encodées sur **cartes perforées**.

* Aujourd’hui, ces instructions sont écrites dans des **fichiers texte** soumis par lot.

* Le JCL est resté **puissant mais rigide** : il exige une **syntaxe précise**.

---

### 3. LES CARTES JCL PRINCIPALES

| Carte       | Rôle principal                     |
| ----------- | ---------------------------------- |
| **JOB**     | Définit le job global              |
| **EXEC**    | Lance une étape (programme)        |
| **DD**      | Décrit un fichier                  |
| **PROC**    | Appelle une procédure préexistante |
| **PEND**    | Termine une procédure              |
| \**//* \*\* | Carte de commentaire               |
| **//**      | Fin de JOB                         |


---

### 4. STRUCTURE D’UN JOB

* Un JOB est structuré par **étapes**.

* Chaque étape contient un **EXEC** et des **DD**.

* La structure suit un **ordre fixe**.

---

### 5. SYNTAXE GÉNÉRALE D’UNE CARTE

* Chaque carte commence par `//` en colonnes 1-2.

* 4 zones distinctes :

     * 1. Nom de la carte (`MYSTEP`)
     * 2. Type (`EXEC`, `DD`, etc.)
     * 3. Paramètres (`PGM=COBTP01`)
     * 4. Commentaire

Exemple :

    	
	//MYSTEP EXEC PGM=COBTP01,TIME=2
   
    
---

### 6. PARAMÈTRES : POSITIONNELS VS MOTS-CLÉS    

**Paramètres positionnels** :

   * Leur ordre **détermine leur rôle**.
   * Ex. : `TOTO,,TUTU` → A=TOTO, B=omise, C=TUTU

**Paramètres à mot-clé** :

   * On **nomme chaque paramètre**.
   * Ex. : `PGM=COBTP01, TIME=(1,30)`

---

### 7. LA CARTE JOB    

* Débute chaque JOB.

* Définit : nom du job, programmeur, classes, priorités, etc.

**Exemple :**

    	
	//MYJOB JOB '4923E02',MSGCLASS=T,MSGLEVEL=(1,1),TIME=(5,0)
    
**Paramètres clés :**

   * `CLASS`, `MSGCLASS`, `TIME`, `PRTY`, `NOTIFY`, `RESTART`, `TYPRUN`, `COND`
   
**Paramètres clés de la carte JOB**

| **Paramètre**        | **Description**                                                     | **Exemple**                   |
| -------------------- | ------------------------------------------------------------------- | ----------------------------- |
| `ACCT` ou `(compte)` | Code de facturation ou numéro de compte (souvent entre parenthèses) | `(1234)`                      |
| `‘description’`      | Nom ou commentaire libre entre apostrophes                          | `'MAINTENANCE BASES'`         |
| `CLASS`              | Catégorie de traitement (A à Z, ou 0 à 9 selon le site)             | `CLASS=A`                     |
| `MSGCLASS`           | Classe de sortie des messages système                               | `MSGCLASS=X`                  |
| `MSGLEVEL`           | Niveau de détail des messages JCL dans la sortie                    | `MSGLEVEL=(1,1)`              |
| `NOTIFY`             | Identifiant TSO à prévenir en fin de job                            | `NOTIFY=&SYSUID`              |
| `TYPRUN`             | Type d’exécution du job (TEST, HOLD, SCAN, etc.)                    | `TYPRUN=SCAN`                 |
| `TIME`               | Limite de temps allouée à l’exécution                               | `TIME=(1,30)` = 1 min 30 sec  |
| `PRTY`               | Priorité du job dans la file d'attente (1 = haute, 15 = basse)      | `PRTY=5`                      |
| `COND`               | Condition d’exécution globale du job                                | `COND=(8,LT)`                 |
| `RESTART`            | Point de redémarrage en cas d’échec                                 | `RESTART=STEP2`               |
| `REGION`             | Quantité de mémoire disponible pour l’exécution                     | `REGION=1024K` ou `REGION=4M` |
| `USER=`              | (optionnel) Identifiant utilisateur sous sécurité RACF              | `USER=JDOE`                   |
| `PASSWORD=`          | Mot de passe associé à `USER=`                                      | `PASSWORD=SECRET1`            |

**Détails supplémentaires utiles**

   * `CLASS` et `MSGCLASS` sont **souvent fixés par le service système**, selon le type de travail (batch, production, test).

   * `MSGLEVEL=(1,1)` :

        * Le **1er chiffre** : messages JCL

        * Le **2e chiffre** : détails sur la soumission des étapes

   * `TYPRUN=SCAN` : très utile pour **valider un job sans l’exécuter**

   * `RESTART=nom_etape` permet de **reprendre l’exécution** après une erreur sans tout relancer
    
---

### 8. LA CARTE EXEC  

   * Lance une **étape du job**

   * Paramètres principaux : `PGM=`, `TIME=`, `REGION=`, `PARM=`

**Exemple :**
		
	//STEP1 EXEC PGM=MONPROG,TIME=(1,30),REGION=250K
		

**Paramètres clés de la carte EXEC**

| **Paramètre** | **Description**                                                                 | **Exemple**                  |
| ------------- | ------------------------------------------------------------------------------- | ---------------------------- |
| `PGM=`        | Nom du **programme à exécuter** (load module en bibliothèque)                   | `PGM=IEFBR14`                |
| `PROC=`       | Nom d’une **procédure (proc)** à appeler (procédure cataloguée)                 | `PROC=COPYPROC`              |
| `PARM=`       | Paramètres **transmis au programme** (chaîne de caractères)                     | `PARM='TEST01,PROD'`         |
| `COND=`       | Condition d’exécution de l’étape, **interprétée en fonction des RC précédents** | `COND=(4,LT)`                |
| `TIME=`       | Temps maximum autorisé pour l'étape                                             | `TIME=(1,30)` → 1min30       |
| `REGION=`     | Quantité de mémoire allouée pour l'étape                                        | `REGION=512K` ou `REGION=2M` |
| `ACCT=`       | Compte à facturer (rare en pratique aujourd’hui)                                | `ACCT=(12345)`               |
| `ADDRSPC=`    | Espace d’adressage pour l’étape (VIRT ou REAL)                                  | `ADDRSPC=REAL`               |
| `DYNAMNBR=`   | Nombre max de fichiers dynamiquement allouables                                 | `DYNAMNBR=20`                |


**Détails complémentaires utiles**

   * `PGM`= est **obligatoire** si l’on exécute un programme directement.

   * `PROC=` est utilisé à la place de `PGM=` quand on appelle une **procédure cataloguée**.

   * `PARM=` est transmis tel quel au programme et peut contenir plusieurs éléments séparés par des virgules.

   * `COND=(RC,val)` indique : **si condition vraie, alors on saute l’étape**.

       * Exemple : `COND=(0,EQ)` → saute si RC précédent est égal à 0.

   * `REGION=` est parfois contrôlé par l’administration système pour éviter les abus mémoire.


---

### 9. LA CARTE DD : DÉFINITION DE FICHIERS  

* DD = Data Definition

* Décrit les **fichiers utilisés par le programme**

* Paramètres : `DSN=`, `DISP=`, `UNIT=`, `DCB=`, `SPACE=`

**Paramètres principaux de la carte DD**

| **Paramètre** | **Description**                                                           | **Exemple**                           |
| ------------- | ------------------------------------------------------------------------- | ------------------------------------- |
| `DSN=`        | Nom du dataset (fichier)                                                  | `DSN=USER01.DATA.FICHIER1`            |
| `DISP=`       | Disposition du fichier (création, conservation, suppression)              | `DISP=(NEW,CATLG,DELETE)`             |
| `UNIT=`       | Unité physique ou logique (disque, bande)                                 | `UNIT=SYSDA`                          |
| `SPACE=`      | Espace disque à allouer (en blocs, pistes ou cylindres)                   | `SPACE=(CYL,(1,1))`                   |
| `DCB=`        | Description du fichier (format, longueur, taille bloc)                    | `DCB=(RECFM=FB,LRECL=80,BLKSIZE=800)` |
| `SYSOUT=`     | Dirige la sortie vers l’imprimante (spool)                                | `SYSOUT=*`                            |
| `DUMMY`       | Utilisé pour ignorer un fichier (faux fichier)                            | `DD DUMMY`                            |
| `*` ou `DATA` | Données en ligne à saisir directement dans le JCL                         | `DD *` ou `DD DATA`                   |
| `DLM=`        | Délimiteur de fin pour les données en ligne                               | `DLM=@@`                              |
| `VOL=SER=`    | Volume spécifique où se trouve le fichier                                 | `VOL=SER=VOL001`                      |
| `LABEL=`      | Gestion des étiquettes pour bandes magnétiques                            | `LABEL=1`                             |
| `AMP=`        | Options avancées d'allocation (accès VSAM, etc.)                          | `AMP='AMORG'`                         |
| `RECFM=`      | Format des enregistrements : F (fixe), V (variable), FB (fixe bloc), etc. | `RECFM=FB`                            |
| `LRECL=`      | Longueur logique d’un enregistrement                                      | `LRECL=80`                            |
| `BLKSIZE=`    | Taille d’un bloc (groupe d’enregistrements)                               | `BLKSIZE=800`                         |


---

### 10.  DISP : DISPOSITIONS DE FICHIERS  

DISP=(statut, fin normale, fin anormale)

   * `NEW`, `OLD`, `SHR`, `MOD`
   * `KEEP`, `DELETE`, `CATLG`, `PASS`

**Exemples :**

   * Création : `DISP=(NEW,CATLG,DELETE)`
   * Partage : `DISP=(SHR)`

**Détails sur le paramètre DISP**

Le plus important et souvent le plus mal compris :

	DISP=(statut, fin_normale, fin_anormale)

| Élément          | Rôle                       | Exemples                                   |
| ---------------- | -------------------------- | ------------------------------------------ |
| **statut**       | `NEW`, `OLD`, `SHR`, `MOD` | `NEW` = création, `SHR` = lecture partagée |
| **fin normale**  | `KEEP`, `CATLG`, `DELETE`  | Que faire si le job réussit                |
| **fin anormale** | `KEEP`, `CATLG`, `DELETE`  | Que faire si le job échoue                 |


**Exemple complet :**

	DISP=(NEW,CATLG,DELETE)

→ crée un fichier, le catalogue si OK, le supprime sinon.

Le paramètre DISP (Disposition) est l’un des plus importants et sensibles dans une carte DD en JCL. Il permet de définir comment le système doit traiter un dataset avant, pendant et après l’exécution d’un job ou d’une étape.
**DISP – Syntaxe complète**

	DISP=(état, action_si_OK, action_si_erreur)

**Paramètres principaux de DISP**


**1. État (Status) — 1er champ**

Décrit **la relation du fichier avec le système avant l'exécution** de l'étape.
| **Valeur** | **Signification**                                                  | **Effet**                           |
| ---------- | ------------------------------------------------------------------ | ----------------------------------- |
| `NEW`      | Le fichier **n’existe pas** et doit être **créé**                  | Le système alloue l’espace          |
| `OLD`      | Le fichier **existe** et doit être **verrouillé** (accès exclusif) | Attention à la contention           |
| `SHR`      | Le fichier **existe** et peut être **partagé**                     | Lecture ou écriture en mode partagé |
| `MOD`      | Le fichier **existe ou sera créé**, et on va **écrire à la fin**   | Très utilisé pour concaténation     |


**2. Action si fin normale (Normal Disposition) — 2e champ**

Décrit ce que le système doit faire **si l’étape se termine sans erreur (RC=0)**.
| **Valeur** | **Signification**                                                       |
| ---------- | ----------------------------------------------------------------------- |
| `KEEP`     | Conserve le fichier mais ne l’ajoute pas au catalogue                   |
| `CATLG`    | Conserve **et catalogue** le fichier (référencement automatique)        |
| `PASS`     | Transmet le fichier aux **étapes suivantes** sans le conserver à la fin |
| `DELETE`   | Supprime le fichier même si tout s’est bien passé                       |


**3. Action si fin anormale (Abnormal Disposition) — 3e champ**

Même chose, mais **si le job ou l’étape échoue** (erreur système ou RC≠0).
| **Valeur** | **Signification**                                       |
| ---------- | ------------------------------------------------------- |
| `KEEP`     | Le fichier est conservé pour diagnostic                 |
| `CATLG`    | Il est conservé **et catalogué malgré l’erreur**        |
| `DELETE`   | Il est supprimé automatiquement (par défaut recommandé) |



**Exemples courants**

**1. Créer un fichier temporaire (et le supprimer en cas d’erreur)**

	DISP=(NEW,CATLG,DELETE)

**2. Lire un fichier existant, partagé avec d’autres jobs**

	DISP=SHR

**3. Lire un fichier existant en exclusivité (danger en production)**

	DISP=OLD

**4. Créer un fichier et le transmettre à l’étape suivante sans le cataloguer**

	DISP=(NEW,PASS,DELETE)

**Rappels**

   * `NEW` implique toujours qu’on doive préciser `SPACE=`, `UNIT=`, etc.

   * `MOD` est dangereux sans `KEEP` : risque de réécriture multiple

   * Toujours penser à ce que le système doit faire **si le job échoue**

   * `CATLG` permet d'accéder au dataset plus tard via ISPF ou un autre job


---

### 11.  DCB ET CARACTÉRISTIQUES  

* `DCB=(RECFM=FB,LRECL=80,BLKSIZE=800)`

* Décrit :

    * Format des enregistrements
    * Longueur logique (LRECL)
    * Taille des blocs (BLKSIZE)
    
**Paramètre DCB – Syntaxe générale**

Le DCB est un groupe de sous-paramètres décrivant la **structure des enregistrements** dans un dataset.

	DCB=(RECFM=xx,LRECL=nnn,BLKSIZE=nnnn)    

**Liste des sous-paramètres `DCB` et leur description**

| **Paramètre** | **Signification**     | **Description**                                     |
| ------------- | --------------------- | --------------------------------------------------- |
| `RECFM=`      | Record Format         | Type et organisation des **enregistrements**        |
| `LRECL=`      | Logical Record Length | **Longueur logique d’un enregistrement**, en octets |
| `BLKSIZE=`    | Block Size            | **Taille d’un bloc** (groupe d’enregistrements)     |
| `DSORG=`      | Data Set Organization | Organisation : séquentielle, partitionnée, VSAM     |
| `BUFNO=`      | Buffer Number         | Nombre de buffers utilisés pour l’I/O               |
| `OPTCD=`      | Option Code           | Options spécifiques pour la lecture/écriture        |
| `MACRF=`      | Macro Record Format   | Mode d’accès aux fichiers (lecture, écriture...)    |

**Détails des principaux sous-paramètres**

**RECFM (Record Format)**

Définit le **format des enregistrements** dans le fichier.

| **Code** | **Type**         | **Description**                              |
| -------- | ---------------- | -------------------------------------------- |
| `F`      | Fixed            | Enregistrements de **longueur fixe**         |
| `FB`     | Fixed Blocked    | Longueur fixe + **groupés en blocs**         |
| `V`      | Variable         | Enregistrements de **longueur variable**     |
| `VB`     | Variable Blocked | Variable + **groupés en blocs**              |
| `U`      | Undefined        | Utilisé avec des fichiers binaires (LOADLIB) |
| `A`      | ASA              | Avec contrôle de format ASA (impression)     |
| `M`      | Machine          | Format machine, souvent utilisé avec `U`     |

**Exemples :**

   * `RECFM=FB` → Enregistrements fixes, groupés

   * `RECFM=VB` → Enregistrements variables, groupés

   * `RECFM=U` → Format indéfini (LOADLIB, SYSUT…)


**LRECL (Logical Record Length)**

   * Indique **combien d’octets** contient **chaque enregistrement logique**.

   * Obligatoire pour `F`, `FB`, `V`, `VB`.

   * Exemple : `LRECL=80` pour fichiers texte standard.


**BLKSIZE (Block Size)**

   * Nombre total d’octets d’un **bloc physique** (plusieurs enregistrements).

   * Doit être un **multiple de LRECL** dans la plupart des cas.

   * Exemple :

        * `LRECL=80` et `BLKSIZE=800` → 10 enregistrements par bloc

**DSORG (Data Set Organization)**

   * Définit le **type de dataset** :
   
| Code | Signification             |
| ---- | ------------------------- |
| `PS` | Physical Sequential       |
| `PO` | Partitioned (PDS)         |
| `DA` | Direct Access             |
| `VS` | VSAM                      |
| `IS` | Indexed Sequential (VSAM) |
   

**BUFNO (Buffer Number)**

   * Nombre de buffers utilisés pour accélérer l’I/O.

   * Plus de buffers = meilleure performance (mais plus de mémoire).

   * Valeur typique : `BUFNO=5` à `BUFNO=20`.
   
**OPTCD / MACRF (Avancé)**

   * `OPTCD=Q` : Accès rapide sans mise à jour du buffer

   * `MACRF=R` : Read

   * `MACRF=W` : Write

   * Peu utilisés dans des jobs simples
   
**Exemple complet de DCB**

				//FICOUT DD DSN=USERID.DATA.FICHIER1,
				//       DISP=(NEW,CATLG,DELETE),
				//       UNIT=SYSDA,SPACE=(CYL,(1,1)),
				//       DCB=(RECFM=FB,LRECL=80,BLKSIZE=800)


**Astuces**

   * **RECFM + LRECL** sont **obligatoires pour la plupart des fichiers séquentiels**.

   * **BLKSIZE** est facultatif : le système peut le calculer automatiquement si on l’omet.

   * Si `RECFM=U` → ne jamais spécifier `LRECL`.

   
---

### 12.  FICHIERS SPÉCIAUX

* **SYSOUT** : sortie imprimante

* **SYSIN** : données intégrées dans le JCL

* **SYSUDUMP** : dump mémoire en cas d’erreur  

---

### 13.  JOBLIB / STEPLIB

   * **JOBLIB** : bibliothèque de programmes valable pour tout le job

   * **STEPLIB** : valable uniquement pour une étape

**Exemple :**
		
	//JOBLIB DD DSN=HNF.LOADLIB,DISP=SHR
		

---

### 14.  CONCATÉNATION DE FICHIERS

Permet d’**enchaîner plusieurs fichiers** dans une même DD :


			//FIC DD DSN=TEST1,DISP=SHR
			//    DD DSN=TEST2,DISP=SHR
			//    DD DSN=TEST3,DISP=SHR


---

### 15.  COMMENTAIRES DANS UN JCL

   * Utiliser //* pour documenter

   * Améliore la **compréhension future** du job

   * Ex :
		
			//* Etape de compilation COBOL
		

---

### 16.  COND ET CONDITIONS D’ABANDON

   * Condition de non-exécution d’une étape

   * Exemple :
		
			COND=(4,LT)  → Si CR > 4, on exécute. Sinon on abandonne.
		
   * Utiliser EVEN / ONLY pour les comportements avancés.

---

## Exercices pratiques en JCL – Progressifs et pédagogiques

### Exercice 1 – Créer un job minimal
**Objectif** : Écrire le JCL le plus simple possible.

   * Crée un job nommé TESTJOB1.

   * Exécute le programme IEFBR14 (programme vide).

   * Ajoute MSGCLASS=X, CLASS=A, MSGLEVEL=(1,1).

**Résultat attendu** : Un job s’exécute et termine sans erreur.

### Exercice 2 – Définir un fichier temporaire
**Objectif** : Apprendre à utiliser `DD` avec fichier temporaire.

   * Crée un job avec une carte DD de type `DSN=&&MONFICHIER`.

   * Alloue un espace sur disque : `SPACE=(TRK,(1,1))`.

   * Ajoute `DISP=(NEW,PASS)` et `UNIT=SYSDA`.

**Résultat attendu** : Le fichier est alloué puis libéré à la fin du job.
 
### Exercice 3 – Créer un fichier permanent
**Objectif** : Créer un dataset physique.

   * Utilise `DSN=USERID.DATA.FICHIER1`.

   * Ajoute `DISP=(NEW,CATLG,DELETE)`, `SPACE=(CYL,(1,1))`.

   * Indique un `UNIT=SYSDA`, et un `DCB` (ex: `RECFM=FB,LRECL=80,BLKSIZE=800`).

**Résultat attendu** : Un fichier réel, consultable via ISPF 3.4.

### Exercice 4 – Impression avec SYSOUT
**Objectif** : Utiliser `SYSOUT` pour imprimer un message.

   * Crée une étape avec :
		```bash
				//IMPRIM DD SYSOUT=*,DCB=(RECFM=FBA,LRECL=133,BLKSIZE=1330)
		```
   * Ajoute une carte `SYSIN` contenant un message :
		```bash
				//SYSIN DD *
				Ceci est un test d'impression JCL
				/*
		```
**Résultat attendu** : Affichage dans SDSF, file d’attente imprimante.
 
### Exercice 5 – Réutiliser un fichier avec référence arrière
**Objectif** : Utiliser `DSN=*.STEP1.FICHIERTEMP`

   * Étape 1 : créer un fichier temporaire.

   * Étape 2 : réutilise ce fichier sans redéfinir `DSN`.

**Résultat attendu** : Réutilisation fonctionnelle du fichier sans recodage.


### Exercice 6 – Utiliser la carte EXEC avec paramètres 
**Objectif** : Paramétrer une exécution via `PGM=`, `TIME=`, `REGION=`, `PARM=`

   * Exécute un programme `PGM=COBTP01`.

   * Ajoute un `TIME=(1,0)`, `REGION=512K`.

   * Ajoute un `PARM='TEST01,PROD'`.

**Résultat attendu** : Transmission de paramètres visibles dans le programme COBOL.


### Exercice 7 – Conditionner l’exécution avec COND
**Objectif** : Ne pas exécuter une étape si la précédente a retourné un code erreur.

    Étape 1 : `PGM=IEFBR14`, retourne RC=0.

    Étape 2 : `PGM=AUTREPROG`, avec `COND=(0,NE)`.

**Résultat attendu** : Étape 2 s’exécute uniquement si RC ≠ 0 → donc **non exécutée ici**.

 
### Exercice 8 – Utiliser une bibliothèque de programmes (JOBLIB)
**Objectif** : Charger un programme à partir d’une bibliothèque spécifique.

   * Ajoute une carte `//JOBLIB DD DSN=USERID.LOADLIB,DISP=SHR` avant la première `EXEC`.

   * Exécute `PGM=HELLOCOB`.

**Résultat attendu** : Le programme est trouvé et lancé depuis la LOADLIB.

 
### Exercice 9 – Concaténer plusieurs fichiers d’entrée
**Objectif** : Lire plusieurs datasets dans le même `DD`.

   * Crée un `//INPFILE DD DSN=DATASET1,DISP=SHR`

   * Ajoute :
		```bash
			//        DD DSN=DATASET2,DISP=SHR
			//        DD DSN=DATASET3,DISP=SHR
		```
**Résultat attendu** : Le programme lit les trois fichiers comme un seul flux.
 
### Exercice 10 – Utiliser SYSIN avec fin personnalisée (DLM)
**Objectif** : Lire des données JCL avec un délimiteur spécifique.

   * Carte :
		```bash
			//SYSIN DD DATA,DLM=$$
			Ligne 1
			Ligne 2
			$$
		```
**Résultat attendu** : Le programme lit les deux lignes puis s’arrête sur `$$`.

 


---
