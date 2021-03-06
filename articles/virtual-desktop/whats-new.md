---
title: Windows Virtual Desktop の最新情報 - Azure
description: Windows Virtual Desktop の新機能と製品の更新プログラム。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 06/15/2020
ms.author: helohr
ms.reviewer: thhickli; darank
manager: lizross
ms.openlocfilehash: 7052e9203532320a7de5197e983d40cb0a34b50d
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374782"
---
# <a name="whats-new-in-windows-virtual-desktop"></a>Windows Virtual Desktop の最新情報

Windows Virtual Desktop は定期的に更新されます。 この記事では、以下について説明します。

- 最新の更新プログラム
- 新機能
- 既存の機能の強化
- バグの修正

この記事は毎月更新されます。 こちらを頻繁に確認して、新しい更新プログラムに関する最新情報を入手してください。

## <a name="june-2020"></a>2020 年 6 月

先月、Windows Virtual Desktop Spring 2020 更新プログラムのプレビューが導入されました。 この更新プログラムには、多くの魅力的な新機能が用意されています。 ここでは、Spring 2020 更新プログラムの最新情報を紹介します。

### <a name="windows-virtual-desktop-is-now-integrated-with-azure-resource-manager-preview"></a>Windows Virtual Desktop が Azure Resource Manager (プレビュー) と統合されました

Windows Virtual Desktop が Azure Resource Manager と統合されました。 最新の更新プログラムでは、すべての Windows Virtual Desktop オブジェクトが Azure Resource Manager リソースになりました。 また、この更新プログラムは、Azure のロールベースのアクセス制御 (RBAC) とも統合されています。 詳細については、「[Azure Resource Manager とは](../azure-resource-manager/management/overview.md)」を参照してください。

この変更による効果は次のとおりです。

- Windows Virtual Desktop が Azure portal と統合されました。 つまり、ポータルで直接すべてを管理できるため、PowerShell、Web アプリ、サードパーティ製のツールは必要ありません。 開始するには、[Azure portal でのホスト プールの作成](create-host-pools-azure-marketplace.md)に関するチュートリアルを参照してください。

- Spring 2020 更新プログラムより前は、RemoteApp とデスクトップを個々のユーザーにしか発行できませんでした。 Azure Resource Manager を使用すると、Azure Active Directory グループにリソースを発行できるようになりました。

- 以前のバージョンの Windows Virtual Desktop には、テナントまたはホスト プールに割り当てることができる組み込み管理者ロールが 4 つありました。 これらのロールは、Azure の[ロールベースのアクセス制御](../role-based-access-control/overview.md)に組み込まれました。 これらのロールは、すべての Windows Virtual Desktop Azure Resource Manager オブジェクトに適用することができます。これにより、完全な多機能な委任モデルを作成できます。

- Spring 2020 更新プログラムでは、ホスト プールを拡張するために Azure Marketplace または GitHub テンプレートを繰り返し実行する必要がなくなりました。 ホスト プールを拡張するために必要なのは、Azure portal で対象のホスト プールに移動し、 **[+ 追加]** を選択して追加のセッション ホストをデプロイすることだけです。

- ホスト プールのデプロイは、[Azure Shared Image Gallery](../virtual-machines/windows/shared-image-galleries.md) と完全に統合されました。 Shared Image Gallery は、イメージのバージョン管理を含む仮想マシン (VM) イメージ定義を格納する、別の Azure サービスです。 また、グローバル レプリケーションを使用して、イメージをコピーし、ローカル デプロイ用に他の Azure リージョンに送信することもできます。

- 従来 PowerShell または診断サービス Web アプリを通じて実行されていた監視機能は、Azure portal の Log Analytics に移動されました。 また、レポートを視覚化するための 2 つのオプションも用意されました。 Kusto クエリを実行し、Workbooks を使用すると、ビジュアル レポートを作成できます。

- Windows Virtual Desktop を使用するために Azure Active Directory (Azure AD) の同意を完了する必要がなくなりました。 Spring 2020 更新プログラムでは、Azure サブスクリプションの Azure AD テナントによってユーザーが認証され、管理者に RBAC 制御が提供されます。


### <a name="powershell-support"></a>PowerShell のサポート

Spring 2020 更新プログラムでは、Azure PowerShell Az モジュールに新しい AzWvd コマンドレットが追加されました。 この新しいモジュールは、.NET Core で実行される PowerShell Core でサポートされます。

このモジュールをインストールするには、「[Windows Virtual Desktop 用 PowerShell モジュールを設定する](powershell-module.md)」の手順に従ってください。

また、使用可能なコマンドの一覧は、[AzWvd PowerShell リファレンス](/powershell/module/az.desktopvirtualization/?view=azps-4.2.0#desktopvirtualization)で確認できます。

新機能の詳細については、[こちらのブログ記事](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245)を確認してください。 

### <a name="additional-gateways"></a>追加のゲートウェイ

接続の待ち時間を短縮するために、南アフリカに新しいゲートウェイ クラスターが追加されました。

### <a name="microsoft-teams-on-windows-virtual-desktop-preview"></a>Windows Virtual Desktop での Microsoft Teams (プレビュー)

Windows Virtual Desktop 用の Microsoft Teams に対していくつかの機能強化が行われました。 最も重要なこととして、Windows Virtual Desktop では、オーディオやビジュアルによる通話の転送がサポートされるようになりました。 ユーザーがオーディオまたはビデオを使用して通話する際にユーザー間に直接パスを作成することで、転送による待ち時間が短縮されます。 距離が短いほどホップ数が少なくなるため、通話時の表示と音が滑らかになります。

詳細については、[こちらのブログ記事](https://azure.microsoft.com/updates/windows-virtual-desktop-media-optimization-for-microsoft-teams-is-now-available-in-public-preview/)を参照してください。

## <a name="next-steps"></a>次のステップ

今後の計画については、[Microsoft 365 Windows 仮想デスクトップ ロードマップ](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop)に関するページを参照してください。

Windows Virtual Desktop およびリモート デスクトップ サービスに関するクライアント向けの更新プログラムについては、次の記事を参照してください。

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)
- [Android](/windows-server/remote/remote-desktop-services/clients/android-whatsnew)
- [Web](/windows-server/remote/remote-desktop-services/clients/web-client-whatsnew)
