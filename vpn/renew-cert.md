<!-- TITLE: Renouvellement du certificat -->

TODO: refaire les images

Si je ne me trompe ces méthodes sont reprisent dans l'application [neutrinet_ynh](https://github.com/Neutrinet/neutrinet_ynh).
Pour mettre à jour cette application :

`$ sudo yunohost app upgrade neutrinet -u https://github.com/Neutrinet/neutrinet_ynh`

Si non, et que vous voyez l'erreur `Certificate has key usage  00a0, expects 00a0` dans le fichier `/var/log/openvpn-client.log`, votre certificat a probablement expiré.
Pour vérifier sa date d'expiration, en tant que `root`:

```sh
$ openssl x509 -in /etc/openvpn/keys/user.crt -enddate
notAfter=Nov 12 13:42:43 2018 GMT
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

La date affichée après le `noAfter` est la date d'expiration.  

Il y a trois procédures possibles pour renouveler le certificat.

# Directement à partir d'une brique

Pour le renouveler, en tant que `root`:

```bassh
cd /opt/neutrinet/renew_cert
ve/bin/python renew_from_cube.py
```

# Via le script Renew_cert de Bram 

Cette méthode consiste à cloner le dépôt git sur votre brique internet ou votre ordinateur :

`git clone https://github.com/neutrinet/renew_cert`

On se déplace dans le dossier qui vient d'être créé :

`cd renew_cert`

On crée un environnement virtuel python3. Cela signifie que les paquets python3 qui seront installé n'affecteront pas le reste du système :

`python3 -m venv ve`

On active ensuite cet environnement:

`source ve/bin/activate`

On installe les dépendances du script :

`pip install -r requirements.txt`

Enfin, on lance le script en lui-même via cette commande :

 `python renew.py TON-NOM-D'UTILISATEUR-NEUTRINET`
 
Il vous sera demandé votre mot de passe pour se connecter au VPN de Neutrinet.

Si tout se passe bien, un sous-dosser nommé certs_YYYY-MM-DD_HH:MM:SS (remplacer les lettres en majuscules par la date et l'heure d'exécution du script) est créé, lequel reprend tous les fichiers de configuration nécessaires au client OpenVPN. 

Les fichiers qui nous intéressent sont client.crt et client.key, c'est-à-dire la clé publique et la clé privée du certificat.
Ces fichiers doivent remplacer ceux situés dans /etc/openvpn (cela peut changer en fonction de l'OS). 
Sur la brique internet, ces fichiers se trouvent dans /etc/openvpn/keys
**Remarque**: Avant de remplacer l'un par l'autre, il est conseillé de faire une copie des anciens certificats, juste au cas où.

Cela donne quelque chose comme (toujours à partir du dossier renew_cert) :

```bash
sudo mv /etc/openvpn/keys/user.crt{,.backup}
sudo mv /etc/openvpn/keys/user.key{,.backup}
sudo mv client.crt /etc/openvpn/keys/user.crt
sudo mv client.key /etc/openvpn/keys/user.key
```

Il ne reste plus qu'à redémarrer le service OpenVPN pour tester qu'il fonctionne toujourts.
**Remarque**: Idéalement, assurez-vous d'être connecté à la brique internet en local, c'est-à-dire via son adresse locale (192.168.1.x dans la plupart des cas).

`sudo systemctl restart openvpn`

Pour vérifier que vous êtes connecté derrière le VPN, vous pouvez lancer la commande :

`ip addr`

Normalement, l'interface `tun0` devra apparaître dans la liste.

# Via le site user.neutrinet.be

Cette dernière méthode demande parfois de la patience, car user.neutrinet.be peut connaître des ratés.

La méthode consiste à se connecter en ssh à la brique internet, puis d'aller dans le dossier où se trouve la clé du VPN (il s'agit du fichier /etc/openvpn/keys/user.key) :

`cd /etc/openvpn/keys`

On crée ensuite une demande de signature de clés (CSR) pour cette clé :

` openssl req -out CSR.csr -new -newkey rsa:4096 -nodes -keyout user.key `

Plusieurs questions vous seront posées, comme le code pays (BE), la région (Bruxelles, ...), la Société etc... auquel le certificat est attaché.
Le plus important est le "Cname", qui est le nom qui sera utilisé par Neutrinet pourra savoir à qui appartient le certificat.
**Remarque**: Il est recommandé de mettre votre adresse e-mail pour le Cname.

Cette commande créé un fichier CSR.csr, qui contient la demande de signature. Vous pouvez afficher son contenu avec :

`cat CSR.csr`

Copiez ce contenu précieusement, et rendez-vous sur https://user.neutrinet.be

Connectez-vous

**image**

Cliquez sur **Users**


Attendez que cela charge, et quand vous voyez votre nom, cliquez dessus, puis sur **View associated clients**


**image**

Ensuite, cliquez sur votre certificat (si vous en avez plusieurs, il faut utiliser celui qui liste une IPv4 - 80.67.181.x), et choisissez l'option **Renew certificate**


**image**

Le certificat actuel s'affiche alors à l'écran. Cliquez sur **Rekey**, puis collez le contenu du CSR (le fichier que vous aviez copié plus tôt). Cliquez sur le bouton **Rekey** pour confirmer.

Une fois cette opération terminée, vous verrez un bandeau comme ceci :

**image**

Vous pouvez alors cliquer sur "**View client details**, puis sur **Download config package**.

Cela va télécharger un fichier qui contient les fichiers de certificat. Dans le dossier /etc/openvpn/keys, copiez le fichier client.crt vers /etc/openvpn/keys/user.crt. 

Voilà, c'est tout!

# Liens

- https://github.com/Neutrinet/neutrinet_ynh
- https://github.com/neutrinet/renew_cert

