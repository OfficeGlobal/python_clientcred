# Python クライアントの資格情報サンプル #

これは、Python/Django アプリにクライアント資格情報 OAuth2 フローを実装する方法を示す非常に簡単なサンプルです。このアプリを使用すると、管理者がログオンして同意することができ、ユーザーは組織内の任意のユーザーの受信トレイ内の最新の 10 件のメールを見ることができます。

## 必要なソフトウェア ##

- [Python 3.4.2](https://www.python.org/downloads/)
- [Django 1.7.1](https://docs.djangoproject.com/en/1.7/intro/install/)
- [共有s:HTTP for Humans](http://docs.python-requests.org/en/latest/)
- [Python-RSA](http://stuvel.eu/rsa)

## サンプルの実行 ##

始める前に、Python と Django がインストールされていると仮定します。Windows ユーザーは、Python インストール ディレクトリと Scripts サブディレクトリを PATH 環境変数に追加する必要があります。

1. サンプル プロジェクトをダウンロードまたは分岐します。
1. `manage.py` が存在するディレクトリに、コマンド プロンプトまたはシェルを開きます。
1. BAT ファイルを実行できる場合は、setup\_project.bat を実行します。それ以外の場合は、手動で 3 つのコマンドを実行します。最後のコマンドでは、後でログオンするために使用するスーパーユーザーを作成するように求められます。
1. インストールを要求します。コマンド ラインからの HTTP for Humans モジュール: `pip install requests`
1. コマンド ラインからの Python-RSA モジュールのインストール: `pip install rsa`
1. [Azure Active Directory にアプリを登録します](https://github.com/jasonjoh/office365-azure-guides/blob/master/RegisterAnAppInAzure.md)。アプリは、サインオン URL が「http://127.0.0.1:8000/」の Web アプリとして登録されている必要があります。また、\[組織内のすべてのメールボックス内のメールを読み取る] のアクセス許可が付与されている必要があります。このアクセス許可は、\[アプリケーションのアクセス許可] ドロップダウン リストから使用できます。
1. [ここ](https://blogs.msdn.microsoft.com/exchangedev/2015/01/21/building-daemon-or-service-apps-with-office-365-mail-calendar-and-contacts-apis-oauth2-client-credential-flow/)に記載されている指示に従って、アプリ用の X509 証明書を構成します。
    > OpenSSL を使用している場合は、[次の手順](https://gist.github.com/carlopires/de085999dc69a13efe60)を試してください。(Carlo に感謝します!)
1. 証明書から RSA 形式の秘密キーを抽出して、PEM ファイルに保存します。(これを OpenSSL で行いました)
`openssl pkcs12 -in <path to PFX file> -nodes -nocerts -passin pass:<cert password> | openssl rsa -out appcert.pem`
1. `.\clientcreds\clientreg.py` ファイルを編集します。 
	1. アプリの登録時に取得したアプリのクライアント ID をコピーし、`ID` 変数の値として貼り付けます。 
	1. RSA 秘密キーを含む PEM ファイルへのフル パスを `cert_file_path` 変数の値として入力します。
	1. 証明書の拇印の値 (アプリケーション マニフェストの `customKeyIdentifier` 値に使用される値と同じ値) をコピーし、`cert_file_thumbprint` 変数の値として貼り付けます。
	1. ファイルを保存します。
1. 開発サーバーを起動します。`python manage.py runserver`
1. 出力は次のようになります。
`システム チェックを実行します...`
    
    `システム チェックで問題が特定されませんでした (0が無音)。`
	`2014 年 12 月 18 日 - 12:36:32`
	`Django バージョン 1.7.1、設定「pythoncontacts.settings」を使用`
	`http://127.0.0.1:8000/ で開発サーバーを起動`
	`CTRL-BREAK でサーバーを終了`。
1. ブラウザーを使用して http://127.0.0.1:8000/ に移動します。
1. これで、管理者アカウントでログインするように求められます。リンクをクリックして、Office 365 テナント管理者アカウントでログインします。
2. メール ページにリダイレクトする必要があります。Office 365 テナントにユーザーの有効なメール アドレスを入力し、\[ユーザーの設定] ボタンをクリックします。このページには、ユーザーの最新の 10 件のメールが読み込まれます。

## 著作権 ##

Copyright (c) Microsoft.All rights reserved.

----------
Twitter ([@JasonJohMSFT](https://twitter.com/JasonJohMSFT)) でぜひフォローしてください。

[Exchange 開発ブログ](http://blogs.msdn.com/b/exchangedev/)をフォローする
