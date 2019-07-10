<!-- TITLE: Outil Accounting -->
<!-- SUBTITLE: A quick summary of Comptabilit -->

# Outil de comptabilité

L'outil est composé de 2 interfaces, une dite "publique" et une dite "d'admin".

## Interface publique

Elle est accessible ici: https://accounting.neutrinet.be.

Elle permet à tout le monde de voir toutes les transactions bancaires (anonymisées) du compte de Neutrinet.
Il est possible de filtrer les transactions par année.

![01 Interface Publique](/uploads/outil-compta/01-interface-publique.png "01 Interface Publique")

Lors de l'impression de la partie public (via la commande Fichier > Imprimer) du navigateur, le menu sera masqué et l'adresse du siège social de Neutrinet sera affiché en en-tête. (un PDF généré à partir de ça devrait suffire pour déposer les comptes)

![02 Interface Publique Impression](/uploads/outil-compta/02-interface-publique-impression.png "02 Interface Publique Impression")

## Interface privée

Elle est accessible ici: https://accounting.neutrinet.be/admin.

Elle affiche ces mêmes transactions, mais de manière non-anonymisées. Elle permet également de filtrer les transactions par année, mais aussi par "type" de transaction (cotisation VPN, commande de brique, ...).

![03 Interface Admin](/uploads/outil-compta/03-interface-admin.png "03 Interface Admin")

Elle permet:
- d'importer des transactions à partir du fichier CSV fourni par ING
- manuellement ajouter/modifier/effacer une transaction

### Import de nouvelles transactions

Il faut d'abord se rendre sur le [Home Bank ING](https://www.ing.be/fr/business/login):

sur l'écran de connection, choisir "Business Bank" (et pas "Home Bank")

![Ing Connection](/uploads/outil-compta/ing-connection.png "Ing Connection")

et exporter les transactions passées:

![Ing](/uploads/outil-compta/ing.png "Ing")

Cliquez ensuite sur le menu "Imports".

![04 Imports Menu](/uploads/outil-compta/04-imports-menu.png "04 Imports Menu")

Là, sélectionnez le fichier CSV et cliquez sur le bouton "Upload".

![05 Import Form](/uploads/outil-compta/05-import-form.png "05 Import Form")

![06 Imported](/uploads/outil-compta/06-imported.png "06 Imported")

L'outil détecte les transactions déjà présentes, et ne les uploade pas 2 fois.
Si le CSV que vous importez contient des transactions qui ont déjà été créées lors d'un précédent import, l'outil ne les importera pas.

L'outil détecte le type de la plupart des transactions, mais il arrive que certaines transactions ne soient pas identifiables par l'outil.

Pour n'afficher que les transactions "inconnues", cliquer sur "Tous les mouvements > Inconnu".

![07 Filter Unknown Transactions](/uploads/outil-compta/07-filter-unknown-transactions.png "07 Filter Unknown Transactions")

### Modifier une transaction

Pour manuellement assigner un "type" à une transaction, il faut l'éditer en cliquant sur le lien qui est sur la date de chaque transaction.

![08 Modify Transaction Link](/uploads/outil-compta/08-modify-transaction-link.png "08 Modify Transaction Link")

Il est alors possible de choisir un autre "type" pour la transaction. Si vous souhaitez choisir un type qui n'est pas présent dans la liste, choisir "Type > Custom Label" et utiliser le champs "Custom Label" juste en dessous.

![09 Modify Transaction Form](/uploads/outil-compta/09-modify-transaction-form.png "09 Modify Transaction Form")

### Manuellement ajouter une transaction

Il peut être utile de manuellement ajouter une transaction qui ne serait pas présente sur le compte (par exemple pour corriger une différence entre les transactions et l'argent qui est réellement présent sur le compte).

Pour cela, il faut cliquer sur le bouton "New":

![10 New Transaction Link](/uploads/outil-compta/10-new-transaction-link.png "10 New Transaction Link")

Une fois sur le formulaire, remplir les champs et cliquer sur le bouton "Create".

**Remarque**: il ne peut pas y avoir 2 transactions avec le même numéro de transaction le même jour.

![11 New Transaction Form](/uploads/outil-compta/11-new-transaction-form.png "11 New Transaction Form")

