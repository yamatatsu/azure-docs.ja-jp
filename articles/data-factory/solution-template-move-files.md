---
title: ファイル ベースのストレージ間でファイルを移動する
description: Azure Data Factory を使用してファイル ベースのストレージ間でファイルを移動するための、ソリューション テンプレートの使用方法について説明します。
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 7/12/2019
ms.openlocfilehash: b3165daa06ed975df9ccb677699d3ceb449327ab
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2019
ms.locfileid: "74941930"
---
# <a name="move-files-with-azure-data-factory"></a>Azure Data Factory でファイルを移動する

この記事では、あるフォルダーから異なるファイル ベースのストアにファイルを移動するために使用できる、ソリューション テンプレートについて説明します。 このテンプレートを使用する一般的なシナリオの 1 つ: ファイルは、ソース ストアのランディング フォルダーに継続的にドロップされます。 スケジュール トリガーを作成することにより、ADF パイプラインでこれらのファイルをソースから移動先ストアに定期的に移動できます。  ADF パイプラインで "ファイルの移動" を実現する方法では、ランディング フォルダーからファイルを取得し、各ファイルを移動先ストアの別のフォルダーにコピーした後、ソース ストアのランディング フォルダーから同じファイルを削除します。

> [!NOTE]
> このテンプレートは、フォルダーを移動するのではなく、ファイルを移動するように設計されていることに注意してください。  フォルダー パスのみを含むようにデータセットを変更してから、コピー アクティビティと削除アクティビティを使用して、フォルダーを表す同じデータセットを参照することにより、フォルダーを移動する場合は、十分注意する必要があります。 その理由は、コピー操作と削除操作の間にそのフォルダーに新しいファイルが到着しないようにする必要があるからです。 Copy アクティビティがコピー ジョブを完了したが、Delete アクティビティがまだ開始されていないときにフォルダーに新しいファイルが到着すると、Delete アクティビティがフォルダー全体を削除することにより、まだ宛先にコピーされていない新しく到着したファイルを削除する可能性があります。

## <a name="about-this-solution-template"></a>このソリューション テンプレートについて

このテンプレートでは、ソースのファイル ベースのストアからファイルが取得されます。 次に、それぞれを移動先ストアに移動します。

このテンプレートには、5 つのアクティビティが含まれています。
- **Getmetadata** では、ソース ストアのフォルダーからファイルとサブフォルダーを含むオブジェクトのリストが取得されます。 オブジェクトは再帰的には取得されません。 
- **Filter** では、**GetMetadata** アクティビティからのオブジェクト リストがフィルター処理されて、ファイルのみが選択されます。 
- **ForEach** では、**Filter** アクティビティからファイル リストが取得されて反復処理され、各ファイルが Copy アクティビティと Delete アクティビティに渡されます。
- **Copy** では、ソース ストアからコピー先ストアに 1 つのファイルがコピーされます。
- **Delete** では、同じ 1 つのファイルがソース ストアから削除されます。

このテンプレートには、4 つのパラメーターが定義されています。
- *SourceStore_Location* は、ファイルの移動元となるソース ストアのフォルダー パスです。 
- *SourceStore_Directory* は、ファイルの移動元となるソース ストアのサブフォルダー パスです。
- *DestinationStore_Location* は、ファイルの移動先となる移動先ストアのフォルダー パスです。 
- *DestinationStore_Directory* は、ファイルの移動先となる移動先ストアのサブフォルダー パスです。

## <a name="how-to-use-this-solution-template"></a>このソリューション テンプレートの使用方法

1. **ファイルの移動**テンプレートに移動します。 ファイル移動元のソース ファイル ストアに対する既存の接続を選択するか、**新しい**接続を作成します。 **DataSource_Folder** と **DataSource_File** では、ソース ファイル ストアの同じ接続が参照されていることに注意してください。

    ![ソースへの新しい接続を作成する](media/solution-template-move-files/move-files1.png)

2. ファイル移動先の移動先ファイル ストアに対する既存の接続を選択するか、**新しい**接続を作成します。

    ![コピー先への新しい接続を作成する](media/solution-template-move-files/move-files2.png)

3. **[このテンプレートを使用]** タブを選択します。
    
4. 次の例に示すように、パイプラインが表示されます。

    ![パイプラインを表示する](media/solution-template-move-files/move-files4.png)

5. **[デバッグ]** を選択し、 **[パラメーター]** で入力し、 **[完了]** を選択します。   パラメーターは、ファイル移動元のフォルダー パスと、ファイル移動先のフォルダー パスです。 

    ![パイプラインを実行する](media/solution-template-move-files/move-files5.png)

6. 結果を確認します。

    ![結果を確認する](media/solution-template-move-files/move-files6.png)

## <a name="next-steps"></a>次の手順

- [Azure Data Factory で LastModifiedDate に基づいて新規および変更済みのファイルをコピーする](solution-template-copy-new-files-lastmodifieddate.md)

- [Azure Data Factory を使用して複数のコンテナーからファイルをコピーする](solution-template-copy-files-multiple-containers.md)