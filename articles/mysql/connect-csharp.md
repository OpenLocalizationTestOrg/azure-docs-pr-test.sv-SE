---
title: "Ansluta till Azure Database för MySQL från C# | Microsoft Docs"
description: "Den här snabbstarten innehåller ett kodexempel i C# (.NET) som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
services: MySQL
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: MySQL-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 07/10/2017
ms.openlocfilehash: f1488f6b4a240165c71c95f759af73d6b9fd7bfe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a><span data-ttu-id="4a153-103">Azure Database för MySQL: Använda .NET (C#) för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="4a153-103">Azure Database for MySQL: Use .NET (C#) to connect and query data</span></span>
<span data-ttu-id="4a153-104">Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett C#-program.</span><span class="sxs-lookup"><span data-stu-id="4a153-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a C# application.</span></span> <span data-ttu-id="4a153-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="4a153-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="4a153-106">I den här artikeln förutsätter vi att du har kunskaper om C# och att du inte har arbetat med Azure Database för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="4a153-106">The steps in this article assume that you are familiar with developing using C#, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a153-107">Krav</span><span class="sxs-lookup"><span data-stu-id="4a153-107">Prerequisites</span></span>
<span data-ttu-id="4a153-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="4a153-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="4a153-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4a153-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="4a153-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4a153-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="4a153-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="4a153-111">You also need to:</span></span>
- <span data-ttu-id="4a153-112">Installera [.NET](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="4a153-112">Install [.NET](https://www.microsoft.com/net/download).</span></span> <span data-ttu-id="4a153-113">Följ stegen i den länkade artikeln för att installera .NET specifikt för din plattform (Windows, Ubuntu, Linux eller Mac OS).</span><span class="sxs-lookup"><span data-stu-id="4a153-113">Follow the steps in the linked article to install .NET specifically for your platform (Windows, Ubuntu Linux, or macOS).</span></span> 
- <span data-ttu-id="4a153-114">[Installera Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="4a153-114">Install [Visual Studio](https://www.visualstudio.com/downloads/).</span></span>
- <span data-ttu-id="4a153-115">Installera [ODBC Driver för MySQL](https://dev.mysql.com/downloads/connector/odbc/).</span><span class="sxs-lookup"><span data-stu-id="4a153-115">Install [ODBC Driver for MySQL](https://dev.mysql.com/downloads/connector/odbc/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="4a153-116">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="4a153-116">Get connection information</span></span>
<span data-ttu-id="4a153-117">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="4a153-117">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="4a153-118">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4a153-118">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="4a153-119">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4a153-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4a153-120">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="4a153-120">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="4a153-121">Klicka på servernamnet.</span><span class="sxs-lookup"><span data-stu-id="4a153-121">Click the server name.</span></span>
4. <span data-ttu-id="4a153-122">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="4a153-122">Select the server's **Properties** page.</span></span> <span data-ttu-id="4a153-123">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="4a153-123">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="4a153-124">![Azure Database för MySQL-servernamn](./media/connect-csharp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="4a153-124">![Azure Database for MySQL server name](./media/connect-csharp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="4a153-125">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="4a153-125">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="4a153-126">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="4a153-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="4a153-127">Använd följande kod för att ansluta och läsa in data med hjälp av SQL-instruktionerna **CREATE TABLE** och **INSERT**.</span><span class="sxs-lookup"><span data-stu-id="4a153-127">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="4a153-128">Koden använder klassen QDBC med metoden [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4a153-128">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="4a153-129">Sedan använder koden metoden [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), anger egenskapen CommandText och anropar metoden [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="4a153-129">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span> 

<span data-ttu-id="4a153-130">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4a153-130">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;

namespace driver
{
    class MySQLCreate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

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
                    @"INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                    INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                    INSERT INTO inventory (name, quantity) VALUES ({4}, {5});",
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

## <a name="read-data"></a><span data-ttu-id="4a153-131">Läsa data</span><span class="sxs-lookup"><span data-stu-id="4a153-131">Read data</span></span>

<span data-ttu-id="4a153-132">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="4a153-132">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="4a153-133">Koden använder klassen QDBC med metoden [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4a153-133">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="4a153-134">Sedan används metoden [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) och metoden [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="4a153-134">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) and method [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) to run the database commands.</span></span> <span data-ttu-id="4a153-135">Metoden [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) används sedan för att gå vidare till posterna i resultaten.</span><span class="sxs-lookup"><span data-stu-id="4a153-135">Next the code uses [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) to advance to the records in the results.</span></span> <span data-ttu-id="4a153-136">Koden använder sedan GetInt32 och GetString för att parsa värdena i posten.</span><span class="sxs-lookup"><span data-stu-id="4a153-136">Then the code uses GetInt32 and GetString to parse the values in the record.</span></span>

<span data-ttu-id="4a153-137">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4a153-137">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;


namespace driver
{
    class MySQLRead
    {

        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

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

## <a name="update-data"></a><span data-ttu-id="4a153-138">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="4a153-138">Update data</span></span>
<span data-ttu-id="4a153-139">Använd följande kod för att ansluta och läsa data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="4a153-139">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="4a153-140">Koden använder klassen QDBC med metoden [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4a153-140">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="4a153-141">Sedan använder koden metoden [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), anger egenskapen CommandText och anropar metoden [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="4a153-141">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span>

<span data-ttu-id="4a153-142">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4a153-142">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLUpdate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

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


## <a name="delete-data"></a><span data-ttu-id="4a153-143">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="4a153-143">Delete data</span></span>
<span data-ttu-id="4a153-144">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="4a153-144">Use the following code to connect and delete the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="4a153-145">Koden använder klassen QDBC med metoden [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4a153-145">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="4a153-146">Sedan använder koden metoden [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), anger egenskapen CommandText och anropar metoden [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="4a153-146">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span>

<span data-ttu-id="4a153-147">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4a153-147">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLDelete
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

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

## <a name="next-steps"></a><span data-ttu-id="4a153-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a153-148">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4a153-149">Migrera MySQL-databasen till Azure Database för MySQL med säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="4a153-149">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
