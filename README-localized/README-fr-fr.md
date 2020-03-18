# Exemple d’informations d’identification du client Python #

Il s’agit d’un exemple très simple illustrant l’implémentation du flux d’Oauth2 d’informations d’identification client dans une application python/django. L’application permet à un administrateur d’ouvrir une session et de donner son accord, puis autorise l’utilisateur à afficher les 10 premiers e-mails dans la boîte de réception des utilisateurs de l’organisation.

## Logiciels requis ##

- [Python 3.4.2](https://www.python.org/downloads/)
- [Django 1.7.1](https://docs.djangoproject.com/en/1.7/intro/install/)
- [Requests : HTTP for Humans](http://docs.python-requests.org/en/latest/)
- [Python-RSA](http://stuvel.eu/rsa)

## Exécution de l’exemple ##

Il est supposé que Python et Django sont installés avant de commencer. Les utilisateurs de Windows doivent ajouter le répertoire d'installation Python et le sous-répertoire Scripts à leur variable d'environnement PATH.

1. Téléchargez ou dupliquez l’exemple de projet.
1. Ouvrez votre invite de commandes ou votre shell dans le répertoire dans lequel se trouve `manage.py`.
1. Si vous pouvez exécuter des fichiers BAT, exécutez setup\_project. bat. Si ce n’est pas le cas, exécutez manuellement les trois commandes dans le fichier. La dernière commande vous invite à créer un superutilisateur que vous utiliserez par la suite pour vous connecter.
1. Installez la bibliothèque Requests : HTTP for Humans depuis la ligne de commande : `pip install requests`
1. Installez le module Python-RSA depuis la ligne de commande : `pip install rsa`
1. [Inscrivez l’application dans Azure Active Directory](https://github.com/jasonjoh/office365-azure-guides/blob/master/RegisterAnAppInAzure.md). L’application doit être inscrite comme une application web avec une URL de connexion « http://127.0.0.1:8000/ ». Elle doit recevoir l’autorisation « Lire le courrier dans toutes les boîtes au lettres de l’organistion », disponible dans la liste déroulante « Autorisations des application ».
1. Configurez un certificat x509 pour votre application en suivant les instructions [ici](https://blogs.msdn.microsoft.com/exchangedev/2015/01/21/building-daemon-or-service-apps-with-office-365-mail-calendar-and-contacts-apis-oauth2-client-credential-flow/).
    > Si vous utilisez OpenSSL, vous pouvez essayer [ces instructions](https://gist.github.com/carlopires/de085999dc69a13efe60). (Merci à Carlo !)
1. Extrayez la clé privée au format RSA de votre certificat et enregistrez-la dans un fichier PEM. (J’ai utilisé OpenSSL pour effectuer cette opération).
`OpenSSL PKCS12-in <chemin d’accès au fichier PFX> -nodes -nocerts -passin pass : < mot de passe de certificat > | openssl rsa -out appcert.pem`
1. Modifiez le fichier `.\clientcreds\clientreg.py`. 
	1. Copiez l’ID client de votre application obtenue durant l’inscription de l’application et collez-le à l’emplacement de la valeur correspondant à la variable `id`. 
	1. Entrez le chemin d’accès complet du fichier PEM contenant la clé privée RSA comme valeur pour la variable `cert_file_path`.
	1. Copiez la valeur d’empreinte de votre certificat (même valeur utilisée pour la valeur `customKeyIdentifier` dans le manifeste d’application) et collez-la en tant que valeur pour la variable `cert_file_thumbprint`.
	1. Enregistrez le fichier.
1. Démarrez le serveur de développement : `python manage.py runserver`
1. Vous devriez voir une sortie comme suit :
`Exécution de vérifications système...`
    
    `La vérification système n’a identifié aucun problème (0 silencieux).`
	`18 décembre 2014-12:36:32`
	`Django version 1.7.1, àl’aide des paramètres « pythoncontacts.settings »`
	`Démarrage du serveur de développement sur http://127.0.0.1:8000/`
	`Quittez le serveur avec la combinaison de touches CTRL + BREAK.`
1. Utilisez votre navigateur pour accéder à http://127.0.0.1:8000/.
1. Vous devez maintenant être invité à vous connecter avec un compte administratif. Cliquez sur le lien pour le faire et connectez-vous avec un compte d’administrateur du locataire Office 365.
2. Vous devez être redirigé vers la page e-mail. Entrez une adresse de courrier valide pour un utilisateur dans le locataire Office 365 et cliquez sur le bouton « Définir un utilisateur ». Les 10 e-mails les plus récents pour l’utilisateur doivent être chargés sur la page.

## Copyright ##

Copyright (c) Microsoft. Tous droits réservés.

----------
Suivez-moi sur Twitter [@JasonJohMSFT](https://twitter.com/JasonJohMSFT)

Suivez le [blog de développement Exchange](http://blogs.msdn.com/b/exchangedev/)
