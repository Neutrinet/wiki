<!-- TITLE: Comptabilit -->
<!-- SUBTITLE: A quick summary of Comptabilit -->

# Outil de comptabilité

L'outil est composé de 2 interfaces, une dite "publique" et une dite "d'admin".

## Interface publique

Elle est accessible ici: https://accounting.neutrinet.be.

Elle permet à tout le monde de voir toutes les transactions bancaires (anonymisées) du compte de Neutrinet.
Il est possible de filtrer les transactions par année.

![01-interface-publique](https://user-images.githubusercontent.com/3520/59200458-8d189880-8b98-11e9-8754-59ee5bbd2fac.png)

Lors de l'impression de la partie public (via la commande Fichier > Imprimer) du navigateur, le menu sera masqué et l'adresse du siège social de Neutrinet sera affiché en en-tête. (un PDF généré à partir de ça devrait suffire pour déposer les comptes)

![02-interface-publique-impression](https://user-images.githubusercontent.com/3520/59200518-ba654680-8b98-11e9-8c38-fb1609411013.png)

## Interface privée

Elle est accessible ici: https://accounting.neutrinet.be/admin.

Elle affiche ces mêmes transactions, mais de manière non-anonymisées. Elle permet également de filtrer les transactions par année, mais aussi par "type" de transaction (cotisation VPN, commande de brique, ...).

![03-interface-admin](https://user-images.githubusercontent.com/3520/59200567-d2d56100-8b98-11e9-85ba-5e034110534e.png)

Elle permet:
- d'importer des transactions à partir du fichier CSV fourni par ING
- manuellement ajouter/modifier/effacer une transaction

### Import de nouvelles transactions

Il faut d'abord se rendre sur le Home Bank ING et exporter les transactions passées.

XXX (capture de l'interface d'ING pour télécharger le CSV)

Cliquez ensuite sur le menu "Imports".

![04-imports-menu](https://user-images.githubusercontent.com/3520/59200623-f3052000-8b98-11e9-8853-a1a5489a5d6e.png)

Là, sélectionnez le fichier CSV et cliquez sur le bouton "Upload".

![05-import-form](https://user-images.githubusercontent.com/3520/59200653-01ebd280-8b99-11e9-96dd-5766f20220be.png)

![06-imported](https://user-images.githubusercontent.com/3520/59200690-14fea280-8b99-11e9-97fe-c6c8ba7dfe94.png)

L'outil détecte les transactions déjà présentes, et ne les uploade pas 2 fois.
Si le CSV que vous importez contient des transactions qui ont déjà été créées lors d'un précédent import, l'outil ne les importera pas.

L'outil détecte le type de la plupart des transactions, mais il arrive que certaines transactions ne soient pas identifiables par l'outil.

Pour n'afficher que les transactions "inconnues", cliquer sur "Tous les mouvements > Inconnu".

![07-filter-unknown-transactions](https://user-images.githubusercontent.com/3520/59200732-2778dc00-8b99-11e9-9d39-87fd54cf0fe5.png)

### Modifier une transaction

Pour manuellement assigner un "type" à une transaction, il faut l'éditer en cliquant sur le lien qui est sur la date de chaque transaction.

![08-modify-transaction-link](https://user-images.githubusercontent.com/3520/59200909-8fc7bd80-8b99-11e9-86dd-2ff4608c1e79.png)

Il est alors possible de choisir un autre "type" pour la transaction. Si vous souhaitez choisir un type qui n'est pas présent dans la liste, choisir "Type > Custom Label" et utiliser le champs "Custom Label" juste en dessous.

![09-modify-transaction-form](https://user-images.githubusercontent.com/3520/59200952-a3732400-8b99-11e9-92d8-929f013f43f4.png)

### Manuellement ajouter une transaction

Il peut être utile de manuellement ajouter une transaction qui ne serait pas présente sur le compte (par exemple pour corriger une différence entre les transactions et l'argent qui est réellement présent sur le compte).

Pour cela, il faut cliquer sur le bouton "New":

![10-new-transaction-link](https://user-images.githubusercontent.com/3520/59200979-b2f26d00-8b99-11e9-93c9-f094acc07d7f.png)

Une fois sur le formulaire, remplir les champs et cliquer sur le bouton "Create".

**Remarque**: il ne peut pas y avoir 2 transactions avec le même numéro de transaction le même jour.

![11-new-transaction-form](https://user-images.githubusercontent.com/3520/59201011-c56ca680-8b99-11e9-9602-6b40309987ee.png)

