<!-- TITLE: Socks Tunnel -->
<!-- SUBTITLE: A quick summary of Socks Tunnel -->

# Créer un tunnel SSH vers sa brique pour pouvoir utiliser le VPN Neutrinet depuis n'importe où

- créer un tunnel SSH vers sa brique avec la commande suivent:
```
$ ssh -D 8080 -f -C -q -N root@mabrique.org
```
- dans Firefox, aller dans Préférences > Network Proxy > Manual Proxy  Configuration
- dans le champs SOCKS Host: localhost
- dans le Port: 8080 (le port renseigné dans la commande SSH)
- OK

Votre IP est maintenant celle de votre brique.
Pour terminer le tunnel:
- `ps aux | grep ssh`
- noter le pid du process ssh
- `kill -9 <pid-du-process-ssh>` 
- remettre No Proxy dans les préférences Firefox

Pour le faire sous Windows:
https://www.digitalocean.com/community/tutorials/how-to-route-web-traffic-securely-without-a-vpn-using-a-socks-tunnel