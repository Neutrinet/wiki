<!-- TITLE: Hub Cube -->
<!-- SUBTITLE: Dédié (corps et âme) à la brique Internet -->

- [Installer une brique Olimex](brique/install)
- [Installer Yunohost et le VPN Neutrinet sur un serveur X86](brique/install-x86)
- [DNS Config for your Cube](brique/dns)
- [Configure Ethernet](https://wiki.labriqueinter.net/doku.php?id=howto:parametrer_une_brique_avec_un_connecteur_usb-ethernet_au_lieu_d_un_connecteur_usb-wifi)

# Se connecter avec un câble série

- ordre pins, en partant du connecteur ethernet: bleu, rouge, vert (ou noir, vert, blanc pour le connecteur à 4 broches)
- sudo screen /dev/ttyUSB0 115200
- connecter en tant que root

# Monter un disque dur externe

- trouver l'UUID de votre disque, connecter le disque puis: `ls /dev/disk/by-uuid/` 
- dans `/etc/fstab`, ajouter: `UUID=<uuid-du-disque> /mnt/<mon-point-de-montage> ext4 defaults,nofail,x-systemd.device-timeout=5 0 2`
