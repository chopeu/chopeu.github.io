# Objectifs journaliers MS COBOL-SQL P4

# Daily – Mardi 17 Juin (Jour-37)


##  Formation JCL – Introduction et Pratique
##  Comprendre et utiliser le JCL dans un environnement MVS

---

## Objectifs

* [ ] Comprendre le rôle du **JCL** dans un environnement **Mainframe MVS**.
* [ ] Maîtriser les **principes de structure** d’un JOB (JOB/EXEC/DD).
* [ ] Savoir **écrire, lire et modifier** des scripts JCL.
* [ ] Être capable de **diagnostiquer les erreurs courantes** et les corriger.

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
    
---

### 8. LA CARTE EXEC  

   * Lance une **étape du job**

   * Paramètres principaux : `PGM=`, `TIME=`, `REGION=`, `PARM=`

**Exemple :**
		
	//STEP1 EXEC PGM=MONPROG,TIME=(1,30),REGION=250K
		

---

### 9. LA CARTE DD : DÉFINITION DE FICHIERS  

* DD = Data Definition

* Décrit les **fichiers utilisés par le programme**

* Paramètres : `DSN=`, `DISP=`, `UNIT=`, `DCB=`, `SPACE=`


---

### 10.  DISP : DISPOSITIONS DE FICHIERS  

DISP=(statut, fin normale, fin anormale)

   * `NEW`, `OLD`, `SHR`, `MOD`
   * `KEEP`, `DELETE`, `CATLG`, `PASS`

**Exemples :**

   * Création : `DISP=(NEW,CATLG,DELETE)`
   * Partage : `DISP=(SHR)`


---

### 11.  DCB ET CARACTÉRISTIQUES  

* `DCB=(RECFM=FB,LRECL=80,BLKSIZE=800)`

* Décrit :

    * Format des enregistrements
    * Longueur logique (LRECL)
    * Taille des blocs (BLKSIZE)

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

    Crée un `//INPFILE DD DSN=DATASET1,DISP=SHR`

    Ajoute :
		```bash
			//        DD DSN=DATASET2,DISP=SHR
			//        DD DSN=DATASET3,DISP=SHR
		```
**Résultat attendu** : Le programme lit les trois fichiers comme un seul flux.
 
### Exercice 10 – Utiliser SYSIN avec fin personnalisée (DLM)
**Objectif** : Lire des données JCL avec un délimiteur spécifique.

    Carte :
		```bash
			//SYSIN DD DATA,DLM=$$
			Ligne 1
			Ligne 2
			$$
		```
**Résultat attendu** : Le programme lit les deux lignes puis s’arrête sur `$$`.

 


---
