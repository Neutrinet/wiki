<!-- TITLE: 12/18 (Membres) -->
<!-- SUBTITLE: Réunion des membres -->


PV de la réunion précédente: https://wiki.neutrinet.be/pvs/2018/11-20-membres

# Présences
* philox
* Jean-Marc
* Daniel
* Tierce
* e-Jim
* Emmanuel

# Météo

* tierce : tout roule, plein de trucs à faire, pas d'heure prévue.
* emmanuel : cool, ça fait longtemps, pas d'heure prévue.
	* un petit point Nubo ?
	* qwé les nouveaux serveurs. ? 
* philox : content d'avoir appris « dd », 21h30 départ.
* daniel : nouveau venu… depuis les Ardennes belges… souhaite un VPN… neutralité, auto hébergement, souhaite une brique.
	* gros travail à faire en Gelbique sur le libre comparé à ce qui se passe en France
* e-Jim : quelques jours à venir de disponibles pour les nouveaux serveurs, pas de news de notre sainte licorne des réseaux, pas d'heure
  * serveurs / infra
  * AG à venir
  * renouvellement des administrateur·ice·s?
  * où et comment?
* jean-marc : moi c'est jean-marc, pas d'heure
# thèmes sortants de la météo
* un petit point Nubo ?
* qwé les nouveaux serveurs. ?
* AG à venir
# hub-admin
## A.G. 2019

- Où ?
  - Voir le pad de Jean-Marc:  https://pads.domainepublic.net/p/neutrinet-AG2019
  - Fourmilière … c'est faisable pour le 26.
    - Adresse : rue d'Omal
    - ils viennent d'ouvrir… et savent pas trop pour les tarifs, conditions, etc
    - moyen de consommer sur place
    - on peut venir en mode auberges espagnole
- Mundo-B pour le 26/01 il ne reste qu'une grande salle à 395€ pour une plage « journée complète »
- FIJ : travaux : pas sur d'être dispo pour le 26.
- Ixelles 
  - des trucs pas chère si on est Ixellois … mais le siège de Neutrinet est à Uccle
  - doit être valider par le Conseil Communal -> délais -> risque de réponses une semaine à l'avance et on connait pas la date des Conseils Communaux
- Pour valider « la fourmilière »
  - Jean-marc passera / relancera « bruno » pour savoir quoi pour les prix
- Pour valider une salle à Ixelles
  - Choisir une salle http://www.ixelles.be/site/627-Salles-de-la-Maison-de-la-Solidarite et réserver ?
  - e-Jim à envoyé un mail pour let 26 pour la salle Salam ou Saleke, on verra pour la réponse.

- Quand ?
  - OK pour le 26 janvier ? : 7
  - Pas OK : 0
  - Abstentions : 0

- À faire ASAP (au plus tard avant le 18 janvier)
  - un ordre du jour
    - approbation des comptes et décharge des Administrateurs
      - bilan
      - élection du CA ( renouvellement des Administrateurs )
      - parler du futur
- une convocation
  - date, lieux, heure début, heure de fin
  - modalités (on part sur une auberge espagnole)
    - brunch de 11h à 14h
    - AG 14h à 14h10
    - pot et papotes 14h11 à 16h
    - bouffe : auberge espagnole
    - glouglou : auberge espagnole


# hub-comm

## remarques sur le nouveau site

- difficile de trouver les infos de PAIEMENT sur le site & le wiki !

## remarques sur « l'app Neutrinet » et la page d'accueil dans l'app Yunohost

- l'app Neutrine affiche tjs l'adresse du 123

## remarques sur api.neutrinet.be

- Il est également a noter qu'aucun des liens "Contact Neutrinet", "Privacy" et "Ethics" ne fonctionnent et que le lien "Login" me renvoie l'erreur "La connexion a échoué" … voir https://support.neutrinet.be/#ticket/zoom/2245
-- ERROR : see other … reste-t-il des IPs disponibles ?

- pour résumer toujours les liens suivants sur la page d'accueil de api.neutrinet.be

-- Login (coin supérieur droit) :
    - https://vpn.neutrinet.be:8000/
-- En bas à gauche :
    - http://neutrinet.be/index.php?title=Contact
    - http://neutrinet.be/index.php?title=Privacy
    - http://neutrinet.be/index.php?title=Ethics

## Proposition de Agnez - Stream Vidéo du 35c3 \O/

- c'est entre de 27 et le 30 décembre 2018
- où est-ce qu'on se rassemblerait pour se mater des Streaming
  - À Bruxelles ?
  - À Limal chez tierce ?


# hub-infra

## pour avancer sur les nouveaux serveurs

- est-ce qu'on se voit cette semaine ?
  - ce mercredi soir irl ou mumble ?
  - ce jeudi jour
  - ce jeudi soir -> noop … on va boire un pot au Toone 
  - ce vendredi jour
  - ce vendredi soir
  - ce samedi jour

## il semblerait qu'il n'y ait plus d'IP de dispo pour isp-ng

## on pourrait travailler sur RADIUS avec les « autres IPs »

# hub-cube

Install party dimanche passé: 
    - Brique A. perforée pour régler le souci de boutons
    - C. carte MicroSD read only, mais un backup (clone) dispo -> nouvelle carte -> ok
    - H. mysql est foutu -> on continue à en causer et tierce continue à chercher (le script auto-fix-mysql ne suffit pas )
    - Jmg -> carte microSD foutue -> reparti d'un clone qu'il avait ( ynh 2.4 ) -> update -> update app Neutrinte -> renouvellement certificat OK -> brique OK
    - soudure d'une antenne wifi cassée (merci ptrhere)

À faire : Envoyer la demande à Olimex de faire des plus gros trous dans leur boitier métallique (photo à l'appui) 

À faire : tester une install Ynh 3.3 directement et / ou l'utilisation des fichiers .cube : https://install.labriqueinter.net/#welcome

À faire : https://labriqueinter.net/dotcubefiles.html