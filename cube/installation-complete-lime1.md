<!-- TITLE: Install d'une LIME1 -->
<!-- SUBTITLE: Exemple d'installation compléte d'une Lime 1-->

# Résumé :

* enregistrement du nom de domaine chez Gandi : ± 4 minutes
* déballage + montage : ± 4 minutes
* carte SD : ±4 minutes
* premier boot -> prête pour un post_install : ±8 minutes
* création compte VPN via https://api.neutrinet.be/ : ±8 minutes
* encodage variables + neutrinet.sh (hors updates) + copier/coller infos DNS chez le registrar + premier «vrais» reboot : ±38 min
* deuxième partie, updates : ± 36 minutes
* troisième partie, le fine tunning : ±12 min
* installation complète : 1h55 minutes

# Détails

```text
Lime1, 16Gb, Wifi Propriétaire
Carte SD : 16Gb Kingston Industrial SDHC Class 10 UHS Class 1
Image de https://repo.labriqueinter.net/  : labriqueinternet_A20LIME_latest_jessie.img.tar.xz   388M 2017-Sep-06 11:48
```

## nom de domaine
±4 minutes en version cochon, sans changer le propriétaire, un peu plus long en y faisant attention.

Ça se fait sur notre compte chez [Gamdi.net](http://gandi.net/)

## déballage & montage 
± 4 minutes

12h43 : déballage du hardware
12h49 : Brique montée, vissée et antenne branchée.

## carte microSD 
±4 minutes

:books: En suivant [ce guide](https://wiki.neutrinet.be/cube/install)

12h50 : début install carte SD
12h51 : ./install-sd.sh -s /dev/sdb -f labriqueinternet_A20LIME_latest_jessie.img.tar.xz -g labriqueinternet_A20LIME_latest_jessie.img.tar.xz.asc
12h54 : done

## premier boot 
Jusqu'à ce qu'elle soit prête pour un post_install : ±8 minutes

12h57 : branchement
12h59 : connecté en ssh, mot de passe changé, j'attends pour vois si elle a encore besoin de rebooter ou pas.
13h02 : toujours connecté, je vois 98.5% cpu pour dovecot/ssl-params, j'attends pour voir si elle a encore besoin de rebooter ou pas.
13h05 : elle fait rien d'autre que m'attendre maintenant :D

## création compte VPN 
Pour Netrinet via https://api.neutrinet.be/ : ±8 minutes

:books: En suivant [ce guide](https://wiki.neutrinet.be/vpn/commander)

13h52 : début enregistrement compte VPN pour le membre
14h00 : compte créé, fichiers contenant les certificats téléchargés

## neutrinet.sh
encodage variables + neutrinet.sh (sans les updates) + copier/coller infos DNS chez le registrar + premier «vrais» reboot : 38 min

17h44 : début de l'installation avec https://raw.githubusercontent.com/labriqueinternet/configuration_scripts/master/neutrinet.sh (ET la ligne upgrade COMMENTÉE)
17h50 : variables copiés ou encodées, l'installation commence

```yaml
…
Success! The application list yunohost has been fetched
Warning: Skipping migration 1 change_cert_group_to_sslcert...
update-rc.d: error: no runlevel symlinks to modify, aborting!
Warning: The configuration file '/etc/default/glances' is now managed by the service glances.
Success! The configuration has been updated for service 'glances'
…
Success! The configuration has been updated for service 'slapd'
Warning: It seems that you have already configured MySQL. YunoHost needs to have a root access to MySQL to runs its applications, and is going to reset the MySQL root password. You can find this new password in /etc/yunohost/mysql.
Error: Script execution failed: /usr/share/yunohost/hooks/conf_regen/34-mysql
Success! YunoHost has been configured
```

17h54 : mot de passe admin Yunohost encodé, la suite continue
18h04 : c'est le moment où le stdout part en escalier 


```text
…
create mode 100755 yunohost/hooks.d/90-vpnclient.tpl
 create mode 100755 yunohost/hooks.d/post_iptable_rules/90-vpnclient
Fetched 5,788 kB in 3s (1,489 kB/s)
																	 Selecting previously unselected package libnl-3-200:armhf.
(Reading database ... 40709 files and directories currently installed.)                      (Reading database ... 
																																			 Preparing to unpack .../libnl-3-200_3.2.24-2_armhf.deb ...
…

```

18h09 : c'est le moment où le stdout redevient correct


```text
…
Processing triggers for php5-fpm (5.6.33+dfsg-0+deb8u1) ...
																																			[master 46a51bd] committing changes in /etc after apt run
 17 files changed, 475 insertions(+), 1 deletion(-)
 create mode 100644 default/crda
 create mode 100644 default/hostapd
…
```

18h16 : VICTOIRE ! Your Cube has been configured properly.

18h16 : début modification enregistrements DNS chez Gandi 
18h17 : tiens? Il manque le DKIM
18h19 : yunohost domain dns-conf DOMAIN.EXT
18h20 : poursuite changement DNS Gandi
18h21 : facile, nouveau domaine donc copier/coller \o/
18h22 : ip a pour voir si le VPN est up : OK 
18h22 : # yunohost tools reboot -f 

## deuxième partie
updates : ±36 min

18h25 : reconnecté en ssh
18h26 : # yunohost --version
yunohost: 2.7.2
yunohost-admin: 2.7.2
moulinette: 2.7.2
ssowat: 2.7.2
18h27 : # ip a -> VPN toujours OK
18h27 : # yunohost tools update
18h30 : toujours connecté, je vois 99.5% cpu pour yunohost, j'attends… je sais que c'est hard pour elle… elle fait ce qu'elle peut… elle donne tout ce qu'elle a… 
18h32 : elle nous lache un gros pavé sur le stdout ... comme une grosse merde qui passait pas facilement ... mais c'est que le début … la suite arrive :D
18h33 : # yunohost tools upgrade
18h35 : ça download à du ±1500 kB/s (vpn) sur une xdsl à du ±4000kB/s (hors vpn)
18h36 : ça unpack dans la joie et l’allégresse…
18h42 : ça unpack dans la joie et l’allégresse, on en est au linux-image-3.16.0-4-armmp (3.16.51-2)…
19h01 : a fini l'upgrade, le VPN fonctionne toujours et c'est parti pour un second « vrais » reboot.
19h01 : # yunohost --version
yunohost: 
  repo: stable
  version: 2.7.10
yunohost-admin: 
  repo: stable
  version: 2.7.7
moulinette: 
  repo: stable
  version: 2.7.7
ssowat: 
  repo: stable
  version: 2.7.7

## troisième partie
le fine tunning : ±12 min
    
19h09 : connecté en ssh, 
19h09 : # ip a -> le VPN ne fonctionne pas
19h10 : # ping huit.re -> ping: sendmsg: Operation not permitted
19h10 : # yunohost firewall stop
19h12 : le VPN est up tout seul (rien fait d'autre que stopper le firewall)

Mais pour contourner ça (et d'autres soucis de résolution de nom ou de date en 1970)

19h12 : # wget https://gist.githubusercontent.com/tbalthazar/8d14dba0fdc76bf31f981c94781ec7a3/raw/76a0c8d43480756dd302ef50f37b2aabd28e3022/neutrinet-connectivity-fix.sh (+ ce qui est expliqué dans le script)
19h18 : # yunohost domain cert-install --no-checks -> le certificat LetsEncrypt est installé, youpie!
19h21 : les trois applications de bases sont accessible via le browser, l'installation est terminée et la demande de création d'un RDNS pour l'IP statique du VPN vient d'être faite.



