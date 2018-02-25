
Il y a un petit script de e-Jim pour ajouter un record PTR (reverse DNS)

Il peut être appelé, sur la VM web, via la commande suivante:
```bash
  /opt/dyndns/dyndns.sh <ipv4 ou ipv6> <nom de domaine à y assigner> <serveur DNS à updater>
```
Pour l'instant, le serveur DNS à utliser est le 172.31.1.253 (interface interne du serveur DNS de Neutrinet)

Exemple: 
```bash
/opt/dyndns/dyndns.sh 80.67.181.163 brique.e-jim.be 172.31.1.253
```

Le script retourne différents codes:


- 0 - Tout s'est bien passé, merci.
- 1 - Le premier argument n'a pas l'air d'être une IP, ni v4 ni v6.
- 2 - Le second arguement n'a pas l'air d'être un nom de domaine valide.
- 3 - Le troisième argument semble n'être ni une ip ni un nom de domaine.
- 4 - Le nom de domaine ne se résout pas dans l'IP indiquée. [1]
- 5 - nsupdate (l'outil utilisé par derrière) a retourné une erreur. 
- 33 - Il n'y a pas le bon nombre d'arguments (3)

[1] C'est une limitation qui n'est pas obligatoire, mais qui est souvent implementée dans ce genre de service. Ça peut s'adapter.

--------------------


There is a small script that
updates the reverse record for a given ip address.

It can be called on the web VM as 
```bash
/opt/dyndns/dyndns.sh <ipv4 or ipv6>
<domain name> <DNS server to be updated>
```
For the moment, the DNS server is 172.31.1.253 (I just set up this
argument for more modularity in the future).

It can have those different return codes:

- 0 - All went fine, thank you.
- 1 - First argument is not a valid IP (either v4 or v6)
- 2 - Second argument is not a valid domain name
- 3 - The third argument is not a valid DNS server name or IP
- 4 - The chosen domain name doesn't resolve into the given IP.  [1]
- 5 - nsupdate (the tool used behind) returned an error.
- 33 - There are less or more than 3 arguments

Most of the time, more info can be found in the stdout.


I hope this can help someone implement reverse DNS into ISPng (including
the needed verification that the user has administrative rights on the IP).
