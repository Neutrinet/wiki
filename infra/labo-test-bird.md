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

## AS
| Host | IP | GW |
|---|---|---|
| WAN_TERNET | 178.63.130.148/32 | 178.63.43.60 |
| | 10.42.40.1/24 | |
| | | |
| WAN_I3D | 10.42.40.2/24 | 10.42.40.1 |
| | | |
| Troll | 10.42.40.14/24 | 10.42.40.1 |
| | 10.42.10.14/24 | |
| | | |
| Orval | 10.42.40.13/24 | 10.42.40.1 |
| | 10.42.10.13/24 | |
| | | |
| GW_Orval | 10.42.10.3/24 | GBP |
| | VRRP 10.42.20.1/28 | |
| | | |
| GW_Troll | 10.42.10.4/24 | BGP |
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
