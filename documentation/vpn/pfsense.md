configurer NEUTRINET dans pfsense (2.3.1_5)

1/ configurer les certificats
System>Cert Manager

Tab "CAs"   (ce certificat sert à protéger l'utilisateur de quelqu'un qui chercherait a se faire passer pour neutrinet)

Add
 + Descriptive name: NEUTRINET_VPN (ou autre...)
 + Certificate data: copy paste the neutrinet certificate (ca.crt in the zip config file)
 + Certificate Private Key: leave blank
 + serial for next certificate: leave blank

Tab "Certificates"

Add         (ce certificat est votre clef personnelle, elle sert à garantir que personne ne se fera passer pour vous en se connectant ==> si le fichier private key se retrouve dans la nature il faut faire opposition/revoquer comme on ferait opposition si on perd son code de carte bancaire)
 + Descriptive name: NEUTRINET_KEY (ou autre...)
 + Certificate data: copy paste your certificate (client.crt in the zip config file)
 + Private key data: copy paste your private key (client.key in the file you generated when you suscribed to the vpn)

2/ configurer le VPN
VPN> OpenVPN

Tab "Client"
 + Server Mode: Peer to peer (SSL/TLS)
 + Protocol: UDP
 + Device mode: tun
 + Interface: WAN
 + Local port: leave blank
 + Server host or address: vpn.neutrinet.be
 + Server port: 1194
 + Proxy host or address: leave blank
 + Proxy port: leave blank
 + Proxy authentification extra options: none
 + Server host name resolution: check "infinitely resolve server"
 + Description: NEUTRINET_CONFIG (ou autre...)
 
 + Username: the email you used when suscribing
 + Password: vous savez ce que c'est...
 
 + TLS authentification: uncheck enable authentification of TLS packets
 + Peer certificate authority: NEUTRINET_VPN
 + Client certificate: client.crt
 + Encryption algorithm: BF-CBC (128bit) ==> je ne suis pas sur que c'est le plus sur.. personnellement je suis plus interessé par le coté IP fixe autohebergement que par la NSA qui m'écoute
 + Hardware Crypto: No Hardware crypto acceleration ==> dans ma vm j'ai pas le choix mais à mon humble avis un processeur récent gère forcément çà.

 + IPV4 Tunnel Network: leave blank
 + IPV6 Tunnel Network: leave blank
 + IPV4 Remote Network/s: leave blank
 + IPV6 Remote Network/s: leave blank
 + Limit outgoing bandwidth: leave blank
 + Compression: No preference (==> default LZO)
 + topology: Subnet
 + Type of Service: uncheck set the TOS IP header...
 + uncheck don't pull routes
 + uncheck don't add/remove route

 + Advanced: leave blank
 
4/ Créer une interface!! 
Interfaces>Assign

 + Add ovpnc1
 + SAVE

Interfaces>OPT1
 
 + check enable
 + description: NEUTRINET_IP_FIXE


5/ start the client
Status > OpenVPN

 click on start
 
5/ check this is working.
Status> Dashboard

normalement il y a votre IP neutrinet!!

==> vous pouvez:
    * soit choisir de tout router via le VPN
    * soit exposer vos services via des port forward vers votre LAN

    
6/ créer un alias pour les ips qui iront sur internet en passant par le VPN
Firewall > Alias
 
 + Add
 + Name: via_neutrinet
 + Description: les ips qui vont sur internet via le VPN
 + Type: Host(s)
 + IP or FQDN: les ips sur le réseau local 192.168.14.*
 
7/ ajouter une règle qui change le routing par défaut.
Firewall > Rules

 + Tab: LAN
 + Add (première règle=> la première de la liste)
 + Action: Pass
 + Disable: uncheck (evidemment)
 + interface: LAN
 + Address family: IPv4
 + Protocol: Any
 + Source: Single host or alias / selectionner l'alias de la partie précédente
 + Destination: Any
 + Log: uncheck
 + description: router via neutrinet VPN
 + Advanced: tout laisser par défaut SAUF
 + Gateway: NEUTRINET_IP_FIXE_VPNV4
 + Save
 
 8/ configurer le NAT sur l'interface OPENVPN
 Firewall > NAT
 
  + Tab: Outbound
  + Radio Buttons: Change to "Hybrid Outbound NAT rule generation. (Automatic Outbound NAT + rules below)
  + Add
  + uncheck disabled
  + uncheck do not NAT
  + Interface: NEUTRINET_IP_FIXE
  + Protocol: any
  + Source: Network 192.168.14.0/24
  + Destination: any
  + uncheck not
  + Translation: Interface address
  + port: leave empty; uncheck static port
  + uncheck no XMLRPC Sync