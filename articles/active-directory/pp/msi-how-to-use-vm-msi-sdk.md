---
title: "VM 上で Azure SDK からユーザー割り当ての管理対象サービス ID を使用する方法"
description: "VM 上でユーザー割り当ての MSI を使って Azure SDK を使用する場合のコード サンプル"
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f9a31a0500a6f5f1c49fc45d5811e28788e6f2b1
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/22/2017
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>ユーザー割り当ての管理対象サービス ID (MSI) による Azure SDK の使用

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]この記事では、各 Azure SDK のユーザー割り当ての MSI サポートの使用例を示す SDK サンプルの一覧を示します。

## <a name="prerequisites"></a>前提条件

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - この記事のすべてのサンプル コード/スクリプトでは、クライアントが、MSI 対応の仮想マシンで実行されていることを前提としています。 お使いの VM にリモート接続するには、Azure ポータルで VM への "接続" 機能を使用します。 VM で MSI を有効にする方法の詳細については、「[Azure CLI を使用して、VM 管理対象サービス ID (MSI) を構成する](msi-qs-configure-cli-windows-vm.md)」、または関連する記事 (PowerShell、Azure Portal、テンプレート、または Azure SDK を使用) のいずれかを参照してください。 

## <a name="sdk-code-samples"></a>SDK コード サンプル

| SDK             | サンプル コード |
| --------------- | ----------- |
| Python          | [MSI を使用して VM 内から認証する](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [MSI 対応 VM からリソースを管理する](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>次の手順

- ライブラリのダウンロード、ドキュメントなどを含む Azure SDK リソースの完全な一覧については、「[Azure SDK](https://azure.microsoft.com/downloads/)」を参照してください。

Microsoft のコンテンツ改善のため、次のコメント セクションよりご意見をお寄せください。








