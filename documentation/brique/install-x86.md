<!-- TITLE: Installation sur X86 -->

TODO: Heu ... c'est pas très clair ... à relire!
# Installation
Install debian jessie base

Install yhunohots sans la post-install

--

# Configuration
## Post install

- Avoir un comte sur https://api.neutrinet.be/
- Avoir les certificats du vpn de neutrinet

-----------------------

Il faut s'assure que l'utilisateur admin existe bien et appartient au groupe sudo

```
adduser admin
adduser admin sudo
```

Premierer etape c'est de télécharger le scrupt (/dev/null) d'installation de neutrinet.

```
cd /root
wget https://raw.githubusercontent.com/labriqueinternet/configuration_scripts/master/neutrinet.sh
chmod +x neutrinet.sh
```

Attention il faut commenter en fin de ficher les bout de script que vous n'avez pas besoin il faut les commenter en rajoutant un diese devant un des mots suivant :
 - get_variables

 - modify_hosts
 - set_locales
 - upgrade_system

 - postinstall_yunohost
 - create_yunohost_user
 - add_labriqueinternet_app_list
 - install_vpnclient
 - configure_vpnclient
 - install_hotspot
 - configure_hostpot
 - install_doctorcube
 - install_neutrinet_ynh
 
 - remove_dyndns_cron
 - restart_api

 - display_win_message


DONE :)
