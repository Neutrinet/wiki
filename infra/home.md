<!-- TITLE: hub infra -->
<!-- SUBTITLE: Infrastructure - Infrastructuur, Sysadmin, Servers & Codes-->

:information_source: Cette page est la **page dédiée du hub infrastructure**, pour voir **les autres pages**, merci de visiter la section `hub-infra` dans notre [documentation](all).

# Origine
:warning:  **OUTDATED - À RELIRE** :warning: 

* Le pad de mars 2016 : https://pad.lqdn.fr/p/neutrinet-infrastructure-hub
* Qui est devenu la page : https://neutrinet.be/hubs/infrastructure.html

# English
:warning:  **OUTDATED - À RELIRE** :warning: 

Objective: keep the infrastructure running, improve it and do cool shit
Needed knowledge: linux system administration knowledge
Good to have knowledge:

* openvpn
* networking skill (routing, bgp, iptables)
* classical deployement workflow (php, python, ruby, other)
* jvm administration (zookeeper, maven) (for ISP-ng for the VPN)

## Active people

* Wannes
* Bram - hit everything with a hammer until it works
* Tharyrok - coordinator

## People

* bram
* ivom
* jeremie
* emile
* tharyrok
* nicolas
* thomas
* wannes

## Activities

Keeping the infrastructure running
* we are finishing the migration from LDN vm at Strasbourg
* we used to have a LXC on UrLab server for all the web stuff, we migrated everything mid february 2016
* we have a 2-in-1 server at Rotterdam with most of our stuff there
	* stuff is split into several vms
* we are also hosting hackeragenda.be for now

Handling the VPN infrastructure
* this is our core part
* it should be running and we should be able to debug when one of the component breaks
* ISP-ng is a home made all-in-one great VPN handling solution that we are using and maintaining

Communication with Gitoyen
* we are member of Gitoyen
* we have our ipv4/6 range there
* we also have a vm for routing
* from time to time we need to talk with them for networking stuff (usually with sebian)

Money
* pay gandi for our domain names, handling bill to administrative hub or ask them to pay
* same for i3d for hosting

## Current things to do

* avoid bus factor
* document how to do reverse dns for ip
* same for PTR records
* document the infrastructure
* explore monitoring solutions
* list pgp and ssh public

## Nice to have

currently we can't access to neutrinet public website (like neutrinet.be or cube.neutrinet.be) inside the VPN

## Future things that can be done

* recode ISP-ng in haskell
* serious monitoring (we have for now but it's not really great)
* serious backup system (we used to have one but we don't anymore since recent migrations)
* put everything under config management? quite big, maybe not worth it


# Français
Bienvenu dans la partie infra
Cette page liste toute la documention du hub infra.

## Responssable
Actuellement voici la liste des memnbres qui ont un acces au serveur : 
* bram
* jim
* tbalthazar
* tharyrok

## Doctrine

* Tout service doit rédemarer sans intervention humaine
* Dans le doute reboot
* D'abord Ansible ensuite appliquer

## Présentation matterielle

## Présentation des vm

## Subtilités logicielles

## Cannaux de communication

* Sur invitation : [mattermost](https://chat.neutrinet.be/neutrinet/channels/hub-infra)

## Ressources

* Dépôts principaux sur : [Github/Neutrinet](https://github.com/neutrinet)

