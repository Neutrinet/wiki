<!-- TITLE: Problems -->
<!-- SUBTITLE: A quick summary of Problems -->

Note rapides qu'il faudrait tester, reproduire, documenter et remonter vers Yunohost ou Neutrinet.sh si nécessaire.
# DOVECOT

Lors de l'install de janvier 2018 ... il y a eu une erreur sur les deux briques (lime 1 et 2)

J'ai associé cette erreur à :

https://forum.yunohost.org/t/app-installation-blocked-by-dovecot-issue/2416/15

Contourné en faisant :

`# apt install dovecot-sieve`
`# yunohost service regen-conf --with-diff dovecot`

Si je me souviens bien, entre  postinstall_yunohost et create_yunohost_user


# APP NEUTRINET

J'ai l'impression que l'APP Neutrinet demandée par install_neutrinet_ynh ne s'installe pas.

J'ai associé cette erreur à : 

https://dev.yunohost.org/issues/1080#change-3974

Contourné en faisant : 

`# yunohost service regen-conf nginx`


Et il faudrait également la mettre à jour :

`# yunohost app upgrade neutrinet -u https://github.com/Neutrinet/neutrinet_ynh
`


# YunoHost post-installation
I install the image on the SD-card using the script. I prepare and boot up my cube and ssh to it. I download the script (wget...) and run it.
After giving the information (name, domain...) the script runs. it get's to *Launching YunoHost post-installation...*, but then it gets stuck at *Success! The service 'yunohost-firewall' has been enabled* 

From the output (I obscured the domain and pasword here):

`+ echo 'Launching YunoHost post-installation...'
Launching YunoHost post-installation...
+ set -x
+ yunohost tools postinstall -d <DOMAIN.TLD> -p <PASSWORD>
Success! LDAP has been initialized
Success! The configuration has been updated for service 'ssl'
Success! The local certification authority has been created.
Success! The configuration has been updated for service 'nsswitch'
Success! Successfully installed a self-signed certificate for domain <DOMAIN.TLD>!
Success! The domain has been created
Success! The SSOwat configuration has been generated
Success! The main domain has been changed
Success! The administration password has been changed
Warning: Some firewall rules commands have failed. For more information, see the log.
Success! The application list yunohost has been fetched
Warning: Skipping migration 1 change_cert_group_to_sslcert...
Warning: Skipping migration 2 migrate_to_tsig_sha256...
Synchronizing state for yunohost-firewall.service with sysvinit using update-rc.d...
Executing /usr/sbin/update-rc.d yunohost-firewall defaults
Executing /usr/sbin/update-rc.d yunohost-firewall enable
Success! The service 'yunohost-firewall' has been enabled
`

NOTE: At the start of running the script I get
`/!\ This script has to be run as root *on* the Cube itself, on a labriqueinternet_A20LIME_2015-11-09.img SD card (or newer)`
I have Lime2 (but the https://wiki.neutrinet.be/cube/install page doesn't differentiate between the two).

## Possible solution ...

On the Yunohost forum [here (fr)](https://forum.yunohost.org/t/post-installation-avant-mise-a-jour/4254) somebody solved the issue WITHOUT upgrading the system BEFORE doing the post-install but AFTER.

It is possible to try it by commenting the line `upgrade_system` in the latest lines of the `neutrinet.sh` script you can find in `/root/neutrinet.sh` if you followed our [installation guide lines](install).

At https://wiki.neutrinet.be/cube/install#configure-your-cube-for-use-with-neutrinet before you run *./neutrinet.sh*, you do the following:
```
nano ./neutrinet.sh
```
Press *ctrl+V* until you hit the bottom, then change

```
get_variables

modify_hosts
set_locales
upgrade_system

postinstall_yunohost
create_yunohost_user
add_labriqueinternet_app_list
install_vpnclient
configure_vpnclient
install_hotspot
configure_hostpot
install_doctorcube
install_neutrinet_ynh

remove_dyndns_cron
restart_api

display_win_message
```
to
```
get_variables

modify_hosts
set_locales
#upgrade_system

postinstall_yunohost
upgrade_system
create_yunohost_user
add_labriqueinternet_app_list
install_vpnclient
configure_vpnclient
install_hotspot
configure_hostpot
install_doctorcube
install_neutrinet_ynh

remove_dyndns_cron
restart_api

display_win_message
```
Notice that the *upgrade_system* line is now active under the *postinstall_yunohost* line.
Then press *ctrl+X*, type *Y* , press enter and continue where you left off by running *./neutrinet.sh*. 

If it still fails, you can also try playing around a bit commenting and uncommenting lines to narrow down where something goes wrong.