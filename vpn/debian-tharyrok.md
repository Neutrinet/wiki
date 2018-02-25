# Config du vpn sur debian

Ma config est la suivante, si le vpn ne marche pas j'ai pas d'internet à la maison.
J'explique que la config du vpn, si la demande ce fait j'expliquerai pour avoir un dhcp, nat, ...

Mon reseau local est en 192.168.12.0/24, l'ip lan de ma bbox est 192.168.1.1.

## Installation openvpn
``` apt-get install openvpn```

## configuration openvpn
Voici la structure des fichiers : 
```
/etc/openvpn
├── client
│   ├── neutrinet
│   │   ├── auth
│   │   ├── ca.crt
│   │   ├── client.crt
│   │   └── client.key
│   └── neutrinet.conf
└── server

```

Dans neutrinet.conf j'ai : 
```
client
dev tun0

# On ne peut pas mettre le ndd de neutrinet ici car on va forcer les route plus tard.
remote 5.200.2.14
proto udp
port 1195

pull
nobind
dev tun
tun-ipv6
keepalive 10 120
comp-lzo adaptive
resolv-retry infinite

# Authentication by login
auth-user-pass /etc/openvpn/neutrinet/auth

# UDP only
explicit-exit-notify

# TLS
tls-client
remote-cert-tls server
ns-cert-type server
ca /etc/openvpn/neutrinet/ca.crt
cert /etc/openvpn/neutrinet/client.crt
key /etc/openvpn/neutrinet/client.key

# Logs
verb 4
mute 5
status /var/log/openvpn-client.status
log-append /var/log/openvpn-client.log

# Routing
route 0.0.0.0 0.0.0.0
#route-ipv6 ::/0
route-ipv6 2000::/3

# neutrinet
cipher AES-256-CBC
tls-version-min 1.2
auth SHA256
topology subnet

```

N'oublier pas d'activer le vpn au démarage : 
``` systemctl enbale openvpn@neutrinet ```

## Forcer les routes
Comme je vous l'ai dit plus haut ma config est que si le vpn neutrinet ne tourne pas j'ai pas internet.

dans /etc/network/interfaces j'ai : 
```
auto lo
iface lo inet loopback

auto eth1
iface eth1 inet static
	address 192.168.12.254
	netmask 255.255.255.0

iface eth1 inet6 static
	address #Votre IpV6 de neutrinet#::1
	netmask 64

auto eth0
iface eth0 inet static
	address 192.168.1.20
	netmask 255.255.255.0
	pre-up echo 1 > /proc/sys/net/ipv6/conf/$IFACE/disable_ipv6
	up route add -net 5.200.2.14 netmask 255.255.255.255 gw 192.168.1.1
	down route del -net 5.200.2.14 netmask 255.255.255.255 gw 192.168.1.1

```

## Nat pour la bbox

Ha oui je vous ai dit que je parlerai pas de nat, bon il faut quand même quelque ligne d'iptables sinon vous n'aurez vraiment rien de fonctionnel.

Du coup je vous balance mes ligne mais sans trop vous expliquer.

```
iptables -A POSTROUTING -s 192.168.12.0/24 -d 192.168.1.1/32 -o eth0 -j MASQUERADE 
iptables -A POSTROUTING -s 192.168.12.0/24 -d 5.200.2.14/32 -o eth0 -j MASQUERADE
iptables -A POSTROUTING -s 192.168.12.0/24 -o tun0 -j MASQUERADE

```

