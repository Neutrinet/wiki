<!-- TITLE: VM gw -->
<!-- SUBTITLE: Infrastructure - vm gw -->

# Présentation
Cette vm permet le routage pour [AS204059](https://bgp.he.net/AS204059)

Elle établit une session BGP avec Troll & Orval qui, eux-mêmes, le font avec i3d.

# Caractéristiques
Hostname   : gw.neutri.net
IPv4        : 80.67.181.1 -- 10.10.10.1 -- 172.16.42.1
IPv6        : 2001:913:1000::1 -- fd91:ce0e:cee4:10::1
HDD         : 10 GB
CPU         : 4 vcpu
RAM         : 1024M
OS          : Debian 8 (jessie)

# Accès SSH
* bram
* jim
* tbalthazar
* tharyrok

# Logiciels
* bird
    * log : /var/log/bird.log
* bird6
    * log : /var/log/bird6.log
# Autre
## Voir le statut BGP
```
birdc show protocol
```
```
birdc6 show protocol
```

## IPv6 VPN
Une bizzarerie est en cours : bien qu'une route statique pour l'IPv6 est configurée dans bird (`/etc/bird/bird6.conf`), cette dernière n'est pas prise en compte. La solution a été de rajouter `up /bin/ip route add 2001:913:1f00::/40 via 2001:913:1000::3` dans `/etc/network/interfaces`.