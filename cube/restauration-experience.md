<!-- TITLE: Restauration d'une Brique -->
<!-- SUBTITLE: Retours sur une tentative de restauration -->

# Préambule

Soit une Brique Lime2 avec une microSD de 64 Go. Celle-ci pose [des problème et semble corrompue](https://forum.yunohost.org/t/brique-foireuse-carte-sd-autre/4780).

Comme j'ai une sauvegarde Yunohost de moins d'une semaine, je décide de mettre une nouvelle carte microSD (une 32 Go) dans ma Brique et restaurer le système.


## [Facultatif] Image disque de la carte microSD

Malgré que ma carte microSD soit corrompue, j'ai pu en faire une image disque sur mon ordinateur avec [cette commande-ci](https://wiki.neutrinet.be/cube/microsd#cloner) : `sudo dd if=/dev/[carte_microSD] | gzip -c > clone.img.gz` 

> [?] L'image compressée fait 51,6 Go au lieu de 64, mais pour pouvoir la monter, il faut la décompresser, du coup je me retrouve avec 51,6 + 64 = 115,6 Go d'espace disque nécessaire. Finalement, une commande pour avoir un simple .img de la taille de la carte SD serait plus commode. (Je ne connais pas cette commande.)
{.is-info}

Pour la monter directement sur l'ordinateur, j'ai suivi [la procédure suivante](https://www.linuxquestions.org/questions/linux-general-1/how-to-mount-img-file-882386/#post4365399). Ayant dû décompresser mon fichier clone.img.gz, je me suis retrouvé avec l'image dans `clone.img/data` (le fichier data faisant ~ 64 Go). Le chemin de l'image à donner pour la monter est donc `clone.img/data`.

Ce qui m'a permis d'aller récupérer des fichiers directement sur ma carte microSD (ce qui, en théorie ne devrait pas être nécessaire).


## Procédure de restauration d'une installation de Yunohost

Pour restaurer Yunohost, la procédure officielle est :
- Faire l'installation via `install-sd.sh` (**Ne pas faire la post-installation**.)
- Copier l'archive de sauvegarde dans `/home/yunohost.backup/archives/`
- Restaurer l'archive

À cela, quelques changements sont liés au fait que ma Brique est configurée pour fonctionner avec le VPN de Neutrinet :
- Faire l'installation via `install-sd.sh` (**Ne pas faire la post-installation**.)
- Copier l'archive de sauvegarde dans `/home/yunohost.backup/archives/`
- Restaurer l'archive
- Transférer le fichier `neutrinet.variables` sur la Brique
- Exécuter **une partie** du script `neutrinet.sh`


# Ma réinstallation…

## Installation

Sur mon ordinateur. Je mets la carte microSD que quand le script le demande.

```bash
$ wget https://repo.labriqueinter.net/install-sd.sh
$ chmod 0755 install-sd.sh
$ ./install-sd.sh -2
```

(Note : `-2` parce que c'est une Lime2.)
(Durée : ~ 8 minutes.)

Je place la carte microSD dans la Brique et je branche celle-ci.

## Restauration

```
$ ssh root@brique
	[Mot de passe par défaut : olinux]
	[La Brique propose de changer le mot de passe.]
root@brique:~# mkdir -p /home/yunohost.backup/archives/
root@brique:~# exit
$ scp /dernière_sauvegarde_de_ma_Brique.tar.gz root@brique:/home/yunohost.backup/archives/
```



-----


Facultatif

Je ne sais pas si c'est toujours nécessaire, mais j'avais souvent des erreurs de `locales` lors de la restauration de mes sauvegardes (voir plus bas), ceci permet d'éviter de polluer les logs avec ces erreurs :

```
ssh root@brique
root@brique:/# dpkg-reconfigure locales
```

Et, dans les menus, choisir `fr_FR.UTF-8` ou `fr_BE.UTF-8` et l'installer par défaut.


-----



Suite :

```
$ ssh root@brique
root@brique:~# yunohost backup restore dernière_sauvegarde_de_ma_Brique
```

(Durée : entre 1h30 et 2h.)

## Partie Neutrinet

Je récupère le fichier `neutrinet_variables` de mon ancienne Brique (dans `/root`).

Je récupére le fichier `neutrinet.sh` dont je modifie la fin en :

```
get_variables

# modify_hosts
# set_locales
# upgrade_system

# postinstall_yunohost
# create_yunohost_user
add_labriqueinternet_app_list
install_vpnclient
configure_vpnclient
# install_hotspot
# configure_hostpot
install_doctorcube
install_neutrinet_ynh

remove_dyndns_cron
restart_api

display_win_message
```

> J'avais fait un test en exécutant `modify_hosts` et `set_locales` avant la restauration de la sauvegardes pour éviter l'errer des `locale settings` mais ça n'a pas fonctionné. Je ne sais pas du coup s'il y a un moment où il faut tout de même exécuter `modify_hosts`. De même que je ne sais pas si tout ce qui s'exécute ici est indispensable.
{.is-info}

 
```
root@brique:~# exit
$ scp neutrinet_variables root@brique:/tmp
$ scp neutrinet.sh root@brique:/tmp
$ ssh root@brique
root@brique:~# cd /tmp
root@brique:/tmp# ./neutrinet.sh
```

(Durée : une petite dizaine de minutes.)

---
Dans les logs, j'ai :
```
Fri May  4 00:49:45 2018 ++ Certificate has key usage  00a0, expects 00a0
```
Ce qui subodore un certificat expiré…

```
root@brique:~# openssl x509 -in /etc/openvpn/keys/user.crt -enddate | grep notAfter
notAfter=Oct 16 15:34:07 2017 GMT
```

Alors que le certificat avait été renouvelé !

> [?] Le fichier `neutrinet_variaibles` contient toutes les informations nécessaires pour configurer le compte VPN chez Neutrinet, mais ces informations *ne sont pas à jour*. Je ne sais pas où seraient ces informations à jour.
{.is-info}

Normalement, l'application `neutrinet_ynh` devrait mettre à jour automatiquement le certificate, mais `yunohost app list -i` montre qu'elle n'est pas installée, hors, le script `neutrinet.sh` aurait dû l'installer. 

Donc :
```
root@brique:~# yunohost app install https://github.com/Neutrinet/neutrinet_ynh
```
---

## Finalisation

C'est fini, mais pour faire ça bien je mets à jour le système + les applications :

```
root@brique:~# yunohost tools update
root@brique:~# yunohost tools upgrade
```



# Parties coupées



## Erreur de `yunohost backup restore` à cause d'une sauvegarde corrompue

Lors de la première tentative de restauration, j'ai utilisé une sauvegarde corrompue (faite le jour où les problèmes ont commencés à apparaître sur ma Brique). `yunohost backupr restore dernière_sauvegarde_de_ma_Brique` donnait :

```
Attention : Le montage de l’archive de sauvegarde a échoué
Traceback (most recent call last):
	[Coupé]
zlib.error: Error -3 while decompressing: invalid block type
```

En essayant tout de suite après avec une autre sauvegarde, j'avais également un échec (je n'ai malheureusement pas noté le message d'erreur) ! Il a fallu que je recommence plusieurs fois pour que ça fonctionne avec une sauvegarde non corrompue (j'ai finalement trouvé plus sûr de recommencer l'installation depuis le début).

Par contre, j'ai toujours eu l'avertissement `Attention : Le montage de l’archive de sauvegarde a échoué`, mais ça n'a pas empêché les restaurations de se faire.



## Vérification des sauvegardes sur l'ordinateur

Sur l'ordinateur : `sudo tar -xf sauvegarde_de_la_brique.tar.gz`

Sur la toute dernière sauvegarde de ma Brique, ça a donné :

```
gzip: stdin: invalid compressed data--format violated
tar: Fin prématurée rencontrée dans l'archive.
tar: Fin prématurée rencontrée dans l'archive.
tar: Error is not recoverable: exiting now
```

… ce qui m'a assuré qu'elle était corrompue et que je n'en tirerai rien.

Sur la sauvegarde précédente, j'ai obtenu :

```
tar: apps/etherpad_mypads/backup/var/www/etherpad_mypads/var/minified_L2phdmFzY3JpcHRzL2xpYi9lcF9kaXNhYmxlX2Vycm9yX21lc3NhZ2VzL3N0YXRpYy9qcy9kaXNhYmxlX2Vycm9yX21lc3NhZ2VzLmpzP2NhbGxiYWNrPXJlcXVpcmUuZGVmaW5l.gz : open impossible: Nom de fichier trop long
tar: apps/etherpad_mypads/backup/var/www/etherpad_mypads/var/minified_L2phdmFzY3JpcHRzL2xpYi9lcF9kaXNhYmxlX2Vycm9yX21lc3NhZ2VzL3N0YXRpYy9qcy9kaXNhYmxlX2Vycm9yX21lc3NhZ2VzLmpzP2NhbGxiYWNrPXJlcXVpcmUuZGVmaW5l : open impossible: Nom de fichier trop long
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/1/V/76biab2+QN9-JPa2eliT : l'horodatage 2019-01-31 10:21:21 est situé 23632080.257580063 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/X/K/rEJ4r9WOezFcAoiYdRLC : l'horodatage 2019-01-31 10:21:20 est situé 23632079.257096372 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/Z/A/99emlbQjC0ng+R6SqgrU : l'horodatage 2019-01-31 10:21:20 est situé 23632079.256475035 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/L/6/fvUkzNwFXu4wDShqsZt0 : l'horodatage 2019-01-31 10:28:36 est situé 23632515.256221086 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/P/P/DpewzEiO8IBRjiseF7M9 : l'horodatage 2019-01-31 10:20:48 est situé 23632047.255849373 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/S/U/6o6dzHtlv4XY2uRmc8-E : l'horodatage 2019-01-31 10:21:19 est situé 23632078.25531713 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/2/K/Z6u6OmsvgJwc7ebtEOgh : l'horodatage 2019-01-31 10:21:19 est situé 23632078.255067397 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/5/E/kKTv1nXZZNLH-mfZ6PPq : l'horodatage 2019-01-31 10:28:36 est situé 23632515.254675655 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/J/2/kS0KKoNDGN-JrVNBsv6r : l'horodatage 2019-01-31 10:21:19 est situé 23632078.254257831 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/I/2/aS6RY8UNDoy4unYxCnsh : l'horodatage 2019-01-31 10:21:21 est situé 23632080.254018032 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/G/X/yZNbBRIFFz8iml0QPV-9 : l'horodatage 2019-01-31 10:21:20 est situé 23632079.253799437 secondes dans le futur.
tar: apps/wallabag2/backup/sources/var/cache/prod/pools/MXg66VSeE9/K/S/ovz6k9K5-k4kRYCGwXj2 : l'horodatage 2019-01-31 10:28:36 est situé 23632515.253254998 secondes dans le futur.
	[Coupé des centaines de lignes comme celles-là.]
tar: Arrêt avec code d'échec à cause des erreurs précédentes
```

Les erreurs ne concernent que Wallabag2 que j'avais installé mais pas employé ni configuré (et un peu oublié…) et etherpad_mypads dont j'avais une version assez ancienne et pas à jour et, surtout, dont j'ai une sauvegarde manuelle des quelques pads que j'employais.

Je suppose que c'est à cause de ces erreurs que `yunohost backup restore` me donne `Attention : Le montage de l’archive de sauvegarde a échoué` et que je peux l'ignorer.



## Erreur des `locale settings`

Lors de mes premiers essais, j'ai eu souvent cette erreur (affichage lors de la commande `yunohost backup restore` :

```
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LANG = "fr_BE.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
```

Répétée des dizaines de fois dans le terminal, polluant assez fortement les logs :

```
Attention : perl: warning: Setting locale failed.
Attention : perl: warning: Please check that your locale settings:
Attention : 	LANGUAGE = (unset),
Attention : 	LC_ALL = (unset),
Attention : 	LANG = "fr_BE.UTF-8"
Attention :     are supported and installed on your system.
Attention : perl: warning: Falling back to the standard locale ("C").
Attention : locale: Cannot set LC_CTYPE to default locale: No such file or directory
Attention : locale: Cannot set LC_MESSAGES to default locale: No such file or directory
Attention : locale: Cannot set LC_ALL to default locale: No such file or directory
```

[La solution est ici](https://forum.yunohost.org/t/warning-language-et-lc-all-lors-de-la-post-installation/3596) :

```
root@brique:/# dpkg-reconfigure locales
```

Et, dans les menus, choisir `fr_FR.UTF-8` ou `fr_BE.UTF-8` et l'installer par défaut.

Je ne sais pas à quoi est due cette erreur ni si elle est systématique.

> Si, par la suite, l'interface web de Yunohost apparait en anglais, vérifier que la bonne variante de langue (FR, BE…) est sélectionnée dans le navigateur. (Préférences > Langues > Choisir… dans Firefox.)



## Erreurs propres à ma configuration


### Hotspot [OK]

J'avais, après `yunohost backup restore`, les lignes suivantes :

```
Attention : Cloning into '/tmp/hotspot-restore-zw2SX'...
Attention : ./install: line 56: ynh_webpath_register: command not found
Attention : Failed to start ynh-hotspot.service: Unit ynh-hotspot.service failed to load: No such file or directory.
```

Comme je n'ai pour l'instant pas d'antenne Wifi sur ma Brique (j'ai prêté l'antenne temporairement), je ne sais pas si je dois tenir compte de cette erreur (c'est peut-être à cause de ça…)
Peut-être aussi est-ce pour ça qu'il existe une partie spécifique d'installation du hotspot wifi dans le script de Neutrinet (?).


### /conf/cron [OK]

J'avais, après `yunohost backup restore`, les lignes suivantes :

```
Attention : cp: cannot stat '/home/yunohost.backup/tmp/20180419-140039/conf/cron/.': No such file or directory
```

Effectivement, je n'ai pas ce dossier dans ma sauvegarde. Je suppose que ce n'est donc pas grave, mais que la restauration prévoit de restaurer ce dossier s'il contient des éléments.


### Le fichier X a été modifié manuellement et ne sera pas mis à jour [non résolu]

J'avais, après `yunohost backup restore`, les lignes suivantes :

```
Attention : Le fichier de configuration « /etc/nginx/conf.d/yunohost_panel.conf.inc » a été modifié manuellement et ne sera pas mis à jour
```

J'avais en effet modifié ce fichier manuellement il y a longtemps pour résoudre [un problème temporaire](https://forum.yunohost.org/t/erreur-nginx-dans-etc-nginx-conf-d-yunohost-panel-conf-inc/3105/11)

Comment permettre à Yunohost de le mettre à jour à l'avenir ? Remplacer le fichier par le plus récent ne résoud pas le problème. Tant qu'il n'y a pas de modifications de ce fichier, ce n'est pas gênant, mais sur le long terme (ou sur le principe), ça peut l'être.


### Nextcloud [pas entièrement résolu]

Au début de mes tests sur la Brique, j'avais réussi à activer l'option `backup_core_only` pour les sauvegardes de Nextcloud. Ce n'est pas une bonne idée, ou alors il faut sauver le dossier `/data` par ailleurs.

Voir [sur le forum de Yunohost](https://forum.yunohost.org/t/nextcloud-restauration-a-partir-dune-sauvegarde-backup-core-only-how-to-restore-with-a-backup-core-only-backup/4795).

Heureusement que j'avais toujours accès au dossier `/data` sur l'ancienne carte microSD de la Brique, que j'ai pu transférer sur la nouvelle.

Tout est à nouveau fonctionnel, mais remettre le setting `backup_core_only` à `0` ne fonctionne pas… Je n'ai donc pas résolu entièrement le problème.