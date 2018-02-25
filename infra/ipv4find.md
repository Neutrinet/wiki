<!-- Liberation des ipv4 -->

# Recupération infos certificat
J'ai executer cette commande 

```
root@vpn-oldsec:/opt/ispng/ca/store# for i in *.crt; do openssl x509 -in ${i} -noout -enddate -email -subject -serial| sed ':a;N;$!ba;s/\n/ /g'; done > /tmp/005
```

# Trouver les infos dans la bdd

Dans un premier temps j'ai chercher les ipv4 dit expirer dans la bdd lier à leurs certificat : 
```
SELECT 
  certificates.serial, 
  address_pool."ipVersion", 
  address_pool.expiry, 
  address_pool.address, 
  ovpn_clients."commonName"
FROM 
  public.address_pool, 
  public.certificates, 
  public.ovpn_clients
WHERE 
  address_pool.client_id = certificates.client_id AND
  certificates.client_id = ovpn_clients.id AND
  address_pool."ipVersion" = 4 AND
  address_pool.expiry <= CURRENT_DATE
ORDER BY
  address_pool.expiry ASC;
```

Le soucis c'est que la date d'expiration dans la bdd est fause. Le 2eme soucis c'est qu'il faut convertir le sérial en base 64.

Du coup j'ai exporte juste le seria de la req précedente. Et converti en base 64 les serial, cela ce trouve dans `/tmp/007`. Pour convertir un chifre en base 64 facilment il y a `bc` avec cette commande : `echo "obase=16; 123456789" | bc`

Ensuite il faut extraire les infos des certificat dit expirer : 
```
root@vpn-oldsec:/tmp# for i in $(cat 007); do cat 005 | grep ${i}; done > 008
```

Mais dans ce fichier en ce rend comte des certificats qui on été renouvlé donc à la louche j'ai fait :
```
cat 008 | grep -v "2017 GMT" > 009
```


La derniere etape est de comparer les certificat qui sont vraiment expirer avec les infos de leur propriétaire. Savoir si c'est juste un oubli, ou si maintenant ils ont en on plus besoin.
Il faut ensuite les retirer de la bdd et faire de même avec l'ipv6 pour même si c'est pas urgent.