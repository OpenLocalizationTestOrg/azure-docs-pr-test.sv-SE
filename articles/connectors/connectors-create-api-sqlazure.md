---
title: aaaAdd hello Azure SQL Database connector i dina Logic Apps | Microsoft Docs
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
ms.openlocfilehash: a9ca0f446d05dc00f310a908eee8d50e41fcd82b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-sql-database-connector"></a><span data-ttu-id="598e5-103">Kom igång med hello Azure SQL Database-koppling</span><span class="sxs-lookup"><span data-stu-id="598e5-103">Get started with hello Azure SQL Database connector</span></span>
<span data-ttu-id="598e5-104">Använd hello Azure SQL Database-anslutningen för att skapa arbetsflöden för din organisation som hanterar data i tabellerna.</span><span class="sxs-lookup"><span data-stu-id="598e5-104">Using hello Azure SQL Database connector, create workflows for your organization that manage data in your tables.</span></span> 

<span data-ttu-id="598e5-105">Med SQL-databas måste du:</span><span class="sxs-lookup"><span data-stu-id="598e5-105">With SQL Database, you:</span></span>

* <span data-ttu-id="598e5-106">Skapa ditt arbetsflöde genom att lägga till en ny databas för kunden tooa kunder eller uppdatera en ordning i en order-databas.</span><span class="sxs-lookup"><span data-stu-id="598e5-106">Build your workflow by adding a new customer tooa customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="598e5-107">Använd åtgärder tooget en rad med data, infoga en ny rad och även ta bort.</span><span class="sxs-lookup"><span data-stu-id="598e5-107">Use actions tooget a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="598e5-108">Till exempel när en post har skapats i Dynamics CRM Online (en utlösare), sedan infoga en rad i en Azure SQL Database (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="598e5-108">For example,  when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Azure SQL Database (an action).</span></span> 

<span data-ttu-id="598e5-109">Det här avsnittet visar hur toouse hello SQL Database-kopplingen i en logikapp och även visar hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="598e5-109">This topic shows you how toouse hello SQL Database connector in a logic app, and also lists hello actions.</span></span>

<span data-ttu-id="598e5-110">toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="598e5-110">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooazure-sql-database"></a><span data-ttu-id="598e5-111">Ansluta tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="598e5-111">Connect tooAzure SQL Database</span></span>
<span data-ttu-id="598e5-112">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="598e5-112">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="598e5-113">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="598e5-113">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="598e5-114">Till exempel tooconnect tooSQL databasen du först skapa en SQL-databas *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="598e5-114">For example, tooconnect tooSQL Database, you first create a SQL Database *connection*.</span></span> <span data-ttu-id="598e5-115">toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess hello-tjänsten som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="598e5-115">toocreate a connection, you enter hello credentials you normally use tooaccess hello service you are connecting to.</span></span> <span data-ttu-id="598e5-116">Ange så SQL autentiseringsuppgifter toocreate hello databasanslutningen i SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="598e5-116">So, in SQL Database, enter your SQL Database credentials toocreate hello connection.</span></span> 

#### <a name="create-hello-connection"></a><span data-ttu-id="598e5-117">Skapa hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="598e5-117">Create hello connection</span></span>
> [!INCLUDE [Create hello connection tooSQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="598e5-118">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="598e5-118">Use a trigger</span></span>
<span data-ttu-id="598e5-119">Den här anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="598e5-119">This connector does not have any triggers.</span></span> <span data-ttu-id="598e5-120">Använda andra utlösare toostart hello logikapp, till exempel en upprepning utlösare, en HTTP-Webhook-utlösare, utlösare som är tillgängliga med övriga kopplingar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="598e5-120">Use other triggers toostart hello logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="598e5-121">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) innehåller ett exempel.</span><span class="sxs-lookup"><span data-stu-id="598e5-121">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="598e5-122">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="598e5-122">Use an action</span></span>
<span data-ttu-id="598e5-123">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="598e5-123">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="598e5-124">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="598e5-124">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="598e5-125">Välj hello plustecken.</span><span class="sxs-lookup"><span data-stu-id="598e5-125">Select hello plus sign.</span></span> <span data-ttu-id="598e5-126">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="598e5-126">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. <span data-ttu-id="598e5-127">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="598e5-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="598e5-128">Skriv ”sql” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.</span><span class="sxs-lookup"><span data-stu-id="598e5-128">In hello text box, type “sql” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. <span data-ttu-id="598e5-129">I vårt exempel väljer **SQL Server - Get-raden**.</span><span class="sxs-lookup"><span data-stu-id="598e5-129">In our example, choose **SQL Server - Get row**.</span></span> <span data-ttu-id="598e5-130">Om det finns redan en anslutning, och välj sedan hello **tabellnamn** från hello nedrullningsbara listan, och ange hello **rad-ID** du vill tooreturn.</span><span class="sxs-lookup"><span data-stu-id="598e5-130">If a connection already exists, then select hello **Table name** from hello drop-down list, and enter hello **Row ID** you want tooreturn.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    <span data-ttu-id="598e5-131">Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="598e5-131">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="598e5-132">[Skapa hello anslutning](connectors-create-api-sqlazure.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="598e5-132">[Create hello connection](connectors-create-api-sqlazure.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="598e5-133">I det här exemplet returnerar vi en rad från en tabell.</span><span class="sxs-lookup"><span data-stu-id="598e5-133">In this example, we return a row from a table.</span></span> <span data-ttu-id="598e5-134">toosee hello data i den här raden Lägg till en annan åtgärd som skapar en fil med hello fält från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="598e5-134">toosee hello data in this row, add another action that creates a file using hello fields from hello table.</span></span> <span data-ttu-id="598e5-135">Till exempel lägga till en åtgärd som OneDrive som använder hello FirstName och LastName fält toocreate en ny fil i hello cloud storage-konto.</span><span class="sxs-lookup"><span data-stu-id="598e5-135">For example, add a OneDrive action that uses hello FirstName and LastName fields toocreate a new file in hello cloud storage account.</span></span> 
   > 
   > 
5. <span data-ttu-id="598e5-136">**Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="598e5-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="598e5-137">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="598e5-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="598e5-138">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="598e5-138">Connector-specific details</span></span>

<span data-ttu-id="598e5-139">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sql/).</span><span class="sxs-lookup"><span data-stu-id="598e5-139">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sql/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="598e5-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="598e5-140">Next steps</span></span>
<span data-ttu-id="598e5-141">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="598e5-141">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="598e5-142">Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="598e5-142">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

