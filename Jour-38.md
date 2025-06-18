# Objectifs journaliers MS COBOL-SQL P4

# Daily – Mercredi 18 Juin (Jour-38)


##  Mise en pratique

---

## Objectifs

* [ ] Maîtriser les **principes de structure** d’un JCL avec la mise en pratique via les exercices.
* [ ] Savoir **écrire, lire et modifier** des scripts JCL.

---

## Déroulement

Cette journée est sous le signe de la manipulation et de la mise en pratique des informations reçues dans la présentation du JCL dans un environnement Mainframe.
La journée va être découpée en objectifs à réaliser (effectuer les exercices) pour faire une correction en groupe et ainsi effecter tous les exercices prévus

---

## RAPPEL DES EXERCICES A REALISER
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
