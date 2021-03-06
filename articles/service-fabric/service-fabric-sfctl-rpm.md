---
title: Azure Service Fabric CLI- sfctl rpm| Microsoft Docs
description: "Service Fabric CLI sfctl rpm のコマンドについて説明します。"
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/26/2017
ms.author: ryanwi
ms.openlocfilehash: 2a9a565cf2c20490475d1b73b0f99fbe63e57dd3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-rpm"></a>sfctl rpm
クエリを実行し、修復マネージャー サービスにコマンドを送信します。

## <a name="commands"></a>コマンド
|コマンド|[説明]|
| --- | --- |
|    approve-force| 指定した修復タスクの承認を強制します。|
|    削除       | 完了した修復タスクを削除します。|
|    list         | 指定されたフィルターに一致する修復タスクの一覧を取得します。|

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
完了した修復タスクを削除します。

この API は、Service Fabric プラットフォームをサポートしています。コードから直接使用するものではありません。 

### <a name="arguments"></a>引数
|引数|[説明]|
| --- | --- |
|    --task-id [必須]| 削除する完了した修復タスクの ID。|
|    --version           | 修復タスクの現在のバージョン番号。 0 以外の場合、この値が修復タスクの実際の現行バージョンと一致する場合にのみ要求が成功します。 0 の場合、バージョン チェックは実行されません。|

### <a name="global-arguments"></a>グローバル引数
|引数|[説明]|
| --- | --- |
|    --debug             | すべてのデバッグ ログを表示するため、ログ記録の詳細度を上げます。|
|    --help -h           | このヘルプ メッセージを表示して終了します。|
|    --output -o         | 出力形式。  使用できる値は、json、jsonc、table、tsv です。  既定値は json です。
|    --query             | JMESPath クエリ文字列。 詳細と例については、http://jmespath.org/ を参照してください。|
|    --verbose           | ログ記録の詳細度を上げます。 すべてのデバッグ ログを得るには --debug を使用します。|


## <a name="sfctl-rpm-list"></a>sfctl rpm list
指定されたフィルターに一致する修復タスクの一覧を取得します。

この API は、Service Fabric プラットフォームをサポートしています。コードから直接使用するものではありません。 

### <a name="arguments"></a>引数
|引数|[説明]|
| --- | --- |
|    --executor-filter| 要求したタスクを一覧に含める必要がある修復 Executor の名前。|
|    --state-filter   | どのタスクの状態を結果リストに含める必要があるかを指定する、次の値の bitwise-OR。 - 1 - 作成済み - 2 - 要求済み - 4 - 準備中 - 8 - 承認済み - 16 - 実行中 - 32 - 修復中 - 64 - 完了。|
|    --task-id-filter | 照合される修復タスク ID プレフィックス。|

### <a name="global-arguments"></a>グローバル引数
|引数|[説明]|
| --- | --- |
|    --debug          | すべてのデバッグ ログを表示するため、ログ記録の詳細度を上げます。|
|    --help -h        | このヘルプ メッセージを表示して終了します。|
|    --output -o      | 出力形式。  使用できる値は、json、jsonc、table、tsv です。  既定値| json です。|
|    --query          | JMESPath クエリ文字列。 詳細と例については、http://jmespath.org/ を参照してください。|
|    --verbose        | ログ記録の詳細度を上げます。 すべてのデバッグ ログを得るには --debug を使用します。|

## <a name="next-steps"></a>次の手順
- Service Fabric CLI を[セットアップ](service-fabric-cli.md)します。
- [サンプル スクリプト](/azure/service-fabric/scripts/sfctl-upgrade-application)を使用して、Service Fabric CLI の使用方法を学習します。