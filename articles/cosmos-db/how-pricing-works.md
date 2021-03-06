---
title: Azure Cosmos DB の価格モデル
description: この記事では、Azure Cosmos DB の価格モデルと、それによってコスト管理とコスト計画がどのように合理化されるかについて説明します。
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: d36b4fd433af716ebd97d88d05922d94bd74c309
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523538"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Azure Cosmos DB の価格モデル

Azure Cosmos DB の価格モデルにより、コストの管理と計画が合理化されます。 Azure Cosmos DB では、プロビジョニング済みのスループットと、消費されたストレージに対して支払います。

* **プロビジョニング済みのスループット**:[プロビジョニング済みのスループット](how-to-choose-offer.md) (予約済みスループットとも呼ばれます) により、あらゆる規模でハイ パフォーマンスが保証されます。 ユーザーが必要なスループット (RU/秒) を指定すると、Azure Cosmos DB では構成されたスループットを保証するために必要なリソースが専用に確保されます。 特定の 1 時間での最大のプロビジョニング スループットに対し、時間単位で請求されます。 手動でスループットをプロビジョニングすることも、[自動スケーリング](provision-throughput-autoscale.md)を使用することもできます。

   > [!NOTE]
   > プロビジョニング済みスループット モデルでは、コンテナーまたはデータベース専用にリソースが割り当てられるので、ワークロードを何も実行しない場合でも、プロビジョニング済みスループットの料金が発生します。

* **消費されたストレージ**:特定の 1 時間にデータおよびインデックスで消費されたストレージの合計量 (GB) に対して固定料金が請求されます。

1 秒あたりの[要求ユニット数](request-units.md) (RU/秒) で指定されるプロビジョニング済みスループットにより、コンテナーまたはデータベース内のデータの読み取りおよび書き込みが可能です。 [データベースまたはコンテナーに対してスループットをプロビジョニングする](set-throughput.md)ことができます。 ワークロードのニーズに基づいて、いつでもスループットをスケールアップ/ダウンできます。 Azure Cosmos DB の価格は弾力的であり、データベースまたはコンテナーに対して構成するスループットに比例します。 スループットとストレージの最小値とスケールの増分により、小規模コンテナーから大規模コンテナーまですべてのセグメントの顧客に価格と弾力性の完全な範囲が提供されます。 各データベースまたはコンテナーについて、100 RU/秒単位でプロビジョニングされたスループット (最低 400 RU/秒) と、ストレージ消費量 (GB) に対し、時間単位で課金されます。 プロビジョニング済みスループットとは異なり、ストレージは使用量ベースで課金されます。 つまり、前もってストレージを予約する必要はありません。 使用したストレージに対してのみ課金されます。

詳しくは、「[Azure Cosmos DB の価格](https://azure.microsoft.com/pricing/details/cosmos-db/)」ページと「[Azure Cosmos DB の課金内容の理解](understand-your-bill.md)」をご覧ください。

Azure Cosmos DB の価格モデルは、すべての API について同じです。 詳しくは、[Azure Cosmos DB の価格モデルがお客様にとっていかにコスト効率がよいか](total-cost-ownership.md)に関する記事をご覧ください。 SLA を保証するためにデータベースまたはコンテナーで最低限必要なスループットがあり、プロビジョニング済みスループットは 100 RU/秒あたりで増減できます。

Azure Cosmos DB アカウントを米国の非政府リージョンにデプロイする場合、データベース ベースおよびコンテナー ベースの両方のスループットに対する現在の最低価格は約 24 ドル/月です。 価格は、お客様が使用しているリージョンによって異なります。最新の価格情報については、「[Azure Cosmos DB の価格](https://azure.microsoft.com/pricing/details/cosmos-db/)」ページを参照してください。 ワークロードで複数のコンテナーを使用する場合は、データベース レベルのスループットを使用することで、コストについて最適化できます。これは、データベース レベルのスループットを使用すると、1 つのデータベースに任意の数のコンテナーを作成し、コンテナー間でスループットを共有できるためです。 次の表は、プロビジョニング済みスループットと異なるエンティティ別のコストをまとめたものです。

|**エンティティ**  | **最小スループット** |**スケール増分** |**プロビジョニングの予約範囲** |
|---------|---------|---------|-------|
|データベース    | 400 RU/秒    | 100 RU/秒   |スループットはデータベースに対して予約されており、データベース内のコンテナーによって共有されます。 |
|コンテナー     | 400 RU/秒   | 100 RU/秒  |スループットは特定のコンテナーに対して予約されます |

前の表で示したように、Azure Cosmos DB での最小スループットの価格は、約 24 ドル/月から始まります。 最小のスループットから開始し、運用ワークロードをサポートするために時間と共にスケールアップする場合、コストは約 6 ドル/月単位で滑らかに上昇します。 価格は、お客様が使用しているリージョンによって異なります。最新の価格情報については、「[Azure Cosmos DB の価格](https://azure.microsoft.com/pricing/details/cosmos-db/)」ページを参照してください。 Azure Cosmos DB の価格モデルは弾力的であり、スケールアップまたはスケールダウンに合わせて価格は滑らかに増減します。

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB を無料で試す

Azure Cosmos DB では、開発者向けに無料のオプションが多数提供されています。 設定できるオプションは次のとおりです。

* **Azure Cosmos DB の Free レベル**: Azure Cosmos DB の Free レベルを使用すると、アプリケーションの利用開始、開発、およびテストを簡単に行えるようになります。また、小規模な実稼働ワークロードを無料で実行することもできます。 アカウント上で Free レベルを有効にすると、そのアカウントでの最初の 400 RU/秒と 5 GB のストレージが無料で利用できるようになります (アカウントの有効期間中)。 Azure サブスクリプションごとに所有できる Free レベル アカウントは 1 つまでです。また、アカウントの作成時に選択する必要があります。 開始するには、[Free レベルで有効化されている Azure portal で新しいアカウントを作成する](create-cosmosdb-resources-portal.md)か、[ARM テンプレート](manage-sql-with-resource-manager.md#free-tier)を使用します。

* **Azure 無料アカウント**:Azure の [Free レベル](https://azure.microsoft.com/free/)では、最初の 30 日間に使用できる 200 ドルの Azure クレジットと 12 か月の無料サービス (数に制限あり) が提供されます。 詳細については、[Azure 無料アカウント](../cost-management-billing/manage/avoid-charges-free-account.md)に関するページをご覧ください。 Azure Cosmos DB は Azure 無料アカウントの一部です。 具体的には、Azure Cosmos DB の場合、この無料アカウントでは、5 GB のストレージと 400 RU/秒のプロビジョニング済みスループットが 1 年間提供されます。

* **Azure Cosmos DB を無料で試す**:Azure Cosmos DB では、Azure Cosmos DB を無料で試すアカウントを使用することで、時間制限をのあるエクスペリエンスが提供されます。 Azure Cosmos DB アカウントを作成して、データベースとコレクションを作成し、クイック スタートとチュートリアルを使用してサンプル アプリケーションを実行できます。 Azure アカウントをサブスクライブしたり、クレジット カードを使用したりせずに、サンプル アプリケーションを実行できます。 「[Cosmos DB を無料で試す](https://azure.microsoft.com/try/cosmosdb/)」では、Azure Cosmos DB が 1 か月間提供され、何回でもアカウントを更新できます。

* **Azure Cosmos DB エミュレーター**:Azure Cosmos DB エミュレーターでは、Azure Cosmos DB サービスを開発目的でエミュレートするローカル環境を利用できます。 エミュレーターは無料で提供され、クラウド サービスとほとんど同じです。 Azure Cosmos DB エミュレーターを使用すると、ローカルでのアプリケーションの開発とテストが、Azure サブスクリプションを作成したりコストをかけたりせずに実施できます。 運用環境に移行する前に、ローカル環境でエミュレーターを使用してアプリケーションを開発できます。 エミュレーターでのアプリケーションの機能に満足した後で、クラウドの Azure Cosmos DB アカウントを使用するように切り替えると、コストを大幅に削減できます。 エミュレーターについて詳しくは、[開発とテストへの Azure Cosmos DB の使用](local-emulator.md)に関する記事をご覧ください。

## <a name="pricing-with-reserved-capacity"></a>予約容量での価格

Azure Cosmos DB の[予約容量](cosmos-db-reserved-capacity.md)は、リソースの料金 1 年分または 3 年分を前払いすることで経費を節減できるサービスです。 1 年分または 3 年分を事前にコミットすることでコストを大幅に減らすことができ、通常価格に比べ、20% から 65% の割引になります。 Azure Cosmos DB の予約容量を利用すると、プロビジョニング済みスループット (RU/秒) を 1 年間または 3 年間前払いすることでコストを削減でき、プロビジョニング済みスループットの割り引きが受けられます。 

予約容量では、割引が適用されても、Azure Cosmos DB リソースのランタイム状態は維持されます。 予約容量は、すべての API (MongoDB、Cassandra、SQL、Gremlin、Azure Tables を含む) および世界中のすべてのリージョンで、同じように利用できます。 予約容量について詳しくは、[予約容量での Azure Cosmos DB リソースの前払い](cosmos-db-reserved-capacity.md)に関する記事をご覧ください。また、予約容量は [Azure portal](https://portal.azure.com/) で購入できます。

## <a name="next-steps"></a>次のステップ

Azure Cosmos DB リソースのコストの最適化について詳しくは、以下の記事をご覧ください。

* [開発とテストのための最適化](optimize-dev-test.md)について確認する
* [Azure Cosmos DB の課金内容の確認](understand-your-bill.md)の詳細について学習します
* [スループット コストの最適化](optimize-cost-throughput.md)の詳細について学習します
* [ストレージ コストの最適化](optimize-cost-storage.md)の詳細について学習します
* [読み取りと書き込みのコストの最適化](optimize-cost-reads-writes.md)の詳細について学習します
* [クエリ コストの最適化](optimize-cost-queries.md)の詳細について学習します
* [複数リージョンの Cosmos アカウント コストの最適化](optimize-cost-regions.md)について確認する
* [Azure Cosmos DB の予約容量](cosmos-db-reserved-capacity.md)について確認する
* [Azure Cosmos DB Emulator](local-emulator.md) について確認する
