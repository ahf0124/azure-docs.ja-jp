---
title: "パフォーマンスに関する推奨事項 - Azure SQL Database | Microsoft Docs"
description: "Azure SQL Database は、現在のクエリのパフォーマンスを向上できる、SQL Database 向けの推奨事項を提供します。"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 09/20/2017
ms.author: sstein
ms.openlocfilehash: ea1069d4ec29ad66562a6798a8b13998d0d2ef89
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2018
---
# <a name="performance-recommendations"></a>パフォーマンスに関する推奨事項

Azure SQL Database は学習してアプリケーションに適応し、カスタマイズされた推奨事項を提供します。それによって、SQL Database のパフォーマンスを最大化できます。 SQL Database の使用状況履歴を分析することで、パフォーマンスが継続的に評価されます。 推奨事項はデータベース固有のワークロード パターンに基づき、そのパフォーマンス改善を支援します。

> [!TIP]
> [自動チューニング](sql-database-automatic-tuning.md)は、推奨されるパフォーマンス調整方法です。 [Intelligent Insights](sql-database-intelligent-insights.md) は、推奨されるパフォーマンス監視方法です。 
>

## <a name="create-index-recommendations"></a>インデックスの作成に関する推奨事項
Azure SQL Database は実行されるクエリを継続的に監視し、パフォーマンスを改善する可能性があるインデックスを特定します。 特定のインデックスが不足していることが確実になると、新しい**インデックスの作成**推奨事項が作成されます。 Azure SQL Database は、一定の期間後にインデックスがもたらすパフォーマンス増加を見積もることで確信度を高めます。 見積もられたパフォーマンス増加に基づき、推奨事項は高、中、低で分類されます。 

推奨事項を利用して作成されたインデックスには常に auto_created というフラグが設定されます。 sys.indexes ビューを見ることでどのインデックスが auto_created であるか確認できます。 自動作成されたインデックスは ALTER/RENAME コマンドをブロックしません。 自動作成されたインデックスを含む列をその上にドロップすると、コマンドが通過し、自動作成されたインデックスとコマンドがドロップされます。 通常のインデックスは、インデックスが付けられた列で ALTER/RENAME コマンドをブロックします。

インデックスの作成に関する推奨事項が適用されると、Azure SQL Database はクエリのパフォーマンスをベースライン パフォーマンスと比較します。 新しいインデックスがパフォーマンス改善をもたらした場合、推奨事項に成功フラグが設定され、影響レポートが入手可能になります。 インデックスが恩恵をもたらさなかった場合、自動的に元に戻されます。 Azure SQL Database ではこの方法で、データベース パフォーマンスが改善する場合にのみ推奨事項が利用されます。

**インデックスの作成**に関する推奨事項にはバックオフ ポリシーがあります。データベースまたはプールのリソース使用率が高すぎる場合、推奨事項は適用されません。 バックオフ ポリシーでは、CPU、データ IO、ログ IO、および使用可能な記憶領域が考慮されます。 過去 30 分以内の CPU、データ IO、またはログ IO の使用率が 80% を超える場合、インデックスの作成は延期されます。 インデックスの作成により使用可能な記憶領域が 10% を下回る場合は、推奨事項がエラー状態に変わります。 数日後に自動チューニングを行ってインデックスが有効であると示される場合には、プロセスが開始します。 このプロセスは、インデックスの作成に十分な使用可能領域がある限り、またはインデックスが有効でないと見なされない限り繰り返されます。

## <a name="drop-index-recommendations"></a>インデックスの削除に関する推奨事項
足りないインデックスの検出だけでなく、Azure SQL Database は既存インデックスのパフォーマンスを継続的に分析します。 インデックスが使用されない場合、Azure SQL Database はその削除を推奨します。 インデックスの削除は 2 つの場合に推奨されます。
* インデックスが別のインデックスの重複である (同じインデックスを付けて追加された列、パーティション スキーマ、フィルター)
* インデックスが長期間 (93 日間) 未使用である

インデックスの削除に関する推奨事項は、実施後に検証も通過します。 パフォーマンスが改善される場合、影響レポートが入手可能になります。 パフォーマンスの低下が検出された場合、推奨事項は戻されます。


## <a name="parameterize-queries-recommendations"></a>クエリのパラメーター化に関する推奨事項
継続的に再コンパイルされてはいるもののクエリ実行プランが同じままのクエリが 1 つ以上あると、**クエリのパラメーター化**に関する推奨事項が表示されます。 こういった条件により、強制パラメーター化の適用が可能になり、クエリ プランをキャッシュし再利用して、パフォーマンスの向上とリソースの使用量削減を実現することができます。 

SQL Server に対して発行されたすべてのクエリは、実行プランの生成のために、最初にコンパイルされる必要があります。 生成された各プランはプラン キャッシュに追加され、同じクエリのその後の実行では、キャッシュからプランを再利用できるため、追加のコンパイルの必要がなくなります 。 

パラメーター化されていない値を含むクエリを送信するアプリケーションは、パラメーター値の異なるすべてのクエリの実行プランが再コンパイルされる、パフォーマンス オーバーヘッドにつながる可能性があります。 多くの場合、パラメーター値の異なる同じクエリによって同一の実行プランが生成されますが、これらのプランは個別にプラン キャッシュに追加されます。 実行プランの再コンパイルにより、データベース リソースが消費され、クエリ実行時間の増加とプラン キャッシュのオーバーフローが発生するため、プランがキャッシュから削除される原因となります。 SQL Server のこの動作は、データベースで強制パラメーター化のオプションを設定することにより変更できます。 

推奨事項が与える影響の予測に役立つよう、実際の CPU 使用率、および予測される CPU 使用率 (推奨事項が適用されたと仮定した場合) の比較が提供されます。 CPU 使用率の削減に加えて、クエリ実行時間では、コンパイルにかかる時間が短縮されます。 プラン キャッシュのオーバーヘッドも減少し、ほとんどのプランがキャッシュに保持されて再利用されるようになります。 **[適用]** をクリックして、迅速かつ簡単にこの推奨事項を適用できます。 

推奨事項を適用すると、データベースの強制パラメーター化が速やかに有効になり、約 24 時間続く監視プロセスが開始されます。 監視期間後に、推奨事項が適用される前後 24 時間の、データベースの CPU 使用率を表示した検証レポートを確認できます。 SQL Database Advisor は、パフォーマンスの不具合が検出された場合に、推奨事項の適用を自動的に元に戻す安全メカニズムを備えています。

## <a name="fix-schema-issues-recommendations-preview"></a>スキーマの問題の修正に関する推奨事項 (プレビュー)

> [!IMPORTANT]
> Microsoft は、現在、"スキーマの問題の修正" に関する推奨事項を廃止しているところです。 データベース パフォーマンスの問題の自動監視には [Intelligent Insights](sql-database-intelligent-insights.md) の使用を開始することをお勧めします。このスキーマの問題には、以前の "スキーマの問題の修正" に関する推奨事項も含まれています。
> 

SQL Database サービスが、Azure SQL Database で発生したスキーマ関連の SQL エラー数が異常であることを検出すると、**スキーマの問題の修正**に関する推奨事項が表示されます。 この推奨事項は、通常、データベースでスキーマ関連のエラー (無効な列名、無効なオブジェクト名など) が 1 時間に複数件発生した場合に表示されます。

「スキーマの問題」は、SQL クエリの定義とデータベース スキーマの定義に不整合があった場合に発生する、SQL Server の構文エラーの種類の 1 つです。 たとえば、クエリで想定される列の 1 つがターゲット テーブルで見つからない、あるいはその逆の場合です。 

Azure SQL Database サービスが、Azure SQL Database で発生したスキーマ関連の SQL エラー数が異常であることを検出すると、「スキーマの問題の修正」に関する推奨事項が表示されます。 下の表は、スキーマの問題に関連したエラーを示しています。

| SQL エラー コード | メッセージ |
| --- | --- |
| 201 |プロシージャまたは関数 '*' にはパラメーター '*' が必要ですが、指定されませんでした。 |
| 207 |列名 '*' が無効です。 |
| 208 |オブジェクト名 '*' が無効です。 |
| 213 |列名または指定された値の数がテーブルの定義と一致しません。 |
| 2812 |ストアド プロシージャ '*' が見つかりませんでした。 |
| 8144 |プロシージャまたは関数 * に指定された引数が多すぎます。 |

## <a name="next-steps"></a>次の手順
推奨事項を監視し、引き続きパフォーマンスの調整対象とします。 データベースのワークロードは動的であり、継続的に変化します。 SQL Database Advisor では、お使いのデータベースのパフォーマンスを向上させる可能性がある推奨事項の監視と提供を継続します。 

* データベース インデックスとクエリ実行プランの自動チューニングについては、[Azure SQL Database の自動チューニングに関するページ](sql-database-automatic-tuning.md)を参照してください。
* パフォーマンスの問題の自動診断および根本原因分析を使用したデータベース パフォーマンスの自動監視については、[Azure SQL Intelligent Insights に関するページ](sql-database-intelligent-insights.md)を参照してください。
* Azure ポータルでパフォーマンス推奨事項を使用する方法については、[Azure ポータルのパフォーマンス推奨事項](sql-database-advisor-portal.md)に関するページを参照してください。
* よく使用されるクエリによるパフォーマンスへの影響を確認する方法については、[クエリ パフォーマンスの洞察](sql-database-query-performance.md)に関する記事をご覧ください。


