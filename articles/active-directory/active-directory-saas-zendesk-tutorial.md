---
title: "チュートリアル: Azure Active Directory と Zendesk の統合 | Microsoft Docs"
description: "Azure Active Directory と Zendesk の間でシングル サインオンを構成する方法について説明します。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.openlocfilehash: 095fdd68cafbb1bf7d753dd82821bdc2fd089ef0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>チュートリアル: Azure Active Directory と Zendesk の統合

このチュートリアルでは、Zendesk と Azure Active Directory (Azure AD) を統合する方法について説明します。

Zendesk と Azure AD の統合には、次の利点があります。

- Zendesk にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的にZendesk にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Zendesk と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Zendesk でのシングル サインオンが有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[1 か月の評価版を入手できます](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの Zendesk の追加
2. Azure AD シングル サインオンの構成とテスト

## <a name="adding-zendesk-from-the-gallery"></a>ギャラリーからの Zendesk の追加
Azure AD への Zendesk の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Zendesk を追加する必要があります。

**ギャラリーから Zendesk を追加するには、次の手順に従います。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Azure Active Directory のボタン][1]

2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[エンタープライズ アプリケーション] ブレード][2]
    
3. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[新しいアプリケーション] ボタン][3]

4. 検索ボックスに「**Zendesk**」と入力し、結果ウィンドウで **[Zendesk]** を選び、**[追加]** をクリックしてアプリケーションを追加します。

    ![結果一覧の Zendesk](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト

このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、Zendesk で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Zendesk ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Zendesk の関連ユーザーの間で、リンク リレーションシップが確立されている必要があります。

Zendesk で、Azure AD の **[ユーザー名]** の値に **[Username]** の値を割り当て、リンク関係を確立します。

Zendesk で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configure-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[Zendesk テスト ユーザーの作成](#create-a-zendesk-test-user)** - Zendesk で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - Britta Simon が Azure AD シングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#test-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にし、Zendesk アプリケーションでシングル サインオンを構成します。

**Zendesk で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure Portal の **Zendesk** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![シングル サインオン構成のリンク][4]

2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![[シングル サインオン] ダイアログ ボックス](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. **[Zendesk のドメインと URL]** セクションで、次の手順を実行します。

    ![[Zendesk のドメインと URL] のシングル サインオン情報](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. **[サインオン URL]** ボックスに、`https://<subdomain>.zendesk.com` のパターンを使用して URL を入力します。

    b. **[識別子]** ボックスに、`<subdomain>.zendesk.com` の形式で値を入力します。

    > [!NOTE] 
    > これらは実際の値ではありません。 実際のサインオン URL と識別子でこれらの値を更新してください。 これらの値を取得するには、[Zendesk クライアント サポート チーム](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise)にお問い合わせください。 
 
4. **[SAML 署名証明書]** セクションで、証明書の **[拇印]** の値をコピーします。

    ![証明書のダウンロードのリンク](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png)

5. Zendesk は、特定の形式で構成された SAML アサーションを受け入れます。 必須の SAML 属性はありませんが、必要に応じて以下の手順に従って **[ユーザー属性]** セクションから属性を追加できます。 

     ![[シングル サインオンの構成]](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. **[属性の追加]** をクリックして **[属性の追加]** ダイアログを開きます。

    ![シングル サインオンの構成の追加](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_04.png)

    ![シングル サインオンの構成の属性の追加](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    b. **[名前]** ボックスに、その行に対して表示される属性名を入力します。

    c. **[値]** 一覧から、その行に対して表示される値を入力します。
    
    d. **[OK]**をクリックします。
 
    > [!NOTE] 
    > 既定では、Azure AD に含まれていない属性を追加するには拡張属性を使用します。 「[SAMLに設定できるユーザー属性](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-)」をクリックすると、**Zendesk** が受け入れる SAML 属性の完全な一覧が表示されます。  

6. **[保存]** ボタンをクリックします。

    ![[シングル サインオンの構成] の [保存] ボタン](./media/active-directory-saas-zendesk-tutorial/tutorial_general_400.png)

7. **[Zendesk Configuration]\(Zendesk 構成\)** セクションで、**[Zendesk を構成する]** をクリックして、**[サインオンの構成]** ウィンドウを開きます。 **[クイック リファレンス]** セクションから**サインアウト URL と SAML シングル サインオン サービス URL** をコピーします。

    ![Zendesk の構成](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

8. 別の Web ブラウザー ウィンドウで、Zendesk 企業サイトに管理者としてログインします。

9. **[Admin]**をクリックします。

10. 左側のナビゲーション ウィンドウで、**[設定]**、**[セキュリティ]** の順にクリックします。

11. **[Security]\(セキュリティ\)** ページで、次の手順に従います。 
   
     ![Security (セキュリティ)](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")

    ![シングル サインオン](./media/active-directory-saas-zendesk-tutorial/ic773090.png "シングル サインオン")

     a. **[Admin & Agents]\(管理者とエージェント\)** タブをクリックします。

     b. **[シングルサインオン (SSO) と SAML]** を選択し、**[SAML]** を選択します。

     c. **[SAML SSO URL]\(SAML SSO URL\)** ボックスに、Azure Portal からコピーした **SAML シングル サインオン サービス URL** の値を貼り付けます。 

     d. **[Remote Logout URL]\(リモート ログアウト URL\)** ボックスに、Azure Portal からコピーした**サインアウト URL** の値を貼り付けます。
        
     e. **[Certificate Fingerprint] \(証明書のフィンガープリント)** テキスト ボックスに、Azure Portal からコピーした証明書の **THUMBPRINT** 値を貼り付けます。
     
     f.SAML 属性の属性名またはスキーマ リファレンスを入力します。 **[Save]** をクリックします。

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 組み込みドキュメント機能の詳細については、[Azure AD の組み込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関する記事をご覧ください。
> 

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

   ![Azure AD のテスト ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. Azure Portal の左側のウィンドウで、**Azure Active Directory** のボタンをクリックします。

    ![Azure Active Directory のボタン](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png)

2. ユーザーの一覧を表示するには、**[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックします。

    ![[ユーザーとグループ] と [すべてのユーザー] リンク](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png)

3. **[ユーザー]** ダイアログ ボックスを開くには、**[すべてのユーザー]** ダイアログ ボックスの上部にある **[追加]** をクリックしてきます。

    ![[追加] ボタン](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png)

4. **[ユーザー]** ダイアログ ボックスで、次の手順に従います。

    ![[ユーザー] ダイアログ ボックス](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png)

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに、ユーザーである Britta Simon の電子メール アドレスを入力します。

    c. **[パスワードを表示]** チェック ボックスをオンにし、**[パスワード]** ボックスに表示された値を書き留めます。

    d. **Create** をクリックしてください。
 
### <a name="create-a-zendesk-test-user"></a>Zendesk テスト ユーザーの作成

Azure AD ユーザーが **Zendesk** にログインできるようにするには、ユーザーを **Zendesk** にプロビジョニングする必要があります。  
アプリで割り当てられたロールに応じて、想定される動作があります。

 1. **エンドユーザー** アカウントは、サインインするときに自動的にプロビジョニングされます。
 2. **エージェント**と**管理者**のアカウントは、サインインの前に **Zendesk** で手動でプロビジョニングする必要があります。
 
**ユーザー アカウントをプロビジョニングするには、次の手順に従います。**

1. **Zendesk** テナントにログインします。

2. **[顧客リスト]** タブを選択します。

3. **[ユーザー]** タブを選択し、**[追加]** をクリックします。
   
    ![ユーザーの追加](./media/active-directory-saas-zendesk-tutorial/ic773632.png "ユーザーの追加")
4. プロビジョニングする既存の Azure AD アカウントの**名前**と**電子メール アドレス**を入力し、**[Save]\(保存\)** をクリックします。
   
    ![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")

> [!NOTE]
> Zendesk から提供されている他の Zendesk ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に Zendesk へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザー ロールを割り当てる][200] 

**Zendesk に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

2. アプリケーションの一覧で **[Zendesk]** を選択します。

    ![アプリケーションの一覧の Zendesk のリンク](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png)  

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![[ユーザーとグループ] リンク][202]

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![[割り当ての追加] ウィンドウ][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="test-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで [Zendesk] タイルをクリックすると、自動的に Zendesk アプリケーションにサインオンします。
アクセス パネルの詳細については、[アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)に関する記事を参照してください。 

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png

