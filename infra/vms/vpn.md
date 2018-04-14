<!-- TITLE: VM vpn -->
<!-- SUBTITLE: Infrastructure - vm vpn -->

# Présentation
Cette vm fait tourné le programe ispng qui est utiliser pour le routage du vpn

# Caracteristique
Hostaneme   : vpn.neutri.net
IPV4        : 80.67.181.3 -- 172.16.42.3
IPV6        : 2001:913:1000::3
HDD         : 10 GB
CPU         : 4 vcpu
RAM         : 4096M
OS          : Debian 8 (jessie)

# Acces ssh
* bram
* jim
* tbalthazar
* tharyrok

# logicielle
* nginx
* openvpn
* postgres
* openldap
* isp-NG

## nginx (vhost)
 * vpn.neutrinet.be
 
## postgres (database)
 * ispng
 * ispng-old
 * ispng_beta

## openvpn (port)
 * 1194 tcp
 * 1194 udp
 * 1195 tcp
 * 1195 udp
 
# autre
## ldap
[C'est ici que cela ce passe](../software/ldap)
## ispng
[C'est ici que cela ce passe](../software/ispng)
