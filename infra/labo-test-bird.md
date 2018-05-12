<!-- TITLE: Labo Test Bird -->
<!-- SUBTITLE: Notre labo de bird -->

![Labo](/uploads/labo.jpg "Labo")

## Acces labo
Vous devez vous connecter à l'ip 178.63.130.148 en ssh puis rebondir d'host en host.
Un acces proxmox est aussi possible il faut demander à tharyrok.

## Vlan
| ID | Name | IP | Host |
|---|---|---|---|
| 10 | Routage | 10.42.10.0/24 | Troll, Orval, GW_Orval, GW_Troll, VPN_Orval, VPN_Troll |
| 20 | Neutrinet | 10.42.20.0/24 | GW_Orval, GW_Troll, VPN_Orval, VPN_Troll, VM_Web |
| 30 | Proxmox | 10.42.30.0/24 | Troll, Orval |
| | | | |
| 40 | Internet | 10.42.40.0/24 | WAN_TERNET, WAN_I3D |
| 50 | I3D | 10.42.50.0/24 | WAN_I3D, Troll, Orval |

## AS
| ID | Name |
|---|---|
| 64496 | Internet |
| 64500 | I3D |
| 64510 | Neutrinet |

## IP
| Host | IP | GW |
|---|---|---|
| WAN_TERNET | 178.63.130.148/32 | 178.63.43.60 |
| | 10.42.40.1/24 | |
| | | |
| WAN_I3D | 10.42.40.2/24 | BGP 10.42.40.1 |
| | 10.42.50.1/24 | |
| | | |
| Troll | 10.42.50.14/24 | BGP10.42.50.1 |
| | 10.42.30.14/24 | |
| | 10.42.10.14/24 | |
| | | |
| Orval | 10.42.50.13/24 | BGP 10.42.50.1 |
| | 10.42.30.13/24 | |
| | 10.42.10.13/24 | |
| | | |
| GW_Orval | 10.42.10.3/24 | BGP 10.42.10.13 |
| | VRRP 10.42.20.1/28 | |
| | | |
| GW_Troll | 10.42.10.4/24 | BGP 10.42.10.14 |
| | VRRP 10.42.20.1/28 | |
| | | |
| VPN_Orval | 10.42.20.3/28 | 10.42.20.1/28 |
| | 10.42.10.23/24 | |
| | | |
| VPN_Troll | 10.42.20.4/28 | 10.42.20.1/28 |
| | 10.42.10.24/24 | |
| | | |
| VM_Web | 10.42.20.7/28 | 10.42.20.1/28 |
| | | |

## Routing (bird)
| Host | Sessions BIRD | Spécificités de ces sessions |  TINC |
|---|---|---|---|---|
|Orval et Troll| Avec le transitaire (I3d) |Chacun annonce les routes qu'il apprend des gateways. On laisse i3d choisir celui qu'il veut parmi les deux. [side note: on pourrait faire du VRRP là aussi] |Tinc est configuré sur un bond sur les interface eth1 de chacune des ces machines|
|Orval et Troll| Avec Gateway 1 et Gateway 2 chacun + Avec le transitaire (i3d)|Il prend les routes depuis les GW en filtrant pour ne prendre que celle issue de notre bloc d'IP. Comme seul la gateway qui est master VRRP a une route vers nos IP (le master VRRP a deux virtuals IP (10.10.10.1 et 80.67.181.1) ainsi qu'une route virtuelle vers 80.67.181.0/24 via 80.67.181.1), seule celle-ci annonce sa route aux deux hyperviseur. ||
|Gateways 1 et 2|avec les hypversiveux|voir plus haut. Chacun a une session BGP avec les deux hyperviseurs ET ils annoncent ce qu'ils ont dans leur kernel (filtré pour laisser passer que les /24) afin qu'ils annoncent le /24 que keepalived (le daemon VRRP) leur met quand il deviennent master. En slave, ils n'annoncent rien.|Ils sont dans le TINC aussi, via le bridge de leur hyperviseur respectif. |
|Gateways 1 et 2| VMs VPN| Les VMs VPN annoncent à chacune des GW les routes des sessions VPN dont elles ont la charge (en /32 puis en /56 + en /128 en IPv6 quand le temps viendra de tester ça). Par contre, elles ne prennent pas en compte les routes annoncées par les GW, met on 80.67.181.1 comme GW par défaut |Non, elle ne sont pas dans le TINC.|
|Chimay (la VM chez des amis)|Le transitaire de là-bas qui parlerait BGP|Par défaut, il y aurait un gros prepend pour faire croire au transitaire qu'il y a plein de hop et qu'il ne faut pas passer par ici, sauf si le reste est down. Mais on annonce quand même tout notre /24 en static et en temps normal on le route vers 10.10.10.1 via le TINC. En cas de plantage de notre infra chez i3d, et donc s'il devient master VRRP (alors qu'il a une priorté très basse), il route vers son service VPN (ne pourra fonctionner qu'une fois qu'on est basé sur radius et non plus ISP-ng)|Oui, il est dans TINC dans le Vlan 10|
|||||