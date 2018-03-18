# Proposition nouvelle infra

Ce document va êtres relativement indigeste.
Je vais séparer la config des serveurs du concept.
Bonne lecture :)

## Choix des technos

Nous allons partir sur une Ubuntu 16.04 LTS comme host pour les hyperviseurs et sur du Debian pour les vm.

Le choix de Ubuntu est dû à drbd9 qui n'est pas disponible sur Debian.

Drbd9 à l’avantage de permettre le choix quel disque il faut cloner sur quel serveur. (exemple: la vm vpn peut être clonée sur 3 serveurs et la vm web sur 2)

Pour l'hyperviseur je choisis Xen car je le connais bien.

Nous utiliserons keepalived pour gérer l’état des serveurs. Ce dernier permet de donnée un script quand il change d’état, ce qui pour moi est plus simple à regarder que linux-ha avec la déclaration de ressources.


## Principe de fonctionnement
### D'un point de vue des serveurs
![Schéma serveur](https://files.neutrinet.be/s/VbbmPaY1c3Zbcif/download)

J'ai volontairement ajouté center au schéma même si aujourd'hui nous n'avons aucune idée de quel serveur cela pourrait être mais cela permet d'imaginer le pire scenario.

Par défaut c'est left qui est allumée avec les vm. Left est en "master" et les autre serveur en "slave".

Quand left ne répond plus (via le réseau) keepalived passe left en slave et right en master.

Au moment de la bascule keepalived stop les vm sur left et les allume sur right. Keepalived modifie aussi bird (les annonces bgp). Drbd se suffit à lui-même et s'occupe comme un grand de l'état des disques dur.

Au moment où left reviens à la vie les syncro de disque se font. Pour rebasculer les vm sur left il faudra le faire manuellement.

Si jamais right et left ne répond plus c'est center qui prend la main avec comme conséquence que seul le vpn marchera.

### D'un point de vue du réseau
![Schéma réseau](https://files.neutrinet.be/s/eBeIH1IinB47ksk/download)

Quand une machine est en master elle annonce que c'est elle qui route le trafic 80.67.181.10/24. Les autre serveur annonce aussi la route mais avec un hop 3 fois supérieur à la machine master. Cela permet de s'assurer que le trafic va directement au master sans passé par un des slaves.

L'ip 80.67.181.1 est celle qui héberge tout le contenu. Elle aura le web,vpn,... Ce qui permet lors d'une bascule d'un serveur à un autre de garder la même ip pour les services.
