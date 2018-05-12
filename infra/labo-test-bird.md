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
