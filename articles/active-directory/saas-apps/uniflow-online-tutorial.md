---
title: チュートリアル:Azure Active Directory シングル サインオン (SSO) と uniFLOW Online の統合 | Microsoft Docs
description: Azure Active Directory と uniFLOW Online の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28313d27-638c-4d50-92ad-d093f2ae9ecf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9eb369047574ef76dd31996fd16399380ea027c8
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74823154"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-uniflow-online"></a>チュートリアル:Azure Active Directory シングル サインオン (SSO) と uniFLOW Online の統合

このチュートリアルでは、uniFLOW Online と Azure Active Directory (Azure AD) を統合する方法について説明します。 Azure AD と uniFLOW Online を統合すると、次のことができます。

* uniFLOW Online にアクセスできるユーザーを Azure AD で制御する。
* ユーザーが自分の Azure AD アカウントを使用して uniFLOW Online に自動的にサインインできるようにする。
* 1 つの中央サイト (Azure Portal) で自分のアカウントを管理します。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory でのアプリケーションへのシングル サインオン](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)」を参照してください。

## <a name="prerequisites"></a>前提条件

開始するには、次が必要です。

* Azure AD サブスクリプション。 サブスクリプションがない場合は、[無料アカウント](https://azure.microsoft.com/free/)を取得できます。
* uniFLOW Online でのシングル サインオン (SSO) が有効なサブスクリプション。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD の SSO を構成してテストします。

* uniFLOW Online では、**SP** Initiated SSO がサポートされます

## <a name="adding-uniflow-online-from-the-gallery"></a>ギャラリーからの uniFLOW Online の追加

Azure AD への uniFLOW Online の統合を構成するには、ギャラリーからマネージド SaaS アプリの一覧に uniFLOW Online を追加する必要があります。

1. 職場または学校アカウントか、個人の Microsoft アカウントを使用して、[Azure portal](https://portal.azure.com) にサインインします。
1. 左のナビゲーション ウィンドウで **[Azure Active Directory]** サービスを選択します。
1. **[エンタープライズ アプリケーション]** に移動し、 **[すべてのアプリケーション]** を選択します。
1. 新しいアプリケーションを追加するには、 **[新しいアプリケーション]** を選択します。
1. **[ギャラリーから追加する]** セクションで、検索ボックスに「**uniFLOW Online**」と入力します。
1. 結果のパネルから **[uniFLOW Online]** を選択し、アプリを追加します。 お使いのテナントにアプリが追加されるのを数秒待機します。

## <a name="configure-and-test-azure-ad-single-sign-on-for-uniflow-online"></a>uniFLOW Online の Azure AD シングル サインオンの構成とテスト

**B.Simon** というテスト ユーザーを使用して、uniFLOW Online に対する Azure AD SSO を構成してテストします。 SSO が機能するためには、Azure AD ユーザーと uniFLOW Online の関連ユーザーとの間にリンク関係を確立する必要があります。

uniFLOW Online に対する Azure AD SSO を構成してテストするには、次の構成要素を完了します。

1. **[Azure AD SSO の構成](#configure-azure-ad-sso)** - ユーザーがこの機能を使用できるようにします。
    * **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - B.Simon で Azure AD のシングル サインオンをテストします。
    * **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - B.Simon が Azure AD シングル サインオンを使用できるようにします。
1. **[uniFLOW Online SSO の構成](#configure-uniflow-online-sso)** - アプリケーション側でシングル サインオン設定を構成します。
    * **[uniFLOW Online テスト ユーザーの作成](#create-uniflow-online-test-user)** - uniFLOW Online で B.Simon に対応するユーザーを作成し、Azure AD のこのユーザーにリンクさせます。
1. **[SSO のテスト](#test-sso)** - 構成が機能するかどうかを確認します。

## <a name="configure-azure-ad-sso"></a>Azure AD SSO の構成

これらの手順に従って、Azure portal で Azure AD SSO を有効にします。

1. [Azure portal](https://portal.azure.com/) の **uniFLOW Online** アプリケーション統合ページで、 **[管理]** セクションを見つけて、 **[シングル サインオン]** を選択します。
1. **[シングル サインオン方式の選択]** ページで、 **[SAML]** を選択します。
1. **[SAML でシングル サインオンをセットアップします]** ページで、 **[基本的な SAML 構成]** の編集 (ペン) アイコンをクリックして設定を編集します。

   ![基本的な SAML 構成を編集する](common/edit-urls.png)

1. **[基本的な SAML 構成]** セクションで、次のフィールドの値を入力します。

    a. **[サインオン URL]** ボックスに、次の形式で URL を入力します。

    | | |
    |-|-|
    | `https://<tenant_domain_name>.eu.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.us.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.sg.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.jp.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.au.uniFLOWonline.com`|

    b. **[識別子 (エンティティ ID)]** テキスト ボックスに、次のパターンで URL を入力します。

    | | |
    |-|-|
    | `https://<tenant_domain_name>.eu.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.us.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.sg.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.jp.uniFLOWonline.com`|
    | `https://<tenant_domain_name>.au.uniFLOWonline.com`|

    > [!NOTE]
    > これらは実際の値ではありません。 実際のサインオン URL と識別子でこれらの値を更新します。 これらの値を取得するには、[uniFLOW Online クライアント サポート チーム](mailto:support@nt-ware.com)に問い合わせてください。 Azure portal の **[基本的な SAML 構成]** セクションに示されているパターンを参照することもできます。

1. uniFLOW Online アプリケーションでは、特定の形式の SAML アサーションを使用するため、カスタム属性マッピングを SAML トークン属性の構成に追加する必要があります。 次のスクリーンショットには、既定の属性一覧が示されています。

    ![image](common/default-attributes.png)

1. その他に、uniFLOW Online アプリケーションでは、いくつかの属性が SAML 応答で返されることが想定されています。それらの属性を次に示します。 これらの属性も値が事前に設定されますが、要件に従ってそれらの値を確認することができます。

    | 名前 |  ソース属性|
    | -----------| --------------- |
    | displayName | user.displayname |
    | nickname | user.onpremisessamaccountname |

1. **[Set up single sign-on with SAML]\(SAML でシングル サインオンをセットアップします\)** ページの **[SAML 署名証明書]** セクションで、コピー ボタンをクリックして **[アプリのフェデレーション メタデータ URL]** をコピーして、お使いのコンピューターに保存します。

    ![証明書のダウンロードのリンク](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションでは、Azure portal 内で B.Simon というテスト ユーザーを作成します。

1. Azure portal の左側のウィンドウから、 **[Azure Active Directory]** 、 **[ユーザー]** 、 **[すべてのユーザー]** の順に選択します。
1. 画面の上部にある **[新しいユーザー]** を選択します。
1. **[ユーザー]** プロパティで、以下の手順を実行します。
   1. **[名前]** フィールドに「`B.Simon`」と入力します。  
   1. **[ユーザー名]** フィールドに「username@companydomain.extension」と入力します。 たとえば、「 `B.Simon@contoso.com` 」のように入力します。
   1. **[パスワードを表示]** チェック ボックスをオンにし、 **[パスワード]** ボックスに表示された値を書き留めます。
   1. **Create** をクリックしてください。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、B.Simon に uniFLOW Online へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、 **[すべてのアプリケーション]** を選択します。
1. アプリケーションの一覧で **[uniFLOW Online]** を選択します。
1. アプリの概要ページで、 **[管理]** セクションを見つけて、 **[ユーザーとグループ]** を選択します。

   ![[ユーザーとグループ] リンク](common/users-groups-blade.png)

1. **[ユーザーの追加]** を選択し、 **[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![[ユーザーの追加] リンク](common/add-assign-user.png)

1. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧から **[B.Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。
1. SAML アサーション内に任意のロール値が必要な場合、 **[ロールの選択]** ダイアログでユーザーに適したロールを一覧から選択し、画面の下部にある **[選択]** をクリックします。
1. **[割り当ての追加]** ダイアログで、 **[割り当て]** をクリックします。

## <a name="configure-uniflow-online-sso"></a>uniFLOW Online SSO の構成

1. 別の Web ブラウザー ウィンドウで、uniFLOW Online Web サイトに管理者としてサインインします。

1. 左側のナビゲーション パネルで **[User]\(ユーザー\)** タブを選択します。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/configure1.png)

1. **[ID プロバイダー]** をクリックします。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/configure2.png)

1. **[Add identity provider]\(ID プロバイダーの追加\)** をクリックします。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/configure3.png)

1. **[ADD IDENTITY PROVIDER]\(ID プロバイダーの追加\)** セクションで、次の手順を実行します。


    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/configure4.png)

    a. 表示名を入力します (例: *AzureAD SSO*)。

    b. **[Provider type]\(プロバイダーの種類\)** で、ドロップダウンの **[WS-Fed]** オプションを選択します。

    c. **[WS-Fed type]\(WS-Fed の種類\)** で、ドロップダウンの **[Azure Active Directory]** オプションを選択します。

    d. **[Save]** をクリックします。

1. **[General]\(全般\)** タブで、次の手順を実行します。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/configure5.png)

    a. 表示名を入力します (例: *AzureAD SSO*)。

    b. **[ADGS Federation Metadata]\(ADGS フェデレーション メタデータ\)** で **[From URL]\(URL から\)** オプションを選択します。

    c. **[Federation Metadata URl]\(フェデレーション メタデータ URI\)** ボックスに、Azure portal からコピーした**アプリのフェデレーション メタデータ URL** の値を貼り付けます。

    d. **[Identity provider]\(ID プロバイダー\)** で **[Enabled]\(有効化\)** を選択します。

    e. **[Automatic user registration]\(自動ユーザー登録\)** で **[Activated]\(アクティブ化\)** を選択します。

    f. **[Save]** をクリックします。

### <a name="create-uniflow-online-test-user"></a>uniFLOW Online テスト ユーザーの作成

1. 別の Web ブラウザー ウィンドウで、uniFLOW Online Web サイトに管理者としてサインインします。

1. 左側のナビゲーション パネルで **[User]\(ユーザー\)** タブを選択します。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/configure1.png)

1. **[Add user]\(ユーザーの追加\)** をクリックします。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/user1.png)

1. **[Create user manually]\(ユーザーを手動で作成\)** をクリックします。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/user2.png)

1. **[CREATE USER MANUALLY]\(ユーザーを手動で作成\)** タブで、組織の要件に従って必要な値を入力します。

    ![uniFLOW Online の構成](./media/uniflow-online-tutorial/user3.png)

## <a name="test-sso"></a>SSO のテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで [uniFLOW Online] タイルをクリックすると、SSO を設定した uniFLOW Online に自動的にサインインします。 アクセス パネルの詳細については、[アクセス パネルの概要](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)に関する記事を参照してください。

## <a name="additional-resources"></a>その他のリソース

- [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory でのアプリケーション アクセスとシングル サインオンとは](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory の条件付きアクセスとは](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD で uniFLOW Online を試す](https://aad.portal.azure.com/)