<!-- TITLE: New Vpn -->
<!-- SUBTITLE: A quick summary of New Vpn -->

# New vpn
## LDN
https://wiki.ldn-fai.net/wiki/OpenVPN_Server_Tutorial
```
* on utilise uniquement des certificats pour l'auth ;

* on a un CA pour le(s) certificat(s) serveur(s) et un autre CA
  disctinct pour les certirficats clients ;

* on récupère le DN du certificat client pour sourcer un fichier bash
  du même nom qui contient les infos pertinentes pour l'utilisateur :

  # IPv4 interonnection address:
  IP4="198.51.100.1"
  # IPv6 interconnection address:
  IP6="2001:DB8:2::1"
  # IPv6 delegated prefix:
  PREFIX="2001:DB8:1::/48"

* on a un script qui utilise ces variables pour générer le fichier de
  conf attendu par OpenVPN (et ajouter une route qui va bien sur le
  serveur OpenVPN).

C'est simple, ça marche.

Les fichier de conf sont générés par puppet à partir de données hiera
(mais ça c'est complètemnet en option).

Le hiera :

  jdoe:
    ipv4        : '198.51.100.1'
    ipv6        : '22001:DB8:2::1'
    prefix      : '2001:DB8:1::/48'
    description : 'John Doe'
    email       : 'john.doe@example.com'

Le puppet qui va bien :

   $openvpn_users = hiera_hash('openvpn::users', {})
   create_resources(openvpn::user, $openvpn_users)
```