---
title: クイック スタート:Azure Maps を使った対話型のマップ検索
description: Microsoft Azure Maps Web SDK を使用して対話型のマップ検索用のデモ Web アプリケーションを作成する方法について学習します。
author: philmea
ms.author: philmea
ms.date: 5/21/2020
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: da225f9a0bac5d179efadb7d507750c8aa0bc13e
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2020
ms.locfileid: "83872115"
---
# <a name="quickstart-create-an-interactive-search-map-by-using-azure-maps"></a>クイック スタート:Azure Maps を使用して対話形式の検索マップを作成する

この記事では、対話形式で検索を行える、Azure Maps のマップ作成機能について説明します。 次の基本的な手順について説明します。

* 自分用の Azure Maps アカウントを作成する。
* デモ版の Web アプリケーションで使用する主キーを取得する。

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

## <a name="sign-in-to-the-azure-portal"></a>Azure portal にサインインする

[Azure portal](https://portal.azure.com) にサインインします。

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Azure Maps でアカウントを作成する

次の手順で新しい Maps アカウントを作成します。

1. [Azure Portal](https://portal.azure.com) の左上隅にある **[リソースの作成]** をクリックします。
2. *[Marketplace を検索]* ボックスに「**Maps**」と入力します。
3. *[結果]* から **[Maps]** を選択します。 マップの下に表示される **[作成]** ボタンをクリックします。
4. **[Azure Maps アカウントの作成]** ページで、次の値を入力します。
    * このアカウントで使う "*サブスクリプション*"。
    * このアカウントの "*リソース グループ*" の名前。 *[新規作成]* を選んで新しく作成することも、 *[既存のものを使用]* を選んで既存のリソース グループを使うこともできます。
    * 新しいアカウントの "*名前*"。
    * このアカウントの "*価格レベル*"。
    * *[ライセンス]* と *[プライバシーに関する声明]* の内容を読み、チェック ボックスをオンにして条件に同意します。
    * **[作成]** ボタンをクリックします。

![ポータルでの Azure Maps アカウントの作成](./media/quick-demo-map-app/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>アカウントの主キーを取得する

Maps アカウントが正常に作成されたら、Maps API のクエリを実行できる主キーを取得します。

1. ポータルで、Maps アカウントを開きます。
2. [設定] セクションで **[認証]** を選択します。
3. **[主キー]** をクリップボードにコピーします。 このチュートリアルで後ほど使用するためにローカルに保存します。

>[!NOTE]
> 主キーの代わりにサブスクリプション キーを使用する場合、マップは正しく表示されません。 また、セキュリティ上の理由から、主キーとセカンダリ キーをローテーションすることをお勧めします。 キーをローテーションするには、セカンダリ キーを使用するようにアプリを更新してデプロイします。次に、主キーの横にある [cycle/refresh]\(サイクル/更新\) ボタンを押して、新しい主キーを生成します。 古い主キーは無効になります。 キー ローテーションの詳細については、「[キー ローテーションと監査で Azure Key Vault を設定する](https://docs.microsoft.com/azure/key-vault/secrets/key-rotation-log-monitoring)」を参照してください。

![Azure portal で主キー Azure Maps キーを取得する](./media/quick-demo-map-app/get-key.png)

## <a name="download-the-application"></a>アプリケーションのダウンロード

1. [interactiveSearch .html](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html) にアクセスします。 ファイルの内容をコピーします。
2. このファイルの内容を **AzureMapDemo.html** としてローカルに保存します。 それをテキスト エディターで開きます。
3. `<Your Azure Maps Key>` という文字列を検索します。 これを前のセクションで取得した**主キー**の値に置き換えます。

## <a name="open-the-application"></a>アプリケーションを開く

1. **AzureMapDemo.html** ファイルを任意のブラウザーで開きます。
2. 表示されたロサンゼルス市の地図を見てください。 拡大/縮小すると、ズームのレベルに応じて、マップに表示される情報が自動的に増減します。
3. マップの既定の中心を変更します。 **AzureMapDemo.html** ファイルで **center** という名前の変数を検索します。 この変数の緯度と経度のペア値を、新しい値 **[-74.0060, 40.7128]** に置き換えます。 ファイルを保存し、ブラウザーを更新します。
4. これで、対話形式の検索を実行できます。 デモ版の Web アプリケーションの左上隅にある検索ボックスに「**レストラン**」と入力します。
5. 検索ボックスの下に表示された住所/場所のリストにマウスを合わせます。 対応するピンがマップに現れ、その場所に関する情報がポップアップに表示されることがわかります。 民間事業のプライバシー保護のために、架空の名前と住所が表示されています。

    ![対話型のマップ検索 Web アプリケーション](./media/quick-demo-map-app/interactive-search.png)

## <a name="clean-up-resources"></a>リソースをクリーンアップする

チュートリアルでは、お使いのアカウントで Azure Maps を使用および構成する方法について詳しく説明します。 チュートリアルに進む場合は、このクイック スタートで作成したリソースをクリーンアップしないでください。 進まない場合は、次の手順に従ってリソースをクリーンアップしてください。

1. **AzureMapDemo.html** Web アプリケーションを実行しているブラウザーを閉じます。
2. Azure portal の左側のメニューから、 **[すべてのリソース]** を選択します。 次に、Azure Maps アカウントを選択します。 **[すべてのリソース]** ブレードの上部にある **[削除]** を選択します。

## <a name="next-steps"></a>次のステップ

このクイック スタートでは、Azure Maps アカウントを作成し、デモ アプリを作成しました。 Azure Maps の詳細については、以下のチュートリアルを参照してください。

> [!div class="nextstepaction"]
> [Azure Maps を使用して近くの目的地を検索する](tutorial-search-location.md)

他のコード例や対話型のコーディング エクスペリエンスについては、以下のガイドを参照してください。

> [!div class="nextstepaction"]
> [Azure Maps Search サービスを使用して住所を検索する](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Azure Maps のマップ コントロールを使用する](how-to-use-map-control.md)
