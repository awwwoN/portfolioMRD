---
title: Advent Of Code
publishDate: 2019-12-01 00:00:00
img: /assets/AdventOfCode/adventOfCode.png
img_alt: Logo non officiel de Advent Of Code
description: Calendrier de l'avent de dév (mini jeu web)
tags:
  - Dev
  - Backend
  - Online mini-games
---

Advent Of Code est un site internet qui, en décembre de chaque année, présente 31 mini jeu à réaliser.
Comme un calendrier de l'avent, il y a chaque jour une nouvelle énigme à résoudre.

Cela se présente sous cette forme :  

![alt text](/assets/AdventOfCode/dayOne.png)
![alt text](/assets/AdventOfCode/dayOne2.png)

On a donc une mise en situation puis on a une question, un peu comme un exercice qu'on pourra avoir en cours.

![alt text](/assets/AdventOfCode/questionDayOne.png)

On a ensuite un lien vers une page internet comprenant les ressources nécessaires afin de réaliser le problème en question.
Ici, on a la liste de tous les elfes ainsi que les calories que chacun transporte.

Afin de résoudre le problème de ce premier jour, j'ai commencé par récupérer la liste présente sur le site.

![alt text](/assets/AdventOfCode/getDataFromFile.png)

Ce qui nous donne :  

![alt text](/assets/AdventOfCode/data.png)

On peut aperçevoir qu'il y a un "\n" entre chaque liste de calorie qu'un elf transporte, cela permet de me faciliter pour la suite.

Une fois la liste récupérée, je peux passer à l'étape suivante, celle-ci consiste à trouver quel elfe transporte le plus de calorie.  

![alt text](/assets/AdventOfCode/getMaxValue.png)

Je commence par récupérer la liste grâce à la fonction créer précédemment.
Je crée ensuite une boucle foreach qui me permet de récupérer chaque calorie que transporte un elf, puis je l'additionne avec la prochaine jusqu'à
la dernière calorie.

![alt text](/assets/AdventOfCode/preg_split.png)

Cette ligne ci-dessus me permet de récupérer les calories une à une. La fonction preg_split permet de fractionner une châine de caractère avec une expression régulière.
Quand on récupère la liste de calories avec la fonction getDateFromFile, on se retrouve avec une châine de caractères comportant les calories de chaque elfes.
Or, on veut récupérer les calories d'un seul elfe puis les additionner. Cette fonction nous permet donc ceci.  

![alt text](/assets/AdventOfCode/splitData.png)

On obtient donc un tableau avec chaque calorie pour un elfe.  

![alt text](/assets/AdventOfCode/sum.png)

Cette ligne ci-dessus permet quant à elle d'additionner chaque valeur du tableau présent au dessus afin d'obtenir le nombre de calories total que transporte un elfe.  

![alt text](/assets/AdventOfCode/calorieMaxElfe.png)  

On peut interpréter ce résultat ainsi :  
Le premier elfe transporte à lui seul 61214 calories.

Á la fin de la fonction getMaxValue, on renvoie :  

![alt text](/assets/AdventOfCode/returnMax.png)

Cela permet de renvoyer la valeur la plus haute du tableau sum.

Une fois ces fonctions créer et fonctionnels, on peut enfin répondre à la question.

![alt text](/assets/AdventOfCode/showValue.png)
![alt text](/assets/AdventOfCode/data_html.png)
![alt text](/assets/AdventOfCode/result.png)

Une fois le résultat trouvé, on retourne sur le site AdventOfCode afin de rentrer ce résultat et si celui-ci est correct, on a accès à une deuxième partie du mini-jeu.
Cette partie consiste à reprendre le code déjà créer puis d'ajouter de nouvelles choses dans le but de répondre à la seconde problématique.  

![alt text](/assets/AdventOfCode/dayOnePartTwo.png)  

Cette fois, on doit trouver la somme des trois elfes transportant le plus de calories.

![alt text](/assets/AdventOfCode/getAmount.png)  

Afin de répondre à cette problématique, j'ai créé une nouvelle fonction qui fonctionne comme la précédente à un changement près, les deux dernières lignes.

![alt text](/assets/AdventOfCode/sortAndReturn.png)

La fonction "rsort" permet de trier un tableau de donnée en order descendant (de la plus grande à la plus petite valeur).

Ensuite, j'utilise ensuite array_sum qui calcule la somme des valeurs d'un tableau. Puis array_slice permet d'extraire une portion d'un tableau.
Ici, on cherche à trouver les trois valeurs les plus élevées. On récupère donc les trois premières valeurs du tableau.

On peut maintenant répondre à la seconde problématique :  

![alt text](/assets/AdventOfCode/showValue2.png)
![alt text](/assets/AdventOfCode/data_html2.png)
![alt text](/assets/AdventOfCode/result2.png)

Je vous invite à consulter mon GitHub pour découvrir les journées 2 à 6 que j'ai réalisées.
