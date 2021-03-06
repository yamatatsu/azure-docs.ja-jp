---
title: チュートリアル:Promapp を構成し、Azure Active Directory を使用した自動ユーザー プロビジョニングに対応させる | Microsoft Docs
description: Promapp に対してユーザー アカウントの自動的なプロビジョニングとプロビジョニング解除を実行するように Azure Active Directory を構成する方法について説明します。
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 41190ba0-9032-4725-9d2c-b3dd9c7786e6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2019
ms.author: Zhchia
ms.openlocfilehash: 3dd7786b4a976e11b263d89b66cbe5c34285606a
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534747"
---
# <a name="tutorial-configure-promapp-for-automatic-user-provisioning"></a>チュートリアル:Promapp を構成し、自動ユーザー プロビジョニングに対応させる

このチュートリアルの目的は、Azure Active Directory (Azure AD) が、ユーザー、グループ、またはその両方を Promapp に自動的にプロビジョニングおよびプロビジョニング解除するよう構成するために、Promapp と Azure AD で実行する手順を示すことです。

> [!NOTE]
> このチュートリアルでは、Azure AD ユーザー プロビジョニング サービスの上にビルドされるコネクタについて説明します。 このサービスが実行する内容、しくみ、よく寄せられる質問の重要な詳細については、「[Azure Active Directory による SaaS アプリへのユーザー プロビジョニングとプロビジョニング解除の自動化](../manage-apps/user-provisioning.md)」を参照してください。
>
> 現在、このコネクタはパブリック プレビュー段階にあります。 プレビュー機能を使用するための一般的な Microsoft Azure 使用条件の詳細については、「[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)」を参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルで説明するシナリオでは、次の前提条件目があることを前提としています。

* Azure AD テナント
* [Promapp テナント](https://www.promapp.com/licensing/)
* 管理者アクセス許可がある Promapp のユーザー アカウント

## <a name="assigning-users-to-promapp"></a>Promapp へのユーザーの割り当て

Azure Active Directory では、選択されたアプリへのアクセスが付与されるユーザーを決定する際に "*割り当て*" という概念が使用されます。 自動ユーザー プロビジョニングのコンテキストでは、Azure AD 内のアプリケーションに割り当て済みのユーザーとグループのみが同期されます。

自動ユーザー プロビジョニングを構成し、有効にする前に、Promapp へのアクセスが必要な Azure AD のユーザー、グループ、またはその両方を特定する必要があります。 特定した後、次の手順に従い、これらのユーザー、グループ、またはその両方を Promapp に割り当てることができます。
* [エンタープライズ アプリケーションにユーザーまたはグループを割り当てる](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-promapp"></a>ユーザーを Promapp に割り当てるときの重要なヒント

* 1 名の Azure AD ユーザーを Promapp に割り当てて、自動ユーザー プロビジョニングの構成をテストすることをお勧めします。 後でユーザーやグループを追加で割り当てられます。

* Promapp にユーザーを割り当てるときは、割り当てダイアログで、有効なアプリケーション固有ロール (使用可能な場合) を選択する必要があります。 **既定のアクセス** ロールのユーザーは、プロビジョニングから除外されます。

## <a name="setup-promapp-for-provisioning"></a>プロビジョニングのための Promapp の設定

1. [Promapp 管理コンソール](https://freetrial.promapp.com/axelerate/Login.aspx)にサインインします。 ユーザー名の下にある **[My Profile]\(マイ プロファイル\)** に移動します。

    ![Promapp 管理コンソール](media/promapp-provisioning-tutorial/admin.png)

2.  **[Access Tokens]\(アクセス トークン\)** で、 **[Create Token]\(トークンの作成\)** ボタンをクリックします。

    ![Promapp SCIM の追加](media/promapp-provisioning-tutorial/addtoken.png)

3.  **[Description]\(説明\)** フィールドに任意の名前を入力し、 **[Scope]\(スコープ\)** ドロップダウン メニューから **[Scim]\(SCIM\)** を選択します。 保存アイコンをクリックします。

    ![Promapp 名前の追加](media/promapp-provisioning-tutorial/addname.png)

4.  これがアクセス トークンを表示できる唯一のタイミングなので、コピーして保存します。 この値を、Azure portal で Promapp アプリケーションの [Provisioning] (プロビジョニング) タブ内の [Secret Token] (シークレット トークン) フィールドに入力します。

    ![Promapp トークンの作成](media/promapp-provisioning-tutorial/token.png)

## <a name="add-promapp-from-the-gallery"></a>ギャラリーから Promapp を追加する

Azure AD での自動ユーザー プロビジョニング用に Promapp を構成する前に、Azure AD アプリケーション ギャラリーから Promapp を管理対象の SaaS アプリケーションの一覧に追加する必要があります。

**Azure AD アプリケーション ギャラリーから Promapp を追加するには、次の手順を行います。**

1. **[Azure portal](https://portal.azure.com)** の左側のナビゲーション パネルで、 **[Azure Active Directory]** を選択します。

    ![Azure Active Directory のボタン](common/select-azuread.png)

2. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。

    ![[エンタープライズ アプリケーション] ブレード](common/enterprise-applications.png)

3. 新しいアプリケーションを追加するには、ウィンドウの上部にある **[新しいアプリケーション]** ボタンを選びます。

    ![[新しいアプリケーション] ボタン](common/add-new-app.png)

4. 検索ボックスに「**Promapp**」と入力し、結果パネルで **[Promapp]** を選択してから、 **[追加]** ボタンをクリックしてアプリケーションを追加します。

    ![結果一覧の Promapp](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-promapp"></a>Promapp への自動ユーザー プロビジョニングを構成する 

このセクションでは、Azure AD でのユーザー、グループ、またはその両方の割り当てに基づいて、Promapp でユーザー、グループ、またはその両方が作成、更新、および無効化されるように Azure AD プロビジョニング サービスを構成する手順について説明します。

> [!TIP]
> [Promapp シングル サインオンのチュートリアル](https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-tutorial)で説明されている手順に従って、Promapp で SAML ベースのシングル サインオンを有効にすることもできます。 シングル サインオンは自動ユーザー プロビジョニングとは別に構成できますが、これらの 2 つの機能は相補的な関係にあります。

### <a name="to-configure-automatic-user-provisioning-for-promapp-in-azure-ad"></a>Azure AD で Promapp の自動ユーザー プロビジョニングを構成するには、次の手順を行います。

1. [Azure Portal](https://portal.azure.com) にサインインします。 **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。

    ![[エンタープライズ アプリケーション] ブレード](common/enterprise-applications.png)

2. アプリケーションの一覧で **[Promapp]** を選択します。

    ![アプリケーションの一覧の Promapp のリンク](common/all-applications.png)

3. **[プロビジョニング]** タブを選択します。

    ![[プロビジョニング] タブ](common/provisioning.png)

4. **[プロビジョニング モード]** を **[自動]** に設定します。

    ![[プロビジョニング] タブ](common/provisioning-automatic.png)

5. **[管理者資格情報]** セクションの **[テナントの URL]** に「`https://api.promapp.com/api/scim`」と入力します。 **[シークレット トークン]** に先ほど取得した**SCIM 認証トークン**の値を入力します。 **[接続テスト]** をクリックして、Azure AD から Promapp に接続できることを確認します。 接続できない場合は、使用中の Promapp アカウントに管理者アクセス許可があることを確認してから、もう一度試します。

    ![テナント URL + トークン](common/provisioning-testconnection-tenanturltoken.png)

6. **[通知用メール]** フィールドに、プロビジョニングのエラー通知を受け取るユーザーまたはグループの電子メール アドレスを入力して、 **[エラーが発生したときにメール通知を送信します]** チェック ボックスをオンにします。

    ![通知用メール](common/provisioning-notification-email.png)

7. **[Save]** をクリックします。

8. **[マッピング]** セクションの **[Synchronize Azure Active Directory Users to Promapp]\(Azure Active Directory ユーザーを Promapp に同期する\)** を選択します。

    ![Promapp ユーザーのマッピング](media/promapp-provisioning-tutorial/usermappings.png)

9. **[属性マッピング]** セクションで、Azure AD から Promapp に同期されるユーザー属性を確認します。 **[Matching]\(照合\)** プロパティとして選択されている属性は、更新処理で Promapp のユーザー アカウントとの照合に使用されます。 **[保存]** ボタンをクリックして変更をコミットします。

    ![Promapp ユーザー属性](media/promapp-provisioning-tutorial/userattributes.png)

11. スコープ フィルターを構成するには、[スコープ フィルターのチュートリアル](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)の次の手順を参照してください。

12. Promapp に対して Azure AD プロビジョニング サービスを有効にするには、 **[設定]** セクションで **[プロビジョニング状態]** を **[オン]** に変更します。

    ![プロビジョニングの状態を [オン] に切り替える](common/provisioning-toggle-on.png)

13. **[設定]** セクションの **[スコープ]** で目的の値を選択して、Promapp にプロビジョニングするユーザーやグループを定義します。

    ![プロビジョニングのスコープ](common/provisioning-scope.png)

14. プロビジョニングの準備ができたら、 **[保存]** をクリックします。

    ![プロビジョニング構成の保存](common/provisioning-configuration-save.png)

これにより、 **[設定]** セクションの **[スコープ]** で 定義したユーザーやグループの初期同期が開始されます。 初期同期は後続の同期よりも実行に時間がかかります。後続の同期は、Azure AD のプロビジョニング サービスが実行されている限り約 40 分ごとに実行されます。 **[同期の詳細]** セクションを使用すると、進行状況を監視できるほか、リンクをクリックしてプロビジョニング アクティビティ レポートを取得できます。このレポートには、Azure AD プロビジョニング サービスによって Promapp に対して実行されたすべてのアクションが記載されています。

Azure AD プロビジョニング ログの読み取りの詳細については、「[自動ユーザー アカウント プロビジョニングについてのレポート](../manage-apps/check-status-user-account-provisioning.md)」をご覧ください。

## <a name="additional-resources"></a>その他のリソース

* [エンタープライズ アプリのユーザー アカウント プロビジョニングの管理](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>次の手順

* [プロビジョニング アクティビティのログの確認方法およびレポートの取得方法](../manage-apps/check-status-user-account-provisioning.md)

