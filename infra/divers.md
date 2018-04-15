<!-- TITLE: Divers -->
<!-- SUBTITLE: A quick summary of Divers -->

# Divers
## Créer une entrée reverse DNS

```
$ ssh username@vpn.neutri.net
$ sudo su
$ cd /home/jim/scripts/dns
$ ./dyn-reverse-dns.sh 1.2.3.4 example.com 172.16.42.2
$ ./dyn-reverse-dns.sh "mettre-ipv6-ici" example.com 172.16.42.2
```