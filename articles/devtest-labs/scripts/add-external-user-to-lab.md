---
title: PowerShell - Azure DevTest Labs のラボに外部ユーザーを追加する
description: この記事では、Azure DevTest Labs のラボに外部ユーザーを追加する Azure PowerShell スクリプトについて説明します。
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: 03a338efe11eddc00217e8e1946824fda27896ec
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286021"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>PowerShell を使用して Azure DevTest Labs のラボに外部ユーザーを追加する

このサンプル PowerShell スクリプトは、Azure DevTest Labs でラボに外部ユーザーを追加します。 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>前提条件
* **ラボ** 。 スクリプトを実行するには、既存のラボが必要です。 

## <a name="sample-script"></a>サンプル スクリプト

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは以下のコマンドを使用します。 

| command | メモ |
|---|---|
| [Get-AzADUser](/powershell/module/az.resources/get-azaduser) | Azure Active Directory からユーザー オブジェクトを再試行します。 |
| [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) | 指定したプリンシパルに、指定のスコープで、指定したロールを割り当てます。 |

## <a name="next-steps"></a>次のステップ

Azure PowerShell の詳細については、[Azure PowerShell のドキュメント](/powershell/)を参照してください。

Azure Lab Services のその他の PowerShell のサンプル スクリプトについては、[Azure Lab Services の PowerShell のサンプル](../samples-powershell.md)に関する記事をご覧ください。
