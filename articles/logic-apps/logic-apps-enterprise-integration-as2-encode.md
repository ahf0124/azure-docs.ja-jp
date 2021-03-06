---
title: "AS2 メッセージをエンコードする - Azure Logic Apps | Microsoft Docs"
description: "Azure Logic Apps で Enterprise Integration Pack の AS2 エンコーダーを使用する方法"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: bccc8f81a3a664e090796ae8a3cbb38c890c0479
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Enterprise Integration Pack を使用して Azure Logic Apps の AS2 メッセージをエンコードする

メッセージを転送するときに、セキュリティと信頼性を確保するには、Encode AS2 Message コネクタを使用します。 このコネクタでは、デジタル署名、暗号化、およびメッセージ処理通知 (MDN) による受信確認が可能になります。これにより、否認防止もサポートできるようになります。

## <a name="before-you-start"></a>開始する前に

必要な項目を次に示します。

* Azure アカウント。[無料アカウント](https://azure.microsoft.com/free)を作成できます。
* 既に定義され、Azure サブスクリプションに関連付けられている[統合アカウント](logic-apps-enterprise-integration-create-integration-account.md)。 Encode AS2 Message コネクタを使用するには、統合アカウントが必要です。
* 統合アカウントで既に定義されている 2 つ以上の[パートナー](logic-apps-enterprise-integration-partners.md)
* 統合アカウントで既に定義されている [AS2 契約](logic-apps-enterprise-integration-as2.md)

## <a name="encode-as2-messages"></a>AS2 メッセージをエンコードする

1. [ロジック アプリを作成](quickstart-create-first-logic-app-workflow.md)します。

2. Encode AS2 Message コネクタには、トリガーがありません。そのため、要求トリガーのように、ロジック アプリを起動するためのトリガーを追加する必要があります。 ロジック アプリ デザイナーで、ロジック アプリにトリガーを追加して、アクションを追加します。

3.  検索ボックスに、フィルターとして「AS2」と入力します。 **[AS2 - Encode AS2 Message (AS2 - AS2 メッセージのエンコード)]** を選択します。
   
    !["AS2" の検索](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. 以前に統合アカウントへの接続を作成していない場合は、ここでその接続を作成するように求められます。 接続の名前を指定し、接続する統合アカウントを選択します。 
   
    ![create connection to integration account](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    アスタリスクが付いているプロパティは必須です。

    | プロパティ | 詳細 |
    | --- | --- |
    | 接続名 * |接続の任意の名前を入力します。 |
    | 統合アカウント * |統合アカウントの名前を入力します。 統合アカウントとロジック アプリが同じ Azure の場所にあることを確認してください。 |

5.  完了したら、接続の詳細は次の例のようになります。 接続の作成を完了するには、**[作成]** を選択します。
   
    ![統合の接続の詳細](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. 接続が作成されたら、次の例に示すように、**AS2-From** と **AS2-To 識別子**の詳細を契約で構成されているとおりに入力し、**本文** (メッセージ ペイロード) の詳細を指定します。
   
    ![provide mandatory fields](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>AS2 エンコーダーの詳細

Encode AS2 コネクタは次のタスクを実行します。 

* AS2/HTTP ヘッダーを適用する
* 送信メッセージに署名する (構成されている場合)
* 送信メッセージを暗号化する (構成されている場合)
* メッセージを圧縮する (構成されている場合)

## <a name="try-this-sample"></a>このサンプルの試用

完全に動作するロジック アプリとサンプルの AS2 シナリオのデプロイを試すには、[AS2 ロジック アプリのテンプレートとシナリオ](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/)を参照してください。

## <a name="view-the-swagger"></a>Swagger の表示
[Swagger の詳細](/connectors/as2/)を参照してください。 

## <a name="next-steps"></a>次の手順
[Enterprise Integration Pack についての詳細情報](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack についての詳細情報") 

