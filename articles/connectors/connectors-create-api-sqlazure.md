---
title: "Lägg till Azure SQL Database-koppling i dina Logic Apps | Microsoft Docs"
description: "Översikt över Azure SQL Database-anslutningen med REST API-parametrar"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a3d5cb909dbfcb00f3fbfa0165bb6cd58eb18688
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-azure-sql-database-connector"></a><span data-ttu-id="51425-103">Kom igång med Azure SQL Database-koppling</span><span class="sxs-lookup"><span data-stu-id="51425-103">Get started with the Azure SQL Database connector</span></span>
<span data-ttu-id="51425-104">Med Azure SQL Database-kopplingen kan skapa arbetsflöden för din organisation som hanterar data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="51425-104">Using the Azure SQL Database connector, create workflows for your organization that manage data in your tables.</span></span> 

<span data-ttu-id="51425-105">Med SQL-databas måste du:</span><span class="sxs-lookup"><span data-stu-id="51425-105">With SQL Database, you:</span></span>

* <span data-ttu-id="51425-106">Skapa ditt arbetsflöde genom att lägga till en ny kund till en databas för kunder eller uppdatera en ordning i en order-databas.</span><span class="sxs-lookup"><span data-stu-id="51425-106">Build your workflow by adding a new customer to a customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="51425-107">Använd åtgärder för att hämta en rad med data, infoga en ny rad och även ta bort.</span><span class="sxs-lookup"><span data-stu-id="51425-107">Use actions to get a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="51425-108">Till exempel när en post har skapats i Dynamics CRM Online (en utlösare), sedan infoga en rad i en Azure SQL Database (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="51425-108">For example,  when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Azure SQL Database (an action).</span></span> 

<span data-ttu-id="51425-109">Det här avsnittet beskrivs hur du använder SQL Database-anslutningen i en logikapp och visar även åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="51425-109">This topic shows you how to use the SQL Database connector in a logic app, and also lists the actions.</span></span>

<span data-ttu-id="51425-110">Läs mer om Logic Apps i [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="51425-110">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-azure-sql-database"></a><span data-ttu-id="51425-111">Anslut till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="51425-111">Connect to Azure SQL Database</span></span>
<span data-ttu-id="51425-112">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51425-112">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="51425-113">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="51425-113">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="51425-114">Till exempel för att ansluta till SQL-databas måste du först skapa en SQL-databas *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="51425-114">For example, to connect to SQL Database, you first create a SQL Database *connection*.</span></span> <span data-ttu-id="51425-115">Om du vill skapa en anslutning kan du ange de autentiseringsuppgifter som du vanligtvis använder för att få åtkomst till tjänsten som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="51425-115">To create a connection, you enter the credentials you normally use to access the service you are connecting to.</span></span> <span data-ttu-id="51425-116">Så ange dina autentiseringsuppgifter för SQL-databas för att skapa anslutningen i SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="51425-116">So, in SQL Database, enter your SQL Database credentials to create the connection.</span></span> 

#### <a name="create-the-connection"></a><span data-ttu-id="51425-117">Skapa anslutningen</span><span class="sxs-lookup"><span data-stu-id="51425-117">Create the connection</span></span>
> [!INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="51425-118">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="51425-118">Use a trigger</span></span>
<span data-ttu-id="51425-119">Den här anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="51425-119">This connector does not have any triggers.</span></span> <span data-ttu-id="51425-120">Använd andra utlösare för att starta logikapp, till exempel en upprepning utlösare, en HTTP-Webhook-utlösare, utlösare som är tillgängliga med övriga kopplingar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="51425-120">Use other triggers to start the logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="51425-121">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) innehåller ett exempel.</span><span class="sxs-lookup"><span data-stu-id="51425-121">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="51425-122">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="51425-122">Use an action</span></span>
<span data-ttu-id="51425-123">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="51425-123">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="51425-124">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="51425-124">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="51425-125">Klicka på plustecknet.</span><span class="sxs-lookup"><span data-stu-id="51425-125">Select the plus sign.</span></span> <span data-ttu-id="51425-126">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av de **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="51425-126">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. <span data-ttu-id="51425-127">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="51425-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="51425-128">I rutan skriver du ”sql” om du vill hämta en lista över alla tillgängliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="51425-128">In the text box, type “sql” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. <span data-ttu-id="51425-129">I vårt exempel väljer **SQL Server - Get-raden**.</span><span class="sxs-lookup"><span data-stu-id="51425-129">In our example, choose **SQL Server - Get row**.</span></span> <span data-ttu-id="51425-130">Om det finns redan en anslutning, väljer du den **tabellnamn** från nedrullningsbara listan, och ange den **rad-ID** du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="51425-130">If a connection already exists, then select the **Table name** from the drop-down list, and enter the **Row ID** you want to return.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    <span data-ttu-id="51425-131">Om du uppmanas att ange anslutningsinformation anger du information för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="51425-131">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="51425-132">[Skapa anslutningen](connectors-create-api-sqlazure.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="51425-132">[Create the connection](connectors-create-api-sqlazure.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="51425-133">I det här exemplet returnerar vi en rad från en tabell.</span><span class="sxs-lookup"><span data-stu-id="51425-133">In this example, we return a row from a table.</span></span> <span data-ttu-id="51425-134">Lägg till en annan åtgärd som skapar en fil med hjälp av fälten från tabellen om du vill se data i den här raden.</span><span class="sxs-lookup"><span data-stu-id="51425-134">To see the data in this row, add another action that creates a file using the fields from the table.</span></span> <span data-ttu-id="51425-135">Till exempel lägga till en OneDrive-åtgärd som fälten Förnamn och efternamn används för att skapa en ny fil i molnet storage-konto.</span><span class="sxs-lookup"><span data-stu-id="51425-135">For example, add a OneDrive action that uses the FirstName and LastName fields to create a new file in the cloud storage account.</span></span> 
   > 
   > 
5. <span data-ttu-id="51425-136">**Spara** dina ändringar (övre vänstra hörnet i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="51425-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="51425-137">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="51425-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="51425-138">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="51425-138">Connector-specific details</span></span>

<span data-ttu-id="51425-139">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/sql/).</span><span class="sxs-lookup"><span data-stu-id="51425-139">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sql/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="51425-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51425-140">Next steps</span></span>
<span data-ttu-id="51425-141">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="51425-141">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="51425-142">Utforska andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="51425-142">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

