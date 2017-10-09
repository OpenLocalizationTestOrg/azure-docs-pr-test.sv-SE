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
# <a name="use-azure-functions-tooconnect-tooan-azure-sql-database"></a><span data-ttu-id="f9e3c-103">Använd Azure Functions tooconnect tooan Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f9e3c-103">Use Azure Functions tooconnect tooan Azure SQL Database</span></span>
<span data-ttu-id="f9e3c-104">Det här avsnittet visar hur toouse Azure Functions toocreate ett schemalagt jobb som rensar upp rader i en tabell i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-104">This topic shows you how toouse Azure Functions toocreate a scheduled job that cleans up rows in a table in an Azure SQL Database.</span></span> <span data-ttu-id="f9e3c-105">nya hello C# funktionen skapas baserat på en fördefinierad timer utlösaren mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-105">hello new C# function is created based on a pre-defined timer trigger template in hello Azure portal.</span></span> <span data-ttu-id="f9e3c-106">toosupport i det här scenariot måste du också ange en anslutningssträng för databasen som en inställning i hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-106">toosupport this scenario, you must also set a database connection string as a setting in hello function app.</span></span> <span data-ttu-id="f9e3c-107">Det här scenariot använder en massåtgärd mot hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-107">This scenario uses a bulk operation against hello database.</span></span> <span data-ttu-id="f9e3c-108">toohave din funktion processen enskilda CRUD-åtgärder i en tabell med Mobilappar, bör du istället använda [Mobile Apps bindningar](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="f9e3c-108">toohave your function process individual CRUD operations in a Mobile Apps table, you should instead use [Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9e3c-109">Krav</span><span class="sxs-lookup"><span data-stu-id="f9e3c-109">Prerequisites</span></span>

+ <span data-ttu-id="f9e3c-110">Det här avsnittet använder en som utlöste-timerfunktion.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-110">This topic uses a timer triggered function.</span></span> <span data-ttu-id="f9e3c-111">Fullständig hello stegen i avsnittet hello [skapa en funktion i Azure som utlöses av en timer](functions-create-scheduled-function.md) toocreate C#-versionen av den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-111">Complete hello steps in hello topic [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md) toocreate a C# version of this function.</span></span>   

+ <span data-ttu-id="f9e3c-112">Det här avsnittet visar ett Transact-SQL-kommando som kör en massåtgärd rensning i hello **SalesOrderHeader** tabell i exempeldatabasen för hello AdventureWorksLT.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-112">This topic demonstrates a Transact-SQL command that executes a bulk cleanup operation in hello **SalesOrderHeader** table in hello AdventureWorksLT sample database.</span></span> <span data-ttu-id="f9e3c-113">toocreate hello AdventureWorksLT exempeldatabasen, fullständig hello stegen i avsnittet hello [skapa en Azure SQL database i hello Azure-portalen](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f9e3c-113">toocreate hello AdventureWorksLT sample database, complete hello steps in hello topic [Create an Azure SQL database in hello Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span> 

## <a name="get-connection-information"></a><span data-ttu-id="f9e3c-114">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="f9e3c-114">Get connection information</span></span>

<span data-ttu-id="f9e3c-115">Du behöver tooget hello anslutningssträngen för hello-databasen som du skapade när du slutfört [skapa en Azure SQL database i hello Azure-portalen](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f9e3c-115">You need tooget hello connection string for hello database you created when you completed [Create an Azure SQL database in hello Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span>

1. <span data-ttu-id="f9e3c-116">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f9e3c-116">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
 
3. <span data-ttu-id="f9e3c-117">Välj **SQL-databaser** hello vänstra menyn och välj en databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-117">Select **SQL Databases** from hello left-hand menu, and select your database on hello **SQL databases** page.</span></span>

4. <span data-ttu-id="f9e3c-118">Välj **visa databasanslutningssträngar** och kopiera hello fullständig **ADO.NET** anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-118">Select **Show database connection strings** and copy hello complete **ADO.NET** connection string.</span></span>

    ![Kopiera hello ADO.NET-anslutningssträngen.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-hello-connection-string"></a><span data-ttu-id="f9e3c-120">Ange hello-anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="f9e3c-120">Set hello connection string</span></span> 

<span data-ttu-id="f9e3c-121">En funktionsapp är värd för hello körningen av dina funktioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-121">A function app hosts hello execution of your functions in Azure.</span></span> <span data-ttu-id="f9e3c-122">Det är en bästa praxis toostore anslutningssträngar och andra hemligheter i funktionen app-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-122">It is a best practice toostore connection strings and other secrets in your function app settings.</span></span> <span data-ttu-id="f9e3c-123">Med hjälp av programinställningar förhindrar oavsiktlig avslöjande av hello-anslutningssträngen med din kod.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-123">Using application settings prevents accidental disclosure of hello connection string with your code.</span></span> 

1. <span data-ttu-id="f9e3c-124">Navigera tooyour funktionsapp som du skapade [skapa en funktion i Azure som utlöses av en timer](functions-create-scheduled-function.md).</span><span class="sxs-lookup"><span data-stu-id="f9e3c-124">Navigate tooyour function app you created [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md).</span></span>

2. <span data-ttu-id="f9e3c-125">Välj **plattformsfunktioner** > **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-125">Select **Platform features** > **Application settings**.</span></span>
   
    ![Programinställningar för hello funktionsapp.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. <span data-ttu-id="f9e3c-127">Rulla nedåt för**anslutningssträngar** och lägga till en anslutningssträng med hello inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-127">Scroll down too**Connection strings** and add a connection string using hello settings as specified in hello table.</span></span>
   
    ![Lägg till en sträng toohello funktionen app anslutningsinställningar.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | <span data-ttu-id="f9e3c-129">Inställning</span><span class="sxs-lookup"><span data-stu-id="f9e3c-129">Setting</span></span>       | <span data-ttu-id="f9e3c-130">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="f9e3c-130">Suggested value</span></span> | <span data-ttu-id="f9e3c-131">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f9e3c-131">Description</span></span>             | 
    | ------------ | ------------------ | --------------------- | 
    | <span data-ttu-id="f9e3c-132">**Namn**</span><span class="sxs-lookup"><span data-stu-id="f9e3c-132">**Name**</span></span>  |  <span data-ttu-id="f9e3c-133">sqldb_connection</span><span class="sxs-lookup"><span data-stu-id="f9e3c-133">sqldb_connection</span></span>  | <span data-ttu-id="f9e3c-134">Använda tooaccess hello lagras anslutningssträngen i funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-134">Used tooaccess hello stored connection string in your function code.</span></span>    |
    | <span data-ttu-id="f9e3c-135">**Värde**</span><span class="sxs-lookup"><span data-stu-id="f9e3c-135">**Value**</span></span> | <span data-ttu-id="f9e3c-136">Kopierade sträng</span><span class="sxs-lookup"><span data-stu-id="f9e3c-136">Copied string</span></span>  | <span data-ttu-id="f9e3c-137">Tidigare hello anslutningssträngen kopieras hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-137">Past hello connection string you copied in hello previous section.</span></span> |
    | <span data-ttu-id="f9e3c-138">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f9e3c-138">**Type**</span></span> | <span data-ttu-id="f9e3c-139">SQL Database</span><span class="sxs-lookup"><span data-stu-id="f9e3c-139">SQL Database</span></span> | <span data-ttu-id="f9e3c-140">Använd hello standardanslutning för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-140">Use hello default SQL Database connection.</span></span> |   

3. <span data-ttu-id="f9e3c-141">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-141">Click **Save**.</span></span>

<span data-ttu-id="f9e3c-142">Nu kan du lägga till hello C# Funktionskoden som ansluter tooyour SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-142">Now, you can add hello C# function code that connects tooyour SQL Database.</span></span>

## <a name="update-your-function-code"></a><span data-ttu-id="f9e3c-143">Uppdatera din funktionskod</span><span class="sxs-lookup"><span data-stu-id="f9e3c-143">Update your function code</span></span>

1. <span data-ttu-id="f9e3c-144">Välj hello timer-utlösta funktion i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-144">In your function app, select hello timer-triggered function.</span></span>
 
3. <span data-ttu-id="f9e3c-145">Lägg till följande sammansättningsreferenser hello överst i hello befintliga Funktionskoden hello:</span><span class="sxs-lookup"><span data-stu-id="f9e3c-145">Add hello following assembly references at hello top of hello existing function code:</span></span>

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. <span data-ttu-id="f9e3c-146">Lägg till följande hello `using` instruktioner toohello funktionen:</span><span class="sxs-lookup"><span data-stu-id="f9e3c-146">Add hello following `using` statements toohello function:</span></span>
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. <span data-ttu-id="f9e3c-147">Ersätta befintliga hello **kör** funktion med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="f9e3c-147">Replace hello existing **Run** function with hello following code:</span></span>
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

    <span data-ttu-id="f9e3c-148">Det här exemplet kommandot uppdaterar hello **Status** kolumnen baserat på hello leverera datum.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-148">This sample command updates hello **Status** column based on hello ship date.</span></span> <span data-ttu-id="f9e3c-149">Det bör uppdatera 32 datarader.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-149">It should update 32 rows of data.</span></span>

5. <span data-ttu-id="f9e3c-150">Klicka på **spara**, titta på hello **loggar** windows hello bredvid fungera körning och Observera hello antalet rader som uppdateras i hello **SalesOrderHeader** tabell.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-150">Click **Save**, watch hello **Logs** windows for hello next function execution, then note hello number of rows updated in hello **SalesOrderHeader** table.</span></span>

    ![Visa hello funktionsloggar.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a><span data-ttu-id="f9e3c-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9e3c-152">Next steps</span></span>

<span data-ttu-id="f9e3c-153">Lär dig sedan hur toouse fungerar med Logic Apps toointegrate med andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-153">Next, learn how toouse Functions with Logic Apps toointegrate with other services.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="f9e3c-154">Skapa en funktion som kan integreras med Logic Apps</span><span class="sxs-lookup"><span data-stu-id="f9e3c-154">Create a function that integrates with Logic Apps</span></span>](functions-twitter-email.md)

<span data-ttu-id="f9e3c-155">Mer information om funktioner finns i hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="f9e3c-155">For more information about Functions, see hello following topics:</span></span>

* [<span data-ttu-id="f9e3c-156">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="f9e3c-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="f9e3c-157">Info för programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="f9e3c-158">Testa Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f9e3c-158">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="f9e3c-159">Beskriver olika verktyg och tekniker för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="f9e3c-159">Describes various tools and techniques for testing your functions.</span></span>  
