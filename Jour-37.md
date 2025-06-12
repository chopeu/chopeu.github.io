# Objectifs journaliers MS COBOL-SQL P4

# Daily – Mardi 17 Juin (Jour-37)


##  Formation JCL – Introduction et Pratique
##  Comprendre et utiliser le JCL dans un environnement MVS

---

## Définitions clés

### Concepts fondamentaux

* [ ] **SGBD (Système de Gestion de Base de Données)** : Logiciel permettant de stocker, manipuler et gérer des données. Exemple : PostgreSQL.
* [ ] **Base de données** : Ensemble structuré d’informations, stockées de manière à pouvoir être consultées et modifiées facilement.
* [ ] **Base de données relationnelle** : Base de données où les données sont organisées en **tables** (lignes + colonnes), avec des relations entre elles.
* [ ] **Clé primaire (Primary Key)** : Champ unique qui identifie chaque ligne dans une table (ex : `id`).
* [ ] **SQL (Structured Query Language)** : Langage permettant de manipuler les bases relationnelles (création de tables, insertion, mise à jour, requêtes, etc.).
* [ ] **CRUD** : Opérations de base sur les données :

  * Create : Créer des données
  * Read   : Lire des données
  * Update : Modifier des données
  * Delete : Supprimer des données

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


## Exercice : Création et interrogation d’une table `clients`

### Étapes dans PostgreSQL (interface `psql`)

```sql
-- 1. Créer une base de données (si besoin)
CREATE DATABASE exercice_sql;
\c exercice_sql  -- Se connecter à la base

-- 2. Créer une table clients
CREATE TABLE clients (
    id SERIAL PRIMARY KEY,
    nom VARCHAR(50),
    prenom VARCHAR(50),
    telephone VARCHAR(20)
);

-- 3. Insérer quelques lignes
INSERT INTO clients (nom, prenom, telephone)
VALUES 
    ('Durand', 'Sophie', '0612345678'),
    ('Lemoine', 'Alex', '0623456789'),
    ('Martin', 'Lucie', '0634567890');

-- 4. Lire toutes les données
SELECT * FROM clients;

-- 5. Rechercher un client par ID (ex: ID = 2)
SELECT * FROM clients WHERE id = 2;
```

---


Très bien. Voici une mise à jour de ton document avec :

* Une clarification sur la différence entre **utilisateur** et **base de données**.
* Quelques **commandes utiles** dans PostgreSQL (`\`).
* Un point sur la **nomenclature SQL/PostgreSQL**.
* Les **types de données** les plus courants.

---

## Concepts complémentaires PostgreSQL

### Utilisateur PostgreSQL vs Base de données

* **Utilisateur PostgreSQL** : Compte autorisé à se connecter au serveur PostgreSQL. Par défaut, il existe un utilisateur nommé `postgres`.

  * Chaque utilisateur peut avoir des permissions spécifiques sur les bases.
  * Création :

    ```bash
    sudo -u postgres createuser nom_utilisateur
    ```

    Avec droits superutilisateur :

    ```bash
    sudo -u postgres createuser --superuser nom_utilisateur
    ```

* **Base de données** : Conteneur logique dans lequel sont stockées les tables, vues, fonctions, etc.
  Chaque base appartient à un utilisateur (le "owner") et peut être utilisée par d'autres si les droits sont accordés.

  * Création :

    ```bash
    createdb nom_base
    ```

---

## Commandes utiles dans l’interface `psql`

Ces commandes commencent par un antislash (`\`) et ne sont **pas** du SQL, ce sont des **méta-commandes** propres à `psql`.

| Commande       | Description                              |
| -------------- | ---------------------------------------- |
| `\l`           | Liste toutes les bases de données        |
| `\c nom_base`  | Se connecter à une base de données       |
| `\dt`          | Liste les tables de la base actuelle     |
| `\du`          | Liste les utilisateurs PostgreSQL        |
| `\q`           | Quitter l'interface `psql`               |
| `\d nom_table` | Décrit la structure d'une table          |
| `\h`           | Aide sur les commandes SQL (`\h SELECT`) |

---

## Nomenclature 

* **Noms de tables** : souvent en minuscules, pluriels (`clients`, `commandes`).
* **Noms de colonnes** : simples et explicites (`nom`, `prenom`, `date_naissance`).
* **Snake\_case** utilisé de préférence (`date_creation`, `code_postal`).
* Les noms **entre guillemets doubles** deviennent sensibles à la casse :

  ```sql
  SELECT "Nom" FROM clients; -- Attention à la casse
  ```

---

## Types de données courants en PostgreSQL

| Type           | Description                           | Exemple                           |
| -------------- | ------------------------------------- | --------------------------------- |
| `INTEGER`      | Entier                                | `42`                              |
| `SERIAL`       | Entier auto-incrémenté (clé primaire) | `id SERIAL PRIMARY KEY`           |
| `VARCHAR(n)`   | Chaîne de caractères (longueur max)   | `nom VARCHAR(50)`                 |
| `TEXT`         | Chaîne de longueur illimitée          | `description TEXT`                |
| `BOOLEAN`      | Vrai / Faux                           | `true`, `false`                   |
| `DATE`         | Date (AAAA-MM-JJ)                     | `'2024-05-26'`                    |
| `TIMESTAMP`    | Date + heure                          | `'2024-05-26 14:00:00'`           |
| `NUMERIC(x,y)` | Nombre avec décimales                 | `prix NUMERIC(10,2)` => `1234.56` |

---
