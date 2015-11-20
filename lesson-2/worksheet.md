# Contrôler des systèmes physiques via le réseau avec deux Raspberry Pi

Il y a trois étapes à réaliser pour configurer les Raspberry Pi pour que l'un puisse contrôler l'autre: configurer le réseau, écrire le programme du serveur, enfin configurer le matériel.

## Configuration du réseau

Avant que les Raspberry Pi puissent communiquer, il doivent être reliés l'un à l'autre au travers d'un réseau. Normalement lorsque vous connectez un appareil au réseau, il reçoit un identifiant unique appelé adresse IP. Comme le système ne comporte que deux Raspberry Pi, nous allons attribuer à chacun d'eux sa propre adresse IP.

1. Suivez le [Guide de configuration d'une adresse IP statique](/lesson-1/rpi-static-ip-address.md) de la leçon 1 pour configurer l'adresse IP d'un des Raspberry Pi.

1. Répétez la procédure sur l'autre Raspberry Pi, et donnez lui l'adresse IP `192.168.0.3`.

**Astuce:** Utilisez des post-it pour afficher l'adresse de chaque Raspberry Pi sinon les choses peuvent devenir compliquées par la suite!

### Tester votre réseau

1. Reliez les deux Raspberry Pi par un c^cble Ethernet
1. Sur le Raspberry Pi dont l'adresse se termine par `.2`, tapez:

    ```bash
    ping 192.168.0.3 -c5
    ```

Vous devriez lire quelque chose comme ceci:

```
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_req=1 ttl=128 time=3.46 ms
[...quatre autres PINGs ...]

--- 192.168.0.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 3.466/3.788/4.380/0.322 ms
```

Si ce n'est pas le cas, vérifiez vos modifications ainsi que le câble réseau. Une fois que les Raspberry Pi fonctionnent en réseau vous êtes prêt à écrire le programme de contrôle.

## Configuration du programme de contrôle

1. Créez un nouveau fichier avec l'éditeur nano en tapant `nano thing-server.py`.
1. Saisissez le programme suivant:

    ```python
    import RPi.GPIO as GPIO
    import time
    import network

    SWITCH = 10
    GPIO.setmode(GPIO.BCM)
    GPIO.setup(SWITCH, GPIO.IN)

    def heard(phrase):
      print "heard:" + phrase
      for a in phrase:
        if a == "\r" or a == "\n":
          pass # strip it
        else:
          if (GPIO.input(SWITCH)):
            network.say("1")
          else:
            network.say("0")

    while True:
      print "waiting for connection"
      network.wait(whenHearCall=heard)
      print "connected"

      while network.isConnected():
        print "server is running"  
        time.sleep(1)

      print "connection closed"
     ```

1. Save the file with `CTRL-O` and then exit nano with `CTRL-X`.

## Setting up the hardware

**Important**: do not connect hardware directly to the pins! Use female header wires that you can plug onto the GPIO pins and your hardware.

### 1. Set up the client machine with an LED

![](images/client-led-setup.png)

### 2. Set up the server with a button

Note: you do not need to use an actual button, just something to connect the GPIO pin to the ground pin. It could be two paper clips or something similar. Again, use header wires that protect the GPIO pins.

![](images/server-button-setup.png)

## Running the program

The **server** machine is connected to a button. It monitors data from the client machine, and when it receives a '?' character it sends back a '1' if the button is pressed and a '0' if it is not.

The **client** machine is connected to an LED. It sends a '?' character every second to the server, and if it gets back a '1' in reply it turns the LED on.

1. Set the first Pi up as a **server** by typing:

    ```bash
    python thing-server.py
    ```

1. The second Pi will be the **client**. You need to tell it the IP address of the server that you want to connect to. For example, to connect to the Raspberry Pi that has the IP address ending in `.2`, type:

    ```bash
    python thing-client.py 192.168.0.2
    ```

1. You should now be able to press the button on the Raspberry Pi connected to the server, and the LED will flash on the client. Try it out!

### Things to think about:

- What is happening, physically and electrically, when you press the button?
- What happens at the other end, electrically, when the server receives a signal?

###Things to try:

- Can you break the program by pressing the button too fast?
- Try to change the frequency of the client requests (the `?` character). What happens if they get sent too fast?
- What happens if you stop the server program by pressing `CTRL-C`?

## What next?

- Change the message that appears when your program starts.
- Change the message that appears when a client connects.
- Change the character the client sends every second. Does it matter?
- Make the LED flash more quickly.
- Make the LED flash for a random length of time.
- Comment your code to explain what each section does.
- If the teacher asks you to, change the network configuration back to a dynamic IP address as shown in the "Clean up" section of the [Static IP address setup guide](/lesson-1/rpi-static-ip-address.md) from lesson 1.
