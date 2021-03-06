---
title: 必須パラメーターの収集
services: cognitive-services
author: IEvangelist
manager: nitinme
description: すべての Cognitive Services のコンテナーのパラメーター
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2019
ms.author: dapine
ms.openlocfilehash: bf4e8f06642a4a764794a75e2b49f15958796c61
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73465910"
---
## <a name="gathering-required-parameters"></a>必須パラメーターの収集

すべての Cognitive Services のコンテナーに対して必須である、3 つの主要なパラメーターがあります。 エンドユーザー使用許諾契約書 (EULA) は、`accept` の値と共に提示される必要があります。 さらに、エンドポイント URL と API キーの両方が必要です。

### <a name="endpoint-uri-endpoint_uri"></a>エンドポイント URL `{ENDPOINT_URI}`

**エンドポイント** URI の値は、Azure portal で、対応する Cognitive Service リソースの *[概要]* ページで入手できます。 *[概要]* ページに移動してエンドポイントの上にマウス ポインターを移動すると、`Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> アイコンが表示されます。 必要に応じてコピーして使用します。

![後で使用するためにエンドポイント URI を収集する](../media/overview-endpoint-uri.png)

### <a name="keys-api_key"></a>キー `{API_KEY}`

このキーはコンテナーを起動するために使用され、Azure portal で、対応する Cognitive Service リソースの [キー] ページで入手できます。 *[キー]* ページに移動し、`Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> アイコンをクリックします。

![後で使用するために 2 つのキーのどちらかを入手する](../media/keys-copy-api-key.png)

> [!IMPORTANT]
> これらのサブスクリプション キーは、Cognitive Service API にアクセスするために使用されます。 キーを共有しないでください。 Azure Key Vault を使用するなどして、安全に保管してください。 これらのキーを定期的に再生成することもお勧めします。 API 呼び出しを行うために必要なキーは 1 つだけです。 最初のキーを再生成するときに、2 番目のキーを使用してサービスに継続的にアクセスすることができます。