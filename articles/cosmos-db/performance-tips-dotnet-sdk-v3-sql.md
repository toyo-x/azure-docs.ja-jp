---
title: .NET SDK v3 用の Azure Cosmos DB のパフォーマンスに関するヒント
description: Azure Cosmos DB .NET v3 SDK のパフォーマンスを向上させるためのクライアント構成オプションについて説明します。
author: j82w
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/16/2020
ms.author: jawilley
ms.openlocfilehash: 30fdc3c2b75d8ae567acfc612514ab080b929c5f
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85850248"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Azure Cosmos DB と .NET のパフォーマンスに関するヒント

> [!div class="op_single_selector"]
> * [.NET SDK v3](performance-tips-dotnet-sdk-v3-sql.md)
> * [.NET SDK v2](performance-tips.md)
> * [Java SDK v4](performance-tips-java-sdk-v4-sql.md)
> * [Async Java SDK v2](performance-tips-async-java.md)
> * [Sync Java SDK v2](performance-tips-java.md)

Azure Cosmos DB は、高速で柔軟性に優れた分散データベースです。待機時間とスループットが保証されており、シームレスにスケーリングできます。 Azure Cosmos DB でデータベースをスケーリングするために、アーキテクチャを大きく変更したり、複雑なコードを記述したりする必要はありません。 スケールアップとスケールダウンは、API 呼び出しを 1 回行うだけの簡単なものです。 詳細については、[コンテナーのスループットをプロビジョニングする方法](how-to-provision-container-throughput.md)または[データベースのスループットをプロビジョニングする方法](how-to-provision-database-throughput.md)に関するページを参照してください。 ただし、Azure Cosmos DB にはネットワーク呼び出しによってアクセスするため、[SQL .NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) を使うと、最高のパフォーマンスを実現するためにクライアント側の最適化を行うことができます。

そのため、データベースのパフォーマンスを向上させる場合は、次のオプションを検討してください。

## <a name="hosting-recommendations"></a>ホスティングの推奨事項

**クエリ集中型ワークロードの場合は、Linux または Windows 32 ビットのホスト処理ではなく、Windows 64 ビットのホスト処理を使用する**

パフォーマンス向上のため、Windows 64 ビットのホスト処理をお勧めします。 SQL SDK には、ローカル環境でクエリを解析して最適化するためのネイティブ ServiceInterop.dll が含まれています。 ServiceInterop.dll は、Windows x64 プラットフォームでのみサポートされています。 Linux および ServiceInterop.dll を使用できない他のサポート対象外プラットフォームでは、最適化されたクエリを取得するために、ゲートウェイに対して追加のネットワーク呼び出しが行われます。 次の種類のアプリケーションでは、既定で 32 ビットのホスト処理が使用されます。 ホスト処理を 64 ビット処理に変更するには、アプリケーションの種類に基づいて次の手順のようにします。

- 実行可能なアプリケーションの場合は、 **[プロジェクトのプロパティ]** ウィンドウの **[ビルド]** タブで [[プラットフォーム ターゲット]](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) を **[x64]** に設定することで、ホスト処理を変更できます。

- VSTest ベースのテスト プロジェクトの場合は、Visual Studio の **[テスト]** メニューで **[テスト]**  >  **[テストの設定]**  >  **[既定のプロセッサ アーキテクチャ] > [X64]** の順に選択することで、ホスト処理を変更できます。

- ローカルでデプロイされた ASP.NET Web アプリケーションの場合は、 **[ツール]**  >  **[オプション]**  >  **[プロジェクトおよびソリューション]**  >  **[Web プロジェクト]** の順に選択して、 **[Web サイトおよびプロジェクト用 IIS Express の 64 ビット バージョンを使用する]** をオンにすることで、ホスト処理を変更できます。

- Azure にデプロイされた ASP.NET Web アプリケーションの場合は、Azure portal の **[アプリケーションの設定]** で **64 ビット** プラットフォームを選択することで、ホスト処理を変更できます。

> [!NOTE] 
> 既定では、新しい Visual Studio プロジェクトは、 **[任意の CPU]** に設定されます。 **x86** に切り替わらないように、プロジェクトを **x64** に設定することをお勧めします。 **[任意の CPU]** に設定されたプロジェクトは、x86 のみの依存関係が追加されると、簡単に **x86** に切り替わる可能性があります。<br/>
> ServiceInterop.dll は、SDK DLL が実行されるフォルダーに配置する必要があります。 これは、手動で DLL をコピーする場合、またはカスタム ビルドおよびデプロイ システムを使用する場合にのみ、問題になります。
    
**サーバー側のガベージ コレクション (GC) を有効にする**

ガベージ コレクションの頻度を減らした方がよい場合もあります。 .NET では、[gcServer](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection) を `true` に設定します。

**クライアント ワークロードをスケールアウトする**

高スループット レベル (1 秒あたりの要求ユニット数が 50,000 超) でテストを行っている場合、コンピューターが CPU 使用率またはネットワーク使用率の上限に達したことでクライアント アプリケーションがボトルネックになることがあります。 この状態に達しても、クライアント アプリケーションを複数のサーバーにスケールアウトすることで引き続き同じ Azure Cosmos DB アカウントで対応できます。

> [!NOTE] 
> CPU 使用率が高いと、待ち時間が長くなり、要求タイムアウトの例外が発生する可能性があります。

## <a name="networking"></a>ネットワーク
<a id="direct-connection"></a>

**接続ポリシー:直接接続モードを使用する**

クライアントが Azure Cosmos DB に接続する方法は、特に監視対象となるクライアント側の待機時間について、パフォーマンスに重要な影響を及ぼします。 クライアントの接続ポリシーを構成する際に使用できる 2 つの主要な構成設定として、接続 "*モード*" と接続 "*プロトコル*" があります。  次の 2 つのモードが用意されています。

   * 直接モード (既定)

     直接モードは、[Microsoft.Azure.Cosmos/.NET V3 SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) を使用している場合は既定の接続モードであり、TCP プロトコルを介した接続がサポートされます。 これによってゲートウェイ モードよりパフォーマンスが向上し、必要なネットワーク ホップ数が少なくなります。

   * ゲートウェイ モード
      
     ゲートウェイ モードでは標準の HTTPS ポートと単一のエンドポイントが使用されるため、ファイアウォールの厳しい制限がある企業ネットワーク内でアプリケーションを実行する場合は、ゲートウェイ モードが最適な選択肢です。 ただし、パフォーマンスのトレードオフとして、ゲートウェイ モードでは、Azure Cosmos DB に対してデータの読み取りまたは書き込みを行うたびに、追加のネットワーク ホップが必要になります。 つまり、ネットワーク ホップ数が少ないため、直接モードの方がパフォーマンスが向上します。 ソケット接続の数に制限がある環境でアプリケーションを実行する場合、ゲートウェイ接続モードも推奨されます。

     Azure Functions (特に[従量課金プラン](../azure-functions/functions-scale.md#consumption-plan)) で SDK を使用する場合は、現在の[接続数の制限](../azure-functions/manage-connections.md)に注意してください。 Azure Functions アプリケーション内で他の HTTP ベースのクライアントも使用している場合は、ゲートウェイ モードの方がよい可能性があります。


ゲートウェイ モードの Azure Cosmos DB では、ポート 443 が使用され、MongoDB 用の Azure Cosmos DB の API を使用している場合はポート 10250、10255、および 10256 が使用されます。 ポート 10250 は、geo レプリケーションを使用しない既定の MongoDB インスタンスにマップされます。 ポート 10255 と 10256 は、geo レプリケーションを使用する MongoDB インスタンスにマップされます。
     
直接モードで TCP を使用する場合は、Azure Cosmos DB で動的 TCP ポートが使用されるため、ゲートウェイ ポートに加えてポート範囲 10000 から 20000 を開いておく必要があります ([プライベート エンドポイント](./how-to-configure-private-endpoints.md)で直接モードを使用する場合、0 から 65535 のすべての範囲の TCP ポートが開いている必要があります)。 標準 Azure VM 構成では、ポートは既定で開かれています。 これらのポートが開いていない場合に TCP を使用しようとすると、[503 サービスを利用できません] エラーが表示されます。 次の表は、さまざまな API で使用可能な接続モードと、各 API に使用されるサービス ポートを示しています。

|接続モード  |サポートされるプロトコル  |サポートされる SDK  |API/サービス ポート  |
|---------|---------|---------|---------|
|Gateway  |   HTTPS    |  すべての SDK    |   SQL (443)、MongoDB (10250、10255、10256)、Table (443)、Cassandra (10350)、Graph (443)    |
|直接    |     TCP    |  .NET SDK    | パブリックまたはサービス エンドポイントを使用する場合: 10000 から 20000 の範囲のポート<br>プライベート エンドポイントを使用する場合: 0 から 65535 の範囲のポート |

Azure Cosmos DB では、HTTPS を介したシンプルなオープン RESTful プログラミング モデルが提供されます。 さらに、RESTful な通信モデルである効率的な TCP プロトコルも用意されており、.NET クライアント SDK を通じて使用できます。 TCP プロトコルでは、最初の認証とトラフィックの暗号化で TLS が使用されます。 最適なパフォーマンスを実現するために、可能であれば TCP プロトコルを使用します。

SDK V3 の場合、`CosmosClientOptions` で `CosmosClient` インスタンスを作成するときに、接続モードを構成します。 直接モードが既定値であることに注意してください。

```csharp
string connectionString = "<your-account-connection-string>";
CosmosClient client = new CosmosClient(connectionString,
new CosmosClientOptions
{
    ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
});
```

TCP は直接モードでのみサポートされるため、ゲートウェイ モードを使用する場合は、ゲートウェイとの通信に常に HTTPS プロトコルが使用さます。

:::image type="content" source="./media/performance-tips/connection-policy.png" alt-text="Azure Cosmos DB の接続ポリシー" border="false":::

**一時的なポートの不足**

インスタンスで接続量が多いか、ポートの使用率が高い場合は、まず、クライアント インスタンスがシングルトンであることを確認します。 言い換えると、クライアント インスタンスは、アプリケーションの有効期間にわたって一意である必要があります。

クライアントは、TCP プロトコルで実行されている場合、非アクティブな状態が 2 分続くと接続を終了する HTTPS プロトコルとは異なり、有効期間の長い接続を使用して待ち時間を最適化します。

アクセス頻度が低く、ゲートウェイ モード アクセスと比べて接続数が多い場合は、次のことが可能です。

* [CosmosClientOptions.PortReuseMode](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.portreusemode) プロパティを `PrivatePortPool` に構成します (フレームワーク バージョン >= 4.6.1 および .net core バージョン >= 2.0 で有効)。このプロパティにより、SDK はさまざまな Azure Cosmos DB の宛先エンドポイントのために一時的なポートの小さなプールを使用できます。
* [CosmosClientOptions.IdleConnectionTimeout](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.idletcpconnectiontimeout) プロパティを構成します。これは 10 分以上である必要があります。 推奨値は 20 分から 24 時間です。

<a id="same-region"></a>

**パフォーマンスを確保するために同じ Azure リージョン内にクライアントを併置する**

可能であれば、Azure Cosmos DB を呼び出すアプリケーションを Azure Cosmos DB データベースと同じリージョンに配置します。 大ざっぱな比較ですが、Azure Cosmos DB の呼び出しは、同じリージョン内であれば 1 から 2 ミリ秒以内で完了するのに対し、米国西部と米国東部との間では待ち時間が 50 ミリ秒を超えます。 要求がクライアントから Azure データセンターの境界まで流れるときに使用されるルートに応じて、この待機時間が要求ごとに異なる可能性があります。 最短の待機時間は、プロビジョニングされた Azure Cosmos DB エンドポイントと同じ Azure リージョン内に呼び出し元アプリケーションを配置することによって実現できます。 利用可能なリージョンの一覧については、「[Azure リージョン](https://azure.microsoft.com/regions/#services)」をご覧ください。

:::image type="content" source="./media/performance-tips/same-region.png" alt-text="Azure Cosmos DB の接続ポリシー" border="false":::

   <a id="increase-threads"></a>

**スレッドまたはタスクの数を増やす**

Azure Cosmos DB の呼び出しはネットワーク経由で行われるため、クライアント アプリケーションで要求間の待ち時間を最短にするために、要求のコンカレンシーの次数を変えることが必要な場合があります。 たとえば、.NET の [タスク並列ライブラリ](https://msdn.microsoft.com//library/dd460717.aspx)を使用する場合、Azure Cosmos DB に対する読み取りタスクまたは書き込みタスクを 100 件単位で作成してください。

**高速ネットワークの有効化**
 
 待機時間と CPU ジッターを減らすために、クライアントの仮想マシンでは高速ネットワークを有効にすることをお勧めします。 「[高速ネットワークを使った Windows 仮想マシンの作成](../virtual-network/create-vm-accelerated-networking-powershell.md)」または「[高速ネットワークを使った Linux 仮想マシンの作成](../virtual-network/create-vm-accelerated-networking-cli.md)」を参照してください。

## <a name="sdk-usage"></a>SDK の使用
**最新の SDK をインストールする**

Azure Cosmos DB SDK は、最適なパフォーマンスを提供するために頻繁に改善されています。 [Azure Cosmos DB SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) のページを参照して、最新の SDK を確認し、改善点を確認してください。

**ストリーム API を使用する**

[.NET SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) には、シリアル化せずにデータを受信して返すことができるストリーム API シリーズが含まれています。 

SDK からの応答を直接使用せずに他のアプリケーション層にリレーする中間層アプリケーションでは、ストリーム API を利用するメリットがあります。 ストリーム処理の例については、[項目管理](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement)のサンプルを参照してください。

**アプリケーションの有効期間中はシングルトン Azure Cosmos DB クライアントを使用する**

各 `CosmosClient` インスタンスはスレッドセーフであり、直接モードで動作しているときには効率的な接続管理とアドレスのキャッシュが実行されます。 効率的な接続管理と SDK クライアントのパフォーマンス向上を実現するために、アプリケーションの有効期間中は、`AppDomain` ごとに単一のインスタンスを使用することをお勧めします。

Azure Functions で作業する場合、インスタンスは既存の[ガイドライン](../azure-functions/manage-connections.md#static-clients)にも従って、1 つのインスタンスを維持する必要があります。

<a id="max-connection"></a>

**書き込み操作に対するコンテンツ応答を無効にする**

高負荷の作成ペイロードがあるワークロードでは、EnableContentResponseOnWrite 要求オプションを false に設定します。 サービスは、作成または更新されたリソースを SDK に返さなくなります。 通常、アプリケーションには作成済みのオブジェクトがあるため、サービスにオブジェクトを返してもらう必要はありません。 ヘッダー値には、まだ要求の料金と同様にアクセスできます。 これにより、SDK がメモリを割り当てたり応答の本文をシリアル化したりする必要がなくなるため、パフォーマンスを向上させることができます。 また、ネットワーク帯域幅の使用量も削減され、パフォーマンスがさらに向上します。  

```csharp
ItemRequestOption requestOptions = new ItemRequestOptions() { EnableContentResponseOnWrite = false };
ItemResponse<Book> itemResponse = await this.container.CreateItemAsync<Book>(book, new PartitionKey(book.pk), requestOptions);
// Resource will be null
itemResponse.Resource
```

**待ち時間ではなくスループットを最適化するために一括処理を有効にする** ワークロードが大量のスループットを必要とし、待ち時間はそれほど重要ではないシナリオでは、一括処理を有効にします。 機能を有効にする方法と、その機能を使用する必要があるシナリオの詳細については、[一括処理の概要](https://devblogs.microsoft.com/cosmosdb/introducing-bulk-support-in-the-net-sdk)に関するページを参照してください。

**ゲートウェイ モードを使用するときはホストあたりの System.Net MaxConnections を増やす**

ゲートウェイ モードを使用すると、Azure Cosmos DB の要求は HTTPS/REST を介して行われます。 それらは、ホスト名または IP アドレスごとの既定の接続数の上限に従います。 場合によっては、Azure Cosmos DB に対する複数の同時接続をクライアント ライブラリで使用できるよう、`MaxConnections` を高い値 (100 から 1,000) に増やす必要があります。 .NET SDK 1.8.0 以降では、[ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) の既定値は 50 です。 その値を変更するには、[Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) をより高い値に設定できます。

**パーティション分割コレクションに対する並列クエリを調整する**

SQL .NET SDK では、並列クエリがサポートされています。この機能を使用すると、パーティション分割コンテナーにクエリを並列的に実行できます。 詳細については、SDK の操作に関連した[コード サンプル](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs)を参照してください。 並列クエリは、シリアル クエリよりもクエリの待機時間とスループットを向上させるように設計されています。 並列クエリには、要件に合わせてチューニングできる 2 つのパラメーターがあります。 
- `MaxConcurrency` では、同時にクエリを実行できるパーティションの最大数を制御します。 
- `MaxBufferedItemCount` では、プリフェッチされる結果の数を制御します。

***コンカレンシーの次数を調整する***

並列クエリは、複数のパーティションに並列にクエリを実行することによって機能します。 ただし、個々のパーティションからのデータは、クエリごとに順番に取得されます。 そのため、[SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) の `MaxConcurrency` をパーティションの数に設定すると、その他のすべてのシステムの条件が同じであれば、クエリのパフォーマンスを最善にできる可能性が最大になります。 パーティションの数がわからない場合は、並列処理の次数を高い数値に設定できます。 システムにより、並列処理の次数として最小値 (パーティションの数、ユーザー指定の入力) が選択されます。

並列クエリが最も有効に機能するのは、クエリに対するデータがすべてのパーティションに均等に分散している場合です。 パーティション分割されたコレクションが、クエリによって返されるすべてまたは大部分のデータがわずかな数のパーティション (最悪の場合は 1 つのパーティション) に集中するように分割されている場合、それらのパーティションがクエリのパフォーマンスのボトルネックになります。

***MaxBufferedItemCount を調整する***
    
並列クエリは、結果の現在のバッチがクライアントによって処理されている間に結果をプリフェッチするように設計されています。 このプリフェッチは、クエリの全体的な遅延の削減に役立ちます。 `MaxBufferedItemCount` パラメーターは、プリフェッチされる結果の数を制限します。 `MaxBufferedItemCount` を、返される結果の予期される数 (またはそれ以上の数) に設定すると、クエリに対するプリフェッチの効果が最大になります。

プリフェッチは、並列処理の次数とは無関係に同じように動作し、すべてのパーティションからのデータに対して単一のバッファーが存在します。  

**RetryAfter 間隔でバックオフを実装する**

パフォーマンス テストでは、調整される要求の割合がわずかになるまで負荷を上げる必要があります。 要求がスロットル状態になった場合、クライアント アプリケーション側でバックオフ値を適用し、サーバー側によって指定された再試行間隔のスロットル時間を後退させるようにしてください。 バックオフにより、再試行までの待ち時間を最小限に抑えることができます。 

詳細については、[RetryAfter](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosexception.retryafter?view=azure-dotnet#Microsoft_Azure_Cosmos_CosmosException_RetryAfter) に関するページを参照してください。
    
次の例のように、追加の診断情報をログに記録し、待ち時間に関する問題をトラブルシューティングするメカニズムがあります。 読み取り待ち時間の長い要求についての診断文字列をログに記録できます。 取得した診断文字列は、特定の要求に対して 429 エラーを受け取った回数を把握するのに役立ちます。

```csharp
ItemResponse<Book> readItemResponse = await this.cosmosContainer.ReadItemAsync<Book>("ItemId", new PartitionKey("PartitionKeyValue"));
readItemResponse.Diagnostics.ToString(); 
```

**スレッドまたはタスクの数を増やす**

この記事の「ネットワーク」セクションの「[スレッドまたはタスクの数を増やす](#increase-threads)」を参照してください。

## <a name="indexing-policy"></a>インデックス作成ポリシー
 
**インデックス作成から未使用のパスを除外して書き込みを高速化する**

Azure Cosmos DB のインデックス作成ポリシーでは、インデックス作成パス (IndexingPolicy.IncludedPaths および IndexingPolicy.ExcludedPaths) を使用して、インデックス作成に含めたり除外したりするドキュメント パスも指定できます。 必要なパスだけでインデックスを作成すると、クエリのパターンが事前にわかっている場合に、書き込みパフォーマンスの向上、書き込み操作に対する RU 料金の削減、およびインデックス ストレージの削減が可能になります。 これは、インデックス作成コストは、インデックスが作成される一意のパスの数に直接関係するためです。 たとえば、次のコードは、ワイルドカード "*" を使用して、ドキュメントのセクション全体 (サブツリー) をインデックス作成から除外する方法を示しています。

```csharp
var containerProperties = new ContainerProperties(id: "excludedPathCollection", partitionKeyPath: "/pk" );
containerProperties.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
containerProperties.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
Container container = await this.cosmosDatabase.CreateContainerAsync(containerProperties);
```

詳細については、[Azure Cosmos DB インデックス作成ポリシー](index-policy.md)に関するページをご覧ください。

## <a name="throughput"></a>スループット
<a id="measure-rus"></a>

**測定と調整によって 1 秒あたりの要求ユニットの使用量を削減する**

Azure Cosmos DB には、データベース操作の豊富なセットが用意されています。 UDF、ストアド プロシージャ、トリガーを使用したリレーショナル クエリや階層クエリなどの操作があります。これらの操作はすべて、データベース コレクション内のドキュメントに対して実行できます。 これらの操作のそれぞれに関連付けられたコストは、操作を完了するために必要な CPU、IO、およびメモリに応じて異なります。 ハードウェア リソースの管理について考える代わりに、各種のデータベース操作を実行しアプリケーション要求を処理するのに必要なリソースに関する単一の測定単位として要求ユニット (RU) を考えることができます。

コンテナーごとに設定された[要求ユニット](request-units.md)の数に基づいて、スループットをプロビジョニングします。 要求ユニットの消費は、1 秒あたりのレートとして評価されます。 コンテナーのプロビジョニング済み要求ユニット レートを超過したアプリケーションは、レートがそのコンテナーにプロビジョニングされているレベルを下回るまで制限されます。 アプリケーションでより高いスループットが必要になった場合は、追加の要求ユニットをプロビジョニングしてスループットを増やすことができます。

クエリの複雑さは、操作で消費される要求ユニット数に影響します。 述語の数、述語の特性、UDF 数、ソース データ セットのサイズのすべてがクエリ操作のコストに影響します。

操作 (作成、更新、または削除) のオーバーヘッドを測定するには、[x-ms-request-charge](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) ヘッダー (あるいは、.NET SDK の `ResourceResponse\<T>` または `FeedResponse\<T>` の同等の `RequestCharge` プロパティ) を調べて、これらの操作で使用される要求ユニット数を測定します。

```csharp
// Measure the performance (Request Units) of writes
ItemResponse<Book> response = await container.CreateItemAsync<Book>(myBook, new PartitionKey(myBook.PkValue));
Console.WriteLine("Insert of item consumed {0} request units", response.RequestCharge);
// Measure the performance (Request Units) of queries
FeedIterator<Book> queryable = container.GetItemQueryIterator<ToDoActivity>(queryString);
while (queryable.HasMoreResults)
    {
        FeedResponse<Book> queryResponse = await queryable.ExecuteNextAsync<Book>();
        Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
    }
```             

このヘッダーで返される要求の使用量は、プロビジョニングしたスループット (2000 RU/秒) の一部です。 たとえば、上記のクエリが 1 KB のドキュメントを 1000 個返した場合、この操作のコストは 1000 になります。 そのため、後続の要求をレート制限する前に、サーバーは 1 秒以内にこのような要求を 2 つだけ受け付けます。 詳細については、[要求ユニット](request-units.md)に関する記事および[要求ユニット計算ツール](https://www.documentdb.com/capacityplanner)のページを参照してください。
<a id="429"></a>

**レート制限と大きすぎる要求レートに対処する**

クライアントがアカウントの予約済みスループットを超えようとしても、サーバーでパフォーマンスの低下が発生することはなく、予約済みのレベルを超えてスループット容量が使用されることもありません。 サーバーは、RequestRateTooLarge (HTTP 状態コード 429) を使用して要求をプリエンプティブに終了します。 それからは、要求を再試行する前にユーザーが待機する必要がある時間 (ミリ秒単位) を示す、[x-ms-retry-after-ms](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) ヘッダーが返されます。

```xml
    HTTP Status 429,
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100
```

SDK はすべてこの応答を暗黙的にキャッチし、サーバーが指定した retry-after ヘッダーを優先して要求を再試行します。 アカウントに複数のクライアントが同時アクセスしている状況でなければ、次回の再試行は成功します。

累積的に動作する複数のクライアントがあり、要求レートを常に超えている場合は、現在クライアントによって内部的に 9 に設定される既定の再試行回数では、十分ではない可能性があります。 このような場合、クライアントではアプリケーションに対して状態コード 429 の CosmosException がスローされます。 

`CosmosClientOptions` インスタンスで `RetryOptions` を設定することにより、既定の再試行回数を変更できます。 既定では、要求レートを超えて要求が続行されている場合に、30 秒の累積待ち時間を過ぎると、状態コードが 429 の CosmosException が返されます。 このエラーは、現在の値が既定値の 9 かユーザー定義の値かにかかわらず、現在の再試行回数が最大再試行回数より少ない場合でも返されます。

自動再試行動作は、ほとんどのアプリケーションで回復性と使いやすさを向上させるのに役立ちます。 ただし、パフォーマンス ベンチマークを実行しているときは (特に待機時間を測定するとき)、最適な動作ではない可能性があります。 実験でサーバー スロットルが発生し、クライアント SDK によって警告なしに再試行が行われると、クライアントが監視する待機時間が急増します。 パフォーマンスの実験中に待機時間が急増するのを回避するには、各操作で返される使用量を測定し、予約済みの要求レートを下回った状態で要求が行われていることを確認します。 詳細については、[要求ユニット](request-units.md)に関する記事を参照してください。

**スループットを向上させるためにサイズの小さいドキュメントに合わせて設計する**

特定の操作の要求の使用量 (要求処理コスト) は、ドキュメントのサイズに直接関係します。 サイズの大きいドキュメントの操作は、サイズの小さいドキュメントの操作よりもコストがかかります。

## <a name="next-steps"></a>次のステップ
少数のクライアント コンピューターでの高パフォーマンス シナリオで Azure Cosmos DB の評価に使用されるサンプル アプリケーションについては、「[Azure Cosmos DB のパフォーマンスとスケールのテスト](performance-testing.md)」を参照してください。

スケーリングと高パフォーマンスのためのアプリケーションの設計について詳しくは、「[Azure Cosmos DB でのパーティション分割とスケーリング](partition-data.md)」をご覧ください。
