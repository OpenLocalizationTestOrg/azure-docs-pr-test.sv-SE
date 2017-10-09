---
title: aaaUse Azure Functions tooperform en databas Rensa uppgiften | Microsoft Docs
description: "Använd Azure Functions tooschedule en aktivitet som ansluter tooAzure SQL-databas tooperiodically Rensa rader."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 063a25fe8d14a75d54e9b72cec9fc1e25fa3ff44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-tooconnect-tooan-azure-sql-database"></a>Använd Azure Functions tooconnect tooan Azure SQL Database
Det här avsnittet visar hur toouse Azure Functions toocreate ett schemalagt jobb som rensar upp rader i en tabell i Azure SQL-databas. nya hello C# funktionen skapas baserat på en fördefinierad timer utlösaren mall i hello Azure-portalen. toosupport i det här scenariot måste du också ange en anslutningssträng för databasen som en inställning i hello funktionsapp. Det här scenariot använder en massåtgärd mot hello-databasen. toohave din funktion processen enskilda CRUD-åtgärder i en tabell med Mobilappar, bör du istället använda [Mobile Apps bindningar](functions-bindings-mobile-apps.md).

## <a name="prerequisites"></a>Krav

+ Det här avsnittet använder en som utlöste-timerfunktion. Fullständig hello stegen i avsnittet hello [skapa en funktion i Azure som utlöses av en timer](functions-create-scheduled-function.md) toocreate C#-versionen av den här funktionen.   

+ Det här avsnittet visar ett Transact-SQL-kommando som kör en massåtgärd rensning i hello **SalesOrderHeader** tabell i exempeldatabasen för hello AdventureWorksLT. toocreate hello AdventureWorksLT exempeldatabasen, fullständig hello stegen i avsnittet hello [skapa en Azure SQL database i hello Azure-portalen](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Hämta anslutningsinformation

Du behöver tooget hello anslutningssträngen för hello-databasen som du skapade när du slutfört [skapa en Azure SQL database i hello Azure-portalen](../sql-database/sql-database-get-started-portal.md).

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
 
3. Välj **SQL-databaser** hello vänstra menyn och välj en databas på hello **SQL-databaser** sidan.

4. Välj **visa databasanslutningssträngar** och kopiera hello fullständig **ADO.NET** anslutningssträngen.

    ![Kopiera hello ADO.NET-anslutningssträngen.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-hello-connection-string"></a>Ange hello-anslutningssträng 

En funktionsapp är värd för hello körningen av dina funktioner i Azure. Det är en bästa praxis toostore anslutningssträngar och andra hemligheter i funktionen app-inställningarna. Med hjälp av programinställningar förhindrar oavsiktlig avslöjande av hello-anslutningssträngen med din kod. 

1. Navigera tooyour funktionsapp som du skapade [skapa en funktion i Azure som utlöses av en timer](functions-create-scheduled-function.md).

2. Välj **plattformsfunktioner** > **programinställningar**.
   
    ![Programinställningar för hello funktionsapp.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Rulla nedåt för**anslutningssträngar** och lägga till en anslutningssträng med hello inställningar som anges i hello tabell.
   
    ![Lägg till en sträng toohello funktionen app anslutningsinställningar.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Inställning       | Föreslaget värde | Beskrivning             | 
    | ------------ | ------------------ | --------------------- | 
    | **Namn**  |  sqldb_connection  | Använda tooaccess hello lagras anslutningssträngen i funktionskoden.    |
    | **Värde** | Kopierade sträng  | Tidigare hello anslutningssträngen kopieras hello föregående avsnitt. |
    | **Typ** | SQL Database | Använd hello standardanslutning för SQL-databas. |   

3. Klicka på **Spara**.

Nu kan du lägga till hello C# Funktionskoden som ansluter tooyour SQL-databas.

## <a name="update-your-function-code"></a>Uppdatera din funktionskod

1. Välj hello timer-utlösta funktion i appen funktion.
 
3. Lägg till följande sammansättningsreferenser hello överst i hello befintliga Funktionskoden hello:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. Lägg till följande hello `using` instruktioner toohello funktionen:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Ersätta befintliga hello **kör** funktion med hello följande kod:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute hello command and log hello # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Det här exemplet kommandot uppdaterar hello **Status** kolumnen baserat på hello leverera datum. Det bör uppdatera 32 datarader.

5. Klicka på **spara**, titta på hello **loggar** windows hello bredvid fungera körning och Observera hello antalet rader som uppdateras i hello **SalesOrderHeader** tabell.

    ![Visa hello funktionsloggar.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Nästa steg

Lär dig sedan hur toouse fungerar med Logic Apps toointegrate med andra tjänster.

> [!div class="nextstepaction"] 
> [Skapa en funktion som kan integreras med Logic Apps](functions-twitter-email.md)

Mer information om funktioner finns i hello följande avsnitt:

* [Azure Functions, info för utvecklare](functions-reference.md)  
  Info för programmerare om att koda funktioner och definiera utlösare och bindningar.
* [Testa Azure Functions](functions-test-a-function.md)  
  Beskriver olika verktyg och tekniker för att testa funktioner.  
