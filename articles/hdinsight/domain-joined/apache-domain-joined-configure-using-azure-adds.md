---
title: "Azure Active Directory Domain Services を使用してドメイン参加済み HDInsight クラスターを構成する - Azure | Microsoft Docs"
description: "Azure Active Directory Domain Services を使ったドメイン参加済み HDInsight クラスターの設定と構成の方法について説明します。"
services: hdinsight
documentationcenter: 
author: bprakash
manager: jhubbard
editor: cgronlun
tags: 
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: bhanupr
ms.openlocfilehash: 77478616eae27828a57a36dc0aaf3884e80ce403
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services を使用してドメイン参加済み HDInsight クラスターを構成する

ドメイン参加済みクラスターは、マルチユーザーのエンタープライズ セキュリティ機能を HDInsight で実現します。 ドメイン参加済み HDInsight クラスターは Active Directory ドメインに直接接続されるため、ドメイン ユーザーは、各自のドメイン資格情報を使用して、クラスターに対する認証を行い、ビッグ データ ジョブを実行することができます。 

ドメイン参加済み HDInsight クラスターが接続できるようにドメイン コントローラーを設定するには、2 つの方法があります。

- Azure Active Directory Domain Services (Azure AD DS)
- Azure IaaS VM 上の Active Directory ドメイン コントローラー

この記事では、Azure Active Directory Domain Services を使用して、ドメイン参加済み HDInsight クラスターを構成する方法について説明します。

## <a name="create-azure-adds"></a>Azure ADDS の作成

HDInsight クラスターを作成する前に、Azure AD DS を作成しておく必要があります。 Azure ADDS の作成については、「[Azure Portal を使用して Azure Active Directory Domain Services を有効にする](../../active-directory-domain-services/active-directory-ds-getting-started.md)」を参照してください。 

> [!NOTE]
> ドメイン サービスを作成する権限が与えられているのはテナント管理者だけです。 HDInsight の既定のストレージとして Azure Data Lake Storage (ADLS) を使用する場合は、ADLS の既定の Azure AD テナントが HDInsight クラスターのドメインと同じであることを確認します。 

ドメイン サービスのプロビジョニング後、HDInsight クラスターを作成するためのサービス アカウントを **Azure AD DC 管理者**グループに作成する必要があります。 サービス アカウントは、Azure AD のグローバル管理者であることが必要です。

## <a name="create-a-domain-joined-hdinsight-cluster"></a>ドメイン参加済み HDInsight クラスターの作成

次に、前のセクションで作成したサービス アカウントと AAD DS を使用して HDInsight クラスターを作成します。

Azure AD ドメイン サービスと HDInsight クラスターは、どちらも同じ Azure 仮想ネットワーク (VNet) に配置した方が簡単です。 両者を異なる VNet に配置する場合は、2 つの VNet をピアリングする必要があります。 詳細については、「[仮想ネットワーク ピアリング](../../virtual-network/virtual-network-peering-overview.md)」をご覧ください。

ドメイン参加済み HDInsight クラスターを作成するときは、次のパラメーターを指定する必要があります。

- **ドメイン名**: Azure AD DS に関連付けるドメイン名  (例: contoso.onmicrosoft.com)。
- **ドメイン ユーザー名**: 前のセクションで作成した Azure AD DC 管理者グループ内のサービス アカウント。 たとえば、「hdiadmin@contoso.onmicrosoft.com」のように入力します。このドメイン ユーザーは、このドメイン参加済み HDInsight クラスターの管理者になります。
- **ドメイン パスワード**: サービス アカウントのパスワード。
- **組織単位**: HDInsight クラスターで使用する OU の識別名。 たとえば「OU=HDInsightOU,DC=contoso,DC=onmicrosohift,DC=com」と入力します。この OU が存在しない場合、HDInsight クラスターがこの OU の作成を試みます。 
- **LDAPS URL**: たとえば、ldaps://contoso.onmicrosoft.com:636 です。
- **アクセス ユーザー グループ**: クラスターに同期させるユーザーが属しているセキュリティ グループ。 たとえば、HiveUsers です。 複数のユーザー グループを指定する場合は、コンマ (,) で区切ってください。
 
次のスクリーンショットは、Azure Portal における構成を示しています。

![Azure HDInsight ドメイン参加済み Active Directory Domain Services の構成](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png)が必要です。


## <a name="next-steps"></a>次の手順
* Hive ポリシーの構成と Hive クエリの実行については、[ドメイン参加済み HDInsight クラスターの Hive ポリシーの構成](apache-domain-joined-run-hive.md)に関する記事をご覧ください。
* SSH を使用してドメイン参加済み HDInsight クラスターに接続する方法については、「[Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)」を参照してください。

