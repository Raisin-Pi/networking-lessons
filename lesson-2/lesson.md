# Leçon 2 - L'Internet des objets: comment les ordinateurs contrôlent d'autres ordinateurs?

En 2020, on estime qu'il y aura [50 milliards d'objets](http://newsroom.cisco.com/feature-content?type=webcontent&articleId=1208342) connectés à Internet, depuis les vaches jusqu'aux réfrigérateurs et aux voitures. C'est ce qu'on, nomme l'Internet des Objets. Dans cette leçon, les élèves vont construire l'Objet le plus simple - un bouton qui allume une LED via le réseau.

Les élèves seront conscients de l'importance et de la généralisation des réseaux suite à la leçon 1. Ils devraient être à l'aise avec les termes utilisés dans cette leçon, comme adresse IP, serveur et client. La conclusion de la leçon 1 pourra être utilisée comme révision pour commencer cette leçon 2.

## Objectifs de la leçon

- Savoir qu'un serveur informatique peut envoyer des données à un autre ordinateur sur un réseau
- Savoir que les données reçues via un réseau peuvent déclencher sur l'ordinateur client une action allant au-delà du simple affichage d'un message à l'écran


## A l’issue de cette leçon

### Tous les élèves seront capables de :

- Expliquer que les ordinateurs d'un réseau peuvent s'échanger des données, et que ces données peuvent déclencher une action sur l'ordinateur récepteur
- Utiliser un programme simple pour contrôler le matériel au travers d'un réseau


### La plupart des élèves seront capables de:

- Modifier un programme pour lui faire faire quelque chose de différent et d'utile

### Certains élèves seront capables de :
- Adapter un programme pour qu'un Raspberry Pi oblige le matériel connecté à un autre Raspberry Pi à se comporter d'une manière spécifique


## Résumé de la leçon
- Introduction à l'informatique connectée par un réseau physique
- Suite de la Leçon 1: contrôle du matériel sur la machine cliente au lieu d'afficher un message
- Contrôler les broches GPIO d'un autre Raspberry Pi via le réseau


## Introduction
Cela peut être fait sur papier ou par glisser-déposer sur un tableau interactif.
1. Les élèves ont une minute pour réorganiser le code Python de [code review](code-review.md) et le remettre dans le bon ordre.
1. Montrez le programme en fonctionnement. Discutez de ce qui se passe et pourquoi cela est fait, par exemple l'importation des modules, des fonctions, les boucles while, etc.
1. Montrez la vidéo du concours Raspberry Pi de PA Consulting Group pour les écoles et expliquez que tous les projets reposent sur un serveur qui contrôle un aspect physique d'un Raspberry Pi par exemple des capteurs, des afficheurs, des pompes, une caméra etc.

## Développement principal
1.	Expliquez que les étudiants devront connecter deux Raspberry Pi comme dans la leçon 1, mais cette fois ils vont contrôler du matériel au lieu d'envoyer le texte à l'écran.
1.	Les élèves doivent configurer le réseau et contrôler les LED sur un autre Raspberry Pi en utilisant la [Feuille de Travaux Pratiques](worksheet.md).
1.	Idées d'extension : ajouter des messages d'écran pour la rétroaction, changer la façon dont les LED clignotent, changer le bouton physique (par exemple, par un "détecteur de pression" fait avec de le feuille d'aluminium), ajouter une autre LED (avancé).
1.	**Facultatif**: Si vous réutilisez les cartes SD sur le réseau, les élèves devront annuler leurs modifications dans le fichier `interfaces`. Demandez-leur de le faire comme indiqué dans la section «Nettoyer» du guide de configuration de l'adresse IP statique disponible dans la leçon précédente.

## Conclusion
Demandez à chaque groupe de se lever et expliquer les changements qu'ils ont fait à leur programme et comment ils l'ont fait. Affichez leur code sur l'écran si vous en avez la possibilité, et demandez-leur d'expliquer la logique et la syntaxe de leurs modifications.

## A faire à la maison
1.	Montrez aux élèves la source de la citation [50 milliards d'Objets](http://newsroom.cisco.com/feature-content?type=webcontent&articleId=1208342)".
1.	Demandez-leur de faire une recherche et de préparer un exposé rapide (1 minute) pour la prochaine leçon, montrant soit que l'IdO est une bonne chose soit qu'il est une mauvaise chose (pas de réponse mitigée). S'ils veulent réaliser une présentation avec des diapositives ils ne pourront employer que des images, pas de mots.
1.	Lors de la prochaine leçon choisissez plusieurs élèves au hasard, demandez leur de se lever et donnez leur la parole, chacun disposera d'un temps égal pour présenter ses arguments. Les votes de la classe désigneront le vainqueur.
