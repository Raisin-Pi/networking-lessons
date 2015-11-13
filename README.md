# Apprendre le réseau

![Networked Raspberry Pi](images/pi-network.jpg)

## Présentation

Ce plan de travail a été conçu comme une introduction à l'utilisation du réseau avec le Raspberry Pi. Les élèves mettront en place et utiliseront différents réseaux, et découvriront les fonctions de base du réseau au travers d'activités pratiques.

Ce plan de travail est spécifiquement destiné aux programmes (britanniques) KS3 et KS4, bien que les deux premières leçons puissent être utilisées pour KS2. Il a été développé dans le cadre du programme national 2014 en Angleterre, mais il n'est pas spécifique à ce programme.


## Que vont apprendre les élèves ?

Au cours des quatre leçons, les élèves apprendront :

- Comment mettre en réseau deux ou plusieurs Raspberry Pis
- Comment programmer un Raspberry Pi pour envoyer des messages à un autre Pi
- Comment contrôler le matériel au travers d'un réseau
- La configuration du réseau et le paramétrage des serveurs
- Les concepts de base des réseaux, y compris:
	-  L'adresse IP
	- Clients et Serveurs
	- DHCP
	- DNS


## Liens avec le programme d'étude de l'informatique

### KS2:

- Comprendre les réseaux informatiques, y compris Internet

### KS3:

- Comprendre les composants matériels et logiciels qui constituent les systèmes informatiques, et la façon dont ils communiquent entre eux et avec les autres systèmes.

Les leçons utilisent le langage de programmation Python et abordent donc plusieurs aspects du programme d'études liés à la programmation.

[National Curriculum Computing Programmes of Study](https://www.gov.uk/government/publications/national-curriculum-in-england-computing-programmes-of-study/national-curriculum-in-england-computing-programmes-of-study#key-stage-3)

## Ressources

Il est suggéré que le travail soit effectué individuellement ou par binôme sur un Raspberry Pi. Chaque Raspberry Pi se connecte à un autre Pi dans les premières leçons et vous aurez besoin d'un nombre pair de Raspberry Pi. Chaque élève ou binôme devrait avoir accès au matériel suivant:

- Un Raspberry Pi avec la configurartion suivante :
  - Une carte SD avec la dernière version de NOOBS et Raspbian installé
  - `network.py` et `thing-client.py` copiés sur la carte SD
  - Un clavier, une souris et un écran
  - Un câble réseau pour relier les deux Raspberry Pi
  - Quatre connecteurs femelle-femelle et une LED (pour la leçon 2 uniquement)

## Leçons

- [Leçon 1: Comment communiquent les ordinateur?](lesson-1/lesson.md)
	- (Notez que cette leçon nécessite que les élèves aient regardé une vidéo auparavant; ceci peut être proposé comme travail à la maison à la suite de la leçon précédente ou proposé en classe le moment venu).
- [Leçon 2: L'internet des objets: Comment les ordinateurs contrôlent d'autres ordinateurs?](lesson-2/lesson.md)
- [Leçon 3: Dynamic Host Configuration Protocol (DHCP)](lesson-3/lesson.md)
- [Leçon 4: Domain Name System (DNS)](lesson-4/lesson.md)

## Licence

Sauf spécification expresse, tout le contenu de ce répertoire est couvert par la licence suivante:

[![Creative Commons Attribution 4.0 International Licence](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

***Apprendre le réseau*** par la [Raspberry Pi Foundation](http://www.raspberrypi.org) est sous licence [Creative Commons Attribution 4.0 International Licence](http://creativecommons.org/licenses/by-sa/4.0/).

D'après https://github.com/raspberrypilearning/networking-lessons

Les leçons 1 et 2 sont basées sur des idées et du code de [David Whale](https://twitter.com/whaleygeek)

Traduction [François MOCQ](http://www.framboise314.fr "framboise314")
