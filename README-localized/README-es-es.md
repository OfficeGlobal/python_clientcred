# Ejemplo de credenciales del cliente de Python #

Este es un ejemplo muy básico en el que se muestra cómo implementar el flujo de OAuth2 de las credenciales de clientes en una aplicación de Python/Django. La aplicación permite a un administrador iniciar sesión y dar consentimiento. A continuación, permite al usuario ver los 10 primeros correos electrónicos de la bandeja de entrada de cualquier usuario de la organización.

## Software necesario ##

- [Python 3.4.2](https://www.python.org/downloads/)
- [Django 1.7.1](https://docs.djangoproject.com/en/1.7/intro/install/)
- [Requests: HTTP for Humans](http://docs.python-requests.org/en/latest/)
- [Python-RSA](http://stuvel.eu/rsa)

## Ejecución del ejemplo ##

Antes de empezar, se presupone que tiene Python y Django instalados. Los usuarios de Windows deben agregar el directorio de instalación de Python y el subdirectorio Scripts a su variable de entorno PATH.

1. Descargue o bifurque el proyecto de ejemplo.
1. Abra el símbolo del sistema o el Shell en el directorio donde se encuentra `manage.py`.
1. Si puede ejecutar archivos BAT, ejecute setup_project.bat. Si no es así, ejecute los tres comandos en el archivo manualmente. El último comando le pedirá que cree un superusuario, que usará más tarde para iniciar sesión.
1. Instale el módulo de Requests: HTTP for Humans desde la línea de comandos: `pip install requests`
1. Instale el módulo Python-RSA desde la línea de comandos: `pip install rsa`
1. [Registre la aplicación en Azure Active Directory](https://github.com/jasonjoh/office365-azure-guides/blob/master/RegisterAnAppInAzure.md). Se debe registrar la aplicación como una aplicación web con una dirección URL de inicio de sesión "http://127.0.0.1:8000/" y se le debe dar el permiso "Leer el correo en todos los buzones de la organización", que está disponible en la lista desplegable "Permisos de la aplicación".
1. Siga las direcciones en este [vínculo](https://blogs.msdn.microsoft.com/exchangedev/2015/01/21/building-daemon-or-service-apps-with-office-365-mail-calendar-and-contacts-apis-oauth2-client-credential-flow/) a fin de configurar un certificado X509 para la aplicación.
    > Si está usando OpenSSL, puede probar [estas instrucciones](https://gist.github.com/carlopires/de085999dc69a13efe60) (gracias a Carlo).
1. Extraiga la clave privada en formato RSA del certificado y guárdela en un archivo PEM. He usado OpenSSL para hacer esto:
`openssl pkcs12 -in <ruta al archivo PFX> -nodes -nocerts -passin pass:<contraseña del certificado> | openssl rsa -out appcert.pem`
1. Edite el archivo `.\clientcreds\clientreg.py`. 
	1. Copie el Id. de cliente de la aplicación obtenido durante el registro de la misma y péguelo como valor de la variable `id`. 
	1. Introduzca la ruta de acceso completa al archivo PEM que contiene la clave privada RSA como el valor para la variable `cert_file_path`.
	1. Copie el valor de la huella digital del certificado (el mismo valor que se utilizó para `customKeyIdentifier` en el manifiesto de la aplicación) y péguelo como el valor para la variable `cert_file_thumbprint`.
	1. Guarde el archivo.
1. Inicie el servidor de desarrollo: `python manage.py runserver`
1. Debe ver un resultado como el siguiente:
`Performing system checks...`
    
    `System check identified no issues (0 silenced).`
	`December 18, 2014 - 12:36:32`
	`Django version 1.7.1, using settings 'pythoncontacts.settings'`
	`Starting development server at http://127.0.0.1:8000/`
	`Quit the server with CTRL-BREAK.`
1. Use el explorador para ir a http://127.0.0.1:8000/.
1. Ahora, se le pedirá que inicie sesión con una cuenta de administrador. Haga clic en el vínculo para hacerlo e inicie sesión con una cuenta de administrador de espacio empresarial de Office 365.
2. Se le redirigirá a la página de correo electrónico. Escriba una dirección de correo electrónico válida para un usuario del espacio empresarial de Office 365 y haga clic en el botón "Establecer usuario". Los 10 correos electrónicos más recientes del usuario se cargarán en la página.

## Derechos de autor ##

Copyright (c) Microsoft. Todos los derechos reservados.

----------
Conectar conmigo en Twitter [@JasonJohMSFT](https://twitter.com/JasonJohMSFT)

Seguir el [blog de desarrollo de Exchange](http://blogs.msdn.com/b/exchangedev/)
