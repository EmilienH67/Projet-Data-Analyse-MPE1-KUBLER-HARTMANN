# Projet-Data-Analyse-MPE1-KUBLER-HARTMANN: 


## I. Introduction + Contextualisation + Problématique: 

 En 2024, 12,4 millions de Français déclarent courir, ce qui représente 25 % de la population. Cette  hausse est intégralement portée par les marcheurs ou coureurs réguliers : + 7 points, de 25 % à 32 %. Cependant, on obsèrve depuis quelques années une hausse considérable des prix de chaussures de footing: 
 
 - Une Nike Alphafly a 310 €
 - Asics Gel-Nimbus a 200 €  

Paradoxalement, les chaussures sont des biens rapidement usables et fabriquées avec des matières plastiques. Ainsi, une solution économique et écologique sont des sites de reventes comme ebay.fr .


Ainsi, nous posons la problématique suivante:

**Comment être alerté en temps réel par des annonces écologique et pertinentes postées par des particuliers ?**


C'est dans ce contexte que s'inscrit notre projet de ce cours. Nous avons construit un prpgramme permettant de notifier un coureur par mail (gmail) lorsque le programme détecte une paire de chaussures correspondants au critères de selections définies au prélable: 
1) **Le prix** : Nous avons choisit une plage de prix entre 60 et 150 Euros. 
2) **La taille** :  44,5 en taille EU




## II. Les packages: 

-   **`blastula`** : Création et envoi d'emails avec des options avancées (personnalisation, inclusion d'images, etc.) → notifications par email.
-   **`keyring`** : Stocker en toute sécurité les informations sensibles comme les identifiants ou mots de passe (ici pour la configuration SMTP Gmail).
-   **`rvest`** : Facilite l'extraction de données d'un site web (web scraping). Utilisé pour récupérer les informations (nom, prix, lien) depuis les annonces eBay.
-   **`dplyr`** : Indispensable pour manipuler, nettoyer et transformer les données sous forme de tableaux.
-   **`cronR`** : Permet de planifier l'exécution de scripts R à des intervalles spécifiques (quotidien, horaire, hebdomadaire, etc.) sans quitter l'environnement R.


## III. Extraction et filtrage des données brutes depuis chaque URL.

1.  Lecture de la page HTML : "read_html(url)"charge le contenu HTML de l'URL.
2. Extraction des données : "html_nodes()identifie" les sections HTML pertinentes (titres, prix, liens).
3. Filtrage des données : Les résultats non pertinents contenant "Shop on eBay" sont exclus.
4. Formatage : Les résultats filtrés sont stockés dans un data frame pour une manipulation facile.


## IV. Application de la fonction et combinaison des résultats

-   "lapply()"applique la fonction "extract_filtered_data" à chaque URL de la liste.
-   Les résultats de chaque URL sont combinés avec "do.call(rbind, ...)", créant une table unique contenant toutes les annonces.

## V. Nettoyage et transformation des données

**Objectif** : Préparer les données pour analyse. - Extraction des prix numériques : Les caractères non numériques sont supprimés. - Remplacement des virgules par des points pour convertir les prix. - Filtrage des prix : Annonces dont le prix est inférieur à 60 EUR ou supérieur à 150 EUR sont exclues. - Sélection des colonnes finales : Seules les colonnes nécessaires (Name, Price, Price_num, Link) sont conservées.

## VI. Création d'un corps de message email et envoi de l'email

**Objectif** : Générer un texte contenant les annonces filtrées. Chaque ligne contient le nom, le prix et un lien cliquable vers l'annonce.
Détails :
Chaque ligne contient le nom, le prix et un lien cliquable vers l'annonce.
Le texte est formaté pour être lisible dans un email.


## VII. Création de l'email

compose_email : Crée un email en format HTML à partir du texte préparé.
Utilisation du Markdown : Le corps de l'email est enrichi en utilisant md() pour intégrer facilement des éléments comme des liens.


## VIII. Envoi de l'email

-   Envoi via le serveur SMTP de Gmail.
-   Paramètres : from, to, subject, et configuration sécurisée des identifiants via "keyrin".

## IX. Automatisation  du code



## X.Conclusion et usage 

Exécution du script R : Vous pouvez exécuter le script directement depuis RStudio ou en ligne de commande pour démarrer l'extraction des données. Le script se charge de tout : il va extraire les annonces de chaussures de sport sur eBay, les filtrer selon les critères définis, puis envoyer un email à l'adresse spécifiée.

Personnalisation des paramètres :

Changez l'URL dans la liste des URLs pour explorer d'autres pages eBay.
Mettez à jour la configuration SMTP dans le fichier pour personnaliser les paramètres d'email (comme votre adresse Gmail).

Exécution automatique : Vous pouvez programmer le script pour s'exécuter régulièrement en utilisant un planificateur de tâches sur votre système (par exemple, cron sur Linux ou Task Scheduler sur Windows).

## Contribution

Vous pouvez contribuer à ce projet en soumettant des pull requests, en signalant des bugs ou en proposant de nouvelles fonctionnalités. 
Toutes les contributions sont les bienvenues notamment pour intégrer Vinted chose que nous n'avons pas réussi à faire...


## Auteurs
Emilien Hartmann
Brice Kubler
