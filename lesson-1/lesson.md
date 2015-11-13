# Leçon 1 - Comment les ordinateurs communiquent-ils ?

https://www.raspberrypi.org/learning/networking-lessons/lesson-1/plan/

Dans cette leçon, les élèves vont construire un réseau simple et l'utiliser pour communiquer via un programme de chat en réseau. Les élèves apprendront comment mettre en réseau deux Raspberry Pi et écriront ensuite écrire un petit programme en Python qui leur permettra d'envoyer de s’envoyer des messages les uns aux autres.
Si les élèves ne sont pas à l'aise avec la configuration du Raspberry Pi et l'écriture et l'exécution des programmes Python, vous pouvez essayer de commencer par un exercice avec le
 [Test de Turing](https://www.raspberrypi.org/learning/turing-test-lessons/).

## Objectifs de la leçon

- Savoir que d'un réseau informatique est constitué de deux ou plusieurs ordinateurs (ou périphériques) reliés entre eux
- Savoir que les ordinateurs ont une adresse IP unique qui permet aux autres ordinateurs de les trouver et de leur envoyer des données

## A l’issue de cette leçon

### Tous les élèves seront capables de :

- Expliquer qu'un réseau informatique est composé de deux (ou plusieurs) ordinateurs reliés entre eux
- Utiliser un programme simple pour envoyer des messages entre deux ordinateurs

### La plupart des élèves seront capables de:

- Expliquer que chaque ordinateur d’un réseau doit avoir une adresse unique appelée adresse IP
- Apporter des modifications simples à leur programme (modification de l’invite affichée à l'écran, ajout de messages de bienvenue)

### Certains élèves seront capables de :

- Apporter des modifications plus complexes à leur programme comme l'envoi de réponse en fonction de mots-clés

## Résumé de la leçon

- Une introduction aux réseaux informatiques simples
- Mettre des Raspberry Pi en réseau
- Ecriture d'un programme de chat

## A faire à la maison

Dans la leçon **précédente** il est demandé que les élèves regardent [cette vidéo sur les bases du réseau](http://www.youtube.com/watch?v=kNJZ-v263zc). Ensuite, ils effectuent des recherches et notent ce qu'ils entendent par les termes suivants : réseau informatique, adresse IP, serveur, client et LAN (réseau local). Pour cette leçon ils doivent connaître ces termes sans devoir relire leurs notes.

## Introduction

Cette introduction est un ensemble d'informations rapides pour initier les élèves à l'universalité des réseaux et leur faire comprendre pourquoi ils sont utiles ou, essentiels pour eux !

1. Introduire l'idée que les réseaux informatiques sont partout. En groupes de deux ou trois, demander aux élèves de noter le plus de périphériques réseau (ou d'utilisations du réseau) qu'ils peuvent en deux minutes. Donnez-leur un ou deux exemples pour les aider à démarrer.

1. Demandez à chaque groupe d'encercler tous les périphériques, figurant sur leur liste, qu'ils ont utilisés, depuis qu'ils se sont levés ce matin. Ils ont une minute pour le faire.

1. Le groupe qui a **touché** le plus de périphériques réseau en 30 secondes gagne; ils sont autorisés à sortir des appareils de leurs sacs, à se lever de leur chaise et ainsi de suite. **ASTUCE:** Ceci est particulièrement efficace (et frénétique !) si vous avez un morceau de musique qui dure 30 secondes et qu'ils doivent s'immobiliser lorsque la musique arrête.

1. Interrogez chaque groupe sur l'un des appareils qu'ils ont choisi et discutez-en avec eux brièvement. Ajoutez quelques exemples moins évidents comme les guichets automatiques des banques et les radars routiers. Insistez sur le fait que le monde actuel repose sur des réseaux informatiques et qu'ils vont maintenant construire leur propre réseau.

## Développement principal

Les concepts que les élèves ont appris dans le travail à faire à la maison seront utilisés et consolidés, lorsqu'ils réaliseront les travaux pratiques.

1. Les élèves doivent mettre en place le réseau et envoyer des messages à l'aide de la fiche de travaux pratiques.Students should set up the network and send messages using the [Fiche de Travaux Pratiques - Elève](worksheet.md).

1. **Facultatif**: Si vous réutilisez les cartes SD sur le réseau, les élèves devront annuler les modifications qu'ils ont faites précédemment dans le fichier `interfaces`. Demandez-leur de le faire en suivant la section «Nettoyage» de la feuille de travaux pratiques.

## Conclusion

Ecrire la liste suivante de mots au tableau :

- Serveur
- Client
- Réseau informatique
- Adresse IP
- Adresse IP statique
- LAN

Choisissez un élève au hasard dans la classe. Il doit sélectionner l'un des mots/termes au tableau, se lever et choisir quelqu'un d'autre dans la classe qui doit alors expliquer ce que signifie ce mot. Cette personne choisit alors la prochaine personne qui expliquera l'un des termes restant.
