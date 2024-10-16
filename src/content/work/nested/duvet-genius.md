---
title: SatiSymfony
publishDate: 2023-05-07 00:00:00
img: /assets/SatiSymfony/satisfactory_cover.jpeg
img_alt: Image du jeu Satisfactory
description: |
  Cette outil de calcul permet d'optimiser la gestion des usines que je construis dans le jeu vidéo Satisfactory.
tags:
  - Design
  - Dev
  - Back-End
  - Video games
---

Au cours de l'année 2023, j'ai beaucoup joué au jeu vidéo Satisfactory. C'est un jeu de construction d'usines en monde ouvert à la première personne, avec une touche d'exploration et de combat.

J'ai créer ce projet dans l'optique de ne pas avoir à me prendre la tête avec les calculs simple mais surtout redondant du jeu.
Dans ce jeu, vous pouvez récupérer des ressources brutes et les transformer en de nouvelles ressources.

Exemple ci-dessous (les images en dessous ne sont pas présentes sur le site) :  


Image 1

![alt text](/assets/SatiSymfony/lingot-de-cuivre.png)

Une fois un lingot de cuivre obtenue, on peut ensuite le re-transformer !

Exemple ci-dessous (les images en dessous ne sont pas présentes sur le site) :  


Image 2  

![alt text](/assets/SatiSymfony/fil-electrique.png)
Pour effectuer ces calculs, j'ai d'abord établi un cahier des charges afin de déterminer les ressources nécessaires à la création de mon site. 
Ce processus s'est réparti en quatre étapes :

- Première étape : création de la base de données
- Deuxième étape : création des formulaires
- Troisième étape : résultats des formulaires sous version simplifiés
- Quatrième étape : résultats des formulaires sous version détaillés 

Première étape :

Comme mentionné précédemment, j'ai dû commencer par créer une base de données. Celle-ci se compose de trois tables :  

Image 3  

![alt text](/assets/SatiSymfony/diagramBDD.png)

On a donc les ressources qui sont tous les matériaux que le joueur peut récolter sur la carte du jeu.
Ensuite, on a les bâtiments qui sont utilisés par le joueur afin de créer de nouvelles ressources (voir image 1 et 2.)
Pour finir, on a les recettes, quand le joueur créer un nouvel objet grâce à un bâtiment, il a besoin d'une recette pour savoir comment le créer. (Voir image 2)

Deuxième étape :

Avant de créer les formulaires, j'ai élaboré une commande en Symfony qui permet d'ajouter toutes les ressources et bâtiments présents dans le jeu à l'aide d'un fichier .json. 
J'ai trouvé en ligne un fichier .json créé par un membre de la communauté de Satisfactory, qui répertorie toutes les ressources et bâtiments. Grâce à cette commande, 
j'ai pu insérer directement les valeurs dans les tables 'ressources' et 'bâtiments' de la base de données, évitant ainsi de les ajouter manuellement une par une.  
J'ai également rédigé une documentation pour permettre à tout utilisateur d'utiliser cette commande sans avoir besoin de comprendre le code sous-jacent.

Une fois la base de données remplie, j'ai entrepris la création des formulaires, offrant ainsi à l'utilisateur la possibilité de créer de nouvelles ressources, 
de modifier celles déjà existantes et de les supprimer facilement.  


Image 4  

![alt text](/assets/SatiSymfony/formulaireRessources.png)  

Image 5  

![alt text](/assets/SatiSymfony/editRessources.png)  

Image 6  

![alt text](/assets/SatiSymfony/createRessources.png)


Les trois images ci-dessus illustrent les formulaires utilisés pour les ressources, tandis que ceux des bâtiments sont assez similaires. 
Cependant, ma base de données se compose de trois tables, dont la dernière intègre un formulaire un peu plus complexe.

Une recette se compose de la sorte :

Une ressource en entrée génère une ressource en sortie. (Voir images 1 et 2)
En prenant la première image comme exemple, on constate qu'un minerai de cuivre placé dans une fonderie produit un lingot de cuivre.

Voici à quoi ressemble le formulaire de recette :  

Image 7  

![alt text](/assets/SatiSymfony/createRecipe.png)  

Celui est un peu plus complexe, on peut voir que le formulaire comporte :

- Nom de la recette
- Key name (son id unique)
- Le nombre de ressources en entrée
- Le nom de la ressource en entrée
- Le nombre de ressources en sortie
- Le nom de la ressource en sortie
- Le bâtiment utilisé
- Le temps qu'un objet met à être produit 

Une fois la recette crée, mon application fait les calculs afin de trouver les ressources en entrée et en sortie nécessaire en 1 min.  
Le dernier paramètre est très important, si on reprend l'exemple de l'image 1 :  

1 minerai de cuivre se transforme en 1 lingot de cuivre en 2 secondes.  

L'application effectue ainsi le calcul du nombre de ressources demandées en entrée et en sortie sur une période d'une minute, ce qui donne :

30 minerai de cuivre -> 30 lingot de cuivre en 1 minute.

Image 8  

![alt text](/assets/SatiSymfony/recipe.png)

Troisième étape :

Nous avons déjà eu un aperçu de ce que devrait donner cette étape précédemment (voir image 8).

L'objectif de cette étape est d'obtenir un rendu clair des recettes et de leur coût en ressources par minute. Pour enrichir les données, 
j'ai décidé d'ajouter quatre formulaires différents pour le rendu.

Les formulaires dans l'ordre sont :

- Un formulaire prenant en compte le nombre de bâtiments que je compte utiliser.
- Un formulaire prenant en compte le nombre de ressources en entrée que je veux utiliser.
- Un formulaire prenant en compte le nombre de ressources en sortie que je compte utiliser.
- L'énergie totale que je souhaite allouer à la création d'une ressource en particulier.

Précision, les bâtiments consomme de l'électricité, ce qui est aussi pris en compte dans les calculs précedent (surtout utile pour le quatrième formulaire).

Les résultats n'étant pas toujours rond, j'ai ajouté la possibilité de :
- Garder les résultats bruts
- Arrondir les résultats au supérieur
- Arrondir les résultats à l'inférieur  

Image 9  

![alt text](/assets/SatiSymfony/buildingEstimation.png)  

Ici, on veut avoir les résultats pour l'utilisation de 5 bâtiments construisant des câbles.  

Image 10  

![alt text](/assets/SatiSymfony/cableEstimation.png)

On peut donc en conclure que :  
Pour l'utilisation de 5 bâtiments créant des câbles, on a besoin de 300 ressources en entrée et cela nous donnera 150 câbles en sortie, et cela
coûtera 20 KWh.

Quatrième étape :

Comme on a pu le constater avec l'exemple juste au-dessus, on ne connaît pas les ressources en entrée nécessaires afin de créer des câbles.  
Cette étape permet donc de montrer à l'utilisateur CHAQUE étape nécessaire à la création d'un objet.

Si on reprend l'exemple des câbles, cela nous donne :  

Image 11  

![alt text](/assets/SatiSymfony/cableEstimationPlus.png)

On en conclu donc que :  
Pour l'utilisation de 5 bâtiments créant des câbles, on a besoin de 300 wire (fil électrique) et on obtient 150 câbles.
Et pour créer 300 fils électriques, on a besoin de 150 lingots de cuivre ainsi que de 10 bâtiments consommant 40 KWh.
Et pour finir, afin de créer 150 lingots de cuivre, on a besoin de 150 minerais de cuivre (non affichée car c'est une ressource naturelle)
ainsi que de 5 bâtiments consommant 80 KWh.

On observe donc que l'affichage des ressources détaillées permet à l'utilisateur de connaître chaque étape de création d'une ressource.