---
title: macOS から Windows Virtual Desktop に接続する - Azure
description: macOS クライアントを使用して Windows Virtual Desktop に接続する方法。
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: helohr
ms.openlocfilehash: a46be5dea8286194ccd2558b2e6e301e2624a5f7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460213"
---
# <a name="connect-with-the-macos-client"></a>macOS クライアントに接続する

> 適用対象: macOS 10.12 以降

ダウンロード可能なクライアントを使用して、ご使用の macOS デバイスから Windows Virtual Desktop リソースにアクセスできます。 このガイドでは、クライアントを設定する方法について説明します。

## <a name="install-the-client"></a>クライアントをインストールします。

開始するには、クライアントを[ダウンロード](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12)してご使用の macOS デバイスにインストールします。

## <a name="subscribe-to-a-feed"></a>フィードのサブスクライブ

管理者から提供されたフィードにサブスクライブすると、ご使用の macOS デバイスで使用可能な管理対象リソースの一覧が取得されます。

フィードをサブスクライブするには:

1. サービスに接続して自分のリソースを取得するには、メイン ページで **[フィードの追加]** を選択します。
2. フィード URL を入力します。 URL またはメール アドレスを指定できます。
   - URL を使用する場合は、管理者によって付与されたものを使用します。 通常、URL は <https://rdweb.wvd.microsoft.com> です。
   - 電子メールを使用するには、メール アドレスを入力します。 これにより、メール アドレスに関連付けられている URL を検索するようにクライアントに指示されます (管理者がそのようにサーバーを構成した場合)。
3. **[サブスクライブ]** を選択します。
4. メッセージが表示されたら、自分のユーザー アカウントでサインインします。

サインインすると、使用可能なリソースの一覧が表示されます。

フィードをサブスクライブすると、フィードのコンテンツが自動で定期的に更新されます。 管理者によって行われる変更に基づいて、リソースが追加、変更、または削除されることがあります。

## <a name="next-steps"></a>次のステップ

macOS クライアントの詳細については、「[macOS クライアントの概要](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac)」のドキュメントで確認してください。
