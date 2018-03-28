<!-- TITLE: Divers -->
<!-- SUBTITLE: Divers ressouces sur le cube -->

:warning: **À ranger / Classer / Compléter** :warning:
# Ressources externes

* [Installer Yunohost et le VPN Neutrinet sur un serveur X86](cube/install-x-86)
* [Configure Ethernet (incomplet)](https://wiki.labriqueinter.net/doku.php?id=howto:parametrer_une_brique_avec_un_connecteur_usb-ethernet_au_lieu_d_un_connecteur_usb-wifi)
* [wiki de la briqueinter.net](https://wiki.labriqueinter.net/doku.php)
* [un pad étendu sur la brique](https://pad.lqdn.fr/p/brique-formation)


# Se connecter avec un câble série

- ordre pins, en partant du connecteur ethernet: bleu, rouge, vert (ou noir, vert, blanc pour le connecteur à 4 broches)
- sudo screen /dev/ttyUSB0 115200
- connecter en tant que root

# Monter un disque dur externe

- trouver l'UUID de votre disque, connecter le disque puis: `ls /dev/disk/by-uuid/` 
- dans `/etc/fstab`, ajouter: `UUID=<uuid-du-disque> /mnt/<mon-point-de-montage> ext4 defaults,nofail,x-systemd.device-timeout=5 0 2`


# Cartes mémoire

![Microsdspeedtable](/uploads/cube/microsdspeedtable.png "Microsdspeedtable")

Source : [wikipedia en anglais](https://en.wikipedia.org/wiki/Secure_Digital)