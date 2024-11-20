# Journal
## 11 novembre
J'ai tout d'abord commencé par suivre les vidéos sur YouTube pour apprendre les bases de QGIS. 
## 12 novembre - Mise en place du projet
J'ai poursuivi le travail en créant mon fichier QGIS pour le projet mais surtout en créant un espace bien organisé avec tous les dossiers nécessaires au projet. J'ai ensuite pu commencer en ajoutant les couches demandées à mon projet (zone trouvée sur moodle, carte nationale, images satelites nationales). Pour finir, j'ai créer le fichier "python ze-pt-ref.py" dans lequel j'ai définir ma zone d'étude.
## 18 novembre - Préparation des données
J'ai commncé par chercher les données suivantes:
### Localisation des pompiers
Ces données de l'oms ont été trouvées sur le site [Geofabrik](https://download.geofabrik.de/europe/switzerland.html).
### Données de population 2023
Ces données sont disponibles dans le [Géocatalogue de la confédération](https://www.bfs.admin.ch/bfs/fr/home/services/geostat/geodonnees-statistique-federale/batiments-logements-menages-personnes/population-menages-depuis-2010.html#geodaten_statpop__content_bfs_fr_home_dienstleistungen_geostat_geodaten-bundesstatistik_gebaeude-wohnungen-haushalte-personen_bevoelkerung-haushalte-ab-2010_jcr_content_par_tabs).
### Données sur les emplois 2022
Ces données sont également disponibles dans le [Géocatalogue de la confédération](https://www.bfs.admin.ch/bfs/fr/home/services/geostat/geodonnees-statistique-federale/etablissements-emplois/statistique-structurel-entreprises-statent-depuis-2011.html).
### Les limites des communes et des cantons
Ces données se trouvent dans jeu de données swissBOUNDARIES3D disponible sur le [site WEB de Swisstopo](https://www.swisstopo.admin.ch/de/landschaftsmodell-swissboundaries3d). Les données utilisées sont celles au format geopackage de janvier 2024.
### Le réseau routier et les surfaces d'eau
Ces données se trouvent dans jeu de données swissTLM3D disponible sur le [site WEB de Swisstopo](https://www.swisstopo.admin.ch/de/landschaftsmodell-swisstlm3d). Les données utilisées sont celles au format shape de mars 2024.
### Modèle numérique d'altitude
Le modèle swissALTIRegio est utilisé dans ce projet. Celui-ci est disponible sur le [site WEB de Swisstopo](https://www.swisstopo.admin.ch/de/hoehenmodell-swissaltiregio). Cependant, comme ce modèle est très volumineux, je travail avec un extrait du swissALTIRegio pour le canton de Vaud uniquement trouvé sur Moodle.
### Surfaces agricoles
Ces données sont fournies par l'administration cantonale vaudoise. Elles sont disponibles en payant via le [portail du canton de Vaud](https://viageo.ch/catalogue/donnee/201360#orderAction). Dans le cadre de l'enseignement, ces données m'ont été fournies gratuitement via Moodle.
## 18 novembre - Suite 
Une fois toutes ces données collectées, j'ai pu commencer à les préparer. 
### Données de population
J'ai commencé par les données de population. À l'aide de la commande se trouvant dans le script "ofs_geostat_gpkg_pop_2023.py" j'utilise ogr2ogr directement dans qgis pour sélectionner dans les données uniquement celles qui se trouvent dans ma zone de risque mais également pour en retenir que la population totale.
### Données sur les emplois
Ensuite, à l'aide de la commande se trouvant dans le script "ofs_geostat_gpkg_emp_2022.py" j'utilise de nouveau ogr2ogr directement dans qgis pour sélectionner, dans les données, uniquement celles qui se trouvent dans ma zone de risque mais également pour en retenir que le nombre d'employé total.
### Limites des communes et des cantons
J'ai créé,via qgis et à partir des données de swissBoundaries3D, le fichier geopackage "admin.gpkg" contenant deux couches au format shape. Une pour les limites des communes et l'autre pour les limites des cantons.
### Visualisation des données
Pour visualiser ce que je viens de faire, j'ai créer un nouveau projet qgis "zi_pop_empl.qgz" contenant ma zone industrielle ainsi que les autres couches céées (données de population et d'emplois et les communes et cantons). 
## 19 novembre Numérisation et extraction de données
### Numérisation du contour de la zone industrielle
Comme ma zone industrielle contient plus d'un polygône, j'ai créé une nouvelle couche "perimetre_ze_joint.shp" dans le même geopackage que ma zone d'étude d'origine. Cette nouvelle couche est une numérisation de ma zone industrielle mais en un seul polygône ctte fois ci.
### Importer les points d'intérêts du jeu de données OSM
J'ai ensuite charger la couche des points d'intérêts depuis le jeu de données OSM. J'ai sélectionné les stations pompiers pour les exportter dans le geopackage "pompiers.gpkg" dans la couche "pompiers". 
#### Sélections des stations dans le périmètre d'évaluation
J'ai ensuite créé la couche "perimetre_evaluation" dans le geopackage où se trouve également ma zone d'origine à l'aide du script python que j'ai utilisé pour le calcul de ma zone de risque. Cette couche a ensuite été utilisée pour supprimer les stations ne se trouvant pas dans le perimètre de la couche "pompiers".
#### Ajouter l'information sur la commune et le canton
J'ai créer la couche "pompiers_admin" à partir de la couche "pompiers" en faisant des jointure avec les couche des cantons et des communes pour que mes stations pompiers aient aussi les attribus suivants en fonction d'où elles se trouvents: numéro du canton, nom du canton, numéro de commune et nom de commune.
### Surfaces agricoles utiles
Pour sélectionner uniquement les surfaces agricoles utiles pour mone étude, j'ai créé le notebook "surfaces_agricoles.ipynb" sur Visual Studio Code.  Ce notebook prend les données des surfaces agricoles vaudoises (fichier csv et fichier gpkg) pour en faire resortir deux autres fichiers. Tout d'abord le fichier "resume_surfaces_agricoles.csv" qui contient deux colonnnes, une pour l'utilisation de la surface et l'autre pour la superficie en hectars. Le second fichier qui ressort est le fichier "surfaces_agricoles_utilisation.gpkg" dans lequel on trouve uniquement les surfaces agricoles se trouvant dans la zone concernée par mon travail. 