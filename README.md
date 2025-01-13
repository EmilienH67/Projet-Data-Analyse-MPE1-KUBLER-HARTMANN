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


##III. Utilisation

Exécution du script R : Vous pouvez exécuter le script directement depuis RStudio ou en ligne de commande pour démarrer l'extraction des données. Le script se charge de tout : il va extraire les annonces de chaussures de sport sur eBay, les filtrer selon les critères définis, puis envoyer un email à l'adresse spécifiée.

Personnalisation des paramètres :

Changez l'URL dans la liste des URLs pour explorer d'autres pages eBay.
Mettez à jour la configuration SMTP dans le fichier pour personnaliser les paramètres d'email (comme votre adresse Gmail).
Exécution automatique : Vous pouvez programmer le script pour s'exécuter régulièrement en utilisant un planificateur de tâches sur votre système (par exemple, cron sur Linux ou Task Scheduler sur Windows).


Exemple d'exécution :

# Exemple d'exécution pour extraire les annonces
urls <- c("https://www.ebay.fr/sch/i.html?_nkw=chaussures+de+course&_sop=12")
# Appliquez le scraping et filtrez les annonces
annonces <- lapply(urls, extract_filtered_data)
# Combinez les résultats
resultats <- do.call(rbind, annonces)
# Envoi d'un email avec les résultats filtrés
send_email(resultats)



