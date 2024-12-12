

# Projet ETL 
## Gestion des Recettes et Ingrédients
### Description
Ce projet a pour objectif de concevoir et d'implémenter un pipeline ETL (Extract, Transform, Load) pour gérer des données de recettes et d'ingrédients. L'application vise à extraire des données brutes de sources externes (API, fichiers JSON), les nettoyer et les transformer, puis les charger dans une base de données relationnelle (PostgreSQL) de manière organisée.

Le pipeline permet :

Une structure normalisée et optimisée des données pour réduire les redondances.
Une facilité d’accès aux informations telles que les recettes, les ingrédients, et leurs relations via des tables bien définies.

## Phase 1 : Extraction (Extract)
### Récupération des données depuis :
Une API externe sur les recettes.
Des fichiers JSON représentant les données d'ingrédients et de recettes.

## Phase 2 : Transformation (Transform)
Cette phase est cruciale pour convertir les données brutes extraites en un format cohérent et utilisable. Elle implique plusieurs étapes de nettoyage, standardisation et structuration.

### 1. Nettoyage des données
Objectif :
Supprimer les anomalies et préparer les données pour qu'elles soient cohérentes et exploitables.

Étapes :
* Supprimer les doublons :

Identifier les enregistrements en double dans les données des recettes ou des ingrédients.
Les supprimer tout en conservant une version unique par nom ou identifiant.
* Gérer les valeurs manquantes :

Vérifier les champs essentiels (par ex. nom des recettes, ingrédients, quantités).
Remplacer les valeurs manquantes par des valeurs par défaut ou supprimer les lignes si nécessaire.
* Nettoyer les chaînes de caractères :

Supprimer les espaces inutiles.
Corriger les fautes de frappe dans les noms d'ingrédients ou de recettes.
### 2. Normalisation des données
Objectif :
Uniformiser les données pour garantir leur compatibilité avec les tables relationnelles.

Étapes :
* Uniformisation des unités de mesure :

Convertir les quantités dans une unité standard (par ex. convertir "2 tasses" en "250 ml").
Créer une table de référence pour les unités (unit_conversion).
* Application du stemming ou lemmatisation :

Utiliser un algorithme comme le Paice stemmer pour réduire les variations d'écriture des ingrédients (par ex. "tomatoes" → "tomato").
Supprimer les mots inutiles ou les variantes grammaticales.
* Formatage des dates :

Standardiser les formats de dates pour les champs comme la création ou l’ajout d’une recette (par ex. YYYY-MM-DD).
### 3. Création des relations entre les entités
Objectif :
Structurer les données pour refléter les relations entre recettes, ingrédients et autres entités.

Étapes :
* Identifier les relations :

Associer chaque recette à ses ingrédients.
Ajouter des quantités précises pour chaque relation.
* Définir des identifiants uniques (UUID) :

Assigner un identifiant unique à chaque recette et à chaque ingrédient pour éviter les conflits.
* Créer des tables de jointure :

Exemple : une table recipe_ingredient pour représenter les quantités d'ingrédients par recette.
### 4. Dé-duplication des ingrédients
Objectif :
Supprimer les redondances dans les noms d'ingrédients.

Étapes :
* Utiliser des techniques NLP pour regrouper les noms similaires.
Exemple : "tomato", "tomatoes", "tomate" → "tomato".
Regrouper les variantes manuellement si les règles automatiques ne suffisent pas.
Mettre à jour la table des ingrédients avec des versions unifiées.
### 5. Génération des clés étrangères
Objectif :
Préparer les données pour respecter l'intégrité référentielle lors du chargement.

Étapes :
* Ajouter des clés étrangères (foreign keys) pour relier les tables.
Exemple :
La clé primaire de ingredient devient une clé étrangère dans recipe_ingredient.
La clé primaire de recipe est liée à recipe_ingredient.
Vérifier la cohérence des relations et des dépendances.
### 6. Validation des données transformées
Objectif :
S’assurer que les données transformées sont prêtes à être chargées dans la base de données.

## Phase 3 : Chargement (Load)
Chargement des données transformées dans une base de données PostgreSQL.
Structure relationnelle pour faciliter les requêtes et l’intégration future.

Les formats des noms, dates, et unités sont uniformes.
* Tests unitaires :

Exécuter des scripts pour vérifier que les transformations donnent les résultats attendus.
* Prévisualisation des tables finales :

Générer un aperçu des tables (recipe, ingredient, recipe_ingredient) pour vérifier leur structure et leur contenu.
