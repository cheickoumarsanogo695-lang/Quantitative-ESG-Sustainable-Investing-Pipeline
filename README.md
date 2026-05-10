# Quantitative-ESG-Sustainable-Investing-Pipeline

## Présentation du projet 

Ce projet vise à construire une infrastructure de recherche ESG de bout en bout combinant :

- intelligence documentaire ESG,
- data engineering,
- extraction de données,
- scoring ESG,
- et méthodes quantitatives appliquées à l’investissement durable.

L’objectif final est de développer un pipeline capable de transformer des documents ESG bruts en signaux exploitables pour :

- l’analyse extra-financière,
- la construction d’indicateurs ESG,
- et la construction de portefeuilles durables robustes.

## Première étape du projet : Pipeline robuste de collecte documentaire ESG

La première phase du projet consiste à construire une architecture robuste permettant :

- la discovery automatisée de documents ESG ;
- la collecte multi-source ;
- la validation et la normalisation des PDFs ;
- le scoring documentaire ;
- la sélection robuste des meilleurs documents ;
- puis leur ingestion dans une base documentaire structurée.

Le pipeline a été conçu avec une logique :

- modulaire ;
- extensible ;
- reproductible ;
- et proche des architectures de data engineering utilisées dans des workflows ESG industriels.
  


                ┌────────────────────┐
                │  Liste entreprises │
                └─────────┬──────────┘
                          │
                          
              ┌──────────────────────┐
              │ Normalisation émetteurs │
              └─────────┬────────────┘
                        │
                        ▼
          ┌─────────────────────────────┐
          │ Discovery documentaire ESG │
          └─────────┬──────────────────┘
                    │
     ┌──────────────┴──────────────┐
     ▼                             ▼
Corporate crawling           Recherche DDGS
(sites officiels)            (web complémentaire)

     └──────────────┬──────────────┘
                    ▼
         ┌────────────────────────┐
         │ Construction PDF Index │
         └──────────┬─────────────┘
                    ▼
         ┌────────────────────────┐
         │ Validation des PDFs    │
         └──────────┬─────────────┘
                    ▼
         ┌────────────────────────┐
         │ Scoring documentaire   │
         └──────────┬─────────────┘
                    ▼
         ┌────────────────────────┐
         │ Sélection robuste      │
         └──────────┬─────────────┘
                    ▼
         ┌────────────────────────┐
         │ Ingestion finale ESG   │
         └────────────────────────┘


### Fonctionnalités implémentées
#### 1. Gestion multi-source des documents ESG

Le pipeline accepte plusieurs modes d’entrée :

- noms d’entreprises ;
- URLs PDF fournies manuellement ;
- fichiers PDF locaux.

Cette flexibilité permet de compléter la collecte automatique lorsque certains documents sont difficiles à détecter.

#### 2. Discovery documentaire hybride

Le système combine :

- exploration directe des sites corporate ;
- et recherche web complémentaire via DuckDuckGo Search (DDGS).

Cette approche améliore significativement :

- la couverture documentaire ;
- la robustesse ;
- et la résilience du pipeline face aux architectures web hétérogènes.
  

             ┌─────────────────────┐
             │ Référentiel ESG Docs │
             └──────────┬──────────┘
                        ▼
             Génération des requêtes
                        │
                        ▼
        ┌────────────────────────────┐
        │ Discovery documentaire ESG │
        └──────────┬─────────────────┘
                   │
      ┌────────────┴────────────┐
      ▼                         ▼
 Exploration corporate      Recherche DDGS
  Pages ESG / IR           Recherche web externe

      └────────────┬────────────┘
                   ▼
          URLs PDF candidates



#### 3. Validation robuste des PDFs

Chaque document détecté est :

- validé techniquement ;
- contrôlé ;
- dédupliqué via SHA-256 ;
- puis normalisé avant ingestion.

Le pipeline gère notamment :

- les erreurs réseau ;
- les timeouts ;
- les faux PDFs ;
- les fichiers corrompus ;
- et les doublons documentaires.

#### 4. Construction d’un index documentaire intermédiaire

Tous les PDFs candidats sont centralisés dans un pdf_index intermédiaire.

Cet index joue plusieurs rôles :

- centralisation des candidats documentaires ;
- suppression des doublons ;
- séparation entre discovery et scoring ;
- réduction des appels réseau ;
- auditabilité de la collecte.

Cette architecture facilite également :

- le debugging ;
- les relances partielles ;
- et l’amélioration future des modèles de scoring.
  

Discovery multi-source
        │
        ▼
┌────────────────────┐
│   PDF INDEX ESG    │
├────────────────────┤
│ URL candidate      │
│ Source page        │
│ Anchor text        │
│ PDF validation     │
│ Company metadata   │
│ Retrieval date     │
└────────────────────┘
        │
        ▼
Scoring documentaire


#### 5. Scoring documentaire ESG

Chaque document candidat est évalué automatiquement selon :

- le nom de l’entreprise ;
- l’année fiscale ;
- les mots-clés ESG ;
- le type documentaire attendu ;
- la cohérence avec la requête cible.

Le pipeline produit :

- un score documentaire ;
- une probabilité estimée ;
- un niveau de confiance ;
- ainsi que les raisons du scoring.

#### 6. Sélection robuste des documents

Le pipeline applique ensuite une logique de sélection robuste afin de distinguer :

- les documents automatiquement sélectionnés ;
- les documents nécessitant une revue manuelle ;
- les documents probablement absents.

Cette étape permet de réduire les faux positifs tout en maintenant une couverture documentaire élevée.

## Stratégie de collecte progressive

Le pipeline implémente une logique de collecte en deux niveaux :

### Run principal

Collecte des documents ESG prioritaires :

- rapports annuels ;
- sustainability statements ;
- rapports climat ;
- plans de vigilance ;
- rapports d’assurance.
- Run secondaire ciblé

### Collecte complémentaire activée uniquement lorsque la couverture documentaire principale est insuffisante :

- politiques ESG ;
- codes de conduite ;
- CDP ;
- SBTi ;
- présentations investisseurs ;
- rapports semestriels.

Cette approche permet :

- d’optimiser les coûts de collecte ;
- de limiter le bruit documentaire ;
- et de concentrer les ressources sur les cas réellement utiles.

Run principal ESG
                │
                ▼
     Analyse couverture ESG
                │
     ┌──────────┴──────────┐
     ▼                     ▼
Couverture bonne     Couverture insuffisante
     │                     │
     ▼                     ▼
 Stop collecte       Run secondaire ciblé


















