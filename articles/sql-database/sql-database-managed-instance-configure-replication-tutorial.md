---
title: 2 つのマネージド インスタンスと SQL Server の間にトランザクション レプリケーションを構成する
description: このチュートリアルでは、Azure VM 上のパブリッシャー マネージド インスタンス、ディストリビューター マネージド インスタンス、SQL Server サブスクライバーの間のレプリケーションと、プライベート DNS ゾーンや VPN ピアリングなどの必要なネットワーク コンポーネントを構成します。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 11/21/2019
ms.openlocfilehash: e1ee616adadcabb2deb3fa08a5f3d4f768fddfd5
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045653"
---
# <a name="tutorial-configure-transactional-replication-between-two-managed-instances-and-sql-server"></a>チュートリアル:2 つのマネージド インスタンスと SQL Server の間にトランザクション レプリケーションを構成する


このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> - レプリケーション パブリッシャーとしてマネージド インスタンスを構成する。 
> - レプリケーション ディストリビューターとしてマネージド インスタンスを構成する。 
> - サブスクライバーとして SQL サーバーを構成する。 

![SQL マネージド インスタンス パブリッシャー、SQL マネージド インスタンス ディストリビューター、SQL Server サブスクライバーの間のレプリケーション](media/sql-database-managed-instance-failover-group-tutorial/sqlmi-to-sql-replication.png)

このチュートリアルは経験豊富なユーザーを対象としたもので、ユーザーは Azure での両方のマネージド インスタンスおよび SQL Server VM のデプロイと接続について理解していることを前提としています。 そのため、このチュートリアルの一部のステップについては簡単な説明になっています。 

詳しくは、[Azure SQL Database マネージド インスタンスの概要](sql-database-managed-instance-index.yml)、[機能](sql-database-managed-instance.md)、[SQL トランザクション レプリケーション](sql-database-managed-instance-transactional-replication.md)に関する記事をご覧ください。

マネージド インスタンス パブリッシャーとマネージド インスタンス サブスクライバーの間にレプリケーションを構成する方法については、[2 つのマネージド インスタンス間でのトランザクション レプリケーションの構成](replication-with-sql-database-managed-instance.md)に関する記事をご覧ください。 

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、前提条件として次のものが必要です。

- [Azure サブスクリプション](https://azure.microsoft.com/free/)。 
- 同じ仮想ネットワーク内に 2 つのマネージド インスタンスをデプロイした経験。 
- オンプレミスまたは Azure VM での SQL Server サブスクライバー。 このチュートリアルでは Azure VM を使用します。  
- [SQL Server Management Studio (SSMS) 18.0 以降](/sql/ssms/download-sql-server-management-studio-ssms)。
- 最新バージョンの [Azure PowerShell](/powershell/azure/install-az-ps?view=azps-1.7.0)。
- Azure Firewall と Windows ファイアウォールの両方のポート 445 および 1433 で、SQL トラフィックを許可します。 

## <a name="1---create-the-resource-group"></a>1 - リソース グループを作成する
次の PowerShell コード スニペットを使用して、新しいリソース グループを作成します。

```powershell-interactive
# set variables
$ResourceGroupName = "SQLMI-Repl"
$Location = "East US 2"

# Create a new resource group
New-AzResourceGroup -Name  $ResourceGroupName -Location $Location
```

## <a name="2---create-two-managed-instances"></a>2 - マネージド インスタンスを 2 つ作成する
[Azure portal](https://portal.azure.com) を使用して、この新しいリソース グループ内に 2 つのマネージド インスタンスを作成します。 

- パブリッシャー マネージド インスタンスの名前は `sql-mi-publisher` (およびランダム化のためのいくつかの文字)、仮想ネットワークの名前は `vnet-sql-mi-publisher` にする必要があります。
- ディストリビューター マネージド インスタンスの名前は `sql-mi-distributor` (およびランダム化のためのいくつかの文字) にして、"_パブリッシャー マネージド インスタンスと同じ仮想ネットワークに配置する_" 必要があります。

   ![ディストリビューターにはパブリッシャーの仮想ネットワークを使用する](media/sql-database-managed-instance-configure-replication-tutorial/use-same-vnet-for-distributor.png)

マネージド インスタンスの作成について詳しくは、[ポータルでのマネージド インスタンスの作成](sql-database-managed-instance-get-started.md)に関する記事をご覧ください

  > [!NOTE]
  > わかりやすくするため、また最も一般的な構成であるため、このチュートリアルでは、ディストリビューター マネージド インスタンスをパブリッシャーと同じ仮想ネットワーク内に配置することを推奨しています。 ただし、別の仮想ネットワークにディストリビューターを作成することもできます。 そのためには、パブリッシャーとディストリビューターの仮想ネットワークの間に VPN ピアリングを構成してから、ディストリビューターとサブスクライバーの仮想ネットワークの間に VPN ピアリングを構成する必要があります。 

## <a name="3---create-a-sql-server-vm"></a>3 - SQL Server の VM を作成する
[Azure portal](https://portal.azure.com) を使用して SQL Server の仮想マシンを作成します。 SQL Server の仮想マシンは次のように設定する必要があります。

- 名前: `sql-vm-sub`
- イメージ: SQL Server 2016 以降
- リソース グループ: マネージド インスタンスと同じ
- 仮想ネットワーク: `sql-vm-sub-vnet` 

Azure に SQL Server の VM をデプロイする方法について詳しくは、[SQL Server VM の作成に関するクイック スタート](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md)をご覧ください。

## <a name="4---configure-vpn-peering"></a>4 - VPN ピアリングを構成する
VPN ピアリングを構成して、2 つのマネージド インスタンスの仮想ネットワークと SQL Server の仮想ネットワークの間の通信を有効にします。 これを行うには、次の PowerShell コード スニペットを使用します。

```powershell-interactive
# Set variables
$SubscriptionId = '<SubscriptionID>'
$resourceGroup = 'SQLMI-Repl'
$pubvNet = 'sql-mi-publisher-vnet'
$subvNet = 'sql-vm-sub-vnet'
$pubsubName = 'Pub-to-Sub-Peer'
$subpubName = 'Sub-to-Pub-Peer'

$virtualNetwork1 = Get-AzVirtualNetwork `
  -ResourceGroupName $resourceGroup `
  -Name $pubvNet 

 $virtualNetwork2 = Get-AzVirtualNetwork `
  -ResourceGroupName $resourceGroup `
  -Name $subvNet  

# Configure VPN peering from publisher to subscriber
Add-AzVirtualNetworkPeering `
  -Name $pubsubName `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id

# Configure VPN peering from subscriber to publisher
Add-AzVirtualNetworkPeering `
  -Name $subpubName `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id

# Check status of peering on the publisher vNet; should say connected
Get-AzVirtualNetworkPeering `
 -ResourceGroupName $resourceGroup `
 -VirtualNetworkName $pubvNet `
 | Select PeeringState

# Check status of peering on the subscriber vNet; should say connected
Get-AzVirtualNetworkPeering `
 -ResourceGroupName $resourceGroup `
 -VirtualNetworkName $subvNet `
 | Select PeeringState

```

VPN ピアリングを確立した後、SQL サーバーで SQL Server Management Studio (SSMS) を起動し、両方のマネージド インスタンスに接続して、接続をテストします。 SSMS を使用してマネージド インスタンスに接続する方法について詳しくは、[SSMS を使用した MI への接続](sql-database-managed-instance-configure-p2s.md#use-ssms-to-connect-to-the-managed-instance)に関する記事をご覧ください。 

![マネージド インスタンスへの接続をテストする](media/sql-database-managed-instance-configure-replication-tutorial/test-connectivity-to-mi.png)

## <a name="5---create-private-dns-zone"></a>5 - プライベート DNS ゾーンを作成する

プライベート DNS ゾーンを使うと、マネージド インスタンスと SQL サーバーの間の DNS ルーティングが可能になります。 

### <a name="create-private-dns-zone"></a>プライベート DNS ゾーンを作成する
1. [Azure Portal](https://portal.azure.com) にサインインします。
1. **[リソースの作成]** を選択して、新しい Azure リソースを作成します。 
1. Azure Marketplace で `private dns zone` を探します。 
1. Microsoft によって発行された **[プライベート DNS ゾーン]** リソースを選択し、 **[作成]** を選択して DNS ゾーンを作成します。 
1. ドロップダウンからサブスクリプションとリソース グループを選択します 
1. DNS ゾーンに任意の名前を指定します (例: `repldns.com`)。 

   ![プライベート DNS ゾーンを作成する](media/sql-database-managed-instance-configure-replication-tutorial/create-private-dns-zone.png)

1. **[Review + create]\(レビュー + 作成\)** を選択します。 プライベート DNS ゾーンのパラメーターを確認し、 **[作成]** を選択してリソースを作成します。 

### <a name="create-a-record"></a>A レコードを作成する

1. 新しい**プライベート DNS ゾーン**に移動して、 **[概要]** を選択します。 
1. **[+ レコード セット]** を選択して、新しい A レコードを作成します。 
1. SQL Server VM の名前と、プライベート内部 IP アドレスを指定します。 

   ![A レコードを構成する](media/sql-database-managed-instance-configure-replication-tutorial/configure-a-record.png)

1. **[OK]** を選択して A レコードを作成します。 

### <a name="link-the-virtual-network"></a>仮想ネットワークのリンク

1. 新しい**プライベート DNS ゾーン**に移動して、 **[仮想ネットワーク リンク]** を選択します。 
1. **[+ 追加]** を選択します。 
1. リンクの名前を指定します (例: `Pub-link`)。 
1. ドロップダウンからサブスクリプションを選択し、パブリッシャー マネージド インスタンスの仮想ネットワークを選択します。 
1. **[自動登録を有効にする]** のチェック ボックスをオンにします。 

   ![仮想ネットワーク リンクを作成する](media/sql-database-managed-instance-configure-replication-tutorial/configure-vnet-link.png)

1. **[OK]** を選択して、仮想ネットワークをリンクします。 
1. 以上の手順を繰り返し、`Sub-link` のような名前でサブスクライバー仮想ネットワークのリンクを追加します。 


## <a name="6---create-azure-storage-account"></a>6 - Azure ストレージ アカウントを作成する

作業ディレクトリ用に [Azure Storage アカウント](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account)を作成し、そのストレージ アカウント内に[ファイル共有](../storage/files/storage-how-to-create-file-share.md)を作成します。 

`\\storage-account-name.file.core.windows.net\file-share-name` の形式のファイル共有パスをコピーします。   

例: `\\replstorage.file.core.windows.net\replshare`

次の形式でストレージ アクセス キーの接続文字列をコピーします: `DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`   

例: `DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net`


詳細については、[ストレージ アカウントのアクセス キーの管理](../storage/common/storage-account-keys-manage.md)に関するページを参照してください。 


## <a name="7---create-a-database"></a>7 - データベースを作成する
パブリッシャー MI 上に新しいデータベースを作成します。 これを行うには、次のステップに従います。

1. 自分の SQL Server で SQL Server Management Studio (SSMS) を起動します。 
1. `sql-mi-publisher` マネージド インスタンスに接続します。 
1. **[新しいクエリ]** ウィンドウを開き、次の T-SQL クエリを実行してデータベースを作成します。

```sql
-- Create the databases
USE [master]
GO

-- Drop database if it exists
IF EXISTS (SELECT * FROM sys.sysdatabases WHERE name = 'ReplTutorial')
BEGIN
    DROP DATABASE ReplTutorial
END
GO

-- Create new database
CREATE DATABASE [ReplTutorial]
GO

-- Create table
USE [ReplTutorial]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table with data
USE [ReplTutorial]
GO

INSERT INTO ReplTest (ID, c1) VALUES (6, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (2, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (3, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (4, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (5, 'pub')
GO
SELECT * FROM ReplTest
GO
```

## <a name="8---configure-distribution"></a>8 - ディストリビューションを構成する 
接続を確立し、サンプル データベースを作成したら、`sql-mi-distributor` マネージド インスタンスでディストリビューションを構成できます。 これを行うには、次のステップに従います。

1. 自分の SQL Server で SQL Server Management Studio (SSMS) を起動します。 
1. `sql-mi-distributor` マネージド インスタンスに接続します。 
1. **[新しいクエリ]** ウィンドウを開き、次の Transact-SQL コードを実行して、ディストリビューター マネージド インスタンス上にディストリビューションを構成します。 

   ```sql
   EXEC sp_adddistpublisher @publisher = 'sql-mi-publisher.b6bf57.database.windows.net', -- primary publisher
        @distribution_db = N'distribution',
        @security_mode = 0,
        @login = N'azureuser',
        @password = N'<publisher_password>',
        @working_directory = N'\\replstorage.file.core.windows.net\replshare',
        @storage_connection_string = N'<storage_connection_string>'
        -- example: @storage_connection_string = N'DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net'

   ```

   > [!NOTE]
   > @working_directory パラメーターには、円記号 (`\`) のみを使用してください。 スラッシュ (`/`) を使用すると、ファイル共有への接続時にエラーが発生する可能性があります。 

1. `sql-mi-publisher` マネージド インスタンスに接続します。 
1. **[新しいクエリ]** ウィンドウを開き、次の Transact-SQL コードを実行して、パブリッシャーにディストリビューターを登録します。 

```sql
Use MASTER
EXEC sys.sp_adddistributor @distributor = 'sql-mi-distributor.b6bf57.database.windows.net', @password = '<distributor_admin_password>' 
```


## <a name="9---create-the-publication"></a>9 - パブリケーションを作成する
ディストリビューションの構成が済むと、パブリケーションを作成できるようになります。 これを行うには、次のステップに従います。 

1. 自分の SQL Server で SQL Server Management Studio (SSMS) を起動します。 
1. `sql-mi-publisher` マネージド インスタンスに接続します。 
1. **オブジェクト エクスプローラー**で **[レプリケーション]** ノードを展開し、 **[ローカル パブリケーション]** フォルダーを右クリックします。 **[新しいパブリケーション...]** を選択します。 
1. **[次へ]** を選択して、ようこそページの後まで移動します。 
1. **[パブリケーション データベース]** ページで、前に作成した `ReplTutorial` データベースを選択します。 **[次へ]** を選択します。 
1. **[パブリケーションの種類]** ページで、 **[トランザクション パブリケーション]** を選択します。 **[次へ]** を選択します。 
1. **[アーティクル]** ページで、 **[テーブル]** のチェック ボックスをオンにします。 **[次へ]** を選択します。 
1. **[テーブル行のフィルター選択]** ページで、フィルターを追加せずに **[次へ]** を選択します。 
1. **[スナップショット エージェント]** ページで、 **[スナップショットをすぐに作成し、サブスクリプションを初期化できるようにそのスナップショットを保持する]** のチェック ボックスをオンにします。 **[次へ]** を選択します。 
1. **[エージェント セキュリティ]** ページで、 **[セキュリティの設定...]** を選択します。スナップショット エージェントに使用する SQL Server ログイン資格情報と、パブリッシャーに接続するための SQL Server ログイン資格情報を指定します。 **[OK]** を選択して、 **[スナップショット エージェントのセキュリティ]** ページを閉じます。 **[次へ]** を選択します。 

   ![スナップショット エージェントのセキュリティを構成する](media/sql-database-managed-instance-configure-replication-tutorial/snapshot-agent-security.png)

1. **[ウィザードのアクション]** ページで **[パブリケーションを作成する]** を選択し、後のためにこのスクリプトを保存する場合は **[パブリケーションを作成するためのステップを含むスクリプト ファイルを生成する]** を選択します (省略可能)。 
1. **[ウィザードの完了]** ページで、パブリケーションに `ReplTest` という名前を指定し、 **[次へ]** を選択してパブリケーションを作成します。 
1. パブリケーションが作成されたら、**オブジェクト エクスプローラー**の **[レプリケーション]** ノードを更新し、 **[ローカル パブリケーション]** を展開して、新しいパブリケーションを確認します。 


## <a name="10---create-the-subscription"></a>10 - サブスクリプションを作成する 

パブリケーションを作成した後は、サブスクリプションを作成できます。 これを行うには、次のステップに従います。 

1. 自分の SQL Server で SQL Server Management Studio (SSMS) を起動します。 
1. `sql-mi-publisher` マネージド インスタンスに接続します。 
1. **[新しいクエリ]** ウィンドウを開き、次の Transact-SQL コードを実行して、サブスクリプションとディストリビューション エージェントを追加します。 サブスクライバー名の一部として DNS を使用します。 

```sql
use [ReplTutorial]
exec sp_addsubscription 
@publication = N'ReplTest',
@subscriber = N'sql-vm-sub.repldns.com', -- include the DNS configured in the private DNS zone
@destination_db = N'ReplSub', 
@subscription_type = N'Push', 
@sync_type = N'automatic', 
@article = N'all', 
@update_mode = N'read only', 
@subscriber_type = 0

exec sp_addpushsubscription_agent
@publication = N'ReplTest',
@subscriber = N'sql-vm-sub.repldns.com', -- include the DNS configured in the private DNS zone
@subscriber_db = N'ReplSub', 
@job_login = N'azureuser', 
@job_password = '<Complex Password>', 
@subscriber_security_mode = 0, 
@subscriber_login = N'azureuser', 
@subscriber_password = '<Complex Password>', 
@dts_package_location = N'Distributor'
GO
```

## <a name="11---test-replication"></a>11 - レプリケーションをテストする 

レプリケーションが構成されたら、パブリッシャーに新しい項目を挿入し、変更がサブスクライバーに反映されることを確認することでテストできます。 

次の T-SQL スニペットを実行してサブスクライバー上に行を表示します。

```sql
Use ReplSub
select * from dbo.ReplTest
```

次の T-SQL スニペットを実行してパブリッシャーに追加の行を挿入し、サブスクライバー上でその行をもう一度確認します。 

```sql
Use ReplTutorial
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>リソースをクリーンアップする

1. [Azure Portal](https://portal.azure.com) で、リソース グループに移動します。 
1. マネージド インスタンスを選び、 **[削除]** を選択します。 テキスト ボックスに「`yes`」と入力して、リソースを削除することを確認し、 **[削除]** を選択します。 このプロセスはバックグラウンドで完了するまで時間がかかる場合があります。完了するまでは、"*仮想クラスター*" やその他の依存リソースを削除することはできません。 [アクティビティ] タブで削除を監視して、マネージド インスタンスが削除されたことを確認します。 
1. マネージド インスタンスが削除されたら、"*仮想クラスター*" を削除します。そのためには、リソース グループでそれを選び、 **[削除]** を選択します。 テキスト ボックスに「`yes`」と入力して、リソースを削除することを確認し、 **[削除]** を選択します。 
1. 残りのリソースを削除します。 テキスト ボックスに「`yes`」と入力して、リソースを削除することを確認し、 **[削除]** を選択します。 
1. リソース グループを削除するには、 **[リソース グループの削除]** を選択し、リソース グループの名前 (`myResourceGroup`) を入力して、 **[削除]** を選びます。 

## <a name="known-errors"></a>既知のエラー

### <a name="windows-logins-are-not-supported"></a>Windows ログインはサポートされていません

`Exception Message: Windows logins are not supported in this version of SQL Server.`

エージェントは Windows ログインを使用して構成されており、代わりに SQL Server ログインを使用する必要があります。 ログイン資格情報を SQL Server ログインに変更するには、 **[パブリケーションのプロパティ]** の **[エージェント セキュリティ]** ページを使用します。 


### <a name="failed-to-connect-to-azure-storage"></a>Azure Storage に接続できませんでした


`Connecting to Azure Files Storage '\\replstorage.file.core.windows.net\replshare' Failed to connect to Azure Storage '' with OS error: 53.`

2019-11-19 02:21:05.07 replstorage で取得された Azure Storage 接続文字列 2019-11-19 02:21:05.07 Azure Files Storage '\\replstorage.file.core.windows.net\replshare' に接続しています 2019-11-19 02:21:31.21 Azure Storage '' への接続に失敗しました。OS エラー: 53。


これは、おそらく、Azure Firewall と Windows ファイアウォールのどちらか一方または両方でポート 445 が閉じられていることが原因です。 

`Connecting to Azure Files Storage '\\replstorage.file.core.windows.net\replshare' Failed to connect to Azure Storage '' with OS error: 55.`

ファイル共有へのファイル パスで円記号ではなくスラッシュを使用すると、このエラーが発生する可能性があります。 これは問題ありません: `\\replstorage.file.core.windows.net\replshare` これは、OS 55 エラーが発生する可能性があります: `'\\replstorage.file.core.windows.net/replshare'`

### <a name="could-not-connect-to-subscriber"></a>サブスクライバーに接続できませんでした

`The process could not connect to Subscriber 'SQL-VM-SUB`
`Could not open a connection to SQL Server [53].`
`A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections.`

考えられる解決策:
- ポート 1433 が開いていることを確認します。 
- サブスクライバーで TCP/IP が有効になっていることを確認します。 
- サブスクライバーを作成するときに DNS 名を使用したことを確認します。 
- 仮想ネットワークがプライベート DNS ゾーンで正しくリンクされていることを確認します。 
- A レコードが正しく構成されていることを確認します。 
- VPN ピアリングが正しく構成されていることを確認します。 

### <a name="no-publications-to-which-you-can-subscribe"></a>サブスクライブできるパブリケーションがありません

**新しいサブスクリプション** ウィザードを使用して新しいサブスクリプションを追加するときに、 **[パブリケーション]** ページで、使用可能なオプションとしてデータベースとパブリケーションが表示されず、次のエラー メッセージが表示されることがあります。

`There are no publications to which yuo can subscribe, either because this server has no publications or because you do not have sufficient privileges to access the publications.`
 
このエラー メッセージが正しく、実際に接続先のパブリッシャーに使用できるパブリケーションがない場合もありますが、十分なアクセス許可がない可能性もあります。このエラーは、古いバージョンの SQL Server Management Studio が原因で発生することもあります。 これを根本原因から排除するには、SQL Server Management Studio 18.0 以降にアップグレードしてみてください。 


## <a name="next-steps"></a>次のステップ

### <a name="enable-security-features"></a>セキュリティ機能の有効化

データベースをセキュリティで保護する方法をまとめた一覧については、次の[マネージド インスタンスのセキュリティ機能](sql-database-managed-instance.md#azure-sql-database-security-features)に関する記事を参照してください。 次のセキュリティ機能について説明しています。

- [マネージド インスタンスの監査](sql-database-managed-instance-auditing.md) 
- [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine)
- [脅威の検出](sql-database-managed-instance-threat-detection.md) 
- [動的データ マスク](/sql/relational-databases/security/dynamic-data-masking)
- [行レベルのセキュリティ](/sql/relational-databases/security/row-level-security) 
- [透過的なデータ暗号化 (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)

### <a name="managed-instance-capabilities"></a>マネージド インスタンスの機能

マネージド インスタンスの機能全体の概要については、以下を参照してください。

> [!div class="nextstepaction"]
> [マネージド インスタンスの機能](sql-database-managed-instance.md)
