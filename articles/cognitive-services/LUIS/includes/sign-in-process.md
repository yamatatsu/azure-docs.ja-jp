---
title: インクルード ファイル
description: インクルード ファイル
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 5e7feac35ddd607d36cb0ddd3bae10cf1b9bc7a7
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74278599"
---
## <a name="sign-in-to-luis-portal"></a>LUIS ポータルにサインインする

LUIS の新しいユーザーは、次の手順に従う必要があります。

1. [LUIS ポータル (プレビュー)](https://preview.luis.ai) にサインインし、国を選択して利用規約に同意します。 代わりに **[マイ アプリ]** が表示される場合は、LUIS リソースが既に存在しているため、次の手順に進んでアプリを作成してください。

1. **[Create Azure resource]\(Azure リソースの作成\)** を選択し、 **[Create an authoring resource to migrate your apps to]\(アプリの移行先となるオーサリング リソースの作成\)** を選択します。

    ![Language Understanding のオーサリング リソースのタイプを選択する](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. リソースの詳細を入力します。

    ![オーサリング リソースを作成する](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    **新しい作成リソースを作成する**ときは、次の情報を指定します。 

    * **[Resource name]\(リソース名\)** - 選択したカスタム名。オーサリングおよび予測エンドポイント クエリの URL の一部として使用されます。
    * **[Tenant]\(テナント\)** - Azure サブスクリプションが関連付けられているテナント。 
    * **[Subscription name]\(サブスクリプション名\)** - リソースに対して課金されるサブスクリプション。
    * **[Resource group]\(リソース グループ\)** - 選択または作成するカスタム リソース グループ名。 リソース グループを使用すると、アクセスと管理のために Azure リソースをグループ化できます。 
    * **[Location]\(場所\)** - 場所の選択肢は、**リソース グループ**の選択に基づいて決まります。
    * **[Pricing tier]\(価格レベル\)** - 価格レベルによって、1 秒および 1 か月あたりの最大トランザクション数が決まります。

1. 作成するリソースの概要が表示されます。 **[次へ]** を選択します。

    ![オーサリング リソースを作成する](../media/sign-in/sign-in-confirm-key-selection.png)

1. **[続行]** を選択して確認します。 

    ![オーサリング リソースを作成する](../media/sign-in/sign-in-confirm-continue.png)