# Intervention sur ldap

Actuellement ldap tourne dans un docker.

Pour avoir un bash il faut lancer cette commande : 
```
docker exec -t -i c71b37b2574d /bin/bash
```
Les logs ce trouve dans `/var/lib/docker/containers/c71b37b2574dcc013d6ab39a564bcec3bf5c8fbbf6129958af190f9e7d22c4c6/c71b37b2574dcc013d6ab39a564bcec3bf5c8fbbf6129958af190f9e7d22c4c6-json.log`

Si vous devez restart ldap il faut restart le docker et ensuite isp-ng

```
docker restart c71b37b2574d
systemctl restart ispng.service
```


J'ai dû baisser fortement la sécurité pour que le client ldap accepte un certificat auto signer

Cela ce passe dans `/etc/ldap/ldap.conf`


Il faut faire attention au lien symbolique que docker peut créer. Ils sont pas gardés à chaque fois. (je ne sais pas si cela marche en fait, mais le client ldap avait le CA du docker qui était un lien symbolique qui marchait pas).