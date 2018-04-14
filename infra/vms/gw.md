<!-- TITLE: VM gw -->
<!-- SUBTITLE: Infrastructure - vm gw -->

# Présentation
Cette vm permet le routage pour AS204059

Elle etabli une session bgp avec troll & orval qui eu même le font avec i3d

# Caracteristique
Hostaneme   : gw.neutri.net
IPV4        : 80.67.181.1 -- 10.10.10.1 -- 172.16.42.1
IPV6        : 2001:913:1000::1 -- fd91:ce0e:cee4:10::1
HDD         : 10 GB
CPU         : 4 vcpu
RAM         : 1024M
OS          : Debian 8 (jessie)

# Acces ssh
* bram
* jim
* tbalthazar
* tharyrok

# logicielle
* bird
    * log : /var/log/bird.log
* bird6
    * log : /var/log/bird6.log
# autre
## Voire le status bgp
```
birdc show protocol
```
```
birdc6 show protocol
```

## ipv6 vpn
Une bazarie est en court bien qu'une route satic pour l'ipv6 soie config dans bird `/etc/bird/bird6.conf` cette dernier n'est pas pris en comte. La solution à été de rajouter `up /bin/ip route add 2001:913:1f00::/40 via 2001:913:1000::3` dans `/etc/network/interfaces`