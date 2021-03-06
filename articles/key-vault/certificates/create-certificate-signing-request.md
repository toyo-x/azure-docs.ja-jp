---
title: Azure Key Vault での CSR の作成とマージ
description: Azure Key Vault での CSR の作成とマージ
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.date: 06/17/2020
ms.author: sebansal
ms.openlocfilehash: ae2d6259bac6a2034edc98de9b0405f32f17fbc3
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85849484"
---
# <a name="creating-and-merging-csr-in-key-vault"></a>Key Vault での CSR の作成とマージ

Azure Key Vault を使用すると、任意の証明機関によって発行されたデジタル証明書を、キー コンテナーに格納することができます。 公開キーと秘密キーのペアを使用して証明書署名要求を作成できます。また、証明書には任意の証明機関の署名を使用できます。 内部のエンタープライズ CA または外部の公開 CA を使用できます。 証明書署名要求 (CSR または証明書要求) は、デジタル証明書の発行を要求するためにユーザーから証明機関 (CA) に送信されるメッセージです。

証明書の一般的な情報については、[Azure Key Vault 証明書](/azure/key-vault/certificates/about-certificates)に関するページを参照してください。

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

## <a name="adding-certificate-in-key-vault-issued-by-a-non-trusted-ca"></a>信頼されていない CA によって発行された証明書を Key Vault に追加する

次の手順を実行すると、Key Vault と提携していない証明機関から証明書を作成できます (たとえば、GoDaddy は信頼されたキー コンテナー CA ではありません)。 


### <a name="azure-powershell"></a>Azure PowerShell



1.  最初に、**証明書ポリシーを作成します**。 このシナリオで選択した CA はサポートされていないため、Key Vault はユーザーの代わりに発行者に対して証明書を登録または更新しません。そのため、発行者名は不明に設定されます。

    ```azurepowershell
    $policy = New-AzKeyVaultCertificatePolicy -SubjectName "CN=www.contosoHRApp.com" -ValidityInMonths 1  -IssuerName Unknown
    ```


2. **証明書署名要求**を作成します。

   ```azurepowershell
   $csr = Add-AzKeyVaultCertificate -VaultName ContosoKV -Name ContosoManualCSRCertificate -CertificatePolicy $policy
   $csr.CertificateSigningRequest
   ```

3. **CA によって署名された CSR 要求**を取得する。`$certificateOperation.CertificateSigningRequest` は、証明書の base4 でエンコードされた証明書署名要求です。 この BLOB を取得し、発行者の証明書要求 Web サイトにダンプできます。 この手順は CA によって異なりますが、この手順を実行する方法については、CA のガイドラインを参照することをお勧めします。 また、certreq や openssl などのツールを使用して、証明書要求に署名し、証明書を生成するプロセスを完了することもできます。


4. Key Vault で**署名済み要求をマージする**。証明書要求が発行者によって署名された後、署名された証明書を戻して、Azure Key Vault で作成された最初の公開キーと秘密キーのペアにマージできます。

    ```azurepowershell-interactive
    Import-AzKeyVaultCertificate -VaultName ContosoKV -Name ContosoManualCSRCertificate -FilePath C:\test\OutputCertificateFile.cer
    ```

    証明書要求が正常にマージされました。

### <a name="azure-portal"></a>Azure portal

1.  任意の CA の CSR を生成するには、証明書を追加するキー コンテナーに移動します。
2.  Key Vault のプロパティ ページで、 **[証明書]** を選択します。
3.  **[生成/インポート]** タブを選択します。
4.  **[証明書の作成]** 画面で、次の値を選択します。
    - **[証明書の作成方法]:** 生成。
    - **[証明書名]:** ContosoManualCSRCertificate。
    - **[Type of Certificate Authority (CA)] (証明機関 (CA) の種類):** 統合されていない CA によって発行された証明書。
    - **[件名]:** `"CN=www.contosoHRApp.com"`
    - 必要に応じて、他の値を選択します。 **Create** をクリックしてください。

    ![証明書のプロパティ](../media/certificates/create-csr-merge-csr/create-certificate.png)
6.  証明書が [証明書] の一覧に追加されていることがわかります。 先ほど作成した新しい証明書を選択します。 証明書の現在の状態は、CA によってまだ発行されていないため、"無効" になります。
7. **[証明書の操作]** タブをクリックし、 **[CSR のダウンロード]** を選択します。
 ![証明書のプロパティ](../media/certificates/create-csr-merge-csr/download-csr.png)

8.  要求が署名されるように、.csr ファイルを CA に持っていきます。
9.  CA によって要求が署名されたら、証明書ファイルを戻し、同じ [証明書の操作] 画面で**署名された要求をマージ**します。

証明書要求が正常にマージされました。

## <a name="troubleshoot"></a>トラブルシューティング

発行された証明書が Azure portal で "無効" 状態になっている場合は、 **[証明書の操作]** に進み、その証明書のエラー メッセージを確認してください。

詳しくは、[Key Vault REST API リファレンス内の証明書の操作](/rest/api/keyvault)の説明をご覧ください。 アクセス許可の設定については、「[Vaults - Create or Update](/rest/api/keyvault/vaults/createorupdate)」(コンテナー - 作成または更新) および「[Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy)」(コンテナー -アクセス ポリシーの更新) をご覧ください。

## <a name="next-steps"></a>次のステップ

- [認証、要求、応答](../general/authentication-requests-and-responses.md)
- [Key Vault 開発者ガイド](../general/developers-guide.md)
