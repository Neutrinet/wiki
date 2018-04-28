<!-- TITLE: Microsd -->
<!-- SUBTITLE: MicroSD memory cards, carte mémoire -->

# English
# Français
## Vérifier

Il est recommandé de faire un backup avant ou mieux encore un [clone](#Cloner) !

Éteindre la brique.

Sortir la carte MicroSD de son slot et la mettre dans un adaptateur MicroSD vers SDCard.

Mettre la carte dans un ordinateur équipé de GNU/Linux.

`$ lsblk` pour identifier la carte qui devrait s'appeler `sdb` ou `sdc` ou  `mmcblk0` et les partitions qui la compose `sdb1` ou `sdc1` ou `mmcblk0p1`.

`sudo fsck.ext4 /dev/sdb1` ou `sudo fsck.ext4 /dev/mmcblk0p1` pour vérifier l'intégrité de la table d'allocation et voir si elle contient des erreurs.

Si il y a des erreurs, vous pouvez tenter une réparation automatique `sudo fsck.ext4 -p /dev/sdb1` ou `sudo fsck.ext4 -p /dev/mmcblk0p1`.

## Class et vitesse

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

## Lecture seule

Carte SD en lecture seule?

En mettant la carte SD dans son adaptateur MicroSD to SD.
En s'assurant que le verrou de l'adaptateur est bien sur la position déverrouillée.
En mettant le tout dans un lecteur de carte SD sur un PC GNU/Linux.

`$ lsblk`

Pour retrouver la carte SD qui devrait se trouver par exemple sur /dev/mmcblk0 ou /dev/sdb.

`$ sudo hdparm /dev/mmcblk0 `

ou /dev/sdb ou tout autre chemin vers la carte SD

Le résultat devrait renvoyer quelque chose comme ça :


```text
/dev/mmcblk0:
 HDIO_DRIVE_CMD(identify) failed: Invalid argument
 readonly      =  0 (off)
 readahead     = 256 (on)
 geometry      = 490976/4/16, sectors = 31422464, start = 0
```


Et si readonly = 1(on) apparaît ... ça veut dire que la carte est en LECTURE SEULE ce qui la rend inexploitable.  Les données n'étant pas perdues elles pourront être lue et recopiées si nécessaire, voir [clonée](#cloner)

## Cloner

Cloner une carte vers un ficier compressé

- éteindre la brique depuis l'interface web ou la ligne de commande
- débrancher la brique
- sortir la carte micro SD
- mettre la carte dans le PC et la repérer avec la commande :

`$ lsblk`

Par exemple : /dev/mmcblk0

`$ sudo dd if=/dev/mmcblk0 | gzip -c  > clone.img.gz`

Ce qui créera un fichier clone.img.gz dans le dossier courrant.

`gunzip -c clone.img.gz | dd of=/dev/mmcblk0`

Ce qui copiera votre clone.img.gz sur une nuvelle carte.

Source : https://serverfault.com/questions/4906/using-dd-for-disk-cloning

:warning: Si votre nouvelle carte est plus petite, même de quelques Mb que la carte d'origine, il faudra [réduire la taille (en)](http://www.aoakley.com/articles/2015-10-09-resizing-sd-images.php) du clone.


# Nederlands