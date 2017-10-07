---
title: "VS Code: Ansluta och läsa data i Azure SQL Database | Microsoft Docs"
description: "Lär dig hur tooconnect tooSQL databas på Azure med hjälp av Visual Studio-koden. Kör sedan Transact-SQL (T-SQL)-uttryck tooquery och redigera data."
metacanonical: 
keywords: ansluta toosql databas
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
ms.openlocfilehash: ed8bdbfc3271b463a21cde5ff6b5f05fd0ff51d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-use-visual-studio-code-tooconnect-and-query-data"></a><span data-ttu-id="753e6-105">Azure SQL Database: Använd Visual Studio Code tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="753e6-105">Azure SQL Database: Use Visual Studio Code tooconnect and query data</span></span>

<span data-ttu-id="753e6-106">[Visual Studio Code](https://code.visualstudio.com/docs) är en grafiska redigerare för macOS, Linux och Windows som stöder tillägg, inklusive hello [mssql tillägget](https://aka.ms/mssql-marketplace) för frågor till Microsoft SQL Server, Azure SQL Database och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="753e6-106">[Visual Studio Code](https://code.visualstudio.com/docs) is a graphical code editor for Linux, macOS, and Windows that supports extensions, including hello [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="753e6-107">Den här snabbstartsguide visar hur toouse Visual Studio Code tooconnect tooan Azure SQL database och sedan använda Transact-SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="753e6-107">This quick start demonstrates how toouse Visual Studio Code tooconnect tooan Azure SQL database, and then use Transact-SQL statements tooquery, insert, update, and delete data in hello database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="753e6-108">Krav</span><span class="sxs-lookup"><span data-stu-id="753e6-108">Prerequisites</span></span>

<span data-ttu-id="753e6-109">Den här snabbstartsguide används som första plats hello resurserna skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="753e6-109">This quick start uses as its starting point hello resources created in one of these quick starts:</span></span>

- [<span data-ttu-id="753e6-110">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="753e6-110">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
- [<span data-ttu-id="753e6-111">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="753e6-111">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
- [<span data-ttu-id="753e6-112">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="753e6-112">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

<span data-ttu-id="753e6-113">Innan du börjar, kontrollera att du har installerat hello senaste versionen av [Visual Studio Code](https://code.visualstudio.com/Download) och läsa in hello [mssql tillägget](https://aka.ms/mssql-marketplace).</span><span class="sxs-lookup"><span data-stu-id="753e6-113">Before you start, make sure you have installed hello newest version of [Visual Studio Code](https://code.visualstudio.com/Download) and loaded hello [mssql extension](https://aka.ms/mssql-marketplace).</span></span> <span data-ttu-id="753e6-114">Installationen vägledning för hello mssql tillägg finns [installera VS kod](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) och se [mssql för Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).</span><span class="sxs-lookup"><span data-stu-id="753e6-114">For installation guidance for hello mssql extension, see [Install VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) and see [mssql for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).</span></span> 

## <a name="configure-vs-code"></a><span data-ttu-id="753e6-115">Konfigurera VS-kod</span><span class="sxs-lookup"><span data-stu-id="753e6-115">Configure VS Code</span></span> 

### <a name="mac-os"></a><span data-ttu-id="753e6-116">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="753e6-116">**Mac OS**</span></span>
<span data-ttu-id="753e6-117">För macOS behöver du tooinstall OpenSSL som är en prerequiste för DotNet Core som mssql-tillägget används.</span><span class="sxs-lookup"><span data-stu-id="753e6-117">For macOS, you need tooinstall OpenSSL which is a prerequiste for DotNet Core that mssql extention uses.</span></span> <span data-ttu-id="753e6-118">Öppna terminalen och ange följande kommandon tooinstall hello **brew** och **OpenSSL**.</span><span class="sxs-lookup"><span data-stu-id="753e6-118">Open your terminal and enter hello following commands tooinstall **brew** and **OpenSSL**.</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a><span data-ttu-id="753e6-119">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="753e6-119">**Linux (Ubuntu)**</span></span>

<span data-ttu-id="753e6-120">Ingen särskild konfiguration behövs.</span><span class="sxs-lookup"><span data-stu-id="753e6-120">No special configuration needed.</span></span>

### <a name="windows"></a><span data-ttu-id="753e6-121">**Windows**</span><span class="sxs-lookup"><span data-stu-id="753e6-121">**Windows**</span></span>

<span data-ttu-id="753e6-122">Ingen särskild konfiguration behövs.</span><span class="sxs-lookup"><span data-stu-id="753e6-122">No special configuration needed.</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="753e6-123">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="753e6-123">SQL server connection information</span></span>

<span data-ttu-id="753e6-124">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="753e6-124">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="753e6-125">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="753e6-125">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="753e6-126">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="753e6-126">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="753e6-127">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="753e6-127">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="753e6-128">På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="753e6-128">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="753e6-129">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="753e6-129">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>

   ![anslutningsinformation](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="753e6-131">Om du har glömt hello inloggningsinformation för din Azure SQL Database-server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="753e6-131">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span> 

## <a name="set-language-mode-toosql"></a><span data-ttu-id="753e6-132">Ange språk läge tooSQL</span><span class="sxs-lookup"><span data-stu-id="753e6-132">Set language mode tooSQL</span></span>

<span data-ttu-id="753e6-133">Ange hello språk läge har angetts för**SQL** i Visual Studio Code tooenable mssql kommandon och T-SQL IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="753e6-133">Set hello language mode is set too**SQL** in Visual Studio Code tooenable mssql commands and T-SQL IntelliSense.</span></span>

1. <span data-ttu-id="753e6-134">Öppna ett Visual Studio Code-fönster.</span><span class="sxs-lookup"><span data-stu-id="753e6-134">Open a new Visual Studio Code window.</span></span> 

2. <span data-ttu-id="753e6-135">Klicka på **oformaterad Text** i hello nedre högra hörnet i statusfältet för hello.</span><span class="sxs-lookup"><span data-stu-id="753e6-135">Click **Plain Text** in hello lower right-hand corner of hello status bar.</span></span>
3. <span data-ttu-id="753e6-136">I hello **språk läge** nedrullningsbara menyn som öppnas, skriver **SQL**, och tryck sedan på **RETUR** tooset hello språk läge tooSQL.</span><span class="sxs-lookup"><span data-stu-id="753e6-136">In hello **Select language mode** drop-down menu that opens, type **SQL**, and then press **ENTER** tooset hello language mode tooSQL.</span></span> 

   ![Språkläge för SQL](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-tooyour-database"></a><span data-ttu-id="753e6-138">Ansluta tooyour databas</span><span class="sxs-lookup"><span data-stu-id="753e6-138">Connect tooyour database</span></span>

<span data-ttu-id="753e6-139">Använd Visual Studio Code tooestablish en anslutning tooyour Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="753e6-139">Use Visual Studio Code tooestablish a connection tooyour Azure SQL Database server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="753e6-140">Kontrollera att du har din server, databas och inloggningsinformation redo innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="753e6-140">Before continuing, make sure that you have your server, database, and login information ready.</span></span> <span data-ttu-id="753e6-141">När du börjar att ange hello anslutningsinformation till profilen, om du ändrar ditt fokus från Visual Studio Code, har toorestart skapar hello-anslutningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="753e6-141">Once you begin entering hello connection profile information, if you change your focus from Visual Studio Code, you have toorestart creating hello connection profile.</span></span>
>

1. <span data-ttu-id="753e6-142">VS-koden trycker du på **CTRL + SKIFT + P** (eller **F1**) tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="753e6-142">In VS Code, press **CTRL+SHIFT+P** (or **F1**) tooopen hello Command Palette.</span></span>

2. <span data-ttu-id="753e6-143">Skriv in **sqlcon** och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="753e6-143">Type **sqlcon** and press **ENTER**.</span></span>

3. <span data-ttu-id="753e6-144">Tryck på **RETUR** tooselect **skapa anslutningsprofilen**.</span><span class="sxs-lookup"><span data-stu-id="753e6-144">Press **ENTER** tooselect **Create Connection Profile**.</span></span> <span data-ttu-id="753e6-145">Detta skapar en anslutningsprofil för SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="753e6-145">This creates a connection profile for your SQL Server instance.</span></span>

4. <span data-ttu-id="753e6-146">Följ anslutningsegenskaper för hello prompter toospecify hello för nya hello-anslutningsprofilen.</span><span class="sxs-lookup"><span data-stu-id="753e6-146">Follow hello prompts toospecify hello connection properties for hello new connection profile.</span></span> <span data-ttu-id="753e6-147">När du har angett varje värde trycker du på **RETUR** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="753e6-147">After specifying each value, press **ENTER** toocontinue.</span></span> 

   | <span data-ttu-id="753e6-148">Inställning</span><span class="sxs-lookup"><span data-stu-id="753e6-148">Setting</span></span>       | <span data-ttu-id="753e6-149">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="753e6-149">Suggested value</span></span> | <span data-ttu-id="753e6-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="753e6-150">Description</span></span> |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="753e6-151">**Servernamn</span><span class="sxs-lookup"><span data-stu-id="753e6-151">**Server name</span></span> | <span data-ttu-id="753e6-152">hello fullständigt kvalificerade servernamnet</span><span class="sxs-lookup"><span data-stu-id="753e6-152">hello fully qualified server name</span></span> | <span data-ttu-id="753e6-153">hello namnet ska vara ungefär så här: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="753e6-153">hello name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="753e6-154">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="753e6-154">**Database name**</span></span> | <span data-ttu-id="753e6-155">mySampleDatabase</span><span class="sxs-lookup"><span data-stu-id="753e6-155">mySampleDatabase</span></span> | <span data-ttu-id="753e6-156">hello namnet på hello databasen toowhich tooconnect.</span><span class="sxs-lookup"><span data-stu-id="753e6-156">hello name of hello database toowhich tooconnect.</span></span> |
   | <span data-ttu-id="753e6-157">**Autentisering**</span><span class="sxs-lookup"><span data-stu-id="753e6-157">**Authentication**</span></span> | <span data-ttu-id="753e6-158">SQL-inloggning</span><span class="sxs-lookup"><span data-stu-id="753e6-158">SQL Login</span></span>| <span data-ttu-id="753e6-159">SQL-autentisering är hello endast autentiseringstyp som vi har konfigurerat i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="753e6-159">SQL Authentication is hello only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="753e6-160">**Användarnamn**</span><span class="sxs-lookup"><span data-stu-id="753e6-160">**User name**</span></span> | <span data-ttu-id="753e6-161">Hej server-administratörskontot</span><span class="sxs-lookup"><span data-stu-id="753e6-161">hello server admin account</span></span> | <span data-ttu-id="753e6-162">Detta är hello-konto som du angav när du skapade hello-server.</span><span class="sxs-lookup"><span data-stu-id="753e6-162">This is hello account that you specified when you created hello server.</span></span> |
   | <span data-ttu-id="753e6-163">**Lösenord (SQL-inloggning)**</span><span class="sxs-lookup"><span data-stu-id="753e6-163">**Password (SQL Login)**</span></span> | <span data-ttu-id="753e6-164">hello lösenord för administratörskontot server</span><span class="sxs-lookup"><span data-stu-id="753e6-164">hello password for your server admin account</span></span> | <span data-ttu-id="753e6-165">Detta är hello lösenord som du angav när du skapade hello-server.</span><span class="sxs-lookup"><span data-stu-id="753e6-165">This is hello password that you specified when you created hello server.</span></span> |
   | <span data-ttu-id="753e6-166">**Spara lösenordet?**</span><span class="sxs-lookup"><span data-stu-id="753e6-166">**Save Password?**</span></span> | <span data-ttu-id="753e6-167">Ja eller nej</span><span class="sxs-lookup"><span data-stu-id="753e6-167">Yes or No</span></span> | <span data-ttu-id="753e6-168">Välj Ja om du inte vill att tooenter hello lösenord varje gång.</span><span class="sxs-lookup"><span data-stu-id="753e6-168">Select Yes if you do not want tooenter hello password each time.</span></span> |
   | <span data-ttu-id="753e6-169">**Ange ett namn för den här profilen**</span><span class="sxs-lookup"><span data-stu-id="753e6-169">**Enter a name for this profile**</span></span> | <span data-ttu-id="753e6-170">Ett anslutningsprofilnamn, t.ex. **mySampleDatabase**</span><span class="sxs-lookup"><span data-stu-id="753e6-170">A profile name, such as **mySampleDatabase**</span></span> | <span data-ttu-id="753e6-171">Ett sparat profilnamn förbättrar anslutningen på efterföljande inloggningar.</span><span class="sxs-lookup"><span data-stu-id="753e6-171">A saved profile name speeds your connection on subsequent logins.</span></span> | 

5. <span data-ttu-id="753e6-172">Tryck på hello **ESC** viktiga tooclose info hälsningsmeddelande som informerar dig om att hello profil skapas och ansluten.</span><span class="sxs-lookup"><span data-stu-id="753e6-172">Press hello **ESC** key tooclose hello info message that informs you that hello profile is created and connected.</span></span>

6. <span data-ttu-id="753e6-173">Kontrollera din anslutning i hello statusfältet.</span><span class="sxs-lookup"><span data-stu-id="753e6-173">Verify your connection in hello status bar.</span></span>

   ![Anslutningsstatus](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a><span data-ttu-id="753e6-175">Frågedata</span><span class="sxs-lookup"><span data-stu-id="753e6-175">Query data</span></span>

<span data-ttu-id="753e6-176">Använd hello följande kod tooquery för hello de 20 största produkter efter kategori med hello [Välj](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="753e6-176">Use hello following code tooquery for hello top 20 products by category using hello [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="753e6-177">I hello **Editor** fönstret Ange hello följande fråga i hello tom frågefönstret:</span><span class="sxs-lookup"><span data-stu-id="753e6-177">In hello **Editor** window, enter hello following query in hello empty query window:</span></span>

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. <span data-ttu-id="753e6-178">Tryck på **CTRL + SKIFT + E** tooretrieve data från hello produkt och produktkategori.</span><span class="sxs-lookup"><span data-stu-id="753e6-178">Press **CTRL+SHIFT+E** tooretrieve data from hello Product and ProductCategory tables.</span></span>

    ![Fråga](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a><span data-ttu-id="753e6-180">Infoga data</span><span class="sxs-lookup"><span data-stu-id="753e6-180">Insert data</span></span>

<span data-ttu-id="753e6-181">Använd hello följande kod tooinsert en ny produkt i hello SalesLT.Product tabellen med hjälp av hello [infoga](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="753e6-181">Use hello following code tooinsert a new product into hello SalesLT.Product table using hello [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="753e6-182">I hello **Editor** , ta bort hello föregående fråga och ange hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="753e6-182">In hello **Editor** window, delete hello previous query and enter hello following query:</span></span>

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

2. <span data-ttu-id="753e6-183">Tryck på **CTRL + SKIFT + E** tooinsert en ny rad i tabellen för hello-produkten.</span><span class="sxs-lookup"><span data-stu-id="753e6-183">Press **CTRL+SHIFT+E** tooinsert a new row in hello Product table.</span></span>

## <a name="update-data"></a><span data-ttu-id="753e6-184">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="753e6-184">Update data</span></span>

<span data-ttu-id="753e6-185">Använd hello följande kod tooupdate hello ny produkt som du tidigare har lagts till med hello [uppdatering](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="753e6-185">Use hello following code tooupdate hello new product that you previously added using hello [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL statement.</span></span>

1.  <span data-ttu-id="753e6-186">I hello **Editor** , ta bort hello föregående fråga och ange hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="753e6-186">In hello **Editor** window, delete hello previous query and enter hello following query:</span></span>

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="753e6-187">Tryck på **CTRL + SKIFT + E** tooupdate hello angivna raden i tabellen för hello-produkten.</span><span class="sxs-lookup"><span data-stu-id="753e6-187">Press **CTRL+SHIFT+E** tooupdate hello specified row in hello Product table.</span></span>

## <a name="delete-data"></a><span data-ttu-id="753e6-188">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="753e6-188">Delete data</span></span>

<span data-ttu-id="753e6-189">Använd hello följande kod toodelete hello ny produkt som du tidigare har lagts till med hello [ta bort](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="753e6-189">Use hello following code toodelete hello new product that you previously added using hello [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="753e6-190">I hello **Editor** , ta bort hello föregående fråga och ange hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="753e6-190">In hello **Editor** window, delete hello previous query and enter hello following query:</span></span>

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="753e6-191">Tryck på **CTRL + SKIFT + E** toodelete hello angivna raden i tabellen för hello-produkten.</span><span class="sxs-lookup"><span data-stu-id="753e6-191">Press **CTRL+SHIFT+E** toodelete hello specified row in hello Product table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="753e6-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="753e6-192">Next steps</span></span>

- <span data-ttu-id="753e6-193">tooconnect och fråga med SQL Server Management Studio finns [Anslut och fråga med SSMS](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="753e6-193">tooconnect and query using SQL Server Management Studio, see [Connect and query with SSMS](sql-database-connect-query-ssms.md).</span></span>
- <span data-ttu-id="753e6-194">En artikel från MSDN-magazine om hur du använder Visual Studio Code finns i [Skapa en IDE-databas med MSSQL-tillägget blogginlägg](https://msdn.microsoft.com/magazine/mt809115).</span><span class="sxs-lookup"><span data-stu-id="753e6-194">For an MSDN magazine article on using Visual Studio Code, see [Create a database IDE with MSSQL extension blog post](https://msdn.microsoft.com/magazine/mt809115).</span></span>
