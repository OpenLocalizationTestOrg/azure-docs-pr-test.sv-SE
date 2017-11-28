---
title: "Lagra ostrukturerade data i Azure Cosmos-databasen med hjälp av funktioner"
description: "Lagra ostrukturerade data i Azure Cosmos-databasen med hjälp av funktioner"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, Cosmos DB, dynamisk beräkning, serverlös arkitektur"
ms.assetid: 
ms.service: functions
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7c18676ff94ec7da17094abc5f33fb3c6a79895f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a><span data-ttu-id="4fb01-104">Lagra ostrukturerade data i Azure Cosmos-databasen med hjälp av funktioner</span><span class="sxs-lookup"><span data-stu-id="4fb01-104">Store unstructured data using Azure Functions and Cosmos DB</span></span>

<span data-ttu-id="4fb01-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) är ett väldigt bra sätt att lagra ostrukturerade och JSON-data.</span><span class="sxs-lookup"><span data-stu-id="4fb01-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a great way to store unstructured and JSON data.</span></span> <span data-ttu-id="4fb01-106">I kombination med Azure Functions, gör Cosmos DB lagring av data snabbt och enkelt med mycket mindre kod än vad som krävs för att lagra data i en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="4fb01-106">Combined with Azure Functions, Cosmos DB makes storing data quick and easy with much less code than required for storing data in a relational database.</span></span>

<span data-ttu-id="4fb01-107">I Azure Functions kan du använda indata- och utdatabindningar för att ansluta till data i en extern tjänst från din funktion på ett deklarativt sätt.</span><span class="sxs-lookup"><span data-stu-id="4fb01-107">In Azure Functions, input and output bindings provide a declarative way to connect to external service data from your function.</span></span> <span data-ttu-id="4fb01-108">I det här avsnittet lär du dig hur du uppdaterar en befintlig C#-funktion för att lägga till en utdatabindning som lagrar ostrukturerade data i ett Cosmos DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="4fb01-108">In this topic, learn how to update an existing C# function to add an output binding that stores unstructured data in a Cosmos DB document.</span></span> 

![Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a><span data-ttu-id="4fb01-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4fb01-110">Prerequisites</span></span>

<span data-ttu-id="4fb01-111">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="4fb01-111">To complete this tutorial:</span></span>

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a><span data-ttu-id="4fb01-112">Lägg till en utdatabindning</span><span class="sxs-lookup"><span data-stu-id="4fb01-112">Add an output binding</span></span>

1. <span data-ttu-id="4fb01-113">Expandera både din funktionsapp och din funktion.</span><span class="sxs-lookup"><span data-stu-id="4fb01-113">Expand both your function app and your function.</span></span>

1. <span data-ttu-id="4fb01-114">Välj **Integrera** och **+ Nya utdata**, som du hittar längst upp till höger på sidan.</span><span class="sxs-lookup"><span data-stu-id="4fb01-114">Select **Integrate** and **+ New Output**, which is at the top right of the page.</span></span> <span data-ttu-id="4fb01-115">Välj **Azure Cosmos DB** och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-115">Choose **Azure Cosmos DB**, and click **Select**.</span></span>

    ![Lägg till en Cosmos DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. <span data-ttu-id="4fb01-117">Använd inställningarna för **Azure Cosmos DB-utdata** på det sätt som beskrivs i tabellen:</span><span class="sxs-lookup"><span data-stu-id="4fb01-117">Use the **Azure Cosmos DB output** settings as specified in the table:</span></span> 

    ![Konfigurera Cosmos-DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | <span data-ttu-id="4fb01-119">Inställning</span><span class="sxs-lookup"><span data-stu-id="4fb01-119">Setting</span></span>      | <span data-ttu-id="4fb01-120">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="4fb01-120">Suggested value</span></span>  | <span data-ttu-id="4fb01-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4fb01-121">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="4fb01-122">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="4fb01-122">**Document parameter name**</span></span> | <span data-ttu-id="4fb01-123">taskDocument</span><span class="sxs-lookup"><span data-stu-id="4fb01-123">taskDocument</span></span> | <span data-ttu-id="4fb01-124">Namn som refererar till Cosmos DB-objektet i koden.</span><span class="sxs-lookup"><span data-stu-id="4fb01-124">Name that refers to the Cosmos DB object in code.</span></span> |
    | <span data-ttu-id="4fb01-125">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="4fb01-125">**Database name**</span></span> | <span data-ttu-id="4fb01-126">taskDatabase</span><span class="sxs-lookup"><span data-stu-id="4fb01-126">taskDatabase</span></span> | <span data-ttu-id="4fb01-127">Namnet på databasen där dokumenten ska sparas.</span><span class="sxs-lookup"><span data-stu-id="4fb01-127">Name of database to save documents.</span></span> |
    | <span data-ttu-id="4fb01-128">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="4fb01-128">**Collection name**</span></span> | <span data-ttu-id="4fb01-129">TaskCollection</span><span class="sxs-lookup"><span data-stu-id="4fb01-129">TaskCollection</span></span> | <span data-ttu-id="4fb01-130">Namnet på en Cosmos DB-databassamling.</span><span class="sxs-lookup"><span data-stu-id="4fb01-130">Name of collection of Cosmos DB databases.</span></span> |
    | <span data-ttu-id="4fb01-131">**Om värdet är true skapas Cosmos DB-databasen och -samlingen**</span><span class="sxs-lookup"><span data-stu-id="4fb01-131">**If true, creates the Cosmos DB database and collection**</span></span> | <span data-ttu-id="4fb01-132">Markerad</span><span class="sxs-lookup"><span data-stu-id="4fb01-132">Checked</span></span> | <span data-ttu-id="4fb01-133">Samlingen finns inte redan, så du måste skapa den.</span><span class="sxs-lookup"><span data-stu-id="4fb01-133">The collection doesn't already exist, so create it.</span></span> |

4. <span data-ttu-id="4fb01-134">Välj **Nytt** bredvid etiketten **Cosmos DB document connection** (Cosmos DB-dokumentanslutning) och välj **+ Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-134">Select **New** next to the **Cosmos DB document connection** label, and select **+ Create new**.</span></span> 

5. <span data-ttu-id="4fb01-135">Använd inställningarna för **Nytt konto** på det sätt som beskrivs i tabellen:</span><span class="sxs-lookup"><span data-stu-id="4fb01-135">Use the **New account** settings as specified in the table:</span></span> 

    ![Konfigurera Cosmos-DB-anslutning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | <span data-ttu-id="4fb01-137">Inställning</span><span class="sxs-lookup"><span data-stu-id="4fb01-137">Setting</span></span>      | <span data-ttu-id="4fb01-138">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="4fb01-138">Suggested value</span></span>  | <span data-ttu-id="4fb01-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4fb01-139">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="4fb01-140">**ID**</span><span class="sxs-lookup"><span data-stu-id="4fb01-140">**ID**</span></span> | <span data-ttu-id="4fb01-141">Namnet på databasen</span><span class="sxs-lookup"><span data-stu-id="4fb01-141">Name of database</span></span> | <span data-ttu-id="4fb01-142">Unikt ID för Cosmos-DB-databas</span><span class="sxs-lookup"><span data-stu-id="4fb01-142">Unique ID for the Cosmos DB database</span></span>  |
    | <span data-ttu-id="4fb01-143">**API**</span><span class="sxs-lookup"><span data-stu-id="4fb01-143">**API**</span></span> | <span data-ttu-id="4fb01-144">SQL (DocumentDB)</span><span class="sxs-lookup"><span data-stu-id="4fb01-144">SQL (DocumentDB)</span></span> | <span data-ttu-id="4fb01-145">Välj API:et för dokumentdatabasen.</span><span class="sxs-lookup"><span data-stu-id="4fb01-145">Select the document database API.</span></span>  |
    | <span data-ttu-id="4fb01-146">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="4fb01-146">**Subscription**</span></span> | <span data-ttu-id="4fb01-147">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4fb01-147">Azure Subscription</span></span> | <span data-ttu-id="4fb01-148">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4fb01-148">Azure Subscription</span></span>  |
    | <span data-ttu-id="4fb01-149">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="4fb01-149">**Resource Group**</span></span> | <span data-ttu-id="4fb01-150">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4fb01-150">myResourceGroup</span></span> |  <span data-ttu-id="4fb01-151">Använd den befintliga resursgruppen som innehåller din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="4fb01-151">Use the existing resource group that contains your function app.</span></span> |
    | <span data-ttu-id="4fb01-152">**Plats**</span><span class="sxs-lookup"><span data-stu-id="4fb01-152">**Location**</span></span>  | <span data-ttu-id="4fb01-153">Västeuropa</span><span class="sxs-lookup"><span data-stu-id="4fb01-153">WestEurope</span></span> | <span data-ttu-id="4fb01-154">Välj en plats nära funktionsappen eller nära andra appar som använder de lagrade dokumenten.</span><span class="sxs-lookup"><span data-stu-id="4fb01-154">Select a location near to either your function app or to other apps that use the stored documents.</span></span>  |

6. <span data-ttu-id="4fb01-155">Skapa databasen genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-155">Click **OK** to create the database.</span></span> <span data-ttu-id="4fb01-156">Det kan ta några minuter att skapa databasen.</span><span class="sxs-lookup"><span data-stu-id="4fb01-156">It may take a few minutes to create the database.</span></span> <span data-ttu-id="4fb01-157">När databasen har skapats lagras databasanslutningssträngen som en funktionsappsinställning.</span><span class="sxs-lookup"><span data-stu-id="4fb01-157">After the database is created, the database connection string is stored as a function app setting.</span></span> <span data-ttu-id="4fb01-158">Namnet på den här appinställningen infogas i **Cosmos DB-kontoanslutningen**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-158">The name of this app setting is inserted in **Cosmos DB account connection**.</span></span> 
 
8. <span data-ttu-id="4fb01-159">När du har angett anslutningssträngen väljer du **Spara** för att skapa bindningen.</span><span class="sxs-lookup"><span data-stu-id="4fb01-159">After the connection string is set, select **Save** to create the binding.</span></span>

## <a name="update-the-function-code"></a><span data-ttu-id="4fb01-160">Uppdatera funktionskoden</span><span class="sxs-lookup"><span data-stu-id="4fb01-160">Update the function code</span></span>

<span data-ttu-id="4fb01-161">Ersätt den befintliga C#-funktionskoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="4fb01-161">Replace the existing C# function code with the following code:</span></span>

```csharp
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, out object taskDocument, TraceWriter log)
{
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    string task = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "task", true) == 0)
        .Value;

    string duedate = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "duedate", true) == 0)
        .Value;

    taskDocument = new {
        name = name,
        duedate = duedate.ToString(),
        task = task
    };

    if (name != "" && task != "") {
        return req.CreateResponse(HttpStatusCode.OK);
    }
    else {
        return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}

```
<span data-ttu-id="4fb01-162">Det här kodexemplet läser frågesträngarna i HTTP-begäran och tilldelar dem till fält i `taskDocument`-objektet.</span><span class="sxs-lookup"><span data-stu-id="4fb01-162">This code sample reads the HTTP Request query strings and assigns them to fields in the `taskDocument` object.</span></span> <span data-ttu-id="4fb01-163">`taskDocument`-bindningen skickar objektdata från den här bindningsparametern för lagring i den bundna dokumentdatabasen.</span><span class="sxs-lookup"><span data-stu-id="4fb01-163">The `taskDocument` binding sends the object data from this binding parameter to be stored in the bound document database.</span></span> <span data-ttu-id="4fb01-164">Databasen skapas första gången funktionen körs.</span><span class="sxs-lookup"><span data-stu-id="4fb01-164">The database is created the first time the function runs.</span></span>

## <a name="test-the-function-and-database"></a><span data-ttu-id="4fb01-165">Testa funktionen och databasen</span><span class="sxs-lookup"><span data-stu-id="4fb01-165">Test the function and database</span></span>

1. <span data-ttu-id="4fb01-166">Expandera det högra fönstret och välj **Test**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-166">Expand the right window and select **Test**.</span></span> <span data-ttu-id="4fb01-167">Under **Fråga** klickar du på **+ Lägg till parameter** och lägger till följande parametrar i frågesträngen:</span><span class="sxs-lookup"><span data-stu-id="4fb01-167">Under **Query**, click **+ Add parameter** and add the following parameters to the query string:</span></span>

    + `name`
    + `task`
    + `duedate`

2. <span data-ttu-id="4fb01-168">Klicka på **Kör** och kontrollera att statusen 200 returneras.</span><span class="sxs-lookup"><span data-stu-id="4fb01-168">Click **Run** and verify that a 200 status is returned.</span></span>

    ![Konfigurera Cosmos-DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. <span data-ttu-id="4fb01-170">Expandera ikonfältet till vänster på Azure-portalen, skriv `cosmos` i sökfältet och välj **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-170">On the left side of the Azure portal, expand the icon bar, type `cosmos` in the search field, and select **Azure Cosmos DB**.</span></span>

    ![Sök efter Cosmos DB-tjänsten](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. <span data-ttu-id="4fb01-172">Välj den databas du skapade och välj sedan **Datautforskaren**.</span><span class="sxs-lookup"><span data-stu-id="4fb01-172">Select the database you created, then select **Data Explorer**.</span></span> <span data-ttu-id="4fb01-173">Expandera **Samlingar**-noderna, välj det nya dokumentet och kontrollera att dokumentet innehåller dina frågesträngsvärden, samt en del ytterligare metadata.</span><span class="sxs-lookup"><span data-stu-id="4fb01-173">Expand the **Collections** nodes, select the new document, and confirm that the document contains your query string values, along with some additional metadata.</span></span> 

    ![Kontrollera Cosmos-DB-posten](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

<span data-ttu-id="4fb01-175">Du har lagt till en bindning till din HTTP-utlösare som lagrar ostrukturerade data i en Cosmos DB-databas.</span><span class="sxs-lookup"><span data-stu-id="4fb01-175">You have successfully added a binding to your HTTP trigger that stores unstructured data in a Cosmos DB database.</span></span>

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="4fb01-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fb01-176">Next steps</span></span>

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="4fb01-177">Mer information om att binda till en Cosmos DB-databas finns i [Azure Functions Cosmos DB-bindningar](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="4fb01-177">For more information about binding to a Cosmos DB database, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>
