---
title: "Azure Service Fabric で構成ファイルをパラメーター化する方法 | Microsoft Docs"
description: "Service Fabric で構成ファイルをパラメーター化する方法を示します"
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 1e7d59ecb231440711b8ed3dc0b27a2b105890c4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric で構成ファイルをパラメーター化する方法

この記事では、Service Fabric で構成ファイルをパラメーター化する方法について説明します。

## <a name="procedure-for-parameterizing-configuration-files"></a>構成ファイルをパラメーター化する手順

この例では、アプリケーションの展開でパラメーターを使って構成値を上書きします。

1. Config\Settings.xml ファイルを開きます。
1. 次の XML を追加して、構成パラメーターを設定します。

    ```xml
      <Section Name="MyConfigSection">
        <Parameter Name="CacheSize" Value="25" />
      </Section>
    ```

1. ファイルを保存して閉じます。
1. `ApplicationManifest.xml` ファイルを開きます。
1. `ConfigOverride` 要素を追加し、構成パッケージ、セクション、およびパラメーターを参照します。

      ```xml
        <ConfigOverrides>
          <ConfigOverride Name="Config">
              <Settings>
                <Section Name="MyConfigSection">
                    <Parameter Name="CacheSize" Value="[Stateless1_CacheSize]" />
                </Section>
              </Settings>
          </ConfigOverride>
        </ConfigOverrides>
      ```

1. ApplicationManifest.xml ファイルの `Parameters` 要素でパラメーターを指定します。

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" />
      </Parameters>
    ```

1. `DefaultValue` を定義します。

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" DefaultValue="80" />
      </Parameters>
    ```

> [!NOTE]
> ConfigOverride を追加すると、Service Fabric は常に、アプリケーション パラメーターか、アプリケーション マニフェストで指定されている既定値を選びます。
>
>

Cloud.xml 発行プロファイルを使って Visual Studio からアプリケーションを発行すると、サービスはポート 80 を使うように構成されます。 MyWebAPI_PortNumber パラメーターを指定しないでアプリケーションを展開すると、サービスはポート 8080 を使います。

## <a name="next-steps"></a>次の手順
この記事で説明されている主要な概念の一部について詳しくは、「[複数の環境のアプリケーション パラメーターを管理する](service-fabric-manage-multiple-environment-app-configuration.md)」をご覧ください。

Visual Studio で使用可能なその他のアプリケーション管理機能については、 [Visual Studio での Service Fabric アプリケーションの管理](service-fabric-manage-application-in-visual-studio.md)に関する記事をご覧ください。