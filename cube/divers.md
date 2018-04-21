<!-- TITLE: Divers -->
<!-- SUBTITLE: Divers ressouces sur le cube -->

:warning: **À ranger / Classer / Compléter** :warning:
# Ressources externes

* [Installer Yunohost et le VPN Neutrinet sur un serveur X86](cube/install-x-86)
* [Configure Ethernet (incomplet)](https://wiki.labriqueinter.net/doku.php?id=howto:parametrer_une_brique_avec_un_connecteur_usb-ethernet_au_lieu_d_un_connecteur_usb-wifi)
* [wiki de la briqueinter.net](https://wiki.labriqueinter.net/doku.php)
* [un pad étendu sur la brique](https://pad.lqdn.fr/p/brique-formation)


# Monter un disque dur externe

- trouver l'UUID de votre disque, connecter le disque puis: `ls /dev/disk/by-uuid/` 
- dans `/etc/fstab`, ajouter: `UUID=<uuid-du-disque> /mnt/<mon-point-de-montage> ext4 defaults,nofail,x-systemd.device-timeout=5 0 2`


# Cartes mémoire

![Microsdspeedtable](/uploads/cube/microsdspeedtable.png "Microsdspeedtable")

Source : [wikipedia en anglais](https://en.wikipedia.org/wiki/Secure_Digital)


```
16Gb Kingston Gold SDHC Class 10 UHS Class 3 on a LIME1
$ sudo dd if=/dev/zero of=test bs=1048576 count=512
536870912 bytes (537 MB) copied, 23.3035 s, 23.0 MB/s


16Gb Transcend Premium 400x SDHC Class 10 UHS Class 1 on a LIME1
$ sudo dd if=/dev/zero of=test bs=1048576 count=512
536870912 bytes (537 MB) copied, 58.7851 s, 9.1 MB/s
536870912 bytes (537 MB) copied, 138.966 s, 3.9 MB/s

16Gb Kingston Industrial SDHC Class 10 UHS Class 1 on a LIME1
$ sudo dd if=/dev/zero of=test bs=1048576 count=512
512+0 records in
512+0 records out
536870912 bytes (537 MB) copied, 47.1847 s, 11.4 MB/s
```

https://en.wikipedia.org/wiki/Secure_Digital#Speed_class_rating