---
title: "Azure Functions の host.json のリファレンス"
description: "Azure Functions の host.json ファイルのリファレンス ドキュメント"
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/09/2017
ms.author: tdykstra
ms.openlocfilehash: 522d0590595b0fc0fef503599f1677658f223bd8
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2017
---
# <a name="hostjson-reference-for-azure-functions"></a>Azure Functions の host.json のリファレンス

*host.json* メタデータ ファイルには、関数アプリのすべての関数に影響するグローバル構成オプションが含まれています。 この記事では、使用できる設定の一覧を紹介します。 JSON スキーマは、http://json.schemastore.org/host にあります。

[アプリ設定](functions-app-settings.md)と [local.settings.json](functions-run-local.md#local-settings-file) ファイルには、他のグローバル構成オプションがあります。

## <a name="sample-hostjson-file"></a>サンプル host.json ファイル

次のサンプル *host.json* ファイルには、すべての使用できるオプションが指定されています。

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

この記事の次のセクションでは、最上位レベルの各プロパティについて説明します。 特記がない場合は、いずれも省略可能です。

## <a name="aggregator"></a>aggregator

[Application Insights のメトリックを計算する](functions-monitoring.md#configure-the-aggregator)ときに集計される関数呼び出しの数を指定します。 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|プロパティ  |既定値 | 説明 |
|---------|---------|---------| 
|batchSize|1,000|集計する要求の最大数。| 
|flushTimeout|00:00:30|集計する最長期間。| 

2 つの制限のいずれかに最初に達した場合、関数呼び出しが集計されます。

## <a name="applicationinsights"></a>applicationInsights

[Application Insights のサンプリング機能](functions-monitoring.md#configure-sampling)を制御します。

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|プロパティ  |既定値 | 説明 |
|---------|---------|---------| 
|isEnabled|false|サンプリングを有効または無効にします。| 
|maxTelemetryItemsPerSecond|5|サンプリングが開始されるしきい値。| 

## <a name="eventhub"></a>eventHub

[Event Hub トリガーとバインディング](functions-bindings-event-hubs.md)の構成設定。

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functions

ジョブのホストが実行される関数の一覧。  空の配列は、すべての関数を実行することを示します。  [ローカルで実行する](functions-run-local.md)場合にのみ使用します。 関数アプリでは、*function.json* の `disabled` プロパティを使用します。*host.json* のこのプロパティは使用しません。

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

すべての関数のタイムアウト期間を示します。 従量課金プランの有効な範囲は 1 秒から 10 分であり、既定値は 5 分です。 App Service プランに制限はありません。既定値は null です (タイムアウトなしを示します)。

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="http"></a>http

[http トリガーとバインディング](functions-bindings-http-webhook.md)の構成設定。

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

ジョブ ホストの一意の ID。 ダッシュを削除した小文字の GUID を指定できます。 ローカルで実行しているときに必要です。 Azure Functions で実行する場合、`id` を省略すると、ID は自動的に生成されます。

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>logger

[ILogger object](functions-monitoring.md#write-logs-in-c-functions) から出力されたログまたは [context.log](functions-monitoring.md#write-logs-in-javascript-functions) ログのフィルターを制御します。

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|プロパティ  |既定値 | 説明 |
|---------|---------|---------| 
|categoryFilter|該当なし|カテゴリ別のフィルターを指定します| 
|defaultLevel|情報|`categoryLevels` 配列に指定されていないカテゴリの場合、このレベル以上のログを Application Insights に送信します。| 
|categoryLevels|該当なし|各カテゴリの Application Insights に送信される最小ログ レベルを指定するカテゴリの配列。 ここで指定されるカテゴリは、同じ値で始まるすべてのカテゴリを制御し、長い値の方が優先されます。 前述のサンプル *host.json* ファイルでは、`Information` レベルの "Host.Aggregator" で始まるすべてのカテゴリ。 `Error` レベルのログである、"Host.Executor" など "Host" で始まるその他すべてのカテゴリ。| 

## <a name="queues"></a>queues

[Storage キュー トリガーとバインディング](functions-bindings-storage-queue.md)の構成設定。

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|プロパティ  |既定値 | 説明 |
|---------|---------|---------| 
|maxPollingInterval|60000|キューのポーリングの最大間隔 (ミリ秒)。| 
|visibilityTimeout|0|メッセージの処理が失敗したときの再試行間隔。| 
|batchSize|16|並列で取得および処理されるキュー メッセージ数。 最大値は 32 です。| 
|maxDequeueCount|5|有害キューに移動する前に、メッセージの処理を試行する回数。| 
|newBatchThreshold|batchSize/2|新しいメッセージのバッチを取得するしきい値。| 

## <a name="servicebus"></a>serviceBus

[Service Bus トリガーとバインディング](functions-bindings-service-bus.md)の構成設定。

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

シングルトン ロック動作の構成設定。 詳細については、「[GitHub issue about singleton support](https://github.com/Azure/azure-webjobs-sdk-script/issues/912)」(シングルトンのサポートに関する GitHub の問題) を参照してください。

```json
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|プロパティ  |既定値 | 説明 |
|---------|---------|---------| 
|lockPeriod|00:00:15|関数レベルのロックの取得期間。 ロックの自動更新。| 
|listenerLockPeriod|00:01:00|リスナーのロックの取得期間。| 
|listenerLockRecoveryPollingInterval|00:01:00|スタートアップ時にリスナーのロックを獲得できなかった場合に、リスナーのロックの回復に使用される時間間隔。| 
|lockAcquisitionTimeout|00:01:00|ランタイムがロックの獲得を試行する最長時間。| 
|lockAcquisitionPollingInterval|該当なし|ロックの獲得の試行間隔。| 

## <a name="tracing"></a>tracing

`TraceWriter` オブジェクトを使用して作成するログの構成設定。 [C# のログの記録](functions-reference-csharp.md#logging)と [Node.js のログの記録](functions-reference-node.md#writing-trace-output-to-the-console)に関するページを参照してください。 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|プロパティ  |既定値 | 説明 |
|---------|---------|---------| 
|consoleLevel|info|コンソール ログのトレース レベル。 オプションは、`off`、`error`、`warning`、`info`、および `verbose` です。|
|fileLoggingMode|debugOnly|ファイルのログ記録のトレース レベル。 オプションは、`never`、`always`、`debugOnly` です。| 

## <a name="watchdirectories"></a>watchDirectories

変更を監視する[共有コード ディレクトリ](functions-reference-csharp.md#watched-directories)のセット。  これらのディレクトリ内のコードを変更した場合に、関数によって変更を選択するようにします。

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="durabletask"></a>durableTask

[Durable Functions](durable-functions-overview.md) の[タスク ハブ](durable-functions-task-hubs.md)。

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

タスク ハブの名前は、先頭文字をアルファベットとする必要があります。また、使用できるのはアルファベットと数値だけです。 指定しない場合、関数アプリの既定のタスク ハブ名は **DurableFunctionsHub** です。 詳細については、[タスク ハブ](durable-functions-task-hubs.md)に関するページをご覧ください。


## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [host.json ファイルを更新する方法について説明します](functions-reference.md#fileupdate)。

> [!div class="nextstepaction"]
> [環境変数のグローバル設定を参照してください。](functions-app-settings.md)
