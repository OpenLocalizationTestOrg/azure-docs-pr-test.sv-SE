---
title: aaaUse .NET Core tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet visar hur toouse .NET Core toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
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
ms.openlocfilehash: 2d10c407f44f43b6baa3bf337cdd1173d9c9c35f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-c-tooquery-an-azure-sql-database"></a><span data-ttu-id="d9b97-103">Använda .NET Core (C#) tooquery en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="d9b97-103">Use .NET Core (C#) tooquery an Azure SQL database</span></span>

<span data-ttu-id="d9b97-104">Den här snabbstartsguide visar hur toouse [.NET Core](https://www.microsoft.com/net/) på Windows/Linux/macOS toocreate C#-program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="d9b97-104">This quick start tutorial demonstrates how toouse [.NET Core](https://www.microsoft.com/net/) on Windows/Linux/macOS toocreate a C# program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9b97-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d9b97-105">Prerequisites</span></span>

<span data-ttu-id="d9b97-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="d9b97-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="d9b97-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d9b97-107">An Azure SQL database.</span></span> <span data-ttu-id="d9b97-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="d9b97-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="d9b97-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="d9b97-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="d9b97-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="d9b97-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="d9b97-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="d9b97-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="d9b97-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="d9b97-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="d9b97-113">Du har installerat [.NET Core för ditt operativsystem](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="d9b97-113">You have installed [.NET Core for your operating system](https://www.microsoft.com/net/core).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="d9b97-114">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="d9b97-114">SQL server connection information</span></span>

<span data-ttu-id="d9b97-115">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d9b97-115">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="d9b97-116">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="d9b97-116">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="d9b97-117">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d9b97-117">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d9b97-118">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="d9b97-118">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="d9b97-119">På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="d9b97-119">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="d9b97-120">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="d9b97-120">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="d9b97-122">Om du glömmer dina inloggningsuppgifter för Azure SQL Database-server kan du navigera toohello SQL server sidan tooview hello admin Databasservernamnet.</span><span class="sxs-lookup"><span data-stu-id="d9b97-122">If you forget your Azure SQL Database server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span> <span data-ttu-id="d9b97-123">Du kan återställa hello lösenord om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d9b97-123">You can reset hello password if necessary.</span></span>

5. <span data-ttu-id="d9b97-124">Klicka på **Visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="d9b97-124">Click **Show database connection strings**.</span></span>

6. <span data-ttu-id="d9b97-125">Granska hello fullständig **ADO.NET** anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="d9b97-125">Review hello complete **ADO.NET** connection string.</span></span>

    ![ADO.NET-anslutningssträng](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> <span data-ttu-id="d9b97-127">Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d9b97-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="d9b97-128">Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="d9b97-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 
>
  
## <a name="create-a-new-net-project"></a><span data-ttu-id="d9b97-129">Skapa ett nytt .NET-projekt</span><span class="sxs-lookup"><span data-stu-id="d9b97-129">Create a new .NET project</span></span>

1. <span data-ttu-id="d9b97-130">Öppna en kommandotolk och skapa en mapp med namnet *sqltest*.</span><span class="sxs-lookup"><span data-stu-id="d9b97-130">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="d9b97-131">Navigera toohello mappen du skapade och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="d9b97-131">Navigate toohello folder you created and run hello following command:</span></span>

    ```
    dotnet new console
    ```

2. <span data-ttu-id="d9b97-132">Öppna ***sqltest.csproj*** med valfri textredigerare och Lägg till System.Data.SqlClient som ett beroende som använder hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="d9b97-132">Open ***sqltest.csproj*** with your favorite text editor and add System.Data.SqlClient as a dependency using hello following code:</span></span>

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
    </ItemGroup>
    ```

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="d9b97-133">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="d9b97-133">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="d9b97-134">Öppna **Program.cs** i din utvecklingsmiljö eller textredigerare</span><span class="sxs-lookup"><span data-stu-id="d9b97-134">In your development environment or favorite text editor open **Program.cs**</span></span>

2. <span data-ttu-id="d9b97-135">Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d9b97-135">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="d9b97-136">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="d9b97-136">Run hello code</span></span>

1. <span data-ttu-id="d9b97-137">Kör följande kommandon hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="d9b97-137">At hello command prompt, run hello following commands:</span></span>

   ```csharp
   dotnet restore
   dotnet run
   ```

2. <span data-ttu-id="d9b97-138">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="d9b97-138">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d9b97-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9b97-139">Next steps</span></span>

- <span data-ttu-id="d9b97-140">[Komma igång med .NET Core för Windows/Linux/macOS hello kommandoraden](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="d9b97-140">[Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="d9b97-141">Lär dig hur för[ansluter och frågar en Azure SQL-databas med hjälp av hello .NET framework och Visual Studio](sql-database-connect-query-dotnet-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="d9b97-141">Learn how too[connect and query an Azure SQL database using hello .NET framework and Visual Studio](sql-database-connect-query-dotnet-visual-studio.md).</span></span>  
- <span data-ttu-id="d9b97-142">Lär dig hur för[utforma din första Azure SQL-databas med hjälp av SSMS](sql-database-design-first-database.md) eller [utforma din första Azure SQL-databas med hjälp av .NET](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="d9b97-142">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="d9b97-143">Mer information om .NET finns i [.NET-dokumentationen](https://docs.microsoft.com/dotnet/).</span><span class="sxs-lookup"><span data-stu-id="d9b97-143">For more information about .NET, see [.NET documentation](https://docs.microsoft.com/dotnet/).</span></span>
