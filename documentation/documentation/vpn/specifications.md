<!-- TITLE: Specifications du VPN-->
<!-- SUBTITLE: À des fins de dépannage ou a titre informatif -->

Nous conservons, à des fins de dépannage, un journal (log) des connexions VPN établies durant les 31 derniers jours. Ce journal contient le certificat utilisé pour la connexion, ainsi que l'adresse IP depuis laquelle cette connexion a été établie et l'heure de la connexion.

Le chiffrement utilisé pour le tunnel lui-même est de l'AES-256 en mode enchaînement des blocs (CBC - https://fr.wikipedia.org/wiki/Mode_d%27op%C3%A9ration_(cryptographie)#Encha.C3.AEnement_des_blocs_:_.C2.AB_Cipher_Block_Chaining_.C2.BB_.28CBC.29)

Les communication de contrôle (échange de données entre le client openvpn et le serveur openvpn) se fait via le protocol TLS (1.2 minimum), et avec des algorithmes de chiffrements permettant tous le PFS forward secrecy puisque basés sur un échange de clés Diffie-Hellman (DHE), avec une préférence logicielle pour les échanges basés sur les courbes éliptiques (https://fr.wikipedia.org/wiki/%C3%89change_de_cl%C3%A9s_Diffie-Hellman).

# Configuration du serveur

```
cipher AES-256-CBC
auth SHA256
tls-cipher TLS-ECDHE-RSA-WITH-AES-128-GCM-SHA256:TLS-ECDHE-ECDSA-WITH-AES-128-GCM-SHA256:TLS-ECDHE-RSA-WITH-AES-256-GCM-SHA384:TLS-DHE-RSA-WITH-AES-256-CBC-SHA256
tls-version-min 1.2
```