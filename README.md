# Terre Vent Feu EAU DATA 
_Romuald Courtois, 15/09/2025_

# 1. Introduction ##

## 1.1 Contexte

L’été 2025 a marqué un tournant pour la France en matière d’incendies de forêt. Le 7 août, l’Aude a connu le plus grand incendie depuis 1949, détruisant 17 000 hectares en moins de 48 heures, mobilisant 2 100 pompiers et tous les moyens aériens nationaux, et causant un mort et 13 blessés. Cette catastrophe s’inscrit dans une tendance plus large : le mois de juillet 2025 a été marqué par une série d’incendies exceptionnels sur le territoire méditerranéen, confirmant les projections scientifiques qui prévoient une multiplication par trois de la période à risque d’ici 2050.

Depuis 2006, l’État français collecte et centralise des informations sur ces incendies à travers la BDIFF (Base de Données sur les Incendies de Forêts en France), hébergée par l’IGN. Cette base compile chaque année les données de plus de 9 000 feux avec une précision communale, offrant un potentiel considérable pour la prévention et la gestion du risque incendie.

## 1.2 Problématique

Malgré l’existence de la BDIFF, les données brutes ne permettent pas encore de prédire efficacement les risques d’incendie. Les défis sont nombreux :
- Fragmentation des données : les fichiers CSV sont disponibles par paquets de 30 000 lignes.
- Absence de géolocalisation précise : les feux sont codés par INSEE et nécessitent un 
  croisement avec d’autres jeux de données pour obtenir les coordonnées exactes.
- Complexité de l’analyse : il est nécessaire de traiter les données à différentes échelles     
  (spatiale, temporelle, saisonnière) pour anticiper les zones et périodes à risque.

Ces contraintes techniques, combinées au fait que 90 % des départs de feu sont d’origine humaine, rendent indispensable le développement d’outils d’intelligence prédictive pour améliorer la prévention et réduire l’impact des incendies.

## 1.3 Objectifs

Le projet vise à exploiter la BDIFF pour développer un outil prédictif du risque d’incendie. Plus spécifiquement, il s’agit de :

1. Reconstituer un dataset unifié à partir des fichiers historiques de la BDIFF (1973–2024).

2. Enrichir les données avec les coordonnées géographiques exactes des communes.

3. Préparer les données pour des analyses multi-échelles (spatiales, temporelles, saisonnières) 
   permettant d’anticiper le risque incendie.

4. Poser les bases d’une version future intégrant des données météorologiques (sécheresse, 
   vent, température…) pour affiner la prédiction et renforcer la prévention.

Ainsi, le projet constitue une première étape vers une gestion proactive et scientifique du risque incendie en France.

# 2. Données ou échantillons

Les données utilisées proviennent de la **Base de Données sur les Incendies de Forêts en France (BDIFF)**. Cette base nationale, créée en 2006, est gérée par le Ministère de l’Agriculture et hébergée par l’IGN. Elle est mise à jour chaque année et couvre actuellement la période **1973–2024**.

## 2.1 Accès aux données
Les fichiers peuvent être téléchargés depuis le site officiel : [BDIFF – Incendies](https://bdiff.agriculture.gouv.fr/incendies)  

Chaque fichier ZIP contient :

- **Définitions 1973.pdf** : définitions des termes utilisés dans les fiches incendies de la 
                             BDIFF.  
- **Incendies.csv** : Fichier ou chaque ligne correspond à un incendie, avec des 
                      caractéristiques détaillées du feu.  
- **Mentions légales.pdf** : informations légales relatives à l’usage des données.

## 2.2 Composition des données 

- *Année* : année du feu  
- *Numéro* : code unique attribué au feu  
- *Département* : numéro du département où le feu s’est déclaré  
- *Code INSEE* : code de la commune servant de localisation pour le feu  
- *Nom de la commune* : nom de la commune correspondant au code INSEE  
- *Date de première alerte* : date et heure de la première alerte, au format `jj/mm/aaaa 
                              hh:mm`  
- *Surfaces brûlées (en m²)*
- *Surface parcourue* : Surface totale affectée par le feu.
- *Surface forêt* : Surface située uniquement en forêt (taux de couvert arboré > 10 %)  
                    - Critères supplémentaires :  
                         - Surface d’un seul tenant ≥ 0,5 ha  
                         - Largeur ≥ 20 m  
- *Surface maquis garrigues* : - Surface parcourue dans les zones de maquis ou de garrigue
                                 (uniquement pour les départements méditerranéens).
- *Autres surfaces naturelles* : Zones naturelles non forestières ou autres terres boisées :  
                                    - Taux de couvert arboré < 10 %  
                                    - Superficie < 0,5 ha (bosquets)  
                                    - Largeur < 20 m (cordons boisés, haies)  
                                    - Landes ligneuses ou herbacées, prairies naturelles, zones 
                                      humides  

- *Surfaces agricoles* : Terrains agricoles touchés : cultures pérennes ou non, prairies
                         temporaires, vergers, agroforesterie, etc.  

- *Surfaces artificialisées* : Terrains artificiels affectés : jardins, pelouses, parcs 
                               urbains, friches, talus.  
                                - Pour les départements méditerranéens, maquis et garrigues 
                                  sont exclus de la définition de forêt et peuvent être précisés par type de peuplement.  
- *Précision des surfaces* : 
    - *Estimées* : surfaces renseignées sans mesures directes  
    - *Mesurées* : surfaces issues de mesures terrain ou SIG (Système d’Information 
                   Géographique)
- *Type de peuplement* : Indique la végétation majoritaire touchée par le feu.  
                         - Valeurs possibles :  
                            - 1 = Landes, garrigues, maquis
                            - 2 = Taillis  
                            - 3 = Futaies feuillues  
                            - 4 = Futaies résineuses  
                            - 5 = Futaies mélangées  
                            - 6 = Régénération et reboisement (naturelle ou artificielle)

- *Nature* : Origine présumée du feu
             - Précise l’origine de l’incendie lorsqu’elle est connue :  
                - *Naturelle* : feu non causé par l’homme (ex. foudre)  
                - *Accidentelle* : lié indirectement à une activité humaine (ex. rupture de 
                                   ligne électrique, véhicule)  
                - *Malveillance* : incendie volontaire  
                - *Involontaire (travaux)* : lié à des activités professionnelles (travaux 
                                             forestiers, agricoles, construction)  
                - *Involontaire (particulier)* : lié à des activités privées ou de loisirs 
                                                 sans intention d’incendier (bricolage, travaux domestiques)
- *Nombre de décès* : personnes décédées pendant l’incendie ou des suites directes de celui-ci.
- *Nombre de bâtiments totalement détruits* : structures en dur entièrement détruites (toiture 
                                              perdue, travaux nécessaires pour réutilisation).  
- *Nombre de bâtiments partiellement détruits* : bâtiments en dur endommagés dont la toiture 
                                                 reste intacte, seule une partie de la structure est touchée.
- *Précision de la donnée* : niveau de qualité et fiabilité de l’enregistrement.

## 2.3 Chronologie
Au fur et a mesure, les informations et la labelisations des données ont évoluées :

**1973 – 2005 : Les premiers enregistrements**
- Les données existent mais ne sont pas centralisées.
- Les fiches incendies sont hétérogènes selon les départements.
- Définitions de base déjà présentes : *Année;Numéro;Département;Code INSEE;Nom de la commune; 
                                        Date de première alerte;Surface parcourue (m2);Type de peuplement;Nature(partiellement)*.
- Pas encore de base nationale unifiée.
- Seule les données du sud de la France et de la Corse sont receuillis

**2006 : Création de la BDIFF**
- Mise en place de la Base de Données sur les Incendies de Forêts en France (BDIFF).
- Gestion assurée par le Ministère de l’Agriculture, hébergement par l’IGN.
- Pour la première fois, les données deviennent nationales, centralisées et standardisées.
Enregistrement systématique des informations principales :
- Identifiants (année, code, commune)
- Localisation (code INSEE, département)
- Chronologie (date de première alerte)
- Surfaces parcourues (forêt, autres terres boisées, non boisées naturelles, non boisées 
  artificielles)
- Caractéristiques (type de peuplement, origine du feu)
- Impacts (décès, bâtiments détruits ou endommagés).

**2023 – 2024 : Nouvelle nomenclature des surfaces**
Réorganisation profonde des catégories de surfaces :
- Distinction explicite :
        - Forêt
        - Autres surfaces naturelles hors forêt
        - Surfaces agricoles
        - Autres surfaces (friches, talus, zones artificialisées comme jardins/parcs)
- Dans l’aire méditerranéenne, maquis/garrigue deviennent une catégorie à part entière, exclus 
  de la définition de « forêt ».
- Précision des surfaces : distinction entre estimées (approximations) et mesurées (terrain ou 
  SIG).
- Maintien et précision du type de peuplement et de l’origine du feu.

Objectif : rendre les données plus comparables, plus exploitables et conformes aux standards internationaux de suivi des incendies.

# 3. Outils et Technologies ##

1. Gestion du projet et développement
- GitHub : gestion de version et collaboration sur le code source.
- Jupyter Notebook / VS Code : environnements de développement interactifs et performants pour 
  coder, tester et documenter le projet.
- Google Colab : exécution du code sur GPU/CPU dans le cloud, utile pour les modèles lourds ou 
  le prototypage rapide.

2. Stockage et gestion des données
- NEON : base de données SQL centralisant les informations sur les incendies de forêt et les 
  données associées.
- Pandas / NumPy : manipulation et analyse de données tabulaires extraites de NEON ou d’autres 
  sources.

3. Modèles de machine learning
- Scikit-learn : modèles classiques pour débuter, incluant la régression, les arbres de 
  décision et les forêts aléatoires.
- XGBoost / LightGBM / CatBoost : modèles de gradient boosting performants pour les données 
  tabulaires.
- TensorFlow / PyTorch : frameworks pour les réseaux neuronaux et modèles complexes.
- HDBSCAN : clustering hiérarchique basé sur la densité, utile pour l’exploration des données 
  ou la détection de zones à risque.

4. Visualisation et interface utilisateur
- Streamlit : création d’applications web interactives pour présenter les résultats et 
  prédictions.

# 4. Méthodes 

## 4.1 Description détaillée de la démarche 

Le projet consiste à développer un modèle d’intelligence artificielle capable de prédire le risque d’incendie de forêt à l’échelle communale en France. La démarche adoptée se déroule en plusieurs étapes :

1. Collecte et préparation des données : extraction des données historiques d’incendies depuis 
   la BDIFF, stockage et requêtes via la base NEON.

**Mise en place**
Les données mis a disposition dans un .csv ont été téléchargé par paquet de 10 ans :
- 1973-1979
- 1980-1989
- 1990-1999
- 2000-2009
- 2010-2019
- 2019-2024

Au total, 140 247 feu ont été répertorié.

Durant ce traitement de données, et notament en raison du temps impartie et de la disponibilité des données, nous avons exlus les colonnes : 
- Surface forêt (m2);Surface maquis garrigues (m2);Autres surfaces naturelles hors forêt (m2);Surfaces agricoles (m2);Autres surfaces (m2);Surface autres terres boisées (m2);Surfaces non boisées naturelles (m2);Surfaces non boisées artificialisées (m2);Surfaces non boisées (m2);Précision des surfaces;Décès ou bâtiments touchés;Nombre de décès;Nombre de bâtiments totalement détruits;Nombre de bâtiments partiellement détruits.

La raison de cette exclusion sont :
- Peu de données dans ces colones (moins de 10% remplies) / Aucune avant 2006
- Ne réfère pas directement à une intensité/ou une violence directe de l'incendie/n'apporte que peu d'information supplémentaire. Némoins, ces données pourront être réintégré facilement par la suite. 

Voici les inforamtions sur les valeurs manquantes des colonnes gardés : 
| Colonnes                | Nombres | Pourcentages|
|-------------------------|---------|-------------|
| annee                   |       0 |    0.000000 |
| numero                  |       0 |    0.000000 |
| code_insee              |       0 |    0.000000 |
| nom_commune             |      27 |    0.019252 |
| date_premiere_alerte    |       0 |    0.000000 |
| surface_parcourue_m2    |       0 |    0.000000 |
| type_de_peuplement      |  62 732 |   44.729656 |
| precision_de_la_donnee  | 139 033 |   99.134384 |

**annee**
La donnée année nous permet de savoir l'année du feu et de trier les inforamtions rapidements.

**numero**
Le numero du feu est unique, il nous permet de voir les doublons mais aussi de vérifier si 2 feu n'ont pas le même code.

**code_insee**
Le code INSEE est un code unique apporté à chaque commune. Il codifie les communes sur la base du Code Officiel Géographique (COG) et est donc unique pour chaque commune.

**Nom de commune**
Les noms de communes manquant sont du à la fusion ou la modification de communes.
Au total depuis 1973 : environ 4 500 à 4 600 communes françaises ont été concernées par une fusion.

Pour compléter les noms de communes manquant mais présentant un code INSEE, nous avons télécharger un fichier .csv sur le site du gouvernenement composé des informations sur 35 141 communes.

## Fichiers Communes de France 20025
Le fichiers .csv des communes de France comportes les informations :
- code_insee
- nom_standard
- nom_sans_pronom
- nom_a,nom_de
- nom_sans_accent
- nom_standard_majuscule
- typecom
- typecom_texte
- reg_code
- reg_nom
- dep_code
- dep_nom
- canton_code
- canton_nom
- epci_code
- epci_nom
- academie_code
- academie_nom
- code_postal
- codes_postaux
- zone_emploi
- code_insee_centre_zone_emploi
- code_unite_urbaine
- nom_unite_urbaine
- taille_unite_urbaine
- type_commune_unite_urbaine
- statut_commune_unite_urbaine
- population
- superficie_hectare
- superficie_km2
- densite
- altitude_moyenne
- altitude_minimale
- altitude_maximale
- latitude_mairie
- longitude_mairie
- latitude_centre
- longitude_centre
- grille_densite
- grille_densite_texte
- niveau_equipements_services
- niveau_equipements_services_texte
- gentile
- url_wikipedia
- url_villedereve

Durant ce traitement de données, et notament en raison du temps impartie, nous avons exlus les colonnes : 
nom_sans_pronom;nom_a,nom_de;nom_sans_accent;nom_standard_majuscule;typecom;typecom_texte;reg_code;reg_nom;dep_code;dep_nom;canton_code;canton_nom;epci_code;epci_nom;academie_code;academie_nom;code_postal;codes_postaux;zone_emploi;code_insee_centre_zone_emploi;code_unite_urbaine;nom_unite_urbaine;taille_unite_urbaine;type_commune_unite_urbaine;statut_commune_unite_urbaine;superficie_hectare;altitude_moyenne;altitude_minimale;altitude_maximale;latitude_mairie;longitude_mairie;grille_densite;grille_densite_texte;niveau_equipements_services;niveau_equipements_services_texte;gentile;url_wikipedia;url_villedereve

Voici les inforamtions sur les valeurs manquantes des colonnes gardés : 
| Colonnes                | Nombres | Pourcentages|
|-------------------------|---------|-------------|
| code_insee              |       0 |    0.000000 |
| nom_standard            |     206 |    0.586210 |
| population              |     206 |    0.586210 |
| superficie_km2          |     206 |    0.586210 |
| densite                 |     209 |    0.594747 |
| latitude_centre         |     215 |    0.611821 |
| longitude_centre        |     215 |   44.611821 |

2. Enrichissement des données : géocodage des communes pour obtenir des coordonnées précises et 
   intégration de variables météorologiques (température, vent, précipitations, humidité) pour améliorer la qualité prédictive.

Lors de l'enrichissement des données incendies grace à l'ajout des coordonnées de latitude/longitude, plusieurs code_insee et villes sont ressorties sans concordances pour combler les données manquantes.

Certains codes INSEE n'était pas à jour, voici la liste des code INSEE mis à jour :
     Avant :  Après
-------------------------
    '05067': '05132',
    '05141': '05039',
    '05020': '50419',
    '48049': '48099',
    '48162': '48166',
    '05034': '05118',
    '48022': '48050',
    '48183': '48009',
    '48040': '48027',
    '48195': '48094',
    '48189': '48127',
    '48076': '48009',
    '48197': '48127',
    '48120': '48087',
    '48184': '48038',
    '48154': '48094',
    '27676': '27676',
    '07256': '07165'

Puis certaines coordonnées manquaient : 
    "Ville" : (Latitude,Longitude)
--------------------------------------------------
    "Marseille": (43.296398, 5.370000),
    "Culey": (48.75, 5.26667),
    "Les Hauts-Talican": (49.3294030, 2.0038440),
    "Lyon": (45.763420, 4.834277),
    "Paris": (48.864716, 2.349014),
    "Bihorel": (49.4547, 1.11611),
    "Saint-Lucien": (48.649, 1.626),
    "L'Oie": (46.7980, -1.1295),
    "Sainte-Florence": (46.796, 1.149 ),

Pour faciliter le traitement, la date et l'heure a été remplacé par le timestamp.

Une fois le fichiers mis à jour et complété, une analyse à permis de mettre en évidence que 2,30% des données pouvaient être présenté comme des doublons.

Le doublon est défini si les données suivantes sont identiques:
- code_insee
- date_jour (basé sur le timestamps)
- surface_parcouru_m2

Suite à l'exploration des données, un même feu est souvant noté plusieurs fois, à des horaires similaires ou différents mais toujours le même jours. Cela peut être du à l'enregistrement par des opérateurs différents dans la base de données. Il ne parait pas utile de les laisser car elle ne présente aucune inforamtions supplémentaires sur la gravité du feu, son origine, sa propagation, etc... 

=== INFORMATIONS DES DOUBLONS ===
Lignes initiales    : 140,247
Lignes finales      : 137,028
Lignes supprimées   : 3,219
Pourcentage supprimé : 2.30%

Enfin, nous avons pu noté pour chaque ligne de feu. Cependant, 680 feu n'ont pas de localisation. Faute de temps, je n'ai pas pu explorer plus et comprendre pourquoi ces communes n'avait pas de localisation.

3. Exploration et analyse des données : identification des tendances spatiales et temporelles 
   détection de clusters ou zones à risque grâce à HDBSCAN, et traitement des valeurs aberrantes.

4. Entraînement et validation des modèles : application de modèles supervisés (Random Forest, 
   XGBoost, LightGBM) et, si pertinent, de réseaux neuronaux (TensorFlow / PyTorch). Les hyperparamètres sont optimisés à l’aide de validations croisées et de métriques adaptées (accuracy, F1-score, ROC-AUC).

5. Interface et visualisation : développement d’une interface interactive avec Streamlit 
   permettant de visualiser les prédictions de risque d’incendie par commune.

- Protocoles expérimentaux ou algorithmiques
  Séparation des données : le dataset est divisé en ensembles train (70 %), validation (15 %), et test (15 %).

- Normalisation et encodage : certaines variables quantitatives sont normalisées, et les 
  variables catégorielles (ex. type de végétation ou causes humaines) sont encodées.

- Entraînement : les modèles supervisés sont entraînés sur l’ensemble train, puis évalués sur 
  l’ensemble validation pour ajuster les hyperparamètres.

- Évaluation finale : performance mesurée sur l’ensemble test, pour éviter tout 
  surapprentissage et garantir la généralisabilité du modèle.

# 5. Plan d’analyse 

## 5.1 Méthodes statistiques  
- **Descriptives** : distribution des surfaces brûlées, fréquences par type de végétation et 
                     origine.  
- **Série temporelle** : tendances saisonnières et décennales.  
- **Spatiales** : cartographie des zones à risque (clusters via HDBSCAN).  
- **Corrélations** : entre surfaces, type de peuplement, conditions climatiques (à venir).  

## 5.2 Visualisations prévues  
- **Cartes interactives** : densité d’incendies par commune.  
- **Courbes temporelles** : évolution annuelle et saisonnière.  
- **Histogrammes** : répartition par type de peuplement.  
- **Matrices de corrélation** : entre variables explicatives et surfaces brûlées.  

# 6. Fiabilité et limites  

- **Biais temporel** : avant 2006, données incomplètes et hétérogènes.  
- **Biais de déclaration** : certaines communes enregistrent mieux que d’autres.  
- **Colonnes exclues** : faute de temps, certaines variables (surfaces détaillées, impacts) ont 
                         été écartées.  
- **Limites méthodologiques** : modèle encore limité à la BDIFF, sans intégration 
                                météorologique.  

# 7. Conclusion  

- **Méthodes** : préparation, enrichissement, modélisation IA.  
- **Points forts** : dataset unifié (1973–2024), nettoyage rigoureux, pipeline reproductible.  
- **Limites** : manque de variables exogènes (climat), précision inégale avant 2006.  
- **Perspectives** :  
  - Intégrer données climatiques en temps réel (Météo-France).  
  - Tester des modèles de deep learning (RNN, LSTM) pour les séries temporelles.  
  - Développer un outil prédictif opérationnel pour les collectivités. 


# 8. Références - Articles, livres, normes, sites web ##
- BDIFF : https://bdiff.agriculture.gouv.fr/aide/generalites
- DATA.GOUV : https://www.data.gouv.fr/datasets
- GeoPandas : https://geopandas.org/en/stable/gallery/index.html
- INSEE : https://www.insee.fr/fr/statistiques
- Streamlit : https://docs.streamlit.io/
- Rapport Assemblée nationale N°2310 – réforme des collectivités locales : https://www2.assemblee-nationale.fr/documents/notice/14/rapports/r2310/%28index%29/dyn/info-site?utm_source=chatgpt.com
- Vie-publique.fr – Qu’est-ce qu’une commune nouvelle ? : https://www.vie-publique.fr/fiches/20184-quest-ce-quune-commune-nouvelle?utm_source=chatgpt.com


