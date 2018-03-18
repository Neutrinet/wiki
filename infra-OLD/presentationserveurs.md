<!-- TITLE: Présentation des serveurs -->

# Serveurs physiques

Nous avons deux serveurs physiques, auxquels s'ajoutera un micro-serveur (board type raspi) pour de l'accès à distance, hébergé chez [i3d](https://www.i3d.net/contact/) à Rotterdam:

| Nom serveur        |       ipv4 publique      |   ipv6 publique                          | ipv4 privée Proxmox | ipv4 privée VM's
| ----------------   |  -------------  | ---------|-----------|----|
|  troll.neutri.net  |    5.200.2.14   |  2a00:1630:10:1012::2   | 192.168.100.2 |10.10.10.14|
|  orval.neutri.net  |    5.200.2.13   |  2a00:1630:10:1012::3   |192.168.100.3|10.10.10.13|
|comptoir.neutri.net |    5.200.2.17   |  2a00:1630:10:1012::4   |NA|NA|

Les deux serveurs sont connectés entre eux par un câble UTP sur les interfaces réseau et pourraient être connectés via l'interface fiber channel qu'ils ont si nécessaire. 

L'accès physique aux serveurs est lié à une [inscription auprès d'i3d](https://customer.i3d.net/controlpanel/) avec nom, plaque de voiture, etc... (partie "Colocation", option Whitelist)

Mot de passe connu de Tharyrok, Bram, Wannes et e-Jim au moins. 

Une fois sur place, il est nécessaire de se rendre dans le local des gardes de l'entrée pour recevoir les cartes d'accès.

# Système et gestion

## Gestion

Proxmox dispose d'une interface web disponible sur https://\<nom-du-serveur\>:8006 MAIS le firewall bloque tous les flux ne venant pas du serveur lui-même, du coup, il faut faire, sous linux, quelque chose comme:

`ssh -D8181 <nom-du-serveur>`

avant de se connecter et configurer son navigateur pour utiliser un proxy socks sur localhost:8181.

Il est possible de se connecter en forwardent le port 8006 : 

`ssh -L8006:127.0.0.1:8006 <nom-du-serveur>`

Et du coup dans le navigateur c'est l'adresse : https://127.0.0.1:8006/


[Tuto plus complet sur l’utilisation de l’option D de ssh, auquel on peut adjoindre -f -N si on veut éviter d’avoir un shell sur la machine et si on veut que ça tourne en daemon](https://ma.ttias.be/socks-proxy-linux-ssh-bypass-content-filters/)

Une fois sur l'interface web, il est possible d'avoir accès à un shell sur une machine directement en double-cliquant dessus. 
Il est également possible de forcer une re-démarrage, d'arrête une VM etc.. juste en cliquant droit sur la VM en question et en choissant l'option adéquate.

## Configuration

Les deux Proxmox sont configuré en cluster, afin de permettre leur redondance.

Pour assurer l'hébergement des VM-mêmes, deux pools glusterfs ont été mis en place: un volume vm-data et un volume vm-vpn, chacun monté dans / data.
Une priorité est accordée au volume vm-vpn (Jim: je sais pas comment - j'en trouve pas la trace dans la conf)

La continuation d'une VM quand un noeud du cluster plante est assuré par le module de haute-disponibilité (HA - high availability) de Proxmox

Les VM sont connectées sur un switch virtuel openswitch sur lequel il y a plusieurs vlan: le VLAN 42 pour la communication inter-vms, le vlan 80 pour les IP neutrinet (80.67.181.0/28 pour les VM), et le vlan 10 pour BGP (et donc la communication avec i3d)

# Serveurs Virtuels

Il y a actuellement 6 vm qui tournent sur les hyperviseurs

|Nom | Numéro de VM | IPv4 | IPv6 |IPv4 privée VM (VLAN 42) |Utilité |
|----|--------------|------|------|--------|--------|
|guizmo34.neutri.net|100|80.67.181.14|2001:913:1000::14||VM mise à disposition de gizmo34 (ex-gitoyen). Fourni entre autre [un smokeping](http://duvel.jesuisfrancobelge.eu/cgi-bin/smokeping.cgi)|
|web.neutri.net|102|80.67.181.4|2001:913:1000::4|172.16.42.4| Serveur web - sert tous nos services (site, nextcloud,...)|
|dns.neutri.net|105|80.67.181.2|2001:913:1000::2|172.16.42.2| Serveur DNS, ne sert que les zones reverse pour l'instant (le DNS forward est hébergé par Gandi)|
|monitor.neutri.net|104|80.67.181.5|2001:913:1000::5|172.16.42.5| Serveur de monitoring - pas encore up|
|gw.neutri.net|106|80.67.181.1|2001:913:1000::1|172.16.42.1| Routeur - annonce nos route et sert de passerelle vers le net pour toutes les IP Neutrinet|
|vpn.neutri.net|107|80.67.181.3|2001:913:1000::3|172.16.42.3| Gère le VPN, rien que ça...|

