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