# Comment configurer votre Raspberry Pi pour avoir une adresse IP statique

Habituellement, lorsque vous connectez un Raspberry Pi à un réseau local (LAN), une adresse IP lui est attribuée automatiquement. Normalement, cette adresse change à chaque fois que vous vous connectez.

Parfois, cependant, vous pouvez souhaiter que votre Raspberry Pi démarre toujours avec la même adresse IP. Cela peut être utile si vous créez un petit réseau autonome, ou un projet autonome comme un robot. Voici comment procéder.
## Setup

Editez le fichier `/etc/network/interfaces` comme ceci:

1. Tapez `sudo nano /etc/network/interfaces` en ligne de commande.

1. Recherchez cette ligne:

    ```bash
    iface eth0 inet dhcp
    ```

1. Remplacez le mot `dhcp` par `static`.

1. Appuyez sur la touche `Entrée` et ajoutez les lignes suivantes:

    ```
    address 192.168.0.2
    netmask 255.255.255.0
    network 192.168.0.0
    broadcast 192.168.0.255
    gateway 192.168.0.1
    ```

1. Enregistrez le fichier avec `CTRL + O` puis quittez nano avec `CTRL + X`.

A chaque démarrage votre Raspberry Pi prendra l'adresse IP `192.168.0.2`; nous n'avons pas utilisé `192.168.0.1` qui est l'adresse réservée pour le routeur. Vous pouvez bien entendu utiliser l'adresse de votre choix, mais elle devra être comprise entre `192.168.0.2` et `192.168.0.255`.

## Tests

1. Redémarrez le Raspberry Pi avec la commande `sudo reboot`.

1. Loguez vous et tapez `ip a`.

1. Vous devriez voir l'adresse IP que vous avez choisie, dans la rubrique `eth0:`.

## Dépannage


Si votre Raspberry Pi n'est pas en réseau, vous ne verrez pas l'adresse IP. Pour résoudre ce problème, connectez votre Raspberry Pi à une prise réseau active, ce peut être un autre Raspberry Pi qui fonctionne; vous pouvez également activer manuellement l'interface réseau en tapant `sudo ifup eth0` suivi par`ip a`.

Si vous ne voyez toujours pas votre adresse IP, ou si elle est différente de celle que vous avez définie, ouvrez le fichier `interfaces`, comme décrit à l'étape 1 des instructions d'installation, et vérifiez l'exactitude de vos modifications.

## Nettoyer - annuler les modifications

Normalement, votre ordinateur n'est pas configuré pour utiliser une adresse IP statique. Vous pouvez remettre la configuration réseau d'origine en éditant le fichier `interfaces` comme suit:

1. Tapez `sudo nano /etc/network/interfaces` en ligne de commande.

1. Recherchez la ligne:

    ```
    iface eth0 inet static
    ```

1. Remplacez le mot `static` par `dhcp`.

1. Mettez en commentaire les lignes ci-dessous en plaçant un `#` au début de la ligne (vous pourriez les détruire complètement mais vous pourrez en avoir besoin ultérieurement):

    ```
#address 192.168.0.2
#netmask 255.255.255.0
#network 192.168.0.0
#broadcast 192.168.0.255
#gateway 192.168.0.1
    ```
1. Enregistrez le fichier avec `CTRL + O`, quittez nano avec `CTRL + X`  puis redémarrez avec `sudo reboot`.
