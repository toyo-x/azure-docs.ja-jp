---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 17eb8f6d5d2cf8b3ec53eb0fd940594645b9e28d
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2020
ms.locfileid: "86276869"
---
|名前 |説明 |ポリシー |Version |
|---|---|---|---|
|[仮想マシン スケール セットに対して Azure Monitor を有効にする](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |指定されたスコープ (管理グループ、サブスクリプション、またはリソース グループ) 内の仮想マシン スケール セットに対して Azure Monitor を有効にします。 Log Analytics ワークスペースをパラメーターとして取得します。 注: お使いのスケール セット upgradePolicy が手動に設定されている場合、セット内のすべての VM でアップグレードを呼び出して拡張機能を適用することが必要になります。 CLI では、これは az vmss update-instances になります。 |6 |1.0.1 |
|[VM 用 Azure Monitor を有効にする](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |指定されたスコープ (管理グループ、サブスクリプション、リソース グループ) 内の仮想マシン (VM) に対して Azure Monitor を有効にします。 Log Analytics ワークスペースをパラメーターとして取得します。 |10 |2.0.0 |
