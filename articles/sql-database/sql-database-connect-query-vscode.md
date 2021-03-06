---
title: "VS Code：在 Azure SQL Database 中連接和查詢資料 | Microsoft Docs"
description: "了解如何使用 Visual Studio Code 在 Azure 上連接到 SQL Database。 然後，執行 TRANSACT-SQL (T-SQL) 陳述式來查詢和編輯資料。"
metacanonical: 
keywords: "連線到 SQL Database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 45405c7bb9993d1fd529b25b599c3cd7f459843c
ms.lasthandoff: 04/20/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL Database︰使用 Visual Studio Code 連接及查詢資料

[Visual Studio Code](https://code.visualstudio.com/docs) 是一個圖形化程式碼編輯器，適用於支援擴充功能的 Linux、macOS 和 Windows，包括可供查詢 Microsoft SQL Server、Azure SQL Database 和 SQL 資料倉儲的 [mssql extension](https://aka.ms/mssql-marketplace)。 此快速入門示範如何使用 Visual Studio Code 來連線至 Azure SQL Database，然後使用 Transact-SQL 陳述式來查詢、插入、更新和刪除資料庫中的資料。

本快速入門可作為在其中一個快速入門中建立之資源的起點︰

- [建立 DB - 入口網站](sql-database-get-started-portal.md)
- [建立 DB - CLI](sql-database-get-started-cli.md)

開始之前，確定您已安裝最新版的 [Visual Studio Code](https://code.visualstudio.com/Download) 並已載入 [mssql 擴充功能](https://aka.ms/mssql-marketplace)。 如需 mssql 擴充功能的安裝指引，請參閱[安裝 VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code)和[適用於 Visual Studio Code 的 mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)。 

## <a name="configure-vs-code"></a>設定 VS Code 

### <a name="mac-os"></a>**Mac OS**
對於 macOS，您必須安裝 OpenSSL，這是 mssql 擴充功能使用之 DotNet Core 的必要條件。 開啟您的終端機，並輸入下列命令以安裝 **brew** 和 **OpenSSL**。 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

不需要特別設定。

### <a name="windows"></a>**Windows**

不需要特別設定。

## <a name="get-connection-information"></a>取得連線資訊

取得連線到 Azure SQL Database 所需的連線資訊。 您在下一個程序中需要完整的伺服器名稱、資料庫名稱和登入資訊。

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 從左側功能表中選取 [SQL Database]，按一下 [SQL Database]頁面上您的資料庫。 
3. 在您資料庫的 [概觀] 頁面上，檢閱如下圖所示的完整伺服器名稱。 您可以將滑鼠移至伺服器名稱上，以帶出 [按一下以複製] 選項。

   ![連線資訊](./media/sql-database-connect-query-ssms/connection-information.png) 

4. 如果您忘記 Azure SQL Database 伺服器的登入資訊，請瀏覽至 [SQL Database 伺服器] 頁面來檢視伺服器系統管理員名稱，並視需要重設密碼。 

## <a name="set-language-mode-to-sql"></a>將語言模式設定為 SQL

在 Visual Studio Code 中將語言模式設定為 **SQL**，以啟用 mssql 命令和 T-SQL IntelliSense。

1. 開啟新的 Visual Studio Code 視窗。 

2. 按一下狀態列右下角的 [純文字]。
3. 在開啟的 [選取語言模式] 下拉式功能表中，輸入 **SQL**，然後按 **ENTER** 將語言模式設定為 SQL。 

   ![SQL 語言模式](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-to-your-database-in-the-sql-database-logical-server"></a>在 SQL Database 邏輯伺服器中連線至您的資料庫

使用 Visual Studio Code 建立對 Azure SQL Database 伺服器的連線。

> [!IMPORTANT]
> 在繼續之前，確定您已備妥伺服器、資料庫和登入資訊。 開始輸入連線設定檔資訊後，如果您的焦點變換自 Visual Studio Code，則必須重新開始建立連線設定檔。
>

1. 在 VS Code 中，按 **CTRL+SHIFT+P** (或 **F1**) 以開啟命令選擇區。

2. 輸入 **sqlcon** 並且按 **ENTER**。

3. 按 **ENTER** 以選取 [建立連線設定檔]。 這會為您的 SQL Server 執行個體建立連線設定檔。

4. 請依照提示指定新連線設定檔的連線屬性。 指定每個值之後，按 **ENTER** 繼續。 

   下表說明連線設定檔屬性。

   | 設定 | 說明 |
   |-----|-----|
   | **伺服器名稱** | 輸入您的完整伺服器名稱，例如 **mynewserver20170313.database.windows.net** |
   | **資料庫名稱** | 輸入您的資料庫名稱，例如 **mySampleDatabase** |
   | **驗證** | 選取 SQL 登入 |
   | **使用者名稱** | 輸入您的伺服器管理帳戶 |
   | **密碼 (SQL 登入)** | 輸入伺服器管理帳戶的密碼 | 
   | **儲存密碼？** | 選取 [是] 或 [否] |
   | **[選用] 輸入這個設定檔的名稱** | 輸入連線設定檔名稱，例如 **mySampleDatabase**。 

5. 按 **ESC** 鍵來關閉通知已建立並連接設定檔的資訊訊息。

6. 在狀態列中確認您的連線。

   ![連線狀態](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a>查詢資料

使用下列程式碼，可藉由使用 [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL 陳述式來依照類別查詢前 20 項產品。

1. 在 [編輯器] 視窗中，於空白查詢視窗中輸入下列查詢︰

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. 按 **CTRL+SHIFT+E** 來擷取 Product 和 ProductCategory 資料表中的資料。

    ![查詢](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a>插入資料

使用下列程式碼，藉由使用 [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL 陳述式將新產品插入 SalesLT.Product 資料表中。

1. 在 [編輯器] 視窗中，刪除先前的查詢並輸入下列查詢︰

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. 按 **CTRL+SHIFT+E** 以在 Product 資料表中插入新資料列。

## <a name="update-data"></a>更新資料

使用下列程式碼，藉由使用 [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL 陳述式更新您先前新增的產品。

1.  在 [編輯器] 視窗中，刪除先前的查詢並輸入下列查詢︰

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. 按 **CTRL+SHIFT+E** 以在 Product 資料表中更新指定的資料列。

## <a name="delete-data"></a>刪除資料

使用下列程式碼，藉由使用 [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL 陳述式刪除您先前新增的產品。

1. 在 [編輯器] 視窗中，刪除先前的查詢並輸入下列查詢︰

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. 按 **CTRL+SHIFT+E** 以在 Product 資料表中刪除指定的資料列。

## <a name="next-steps"></a>後續步驟

- 若要使用 SQL Server Management Studio 來連線和查詢，請參閱[使用 SSMS 連線及查詢](sql-database-connect-query-ssms.md)
- 若要使用 .NET 進行連線和查詢，請參閱[使用 .NET 進行連線和查詢](sql-database-connect-query-dotnet.md)。
- 若要使用 PHP 進行連線和查詢，請參閱[使用 PHP 進行連線和查詢](sql-database-connect-query-php.md)。
- 若要使用 Node.js 進行連線和查詢，請參閱[使用 Node.js 進行連線和查詢](sql-database-connect-query-nodejs.md)。
- 若要使用 Java 進行連線和查詢，請參閱[使用 Java 進行連線和查詢](sql-database-connect-query-java.md)。
- 若要使用 Python 進行連線和查詢，請參閱[使用 Python 進行連線和查詢](sql-database-connect-query-python.md)。
- 若要使用 Ruby 進行連線和查詢，請參閱[使用 Ruby 進行連線和查詢](sql-database-connect-query-ruby.md)。

