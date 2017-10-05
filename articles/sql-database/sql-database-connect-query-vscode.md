---
title: "VS Code: Ansluta och läsa data i Azure SQL Database | Microsoft Docs"
description: "Lär dig hur du ansluter till SQL Database på Azure med hjälp av Visual Studio Code. Kör sedan Transact-SQL-uttryck (T-SQL) för att skicka frågor och redigera data."
metacanonical: 
keywords: ansluta till sql database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/20/2017
ms.author: carlrab
ms.openlocfilehash: 4076b1e7ab3a70009217a1deff72da4bff0dc871
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a><span data-ttu-id="da427-105">Azure SQL Database: Använd Visual Studio Code för att ansluta och skicka frågor till data</span><span class="sxs-lookup"><span data-stu-id="da427-105">Azure SQL Database: Use Visual Studio Code to connect and query data</span></span>

<span data-ttu-id="da427-106">[Visual Studio Code](https://code.visualstudio.com/docs) är en grafisk kodredigerare för Linux, macOS och Windows som stöder tillägg, inklusive [mssql-tillägget](https://aka.ms/mssql-marketplace) för frågor till Microsoft SQL Server, Azure SQL Database och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="da427-106">[Visual Studio Code](https://code.visualstudio.com/docs) is a graphical code editor for Linux, macOS, and Windows that supports extensions, including the [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="da427-107">Den här snabbstarten visar hur du använder Visual Studio Code för att ansluta till en Azure SQL-databas och sedan använda Transact-SQL-uttryck för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="da427-107">This quick start demonstrates how to use Visual Studio Code to connect to an Azure SQL database, and then use Transact-SQL statements to query, insert, update, and delete data in the database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da427-108">Krav</span><span class="sxs-lookup"><span data-stu-id="da427-108">Prerequisites</span></span>

<span data-ttu-id="da427-109">Den här snabbstarten använder resurser som har skapats i någon av dessa snabbstarter som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="da427-109">This quick start uses as its starting point the resources created in one of these quick starts:</span></span>

- [<span data-ttu-id="da427-110">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="da427-110">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
- [<span data-ttu-id="da427-111">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="da427-111">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
- [<span data-ttu-id="da427-112">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="da427-112">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

<span data-ttu-id="da427-113">Innan du börjar bör du kontrollera att du har installerat den senaste versionen av [Visual Studio Code](https://code.visualstudio.com/Download) och har läst in [mssql-tillägget](https://aka.ms/mssql-marketplace).</span><span class="sxs-lookup"><span data-stu-id="da427-113">Before you start, make sure you have installed the newest version of [Visual Studio Code](https://code.visualstudio.com/Download) and loaded the [mssql extension](https://aka.ms/mssql-marketplace).</span></span> <span data-ttu-id="da427-114">Installationsanvisningar finns i [Installera VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) för mssql-tillägget och [mssql för Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).</span><span class="sxs-lookup"><span data-stu-id="da427-114">For installation guidance for the mssql extension, see [Install VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) and see [mssql for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).</span></span> 

## <a name="configure-vs-code"></a><span data-ttu-id="da427-115">Konfigurera VS-kod</span><span class="sxs-lookup"><span data-stu-id="da427-115">Configure VS Code</span></span> 

### <a name="mac-os"></a><span data-ttu-id="da427-116">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="da427-116">**Mac OS**</span></span>
<span data-ttu-id="da427-117">För macOS måste du installera OpenSSL, som är ett förhandskrav för den DotNet Core som används i mssql-tillägget.</span><span class="sxs-lookup"><span data-stu-id="da427-117">For macOS, you need to install OpenSSL which is a prerequiste for DotNet Core that mssql extention uses.</span></span> <span data-ttu-id="da427-118">Ange följande kommandon för att installera **brew** och **OpenSSL**.</span><span class="sxs-lookup"><span data-stu-id="da427-118">Open your terminal and enter the following commands to install **brew** and **OpenSSL**.</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a><span data-ttu-id="da427-119">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="da427-119">**Linux (Ubuntu)**</span></span>

<span data-ttu-id="da427-120">Ingen särskild konfiguration behövs.</span><span class="sxs-lookup"><span data-stu-id="da427-120">No special configuration needed.</span></span>

### <a name="windows"></a><span data-ttu-id="da427-121">**Windows**</span><span class="sxs-lookup"><span data-stu-id="da427-121">**Windows**</span></span>

<span data-ttu-id="da427-122">Ingen särskild konfiguration behövs.</span><span class="sxs-lookup"><span data-stu-id="da427-122">No special configuration needed.</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="da427-123">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="da427-123">SQL server connection information</span></span>

<span data-ttu-id="da427-124">Skaffa den anslutningsinformation du behöver för att ansluta till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="da427-124">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="da427-125">Du behöver det fullständiga servernamnet, databasnamnet och inloggningsinformationen i nästa procedurer.</span><span class="sxs-lookup"><span data-stu-id="da427-125">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="da427-126">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="da427-126">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="da427-127">Välj **SQL-databaser** på den vänstra menyn och klicka på databasen på sidan **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="da427-127">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="da427-128">Granska serverns fullständiga namn på sidan **Översikt** för databasen, se bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="da427-128">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="da427-129">Om du hovrar över servernamnet visas alternativet **Kopiera genom att klicka**.</span><span class="sxs-lookup"><span data-stu-id="da427-129">You can hover over the server name to bring up the **Click to copy** option.</span></span>

   ![anslutningsinformation](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="da427-131">Om du har glömt inloggningsinformationen för Azure SQL Database-server öppnar du serversidan i SQL Database. Där ser du administratörsnamnet för servern och kan återställa lösenordet vid behov.</span><span class="sxs-lookup"><span data-stu-id="da427-131">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span> 

## <a name="set-language-mode-to-sql"></a><span data-ttu-id="da427-132">Ange språkläge till SQL</span><span class="sxs-lookup"><span data-stu-id="da427-132">Set language mode to SQL</span></span>

<span data-ttu-id="da427-133">Ställ in språkläget på **SQL** i Visual Studio Code för att aktivera mssql-kommandon och T-SQL IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="da427-133">Set the language mode is set to **SQL** in Visual Studio Code to enable mssql commands and T-SQL IntelliSense.</span></span>

1. <span data-ttu-id="da427-134">Öppna ett Visual Studio Code-fönster.</span><span class="sxs-lookup"><span data-stu-id="da427-134">Open a new Visual Studio Code window.</span></span> 

2. <span data-ttu-id="da427-135">Klicka på **Oformaterad text** nere till höger i statusfältet.</span><span class="sxs-lookup"><span data-stu-id="da427-135">Click **Plain Text** in the lower right-hand corner of the status bar.</span></span>
3. <span data-ttu-id="da427-136">Listrutan **Select language mode** (Välj språkläge) öppnas. Skriv **SQL**, tryck på **ENTER** och ange språkläge för SQL.</span><span class="sxs-lookup"><span data-stu-id="da427-136">In the **Select language mode** drop-down menu that opens, type **SQL**, and then press **ENTER** to set the language mode to SQL.</span></span> 

   ![Språkläge för SQL](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-to-your-database"></a><span data-ttu-id="da427-138">Ansluta till databasen</span><span class="sxs-lookup"><span data-stu-id="da427-138">Connect to your database</span></span>

<span data-ttu-id="da427-139">Använd Visual Studio Code för att upprätta en anslutning till Azure SQL Database-servern.</span><span class="sxs-lookup"><span data-stu-id="da427-139">Use Visual Studio Code to establish a connection to your Azure SQL Database server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da427-140">Kontrollera att du har din server, databas och inloggningsinformation redo innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="da427-140">Before continuing, make sure that you have your server, database, and login information ready.</span></span> <span data-ttu-id="da427-141">Om du ändrar fokus från Visual Studio-koden när du har börjat ange information om anslutningsprofilen måste du börja om med att skapa anslutningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="da427-141">Once you begin entering the connection profile information, if you change your focus from Visual Studio Code, you have to restart creating the connection profile.</span></span>
>

1. <span data-ttu-id="da427-142">I VS Code trycker du på **CTRL+SHIFT+P** (eller **F1**) för att öppna kommandopaletten.</span><span class="sxs-lookup"><span data-stu-id="da427-142">In VS Code, press **CTRL+SHIFT+P** (or **F1**) to open the Command Palette.</span></span>

2. <span data-ttu-id="da427-143">Skriv in **sqlcon** och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="da427-143">Type **sqlcon** and press **ENTER**.</span></span>

3. <span data-ttu-id="da427-144">Tryck på **RETUR** för att välja **Create Connection Profile** (Skapa anslutningsprofil).</span><span class="sxs-lookup"><span data-stu-id="da427-144">Press **ENTER** to select **Create Connection Profile**.</span></span> <span data-ttu-id="da427-145">Detta skapar en anslutningsprofil för SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="da427-145">This creates a connection profile for your SQL Server instance.</span></span>

4. <span data-ttu-id="da427-146">Följ anvisningarna för att ange anslutningsegenskaper för den nya anslutningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="da427-146">Follow the prompts to specify the connection properties for the new connection profile.</span></span> <span data-ttu-id="da427-147">När du har angett ett värde trycker du på **RETUR** för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="da427-147">After specifying each value, press **ENTER** to continue.</span></span> 

   | <span data-ttu-id="da427-148">Inställning</span><span class="sxs-lookup"><span data-stu-id="da427-148">Setting</span></span>       | <span data-ttu-id="da427-149">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="da427-149">Suggested value</span></span> | <span data-ttu-id="da427-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="da427-150">Description</span></span> |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="da427-151">**Servernamn</span><span class="sxs-lookup"><span data-stu-id="da427-151">**Server name</span></span> | <span data-ttu-id="da427-152">Fullständigt kvalificerat servernamn</span><span class="sxs-lookup"><span data-stu-id="da427-152">The fully qualified server name</span></span> | <span data-ttu-id="da427-153">Namnet ska vara ungefär så här: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="da427-153">The name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="da427-154">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="da427-154">**Database name**</span></span> | <span data-ttu-id="da427-155">mySampleDatabase</span><span class="sxs-lookup"><span data-stu-id="da427-155">mySampleDatabase</span></span> | <span data-ttu-id="da427-156">Namnet på databasen som användaren ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="da427-156">The name of the database to which to connect.</span></span> |
   | <span data-ttu-id="da427-157">**Autentisering**</span><span class="sxs-lookup"><span data-stu-id="da427-157">**Authentication**</span></span> | <span data-ttu-id="da427-158">SQL-inloggning</span><span class="sxs-lookup"><span data-stu-id="da427-158">SQL Login</span></span>| <span data-ttu-id="da427-159">SQL-autentisering är den enda autentiseringstypen som vi har konfigurerat i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="da427-159">SQL Authentication is the only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="da427-160">**Användarnamn**</span><span class="sxs-lookup"><span data-stu-id="da427-160">**User name**</span></span> | <span data-ttu-id="da427-161">Serveradministratörskontot</span><span class="sxs-lookup"><span data-stu-id="da427-161">The server admin account</span></span> | <span data-ttu-id="da427-162">Detta är det konto som du angav när du skapade servern.</span><span class="sxs-lookup"><span data-stu-id="da427-162">This is the account that you specified when you created the server.</span></span> |
   | <span data-ttu-id="da427-163">**Lösenord (SQL-inloggning)**</span><span class="sxs-lookup"><span data-stu-id="da427-163">**Password (SQL Login)**</span></span> | <span data-ttu-id="da427-164">Lösenordet för serveradministratörskontot</span><span class="sxs-lookup"><span data-stu-id="da427-164">The password for your server admin account</span></span> | <span data-ttu-id="da427-165">Detta är det lösenord som du angav när du skapade servern.</span><span class="sxs-lookup"><span data-stu-id="da427-165">This is the password that you specified when you created the server.</span></span> |
   | <span data-ttu-id="da427-166">**Spara lösenordet?**</span><span class="sxs-lookup"><span data-stu-id="da427-166">**Save Password?**</span></span> | <span data-ttu-id="da427-167">Ja eller nej</span><span class="sxs-lookup"><span data-stu-id="da427-167">Yes or No</span></span> | <span data-ttu-id="da427-168">Välj Ja om du inte vill ange lösenordet varje gång.</span><span class="sxs-lookup"><span data-stu-id="da427-168">Select Yes if you do not want to enter the password each time.</span></span> |
   | <span data-ttu-id="da427-169">**Ange ett namn för den här profilen**</span><span class="sxs-lookup"><span data-stu-id="da427-169">**Enter a name for this profile**</span></span> | <span data-ttu-id="da427-170">Ett anslutningsprofilnamn, t.ex. **mySampleDatabase**</span><span class="sxs-lookup"><span data-stu-id="da427-170">A profile name, such as **mySampleDatabase**</span></span> | <span data-ttu-id="da427-171">Ett sparat profilnamn förbättrar anslutningen på efterföljande inloggningar.</span><span class="sxs-lookup"><span data-stu-id="da427-171">A saved profile name speeds your connection on subsequent logins.</span></span> | 

5. <span data-ttu-id="da427-172">Tryck på tangenten **ESC** för att stänga meddelandet som informerar om att profilen har skapats och anslutits.</span><span class="sxs-lookup"><span data-stu-id="da427-172">Press the **ESC** key to close the info message that informs you that the profile is created and connected.</span></span>

6. <span data-ttu-id="da427-173">Kontrollera anslutningen i statusfältet.</span><span class="sxs-lookup"><span data-stu-id="da427-173">Verify your connection in the status bar.</span></span>

   ![Anslutningsstatus](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a><span data-ttu-id="da427-175">Frågedata</span><span class="sxs-lookup"><span data-stu-id="da427-175">Query data</span></span>

<span data-ttu-id="da427-176">Använd följande kod för att söka efter de 20 främsta produkterna med Transact-SQL-instruktionen [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).</span><span class="sxs-lookup"><span data-stu-id="da427-176">Use the following code to query for the top 20 products by category using the [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="da427-177">I fönstret **Editor** anger du följande fråga i frågefönstret:</span><span class="sxs-lookup"><span data-stu-id="da427-177">In the **Editor** window, enter the following query in the empty query window:</span></span>

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. <span data-ttu-id="da427-178">Tryck på **CTRL+SKIFT+E** att hämta data från Product- och ProductCategory-tabeller.</span><span class="sxs-lookup"><span data-stu-id="da427-178">Press **CTRL+SHIFT+E** to retrieve data from the Product and ProductCategory tables.</span></span>

    ![Fråga](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a><span data-ttu-id="da427-180">Infoga data</span><span class="sxs-lookup"><span data-stu-id="da427-180">Insert data</span></span>

<span data-ttu-id="da427-181">Använd följande kod för att infoga en ny produkt i tabellen SalesLT.Product med Transact-SQL-instruktionen [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).</span><span class="sxs-lookup"><span data-stu-id="da427-181">Use the following code to insert a new product into the SalesLT.Product table using the [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="da427-182">I fönstret **Editor** tar du bort föregående fråga och skriver in följande fråga:</span><span class="sxs-lookup"><span data-stu-id="da427-182">In the **Editor** window, delete the previous query and enter the following query:</span></span>

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

2. <span data-ttu-id="da427-183">Tryck på **CTRL+SKIFT+E** för att infoga en ny rad i Product-tabellen.</span><span class="sxs-lookup"><span data-stu-id="da427-183">Press **CTRL+SHIFT+E** to insert a new row in the Product table.</span></span>

## <a name="update-data"></a><span data-ttu-id="da427-184">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="da427-184">Update data</span></span>

<span data-ttu-id="da427-185">Med följande kod uppdaterar du den nya produkt du tidigare lade till med Transact-SQL-instruktionen [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).</span><span class="sxs-lookup"><span data-stu-id="da427-185">Use the following code to update the new product that you previously added using the [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL statement.</span></span>

1.  <span data-ttu-id="da427-186">I fönstret **Editor** tar du bort föregående fråga och skriver in följande fråga:</span><span class="sxs-lookup"><span data-stu-id="da427-186">In the **Editor** window, delete the previous query and enter the following query:</span></span>

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="da427-187">Tryck på **CTRL+SKIFT+E** för att uppdatera angiven rad i Product-tabellen.</span><span class="sxs-lookup"><span data-stu-id="da427-187">Press **CTRL+SHIFT+E** to update the specified row in the Product table.</span></span>

## <a name="delete-data"></a><span data-ttu-id="da427-188">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="da427-188">Delete data</span></span>

<span data-ttu-id="da427-189">Med följande kod tar du bort den nya produkt du tidigare lade till med Transact-SQL-instruktionen [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).</span><span class="sxs-lookup"><span data-stu-id="da427-189">Use the following code to delete the new product that you previously added using the [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="da427-190">I fönstret **Editor** tar du bort föregående fråga och skriver in följande fråga:</span><span class="sxs-lookup"><span data-stu-id="da427-190">In the **Editor** window, delete the previous query and enter the following query:</span></span>

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="da427-191">Tryck på **CTRL+SHIFT+E** för att ta bort angiven rad i Product-tabellen.</span><span class="sxs-lookup"><span data-stu-id="da427-191">Press **CTRL+SHIFT+E** to delete the specified row in the Product table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da427-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da427-192">Next steps</span></span>

- <span data-ttu-id="da427-193">Om du vill ansluta och fråga med SQL Server Management Studio kan du läsa [Anslut och fråga med SSMS](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="da427-193">To connect and query using SQL Server Management Studio, see [Connect and query with SSMS](sql-database-connect-query-ssms.md).</span></span>
- <span data-ttu-id="da427-194">En artikel från MSDN-magazine om hur du använder Visual Studio Code finns i [Skapa en IDE-databas med MSSQL-tillägget blogginlägg](https://msdn.microsoft.com/magazine/mt809115).</span><span class="sxs-lookup"><span data-stu-id="da427-194">For an MSDN magazine article on using Visual Studio Code, see [Create a database IDE with MSSQL extension blog post](https://msdn.microsoft.com/magazine/mt809115).</span></span>
