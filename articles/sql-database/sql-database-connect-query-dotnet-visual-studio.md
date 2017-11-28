---
title: aaaUse och Visual Studio .NET tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Visual Studio toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 038cfb9c680217dfeea5a9996a0abed88cc80559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-c-with-visual-studio-tooconnect-and-query-an-azure-sql-database"></a><span data-ttu-id="0c311-103">Använd .NET (C#) med Visual Studio tooconnect och frågar en Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="0c311-103">Use .NET (C#) with Visual Studio tooconnect and query an Azure SQL database</span></span>

<span data-ttu-id="0c311-104">Den här snabbstartsguide visar hur toouse hello [.NET framework](https://www.microsoft.com/net/) toocreate C#-program med Visual Studio tooconnect tooan Azure SQL database och använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="0c311-104">This quick start tutorial demonstrates how toouse hello [.NET framework](https://www.microsoft.com/net/) toocreate a C# program with Visual Studio tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c311-105">Krav</span><span class="sxs-lookup"><span data-stu-id="0c311-105">Prerequisites</span></span>

<span data-ttu-id="0c311-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c311-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="0c311-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0c311-107">An Azure SQL database.</span></span> <span data-ttu-id="0c311-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="0c311-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="0c311-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="0c311-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="0c311-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="0c311-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="0c311-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c311-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="0c311-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="0c311-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="0c311-113">En installation av [Visual Studio Community 2017, Visual Studio Professional 2017 eller Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0c311-113">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="0c311-114">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="0c311-114">SQL server connection information</span></span>

<span data-ttu-id="0c311-115">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0c311-115">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="0c311-116">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="0c311-116">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="0c311-117">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0c311-117">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0c311-118">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="0c311-118">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="0c311-119">På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="0c311-119">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="0c311-120">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="0c311-120">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="0c311-122">Om du glömmer dina inloggningsuppgifter för Azure SQL Database-server kan du navigera toohello SQL server sidan tooview hello admin Databasservernamnet.</span><span class="sxs-lookup"><span data-stu-id="0c311-122">If you forget your Azure SQL Database server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span> <span data-ttu-id="0c311-123">Du kan återställa hello lösenord om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0c311-123">You can reset hello password if necessary.</span></span>

5. <span data-ttu-id="0c311-124">Klicka på **Visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="0c311-124">Click **Show database connection strings**.</span></span>

6. <span data-ttu-id="0c311-125">Granska hello fullständig **ADO.NET** anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="0c311-125">Review hello complete **ADO.NET** connection string.</span></span>

    ![ADO.NET-anslutningssträng](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> <span data-ttu-id="0c311-127">Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0c311-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="0c311-128">Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="0c311-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 
>
  
## <a name="create-a-new-visual-studio-project"></a><span data-ttu-id="0c311-129">Skapa ett nytt Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="0c311-129">Create a new Visual Studio project</span></span>

1. <span data-ttu-id="0c311-130">I Visual Studio väljer du **Arkiv**, **Nytt** och **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0c311-130">In Visual Studio, choose **File**, **New**, **Project**.</span></span> 
2. <span data-ttu-id="0c311-131">I hello **nytt projekt** dialogrutan och visa **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="0c311-131">In hello **New Project** dialog, and expand **Visual C#**.</span></span>
3. <span data-ttu-id="0c311-132">Välj **Konsolapp** och ange *sqltest* hello projektnamn.</span><span class="sxs-lookup"><span data-stu-id="0c311-132">Select **Console App** and enter *sqltest* for hello project name.</span></span>
4. <span data-ttu-id="0c311-133">Klicka på **OK** toocreate och öppna hello nytt projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c311-133">Click **OK** toocreate and open hello new project in Visual Studio</span></span>
4. <span data-ttu-id="0c311-134">Högerklicka på **sqltest** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="0c311-134">In Solution Explorer, right-click **sqltest** and click **Manage NuGet Packages**.</span></span> 
5. <span data-ttu-id="0c311-135">På hello **Bläddra**, söka efter ```System.Data.SqlClient``` och, när hitta, välja den.</span><span class="sxs-lookup"><span data-stu-id="0c311-135">On hello **Browse**, search for ```System.Data.SqlClient``` and, when found, select it.</span></span>
6. <span data-ttu-id="0c311-136">I hello **System.Data.SqlClient** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="0c311-136">In hello **System.Data.SqlClient** page, click **Install**.</span></span>
7. <span data-ttu-id="0c311-137">När hello-installationen har slutförts, granska hello ändringar och klicka sedan på **OK** tooclose hello **Preview** fönster.</span><span class="sxs-lookup"><span data-stu-id="0c311-137">When hello install completes, review hello changes and then click **OK** tooclose hello **Preview** window.</span></span> 
8. <span data-ttu-id="0c311-138">Om ett fönster för **Godkännande av licens** visas klickar du på **Jag accepterar**.</span><span class="sxs-lookup"><span data-stu-id="0c311-138">If a **License Acceptance** window appears, click **I Accept**.</span></span>

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="0c311-139">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="0c311-139">Insert code tooquery SQL database</span></span>
1. <span data-ttu-id="0c311-140">Växla för (eller öppna vid behov) **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="0c311-140">Switch too(or open if necessary) **Program.cs**</span></span>

2. <span data-ttu-id="0c311-141">Ersätt hello innehållet i **Program.cs** med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="0c311-141">Replace hello contents of **Program.cs** with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
            Console.ReadLine();
        }
    }
}
```

## <a name="run-hello-code"></a><span data-ttu-id="0c311-142">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="0c311-142">Run hello code</span></span>

1. <span data-ttu-id="0c311-143">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="0c311-143">Press **F5** toorun hello application.</span></span>
2. <span data-ttu-id="0c311-144">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="0c311-144">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c311-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c311-145">Next steps</span></span>

- <span data-ttu-id="0c311-146">Lär dig hur för[Anslut och fråga en Azure SQL database med .NET core](sql-database-connect-query-dotnet-core.md) på macOS/Windows/Linux.</span><span class="sxs-lookup"><span data-stu-id="0c311-146">Learn how too[connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="0c311-147">Lär dig mer om [komma igång med .NET Core för Windows/Linux/macOS hello kommandoraden](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="0c311-147">Learn about [Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="0c311-148">Lär dig hur för[utforma din första Azure SQL-databas med hjälp av SSMS](sql-database-design-first-database.md) eller [utforma din första Azure SQL-databas med hjälp av .NET](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="0c311-148">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="0c311-149">Mer information om .NET finns i [.NET-dokumentationen](https://docs.microsoft.com/dotnet/).</span><span class="sxs-lookup"><span data-stu-id="0c311-149">For more information about .NET, see [.NET documentation](https://docs.microsoft.com/dotnet/).</span></span>
