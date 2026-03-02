# 🚲 MiBici Analytics : 11 ans de Mobilité Urbaine à Guadalajara

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Dask](https://img.shields.io/badge/Big%20Data-Dask-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📖 À propos du projet
Ce projet analyse l'intégralité des données du système de vélos en libre-service de Guadalajara (**MiBici**) de son lancement en 2014 jusqu'à 2025.
L'objectif est double : démontrer une capacité d'ingénierie de données sur des volumes massifs (Big Data) et fournir des recommandations stratégiques basées sur l'analyse des flux de mobilité.

**Villes couvertes** : Guadalajara, Zapopan, Tlaquepaque (Aire métropolitaine : ~5M hab).

---
## Source de données
Les données brutes sont accessibles sur le portail MiBici Datos Abiertos. Pour assurer le bon fonctionnement des notebooks, les fichiers CSV doivent être organisés selon la structure suivante :

data/  
└── [YYYY]/  
    └── datos_abiertos_[YYYY]_[MM].csv

Contrairement aux données de trajets (trop volumineuses), le référentiel des stations enrichi est inclus dans ce dépôt (/data/nomenclatura_2025_11.csv).  
**Source initiale** : Nomenclature officielle MiBici (Version Nov. 2025).  
**Data Enrichment** : Le fichier source présentait des lacunes critiques (coordonnées GPS manquantes pour les stations récentes).  
**Traitement** : Géocodage manuel via Google Maps pour garantir une couverture à 100% des stations actives au 31/12/2025.  
**Usage** : Ce fichier sert de table de jointure unique pour toutes les analyses spatiales et la cartographie du réseau.  

## 📊 Le Projet en Chiffres Clés
* **Volume** : 34 437 707 trajets traités.
* **Distance totale** : 51,9 millions de km (1295 fois le tour de la Terre).
* **Utilisateur type** : Homme, 33 ans, trajet pendulaire de 11 minutes.
* **Topologie** : 382 stations, mais 2,6% des stations captent 12% du trafic.

---

## 🛠️ Stack Technique & Défis
Traiter 34 millions de lignes sur une machine locale a nécessité une approche **"Out-of-Core"** pour contourner les limitations de mémoire RAM.

* **Data Engineering** : `Dask` pour le calcul distribué et le partitionnement.
* **Stockage** : Format `Parquet` (compression Snappy) pour optimiser les I/O.
* **Optimisation** : Typage strict (`category`, `int32`) réduisant l'empreinte mémoire de 70%.
* **Analyse & Viz** : `Pandas`, `Seaborn`, `Matplotlib`.

---

## 💡 Insights Majeurs

### 1. Cycle de Vie & Saturation (2025)
Après l'impact du COVID-19 (-38% de volume), le service a retrouvé sa croissance jusqu'en 2024. **L'année 2025 marque un tournant** : baisse du volume (-1,9%) et de l'acquisition (-2,4%) alors que la durée des trajets stagne.
> **Diagnostic** : Le réseau a atteint un plafond de verre infrastructurel. La priorité n'est plus l'acquisition client, mais la capacité des stations.

### 2. Usage Pendulaire & Climat
Le service chute de **40% le week-end**, confirmant son usage utilitaire (domicile-travail).
Un phénomène saisonnier unique a été identifié : **"L'énigme du printemps"**. Durant les mois les plus chauds et arides (avril-mai), l'usage diurne chute au profit d'un report après 20h pour éviter la chaleur.

### 3. Sociologie des Usagers
* **Genre** : Forte prédominance masculine (73%). Les femmes (27%) décrochent significativement après 20h, soulignant un enjeu de sécurité urbaine. 
* **Fidélité** : 80% de rétention à 1 mois. Le service convertit massivement les essayeurs en cyclistes réguliers.

### 4. Goulots d'Étranglement Logistiques
L'analyse a identifié des **"Puits"** (stations qui saturent) et des **"Sources"** (stations qui se vident).
* **Cas critique** : À 18h, la Station 51 sature (+28 vélos/heure).
* **Découverte** : 12,7% de ce flux provient d'un unique cluster résidentiel (Stations 194-199). C'est un "entonnoir logistique" qui nécessite des mesures de régulation spécifiques.

---


