---
title: "Ansluta till Azure Database för PostgreSQL med C# | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i C# (.NET) som du kan använda för att ansluta till och fråga efter data från Azure Database för PostgreSQL."
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
ms.openlocfilehash: 91e0269e310688dc88d139430ccf386a1d26a61c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-net-c-to-connect-and-query-data"></a><span data-ttu-id="77559-103">Azure Database för PostgreSQL: Använda .NET (C#) för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="77559-103">Azure Database for PostgreSQL: Use .NET (C#) to connect and query data</span></span>
<span data-ttu-id="77559-104">Den här snabbstarten visar hur du ansluter till en Azure Database för PostgreSQL med hjälp av ett C#-program.</span><span class="sxs-lookup"><span data-stu-id="77559-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using a C# application.</span></span> <span data-ttu-id="77559-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="77559-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="77559-106">I den här artikeln förutsätter vi att du har kunskaper om C# och att du inte har arbetat med Azure Database för PostgreSQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="77559-106">The steps in this article assume that you are familiar with developing using C#, and that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77559-107">Krav</span><span class="sxs-lookup"><span data-stu-id="77559-107">Prerequisites</span></span>
<span data-ttu-id="77559-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="77559-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="77559-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="77559-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="77559-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="77559-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="77559-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="77559-111">You also need to:</span></span>
- <span data-ttu-id="77559-112">Installera [.NET Framework](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="77559-112">Install [.NET Framework](https://www.microsoft.com/net/download).</span></span> <span data-ttu-id="77559-113">Följ stegen i den länkade artikeln för att installera .NET specifikt för din plattform (Windows, Ubuntu, Linux eller Mac OS).</span><span class="sxs-lookup"><span data-stu-id="77559-113">Follow the steps in the linked article to install .NET specifically for your platform (Windows, Ubuntu Linux, or macOS).</span></span> 
- <span data-ttu-id="77559-114">Installera [Visual Studio](https://www.visualstudio.com/downloads/) eller Visual Studio Code för att skriva och redigera kod.</span><span class="sxs-lookup"><span data-stu-id="77559-114">Install [Visual Studio](https://www.visualstudio.com/downloads/) or Visual Studio Code to type and edit code.</span></span>
- <span data-ttu-id="77559-115">Installera [Npgsql](http://www.npgsql.org/doc/index.html)-biblioteket på det sätt som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="77559-115">Install [Npgsql](http://www.npgsql.org/doc/index.html) library as described below.</span></span>

## <a name="install-npgsql-references-into-your-visual-studio-solution"></a><span data-ttu-id="77559-116">Installera Npgsql-referenser i din Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="77559-116">Install Npgsql references into your Visual Studio solution</span></span>
<span data-ttu-id="77559-117">Använd ADO.NET-biblioteket Npgsql (öppen källkod) när du ansluter från C#-programmet till PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="77559-117">To connect from the C# application to PostgreSQL, use the open source ADO.NET library called Npgsql.</span></span> <span data-ttu-id="77559-118">Med NuGet kan du enkelt ladda ned och hantera referenserna.</span><span class="sxs-lookup"><span data-stu-id="77559-118">NuGet helps download and manage the references easily.</span></span>

1. <span data-ttu-id="77559-119">Skapa en ny C#-lösning eller öppna en befintlig lösning:</span><span class="sxs-lookup"><span data-stu-id="77559-119">Create a new C# solution, or open an existing one:</span></span> 
   - <span data-ttu-id="77559-120">Skapa en lösning i Visual Studio genom att klicka på **Nytt** > **Projekt** på Arkiv-menyn.</span><span class="sxs-lookup"><span data-stu-id="77559-120">Within Visual Studio, create a solution, by clicking File menu **New** > **Project**.</span></span>
   - <span data-ttu-id="77559-121">I dialogrutan Nytt projekt expanderar du **Mallar** > **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="77559-121">In the New Project dialogue, expand **Templates** > **Visual C#**.</span></span> 
   - <span data-ttu-id="77559-122">Välj en lämplig mall, till exempel **Konsolprogram (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="77559-122">Choose an appropriate template such as **Console App (.NET Core)**.</span></span>

2. <span data-ttu-id="77559-123">Installera Npgsql med Nuget Package Manager:</span><span class="sxs-lookup"><span data-stu-id="77559-123">Use the Nuget Package Manager to install Npgsql:</span></span>
   - <span data-ttu-id="77559-124">Klicka på **Verktyg**-menyn > **NuGet Package Manager** > **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="77559-124">Click the **Tools** menu > **NuGet Package Manager** > **Package Manager Console**.</span></span>
   - <span data-ttu-id="77559-125">Skriv `Install-Package Npgsql` i **Package Manager-konsolen**</span><span class="sxs-lookup"><span data-stu-id="77559-125">In the **Package Manager Console**, type `Install-Package Npgsql`</span></span>
   - <span data-ttu-id="77559-126">Installationskommandot hämtar Npgsql.dll och relaterade sammansättningar och lägger till dem som beroenden i lösningen.</span><span class="sxs-lookup"><span data-stu-id="77559-126">The install command downloads the Npgsql.dll and related assemblies and adds them as dependencies in the solution.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="77559-127">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="77559-127">Get connection information</span></span>
<span data-ttu-id="77559-128">Hämta den information som du behöver för att ansluta till Azure Database för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="77559-128">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="77559-129">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="77559-129">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="77559-130">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="77559-130">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="77559-131">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="77559-131">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="77559-132">Klicka på servernamnet **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="77559-132">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="77559-133">Välj serverns **översikt**-sida.</span><span class="sxs-lookup"><span data-stu-id="77559-133">Select the server's **Overview** page.</span></span> <span data-ttu-id="77559-134">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="77559-134">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="77559-135">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-csharp/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="77559-135">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-csharp/1-connection-string.png)</span></span>
5. <span data-ttu-id="77559-136">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="77559-136">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="77559-137">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="77559-137">Connect, create table, and insert data</span></span>
<span data-ttu-id="77559-138">Använd följande kod för att ansluta och läsa in data med hjälp av SQL-instruktionerna **CREATE TABLE** och **INSERT**.</span><span class="sxs-lookup"><span data-stu-id="77559-138">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="77559-139">Koden använder klassen NpgsqlCommand med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) för att upprätta en anslutning till PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="77559-139">The code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) to establish a connection to PostgreSQL.</span></span> <span data-ttu-id="77559-140">Sedan använder koden metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), anger egenskapen CommandText och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="77559-140">Then the code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), sets the CommandText property, and calls method [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) to run the database commands.</span></span> 

<span data-ttu-id="77559-141">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="77559-141">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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
        // Obtain connection string information from the portal
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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="read-data"></a><span data-ttu-id="77559-142">Läsa data</span><span class="sxs-lookup"><span data-stu-id="77559-142">Read data</span></span>
<span data-ttu-id="77559-143">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="77559-143">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="77559-144">Koden använder klassen NpgsqlCommand med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) för att upprätta en anslutning till PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="77559-144">The code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) to establish a connection to PostgreSQL.</span></span> <span data-ttu-id="77559-145">Sedan används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) och metoden [ExecuteReader()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="77559-145">Then the code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) and method [ExecuteReader()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) to run the database commands.</span></span> <span data-ttu-id="77559-146">Metoden [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) används sedan för att gå vidare till posterna i resultaten.</span><span class="sxs-lookup"><span data-stu-id="77559-146">Next the code uses [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) to advance to the records in the results.</span></span> <span data-ttu-id="77559-147">Koden använder sedan [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) och [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) för att parsa värdena i posten.</span><span class="sxs-lookup"><span data-stu-id="77559-147">Then the code uses [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) and [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) to parse the values in the record.</span></span>

<span data-ttu-id="77559-148">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="77559-148">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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
        // Obtain connection string information from the portal
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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


## <a name="update-data"></a><span data-ttu-id="77559-149">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="77559-149">Update data</span></span>
<span data-ttu-id="77559-150">Använd följande kod för att ansluta och läsa data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="77559-150">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="77559-151">Koden använder klassen NpgsqlCommand med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) för att upprätta en anslutning till PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="77559-151">The code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) to establish a connection to PostgreSQL.</span></span> <span data-ttu-id="77559-152">Sedan använder koden metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), anger egenskapen CommandText och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="77559-152">Then the code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), sets the CommandText property, and calls method [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) to run the database commands.</span></span>

<span data-ttu-id="77559-153">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="77559-153">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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
        // Obtain connection string information from the portal
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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


## <a name="delete-data"></a><span data-ttu-id="77559-154">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="77559-154">Delete data</span></span>
<span data-ttu-id="77559-155">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="77559-155">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

 <span data-ttu-id="77559-156">Koden använder klassen NpgsqlCommand med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) för att upprätta en anslutning till PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="77559-156">The code uses NpgsqlCommand class with method [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) to establish a connection to PostgreSQL.</span></span> <span data-ttu-id="77559-157">Sedan använder koden metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), anger egenskapen CommandText och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="77559-157">Then the code uses method [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), sets the CommandText property, and calls method [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) to run the database commands.</span></span>

<span data-ttu-id="77559-158">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="77559-158">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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
        // Obtain connection string information from the portal
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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="77559-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77559-159">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="77559-160">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="77559-160">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
