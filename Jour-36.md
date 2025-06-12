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

Cette puissance d’un système informatique dépend de nombreux paramètres :
*	Rapidité de calcul du processeur central
*	Nombre de canaux
*	Taille de la mémoire centrale
*	Performance des périphériques
*	Type de système d’exploitation
*	Réglage (optimisation) du système d’exploitation.

En règle générale, la puissance d’une machine se donne en MIPS (Million d’Instructions Par Seconde).


```bash
psql --version
```

Si non installé :

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### 2. Démarrer PostgreSQL et se connecter

```bash
sudo service postgresql start
sudo -u postgres psql
```

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
