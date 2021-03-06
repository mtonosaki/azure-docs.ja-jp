---
title: 予約容量を使用したコンピューティングの前払い - Azure Cache for Redis
description: 予約容量を使用した計算リソースの Azure Cache for RedisRedis を使用した前払い
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: aded023c9f4c045f612e33d32c1e3ac71afddf02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2020
ms.locfileid: "77530208"
---
# <a name="prepay-for-azure-cache-for-redis-compute-resources-with-reserved-capacity"></a>予約容量を使用した計算リソースの Azure Cache for RedisRedis を使用した前払い

Azure Cache for Redis は、従量課金制の料金と比較して、コンピューティング リソースを事前に支払ってコストを節約するのに役立ちます。 予約容量用の Azure Cache for Redis では、1 年分または 3 年分をキャッシュで前払いすることで、計算コストを大幅に節約できます。 予約容量用にAzure Cache for Redis を購入するには、Azure リージョン、サービス レベル、用語を指定する必要があります。

特定の Azure Cache for Redis インスタンスに予約を割り当てる必要はありません。 既に稼働している Azure Cache for Redis または新たにデプロイされた Azure Cache for Redis では、予約済みのキャッシュ サイズまでの予約価格が自動的に得られます。 予約を購入すると、計算コストを 1 年間または 3 年間分前払いすることになります。 予約を購入すると、予約属性に一致する Azure Cache for Redis の計算料金は従量課金制で請求されなくなります。 キャッシュに関連付けられているネットワーキング、またはストレージの料金は、予約の対象になりません。 予約期間が満了した時点で、課金特典の有効期限は切れ、従量課金料金が Azure Cache for Redis に適用されます。 予約は自動更新されません。 価格の詳細については、[Azure Cache for Redis の予約容量オファー](https://azure.microsoft.com/pricing/details/cache)に関するページを参照してください。

Azure Cache for Redis の予約容量は [Azure portal](https://portal.azure.com/) で購入できます。 予約容量を購入するには:

* 少なくとも 1 つのエンタープライズ サブスクリプションまたは従量課金制料金の個々のサブスクリプションで所有者ロールである必要があります。
* Enterprise サブスクリプションの場合、[EA ポータル](https://ea.azure.com/)で **[予約インスタンスを追加します]** を有効にする必要があります。 または、その設定が無効になっている場合は、ユーザーはサブスクリプションの EA 管理者である必要があります。
* クラウド ソリューション プロバイダー (CSP) プログラムの場合、管理者エージェントまたはセールス エージェントのみが Azure Cache for Redis の予約容量を購入できます。

エンタープライズおよび従量課金制のお客様に対する予約購入の課金方法の詳細については、「[エンタープライズ加入契約に適用される Azure の予約の使用状況について](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)」および「[従量課金制サブスクリプションに適用される Azure の予約の使用状況について](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)」をご覧ください。


## <a name="determine-the-right-cache-size-before-purchase"></a>購入する前に適切なキャッシュのサイズを決定する

予約のサイズは、既存のまたはすぐにデプロイされる予定のキャッシュ (特定のリージョン内で同じサービス レベルを使用するもの) で使用される計算量の合計に基づいて決める必要があります。

たとえば、1 つの汎用の Gen5 - 32 仮想コア キャッシュと、2 つのメモリ最適化済みの Gen5 - 16 仮想コア キャッシュを実行しているとします。 さらに、来月中に汎用の Gen5 - 32 仮想コア データベース サーバーを 1 つと、メモリ最適化済みの Gen5 - 16 仮想コア データベース サーバーを 1 つデプロイする予定だとします。 少なくとも 1 年間はこれらのリソースが必要になることがわかっているとします。 この場合、単一データベースの汎用 - Gen5 用に 64 (2x32) 個の仮想コア 1 年予約分と、単一データベース メモリ最適化済み - Gen5 用に 48 (2x16 + 16) 個の仮想コア 1 年予約分を購入する必要があります。


## <a name="buy-azure-cache-for-redis-reserved-capacity"></a>予約容量用に Azure Cache for Redis を購入する

1. [Azure portal](https://portal.azure.com/) にサインインします。
2. **[すべてのサービス]**  >  **[予約]** を選択します。
3. **[追加]** を選択し、[購入予約] ペインで **[Azure Cache for Redis]** を選択してキャッシュの新しい予約を購入します。
4. 必須フィールドに入力します。 選択した属性と一致する既存または新規のデータベースが、予約容量の割引を受けられます。 割引を受ける Azure Cache for Redis インスタンスの実際の数は、選択したスコープと数量によって変わります。


![予約価格の概要](media/cache-reserved-pricing/cache-reserved-price.png)


次の表で、必須フィールドについて説明します。

| フィールド | 説明 |
| :------------ | :------- |
| サブスクリプション   | Azure Cache for Redis の予約容量の予約の支払いに使用するサブスクリプション。 サブスクリプションの支払方法に対して、Azure Cache for Redis の予約容量の予約の前払いコストが課金されます。 サブスクリプションの種類は、マイクロソフト エンタープライズ契約 (プラン番号:MS-AZR-0017P または MS-AZR-0148P) または従量課金制料金の個々の契約 (プラン番号:MS-AZR-0003P または MS-AZR-0023P)。 エンタープライズ サブスクリプションの場合、登録の年額コミットメント残高から料金が差し引かれるか、超過料金として課金されます。 従量課金制料金の個々のサブスクリプションの場合、クレジット カードまたはサブスクリプションの請求書に記載されている支払方法に料金が課金されます。
| Scope | 1 つのサブスクリプションまたは複数のサブスクリプション (共有スコープ) を予約のスコープにすることができます。 以下を選択した場合: </br></br> **共有** – 予約割引は、課金のコンテキスト内にある任意のサブスクリプションで実行されている Azure Cache for Redis インスタンスに適用されます。 エンタープライズのお客様の場合、共有スコープが対象の登録であり、登録内のすべてのサブスクリプションが含まれます。 従量課金制のお客様の場合、共有スコープは、アカウント管理者が作成するすべての従量課金制サブスクリプションです。</br></br> **単一サブスクリプション** - 予約割引は選択したサブスクリプションの Azure Cache for Redis インスタンスに適用されます。 </br></br> **[1 つのリソース グループ]** - 予約割引は、選択したサブスクリプションおよびそのサブスクリプション内の選択したリソース グループ内の Azure Cache for Redis インスタンスに適用されます。
| リージョン | Azure Cache for Redis 予約容量の予約の対象となる Azure リージョン。
| Pricing tier | Azure Cache for Redis サーバーのサービス レベル。
| 期間 | 1 年間または 3 年間
| Quantity | Azure Cache for Redis の予約容量の予約内で購入される計算リソース数。 この数量は、予約し、請求時に割り引きを受ける、選択された Azure リージョンとパフォーマンス レベルに含まれるキャッシュの数です。 たとえば、米国東部リージョンで、合計キャッシュ容量を 26 GB とする Azure Cache for Redis サーバーを実行している場合、あるいは実行する予定の場合、すべてのキャッシュを最大限に活用するため、数量に 26 を指定します。

## <a name="cancel-exchange-or-refund-reservations"></a>予約の取り消し、交換、または返金

一定の制限付きで、予約の取り消し、交換、または返金を行うことができます。 詳しくは、「[Azure の予約のセルフサービスによる交換と払戻](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund)」を参照してください。

## <a name="cache-size-flexibility"></a>キャッシュのサイズの柔軟性

同じパフォーマンス レベルとリージョン内であれば、予約容量のベネフィットを失うことなく、キャッシュのサイズを柔軟にスケールアップまたはスケールダウンできます。

## <a name="need-help-contact-us"></a>お困りの際は、 お問い合わせ

ご質問がある場合やヘルプが必要な場合は、[サポート リクエストを作成](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)してください。

## <a name="next-steps"></a>次のステップ

予約割引は、予約スコープと属性に一致する Azure Cache for Redis インスタンスに対して自動的に適用されます。 予約のスコープは、Azure portal、PowerShell、Azure CLI、または API で更新できます。

*  Azure Cache for Redis に予約容量割引が適用される方法については、「[Azure の予約割引の概要](../cost-management-billing/reservations/understand-azure-cache-for-redis-reservation-charges.md)」をご覧ください。

* Azure の予約の詳細については、次の記事を参照してください。

    * [Azure の予約とは](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
    * [Azure の予約の管理](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
    * [Azure の予約割引を理解する](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
    * [従量課金制サブスクリプションの予約使用量について](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-mysql)
    * [エンタープライズ加入契約の予約使用量について](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
    * [パートナー センターのクラウド ソリューション プロバイダー (CSP) プログラムでの Azure の予約](https://docs.microsoft.com/partner-center/azure-reservations)

