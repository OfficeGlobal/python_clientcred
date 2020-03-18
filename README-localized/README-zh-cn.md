# Python 客户端凭据示例 #

这是一个非常粗糙的示例，说明如何在 Python/Django 应用中实现客户端凭据 OAuth2 流。该应用允许管理员登录并授予许可，然后允许用户查看组织中任何用户的收件箱中的前 10 封电子邮件。

## 所需软件 ##

- [Python 3.4.2](https://www.python.org/downloads/)
- [Django 1.7.1](https://docs.djangoproject.com/en/1.7/intro/install/)
- [请求：HTTP for Humans](http://docs.python-requests.org/en/latest/)
- [Python-RSA](http://stuvel.eu/rsa)

## 运行本示例 ##

在开始之前，假定你已安装了 Python 和 Django。Windows 用户应将 Python 安装目录和脚本子目录添加到其 PATH 环境变量中。

1. 下载示例项目或为其创建分支。
1. 在 `manage.py` 所在的目录中打开命令提示符或 shell。
1. 如果可以运行 BAT 文件，请运行 setup\_project.bat。如果没有，请手动运行文件中的三个命令。最后一个命令将提示你创建超级用户，稍后将使用它登录。
1. 安装请求：HTTP for Humans 模块，方法是在命令行中运行：`pip install requests`
1. 从命令行安装 Python-RSA 模块：`pip install rsa`
1. [在 Azure Active Directory 中注册应用](https://github.com/jasonjoh/office365-azure-guides/blob/master/RegisterAnAppInAzure.md)。该应用应注册为登录 URL 为“http://127.0.0.1:8000/”的 Web 应用，并应获得“读取该组织中所有邮箱的邮件”的权限，该权限可在“应用程序权限”下拉列表中找到。
1. 按照[此处](https://blogs.msdn.microsoft.com/exchangedev/2015/01/21/building-daemon-or-service-apps-with-office-365-mail-calendar-and-contacts-apis-oauth2-client-credential-flow/)的说明为你的应用配置 X509 证书。
    > 如果你使用的是 OpenSSL，可以尝试按照[这些说明](https://gist.github.com/carlopires/de085999dc69a13efe60)操作。（感谢 Carlo！）
1. 从证书提取 RSA 格式的私钥，并将其保存到 PEM 文件。（我使用了 OpenSSL 执行此操作）。
`openssl pkcs12 -in <path to PFX file> -nodes -nocerts -passin pass:<cert password> | openssl rsa -out appcert.pem`
1. 编辑 `.\clientcreds\clientreg.py` 文件。 
	1. 复制在应用注册期间获取的应用客户端 ID，并将其粘贴作为 `id` 变量的值。 
	1. 输入包含 RSA 私钥的 PEM 文件的完整路径作为 `cert_file_path` 变量的值。
	1. 复制证书的指纹值（此值也是用于应用程序清单中的 `customKeyIdentifier` 值的值），并将其粘贴作为 `cert_file_thumbprint` 变量的值。
	1. 保存文件。
1. 启动开发服务器：`python manage.py runserver`
1. 你应该会看到如下所示的输出：
`Performing system checks...`
    
    `System check identified no issues (0 silenced).`
	`December 18, 2014 - 12:36:32`
	`Django version 1.7.1, using settings'pythoncontacts.settings'`
	`Starting development server at http://127.0.0.1:8000/`
	`Quit the server with CTRL-BREAK.`
1. 使用浏览器访问 http://127.0.0.1:8000/。
1. 现在应该会提示你使用管理帐户进行登录。单击链接执行此操作并使用 Office 365 租户管理员帐户进行登录。
2. 随后应该会重定向到邮件页面。输入 Office 365 租户中某个用户的有效电子邮件地址，然后单击“设置用户”按钮。用户最新的 10 封电子邮件应该会加载到页面上。

## 版权信息 ##

版权所有 (c) Microsoft。保留所有权利。

----------
在 Twitter 上通过 [@JasonJohMSFT](https://twitter.com/JasonJohMSFT) 与我联系

关注 [Exchange 开发人员博客](http://blogs.msdn.com/b/exchangedev/)
