---
title: "教學課程：Azure Active Directory 與 Bime 整合 | Microsoft Docs"
description: "了解如何使用 Bime 搭配 Azure Active Directory 來啟用單一登入、自動化佈建和更多功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: bd3dd077bef87a78904ffd5d2be469b6b8bc8959
ms.openlocfilehash: 7857480d033e4d570aa48569e08bb30846b280f6
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a>教學課程：Azure Active Directory 與 Bime 整合
本教學課程的目的是要示範 Azure 與 Bime 的整合。  
本教學課程中說明的案例假設您已經具有下列項目：

* 有效的 Azure 訂閱
* Bime 租用戶

完成本教學課程之後，您指派給 Bime 的 Azure AD 使用者就能夠單一登入您 Bime 公司網站 (服務提供者起始登入) 的應用程式，或是使用 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。

本教學課程中說明的案例由下列建置組塊組成：

* 啟用 Bime 的應用程式整合
* 設定單一登入 (SSO)
* 設定使用者佈建
* 指派使用者

![案例](./media/active-directory-saas-bime-tutorial/IC775552.png "案例")

## <a name="enable-the-application-integration-for-bime"></a>啟用 Bime 的應用程式整合
本節的目的是要說明如何啟用 Bime 的應用程式整合。

**若要啟用 Bime 的應用程式整合，請執行下列步驟：**

1. 在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory] 。
   
   ![Active Directory](./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
   ![應用程式](./media/active-directory-saas-bime-tutorial/IC700994.png "應用程式")
4. 按一下頁面底部的 [新增]  。
   
   ![新增應用程式](./media/active-directory-saas-bime-tutorial/IC749321.png "新增應用程式")
5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
   ![從資源庫新增應用程式](./media/active-directory-saas-bime-tutorial/IC749322.png "從資源庫新增應用程式")
6. 在**搜尋方塊**中，輸入 **Bime**。
   
   ![應用程式資源庫](./media/active-directory-saas-bime-tutorial/IC775553.png "應用程式資源庫")
7. 在結果窗格中，選取 [Bime]，然後按一下 [完成] 以加入應用程式。
   
   ![Bime](./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
   
## <a name="configure-single-sign-on"></a>設定單一登入

本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己的 Azure AD 帳戶驗證至 Bime。  

設定 Bime 的 SSO 需要您從憑證擷取指紋值。 如果您不熟悉這個程序，請參閱 [如何抓取憑證的指紋值](http://youtu.be/YKQF266SAxI)。

**若要設定單一登入，請執行下列步驟：**

1. 在 Azure 傳統入口網站的 [Bime] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。
   
   ![設定單一登入](./media/active-directory-saas-bime-tutorial/IC771709.png "設定單一登入")
2. 在 [要如何讓使用者登入 Bime] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按 [下一步]。
   
   ![設定單一登入](./media/active-directory-saas-bime-tutorial/IC775555.png "設定單一登入")
3. 在 [設定應用程式 URL] 頁面的 [Bime 登入 URL] 文字方塊中，使用下列模式輸入您的 URL："*https://\<tenant-name\>.Bimeapp.com*"，然後按 [下一步]。
   
   ![設定應用程式 URL](./media/active-directory-saas-bime-tutorial/IC775556.png "設定應用程式 URL")
4. 於 [在 Bime 設定單一登入] 頁面上，按 [下載憑證] 以下載您的憑證，然後在本機將憑證檔案另存為 **c:\\\Bime.cer**。
   
   ![設定單一登入](./media/active-directory-saas-bime-tutorial/IC775557.png "設定單一登入")
5. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 Bime 公司網站。
6. 在工具列中，按一下 [管理]，然後按一下 [帳戶]。
   
   ![管理](./media/active-directory-saas-bime-tutorial/IC775558.png "管理")
7. 在 [帳戶組態] 頁面上，執行下列步驟：
   
   ![設定單一登入](./media/active-directory-saas-bime-tutorial/IC775559.png "設定單一登入")
   
   1. 選取 [啟用 SAML 驗證] 。
   2. 在 Azure 傳統入口網站的 [在 Bime 設定單一登入] 對話頁面上，複製 [遠端登入 URL] 值，然後將它貼至 [遠端登入 URL] 文字方塊中。
   3. 從匯出的憑證複製 [指紋] 值，然後將它貼入 [憑證指紋] 文字方塊。       
      
      >[!TIP]
      >如需詳細資訊，請參閱[如何抓取憑證的指紋值](http://youtu.be/YKQF266SAxI)。 
      > 
   4. 按一下 [儲存] 。
8. 在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。
   
   ![設定單一登入](./media/active-directory-saas-bime-tutorial/IC775560.png "設定單一登入")
   
## <a name="configure-user-provisioning"></a>設定使用者佈建

若要讓 Azure AD 使用者可以登入 Bime，必須將他們佈建到 Bime。  

* Bime 需以手動方式佈建。

**若要設定使用者佈建，請執行下列步驟：**

1. 登入您的 **Bime** 租用戶。
2. 在工具列中，按一下 [管理]，然後按一下 [使用者]。
   
   ![管理](./media/active-directory-saas-bime-tutorial/IC775561.png "管理")
3. 在 [使用者清單] 中，按一下 新增使用者\("+")。
   
   ![使用者](./media/active-directory-saas-bime-tutorial/IC775562.png "使用者")
4. 在 [使用者詳細資料]  對話頁面上，執行下列步驟：
   
   ![使用者詳細資料](./media/active-directory-saas-bime-tutorial/IC775563.png "使用者詳細資料")   
  1. 輸入您想要佈建之有效 AAD 帳戶的名字、姓氏、登入、電子郵件。
  2. 按一下 [儲存]。

>[!NOTE]
>您可以使用任何其他的 Bime 使用者帳戶建立工具或 Bime 提供的 API 來佈建 AAD 使用者帳戶。
>  

## <a name="assign-users"></a>指派使用者
若要測試您的組態，則需指派您所允許使用您應用程式的 Azure AD 使用者，藉此授予其存取組態的權限。

**若要指派使用者給 Bime，請執行下列步驟：**

1. 在 Azure 傳統入口網站中建立測試帳戶。
2. 在 [Bime] 應用程式整合頁面上，按一下 [指派使用者]。
   
   ![指派使用者](./media/active-directory-saas-bime-tutorial/IC775564.png "指派使用者")
3. 選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。
   
   ![是](./media/active-directory-saas-bime-tutorial/IC767830.png "是")

如果要測試您的單一登入設定，請開啟存取面板。 如需 [存取面板] 的詳細資訊，請參閱 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。


