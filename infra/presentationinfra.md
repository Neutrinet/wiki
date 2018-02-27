<!-- TITLE: Présentation -->
<!-- SUBTITLE: Nouvelle infra et configurations -->

Cette page va retracer la mise en place de la nouvelle infra de neutrinet ainsi que les differentes configuration.

L'objectif voulu avec cette infra est la remise en place du vpn en un minimum de temps.

# Matériel et lieux

## i3d
Nous avons un contrat de collocation chez i3d à Rotterdam qui nous fournis 1U avec 2 liens à 50Mbps 95% (sur le mois) et une session bgp.

Dans le rack ce cahche 2 Supermicro X7DBT avec un intel E5345 et 20G de ram (c'est "troll" et "orval").

Actuellement nous allons commander un lien pour le "comptoir" qui est un edge route lite 3 qui fournis l'acces au carte ipmi.

## Gitoyen
Nous avons une vm chez eux qui s'apelle "chimay" qui fournis une session bgp.

## The futur
Nous cherchon actuellement un moyen d'avoir un 3e serveur pour herbeger le vpn. Cet endroit doit pouvoir herberger un proxmox avec minimum 8G de ram et une session bgp.

# Les outils
Voici une liste non exosive d'outils que nous utilisons : 

 - Proxmox (gestion des vm)
 - Open vSwitch (switch logicelle)
 - Bird (annonce bgp)
 - Mattermost (communication interne)
 - Ispng (notre outils maison pour la gestion client vpn)
 - Openvpn (le vpn)
 - Tinc (Un autre vpn)
 - GlusterFS

# Le réseau
## Les vlan
| Vlan | IP | utilisation | Membre |
|---|---|---|---|
|  | 192.168.100.0/30 | Reseau entre troll & orval | Troll, Orval |
|  | 192.168.69.0/30 | Lien ipmi troll | Carte ipmi Troll, Comptoir |
|  | 192.168.69.4/30 | Lien ipmi orval | Carte ipmi Orval, Comptoir |
| 10 | 10.10.10.0/24 | Reseau de routage | Troll, Orvall, VM GW, Chimay |
| 10 | fd91:ce0e:cee4:10::/64 | | |
| 80 | 80.67.181.0/28 | Reseau server de neutrinet | Tout les VM |
| 80 | 2001:913:1000::/40 | | |
| 42 | 172.16.42.0/24 | Reseau interne | Tout les VM, Troll, Orvla, Chimay |
| | 192.168.200.0/24 | Reseau tinc | Troll, Orval, Chimay |

## Schema physique

![Reseau Phy](/uploads/infra/reseau-phy "Reseau Phy"){.align-center}

## Schema VM

![Reseau Vpn](/uploads/infra/reseau-vpn.png "Reseau Vpn"){.align-center}

## Explication
### Le swicht
Nous utilisont Open vSwitch pour créer un méga switch avec tout les vlan entre tout nos hôtes. Un lien GRE existe entre Troll et Orval. Un lien GRE dans un Tinc L2 existe entre Orval et Chimay et entre Troll et Chimay.

### Le routage
Troll et orval annonce leur gateway à la VM GW, celon l'hote qui herbegr la route annoncé est pondéré.
Cela permet de migré la vm d'un hote à l'autre et de perdre un hote sans que la VM GW perde une route par default.

La VM GW annonce les réseaux de Neutrinet à l'hôte qui l'hérberge et ce dernier l'annonce à i3d qui l'annonce au reste du monde.

# Les serveurs
## Physiques

| Nom | OS | IP | VLAN | Utilité |
|---|---|---|---|---|
| [Troll](phy/troll) | Proxmox 4 | 5.200.2.14 - 2a00:1630:10:1012::2| | Hote pour les vm |
| | | 10.10.10.14 - fd91:ce0e:cee4:10::59| 10 | |
| | | 172.16.42.114 | 42 | |
| | | 192.168.200.14 | | Interconnexion tinc |
|---|---|---|---|---|
| [Orval](phy/orval) | Proxmox 4 | 5.200.2.13 - 2a00:1630:10:1012::1| | Hote pour les vm |
| | | 10.10.10.13 - fd91:ce0e:cee4:10::13| 10 | |
| | | 172.16.42.113 | 42 | |
| | | 192.168.200.13 | | Interconnexion tinc |
|---|---|---|---|---|
| [Chimay](phy/chimay) | Debian 8 | 80.67.163.59 - 2001:910:0:41::59| | Anonnce route neutrinet dans le reseau Gitoyen |
| | | 10.10.10.59 - fd91:ce0e:cee4:10::59 | 10 | |
| | | 172.16.42.159 | 42 | |
| | | 192.168.200.59 | | Interconnexion tinc |
|---|---|---|---|---|
| [Comptoir](phy/comptoir) | OpenBSD 5 | 5.200.2.17 - 2a00:1630:10:1012::3| | Bastion pour les carte ipmi|
|---|---|---|---|---|

## Les VMs

| Nom | OS | IP | VLAN | Utilité |
|---|---|---|---|---|
| [GW](vm/gw) | Debian 8 | 80.67.181.1 - 2001:913:1000::1 | 80 | GW pour le reseau neutrinet & annonce des routes|
| | | 10.10.10.1 - fd91:ce0e:cee4:10::1| 10 | |
| | | 172.16.42.1 | 42 | |
|---|---|---|---|---|
| [DNS](vm/dns) | Debian 8 | 80.67.181.2 - 2001:913:1000::2 | 80 | Serveur reverse dns pour les ip de Neutrinet|
| | | 172.16.42.2 | 42 | |
|---|---|---|---|---|
| [VPN](vm/vpn) | Debian 8 | 80.67.181.3 - 2001:913:1000::3 | 80 | IspNG et OpenVpn|
| | | 172.16.42.3 | 42 | |
|---|---|---|---|---|
| [WEB](vm/web) | Debian 8 | 80.67.181.4 - 2001:913:1000::4 | 80 | Seveur web de tout nos services|
| | | 172.16.42.4 | 42 | |
|---|---|---|---|---|
| [MONITOR](vm/monitor) | Debian 8 | 80.67.181.5 - 2001:913:1000::5 | 80 | Serveur de monitoring et supervision|
| | | 172.16.42.5 | 42 | |
|---|---|---|---|---|
| [MAIL](vm/mail) | Debian 8 | 80.67.181.6 - 2001:913:1000::6 | 80 | Serveur mail de Neutrinet|
| | | 172.16.42.6 | 42 | |
|---|---|---|---|---|
| [SUPPORT](vm/support) | Debian 9 | 80.67.181.7 - 2001:913:1000::7 | 80 | Serveur pour le support|
| | | 172.16.42.7 | 42 | |
|---|---|---|---|---|

# Les scenari
## Perte d'un hote
Toutes les vm sont basculé sur l'hote qui est up.
## Perte des 2 hôtes
C'est la cata
# La fin