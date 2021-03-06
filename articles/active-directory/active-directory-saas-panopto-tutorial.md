---
title: "教學課程：Azure Active Directory 與 Panopto 整合 | Microsoft Docs"
description: "了解如何使用 Panopto 搭配 Azure Active Directory 來啟用單一登入、自動化佈建和更多功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 078a2ea0db006cf976f89a55c65a536a7b9f04aa
ms.lasthandoff: 04/03/2017


---

# <a name="tutorial-azure-active-directory-integration-with-panopto"></a>教學課程：Azure Active Directory 與 Panopto 整合
本教學課程的目的是要示範 Azure 與 Panopto 的整合。 

本教學課程中說明的案例假設您已經具有下列項目：

* 有效的 Azure 訂閱
* Panopto 租用戶

完成本教學課程之後，您指派給 Panopto 的 Azure AD 使用者就能夠單一登入您 Panopto 公司網站 (服務提供者起始登入) 的應用程式，或是使用 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。

本教學課程中說明的案例由下列建置組塊組成：

1. 啟用 Panopto 的應用程式整合
2. 設定單一登入 (SSO)
3. 設定使用者佈建
4. 指派使用者

![案例](./media/active-directory-saas-panopto-tutorial/IC777665.png "案例")

## <a name="enable-the-application-integration-for-panopto"></a>啟用 Panopto 的應用程式整合
本節的目的是要說明如何啟用 Panopto 的應用程式整合。

**若要啟用 Panopto 的應用程式整合，請執行下列步驟：**

1. 在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory] 。
   
   ![Active Directory](./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
   ![應用程式](./media/active-directory-saas-panopto-tutorial/IC700994.png "應用程式")
4. 按一下頁面底部的 [新增]  。
   
   ![新增應用程式](./media/active-directory-saas-panopto-tutorial/IC749321.png "新增應用程式")
5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
   ![從資源庫新增應用程式](./media/active-directory-saas-panopto-tutorial/IC749322.png "從資源庫新增應用程式")
6. 在 [搜尋方塊]中，輸入 **Panopto**。
   
   ![應用程式資源庫](./media/active-directory-saas-panopto-tutorial/IC777666.png "應用程式資源庫")
7. 在結果窗格中，選取 [Panopto]，然後按一下 [完成] 以新增應用程式。
   
   ![Panopto](./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
   
## <a name="configure-single-sign-on"></a>設定單一登入

本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己在 Azure AD 中的帳戶在 Panopto 中進行驗證。  

在此程序中，您必須建立 Base-64 編碼的憑證檔案。 

如果您不熟悉此程序，請參閱 [如何將二進位憑證轉換成文字檔](http://youtu.be/PlgrzUZ-Y1o)。

**若要設定 SSO，請執行下列步驟：**

1. 在 Azure 傳統入口網站的 [Panopto] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。
   
   ![設定單一登入](./media/active-directory-saas-panopto-tutorial/IC777667.png "設定單一登入")
2. 在 [您希望使用者如何登入 Panopto] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按一下 [下一步]。
   
   ![設定單一登入](./media/active-directory-saas-panopto-tutorial/IC777668.png "設定單一登入")
3. 在 [設定應用程式 URL] 頁面的 [Panopto 登入 URL] 文字方塊中，使用下列模式輸入您的 URL："*https://\<租用戶名稱\>。Panopto.com*"，然後按一下 [下一步]。
   
   ![設定應用程式 URL](./media/active-directory-saas-panopto-tutorial/IC777528.png "設定應用程式 URL")
4. 在 [設定在 Panopto 單一登入] 頁面上，按一下 [下載憑證] 以下載您的憑證，然後將憑證檔案儲存在您的電腦中。
   
   ![設定單一登入](./media/active-directory-saas-panopto-tutorial/IC777669.png "設定單一登入")
5. 在不同的 Web 瀏覽器視窗中，以系統管理員身分登入您的 Panopto 公司網站。
6. 在左側的工具列中，按一下 [系統]，然後按一下 [識別提供者]。
   
   ![系統](./media/active-directory-saas-panopto-tutorial/IC777670.png "系統")
7. 按一下 [新增提供者] 。
   
   ![身分識別提供者](./media/active-directory-saas-panopto-tutorial/IC777671.png "身分識別提供者")
8. 在 [SAML 提供者] 區段中，執行下列步驟：
   
   ![SaaS 設定](./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS 設定")
   
   1. 從 [提供者類型] 清單中，選取 [SAML20]。
   2. 在 [執行個體名稱]  文字方塊中，輸入執行個體的名稱。
   3. 在 [好記的描述]  文字方塊中，輸入好記的描述。
   4. 在 Azure 傳統入口網站的 [設定在 Panopto 單一登入] 對話方塊頁面上，複製 [簽發者 URL] 值，然後貼至 [簽發者] 文字方塊中。
   5. 在 Azure 傳統入口網站中的 [設定在 Panopto 單一登入] 對話頁面上，複製 [SAML SSO URL] 值，然後將它貼至 [退回頁面 URL] 文字方塊中。
   6. 從您下載的憑證建立「Base-64 編碼」  檔案。    
   
      >[!TIP]
      >如需詳細資訊，請參閱 [如何將二進位憑證轉換成文字檔](http://youtu.be/PlgrzUZ-Y1o)。
      >
      
   7. 在記事本中開啟 base-64 編碼的憑證，將其內容複製到剪貼簿，然後貼到 [公開金鑰]  文字方塊中。
   8. 按一下 [儲存] 。

 ![儲存](./media/active-directory-saas-panopto-tutorial/IC777673.png "儲存")
9. 在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。
   
  ![設定單一登入](./media/active-directory-saas-panopto-tutorial/IC777674.png "設定單一登入")
   
## <a name="configure-user-provisioning"></a>設定使用者佈建

沒有動作項目可讓您設定 Panopto 使用者佈建。  
當指派的使用者嘗試使用存取面板登入 Panopto 時，Panopto 會檢查使用者是否存在。  

如果尚無可用的使用者帳戶，Panopto 會自動予以建立。

>[!NOTE]
>您可以使用任何其他的 Panopto 使用者帳戶建立工具或 Panopto 提供的 API，佈建 Azure AD 使用者帳戶。
>
>

## <a name="assign-users"></a>指派使用者
若要測試您的組態，則需指派您所允許使用您應用程式的 Azure AD 使用者，藉此授予其存取組態的權限。

**若要將使用者指派給 Panopto，請執行下列步驟：**

1. 在 Azure 傳統入口網站中建立測試帳戶。
2. 在 [Panopto] 應用程式整合頁面上，按一下 [指派使用者]。
   
   ![指派使用者](./media/active-directory-saas-panopto-tutorial/IC777675.png "指派使用者")
3. 選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。
   
   ![是](./media/active-directory-saas-panopto-tutorial/IC767830.png "是")

如果要測試您的 SSO 設定，請開啟存取面板。 如需 [存取面板] 的詳細資訊，請參閱 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。


