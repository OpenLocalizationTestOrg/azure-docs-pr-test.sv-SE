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
# <a name="azure-database-for-postgresql-use-net-c-tooconnect-and-query-data"></a>Azure-databas för PostgreSQL: Använd .NET (C#) tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med C#-program. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med C# och att du är ny tooworking med Azure-databas för PostgreSQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa DB – Portal](quickstart-create-server-database-portal.md)
- [Skapa DB – CLI](quickstart-create-server-database-azure-cli.md)

Du måste också:
- Installera [.NET Framework](https://www.microsoft.com/net/download). Hello åtgärderna i hello länkad artikel tooinstall .NET specifikt för din plattform (Windows, Ubuntu Linux eller macOS). 
- Installera [Visual Studio](https://www.visualstudio.com/downloads/) eller Visual Studio Code tootype och redigera kod.
- Installera [Npgsql](http://www.npgsql.org/doc/index.html)-biblioteket på det sätt som beskrivs nedan.

## <a name="install-npgsql-references-into-your-visual-studio-solution"></a>Installera Npgsql-referenser i din Visual Studio-lösning
tooconnect från hello C#-program tooPostgreSQL, Använd hello öppen källkod ADO.NET-biblioteket som kallas Npgsql. NuGet kan ladda ned och enkelt hantera hello referenser.

1. Skapa en ny C#-lösning eller öppna en befintlig lösning: 
   - Skapa en lösning i Visual Studio genom att klicka på **Nytt** > **Projekt** på Arkiv-menyn.
   - Expandera i hello nytt projekt dialog, **mallar** > **Visual C#**. 
   - Välj en lämplig mall, till exempel **Konsolprogram (.NET Core)**.

2. Använd hello Nuget Package Manager tooinstall Npgsql:
   - Klicka på hello **verktyg** menyn > **NuGet Package Manager** > **Pakethanterarkonsolen**.
   - I hello **Pakethanterarkonsolen**, typ`Install-Package Npgsql`
   - hello installera kommandot hämtningar hello Npgsql.dll och relaterade sammansättningar och lägger till dem som beroenden i hello-lösning.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.
3. Klicka på servernamnet för hello **mypgserver 20170401**.
4. Välj hello server **översikt** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-csharp/1-connection-string.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="connect-create-table-and-insert-data"></a>Ansluta, skapa tabell och infoga data
Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner. hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning. Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand)anger hello CommandText-egenskapen och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello databasen kommandon. 

Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas. 

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

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning. Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) och metoden [ExecuteReader()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) toorun hello databasen kommandon. Nästa hello koden använder [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) tooadvance toohello poster i hello resultat. Hello koden använder [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) och [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) tooparse hello värden i hello-post.

Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas. 

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


## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen. hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning. Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand)anger hello CommandText-egenskapen och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello databasen kommandon.

Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas. 

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


## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

 hello koden använder NpgsqlCommand klass med metoden [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) tooestablish tooPostgreSQL en anslutning. Sedan hello används metoden [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand)anger hello CommandText-egenskapen och anropar metoden [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) toorun hello databasen kommandon.

Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas. 

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

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
