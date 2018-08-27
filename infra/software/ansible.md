<!-- TITLE: Ansible -->
<!-- SUBTITLE: Utilisation d'Ansible -->

# Français
## Description
><a href="https://docs.ansible.com/#project" target="_blank">Ansible</a> est un programme écrit en Python, il permet l'exécution de scripts, principalement pour l'exécution de tâches administratives à partir d'une liste de machines cibles. ( <a href="https://docs.ansible.com/ansible/latest/index.html" target="_blank">Pour la doc c'est ici</a> )
>Ansible peut-être utilisé pour : 
>
>	- La gestion de la configuration des machines.
>	- Le déploiement d'application.
>	- L'approvisionnement de services.
>	- L'orchestration des parcs informatiques.
>	- La gestion des cycles de fourniture de services.
>	- La gestion sécuritaire et la conformité des configurations d'un parc informatique.
>
>Ansible ne fonctionne pas sous forme de démon ( en tout cas dans sa version communautaire ), il ne nécessite donc pas une machine dédiée. Il permet donc une délégation des tâches administratives en le configurant de manière à s'exécuter sur des postes d'utilisateurs différents en partageant une même base de scripts ( Les scripts étant appelés playbooks dans le jargon d'Ansible ).
>Ansible s'exécute séquentiellement de machine hôte en machine hôte. Il est de ce fait adapté aux petites infrastructures.
>Ansible ne nécessite pas d'agent spécifique : sous linux, il utilise SSH pour se connecter aux machines cibles.
>Les playbooks sont des fichiers YAML en plein texte ( lisible par des êtres humains ) qui décrivent l'état désiré de quelque-chose. Ces fichiers peuvent être utilisés pour construire des environnements d'applications complets.
>Ansible utilises des variables sources qui peuvent être utilisées dans :
> - Les playbooks.
> - les fichiers ( utilisés par les playbooks ).
> - Les inventaires ( group vars, host vars )
> - La ligne de commande.
> - Les variables découvertes lors de l'exécution d'Ansible.
> - Ansible Tower ( partie non communautaire que nous n'utilisons pas )
> Les playbooks sont appliqués aux éléments décrits dans les fichiers d'inventaires.
> Les inventaires sont des listes d'éléments : Lignes statiques de serveurs, listes dynamiques de serveurs et ranges réseaux.
>** En résumé :**
> Les playbooks contiennent des jeux de tâches ( jeux de tâches = Plays en jargon Ansible )
> Les Plays contiennent des tâches ( tâches = Tasks )
> Ces tâches exécutent des Modules. 
> Les "Tasks" sont exécutées séquentiellement.
> Une tâche spécifique ( appelée Handlers ) est exécutée après les "Tasks", ce Handlers est exécuté une fois à la fin de chaque "Plays". Cette tâche sert par exemple à redémarrer un service une fois que les différents modules de configurations ont été exécutés.    
> - Les rôles sont des playbooks spéciaux autonome, qui contiennent la descriptions des tâches (taks), des variables, des modèles de configurations et autres fichiers. 
		
## Historique

  * Vers 2016 on a commencé à utiliser Ansible.
  * En prévision du remplacement de nos serveurs, on a organiser un premier [week-end](/pvs/2018/08-25-weekend-ansible)

## Important

  En utilisant Ansible ou toute autre solution similaire visant à automatiser les tâches d'administrations, il est **fortement déconseillé** ou plutôt **interdit** de modifier la configuration d'un serveur en éditant les fichiers de configurations manuellement.

## Actualité



