<!-- TITLE: Apps Neutrinet -->
<!-- SUBTITLE: A quick summary of Apps Neutrinet -->

# Apps Neutrinet

Notre [script d'installation](https://github.com/labriqueinternet/configuration_scripts/blob/master/neutrinet.sh) installe l'application neutrinet_ynh qui ajoute une [liste d'applications](https://github.com/Neutrinet/neutrinet_ynh/blob/master/scripts/install#L75) à Yunohost afin de pouvoir installer et mettre à jour les applications spécifiques à Neutrinet.
Il n'y a pour l'instant qu'une seule application dans cette liste: [neutrinet_ynh](https://github.com/Neutrinet/neutrinet_ynh).
Celle-ci configure le VPN, renouvelle les certificats si besoin, ...

## Mise à jour

Après avoir modifié le code de l'application et mis à jour le [script d'upgrade](https://github.com/Neutrinet/neutrinet_ynh/blob/master/scripts/upgrade), il faut encore mettre à jour la [liste des applications Neutrinet](https://neutrinet.be/apps.json) afin que Yunohost détecte la nouvelle version. Il faut mettre la jour `revision` git avec le dernier `sha` qui est sur master.
