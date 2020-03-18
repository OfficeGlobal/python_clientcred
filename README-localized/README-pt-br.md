# Exemplo de Credenciais de Cliente do Python #

Este é um exemplo muito rudimentar que ilustra como implementar o fluxo de credenciais do cliente OAuth2 em um aplicativo Python/Django. O aplicativo permite que um administrador faça logon e dê consentimento e então permite que o usuário visualize os primeiros 10 e-mails na caixa de entrada de qualquer usuário na organização.

## Software necessário ##

- [Python 3.4.2](https://www.python.org/downloads/)
- [Django 1.7.1](https://docs.djangoproject.com/en/1.7/intro/install/)
- [Solicitações: HTTP for Humans](http://docs.python-requests.org/en/latest/)
- [Python-RSA](http://stuvel.eu/rsa)

## Execução do exemplo ##

Presume-se que você tenha o Python e o Django instalados antes de começar. Os usuários do Windows devem adicionar a pasta de instalação do Python e o subdiretório Scripts à variável de ambiente PATH.

1. Baixe ou bifurque o projeto de exemplo.
1. Abra o seu prompt de comando ou o Shell no diretório onde `manage.py` está localizado.
1. Se você puder executar arquivos BAT, execute setup\_project. bat. Caso contrário, execute os três comandos automaticamente no arquivo. O último comando solicitará que você crie um superusuário, que será usado mais tarde para fazer logon.
1. Instale as Solicitações: o modulo HTTP for Humans na linha de comando: `pip install requests`
1. Instale o módulo Python-RSA da linha de comando: `pip install rsa`
1. [Registre o aplicativo no Azure Active Directory](https://github.com/jasonjoh/office365-azure-guides/blob/master/RegisterAnAppInAzure.md). É necessário registrar o aplicativo como um aplicativo Web usando a URL de Entrada "http://127.0.0.1:8000/" e atribuir a ele a permissão "Ler correio em todas as caixas de correio da organização", que está disponível na lista suspensa "Permissões delegadas".
1. Configure um certificado X509 para seu aplicativo seguindo as instruções [aqui](https://blogs.msdn.microsoft.com/exchangedev/2015/01/21/building-daemon-or-service-apps-with-office-365-mail-calendar-and-contacts-apis-oauth2-client-credential-flow/).
    > Se você estiver usando o OpenSSL, experimente [estas instruções](https://gist.github.com/carlopires/de085999dc69a13efe60). (Obrigado ao Carlo!)
1. Extraia a chave particular no formato RSA do seu certificado e salve-a em um arquivo PEM. (Usei o OpenSSL para fazer isso).
`openssl pkcs12 -in <path to PFX file> -nodes -nocerts -passin pass:<cert password> | openssl rsa -out appcert.pem`
1. Edite o arquivo `.\clientcreds\clientreg.py`. 
	1. Copie a ID do cliente obtida durante o registro do aplicativo e cole-a como o valor da variável `id`. 
	1. Insira o caminho completo para o arquivo PEM que contém a chave particular privada como o valor da `cert_file_path` variável.
	1. Copie o valor da impressão digital do seu certificado (mesmo valor usado para o `customKeyIdentifier` valor no manifesto do aplicativo) e cole-o como o valor da variável `cert_file_thumbprint`.
	1. Salve o arquivo.
1. Inicie o servidor de desenvolvimento: `python manage.py runserver`
1. Você deve ver um resultado como:
`Executando verificações do sistema...`
    
    `A verificação do sistema não identificou nenhum problema (0 silenciado).`
	`18 de dezembro de 2014 - 12:36:32`
	`Django versão 1.7.1, usando as configurações 'pythoncontacts.settings'`
	`Iniciando servidpr de desenvolvimento em http://127.0.0.1:8000/`
	`Saia do servidor com CTRL-BREAK.`
1. Use seu navegador para acessar http://127.0.0.1:8000/.
1. Agora, você deve ser avisado para fazer login com uma conta administrativa. Clique no link para fazê-lo e entre com uma conta de administrador de locatário do Office 365.
2. Você deve ser redirecionado para a página de e-mail. Insira um endereço de e-mail válido para um usuário no locatário do Office 365 e clique no botão "Definir Usuário". Os 10 e-mails mais recentes para o usuário devem ser carregados na página.

## Direitos autorais ##

Copyright (c) Microsoft. Todos os direitos reservados.

----------
Conecte-se comigo no Twitter [@JasonJohMSFT](https://twitter.com/JasonJohMSFT)

Siga o [Blog de desenvolvedores do Exchange](http://blogs.msdn.com/b/exchangedev/)
