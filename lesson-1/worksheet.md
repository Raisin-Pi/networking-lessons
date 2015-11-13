# Créer un réseau de chat avec deux Raspberry Pi

## Feuille de Travaux pratiques - Elève

Dans cette leçon, vous allez mettre en place deux Raspberry Pi pour créer un réseau, et utiliser un programme en Python pour échanger des messages entre eux. Il y a deux étapes : configurer le réseau et écrire le programme.

## Configuration du réseau

Avant que les Raspberry Pi soient capables de communiquer, ils doivent être reliés entre eux via un réseau. Normalement, quand un appareil se connecte à un réseau, il se voit attribuer un identifiant unique appelé une adresse IP. Comme nous avons seulement deux Raspberry Pi, nous allons donner à chaque Raspberry Pi sa propre adresse IP.

1. Suivez le [guide de configuration de l'adresse IP statique](rpi-static-ip-address.md) pour configurer l'adresse IP.

1. Répétez cette procédure avec l'autre Raspberry Pi, et donnez-lui l'adresse IP `192.168.0.3`.
**Astuce :** Utilisez un post-it pour marquer physiquement l'adresse IP sur chacun des Raspberry Pi, sinon les choses vont devenir compliquées plus tard!

### Testez votre réseau

1. Reliez les deux Raspberry Pi par un câble Ethernet
1. Sur le Raspberry Pi qui a l'adresse IP se terminant par `.2`, tapez :

    ```bash
    ping 192.168.0.3 -c5
    ```

Vous devriez voir quelque chose comme ceci :

```
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_req=1 ttl=128 time=3.46 ms
[...four more PINGs ...]

--- 192.168.0.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 3.466/3.788/4.380/0.322 ms
```

Sinon, vérifiez les modifications que vous avez faites dans le fichier interfaces ainsi que le branchement des câbles réseau (les LED sont-elles allumées ?). Une fois que les Raspberry Pi sont en réseau, vous êtes prêt à écrire le programme de chat.

## Mise en place du programme de chat

1. Créez un nouveau fichier avec l'éditeur nano en tapant  `nano chat.py`.
1. Tapez le programme suivant :

    ```python
    # Programme de chat simple

    import network
    import sys

    def heard(phrase):
      print("eux:" + phrase)

    if (len(sys.argv) >= 2):
      network.call(sys.argv[1], whenHearCall=heard)
    else:  
      network.wait(whenHearCall=heard)

    while network.isConnected():
      #phrase = raw_input() # à utiliser en python2
      phrase = input() # à utiliser en python3
      print("moi:" + phrase)
      network.say(phrase)
    ```

1. Enregistrez le fichier avec `CTRL-O` puis quittez nano avec  `CTRL-X`.

1. Configurez le premier Raspberry Pi en **serveur** en tapant :

    ```bash
    python chat.py
    ```

1. Le deuxième Raspberry Pi sera le **client**. Vous devez lui indiquer l'adresse IP du serveur auquel vous souhaitez vous connecter. Par exemple, pour se connecter à un Raspberry Pi qui a l'adresse IP se terminant par  `.2`, entrez :

    ```bash
    python chat.py 192.168.0.2
    ```

1. Vous devriez maintenant être en mesure de saisir des messages sur chacun des Raspberry Pi, et ils apparaîtront sur l'autre écran lorsque vous appuierez sur la touche `Entrée`.

Essayez ! Envoyez des messages depuis le serveur vers le client et vice versa.

### Réfléchissez à ce qui se passe lors de l'envoi des messages :

-	Que se passe-t-il à l'écran?
-	Qu'arrive-t-il physiquement aux messages lorsque vous appuyez sur la touche entrée?
-	Comment les messages savent-ils où ils doivent aller?

### Les choses à essayer :

-	Pouvez-vous bloquer le programme en envoyant des messages trop vite ou en même temps?
-	Que se passe-t-il si vous arrêtez le programme serveur en appuyant sur `CTRL-C`?
-	Modifier `chat.py` pour remplacer les mots  'moi:' et 'eux:' par vos propres noms.

## Et après ?

- Affichez un message de bienvenue lorsque votre programme démarre.
- Affichez un message de bienvenue sur l'écran de l'appelant quand il se connecte.
- Affichez un compteur de messages avant "moi:" et "eux:" à chaque message.
- Lorsque vous tapez une certaine lettre ou un mot choisi, modifiez le programme pour qu'il envoie une phrase entière à votre interlocuteur.
- Lorsque le mot «aléatoire» est tapé, envoyez un nombre de messages aléatoires différents à votre interlocuteur.
- Lorsque vous recevez certains mots de votre appelant, envoyez automatiquement une phrase entière vers celui-ci (une phrase différente pour les différents mots).
- Si l'enseignant vous le demande, modifiez la configuration du réseau pour revenir à une adresse IP dynamique, comme indiqué dans la section «Nettoyer» du [Guide de configuration de l'adresse IP statique](rpi-static-ip-address.md).
