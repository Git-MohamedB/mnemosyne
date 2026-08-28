# Mnémosyne - Suivi des Cohortes

Application web de visualisation des parcours étudiants via diagrammes Sankey.

## Fonctionnalités

- **Diagramme Sankey** : Visualisation des flux étudiants BUT1 → BUT2 → BUT3 → Diplômé. Prise en compte du niveau de dette pour une granularité précise des passages.
- **Filtres Avancés** : Par formation (les 6 BUT + Tout l'IUT), année de promotion, et statut de diplomation.
- **Bilan de Compétences** : Analyse détaillée du ratio de compétences validées (ex: 5/6, 6/6) avec séparation des étudiants "Sans dette" et "Avec dette".
- **Exports Multiples** : Génération de rapports PDF complets avec charte graphique, et export des données brutes au format JSON pour l'administration.
- **Administration Sécurisée** : Importation et normalisation intelligente des fichiers ScoDoc (correction automatique des variantes de noms de formations et détection FI/FA).

## Installation

### Prérequis
- PHP 7.4+ avec extensions PDO, JSON, ZIP
- MySQL/MariaDB 5.7+
- Serveur web Apache/Nginx ou Docker

### Étapes

1. **Cloner le projet**
```bash
git clone <url_depot>
cd mnemosyne
```

2. **Configurer la base de données**
```bash
# Créer le fichier env.php depuis l'exemple
cp env.example.php env.php
# Éditer env.php avec vos identifiants BDD
```

3. **Importer le schéma SQL**
```sql
mysql -u <user> -p <database> < mnemosyne-projet_bdd.sql
```

4. **Créer un administrateur**
```sql
INSERT INTO admin (identifiant, mot_de_passe) 
VALUES ('admin', '$2y$10$...');  -- Hash bcrypt du mot de passe
```

5. **Synchroniser les données**
   - Placer les fichiers JSON ScoDoc dans `SAE_json/`
   - Connexion admin → Bouton "Synchroniser"

## Structure du Projet

L'application utilise une architecture MVC modulaire et sécurisée :

```
├── app/
│   ├── api/                # APIs JSON (Sankey, Bilan compétences)
│   ├── controllers/        # Contrôleurs métier (Admin, Auth)
│   ├── core/               # Cœur (Database Singleton, CSRF, Routeur)
│   ├── models/             # Accès BDD (AdminModel, FormationModel)
│   └── views/              # Vues PHP (admin, auth, consult)
├── css/                    # CSS Modulaire (components.css, styles.css)
├── docs/                   # Documentation et rapports
│   ├── DOCTECHNIQUE.md     # Documentation technique
│   ├── MANUEL_ADMIN.md     # Manuel d'utilisation administrateur
│   └── S4/                 # Rapports et livrables S4
├── import/                 # Scripts d'import et normalisation ScoDoc
├── SAE_json/               # Fichiers bruts d'import
├── tests/                  # Scripts de tests
├── index.php               # Point d'entrée principal (Consultation)
├── admin.php               # Point d'entrée d'administration
├── login.php               # Point d'entrée authentification
├── env.example.php         # Variables d'environnement (Git-ignored)
├── script.js               # Logique JavaScript (Sankey, Exports)
└── mnemosyne-projet_bdd.sql# Schéma Base de Données
```

## Sécurité

- Modèle MVC isolant la logique de la présentation
- Identifiants BDD protégés hors du dépôt (`env.php` ignoré via `.gitignore`)
- Authentification stricte par session PHP (Régénération d'ID)
- Protection CSRF (Jetons uniques) sur les formulaires
- Mots de passe hashés (bcrypt)
- Requêtes PDO préparées contre les injections SQL
- Données étudiantes anonymisées

## Cadre du Projet

Projet SAE - BUT Informatique
