<!-- TITLE: Passer commande pour un VPN -->
# 0. Où commencer ?

- ici : https://vpn.neutrinet.be

# 1. Email

Utilisez une de vos adresses mail parce qu'elle sera utilisée :

- comme nom d'utilisateur pour votre VPN
- comme nom d'utilisateur pour https://user.neutrinet.be/
- pour vous envoyer le lien de téléchargement de vos certificats.


*Si vous n'utilisez pas une adresse existante, le seul moyen d'obtenir vos certificats pour le VPN, sera de vous connecter sur https://user.neutrinet.be APRÈS avois terminé la procédure jusqu'au bout.*

![Vpn 01 Email](/uploads/vpn/vpn-01-email.png "Vpn 01 Email"){.align-center}


# 2. Mot de passe 

Choisissez un mot de passe, plus c'est long, mieux c'est.

- ce sera votre mot de passe pour le VPN
- également celui pour accéder à https://user.neutrinet.be/

![Vpn 02 Password](/uploads/vpn/vpn-02-password.png "Vpn 02 Password"){.align-center}

# 3. Méthodes d'identification

En théorie vous pouvez utiliser votre carte d'identité élecronique belge.  Mais je n'ai jamais essayé, alors choisissez [Use a password]


Si vous tenez à utiliser votre carte d'identité, jetez un coup d'œil [ici](https://eid.belgium.be/fr) en premier.

![Vpn 03 Identification](/uploads/vpn/vpn-03-identification.png "Vpn 03 Identification"){.align-center}

# 4. Sans BeID ... enregistrement manuel

Remplissez le formulaire avec vos coordonnées.

**Rappel**, nous fournissons un service chiffré de transport de vos données, non un outil d'amonyni... d'aminony ... d'anonymisation... donc à priori vous devriez être ... vous.

![Vpn 04 Manual Registration](/uploads/vpn/vpn-04-manual-registration.png "Vpn 04 Manual Registration"){.align-center}
# 5. Installation (Setup)

Il existe deux méthodes *(les 19 autres sont tenues secrêtes ;5 )* pour générer votre paire de clé publique et privée.

- Utiliser votre carte d'identité (voir point 3) ou bien, 
- Générer votre clé vous même et envoyer le contenu CSR.

![Vpn 05 Setup](/uploads/vpn/vpn-05-setup.png "Vpn 05 Setup"){.align-center}

# 6. Génération de la paire de clés

**Rappel**, nous fournissons un transfort chiffré derrière sur une IP fixe, pas un service complet d'anonymisation... donc votre IP est votre responsabilité.

- Sur votre PC (GNU/Linux supposé), il faut créer un dossier et rentré dedans.  Il contiendra les fichiers importants pour votre VPN.
- Générez votre paire de clé à l'aide de votre CSR comme expliqué sur le site.
  - vous êtes entrain de fabriquer votre clé publique et votre clé privée
  - les informations que vous fournissez peuvent ou non vous représenter
  - merci de renseigner l'adresse mail utilisée au point 1 en tant que FQDN et EMAIL, ce sera plus facile pour vous aider en cas de problème.
  - n'utilisez PAS de *chalenge password* 

![Vpn 06 A Keypair](/uploads/vpn/vpn-06-a-keypair.png "Vpn 06 A Keypair"){.align-center}
![Vpn 06 B Keypair](/uploads/vpn/vpn-06-b-keypair.png "Vpn 06 B Keypair"){.align-center}
![Vpn 06 C Keypair Csr](/uploads/vpn/vpn-06-c-keypair-csr.png "Vpn 06 C Keypair Csr"){.align-center}

# 7. Preque fini!

- activez IPv4 (si vous avez besoin d'une IPv4) et cela vous attribuera VOTRE IPv4.
- l'IPv6 est générée automatiquement et assignée lors de votre connexion VPN.
- ne vous inquiétez pas du champ *«review your information».
- cochez la case *«Stuff for nerdz»*, c'est IMPORTANT pour recevoir le bon lien de téléchargement dans votre mail.

![Vpn 07 A Almost Done](/uploads/vpn/vpn-07-a-almost-done.png "Vpn 07 A Almost Done"){.align-center}
![Vpn 07 B Almost Done Filed](/uploads/vpn/vpn-07-b-almost-done-filed.png "Vpn 07 B Almost Done Filed"){.align-center}

# 8.  Terminè!

- vérifiez votre boite de réception ou votre dossier spam.

![Vpn 08 Done](/uploads/vpn/vpn-08-done.png "Vpn 08 Done"){.align-center}