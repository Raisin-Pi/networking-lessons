# Leçon 3 - Protocole de configuration dynamique de l'hôte (DHCP)
#### Dynamic Host Configuration Protocol

Dans cette leçon, les élèves apprendront comment le Raspberry Pi peut être utilisé pour montrer le fonctionnement du DHCP sur un réseau isolé.

Maintenant, il devrait être clair que la modification répétée du fichier `/etc/network/interfaces` est longue et fastidieuse. Il ya un certain nombre d'inconvénients à donner des adresses IP statiques à tous les ordinateurs d'un réseau. Pensez à ce qui se passerait si vous vouliez ajouter beaucoup  plus d'ordinateurs à votre réseau.

- Les utilisateurs devraient configurer manuellement des adresses IP
- Les utilisateurs devraient s'assurer qu'il n'y a pas deux  ordinateurs avec la même adresse
- Cela prend du temps de modifier manuellement le fichier de configuration sur chaque ordinateur
- Ce n'est pas l'idéal pour les appareils mobiles comme les ordinateurs portables, qui rejoignent et quittent fréquemment le réseau

Comment pouvons-nous rendre tout ceci plus facile?

Pour une grande partie de cette leçon, il est préférable que le travail soit effectué par des élèves en binome. Le hub ou le switch Ethernet doivent rester complètement isolés, sans aucun câble Ethernet les reliant au réseau principal de l'école.

## Objectifs de la leçon

- Comprendre que ce qu'est le Protocole de Configuration Dynamique de l'hôte (DHCP)
- Connaître le rôle qu'il joue dans la structure globale d'un réseau informatique

## A l’issue de cette leçon

### Tous les élèves seront capables de :
- Comprendre la nécessité de disposer d'un DHCP dans un réseau informatique
- Utiliser un serveur DHCP pour qu'un Raspberry Pi obtienne une adresse automatiquement

### La plupart des élèves seront capables de :
- Comprendre la logique interne d'un serveur DHCP

## Résumé de la leçon

- Discussion sur le processus logique suivi par le service DHCP
- Configuration d'un Raspberry Pi pour en faire un serveur DHCP
- Utilisation d'autres Raspberry Pi qui obtiendront des adresses IP fournies par le serveur
- Test du réseau

## Introduction

Tout d'abord, voyons ce qu'est un **serveur**. Un serveur est essentiellement un ordinateur dont le but principal est de fournir un service. Un serveur web, par exemple, offre le service de transmission de pages Web, images et fichiers sur Internet. Un serveur Minecraft fournit le service de mémorisation du monde en 3D, se souvenant de l'endroit où les blocs se trouvent et  permettant aux joueurs de se voir l'un l'autre. Les serveurs sont des ordinateurs qui sont dédiés à une tâche (mais ils peuvent aussi être dédiés à plus d'une tâche).

Un ordinateur ou une application qui souhaite utiliser un serveur est souvent appelé un **client** parce qu'il se comporte comme un consommateur vis à vis d'un serveur. Les navigateurs Internet sont parfois appelés clients Internet parce qu'ils se comportent comme des clients par rtapport à un à un serveur web. Vous avez probablement entendu parler de votre jeu Minecraft comme d'un client du jeu pour la même raison.

Ne serait-ce pas merveilleux si nous pouvions avoir un serveur qui prendrait la peine d'attribuer des adresses IP sur notre réseau et qui se souvinedrait de qui possède quelle adresse ?

C'est exactement pour cela que le DHCP est prévu. DHCP signifie **D**ynamic **H**ost **C**onfiguration **P**rotocol (Protocole de Configuration Dynamique des Hôtes);  **Dynamic** Signifie en constante évolution, **Host** est juste un autre mot pour désigner un ordinateur, **Configuration** se réfère à la configuration des paramètres de votre réseau, et **Protocol** désigne un ensemble de règles qui définissent comment les choses doivent se faire.
## Début de l'activité

Une démonstration sans ordinateur est un bon début pour montrer le processus logique utilisé par un serveur DHCP.

Commencez par désigner un élève qui sera le serveur DHCP; Clui-ci possède des cartons, du papier et un crayon ou un stylo. Les autres élèves vont maintenant se comporter comme des clients/hôtes dynamiques (des ordinateurs qui changent constamment) sur le réseau.

Le serveur DHCP doit suivre un un ensemble de règles; c'est ce qui représente la partie "protocole" dans le nom. L'un des hôtes/clients veut maintenant rejoindre le réseau; son nom est **Dave**. Voilà comment la conversation devrait se passer :

- HOTE : "Bonjour je suis **Dave** est-ce qu'il y a un serveur DHCP par là ?"
- DHCP : "Oui. Je suis ici, **Dave** et je peux te donner l'adresse X."
- HOTE : "serveur DHCP, puis-je prendre l'adresse X s'il te plaît."
- DHCP : "**Dave**, voici l'adresse X. Tu peux la garder pendant 12 heures."

Le serveur DHCP tend un carton à **Dave**. Sur ce carton figure l'adresse de **Dave**. Le serveur DHCP écrit sur son papier le nom de **Dave** ainsi que l'adresse qu'il lui a donnée, l'heure à laquelle il l'a donnée ainsi que la durée du bail (12 heures). L'heure à laquelle l'adresse a été donnée est utilisée pour calculer l'âge du bail.

Arrêtons-nous un moment pour voir si une partie de cette conversation était inattenduz. Lorsqu'un ordinateur rejoint un réseau, il n'a aucun moyen de savoir si un serveur DHCP est disponible, il envoie un signal (broadcast = diffusion) à l'ensemble du réseau demandant s'il y en a un. S'il y en a un qui est disponible, il répondra à l'hôte en lui proposant une adresse; l'hôte demande alors officiellement une adresse. La partie que vous pourriez ne pas avoir attendu est que l'adresse est donnée avec une durée de bail, dans ce cas 12 heures. Demandez-vous pourquoi cela existe et continuez cette leçon.

Maintenant, supposons que **Dave** veuille quitter le réseau ou est en train de s'arrêter. La conversation ressemblerait à ceci :

- HOTE: "serveur DHCP, je suis **Dave** et je te rends mon adresse IP."
- DHCP: "Merci **Dave**, au revoir."

**Dave** rend alors la carte portant son adresse au serveur DHCP. Le serveur DHCP range la carte avec les autres et raye le nom de Dave sur sa feuille de papier. Cette carte d'adresse pourra maintenant être remise à un autre ordinateur/hôte qui rejoindrait le réseau.

Maintenant, considérons ce qui pourrait arriver si **Dave** n'avait pas été arrêté proprement. Supposons que le câble d'alimentation ait été soudainement débranché et qu'il n'ait pas eu l'occasion de rendre correctement son adresse au serveur DHCP; ou peut-être a-t-il décidé tout simplement de s'éteindre en gardant l'adresse! Que se passerait-il alors? Le serveur DHCP ne donnera pas deux fois la même adresse , alors on perd cette adresse pour toujours ?

C'est ici que la durée du bail entre en jeu! L'adresse sera inutilisable, mais seulement jusqu'à l'expiration de la durée du bail. Après 12 heures voilà ce qui pourrait arriver :
- HOTE : "Bonjour je suis **Fred** est-ce qu'il y a un serveur DHCP par là ?"
- DHCP : "Oui. Je suis ici, **Fred** Comme il y a plus de 12h que je n'ai pas de nouvelles de **Dave** je peux te donner l'adresse X."
- HOTE : "serveur DHCP, puis-je prendre l'adresse X s'il te plaît."
- DHCP : "**Fred**, voici l'adresse X. Tu peux la garder pendant 12 heures."

Le serveur DHCP fait un nouveau carton avec cette même adresse et le tend à **Fred**, raye le nom le nom **Dave**  de la liste, et le remplace par **Fred**.

Voilà donc comment ce problème est traité; toutes les adresses sont attribuées avec un délai déterminé afin que dans le cas d'un hôte qui tombe brutalement en panne ou quitte soudain le réseau, le serveur DHCP puisse petit à petit reprendre possession de ces adresses au fur et à mesure que leurs durées de location expirent.

Une variante de cette conversation pourrait être :

- HOTE : "serveur DHCP je suis **Dave**, 12 heures se sont écoulées alors est-ce que je peux renouveler l'adresse X s'il te plaît ?"
- DHCP: D'accord **Dave** tu peux garder l'adresse pendant 12 heures supplémentaires.

Le serveur DHCP note ensuite sur son papier l'heure à laquelle l'adresse a été de nouveau accordée à **Dave**. Notez que tous les serveurs DHCP n'utiliseront pas un bail de 12 heures; la durée pourra être plus ou moins longue en fonction du serveur. Maintenant, si **Fred** veut rejoindre le réseau, il obtiendra une adresse différente de celle de **Dave**.

## Activité pratique principale

Tout d'abord, sélectionnez l'un des Raspberry Pi qui sera le serveur DHCP. Ce peut être une bonne idée de l'identifier en collant une étiquette dessus e munir d'un auto mettre un autocollant sur elle ou en le déplaçant vers un autre endroit pour éviter toute confusion par la suite. Nous allons devoir installer un logiciel sur ce Raspberry Pi, donc pour cette première partie, connectez le à un autre réseau local pour accéder à Internet.

### A faire sur le Raspberry Pi serveur uniquement

** Note :** Comme un seul Raspberry Pi sera configuré en serveur DHCP, il vaut mieux que cette partie de l'activité soit réalisée par une seule personne. Les autres élèves sont des observateurs. On n'a pas besoin de plus d'un serveur DHCP; en fait, s'il y en a plus d'un, cela peut causer des problèmes !

Entrez les commandes suivantes:

`` `bash
sudo apt-get update
sudo apt-get install dnsmasq
`` `

Une fois que c'est fini vous pouvez vous déconnecter du réseau local qui accède à Internet et reconnecter le Raspberry Pi au hub/switch utilisé pour cette partie pratique

Par convention, la plupart des serveurs DHCP ont une adresse IP statique qui est le premier ou le plus petit nombre dans l'espace d'adressage IP du réseau. Par exemple, la plupart des réseaux privés utilisent un espace d'adressage IP local de `192.168.0.X`, où` X` est un nombre qui est différent pour chaque périphérique. Si on suit cette convention, notre serveur DHCP aura une adresse IP statique de `192.168.0.1`; Notez le `.1` à la fin. Les adresses IP qu'il pourra donner ensuite démarreront à `192.168.0.2`,` .3`, `.4`, et ainsi de suite jusqu'à` .254`.

Pour respecter cette convention, commençons par donner une adresse statique au Raspberry Pi qui sera serveur DHCP. Pour configurer cela, nous devons modifier à nouveau le fichier d'interface réseau. Entrez la commande suivante :

```bash
sudo nano /etc/network/interfaces
```

Dans ce fichier `eth0` se réfère au port Ethernet Raspberry Pi et` wlan0` se réfère à un dongle sans fil si vous en utilisez un. Trouvez la ligne suivante :

```bash
iface eth0 inet dhcp
```

Cette ligne indique au Raspberry Pi qu'il doit essayer d'obtenir une adresse IP d'un serveur DHCP pour l'interface `eth0`. Donc cela en fait essentiellement un **client** DHCP, mais nous voulons en faire un serveur **DHCP** donc cette ligne doit être désactivée. Mettez un dièse `#` au début de la ligne et ajoutez ensuite les quatre lignes suivantes pour configurer l'adresse IP statique, comme vous l'avez fait dans les exercices précédents:

```
# iface eth0 inet dhcp
auto eth0
iface eth0 inet static
address 192.168.0.1
netmask 255.255.255.0
```

Appuyez sur `Ctrl - O` puis sur `Entrée` Pour enregistrer,  puis sur `Ctrl - X` afin de quitter nano. Maintenant, entrez la commande suivante pour redémarrer le service réseau du Raspberry Pi :

```bash
sudo service networking restart
```

Ce Raspberry Pi sera maintenant toujours à l'adresse IP `192.168.0.1`. Vous pouvez confirmer cela en entrant la commande `ifconfig`; l'adresse IP doit être affichée sur la deuxième ligne juste après `inet addr`.

Ensuite, nous allons configurer le logiciel serveur DHCP, `dnsmasq`, qui a été installé auparavant. Nous allons définir explicitement un fichier de configuration pour le service `dnsmasq`, mais nous allons d'abord sauvegarder le fichier de configuration par défaut, puis mettre le notre à sa place. Entrez les commandes suivantes :

```bash
cd /etc
sudo mv dnsmasq.conf dnsmasq.default
sudo nano dnsmasq.conf
```

Vous devriez maintenant être en train de modifier un fichier vide. Copiez et collez ce qui suit dans le fichier :

```
interface = eth0
dhcp-range = 192.168.0.2,192.168.0.254,255.255.255.0,12h
```

La première ligne indique à `dnsmasq` qu'il doit écouter les requêtes DHCP sur le port Ethernet du Raspberry Pi. La deuxième ligne indique la *plage* d'adresses IP qui peuvent être distribuées; remarquez le `12h` à la fin de la ligne qui précise la durée du bail.

Appuyez sur `Ctrl - O` puis` Entrée` pour enregistrer le fichier, puis sur `Ctrl - X` pour quitter nano. Avant d'activer le serveur, assurez-vous que le Raspberry Pi serveur DHCP est le seul appareil connecté au hub/switch ; débranchez toutes les autres connexions Ethernet. Entrez la commande suivante pour redémarrer le service `dnsmasq`:

`` `bash
sudo service dnsmasq restart
`` `

Le service DHCP est maintenant actif et écoute les requêtes demandes des ordinateurs clients.

### Sur tous les autres clients Raspberry Pi

Avant de rebrancher les Raspberry Pi clients au hub/switch, vérifiez que leurs fichiers `/etc/network/interfaces` sont configurés pour obtenir une adresse IP depuis un serveur DHCP. Entrez la commande suivante :

```bash
sudo nano/etc/network/interfaces
```

Vérifiez qu'une adresse IP statique n'est **pas** spécifiée et contrôlez la présence de la ligne `iface eth0 inet dhcp` ; voici un exemple.

```
iface eth0 inet dhcp
# auto eth0
# iface eth0 inet static
# address 192.168.0.1
# netmask 255.255.255.0
```

Appuyez sur `Ctrl - O` puis sur `Entrée` pour enregistrer le fichier, puis sur `Ctrl - X` pour quitter nano.

Redémarrez le service réseau sur les clients avec la commande `sudo service networking restart`; vous pouvez ensuite continuer et les reconnecter au hub/switch. Ils doivent immédiatement récupérer une adresse IP depuis le serveur DHCP.

Vérifiez le en utilisant la commande `ifconfig` à nouveau ; les adresses IP indiquées devraient être situées dans la plage spécifiée sur le serveur.

### Test du réseau

Une fois que tout le monde a une adresse IP le réseau devrait fonctionner comme prévu. Testez-le en utilisant votre programme de chat ou en jouant à Minecraft. Assurez-vous que tout le monde peut réussir à pinger le serveur DHCP avec la commande `ping 192.168.0.1`, et qu'ils peuvent se pinger les uns les autres avec la commande` ping 192.168.0.X` (où X est la quatrième partie de leur adresse IP). Le serveur doit également pouvoir faire un ping sur clients.

### Aller plus loin
Si vous voulez aller plus loin et d'observer la communication entre le serveur DHCP et les clients, les commandes suivantes peuvent être utilisées sur les Raspberry Pi **client**.

Tout d'abord, pour arrêter l'interface Ethernet et rendre votre adresse IP au serveur DHCP, entrez cette commande:

```bash
sudo ifdown eth0
```

Vous devriez voir quelque chose qui ressemble au texte ci-dessous. Notez la ligne `DHCPRELEASE`; c'est ici que l'adresse IP est remise au serveur.

```
Listening on LPF/eth0/b8:27:eb:aa:bb:cc
Sending on   LPF/eth0/b8:27:eb:aa:bb:cc
Sending on   Socket/fallback
DHCPRELEASE on eth0 to 192.168.0.1 port 67
```

Ensuite, utilisez la commande suivante pour redémarrer l'interface Ethernet et obtenir une adresse IP du serveur DHCP :

```bash
sudo ifup eth0
```

Vous devriez voir une sortie similaire au texte ci-dessous. Notez les lignes `DHCPDISCOVER`,` DHCPREQUEST`, `DHCPOFFER`, et`DHCPACK`. Faites le rapprochement avec ce qui a été vu au début de l'activité. Pouvez vous identifier la phrase à laquelle chacune correspond ?

```
Listening on LPF/eth0/b8:27:eb:aa:bb:cc
Sending on   LPF/eth0/b8:27:eb:aa:bb:cc
Sending on   Socket/fallback
DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval 7
DHCPREQUEST on eth0 to 255.255.255.255 port 67
DHCPOFFER from 192.168.0.1
DHCPACK from 192.168.0.1
bound to 192.168.0.X -- renewal in 40000 seconds.
```

En utilisation normale, vous ne devez pas utiliser ces commandes parce que la même chose se produit automatiquement lorsque le Raspberry Pi démarre, s'arrête ou que son port Ethernet est connecté à un autre appareil.

## Résumé

Vous pouvez maintenant inviter les élèves à discuter des similitudes entre l'exercice pratique et l'activité de démarrage.

Une question qui devrait être soulevée est de savoir comment le serveur DHCP peut identifier chaque ordinateur qui s'adresse à lui. Dans l'activité de démarrage, l'ordinateur client a indiqué : "Je suis Dave" au serveur DHCP **avant** d'avoir reçu une adresse IP. Le serveur DHCP a ensuite écrit **Dave** sur son papier en face de l'adresse IP qu'il lui a donnée. Quel est l'équivalent de ceci pour un véritable ordinateur ?

La réponse est l'adresse **MAC** (parfois appelée l'adresse physique). MAC signifie Media Access Control ; c'est un identifiant unique qui est inscrit dans le matériel d'un  périphérique Ethernet par le fabricant. Tous les périphériques réseau, y compris ceux qui utilisent le WiFi et le Bluetooth, possèdent une adresse MAC. Une adresse MAC fait six octets de long et est souvent représenté comme six nombres hexadécimaux séparés par des virgules ou des tirets comme ceci: `01:23:45:67:89:ab`.

L'adresse MAC d'un Raspberry Pi peut être montrée en utilisant la commande `ifconfig`; regardez sous `eth0` et sur la première ligne juste après` HWaddr` (adresse matérielle). L'adresse MAC ressemblera à quelque chose comme `B8:27:eb:aa:bb:cc`. Une adresse MAC de Raspberry Pi commence toujours par `B8:27:eb`. En fait, c'est l'adresse MAC de l'ordinateur client que le serveur DHCP enregistre pour garder une trace de qui possède quelle adresse IP.

Regardez également ce que envoient sur l'écranles commandes `ifup` et `ifdown` vues plus haut !

## Travail à la maison

Ce sera un concours lancé pour trouver un dispositif à la maison ou à l'école qui est doté d'un serveur DHCP. Écrivez 100 mots sur les raisons de la présence de ce serveur DHCP  en ce lieu.
