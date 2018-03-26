<!-- TITLE: Cube -->
<!-- SUBTITLE: Infos en vrac, à étoffer ou à ranger -->


- [Installer Yunohost et le VPN Neutrinet sur un serveur X86](cube/install-x-86)

- [Configure Ethernet](https://wiki.labriqueinter.net/doku.php?id=howto:parametrer_une_brique_avec_un_connecteur_usb-ethernet_au_lieu_d_un_connecteur_usb-wifi)

- [wiki de la brique](https://wiki.labriqueinter.net/doku.php)

- [un pad sur la brique](https://pad.lqdn.fr/p/brique-formation)
# Se connecter avec un câble série

- ordre pins, en partant du connecteur ethernet: bleu, rouge, vert (ou noir, vert, blanc pour le connecteur à 4 broches)
- sudo screen /dev/ttyUSB0 115200
- connecter en tant que root

# Monter un disque dur externe

- trouver l'UUID de votre disque, connecter le disque puis: `ls /dev/disk/by-uuid/` 
- dans `/etc/fstab`, ajouter: `UUID=<uuid-du-disque> /mnt/<mon-point-de-montage> ext4 defaults,nofail,x-systemd.device-timeout=5 0 2`


# Cartes mémoire

![Microsdspeedtable](/uploads/cube/microsdspeedtable.png "Microsdspeedtable")

Source : [wikibedia en anglais](https://en.wikipedia.org/wiki/Secure_Digital)