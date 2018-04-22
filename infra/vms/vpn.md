<!-- TITLE: VM vpn -->
<!-- SUBTITLE: Infrastructure - vm vpn -->

# Présentation
Cette VM fait tourner le programme ISPng qui est utilisé pour le routage du VPN.

# Caractéristiques
Hostname   : vpn.neutri.net
IPv4        : 80.67.181.3 -- 172.16.42.3
IPv6        : 2001:913:1000::3
HDD         : 10 GB
CPU         : 4 vcpu
RAM         : 4096M
OS          : Debian 8 (jessie)

# Accès SSH
* bram
* jim
* tbalthazar
* tharyrok

# Logiciels
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
 
# Autre
## LDAP
[C'est ici que cela se passe](../software/ldap)

## ISPng
[C'est ici que cela se passe](../software/ispng)
