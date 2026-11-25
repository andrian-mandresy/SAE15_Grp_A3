# SAE15_Grp_A3
SAE 15 - CARTOGRAPHIE INTERACTIVE DES ANTENNES 
#1. PRESENTATION DU PROJET

Dans le cadre de la SAE 15 a l'IUT, l'objectif de ce projet est d'analyser et
de visualiser les donnees ouvertes de l'ANFR (Agence Nationale des Frequences)
concernant les installations radioelectriques de plus de 5 watts en France.
Pour traiter ces fichiers volumineux et representer les antennes sur le
territoire francais, nous avons developpe une solution en Python generant une
carte satellite interactive fluide regroupant plus de 200 000 installations.

2. STRUCTURE DU CODE ET DES FICHIERS

Pour eviter que le programme ne mette plusieurs minutes a demarrer a chaque
lancement, nous avons separe notre code en trois etapes distinctes :

- installation_dependances.py (imporatation(1).ipynb) :
Permet d'installer automatiquement la bibliotheque folium sur
l'environnement Python de l'utilisateur.

- 1_preparation_donnees.py (carte-satellite(1).ipynb) :
* Charge les 7 fichiers .txt bruts de l'ANFR 
* Fusionne les tables, convertit les coordonnees GPS et nettoie les textes.
* Genere un fichier unique nomme "antenne_avec_coords.csv" qui sert de
sauvegarde propre (cache).
- 2_generation_carte.py (fichier-coordonée(1).ipynb) :

* Lit directement le fichier "antenne_avec_coords.csv" 
* Cree les fiches d'informations (popups) et genere la carte finale au
format "carte_antennes_integral.html".

3. DEMARCHE ALGORITHMIQUE

Face a un tres grand volume de donnees (plusieurs centaines de milliers de
lignes), une approche classique avec des boucles "for" faisait ralentir
l'ordinateur. Voici notre demarche en 4 etapes :

Etape A - Selection et jointures relationnelles (pd.merge) :

On ne garde que les colonnes utiles pour economiser la RAM
On relie les tables techniques (sup_support, sup_station, sup_antenne) avec
les tables de reference (sup_exploitant, sup_proprietaire, etc.) grace a
leurs identifiants communs (STA_NM_ANFR, ADM_ID, TPO_ID...).

Etape B - Conversion mathematique des coordonnees GPS :

Dans les fichiers de l'ANFR, les coordonnees sont en Degres, Minutes,
Secondes. Nous appliquons la formule suivante pour les convertir en
degres decimaux utilisables sur une carte :
Degres Decimaux = Degres + (Minutes / 60) + (Secondes / 3600)
On applique ensuite un signe negatif (-1) si la direction est Sud (S) ou
Ouest (W / O)

Etape C - Vectorisation (Zero boucle Python) :

Au lieu de parcourir les lignes une par une, on utilise la puissance de
Pandas pour creer les blocs HTML des popups sur toute la colonne d'un seul
coup.
Etape D - Optimisation de l'affichage (FastMarkerCluster) :

L'affichage de plus de 200 000 marqueurs fait saturer les navigateurs web
classiques. Nous utilisons le plugin FastMarkerCluster avec une fonction
JavaScript qui regroupe automatiquement les antennes par zones selon le
niveau de zoom.

4. EXTENSIONS ET BIBLIOTHEQUES NECESSAIRES

Pour executer ce projet, vous avez besoin de Python 3 ainsi que des
bibliotheques suivantes :
- pandas :
Indispensable pour lire les gros fichiers .txt, faire les fusions
de tables et manipuler les donnees rapidement.
- folium :
Permet de generer la carte interactive avec le fond satellite
(Esri World Imagery) et les clusters de points.
- sys (inclus dans Python) : 
Utilise pour executer l'installation des
modules directement depuis le code.
5. CONTRAINTES TECHNIQUES ET PREREQUIS POUR LES UTILISATEURS

Si vous n'etes pas membre du groupe et souhaitez tester ce code sur votre
machine, voici les elements indispensables au prealable :

- Fichiers de donnees requis au prealable :

Il est obligatoire de posseder sur son ordinateur le jeu de donnees complet
de l'ANFR (dossiers "data" et "ref"). Le programme a imperativement besoin
des fichiers des antennes et des supports (notamment "sup_antenne.txt",
"sup_support.txt", "sup_station.txt", ainsi que les tables de reference)
pour fonctionner.

- Chemins des fichiers locaux :

Dans le premier script, les chemins pointent actuellement vers un lecteur
reseau de l'IUT (Z:\SAE\SAE15\...). Il faut modifier les variables
"chemin_ref" et "chemin_data" pour indiquer l'emplacement exact ou vous
avez enregistre les dossiers de l'ANFR sur votre ordinateur.

- Fichiers lourds exclus de GitHub :

Les fichiers bruts de l'ANFR ainsi que le fichier genere
"antenne_avec_coords.csv" et la carte .html (qui pese plusieurs dizaines
de Mo) ne sont pas stockes sur le depot GitHub. Il faut absolument
lancer l'etape 1 (imporatation(1).ipynb) une premiere fois pour generer
le fichier CSV.

- Connexion Internet requise :

Lors de l'ouverture du fichier "carte_antennes_integral.html" dans votre
navigateur, une connexion Internet est necessaire pour charger les images
satellites depuis les serveurs d'Esri ainsi que les icones.

