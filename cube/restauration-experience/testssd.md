<!-- TITLE: Tests de cartes SD -->

Tout ceci n'est pas forcément utile…

Comme j'ai pu, du coup, faire differents tests sur des cartes microSD, je mets ici les vérifications que j'ai faites.

## Tests 

J'ai testé différentes commandes et outils sur trois cartes microSD :

1. Une Transcend Premium 300x XC I de 64 Go. UHS Class 1 (U1)
	Une partition EXT4. Celle qui était dans ma Brique, épuisée.

2. Une SanDisk Ultra HC I de 32 Go. Class 10 (C10)
	Une partition EXT4. Très peu utilisée (seulement pour de temps en temps transférer des fichiers).

3. Une SanDisk Ultra PLUS HC I de 16 Go. UHS Class 1 (U1)
	Une partition FAT. Qui était dans un appareil Android, indétectable sur l'appareil, monte sur un ordinateur mais peut planter à la lecture ou la copie de certains fichiers.

D'après la [page Wikipedia anglaise](https://en.wikipedia.org/wiki/Secure_Digital#Speed_class_rating), les trois cartes ont donc une vitesse de lecture et d'écriture *minimale* semblable, soit 10 Mo/s.


Voici les différentes commandes et outils :

A. `fsck.ext4 /dev/[carteSD]`, d'après [https://wiki.neutrinet.be/cube/microsd#verifier] pour 1 et 2 ; `fsck.fat /dev/[carteSD]` pour la 3.

B. `badblocks -s -v -n /dev/[carteSD]`, d'après [https://help.endian.com/hc/en-us/articles/218146758-Verify-if-the-micro-SD-card-is-corrupt] ([notes et remarque](https://forum.yunohost.org/t/restauration-dune-brique-suite-a-une-carte-sd-corrompue/4780/3?u=ptrph)).

C. `f3write` et `f3write`, voir [https://fight-flash-fraud.readthedocs.io/en/latest/usage.html]

D. `f3probe`, voir aussi [https://fight-flash-fraud.readthedocs.io/en/latest/usage.html] (**Attention, détruit les données sur la carte.**)

Pour un simple test de vitesse en écriture directement sur la Brique, il y a [plus simple comme commande](https://wiki.neutrinet.be/cube/microsd#class-et-vitesse).

---

### A.1. **`fsck` sur carte 64 Go corrompue**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci voit la carte mais ne peut pas la monter (« can't read superblock »).
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

```
$ sudo fsck.ext4 /dev/mmcblk0p1
e2fsck 1.43.5 (04-Aug-2017)
/dev/mmcblk0p1 : récupération du journal
le drapeau needs_recovery n'est pas activé, mais le journal contient des données.
Exécuter quand même le journal<o>? oui
fsck.ext4: Unknown code ____ 251 lors de la récupération du journal de /dev/mmcblk0p1
fsck.ext4: impossible d'initialiser les drapeaux du superbloc sur /dev/mmcblk0p1


/dev/mmcblk0p1 : **ATTENTION : le système de fichiers contient encore des erreurs**
```

### A.2. **`fsck` sur carte 32 Go fonctionnelle**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci peut monter la carte.
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

```
$ sudo fsck.ext4 /dev/mmcblk0p1
e2fsck 1.43.5 (04-Aug-2017)
/dev/mmcblk0p1 : propre, 204914/1816416 fichiers, 5011775/7791488 blocs
```

### A.3. **`fsck` sur carte 16 Go corrompue**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci peut monter la carte (mais peut planter à la lecture ou copie de certains fichiers).
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

```
$ sudo fsck.fat /dev/mmcblk0p1
fsck.fat 4.1 (2017-01-24)
0x41: Dirty bit is set. Fs was not properly unmounted and some data may be corrupt.
1) Remove dirty bit
2) No action
? 2
There are differences between boot sector and its backup.
This is mostly harmless. Differences: (offset:original/backup)
  65:01/00
1) Copy original to backup
2) Copy backup to original
3) No action
? 3
/[coupé]
  Has a large number of bad entries. (17/21)
Drop directory ? (y/n) y
Reclaimed 204976 unused clusters (6716653568 bytes).
Free cluster summary wrong (199841 vs. really 404817)
1) Correct
2) Don't correct
? 2
Perform changes ? (y/n) n
/dev/mmcblk0p1: 873 files, 81119/485936 clusters
```

(Je ne corrige rien avant de tester les autres commandes.)
(Ajout : faire les réparation ne changera rien aux erreurs des autres commandes.)



### B.1. **`badblocks` sur carte 64 Go corrompue**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci voit la carte mais ne peut pas la monter (« can't read superblock »).
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

```
$ sudo badblocks -s -v -n /dev/mmcblk0
Vérification des blocs défectueux dans un mode non destructif de lecture-
écriture
Du bloc 0 au bloc 62389247
Vérification des blocs défectueux (test non destructif de lecture-écriture)
Test en cours avec un motif aléatoire : badblocks: Erreur d'entrée/sortie lors du test d'écriture de données, bloc 0
0
1
2
3
4
5
[…]
```

(Les chiffres défilent tout de suite, signe d'une carte corrompue, processus interrompu, ça ne sert à rien d'aller jusqu'au bout.)

### B.2. **`badblocks` sur carte 32 Go fonctionnelle**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci peut monter la carte.
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

```
$ sudo badblocks -s -v -n /dev/mmcblk0
Vérification des blocs défectueux dans un mode non destructif de lecture-
écriture
Du bloc 0 au bloc 31166975
Vérification des blocs défectueux (test non destructif de lecture-écriture)
Test en cours avec un motif aléatoire : 0.36% effectué, 1:01 écoulé. (0/0/0 erreurs)

Interrupted at block 111808

Interruption, nettoyage en cours
```

(Test très lent, interrompu manuellement mais possible de laisser tourner pour vérifier l'intégralité de la carte (? — voir si pertinent pour carte SD).)
(Interprétation des erreurs : lecture/écriture/comparaison.)


### B.3. **`badblocks` sur carte 16 Go corrompue**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci peut monter la carte (mais peut planter à la lecture ou copie de certains fichiers).
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

```
$ sudo badblocks -s -v -n /dev/mmcblk0
Vérification des blocs défectueux dans un mode non destructif de lecture-
écriture
Du bloc 0 au bloc 15558143
Vérification des blocs défectueux (test non destructif de lecture-écriture)
Test en cours avec un motif aléatoire : badblocks: Erreur d'entrée/sortie lors du test d'écriture de données, bloc 0
0 0.00% effectué, 0:38 écoulé. (0/0/0 erreurs)
1
2
3
4
5
[…]
60
61
62
63
0.00% effectué, 1:03 écoulé. (0/0/64 erreurs)

Interrupted at block 64

Interruption, nettoyage en cours
```

(Plus lent que pour la carte de 64 Go, les erreurs se font à la comparaison et pas à la lecture. Interrompu manuellement aussi.)


### C.1. **`f3write`/`f3read` sur carte 64 Go corrompue**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci voit la carte mais ne peut pas la monter (« can't read superblock »).
`lsblk` indique que la carte est sur `/dev/mmcblk0` (`/dev/mmcblk0p1` pour la partition).

Comme la partition ne monte pas, le test est impossible.


### C.2. **`f3write`/`f3read` sur carte 32 Go fonctionnelle**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci peut monter la carte.
Elle est montée sur `media/utilisateur/carteSD`.

```
$ f3write /media/utilisateur/carteSD
F3 write 7.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

Free space: 29.71 GB
Creating file 1.h2w ... OK!                         
Creating file 2.h2w ... OK!                         
Creating file 3.h2w ... OK!                          
Creating file 4.h2w ... OK!                          
Creating file 5.h2w ... OK!                          
Creating file 6.h2w ... OK!                          
Creating file 7.h2w ... OK!                          
Creating file 8.h2w ... OK!                          
Creating file 9.h2w ... OK!                          
Creating file 10.h2w ... OK!                          
Creating file 11.h2w ... OK!                          
Creating file 12.h2w ... OK!                          
Creating file 13.h2w ... OK!                          
Creating file 14.h2w ... OK!                          
Creating file 15.h2w ... OK!                          
Creating file 16.h2w ... OK!                          
Creating file 17.h2w ... OK!                          
Creating file 18.h2w ... OK!                          
Creating file 19.h2w ... OK!                          
Creating file 20.h2w ... OK!                          
Creating file 21.h2w ... OK!                          
Creating file 22.h2w ... OK!                          
Creating file 23.h2w ... OK!                          
Creating file 24.h2w ... OK!                         
Creating file 25.h2w ... OK!                         
Creating file 26.h2w ... OK!                         
Creating file 27.h2w ... OK!                         
Creating file 28.h2w ... OK!                         
Creating file 29.h2w ... OK!                         
Creating file 30.h2w ... OK!                        
Free space: 0.00 Byte
Average writing speed: 12.06 MB/s
```

```
$ f3read /media/utilisateur/carteSD
F3 read 7.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

                  SECTORS      ok/corrupted/changed/overwritten
Validating file 1.h2w ... 2097152/        0/      0/      0
Validating file 2.h2w ... 2097152/        0/      0/      0
Validating file 3.h2w ... 2097152/        0/      0/      0
Validating file 4.h2w ... 2097152/        0/      0/      0
Validating file 5.h2w ... 2097152/        0/      0/      0
Validating file 6.h2w ... 2097152/        0/      0/      0
Validating file 7.h2w ... 2097152/        0/      0/      0
Validating file 8.h2w ... 2097152/        0/      0/      0
Validating file 9.h2w ... 2097152/        0/      0/      0
Validating file 10.h2w ... 2097152/        0/      0/      0
Validating file 11.h2w ... 2097152/        0/      0/      0
Validating file 12.h2w ... 2097152/        0/      0/      0
Validating file 13.h2w ... 2097152/        0/      0/      0
Validating file 14.h2w ... 2097152/        0/      0/      0
Validating file 15.h2w ... 2097152/        0/      0/      0
Validating file 16.h2w ... 2097152/        0/      0/      0
Validating file 17.h2w ... 2097152/        0/      0/      0
Validating file 18.h2w ... 2097152/        0/      0/      0
Validating file 19.h2w ... 2097152/        0/      0/      0
Validating file 20.h2w ... 2097152/        0/      0/      0
Validating file 21.h2w ... 2097152/        0/      0/      0
Validating file 22.h2w ... 2097152/        0/      0/      0
Validating file 23.h2w ... 2097152/        0/      0/      0
Validating file 24.h2w ... 2097152/        0/      0/      0
Validating file 25.h2w ... 2097152/        0/      0/      0
Validating file 26.h2w ... 2097152/        0/      0/      0
Validating file 27.h2w ... 2097152/        0/      0/      0
Validating file 28.h2w ... 2097152/        0/      0/      0
Validating file 29.h2w ... 2097152/        0/      0/      0
Validating file 30.h2w ... 1482944/        0/      0/      0

  Data OK: 29.71 GB (62300352 sectors)
Data LOST: 0.00 Byte (0 sectors)
	       Corrupted: 0.00 Byte (0 sectors)
	Slightly changed: 0.00 Byte (0 sectors)
	     Overwritten: 0.00 Byte (0 sectors)
Average reading speed: 77.45 MB/s
```


### C.3. **`f3write`/`f3read` sur carte 16 Go corrompue**

Carte microSD dans adaptateur SD connecté directement à l'ordinateur. Celui-ci peut monter la carte (mais peut planter à la lecture ou copie de certains fichiers).
Elle est montée sur `media/utilisateur/carteSD`.

```
$ f3write /media/utilisateur/carteSD
F3 write 7.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

Free space: 6.10 GB
Creating file 1.h2w ... Write failure: Input/output error

WARNING:
The write error above may be due to your memory card overheating
under constant, maximum write rate. You can test this hypothesis
touching your memory card. If it is hot, you can try f3write
again, once your card has cooled down, using parameter --max-write-rate=2048
to limit the maximum write rate to 2MB/s, or another suitable rate.

Creating file 2.h2w ... Write failure: Input/output error
Creating file 3.h2w ... Write failure: Input/output error
Creating file 4.h2w ... Write failure: Input/output error
Creating file 5.h2w ... Write failure: Input/output error
Creating file 6.h2w ... Write failure: Input/output error
Creating file 7.h2w ... Write failure: Input/output error
Free space: 6.10 GB
Writing speed not available
```

Le test en lecture n'est pas possible du coup (`f3read` lit les fichiers écrits par `f3write` et compare leur intégrité).


### D.1. **`f3probe` sur carte 64 Go corrompue**

Carte microSD dans adaptateur SD connecté à l'ordinateur via un lecteur de carte SD en USB. L'ordinateur voit la carte mais ne peut pas la monter (« can't read superblock »).
`lsblk` indique que la carte est sur `/dev/sdc` (`/dev/sdc1` pour la partition).
**Attention, cet outil détruit les données sur la carte, être sûr de la manœuvre et ne pas se tromper de point de montage.**

```
$ sudo f3probe --destructive --time-ops /dev/sdc
F3 probe 7.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

f3probe: Can't open device `/dev/sdc': Read-only file system
```

Pour une raison que j'ignore, via l'adaptateur USB, cette carte est en lecture seule (elle ne l'est pas sans, et les autres cartes sur le même adaptateur ne le sont pas).


### D.2. **`f3probe` sur carte 32 Go fonctionnelle**

Carte microSD dans adaptateur SD connecté à l'ordinateur via un lecteur de carte SD en USB. L'ordinateur peut monter la carte.
`lsblk` indique que la carte est sur `/dev/sdc` (`/dev/sdc1` pour la partition).
**Attention, cet outil détruit les données sur la carte, être sûr de la manœuvre et ne pas se tromper de point de montage.**

```
$ sudo f3probe --destructive --time-ops /dev/sdc

F3 probe 7.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

WARNING: Probing normally takes from a few seconds to 15 minutes, but
         it can take longer. Please be patient.

Good news: The device `/dev/sdc' is the real thing

Device geometry:
	         *Usable* size: 29.72 GB (62333952 blocks)
	        Announced size: 29.72 GB (62333952 blocks)
	                Module: 32.00 GB (2^35 Bytes)
	Approximate cache size: 0.00 Byte (0 blocks), need-reset=no
	   Physical block size: 512.00 Byte (2^9 Bytes)

Probe time: 8'09"
 Operation: total time / count = avg time
      Read: 2.20s / 4815 = 458us
     Write: 8'04" / 4192321 = 115us
     Reset: 554.2ms / 1 = 554.2ms
```


### D.3. **`f3probe` sur carte 16 Go corrompue**

Carte microSD dans adaptateur SD connecté à l'ordinateur via un lecteur de carte SD en USB. L'ordinateur peut monter la carte (mais peut planter à la lecture ou copie de certains fichiers).
`lsblk` indique que la carte est sur `/dev/sdc` (`/dev/sdc1` pour la partition).
**Attention, cet outil détruit les données sur la carte, être sûr de la manœuvre et ne pas se tromper de point de montage.**

```
$ sudo f3probe --destructive --time-ops /dev/sdc

F3 probe 7.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

WARNING: Probing normally takes from a few seconds to 15 minutes, but
         it can take longer. Please be patient.

Bad news: The device `/dev/sdc' is damaged

Device geometry:
	         *Usable* size: 0.00 Byte (0 blocks)
	        Announced size: 14.84 GB (31116288 blocks)
	                Module: 16.00 GB (2^34 Bytes)
	Approximate cache size: 0.00 Byte (0 blocks), need-reset=no
	   Physical block size: 512.00 Byte (2^9 Bytes)

Probe time: 1'06"
 Operation: total time / count = avg time
      Read: 0us / 0 = 0us
     Write: 1'06" / 4096 = 16.3ms
     Reset: 0us / 0 = 0us
```

---

## Conclusion

`badblocks` permet de rapidement vérifier que la carte peut-être physiquement corrompue, alors que `fsck` vérifie avant tout le système de fichier (si je ne m'abuse et peut-être en simplifiant). Étant donné la structure des cartes SD, `badblocks` ne peut pas être fiable sur le nombre de blocs défectueux, mais peut donner un indice sur le type d'erreurs (lecture, écriture, comparaison).
F3 (`f3write`/`f3read`) permet plutôt de tester les performances et la fiabilité des cartes. Ce qui peut être utile avant installation (carte récupérée, retrouvée…), ou après un certain temps afin de voir si elle tient toujours le coup, ou pour vérifier qu'une carte neuve a les performances et la capacité annoncée (ce qui est la raison première de F3).
Comme `f3probe` peut détruire les fichiers contenus sur la carte, il n'est pas pertinent pour diagnostiquer une carte qui contient des données. Il peut être utile pour vérifier rapidement qu'une carte neuve n'est pas contrefaite ou s'assurer qu'elle soit bien défectueuse avant d'être jetée.
Comme les cartes SD s'usent, il est probablement sage de ne pas faire trop de tests non plus…
