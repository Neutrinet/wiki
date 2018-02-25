# Infrasctuture de neutrinet

## La vue globale
![Schéma principal](https://files.neutrinet.be/s/66Ekzz9oJ7PhXQo/download)

### Chez i3d
Troll et Orval sont 2 hyperviseur proxmox. Le port 8006 en http n'est accesible que sur le localhost.

Pour vous y connecter il faut utiliser cette commande : 
```
ssh -L8003:127.0.0.1:8003 troll.neutri.net
```

Remplacer troll par orval si troll est en default. Peut importe sur quel serveur vous accédé à l'interface web vous vérez le cluster au complet.

Il y a 2 point de stokage vm-vpn et vm-data.
Le vm-vpn contien que les disques avec la vm GW et VPN. Ce point de stokage est prévu pour être répliqué sur un 3eme serveur.
Le point de stokage vm-data contien les autre vm et est prévu pour être disponible que sur troll et orval.

### Chez gitoyent
Nous avons une vm, cette dernier est connecter au switch OpenSwitch et ne sert que pour des annonce bgp.

## Le routage

Il y a un reseau entre la vm GW et troll, chimay, orval. C'est le vlan 10 sur le switch (ipv4 : 10.10.10.0/24 & ipv6 :fd91:ce0e:cee4:10::/64).

### GW
```
IPV6 : fd91:ce0e:cee4:10::1
IPV4 : 10.10.10.1

Route annoncée : 
80.67.181.0/28
80.67.181.128/25 via 80.67.181.3

2001:913:1000::/40
2001:913:1f00::/40 via 2001:913:1000::3
```

### troll
```
IPV6 : fd91:ce0e:cee4:10::1
IPV4 : 10.10.10.1

Route annoncée :

default via 5.200.2.1 (juste pour la gw)
unreachable 80.67.181.0/24


default via 2a00:1630:10:1000::1 (juste pour la gw)
unreachable 2001:913:1000::/36
```

### orval
```
IPV6 : fd91:ce0e:cee4:10::1
IPV4 : 10.10.10.1

Route annoncée :

default via 5.200.2.1 (juste pour la gw)
unreachable 80.67.181.0/24


default via 2a00:1630:10:1000::1 (juste pour la gw)
unreachable 2001:913:1000::/36
```

### Annonce de la default route
Cela peut etonée que troll, chimay et orval annonce leur default route à la gw. En réalité cela permet de perdre un serveu et de toujours annoncée une route par default pour la gw.