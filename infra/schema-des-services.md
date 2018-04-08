<!-- TITLE: Schéma des services -->
<!-- SUBTITLE: Schéma des services de Neutrinet  -->

# Schéma des services
Cette page est en chantier, elle a pour but de commencer une discussion au sujet des différents services de Neutrinet et comment ils sont interconnectés.
Le but final est de tomber d'accord sur:
- les services que l'on va installer (OpenVPN, FreeRadius, ...)
- le(s) datastore(s) que l'on va utiliser (LDAP, Postgres, ...)
- les applications que l'on va déveloper (members.neutrinet.be, ...)
- la manière dont ces différents services vont communiquer ensemble et avec le(s) datastore(s)

Cette page n'a pas pour but de:
- décrire l'infrastructure physique des serveurs
- décrire en détail les fonctionnalités de chaque service

![Schema Services Neutrinet](/uploads/infra/schema-services-neutrinet.png "Schema Services Neutrinet")

Ce schéma a été généré avec https://draw.io
Si vous voulez le modifier:
- rendez vous sur https://draw.io
- uploadez le fichier xml attaché ici plus bas
- faites vos modifications
- exportez un PNG pour la prévisualisation ici
- important: sauvez le schéma au format XML depuis https://draw.io, et uploadez le fichier XML ici pour que les autres puissent à leur tour le modifier
[Schema Services Neutrinet](/uploads/infra/schema-services-neutrinet.xml "Schema Services Neutrinet")

## Entités
- Membre: un membre qui accède aux services de Neutrinet
- members.neutrinet.be: l'app qui permettre au membre de gérer son compte (création/mise à jour). La liste des choses que cette app fera/ne fera pas sera discutée plus tard, partons de l'idée qu'elle a pour principal but ici de créer/modifier un membre dans le LDAP
- chat.neutrinet.be: notre chat. Nos autres services (e.g: NextCloud) se comporteraient de la même manière
- OpenVPN: notre serveur VPN
- FreeRadius: notre serveur d'Authentification/Authorization/Accounting pour OpenVPN
- LDAP: annuaire où est stockée la liste des membres

## Interactions
1. Un membre se crée un compte via members.neutrinet.be en spécifiant son email et mot de passe
2. members.neutrinet.be crée 2 entrées pour ce membre dans LDAP:
	a. son compte utilisateur pour les services de Neutrinet, qui sera utilisé pour se connecter à members.neutrinet.be, chat.neutrinet.be, ...
	b. un token qui sera utilisé pour la connection au VPN (l'idée est de séparer le mot de passe principal et celui utilisé pour le VPN)
3. Le membre se connecte au VPN avec son adresse email et le token qu'il obtient via members.neutrinet.be
4. OpenVPN délègue l'authentification à FreeRadius
5. FreeRadius vérifie l'email/token dans LDAP
6. Le membre se connecte à chat.neutrinet.be en utilisant son email et son mot de passe principal
7. chat.neutrinet.be vérifie l'email/mot de passe dans LDAP