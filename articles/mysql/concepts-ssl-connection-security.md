---
title: SSL 接続 - Azure Database for MySQL
description: SSL 接続を適切に使用できるように、Azure Database for MySQL と関連アプリケーションを構成するための情報
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d677e7c80d98b15a638b00c5b8f4d390a492c7fa
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770817"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Azure Database for MySQL の SSL 接続
Azure Database for MySQL では、Secure Sockets Layer (SSL) を使用して、データベース サーバーをクライアント アプリケーションに接続できます。 データベース サーバーとクライアント アプリケーションの間に SSL 接続を適用すると、サーバーとアプリケーションの間のデータ ストリームが暗号化され、"man in the middle" 攻撃から保護されます。

## <a name="default-settings"></a>既定の設定
既定では、MySQL サーバーへの接続時に SSL 接続を要求するようにデータベース サービスを構成する必要があります。  可能な場合は、SSL オプションを無効にしないことをお勧めします。 

Azure Portal や CLI を使用して新しい Azure Database for MySQL サーバーをプロビジョニングする場合、SSL 接続の適用が既定で有効になります。 

さまざまなプログラミング言語の接続文字列が Azure Portal に表示されます。 これらの接続文字列には、データベースへの接続に必要な SSL パラメーターが含まれます。 Azure Portal で、お使いのサーバーを選択します。 **[設定]** の見出しにある **[接続文字列]** を選択します。 SSL パラメーターはコネクタによって異なります ("ssl=true"、"sslmode=require"、"sslmode=required" など)。

アプリケーションの開発時に SSL 接続を有効または無効にする方法については、[SSL の構成方法](howto-configure-ssl.md)に関するページをご覧ください。 

## <a name="next-steps"></a>次の手順
[Azure Database for MySQL の接続ライブラリ](concepts-connection-libraries.md)
