---
title: Azure CLI のサンプル - ネットワーク
description: Azure CLI のサンプル
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 397f2999821e97a1f12362e2aa2fa20b20e78596
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232408"
---
# <a name="azure-cli-samples-for-networking"></a>ネットワーク用の Azure CLI サンプル

次の表には、Azure CLI を使用して構築された Bash スクリプトへのリンクが含まれています。

| スクリプト | 説明 |
|-|-|
|**Azure リソースの間の接続**||
| [多層アプリケーション用の仮想ネットワークの作成](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | フロントエンド サブネットとバックエンド サブネットを含む仮想ネットワークを作成します。 フロントエンド サブネットへのトラフィックは HTTP と SSH に制限され、バックエンド サブネットへのトラフィックは MySQL、ポート 3306 に制限されます。 |
| [2 つの仮想ネットワークのピアリング](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | 同じリージョンに 2 つの仮想ネットワークを作成し、接続します。 |
| [ネットワーク仮想アプライアンス経由のトラフィックのルーティング](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | フロントエンド サブネットとバックエンド サブネットを含む仮想ネットワークと、2 つのサブネット間でトラフィックをルーティングできる VM を作成します。 |
| [VM ネットワークの着信トラフィックおよび送信トラフィックのフィルター処理](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | フロントエンド サブネットとバックエンド サブネットを含む仮想ネットワークを作成します。 フロントエンド サブネットへの着信ネットワーク トラフィックは、HTTP、HTTPS、および SSH に制限されます。 バックエンド サブネットからインターネットへの送信トラフィックは許可されません。 |
|**負荷分散とトラフィックの方向**||
| [VM 上の複数の Web サイトの負荷分散](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Azure 可用性セットに接続され、Azure Load Balancer 経由でアクセス可能な、複数の IP 構成を持つ仮想マシンを 2 つ作成します。 |
| [アプリケーションの高可用性を確保するための、複数リージョン間でのトラフィック転送](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  2 つの App Service プラン、2 つの Web アプリ、1 つの Traffic Manager プロファイル、および 2 つの Traffic Manager エンドポイントを作成します。 |
| | |
