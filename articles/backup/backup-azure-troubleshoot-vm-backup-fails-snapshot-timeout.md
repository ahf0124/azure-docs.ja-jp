---
title: "Azure Backup の失敗のトラブルシューティング: ゲスト エージェントの状態を確認できない | Microsoft Docs"
description: "エージェント、拡張機能、ディスクに関する Azure Backup のエラーの症状、原因、解決策"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: "Azure Backup; VM エージェント; ネットワーク接続;"
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: 0be2391268e11593802cb0f455e8c4553f0d4731
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Azure Backup の失敗のトラブルシューティング: エージェント/拡張機能に関する問題

この記事では、VM エージェントと拡張機能との通信の問題に関連する Backup のエラーを解決するためのトラブルシューティング手順について説明します。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM エージェントが Azure Backup と通信できない

> [!NOTE]
> Azure Linux VM バックアップで 2018 年 1 月 4 日以降にエラーが発生した場合、影響を受けた VM で次のコマンドを実行し、バックアップを再試行してください

    sudo rm -f /var/lib/waagent/*.[0-9]*.xml

Azure Backup サービスに VM を登録してスケジュール設定すると、Backup サービスは、VM エージェントと通信してジョブを開始し、ポイントインタイム スナップショットを作成します。 以下のいずれかの状況によって、スナップショットをトリガーできず、バックアップ エラーにつながる場合があります。 以下のトラブルシューティングの手順を指定の順序で実行してから、操作を再試行してください。

##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>原因 1: [VM がインターネットにアクセスできない](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>原因 2: [エージェントが VM にインストールされているが応答しない (Windows VM の場合)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>原因 3: [VM にインストールされているエージェントが古くなっている (Linux VM の場合)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>原因 4: [スナップショットの状態を取得できないか、スナップショットを作成できない](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>原因 5: [バックアップ拡張機能の更新または読み込みに失敗した](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-6-azure-classic-vms-may-require-additional-step-to-complete-registrationazure-classic-vms-may-require-additional-step-to-complete-registration"></a>原因 6: [Azure クラシック VM で登録を完了するために追加の手順が必要な可能性がある](#azure-classic-vms-may-require-additional-step-to-complete-registration)

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>仮想マシンがネットワークに接続していないためにスナップショット操作が失敗した
Azure Backup サービスに VM を登録して、スケジュール設定すると、Backup サービスは、VM のバックアップ拡張機能と通信してジョブを開始し、ポイントインタイム スナップショットを作成します。 以下のいずれかの状況によって、スナップショットをトリガーできず、バックアップ エラーにつながる場合があります。 以下のトラブルシューティングの手順を指定の順序で実行してから、操作を再試行してください。
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>原因 1: [VM がインターネットにアクセスできない](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>原因 2: [スナップショットの状態を取得できないか、スナップショットを作成できない](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>原因 3: [バックアップ拡張機能の更新または読み込みに失敗した](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot 拡張機能の操作に失敗した

Azure Backup サービスに VM を登録して、スケジュール設定すると、Backup サービスは、VM のバックアップ拡張機能と通信してジョブを開始し、ポイントインタイム スナップショットを作成します。 以下のいずれかの状況によって、スナップショットをトリガーできず、バックアップ エラーにつながる場合があります。 以下のトラブルシューティングの手順を指定の順序で実行してから、操作を再試行してください。
##### <a name="cause-1-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>原因 1: [スナップショットの状態を取得できないか、スナップショットを作成できない](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>原因 2: [バックアップ拡張機能の更新または読み込みに失敗した](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>原因 3: [VM がインターネットにアクセスできない](#the-vm-has-no-internet-access)
##### <a name="cause-4-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>原因 4: [エージェントが VM にインストールされているが応答しない (Windows VM の場合)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>原因 5: [VM にインストールされているエージェントが古くなっている (Linux VM の場合)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-to-perform-the-operation-as-the-vm-agent-is-not-responsive"></a>VM エージェントが応答していないために操作を実行できない

Azure Backup サービスに VM を登録して、スケジュール設定すると、Backup サービスは、VM のバックアップ拡張機能と通信してジョブを開始し、ポイントインタイム スナップショットを作成します。 以下のいずれかの状況によって、スナップショットをトリガーできず、バックアップ エラーにつながる場合があります。 以下のトラブルシューティングの手順を指定の順序で実行してから、操作を再試行してください。
##### <a name="cause-1-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>原因 1: [エージェントが VM にインストールされているが応答しない (Windows VM の場合)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>原因 2: [VM にインストールされているエージェントが古くなっている (Linux VM の場合)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>原因 3: [VM がインターネットにアクセスできない](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-the-operation-in-a-few-minutes"></a>バックアップ操作は内部エラーのため失敗しました - 数分以内に操作をやり直してください

Azure Backup サービスに VM を登録して、スケジュール設定すると、Backup サービスは、VM のバックアップ拡張機能と通信してジョブを開始し、ポイントインタイム スナップショットを作成します。 以下のいずれかの状況によって、スナップショットをトリガーできず、バックアップ エラーにつながる場合があります。 以下のトラブルシューティングの手順を指定の順序で実行してから、操作を再試行してください。
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>原因 1: [VM がインターネットにアクセスできない](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-installed-in-the-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>原因 2: [エージェントが VM にインストールされているが応答しない (Windows VM の場合)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>原因 3: [VM にインストールされているエージェントが古くなっている (Linux VM の場合)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>原因 4: [スナップショットの状態を取得できないか、スナップショットを作成できない](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>原因 5: [バックアップ拡張機能の更新または読み込みに失敗した](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-6-backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lockbackup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>原因 6: [リソース グループのロックが原因で、Backup サービスに古い復元ポイントを削除するためのアクセス許可がない](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)

## <a name="the-specified-disk-configuration-is-not-supported"></a>指定されたディスク構成がサポートされていません

> [!NOTE]
> 1 TB を超えるディスクが存在する VM のバックアップをサポートするためのプライベート プレビューがあります。 詳しくは、[大容量ディスク VM バックアップ サポートのプライベート プレビュー](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)に関するページをご覧ください。
>
>

現在 Azure Backup は [1023 GB を超える](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm)ディスク サイズをサポートしていません。 
- 1 TB を超えるディスクがある場合は、1 TB より小さい[新規ディスクを接続](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal)してください。 <br>
- 次に、1 TB を超えるディスクから、新規作成した 1 TB より小さいサイズのディスクにデータをコピーします。 <br>
- すべてのデータがコピーされたことを確認し、1 TB を超えるディスクを取り外します。
- バックアップを開始します。

## <a name="causes-and-solutions"></a>原因とソリューション

### <a name="the-vm-has-no-internet-access"></a>VM がインターネットにアクセスできない
デプロイ要件によっては、VM がインターネットにアクセスできない場合や、設定されている制限により、Azure インフラストラクチャにアクセスできない場合があります。

バックアップ拡張機能が正常に機能するには、Azure のパブリック IP アドレスへの接続が必要です。 この拡張機能が、VM のスナップショットを管理するコマンドを、Azure Storage エンドポイント (HTTP URL) に送信するためです。 拡張機能がパブリック インターネットにアクセスできない場合は、Backup は最終的に失敗します。

####  <a name="solution"></a>解決策
この問題を解決するには、次の方法のいずれかを試してください。
##### <a name="allow-access-to-the-azure-storage-corresponding-to-the-region"></a>リージョンに対応する Azure Storage へのアクセスを許可する

[サービス タグ](../virtual-network/security-overview.md#service-tags)を使用して、特定のリージョンのストレージに接続できます。 ストレージ アカウントへのアクセスを許可するルールが、インターネット アクセスをブロックするルールよりも優先度が高いことを確認してください。 

![リージョンのストレージ タグが与えられた NSG](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> ストレージ サービス タグは特定のリージョンでのみ利用できます。また、これはプレビュー版です。 リージョンの一覧については、「[ストレージのサービス タグ](../virtual-network/security-overview.md#service-tags)」を参照してください。

##### <a name="create-a-path-for-http-traffic-to-flow"></a>フローに対する HTTP トラフィック用のパスを作成する

1. ネットワーク制限 (ネットワーク セキュリティ グループなど) を設定している場合は、トラフィックをルーティングするための HTTP プロキシ サーバーをデプロイします。
2. HTTP プロキシからインターネットへのアクセスを許可するには、規則をネットワーク セキュリティ グループに追加しましす (ネットワーク セキュリティ グループがある場合)。

VM バックアップの HTTP プロキシを設定する方法については、「[Azure 仮想マシンをバックアップする環境の準備](backup-azure-arm-vms-prepare.md#establish-network-connectivity)」を参照してください。

Managed Disks を使用している場合、ファイアウォールに追加のポート (8443) が開かれている必要がある場合があります。

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>エージェントが VM にインストールされているが応答しない (Windows VM の場合)

#### <a name="solution"></a>解決策
VM エージェントが破損しているまたはサービスが停止している可能性があります。 VM エージェントを再インストールすると、最新バージョンを取得し、通信を再開するのに役立ちます。

1. 仮想マシンのサービス (services.msc) で Windows ゲスト エージェント サービスが実行されているかどうかを確認します。 Windows ゲスト エージェント サービスの再起動と Backup の開始を試みます。<br>
2. Windows ゲスト エージェント サービスがサービスに表示されない場合は、このサービスがインストールされているかどうかを [プログラムと機能] で確認します。
4. プログラムと機能に表示されない場合、Windows ゲスト エージェントをアンインストールします。
5. [最新バージョンのエージェント MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) をダウンロードしてインストールします。 インストールを実行するには、管理者特権が必要です。
6. これでサービスに Windows ゲスト エージェント サービスが表示されます。
7. ポータルで「今すぐバックアップ」をクリックして、オンデマンド/アドホック バックアップをお試しください。

また、仮想マシンの**[システム内に .NET 4.5 がインストールされている](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**ことを確認します。 これは、VM エージェントがサービスと通信するために必要です。

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM にインストールされているエージェントが古くなっている (Linux VM の場合)

#### <a name="solution"></a>解決策
Linux VM の場合、エージェントに関連するエラーまたは拡張機能に関連するエラーのほとんどは、古い VM エージェントに影響する問題が原因で発生します。 この問題のトラブルシューティングを行うには、次の一般的なガイドラインに従います。

1. [Linux VM エージェントを更新](../virtual-machines/linux/update-agent.md)する手順に従います。

 >[!NOTE]
 >ディストリビューション リポジトリを通してのみエージェントを更新することを "*強くお勧め*" します。 エージェント コードを直接 GitHub からダウンロードして、更新することはお勧めしません。 最新のエージェントをディストリビューションで使用できない場合は、そのエージェントをインストールする方法をディストリビューション サポートにお問い合わせください。 最新のエージェントを確認するには、GitHub リポジトリの [Windows Azure Linux エージェント](https://github.com/Azure/WALinuxAgent/releases)のページをご覧ください。

2. `ps -e` コマンドを実行して、Azure エージェントが VM で実行されていることを確認します。

 このプロセスが実行されていない場合は、次のコマンドを使用してプロセスを再起動します。

 * Ubuntu の場合: `service walinuxagent start`
 * その他のディストリビューションの場合: `service waagent start`

3. [エージェントの自動再起動を構成します](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash)。
4. 新しいテスト バックアップを実行します。 エラーが解決しない場合は、お客様の VM から次のログを収集してください。

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

waagent の詳細ログが必要な場合は、次の手順に従います。

1. /etc/waagent.conf ファイルで、次の行を見つけます: **Enable verbose logging (y|n)**
2. **Logs.Verbose** の値を *n* から *y* に変更します。
3. 変更を保存した後、このセクションの前の手順に従って waagent を再起動します。

### <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>スナップショットの状態を取得できないか、スナップショットを作成できない
VM のバックアップは、基礎となるストレージ アカウントへのスナップショット コマンドの発行に依存します。 ストレージ アカウントにアクセスできないか、スナップショット タスクの実行が遅れているために、Backup が失敗することがあります。

#### <a name="solution"></a>解決策
次の場合にスナップショットのタスクが失敗することがあります。

| 原因 | 解決策 |
| --- | --- |
| VM で SQL Server のバックアップが構成されている。 | 既定では、VM のバックアップは Windows VM で VSS 完全バックアップとして実行されます。 SQL Server ベースのサーバーを実行し、SQL Server のバックアップが構成されている VM では、スナップショットの実行の遅延が発生する場合があります。<br><br>スナップショットに関する問題により Backup エラーが発生する場合は、次のレジストリ キーを設定してください。<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| VM が RDP でシャットダウンされているため、VM の状態が正しく報告されない。 | リモート デスクトップ プロトコル (RDP) で VM をシャットダウンした場合は、ポータルでその VM の状態が正しいかどうかを確認します。 正しくない場合は、VM のダッシュボードの **[シャットダウン]** オプションを使用して、ポータルで VM をシャットダウンします。 |
| VM が DHCP からホスト/ファブリック アドレスを取得できない。 | IaaS VM バックアップが正しく機能するには、ゲスト内で DHCP が有効になっている必要があります。  VM が DHCP 応答 245 からホスト/ファブリック アドレスを取得できない場合は、拡張機能をダウンロードしたり実行したりできません。 静的プライベート IP が必要な場合は、プラットフォームを通じて構成する必要があります。 VM 内の DHCP オプションは有効のままにしておいてください。 詳細については、[静的内部プライベート IP の設定](../virtual-network/virtual-networks-reserved-private-ip.md)に関するページをご覧ください。 |

### <a name="the-backup-extension-fails-to-update-or-load"></a>バックアップ拡張機能の更新または読み込みに失敗した
拡張機能を読み込むことができないと、スナップショットを作成できないため、Backup が失敗します。

#### <a name="solution"></a>解決策

**Windows ゲストの場合:** iaasvmprovider サービスが有効になっていて、そのスタートアップの種類が *[自動]* であることを確認します。 サービスがこのように構成されていない場合は、そのサービスを有効にして、次のバックアップが成功するかどうかを確認します。

**Linux ゲストの場合:** 最新バージョンの VMSnapshot for Linux (Backup で使用される拡張機能) が 1.0.91.0 であることを確認します。<br>


バックアップ拡張機能の更新または読み込みがまだ失敗する場合は、VMSnapshot 拡張機能をインストールして強制的に再度読み込まれるようにします。 次回のバックアップ時に、拡張機能が再度読み込まれます。

拡張機能をアンインストールするには、以下の手順を実行します。

1. [Azure ポータル](https://portal.azure.com/)にアクセスします。
2. バックアップの問題が発生している VM を見つけます。
3. **[設定]**をクリックします。
4. **[拡張機能]** をクリックします。
5. **[Vmsnapshot 拡張機能]** をクリックします。
6. **[アンインストール]** をクリックします。

この手順により、次回のバックアップ時に拡張機能が再インストールされます。

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>リソース グループのロックが原因で、Backup サービスに古い復元ポイントを削除するためのアクセス許可がない
この問題は、ユーザーがリソース グループをロックし、Backup サービスが古い復元ポイントを削除でない管理対象 VM に固有です。 バックエンドから最大 18 個の復元ポイントの制限が課されているため、これにより新しいバックアップの開始が失敗します。

#### <a name="solution"></a>解決策

この問題を解決するには、次の手順を使用して復元ポイント コレクションを削除します。 <br>
 
1. VM が存在するリソース グループのロックを削除します 
     
2. Chocolatey を使用して ARMClient をインストールします <br>
   https://github.com/projectkudu/ARMClient
     
3. ARMClient にログインします <br>
             `.\armclient.exe login`
         
4. VM に対応する復元ポイント コレクションを取得します <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    例: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
             
5. 復元ポイント コレクションを削除します <br>
            `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
 
6. 次のスケジュールされたバックアップで、復元ポイント コレクションと新しい復元ポイントが自動的に作成されます 
 
7. 復元ポイントの上限は 18 個であり、これに達するとバックアップの開始に失敗するため、リソース グループをもう一度ロックすると問題が再現します 

