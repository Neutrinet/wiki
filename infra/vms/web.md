<!-- TITLE: VM web -->
<!-- SUBTITLE: Infrastructure - vm web -->

# Présentation
Cette VM délivre la plupart de nos sites webs.

# Caractéristiques
Hostname   : web.neutri.net
IPv4        : 80.67.181.4 172.16.42.4
IPv6        : 2001:913:1000::4
HDD         : 75 GB
CPU         : 4 vcpu
RAM         : 4096
OS          : Debian 8 (jessie)

# Accès SSH
* bram
* jim
* tbalthazar
* tharyrok

# Logiciels
* node
* matterbridge
* mongodb
* nginx
* php (5.6, 7.0,7.2)
* mattermost
* postgresql
* supervisor

## nginx (vhost)

**En prod**

* admin.neutrinet.be
* api.neutrinet.be
* beta-wiki.neutrinet.be
* chat.neutrinet.be
* cube.neutrinet.be
* files.neutrinet.be
* hackeragenda.be
* hackeragenda.fr
* neutrinet.be
* support.neutrinet.be
* user.neutrinet.be
* vg.hackeragenda.be
* wiki.neutrinet.be
* wiki-old.neutrinet.be

**Sandbox**

* auth.neutrinet.be
* beta.neutrinet.be

**Cassé ou ???**

* ffdnapi.neutrinet.be (redirect vers neutrine.be)
* hackeragenda.urlab.be (not found)
* forum.neutrinet.be (bad gw)

## postgresql
* admin-neutrinet-be
* beta-support-neutrinet-be
* board-neutrinet-be
* brimir
* carnet_rose
* chat-neutrinet-be
* hackeragenda-be
* hexagon
* nextcloud
* owncloud
* pvpads
* sentry
* zammad

## supervisor
* backoffice
* forum-neutrinet-be
* hackeragenda
* hackeragenda_fr
* hackeragenda_vg_be
* hexagon-worker
* wiki-neutrinet-be

## systemd
* matterbridge.service
* mattermost.service
* pm2-beta-wiki-neutrinet-be.service
* realms-wiki.service

# Autre
