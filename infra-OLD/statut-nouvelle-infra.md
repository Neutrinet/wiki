# Statut nouvelle infra

## Problèmes rapportés

- date: description - @rapporteur


## Problèmes connus

- rien pour l'instant
 
## Problèmes résolus

- 25/04/2017: @nonooo ~~impossible de me connecter au VPN pour l'instant voici les logs de mon serveur. est-ce que je suis le seul?~~
 
~~Apr 25 16:15:39 	openvpn 	25692 	SIGUSR1[soft,ping-restart] received, process restarting~~

~~Apr 25 16:15:39 	openvpn 	25692 	[UNDEF] Inactivity timeout (--ping-restart), restarting ~~

~~Apr 25 16:15:26 	openvpn 	95250 	UDPv4 link remote: [AF_INET]80.67.181.3:1194~~

~~Apr 25 16:15:26 	openvpn 	95250 	UDPv4 link local (bound): [AF_INET]78.29.203.254~~

~~Apr 25 16:15:26 	openvpn 	95250 	NOTE: the current --script-security setting may allow this configuration to call user-defined scripts~~

~~Apr 25 16:15:26 	openvpn 	95250 	WARNING: No server certificate verification method has been enabled. See http://openvpn.net/howto.html#mitm for more info.~~

~~Apr 25 16:15:26 	openvpn 	95047 	WARNING: file '/var/etc/openvpn/client1.up' is group or others accessible~~

~~Apr 25 16:15:26 	openvpn 	95047 	library versions: OpenSSL 1.0.1s-freebsd 1 Mar 2016, LZO 2.09~~

~~Apr 25 16:15:26 	openvpn 	95047 	OpenVPN 2.3.11 amd64-portbld-freebsd10.3 [SSL (OpenSSL)] [LZO] [MH] [IPv6] built on May 16 2016 ~~

- 2017-03-20: ~~depuis ma brique, pas moyen d'atteindre neutrinet.be ou chat.neutrinet.be par contre, j'arrive à atteindre d'autres sites dans l'iprange de neutrinet (par ex https://vrac.browny.pink. traceroute: https://paste.yunohost.org/iqesahocev.md~~
  - ~~par @tbalthazar~~
  - ~~@tharyrok est dessus, impossible à reproduire pour le moment.~~
  - ~~2017-03-21: résumé: pas possible d'attenindre la VM Web quand on est derrière le VPN (autrement, ça marche)~~
- 2017-03-20: ~~mattermost semble planter et ne pas redémarrer tout seul~~
  - ~~par @tbalthazar~~
  - ~~@tbalthazar a mis à jour mattermost (dernière version, 3.7.2), pingez moi par email si https://chat.neutrinet.be est inaccessible~~
 - 2017-03-21: ~~La vm web à été installer sur le nouveau driver, la vm gw & vpn sur l'ancien driver. Ce driver ecrit mal sur le disque dur. Depuis le debut j'ai (tharyrok) ecarter toutes les pistes sauf celle là. Une réinstallation de c'est 2 vm est nécésaire et une coupe du vpn le temps de bascuelement sur les nouvelle vm. Cela devrais résoudre le soucis que les membres connectées au vpn ne puissent accédé au site web.~~
- 26/03/17 : ~~Reinstall des vm gw & vpn. Les client vpn peuvent accédé au vm de la nouvelle infra~~
  - ~~Par @tharyrok~~

## Downtime/maintenance plannifiée

- rien pour l'instant
