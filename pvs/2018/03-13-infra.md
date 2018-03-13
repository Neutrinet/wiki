<!-- TITLE: 03 13 Infra -->
<!-- SUBTITLE: Réunion mumble du hub infra-->

#Réunion hub tech - 13/03/18

##Ordre du jour

   * Ce fixer le 2eme mardi du mois à 20H pour la réunion hub tech: OK - mis dans l'agenda
   * OpenVPN

       * **Radius -> Retenu**
       * Stand-alone OpenVPN (script pris par un hook OpenVPN à la connexion et la déconnexion): simple, léger, très manuel, moins scalable/standard/maintenable

   * CentOS (cycle de maj plus long)

       * Moins de connaissance
       * Moins de logicielle fourni dans les repos de base
       * On reste sur Debian

   * Passage à stretch
       * Pas vraiment une priorité
       * avec ansible
       * remettre orval dans la course
   * Ansible (comtribution ?)
       * est-ce que ce serait bien, si on repart sur une nouvelle infra, de directement faire ça avec Ansible?
   * Alternative à glusterfs (ne suit pas sur le I/O)
       * tl;dr: on ne va pas migrer nos services actuels hors de glusterfs, mais on va monter la nouvelle infra sur un nouveau filesystem, et quand on migrera nos services actuels, ils seront de facto sur le nouveau filesystem
       * DRDB : on ce bloque à 2 serveur mais on peut sync à interval régulier les bonne vm sur un 3eme server
           * Où s’arranger pour avoir 2 (3?) vm vpn qui tourne
       * Changement avec orval
       * Neutrinet: en mode dégradé since before it was cool

   * Séparer les site des amis
       * pas d'avis contraire
       * en même temps que le passage à stretch

   * Backup -> Actuellement c'est ultra light
       * on est ok pour les mettre chez les GAFAM :)

   * Nouvelle interface inscription VPN
       * Thomas va dev une interface à Hexagone pour inscription + gestion de sa vie de membre (Attention, pas refaire un blob #isp-ngBornAgain - )
           * Commande de brique
           * VPN
           * Compta
       * On se met d'accord avant sur la nature modulaire ou plus intégrée des différents outils

   * Carnet rose ? (ce qu'il en deviens)

   * Mattermost licence
       * Demande effectuée: 50 €/an 

   * LDAP (hexagon)
       * Je galere à faire le vagrant mais j'approche de la fin :)

   * Comptoir (manque de puissance)
       * J'ai fait marcher les carte ipmi (uniquement serial)

   * Documentation
       * Préparer une journée de travail

   * Trouver un nom pour le futur ISP-ng
       * isp-ng-ng

   * Réunion nubo
       * \url{https://pads.domainepublic.net/p/42u}
PLANNING:

   * Prévoir une réunion pour la doc
   * Préparer le we de la reinstall d'orval
   * faire le we de la reinstall d'orval
