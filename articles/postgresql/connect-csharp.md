---
title: "Ansluta tooAzure databas för PostgreSQL från C# | Microsoft Docs"
description: "Denna Snabbstart innehåller en C# (.NET) kodexemplet du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: csharp
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 5ba7426f8ad263193cdb208b3531da0ceff181dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-net-c-tooconnect-and-query-data"></a><span data-ttu-id="5d19f-103">Azure-databas för PostgreSQL: Använd .NET (C#) tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="5d19f-103">Azure Database for PostgreSQL: Use .NET (C#) tooconnect and query data</span></span>
<span data-ttu-id="5d19f-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med C#-program.</span><span class="sxs-lookup"><span data-stu-id="5d19f-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a C# application.</span></span> <span data-ttu-id="5d19f-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="5d19f-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="5d19f-106">hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med C# och att du är ny tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5d19f-106">hello steps in this article assume that you are familiar with developing using C#, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d19f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="5d19f-107">Prerequisites</span></span>
<span data-ttu-id="5d19f-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="5d19f-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="5d19f-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="5d19f-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="5d19f-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="5d19f-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="5d19f-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="5d19f-111">You also need to:</span></span>
- <span data-ttu-id="5d19f-112">Installera [.NET Framework](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="5d19f-112">Install [.NET Framework](https://www.microsoft.com/net/download).</span></span> <span data-ttu-id="5d19f-113">Hello åtgärderna i hello länkad artikel tooinstall .NET specifikt för din plattform (Windows, Ubuntu Linux eller macOS).</span><span class="sxs-lookup"><span data-stu-id="5d19f-113">Follow hello steps in hello linked article tooinstall .NET specifically for your platform (Windows, Ubuntu Linux, or macOS).</span></span> 
- <span data-ttu-id="5d19f-114">Installera [Visual Studio](https://www.visualstudio.com/downloads/) eller Visual Studio Code tootype och redigera kod.</span><span class="sxs-lookup"><span data-stu-id="5d19f-114">Install [Visual Studio](https://www.visualstudio.com/downloads/) or Visual Studio Code tootype and edit code.</span></span>
- <span data-ttu-id="5d19f-115">Installera [Npgsql](http://www.npgsql.org/doc/index.html)-biblioteket på det sätt som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="5d19f-115">Install [Npgsql](http://www.npgsql.org/doc/index.html) library as described below.</span></span>

## <a name="install-npgsql-references-into-your-visual-studio-solution"></a><span data-ttu-id="5d19f-116">Installera Npgsql-referenser i din Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="5d19f-116">Install Npgsql references into your Visual Studio solution</span></span>
<span data-ttu-id="5d19f-117">tooconnect från hello C#-program tooPostgreSQL, Använd hello öppen källkod ADO.NET-biblioteket som kallas Npgsql.</span><span class="sxs-lookup"><span data-stu-id="5d19f-117">tooconnect from hello C# application tooPostgreSQL, use hello open source ADO.NET library called Npgsql.</span></span> <span data-ttu-id="5d19f-118">NuGet kan ladda ned och enkelt hantera hello referenser.</span><span class="sxs-lookup"><span data-stu-id="5d19f-118">NuGet helps download and manage hello references easily.</span></span>

1. <span data-ttu-id="5d19f-119">Skapa en ny C#-lösning eller öppna en befintlig lösning:</span><span class="sxs-lookup"><span data-stu-id="5d19f-119">Create a new C# solution, or open an existing one:</span></span> 
   - <span data-ttu-id="5d19f-120">Skapa en lösning i Visual Studio genom att klicka på **Nytt** > **Projekt** på Arkiv-menyn.</span><span class="sxs-lookup"><span data-stu-id="5d19f-120">Within Visual Studio, create a solution, by clicking File menu **New** > **Project**.</span></span>
   - <span data-ttu-id="5d19f-121">Expandera i hello nytt projekt dialog, **mallar** > **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="5d19f-121">In hello New Project dialogue, expand **Templates** > **Visual C#**.</span></span> 
   - <span data-ttu-id="5d19f-122">Välj en lämplig mall, till exempel **Konsolprogram (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="5d19f-122">Choose an appropriate template such as **Console App (.NET Core)**.</span></span>

2. <span data-ttu-id="5d19f-123">Använd hello Nuget Package Manager tooinstall Npgsql:</span><span class="sxs-lookup"><span data-stu-id="5d19f-123">Use hello Nuget Package Manager tooinstall Npgsql:</span></span>
   - <span data-ttu-id="5d19f-124">Klicka på hello **verktyg** menyn > **NuGet Package Manager** > **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="5d19f-124">Click hello **Tools** menu > **NuGet Package Manager** > **Package Manager Console**.</span></span>
   - <span data-ttu-id="5d19f-125">I hello **Pakethanterarkonsolen**, typ`Install-Package Npgsql`</span><span class="sxs-lookup"><span data-stu-id="5d19f-125">In hello **Package Manager Console**, type `Install-Package Npgsql`</span></span>
   - <span data-ttu-id="5d19f-126">hello installera kommandot hämtningar hello Npgsql.dll och relaterade sammansättningar och lägger till dem som beroenden i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="5d19f-126">hello install command downloads hello Npgsql.dll and related assemblies and adds them as dependencies in hello solution.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="5d19f-127">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="5d19f-127">Get connection information</span></span>
<span data-ttu-id="5d19f-128">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5d19f-128">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="5d19f-129">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="5d19f-129">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="5d19f-130">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5d19f-130">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5d19f-131">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="5d19f-131">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="5d19f-132">Klicka på servernamnet för hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="5d19f-132">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="5d19f-133">Välj hello server **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="5d19f-133">Select hello server's **Overview** page.</span></span> <span data-ttu-id="5d19f-134">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="5d19f-134">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="5d19f-135">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-csharp/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="5d19f-135">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-csharp/1-connection-string.png)</span></span>
5. <span data-ttu-id="5d19f-136">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5d19f-136">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="5d19f-137">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="5d19f-137">Connect, create table, and insert data</span></span>
<span data-ttu-id="5d19f-138">Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="5d19f-138">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="5d19f-139">hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="5d19f-139">hello code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish a connection tooPostgreSQL.</span></span> <span data-ttu-id="5d19f-140">Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand)anger hello CommandText-egenskapen och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello databasen kommandon.</span><span class="sxs-lookup"><span data-stu-id="5d19f-140">Then hello code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), sets hello CommandText property, and calls method [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello database commands.</span></span> 

<span data-ttu-id="5d19f-141">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="5d19f-141">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresCreate
    {
        // Obtain connection string information from hello portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4}; SSL Mode=Prefer; Trust Server Certificate=true",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"
                                INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                                INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                                INSERT INTO inventory (name, quantity) VALUES ({4}, {5});
                            ",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}
```

## <a name="read-data"></a><span data-ttu-id="5d19f-142">Läsa data</span><span class="sxs-lookup"><span data-stu-id="5d19f-142">Read data</span></span>
<span data-ttu-id="5d19f-143">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="5d19f-143">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="5d19f-144">hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="5d19f-144">hello code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish a connection tooPostgreSQL.</span></span> <span data-ttu-id="5d19f-145">Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) och metoden [ExecuteReader()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) toorun hello databasen kommandon.</span><span class="sxs-lookup"><span data-stu-id="5d19f-145">Then hello code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) and method [ExecuteReader()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) toorun hello database commands.</span></span> <span data-ttu-id="5d19f-146">Nästa hello koden använder [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) tooadvance toohello poster i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="5d19f-146">Next hello code uses [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) tooadvance toohello records in hello results.</span></span> <span data-ttu-id="5d19f-147">Hello koden använder [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) och [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) tooparse hello värden i hello-post.</span><span class="sxs-lookup"><span data-stu-id="5d19f-147">Then hello code uses [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) and [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) tooparse hello values in hello record.</span></span>

<span data-ttu-id="5d19f-148">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="5d19f-148">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresRead
    {
        // Obtain connection string information from hello portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}
```


## <a name="update-data"></a><span data-ttu-id="5d19f-149">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="5d19f-149">Update data</span></span>
<span data-ttu-id="5d19f-150">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="5d19f-150">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="5d19f-151">hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="5d19f-151">hello code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish a connection tooPostgreSQL.</span></span> <span data-ttu-id="5d19f-152">Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand)anger hello CommandText-egenskapen och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello databasen kommandon.</span><span class="sxs-lookup"><span data-stu-id="5d19f-152">Then hello code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), sets hello CommandText property, and calls method [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello database commands.</span></span>

<span data-ttu-id="5d19f-153">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="5d19f-153">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresUpdate
    {
        // Obtain connection string information from hello portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}
```


## <a name="delete-data"></a><span data-ttu-id="5d19f-154">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="5d19f-154">Delete data</span></span>
<span data-ttu-id="5d19f-155">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="5d19f-155">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

 <span data-ttu-id="5d19f-156">hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="5d19f-156">hello code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish a connection tooPostgreSQL.</span></span> <span data-ttu-id="5d19f-157">Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand)anger hello CommandText-egenskapen och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello databasen kommandon.</span><span class="sxs-lookup"><span data-stu-id="5d19f-157">Then hello code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), sets hello CommandText property, and calls method [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello database commands.</span></span>

<span data-ttu-id="5d19f-158">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="5d19f-158">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresDelete
    {
        // Obtain connection string information from hello portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("DELETE FROM inventory WHERE name = {0};",
                "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="5d19f-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d19f-159">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="5d19f-160">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="5d19f-160">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
