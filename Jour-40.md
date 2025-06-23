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


|        UNIT                  |
|------------------------------|
| SYSDA  → 🖴 Disque dur        |
| TAPE   → 📼 Bande            |
| 3390   → 🖴 Type disque IBM   |


---

## La gestion de projet  

### 2. Introduction à la gestion de projets
Objectif de la séquence :
Faire comprendre ce qu’est un projet, pourquoi on parle de gestion de projet et les enjeux associés.

#### 2.1 Définitions clés

* **Projet** : Ensemble d’activités temporaires visant à créer un produit, un service ou un résultat unique.

	➤ Exemple : Développer une application mobile, organiser une journée portes ouvertes.

* **Gestion de projet** : Ensemble des méthodes, outils et compétences permettant de planifier, piloter et clôturer un projet avec succès.

* **Cycle de vie d’un projet** : Ensemble des étapes successives de la vie d’un projet, de l’idée à la clôture.


#### 2.2 Objectifs d’un projet

Souvent résumés par le **triangle qualité – coût – délai** (ou triangle d’or du projet) :

* **Qualité** : répond-t-on au besoin réel ?

* **Coûts** : respecte-t-on le budget alloué ?

* **Délais** : livre-t-on dans les temps ?

Ces trois éléments sont interdépendants : modifier un angle impacte les autres.


#### 2.3 Exemples concrets

* **Créer un site web pour une PME locale** :

	➤ Livrables : maquette, site fonctionnel, notice d'utilisation.

	➤ Contraintes : budget serré, délais courts, image de marque soignée.

* **Organiser un événement interne (formation ou salon)** :

	➤ Étapes : réserver la salle, communiquer, gérer l’inscription, préparer le contenu, gérer les imprévus.

#### 2.4 Le cycle de vie d’un projet

Objectif de la séquence :
Comprendre les grandes étapes d’un projet et savoir les appliquer à un exemple concret.

Étapes du cycle de vie :

1. **Initiation**

	* Identifier le besoin, analyser la faisabilité, formaliser les objectifs.

	* Document clé : **cahier des charges simplifié** ou note d’intention.

2. **Planification**

	* Découpage des tâches, estimation des durées, budget, ressources.

	* Outils : **diagramme de Gantt, planning, feuille de route**.

3. **Exécution**

	* Réalisation concrète des tâches par l’équipe projet.

	* Suivi régulier, adaptation, pilotage.

4. **Suivi / Contrôle**

	* S’assurer que tout se déroule comme prévu (qualité, coûts, délais).

	* Réunions de suivi, tableaux de bord, gestion des risques.

5. **Clôture**

	* Livraison finale, validation client, bilan projet (retour d’expérience).

	* Archive des documents, apprentissages pour la suite.

#### 2.5 Rôles et responsabilités dans un projet

Objectif de la séquence :
Comprendre qui intervient dans un projet et qui est responsable de quoi.

Les rôles clés :

**Chef de projet**

* Pilote le projet, planifie, coordonne, arbitre.

* Interface entre les parties prenantes et l’équipe.

**Équipe projet**

* Réalise les tâches techniques ou fonctionnelles.

* Exemple : développeurs, designers, testeurs.

**Client**

* Commanditaire du projet, formule les besoins.

* Valide les livrables.

**Parties prenantes**

* Toute personne impactée ou impliquée dans le projet : utilisateurs finaux, financeurs, direction, etc.


#### 2.6 Les outils de pilotage projet

Objectif :
Découvrir les outils concrets pour suivre l’avancement, piloter les tâches et visualiser le projet dans le temps.

**Outils de suivi** :

* **Diagramme de Gantt** (outil visuel d’ordonnancement)

	➤ Exemple : GanttProject, TeamGantt, MS Project

* **Kanban** (organisation visuelle des tâches par colonnes)

	➤ Exemple : Trello, Jira, ClickUp

* **Tableaux de bord** (indicateurs projet) : % d’avancement, retard, charges.

**Indicateurs clés** :

* Avancement global

* Dépassement des délais ou des coûts

* Répartition de la charge

* Risques identifiés et en cours


#### 2.7 Méthodes agiles vs méthode traditionnelle

Objectif :
Comprendre la différence entre la gestion de projet classique (cycle en V) et les méthodes agiles, avec mise en pratique.

* **Méthode traditionnelle (cycle en V)**

	* Approche séquentielle : on passe d’une étape à l’autre.

	* Tout est défini au départ (coûts, délais, périmètre).

	* Exemples : construction, projet bien cadré.

* **Méthode agile**

	* Approche itérative, centrée sur la **valeur livrée rapidement**.

* Exemple principal : **Scrum**

	* Rôles : Product Owner, Scrum Master, Équipe

	* Artefacts : backlog, sprints, revues, rétrospectives

Adaptée à l’informatique, aux projets évolutifs ou incertains.


#### 2.8 Les risques et les imprévus

Objectif :
Savoir anticiper et gérer les risques dans un projet.

* **Identifier les risques** :

	* Retards, problèmes techniques, absence d’un membre de l’équipe

	* Demandes changeantes du client

* **Outils** :

	* **Matrice de risques** (probabilité / impact)

	* **Plan de mitigation** : que faire si le risque se produit ?

	* **Journal des risques** à tenir à jour

#### 2.9 Clôture et retour d’expérience

Objectif :
Savoir bien conclure un projet et tirer les leçons pour les suivants.

**Contenus** :

* Livraison finale : validation du client

* Archivage des documents

* Bilan projet (ce qui a bien fonctionné / à améliorer)

* REX : retour d’expérience pour capitaliser