---
title: "Azure に Python Web アプリを作成する| Microsoft Docs"
description: "Azure App Service の Web Apps で、初めての Python の Hello World を数分でデプロイします。"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/26/2017
ms.author: cephalin;cfowler
ms.custom: mvc, devcenter
ms.openlocfilehash: 34e26e9dfdf53536c21023b9519209c474253b25
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-python-web-app-in-azure"></a>Azure に Python Web アプリを作成する

[Azure Web Apps](app-service-web-overview.md) では、高度にスケーラブルな自己適用型の Web ホスティング サービスを提供しています。  このクイック スタートでは、Python アプリを開発して Azure Web Apps にデプロイする方法について説明します。 [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) を使って Web アプリを作成し、Git を使用して Web アプリに Python のサンプル コードをデプロイします。

![Azure で実行されるサンプル アプリ](media/app-service-web-get-started-python/hello-world-in-browser.png)

以下の手順は、Mac、Windows、または Linux コンピューターを使用して実行できます。 前提条件のインストールを終えてから、以降の手順を完了するまでに約 5 分かかります。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、以下が必要です。

* <a href="https://git-scm.com/" target="_blank">Git をインストールする</a>
* <a href="https://www.python.org/downloads/" target="_blank">Python をインストールする</a>

## <a name="download-the-sample"></a>サンプルのダウンロード

ターミナル ウィンドウで、次のコマンドを実行して、サンプル アプリのリポジトリをローカル コンピューターに複製します。

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

サンプル コードが含まれているディレクトリに移動します。

```bash
cd python-docs-hello-world
```

## <a name="run-the-app-locally"></a>アプリをローカルで実行する

`pip` を使って必要なパッケージをインストールします。

```bash
pip install -r requirements.txt
```

ターミナル ウィンドウを開き、`Python` コマンドを使って組み込みの Python Web サーバーを起動して、ローカルでアプリケーションを実行します。

```bash
python main.py
```

Web ブラウザーを開き、`http://localhost:5000` のサンプル アプリに移動します。

ページに表示されているサンプル アプリの **Hello World** メッセージが表示されます。

![ローカルで実行されるサンプル アプリ](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

ターミナル ウィンドウで **Ctrl + C** キーを押して、Web サーバーを終了します。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

## <a name="create-a-web-app"></a>Web アプリを作成する

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-python-no-h.md)]

新しく作成された Web アプリに移動します。 _&lt;app name>_ は、アプリの一意の名前に置き換えてください。

```bash
http://<app name>.azurewebsites.net
```

![空の Web アプリ ページ](media/app-service-web-get-started-python/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)]

```bash
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>アプリの参照

Web ブラウザーを使用して、デプロイされたアプリケーションを参照します。

```bash
http://<app_name>.azurewebsites.net
```

Python のサンプル コードは、Azure App Service の Web アプリで実行されています。

![Azure で実行されるサンプル アプリ](media/app-service-web-get-started-python/hello-world-in-browser.png)

**お疲れさまでした。** App Service に初めての Python アプリをデプロイしました。

## <a name="update-and-redeploy-the-code"></a>コードを更新して再デプロイする

ローカルのテキスト エディターを使用して、Python アプリ内の `main.py` ファイルを開き、`return` ステートメントの横のテキストを少し変更します。

```python
return 'Hello, Azure!'
```

ローカル ターミナル ウィンドウで、変更を Git にコミットし、コード変更を Azure にプッシュします。

```bash
git commit -am "updated output"
git push azure master
```

デプロイが完了したら、「[アプリの参照](#browse-to-the-app)」の手順で開いたブラウザー ウィンドウに戻り、ページを更新します。

![Azure で実行される更新済みのサンプル アプリ](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>新しい Azure Web アプリを管理する

<a href="https://portal.azure.com" target="_blank">Azure Portal</a> に移動し、作成した Web アプリを管理します。

左側のメニューで **[App Services]** をクリックした後、Azure Web アプリの名前をクリックします。

![Azure Web アプリへのポータル ナビゲーション](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Web アプリの [概要] ページを確認します。 ここでは、参照、停止、開始、再開、削除のような基本的な管理タスクを行うことができます。

![Azure Portal の App Service ブレード](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

左側のメニューは、アプリを構成するためのさまざまなページを示しています。

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [既存のカスタム DNS 名を Azure Web Apps にマップする](app-service-web-tutorial-custom-domain.md)
