# Projet-Data-Analyse-MPE1-KUBLER-HARTMANN: 


## I. Introduction, Contextualisation et Problématique: 

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


## III. Extraction et filtrage des données brutes depuis chaque URL
1. Lecture de la page HTML

Dans cette première étape, nous utilisons la fonction read_html(url) pour charger le contenu d'une page web à partir de l'URL fournie. Cela permet de récupérer le code HTML de la page, qui est essentiellement un ensemble d'instructions et de balises définissant la structure de la page web (texte, images, liens, etc.).

2. Extraction des données

Une fois la page HTML lue, nous avons besoin d'identifier les parties spécifiques de cette page qui contiennent les informations que nous recherchons, telles que le nom des produits, les prix et les liens vers les annonces. C'est ce que nous faisons avec la fonction html_nodes(), qui permet de spécifier des "sélecteurs" (souvent des classes ou des balises HTML) correspondant aux éléments d'intérêt.

3. Filtrage des données

Après avoir extrait les informations de la page, nous devons nettoyer les données en supprimant celles qui ne sont pas pertinentes. Dans ce cas, nous excluons les résultats contenant des liens comme "Shop on eBay" qui ne sont pas des annonces pertinentes. Ce filtrage est effectué en identifiant des chaînes de texte spécifiques dans les données extraites (par exemple, les titres ou les liens) et en les excluant si elles correspondent à ces chaînes indésirables.

L'objectif ici est de ne conserver que les données qui nous intéressent, comme les annonces de chaussures de course postées par des particuliers, et de se débarrasser des éléments de la page qui ne sont pas utiles pour notre analyse.

4. Formatage

Une fois que nous avons filtré les données, nous les organisons dans un format plus facile à manipuler et à analyser, comme un tableau. Cela se fait en stockant les informations extraites (nom, prix, lien) dans un data frame, un objet de R très pratique pour travailler avec des données structurées.

Ce data frame permet de facilement manipuler les données pour les étapes suivantes, telles que l'analyse statistique, la transformation des prix en valeurs numériques, ou même l'envoi par email sous une forme lisible.

## IV. Application de la fonction et combinaison des résultats

1. Application de la fonction avec lapply()

La fonction lapply() est utilisée ici pour appliquer la fonction extract_filtered_data à chaque URL présente dans une liste. Une liste d'URLs peut contenir plusieurs liens pointant vers des pages de résultats (par exemple, différentes pages d'annonces de chaussures sur eBay).

Dans ce cas, lapply() parcourt chaque URL dans la liste, et pour chaque URL, elle exécute la fonction extract_filtered_data. Cette fonction est responsable de récupérer, nettoyer et organiser les données extraites pour chaque page.

Ainsi, si la liste contient plusieurs URLs, lapply() exécutera extract_filtered_data sur chaque page, une par une.

2. Combinaison des résultats avec do.call(rbind, ...)

Après que lapply() ait appliqué la fonction sur chaque URL et renvoyé les résultats sous forme de plusieurs tableaux (data frames), il faut maintenant combiner ces tableaux en un seul tableau. Cela est réalisé avec do.call(rbind, ...).

rbind() : Cette fonction permet de "lier" (bind) plusieurs tableaux de données (data frames) ensemble, en les empilant les uns sur les autres (ligne par ligne). Elle prend deux ou plusieurs data frames en entrée et les fusionne.
do.call() : Cette fonction permet d'exécuter une fonction avec une liste d'arguments. Dans ce cas, elle applique rbind() à une liste de data frames.
Ainsi, do.call(rbind, ...) prend tous les data frames générés par lapply() et les fusionne en un seul grand tableau contenant toutes les annonces extraites.

## V. Nettoyage et transformation des données

L'objectif ici est de préparer les données pour l'analyse en procédant aux étapes suivantes :

Extraction des prix numériques : On supprime tous les caractères non numériques pour ne garder que les chiffres.
Remplacement des virgules par des points : On remplace les virgules par des points pour que les prix soient reconnus sous format numérique.
Filtrage des prix : On exclut les annonces dont le prix est inférieur à 60 EUR ou supérieur à 150 EUR.
Sélection des colonnes finales : On conserve uniquement les colonnes nécessaires pour l'analyse : Name, Price, Price_num, et Link.
Cela permet d'avoir des données prêtes à l'emploi pour des analyses plus approfondies.

## VI. Création d'un corps de message email et envoi de l'email

L'objectif est de générer un texte formaté pour un email, contenant les annonces filtrées. Chaque ligne inclut :

Le nom de l'article,
Le prix de l'article,
Un lien cliquable vers l'annonce.
Le texte est structuré pour être lisible et facilement compréhensible dans un email.

## VII. Création de l'email

L'objectif est d'envoyer un email via le serveur SMTP de Gmail. Pour cela :

Les informations de l'email (expéditeur, destinataire, sujet) sont configurées.
La fonction smtp_send utilise une configuration sécurisée via "keyring" pour gérer les identifiants de manière sécurisée.


## VIII. Envoi de l'email

L'email est envoyé via le serveur SMTP de Gmail, avec les paramètres suivants :

from : adresse de l'expéditeur.
to : adresse du destinataire.
subject : sujet de l'email.
Configuration sécurisée : les identifiants sont gérés de manière sécurisée avec "keyring".

## IX. Automatisation  du code

Ce projet permet d'extraire des informations sur les chaussures de course à pied sur eBay, de les filtrer et de les envoyer par email chaque jour à 08:00 AM via une tâche cron programmée.

Prérequis
Packages R nécessaires : cronR, blastula, keyring, rvest, dplyr
Serveur SMTP configuré pour l'envoi d'emails
Automatisation
Le code configure une tâche cron qui exécute le script Chaussures_prix_taille.R tous les jours à 08:00 AM.

Définir la commande à exécuter : spécifiez le chemin du script.
Ajouter la tâche cron avec une fréquence quotidienne à 08:00.
Vérifier les tâches cron programmées.
Exécution manuelle du script

A noter: Il est aussi possible d'exécuter manuellement le script pour tester son bon fonctionnement.

## X.Conclusion et usage 

Il est désormais possible d'exécuter le script directement depuis RStudio ou en ligne de commande pour démarrer l'extraction des données. Le script se charge de tout : il va extraire les annonces de chaussures de sport sur eBay, les filtrer selon les critères définis, puis envoyer un email à l'adresse spécifiée.

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
