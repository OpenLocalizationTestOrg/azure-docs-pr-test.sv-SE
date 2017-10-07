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
# <a name="use-net-c-with-visual-studio-tooconnect-and-query-an-azure-sql-database"></a>Använd .NET (C#) med Visual Studio tooconnect och frågar en Azure SQL-databas

Den här snabbstartsguide visar hur toouse hello [.NET framework](https://www.microsoft.com/net/) toocreate C#-program med Visual Studio tooconnect tooan Azure SQL database och använda Transact-SQL-instruktioner tooquery data.

## <a name="prerequisites"></a>Krav

toocomplete detta snabb start kursen, kontrollera att du har hello följande:

- En Azure SQL-databas. Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter: 

   - [Skapa DB – Portal](sql-database-get-started-portal.md)
   - [Skapa DB – CLI](sql-database-get-started-cli.md)
   - [Skapa DB – PowerShell](sql-database-get-started-powershell.md)

- En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.
- En installation av [Visual Studio Community 2017, Visual Studio Professional 2017 eller Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

## <a name="sql-server-connection-information"></a>Anslutningsinformation för en SQL-server

Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas. Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 
3. På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello. Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Om du glömmer dina inloggningsuppgifter för Azure SQL Database-server kan du navigera toohello SQL server sidan tooview hello admin Databasservernamnet. Du kan återställa hello lösenord om det behövs.

5. Klicka på **Visa databasanslutningssträngar**.

6. Granska hello fullständig **ADO.NET** anslutningssträngen.

    ![ADO.NET-anslutningssträng](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen. Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-visual-studio-project"></a>Skapa ett nytt Visual Studio-projekt

1. I Visual Studio väljer du **Arkiv**, **Nytt** och **Projekt**. 
2. I hello **nytt projekt** dialogrutan och visa **Visual C#**.
3. Välj **Konsolapp** och ange *sqltest* hello projektnamn.
4. Klicka på **OK** toocreate och öppna hello nytt projekt i Visual Studio
4. Högerklicka på **sqltest** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**. 
5. På hello **Bläddra**, söka efter ```System.Data.SqlClient``` och, när hitta, välja den.
6. I hello **System.Data.SqlClient** klickar du på **installera**.
7. När hello-installationen har slutförts, granska hello ändringar och klicka sedan på **OK** tooclose hello **Preview** fönster. 
8. Om ett fönster för **Godkännande av licens** visas klickar du på **Jag accepterar**.

## <a name="insert-code-tooquery-sql-database"></a>Infoga kod tooquery SQL-databas
1. Växla för (eller öppna vid behov) **Program.cs**

2. Ersätt hello innehållet i **Program.cs** med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.

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

## <a name="run-hello-code"></a>Köra hello kod

1. Tryck på **F5** toorun hello program.
2. Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.

## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[Anslut och fråga en Azure SQL database med .NET core](sql-database-connect-query-dotnet-core.md) på macOS/Windows/Linux.  
- Lär dig mer om [komma igång med .NET Core för Windows/Linux/macOS hello kommandoraden](/dotnet/core/tutorials/using-with-xplat-cli).
- Lär dig hur för[utforma din första Azure SQL-databas med hjälp av SSMS](sql-database-design-first-database.md) eller [utforma din första Azure SQL-databas med hjälp av .NET](sql-database-design-first-database-csharp.md).
- Mer information om .NET finns i [.NET-dokumentationen](https://docs.microsoft.com/dotnet/).
