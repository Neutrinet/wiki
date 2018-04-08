<!-- TITLE: Schéma des services -->
<!-- SUBTITLE: Schéma des services de Neutrinet  -->

# Schéma des services
Cette page est en chantier, elle a pour but de décrire les différents services de Neutrinet et comment ils sont interconnectés.

![Schema Services Neutrinet](/uploads/infra/schema-services-neutrinet.png "Schema Services Neutrinet")


## Entités
- Membre: un membre qui accède aux services de Neutrinet
- members.neutrinet.be: l'app qui permettre au membre de gérer son compte (création/mise à jour). La liste des choses que cette app fera/ne fera pas sera discutée plus tard, partons de l'idée qu'elle a pour principal but ici de créer/modifier un membre dans le LDAP
- OpenVPN: notre serveur VPN
- FreeRadius: notre serveur d'Authentification/Authorization/Accounting pour OpenVPN
- LDAP: annuaire où est stockée la liste des membres