---
title: チュートリアル:パターン - LUIS
titleSuffix: Azure Cognitive Services
description: このチュートリアルでは、パターンを使用して意図とエンティティの予測を改善すると共に、与える発話の例を減らします。 このパターンは、エンティティと無視できるテキストを識別するための構文を含むテンプレート発話の例として与えられます。
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: 69894dfc6bcbe9eb56451524c78e82da2745aa52
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75979764"
---
# <a name="tutorial-add-common-pattern-template-utterance-formats-to-improve-predictions"></a>チュートリアル:一般的なパターン テンプレート発話フォーマットを追加して予測を改善する

このチュートリアルでは、パターンを使用して意図とエンティティの予測を改善します。これによって、与える発話の例を減らすことができます。 このパターンは、意図に割り当てられるテンプレート発話であり、エンティティおよび無視できるテキストを識別するための構文を含んでいます。

**このチュートリアルで学習する内容は次のとおりです。**

> [!div class="checklist"]
> * パターンを作成する
> * パターン予測の改善を検証する
> * テキストを無視できるものとマークし、パターン内で入れ子にする
> * テスト パネルを使用してパターンの成功を検証する

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="utterances-in-intent-and-pattern"></a>意図とパターンにおける発話

LUIS アプリに格納される発話には、次の 2 種類があります。

* 意図における発話の例
* パターンにおけるテンプレート発話

テンプレート発話をパターンとして追加することにより、意図に対して与える発話の例を全体的に減らすことができます。

パターンは、式の照合と機械学習の組み合わせとして適用されます。  テンプレート発話は、発話の例と共に、どのような発話が意図に適合するかを LUIS がより適切に解釈できるようにします。

## <a name="import-example-app-and-clone-to-new-version"></a>サンプル アプリをインポートして新しいバージョンにクローンする

次の手順に従います。

1.  [アプリの JSON ファイル](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-batchtest-HumanResources.json?raw=true)をダウンロードして保存します。

1. その JSON を、[プレビュー LUIS ポータル](https://preview.luis.ai)で新しいアプリにインポートします。

1. **[管理]** セクションの **[バージョン]** タブで、バージョンを複製し、それに `patterns` という名前を付けます。 複製は、元のバージョンに影響を及ぼさずに LUIS のさまざまな機能を使用するための優れた方法です。 バージョン名は URL ルートの一部として使用されるため、URL 内で有効ではない文字を名前に含めることはできません。

## <a name="create-new-intents-and-their-utterances"></a>新しい意図とその発話を作成する

1. ナビゲーション バーから **[Build]\(ビルド\)** を選択します。

1. **[Intents]\(意図\)** ページの **[+ Create]\(+ 作成\)** を選択して新しい意図を作成します。

1. ポップアップ ダイアログ ボックスに「`OrgChart-Manager`」と入力して、 **[完了]** を選択します。

    ![新しいメッセージ ポップアップ ウィンドウを作成する](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

1. 発話の例を意図に追加します。 これらの発話は "_厳密_" には異なりますが、抽出可能なパターンは確かに存在します。

    |発話の例|
    |--|
    |`Who is John W. Smith the subordinate of?`|
    |`Who does John W. Smith report to?`|
    |`Who is John W. Smith's manager?`|
    |`Who does Jill Jones directly report to?`|
    |`Who is Jill Jones supervisor?`|

    意図の発話で従業員エンティティの代わりに keyPhrase エンティティにラベルが付いているかどうかは気にしないでください。 どちらも [テスト] ウィンドウとエンドポイントで正しく予測されます。

1. 左側のナビゲーションで、 **[Intents]\(意図\)** を選択します。

1. **[+ Create]\(+ 作成\)** を選択して新しい意図を作成します。 ポップアップ ダイアログ ボックスに「`OrgChart-Reports`」と入力して、 **[完了]** を選択します。

1. 発話の例を意図に追加します。

    |発話の例|
    |--|
    |`Who are John W. Smith's subordinates?`|
    |`Who reports to John W. Smith?`|
    |`Who does John W. Smith manage?`|
    |`Who are Jill Jones direct reports?`|
    |`Who does Jill Jones supervise?`|

### <a name="caution-about-example-utterance-quantity"></a>発話の例の量に関する注意

[!INCLUDE [Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

### <a name="train-the-app-before-testing-or-publishing"></a>テストまたは発行する前に、アプリをトレーニングする

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

### <a name="publish-the-app-to-query-from-the-endpoint"></a>アプリを発行してエンドポイントからクエリを実行する

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

### <a name="get-intent-and-entities-from-endpoint"></a>エンドポイントから意図とエンティティを取得する

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. アドレスの URL の末尾に移動し、「`Who is the boss of Jill Jones?`」と入力します。 最後のクエリ文字列パラメーターは発話の `query` です。

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.313054234
                },
                "OrgChart-Reports": {
                    "score": 0.2462688
                },
                "EmployeeFeedback": {
                    "score": 0.0488328524
                },
                "GetJobInformation": {
                    "score": 0.0156933
                },
                "MoveEmployee": {
                    "score": 0.011265873
                },
                "Utilities.StartOver": {
                    "score": 0.003065792
                },
                "Utilities.Stop": {
                    "score": 0.00300148362
                },
                "Utilities.Cancel": {
                    "score": 0.00271081156
                },
                "None": {
                    "score": 0.00212835032
                },
                "ApplyForJob": {
                    "score": 0.0020669254
                },
                "Utilities.Confirm": {
                    "score": 0.00200891262
                },
                "FindForm": {
                    "score": 0.00194145238
                },
                "Utilities.Help": {
                    "score": 0.00182301877
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "Employee": [
                    [
                        "Employee-45612"
                    ]
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "boss of Jill Jones",
                            "startIndex": 11,
                            "length": 18,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "Employee": [
                        {
                            "type": "Employee",
                            "text": "Jill Jones",
                            "startIndex": 19,
                            "length": 10,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

このクエリは成功しましたか。 このトレーニング サイクルの場合、それは成功しました。 上位 2 つの意図のスコアは近いですが、最も高い意図は有意に高い (60% 超) とは言えず、また次の意図のスコアよりも十分高いとは言えません。

LUIS トレーニングは毎回まったく同じではなく、少し変動があるため、次のトレーニング サイクルではこれらの 2 つのスコアが逆になる可能性があります。 その結果、間違った意図が返されることがあります。

正しい意図のスコアを割合で大幅に高く、また次に高いスコアからさらに離れたものにするパターンを使用してください。

この 2 つ目のブラウザー ウィンドウは開いたままにします。 後でまた使います。

## <a name="template-utterances"></a>テンプレート発話
人事ドメインの性質上、組織内の従業員の関係について一般的な尋ね方がいくつかあります。 次に例を示します。

|発話|
|--|
|`Who does Jill Jones report to?`|
|`Who reports to Jill Jones?`|

発話の多数の例を提供せずに、それぞれの文脈の一意性を判断するには、これらの発話は類似しすぎています。 意図のパターンを追加することにより、発話の多数の例を提供しなくても、LUIS は意図の一般的な発話パターンを学習します。

この意図のテンプレート発話例には、次のものが含まれます:

|テンプレート発話例|構文の意味|
|--|--|
|`Who does {Employee} report to[?]`|交換可能な `{Employee}`<br>無視 `[?]`|
|`Who reports to {Employee}[?]`|交換可能な `{Employee}`<br>無視 `[?]`|

`{Employee}` 構文では、テンプレート発話内でのエンティティの位置とそれがどのエンティティであるかがマークされます。 オプションの構文 `[?]` は、オプションである単語または句読点をマークします。 LUIS は、かっこ内のオプションのテキストを無視し、発話を照合します。

構文は正規表現のように見えますが、正規表現ではありません。 中かっこ `{}` と角かっこ `[]` の構文のみがサポートされています。 これらは 2 階層まで入れ子にすることができます。

パターンが発話に一致するには、まず、発話内のエンティティがテンプレート発話内のエンティティに一致している必要があります。 つまり、エンティティを含んだパターンがうまく機能するためには、発話の例に、予測精度の高いエンティティの例が十分に存在していなければなりません。 ただし、テンプレートは意図の予測にのみ役立ち、エンティティの予測には役立ちません。

**パターンを使用すると提供される発話の例を少なくすることができますが、エンティティが検出されない場合、パターンは一致しません。**

### <a name="add-the-patterns-for-the-orgchart-manager-intent"></a>OrgChart-Manager 意図のパターンを追加する

1. 上部メニューの **[ビルド]** を選択します。

1. 左側のナビゲーションの **[Improve app performance]\(アプリのパフォーマンスの改善\)** で、 **[Patterns]\(パターン\)** を選択します。

1. **OrgChart-Manager** 意図を選択し、次のテンプレート発話を入力します。

    |テンプレート発話|
    |:--|
    |`Who is {Employee} the subordinate of[?]`|
    |`Who does {Employee} report to[?]`|
    |`Who is {Employee}['s] manager[?]`|
    |`Who does {Employee} directly report to[?]`|
    |`Who is {Employee}['s] supervisor[?]`|
    |`Who is the boss of {Employee}[?]`|

1. [パターン] ページで **OrgChart-Reports** 意図を選択し、次のテンプレート発話を入力します。

    |テンプレート発話|
    |:--|
    |`Who are {Employee}['s] subordinates[?]`|
    |`Who reports to {Employee}[?]`|
    |`Who does {Employee} manage[?]`|
    |`Who are {Employee} direct reports[?]`|
    |`Who does {Employee} supervise[?]`|
    |`Who does {Employee} boss[?]`|

### <a name="query-endpoint-when-patterns-are-used"></a>パターンが使用されるときにエンドポイントにクエリを実行する

パターンがアプリに追加されたので、予測ランタイム エンドポイントでアプリのトレーニング、公開、およびクエリを実行します。

1. **[Train]\(トレーニング\)** を選択します。 トレーニングが完了したら、 **[Publish]\(発行\)** を選択し、 **[Production]\(運用\)** スロットを選択して、 **[Done]\(完了\)** を選択します。

1. 発行が完了したら、ブラウザーのタブをエンドポイント URL のタブに再び切り替えます。

1. アドレスの URL の末尾に移動し、発話として「`Who is the boss of Jill Jones?`」と入力します。 最後のクエリ文字列パラメーターは `query` です。

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.999997854
                },
                "OrgChart-Reports": {
                    "score": 6.13748343E-05
                },
                "EmployeeFeedback": {
                    "score": 8.052567E-06
                },
                "GetJobInformation": {
                    "score": 1.18197136E-06
                },
                "MoveEmployee": {
                    "score": 7.65549657E-07
                },
                "None": {
                    "score": 3.975E-09
                },
                "Utilities.StartOver": {
                    "score": 1.53E-09
                },
                "Utilities.Confirm": {
                    "score": 1.38181822E-09
                },
                "Utilities.Help": {
                    "score": 1.38181822E-09
                },
                "Utilities.Stop": {
                    "score": 1.38181822E-09
                },
                "Utilities.Cancel": {
                    "score": 1.25833333E-09
                },
                "FindForm": {
                    "score": 1.15384613E-09
                },
                "ApplyForJob": {
                    "score": 5.26923061E-10
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "Employee": [
                    [
                        "Employee-45612"
                    ]
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "boss of Jill Jones",
                            "startIndex": 11,
                            "length": 18,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "Employee": [
                        {
                            "type": "Employee",
                            "text": "Jill Jones",
                            "startIndex": 19,
                            "length": 10,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

これで、意図の予測の確度は大幅に高くなり、次に高い意図のスコアは大幅に小さくなりました。 トレーニング時に、この 2 つの意図が入れ替わることはありません。

### <a name="working-with-optional-text-and-prebuilt-entities"></a>省略可能なテキストと事前構築済みエンティティの使用

このチュートリアルの前掲のパターン テンプレートの発話には、文字 s の所有格での使用 `'s` や疑問符の使用 `?` など、省略可能なテキストの例がいくつか含まれました。 発話テキストで現在および将来の日付を考慮する必要があるとします。

発話の例は次のようになります。

|Intent|省略可能なテキストと事前構築済みエンティティを含む発話の例|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

これらの各例では、LUIS が正しく予測するために必要な動詞の時制 `was`、`is`、`will be`、および日付 `March 3`、`now`、`in a month` が使用されています。 この表の最後の 2 つの例では、`in` と `on` 除きほとんど同じテキストが使用されていることに注意してください。

この省略可能な情報を考慮したテンプレート発話の例:

|Intent|省略可能なテキストと事前構築済みエンティティを含む発話の例|
|:--|:--|
|OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?`|
|OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|


省略可能な構文の角かっこ `[]` を使用すると、この省略可能なテキストをテンプレート発話に簡単に追加でき、最大 2 レベルまで入れ子にでき `[[]]`、エンティティやテキストを含めることができます。


**質問: 各テンプレート発話の最初の文字 `w` がすべて小文字になっているのはなぜですか。必要に応じて大文字または小文字にする必要はありませんか。** クライアント アプリケーションによってクエリ エンドポイントに送信される発話は、小文字に変換されます。 テンプレート発話は大文字でも小文字でもよく、エンドポイント発話もどちらでもかまいません。 比較は常に小文字への変換後に行われます。

**質問: 3 月 3 日が数値 `3` および日付 `March 3` の両方として予測される場合、事前構築済みの数がテンプレート発話の一部になっていないのはなぜですか。** コンテキスト上、テンプレート発話では日付を `March 3` のようにリテラルで、または `in a month` のように抽象化して使用しています。 日付は数値を含むことができますが、数値は必ずしも日付として見えなくてもかまいません。 常に、予測 JSON の結果で返したい種類をもっともよく表すエンティティを使用します。

**質問: `Who will {Employee}['s] manager be on March 3?` のような不適切な語句の発話はどうなりますか。** `will` と `be` が分けられるこのような文法的に異なる動詞の時制は、新しいテンプレート発話にする必要があります。 既存のテンプレート発話は一致しません。 発話の意図は変わっていませんが、発話での単語の配置が変わっています。 この変更は、LUIS での予測に影響します。 動詞の時制の[グループ化や OR 演算子](#use-the-or-operator-and-groups)を使用して、このような発話を組み合わせることができます。

**注意: エンティティが最初に見つけられた後、パターンが照合されます。**

### <a name="edit-the-existing-pattern-template-utterance"></a>既存のパターン テンプレート発話を編集する

1. プレビュー LUIS ポータルで、上部メニューの **[Build]\(ビルド\)** を選択し、左側のメニューで **[Patterns]\(パターン\)** を選択します。

1. 既存のテンプレート発話 `Who is {Employee}['s] manager[?]` を検索し、右側の省略記号 ([***...***]) を選択し、ポップアップ メニューから **[編集]** を選択します。

1. テンプレートの発話を `who is {Employee}['s] manager [[on]{datetimeV2}?]` に変更します。

### <a name="add-new-pattern-template-utterances"></a>新しいパターン テンプレート発話を追加する

1. **[Build]\(ビルド\)** の **[Patterns]\(パターン\)** セクションで、複数の新しいパターン テンプレート発話を追加します。 [Intent]\(意図\) ドロップダウン メニューから **OrgChart-Manager** を選択し、次のテンプレート発話を入力します。

    |Intent|省略可能なテキストと事前構築済みエンティティを含む発話の例|
    |--|--|
    |OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. ナビゲーション バーの **[Train]\(トレーニング\)** を選択して、アプリをトレーニングします。

3. トレーニングが完了したら、パネル上部の **[Test]\(テスト\)** を選択して、テスト パネルを開きます。

4. テスト発話をいくつか入力し、パターンが一致して意図スコアが十分に高いことを確認します。

    最初の発話を入力した後、結果の下の **[Inspect]\(検査\)** を選択してすべての予測結果を見ることができます。 各発話には **OrgChart-Manager** 意図があるため、Employee and datetimeV2 のエントリの値が抽出されるはずです。

    |発話|
    |--|
    |`Who will be Jill Jones manager`|
    |`who will be jill jones's manager`|
    |`Who will be Jill Jones's manager?`|
    |`who will be Jill jones manager on March 3`|
    |`Who will be Jill Jones manager next Month`|
    |`Who will be Jill Jones manager in a month?`|

これらの発話はすべてエンティティ内で検出され、したがって同じパターンに一致し、高い予測スコアになります。 さまざまなバリエーションの発話と一致するパターンをいくつか追加しました。 パターンとしてテンプレート発話を機能させるために、発話の例を意図に対して追加する必要はありません。

このようにパターンを使用することで、次の結果が得られました。
* 予測スコアが向上した
* 意図における発話の例は同じまま
* 適切に作成されたテンプレート発話をいくつかパターンに含めただけ

### <a name="use-the-or-operator-and-groups"></a>OR 演算子とグループを使用する

前述のテンプレート発話の一部はとても似ています。 テンプレート発話を減らすには、**グループ** `()` と **OR** `|` 構文を使用します。

次の 2 つのパターンは、グループ `()` と OR `|` の構文を使用して 1 つのパターンに結合することができます。

|Intent|省略可能なテキストと事前構築済みエンティティを含む発話の例|
|--|--|
|OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

新しいテンプレート発話は次のようになります。

`who ( was | is | will be ) {Employee}['s] manager [([in]|[on]){datetimeV2}?]`.

ここでは、必須の動詞の時制を囲む**グループ**と、**or** パイプでつなげた省略可能な `in` および `on` を使用しています。

1. **[パターン]** ページで **[OrgChart-Manager]** フィルターを選択します。 `manager` を検索して一覧を絞り込みます。

1. いずれかのバージョンのテンプレート発話を保存し (次の手順で編集します)、他のバリエーションを削除します。

1. テンプレートの発話を次のように変更します。

    `who ( was | is | will be ) {Employee}['s] manager [([in]|[on]){datetimeV2}?]`

2. ナビゲーション バーの **[Train]\(トレーニング\)** を選択して、アプリをトレーニングします。

3. トレーニングが完了したら、パネル上部の **[Test]\(テスト\)** を選択して、テスト パネルを開きます。

    テスト ウィンドウを使用して発話のバージョンをテストします。

    |テスト ウィンドウに入力する発話|
    |--|
    |`Who is Jill Jones manager this month`|
    |`Who is Jill Jones manager on July 5th`|
    |`Who was Jill Jones manager last month`|
    |`Who was Jill Jones manager on July 5th`|
    |`Who will be Jill Jones manager in a month`|
    |`Who will be Jill Jones manager on July 5th`|

より多くのパターン構文を使用すれば、高い予測スコアを保ったまま、アプリ内で管理すべきテンプレート発話の数を減らすことができます。

### <a name="use-the-utterance-beginning-and-ending-anchors"></a>発話の開始と終了のアンカーを使用する

パターン構文には、キャレット `^` の最初と最後の発話アンカー構文が用意されています。 最初と最後の発話アンカーは、非常に具体的でリテラルな可能性がある発話をターゲットにするために一緒に使用するか、意図をターゲットにするために個別に使用することができます。

## <a name="using-patternany-entity"></a>Pattern.any エンティティを使用する

pattern.any エンティティは、エンティティの表現が原因で発話の残りの部分からエンティティの終わりを判別するのが難しい自由形式データを見つけるために使用できます。

この人事アプリは、従業員が会社のフォームを見つける際に役立ちます。

|発話|
|--|
|**HRF-123456** はどこにありますか?|
|誰が **HRF-123234** を作成しましたか?|
|**HRF-456098** はフランス語で発行されますか?|

ただし、各フォームには、前の表で使用されているフォーマット済みの名前と、`Request relocation from employee new to the company 2018 version 5` のようなわかりやすい名前の両方があります。

わかりやすいフォーム名を持つ発話は次のようになります。

|発話|
|--|
|**会社の新しい従業員からの配置換えリクエスト 2018 バージョン 5** はどこにありますか?|
|誰が **"会社の新しい従業員からの配置換えリクエスト 2018 バージョン 5"** を作成しましたか?|
|**会社の新しい従業員からの配置換えリクエスト 2018 バージョン 5** はフランス語で発行されますか?|

さまざまな長さには、エンティティの末尾がどこであるかについて LUIS を混乱させる可能性のある単語が含まれています。 パターンで Pattern.any エンティティを使用すると、フォーム名の先頭と末尾を指定できるため、LUIS はフォーム名を正しく抽出できます。

|テンプレート発話の例|
|--|
|{FormName} はどこですか[?]|
|誰が {FormName} を作成しましたか[?]|
|{FormName} はフランス語で発行されますか[?]|

### <a name="add-example-utterances-with-patternany"></a>Pattern.any を使用して発話の例を追加する

1. 上部のナビゲーションから **[ビルド]** を選択し、左側のナビゲーションから **[Intents]\(意図\)** を選択します。

1. 意図の一覧から **FindForm** を選択します。

1. 発話の例をいくつか追加します。

    |発話の例|フォーム名|
    |--|--|
    |フォーム **ラボで火災が発生したときにするべきこと** はどこにあり、私がそれを読んだ後に誰が署名する必要がありますか?|ラボで火災が発生したときにするべきこと
    |**会社の新しい従業員からの配置換えリクエスト** はサーバー上のどこにありますか?|会社の新しい従業員からの配置換えリクエスト|
    |"**メイン キャンパスでの健康とウェルネスのリクエスト**" を作成したのは誰で、最新バージョンはいくつですか?|メイン キャンパスでの健康とウェルネスのリクエスト|
    |私は "**物理資産を含むオフィス移転リクエスト**" という名前のフォームを探しています。 |物理資産を含むオフィス移転リクエスト|

    Pattern.any エンティティがない場合、フォーム名の多くのバリエーションのため、フォームのタイトルの末尾はどこかを LUIS が理解することは難しくなります。

### <a name="create-a-patternany-entity"></a>Pattern.any エンティティの作成
Pattern.any エンティティは、さまざまな長さのエンティティを抽出します。 パターン構文によってエンティティの先頭と末尾がマークされるため、これはそのパターン内でのみ機能します。

1. 左側のナビゲーションの **[Entities]\(エンティティ\)** を選択します。

1. **[+ Create]\(+ 作成\)** を選択し、名前「`FormName`」を入力し、タイプとして **[Pattern.any]** を選択します。 **作成** を選択します。

### <a name="add-a-pattern-that-uses-the-patternany"></a>Pattern.any を使用するパターンを追加する

1. 左側のナビゲーションの **[Patterns]\(パターン\)** を選択します。

1. 意図 **[FindForm]** を選択します。

1. 新しいエンティティを使用する次のテンプレート発話を入力します。

    |テンプレート発話|
    |--|
    |`Where is the form ["]{FormName}["] and who needs to sign it after I read it[?]`|
    |`Where is ["]{FormName}["] on the server[?]`|
    |`Who authored ["]{FormName}["] and what is the most current version[?]`|
    |`I'm looking for the form named ["]{FormName}["][.]`|

1. アプリをトレーニングします。

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>新しいパターンで自由形式のデータが抽出されるかどうかをテストする
1. 上部のバーの **[Test]\(テスト\)** を選択して、テスト パネルを開きます。

1. 次の発話を入力します。

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

1. 結果の下の **[検査]** を選択して、エンティティと意図のテスト結果を確認します。

    まずエンティティ `FormName` が検出され、次にパターンが検出されて意図が決定されます。 テスト結果でエンティティが検出されず、その結果としてパターンが検出されない場合は、(パターンではなく) その意図の発話の例をさらに追加する必要があります。

1. 上部のナビゲーションの **[Test]\(テスト\)** を選択してテスト パネルを閉じます。

### <a name="using-an-explicit-list"></a>明示的なリストの使用

Pattern.any が含まれているパターンでエンティティが正しく抽出されない場合は、[明示的なリスト](reference-pattern-syntax.md#explicit-lists)を使用してこの問題を修正します。

## <a name="what-did-this-tutorial-accomplish"></a>このチュートリアルで達成されたこと

このチュートリアルでは、LUIS で意図を予測しやすくすると共に、発話の例を追加しなくてもスコアを大幅に引き上げることができるようパターンを追加しました。 エンティティと無視可能テキストのマーク付けによって、LUIS でより多様な発話にパターンを適用できるようになりました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>次のステップ


> [!div class="nextstepaction"]
> [パターンと共にロールを使用する方法について](luis-tutorial-pattern.md)
