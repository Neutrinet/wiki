<!-- TITLE: Vpn Problem -->
<!-- SUBTITLE: Fixing one kind of VPN issue -->

# Pour les soucis de VPN

Pour retrouver une brique sur un LAN quand le VPN marche pas ...

Sur un PC GNU/Linux :

```
$ sudo apt install arp-scan
$ wget https://repo.labriqueinter.net/install-sd.sh
$ chmod +x install-sd.sh
$./install-sd.sh -l
```

Ceci liste la ou les briques sur le LAN et affiche les adresse IP locales pour pouvoir s'y connecter.

```$ ssh root@ip de la brique```

Est-ce que tu sais faire un ping vers le monde extérieur? 

```# ping huit.re```

Si non, il faudrait commencer par 

```# yunohost firewall stop```

Et attendre 5/10 minutes et voir si tu peux te reco au domaine ou à l'IP publique.

Si non, mais que tu sais faire un ping vers le monde extérieur tout en ayant toujours pas de VPN

```
# wget https://gist.githubusercontent.com/tbalthazar/8d14dba0fdc76bf31f981c94781ec7a3/raw/76a0c8d43480756dd302ef50f37b2aabd28e3022/neutrinet-connectivity-fix.sh
# chmod +x neutrinet-connectivity-fix.sh
# ./neutrinet-connectivity-fix.sh
```