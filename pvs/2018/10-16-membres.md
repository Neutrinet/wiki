<!-- TITLE: 10/16 (Membres) -->
<!-- SUBTITLE: Réunions des membres -->

Précédent PV : https://neutrinet.be/pvs/2018/09-18-membres

# Présences
- tierce
- e-Jim
- Jean-Marc
- Emmanuel

# Météo

tierce : content d'être là, les serveurs tournent à la maison dans la chambre d'amis.
jim:  La forme. Pas fait de Neutrinet depuis la dernière réunion à part re-démarrer l'un ou l'autre serveur. 
Emmanuel : OK je pense.
jean-marc : plein d'assoss … on se voit pas assez :P


# hub admin

État du compte avant paiement : 2059,60€
État du compte après paiement : 1644,43€ en trésorerie (hors IBPT)

Factures à payer 

- Ibpt: 329 € annuels -> cotisation en tant que FAI 
  - en attente de clarification de communication structurée
- i3d: 173,03 € mensuels
  - payé
- Flyers.be 92,32€
	- payé

Noms de domaine : tierce alimente le compte Gandi

- +150€ chez Gandi

# Hub Infra

- Pas de nouvelles.
- Une nouvelle journée de travail serait-elle un bon moyen d'avancer sur la configuration des nouveaux serveurs ? - e-Jim lance un framadate
- Rappel de l'historique 42U

# Hub cube

- Néant total - pas une commande. - faut se mettre au nouveau site (v. point hub com)
- Testé la migration d'une LIME2 et LIME1 depuis Yunohost 2.xx (jessie) vers 3.xxx (strech) et ça fonctionne !
- J'ai testé la même chose sur une installation SATA mais là ça merde parce que avec le /boot sur la carte MicroSD, le initramfs ne met pas à jours vers le bon noyaux, la brique démarrant une « stretch » sur l'ancien noyaux elle fonctionne, mais pas les tun0 (openvpn). Mais il y a une solution! (que j'ai dû noter quelque part). Je pourrai refaire le test avec une autre brique.
- En gros mieux vaut migrer sur la carte MicroSD avant de passer l'installation sur un disque SATA!  Et pour info Ynh2 (jessie) ET Ynh3 (stretch) fonctionnent bien sur un SATA : https://wiki.labriqueinter.net/doku.php?id=howto:install_sata
MERCI TIERCE!!!!
- Encore à tester: 
    - l'image LIME2 de Septembre 2018 : https://repo.labriqueinter.net/ (Ynh 2.xx)
    - et celles-ci : https://yunohost.org/#/images (Ynh 3.0.0)

# Hub communication

- Ce mercredi 24 octobre : journée de travail pour le site chez tierce à Limal !
--> on referait une landing page en Grav/Hugo/Pelican/PlumeCMS 
- https://www.reddit.com/r/selfhosted/comments/4d6w8q/filebased_amp_open_source_content_management/
- https://staticsitegenerators.net/
- https://www.staticgen.com/

- Le Wiki.js en version 2.0 étant prévu pour le Q4 2018, on va attendre un peu pour tester si c'est vraiment mieux (navigation, responsive, historique,… 

- Stickers: ~~Jim a loosé.~~ À commandé 500 Flyers \0/
- Fête après la migration vers les nouveaux serveurs.


# « Hub DC »

- Allez, allez, aaallez … Allez le hub DC! :D