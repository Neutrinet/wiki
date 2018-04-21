<!-- TITLE: Microsd -->
<!-- SUBTITLE: MicroSD memory cards, carte mémoire -->

# English
# Français
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

## Cloner

Cloner une carte vers un ficier compressé

- éteindre la brique depuis l'interface web ou la ligne de commande
- débrancher la brique
- sortir la carte micro SD
- mettre la carte dans le PC et la repérer avec la commande :

`$ lsblk`

par exemple : /dev/mmcblk0

`$ sudo dd if=/dev/mmcblk0 | gzip -c  > /image.img.gz`


Source : https://serverfault.com/questions/4906/using-dd-for-disk-cloning
# Nederlands