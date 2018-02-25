# Renouvellement du certificat

Si vous voyez l'erreur `Certificate has key usage  00a0, expects 00a0` dans le fichier `/var/log/openvpn-client.log`, votre certificat a probablement expiré.
Pour vérifier sa date d'expiration, en tant que `root`:
```
$ openssl x509 -in /etc/openvpn/keys/user.crt -enddate
notAfter=Nov 12 13:42:43 2018 GMT
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```
La date affichée après le `noAfter` est la date d'expiration.  
Il y a trois procédures possibles pour renouveler le certificat.

### Directement à partir d'une brique

Pour le renouveler, en tant que `root`:
```
$ cd /opt/neutrinet/renew_cert
$ ve/bin/python renew_from_cube.py
```

### Procédure par le script Renew_cert de Bram 

La procédure est donc de cloner le dépôt git sur ton cubie board, ou d'ailleurs sur n'importe quel ordinateur:

```git clone https://github.com/neutrinet/renew_cert```

(dans le dossier qui te convient le mieux)
Et tu rentres dans le sous-dossier qui vient d'être créé:

```cd renew_cert```

Ensuite, tu crées un environnement virtuel python (ce qui veut dire que les paquets pythons que tu installeras n'affecteront pas le reste de ton système) :

```virtualenv ve```

Puis, tu actives cet environnement:

```source ve/bin/activate```

Enfin, tu installes les dépendances du script (peut faire une petite erreur sur Jessie, mais fonctionne quand même):

```pip install -r requirements.txt```

Pour finir, tu peux enfin lancer le script en lui-même en tapant cette commande

 ```python renew_local.py TON-NOM-D'UTILISATEUR-NEUTRINET TON-MOT-DE-PASSE```

Je te conseillerais de mettre un espace avant cette commande, et ce afin d'éviter qu'elle ne se trouve dans ton historique de commandes.état
Le script prendra quelques minutes, t'informant des différentes étapes qu'il passe (Login, Get client data, Generate new cert using openssl, See if I already have a cert, I already have a cert, let's update it, Put new cert online et enfin Download new config si tout s'est bien passé).

Tu auras alors dans le dossier renew_cert, un sous-dosser appelé certs_2017-07-xx_XX:XX:XX (ou les derniers X dépendant de la date et l'heure d'exécution du script), qui reprend tous les fichiers de configuration demandé par ton client openvpn.

Ce sont essentiellement les fichiers client.crt et client.key qui t'intéresse, et qui doivent remplacer les fichiers du même nom situé (pour autant que tu utilises la configuration standard openvpn de Debian), dans /etc/openvpn/ ou dans /etc/openvpn/neutrinet
Avant de remplacer l'un par l'autre, je suggèrerais de faire une copie des anciens, au cas où.
Cela donnerait quelque chose comme (toujours en étant dans le dossier renew_cert):

```
sudo mv /etc/openvpn/neutrinet/client.crt /etc/openvpn/neutrinet/client.crt.backup
sudo mv /etc/openvpn/neutrinet/client.key /etc/openvpn/neutrinet/client.key.backup
sudo mv client.crt /etc/openvpn/neutrinet/client.crt
sudo mv client.key /etc/openvpn/neutrinet/client.key
```

OU (apparement dans centains cas -à définir- user. remplace client.)

```
sudo mv /etc/openvpn/neutrinet/user.crt /etc/openvpn/neutrinet/user.crt.backup
sudo mv /etc/openvpn/neutrinet/user.key /etc/openvpn/neutrinet/user.key.backup
sudo mv user.crt /etc/openvpn/neutrinet/user.crt
sudo mv user.key /etc/openvpn/neutrinet/user.key
```


Après quoi, il ne te resterait plus qu'à re-démarrer le service VPN pour tester qu'il fonctionne toujours (idéalement, assure-toi de te connecter au cubie localement, via son adresse locale (192.168.1.x dans la plupart des cas).


``` sudo systemctl restart openvpn ```


----------------------------------

### Procédure manuelle et via le site user.neutrinet.be




Deuxième méthode (qui parfois demande la patience car actuellement user.neutrinet.be connait des ratés):

La seconde méthode consiste à se connecter en ssh au cubieboard, aller dans le dossier où se trouve la clef du VPN (a priori, il s'agit du fichier /etc/openvpn/neutrinet/client.key ou /etc/openvpn/client.key )

```cd /etc/openvpn/neutrinet```

Puis de créer une demande de signature de clefs (CSR) pour cette clef

```openssl req -new -sha1 -out client.csr -key user.key```

Plusieurs questions te seront posées, comme le code pays (BE), la région (Bruxelles, ...), la Société etc... auquel le certificat est attaché. Le plus important est le "Cname", qui est le nom sous lequel tu vas apparaitre pour neutrinet.

Cette commande créé un fichier client.csr, qui contient la demande de signature. Tu peux en voir le contenu en faisant

```cat client.csr```

Copie ce contenu précieusement, et rends-toi sur https://user.neutrinet.be

Connecte-toi

image

Clique sur users


Attends que cela charge, et quand tu vois ton nom, clique dessus, puis sur "View associated clients"


image



Ensuite clique sur ton certificat (si tu en as plusieurs, il faut utiliser celui qui liste une IPv4 - 80.67.181.x), et choisi l'option "Renew certificate"


image



Il va alors te présenter ton certificat actuel. si tu cliquer sur Rekey, tu auras la possibilité de coller le CSR (le fichier généré plus tôt, que tu as copié), puis de cliquer sur Rekey.

Une fois cette opération terminée, tu verras un bandeau comme ceci:

image



Tu pourras alors cliquer sur "View client details", puis sur Download config package, qui est un zip qui contient le fichier de certificat qu'il faudra remplacer dans, à priori, /etc/openvpn/neutrinet (voir plus haut, en fonction du dossier qui tu as décidé d'utiliser à la configuration du service).



-----------------


Voilà, c'est tout!

### Liens

- https://github.com/Neutrinet/neutrinet_ynh
- https://github.com/neutrinet/renew_cert

