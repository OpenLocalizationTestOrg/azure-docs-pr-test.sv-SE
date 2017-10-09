---
title: aaaStore Ostrukturerade data med Azure Functions och Cosmos DB
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
ms.openlocfilehash: 48d6899c20d3e6f6b062725fca329972ead3c696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a><span data-ttu-id="b8464-104">Lagra ostrukturerade data i Azure Cosmos-databasen med hjälp av funktioner</span><span class="sxs-lookup"><span data-stu-id="b8464-104">Store unstructured data using Azure Functions and Cosmos DB</span></span>

<span data-ttu-id="b8464-105">[Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/) är ett bra sätt toostore Ostrukturerade och JSON-data.</span><span class="sxs-lookup"><span data-stu-id="b8464-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a great way toostore unstructured and JSON data.</span></span> <span data-ttu-id="b8464-106">I kombination med Azure Functions, gör Cosmos DB lagring av data snabbt och enkelt med mycket mindre kod än vad som krävs för att lagra data i en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="b8464-106">Combined with Azure Functions, Cosmos DB makes storing data quick and easy with much less code than required for storing data in a relational database.</span></span>

<span data-ttu-id="b8464-107">I Azure Functions ange bindningar för inkommande och utgående en deklarativ metod tooconnect tooexternal tjänstdata från din funktion.</span><span class="sxs-lookup"><span data-stu-id="b8464-107">In Azure Functions, input and output bindings provide a declarative way tooconnect tooexternal service data from your function.</span></span> <span data-ttu-id="b8464-108">I det här avsnittet lär du dig hur fungerar tooupdate en befintlig C# tooadd en bindning för utdata som lagrar Ostrukturerade data i ett Cosmos-DB-dokument.</span><span class="sxs-lookup"><span data-stu-id="b8464-108">In this topic, learn how tooupdate an existing C# function tooadd an output binding that stores unstructured data in a Cosmos DB document.</span></span> 

![Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a><span data-ttu-id="b8464-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b8464-110">Prerequisites</span></span>

<span data-ttu-id="b8464-111">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="b8464-111">toocomplete this tutorial:</span></span>

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a><span data-ttu-id="b8464-112">Lägg till en utdatabindning</span><span class="sxs-lookup"><span data-stu-id="b8464-112">Add an output binding</span></span>

1. <span data-ttu-id="b8464-113">Expandera både din funktionsapp och din funktion.</span><span class="sxs-lookup"><span data-stu-id="b8464-113">Expand both your function app and your function.</span></span>

1. <span data-ttu-id="b8464-114">Välj **integrera** och **+ nya utdata**, som är hello uppifrån hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="b8464-114">Select **Integrate** and **+ New Output**, which is at hello top right of hello page.</span></span> <span data-ttu-id="b8464-115">Välj **Azure Cosmos DB** och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b8464-115">Choose **Azure Cosmos DB**, and click **Select**.</span></span>

    ![Lägg till en Cosmos DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. <span data-ttu-id="b8464-117">Använd hello **Azure Cosmos DB utdata** inställningar som anges i hello tabell:</span><span class="sxs-lookup"><span data-stu-id="b8464-117">Use hello **Azure Cosmos DB output** settings as specified in hello table:</span></span> 

    ![Konfigurera Cosmos-DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | <span data-ttu-id="b8464-119">Inställning</span><span class="sxs-lookup"><span data-stu-id="b8464-119">Setting</span></span>      | <span data-ttu-id="b8464-120">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b8464-120">Suggested value</span></span>  | <span data-ttu-id="b8464-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b8464-121">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="b8464-122">**Dokumentparameternamn**</span><span class="sxs-lookup"><span data-stu-id="b8464-122">**Document parameter name**</span></span> | <span data-ttu-id="b8464-123">taskDocument</span><span class="sxs-lookup"><span data-stu-id="b8464-123">taskDocument</span></span> | <span data-ttu-id="b8464-124">Namnet som refererar toohello Cosmos DB objektet i kod.</span><span class="sxs-lookup"><span data-stu-id="b8464-124">Name that refers toohello Cosmos DB object in code.</span></span> |
    | <span data-ttu-id="b8464-125">**Databasnamn**</span><span class="sxs-lookup"><span data-stu-id="b8464-125">**Database name**</span></span> | <span data-ttu-id="b8464-126">taskDatabase</span><span class="sxs-lookup"><span data-stu-id="b8464-126">taskDatabase</span></span> | <span data-ttu-id="b8464-127">Namnet på databasen toosave dokument.</span><span class="sxs-lookup"><span data-stu-id="b8464-127">Name of database toosave documents.</span></span> |
    | <span data-ttu-id="b8464-128">**Samlingsnamn**</span><span class="sxs-lookup"><span data-stu-id="b8464-128">**Collection name**</span></span> | <span data-ttu-id="b8464-129">TaskCollection</span><span class="sxs-lookup"><span data-stu-id="b8464-129">TaskCollection</span></span> | <span data-ttu-id="b8464-130">Namnet på en Cosmos DB-databassamling.</span><span class="sxs-lookup"><span data-stu-id="b8464-130">Name of collection of Cosmos DB databases.</span></span> |
    | <span data-ttu-id="b8464-131">**Om värdet är true, skapas hello Cosmos-DB-databas och samling**</span><span class="sxs-lookup"><span data-stu-id="b8464-131">**If true, creates hello Cosmos DB database and collection**</span></span> | <span data-ttu-id="b8464-132">Markerad</span><span class="sxs-lookup"><span data-stu-id="b8464-132">Checked</span></span> | <span data-ttu-id="b8464-133">hello samlingen inte redan finns, så du måste skapa den.</span><span class="sxs-lookup"><span data-stu-id="b8464-133">hello collection doesn't already exist, so create it.</span></span> |

4. <span data-ttu-id="b8464-134">Välj **ny** nästa toohello **Cosmos dokumentet databasanslutningen** och väljer **+ Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="b8464-134">Select **New** next toohello **Cosmos DB document connection** label, and select **+ Create new**.</span></span> 

5. <span data-ttu-id="b8464-135">Använd hello **nytt konto** inställningar som anges i hello tabell:</span><span class="sxs-lookup"><span data-stu-id="b8464-135">Use hello **New account** settings as specified in hello table:</span></span> 

    ![Konfigurera Cosmos-DB-anslutning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | <span data-ttu-id="b8464-137">Inställning</span><span class="sxs-lookup"><span data-stu-id="b8464-137">Setting</span></span>      | <span data-ttu-id="b8464-138">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="b8464-138">Suggested value</span></span>  | <span data-ttu-id="b8464-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b8464-139">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="b8464-140">**ID**</span><span class="sxs-lookup"><span data-stu-id="b8464-140">**ID**</span></span> | <span data-ttu-id="b8464-141">Namnet på databasen</span><span class="sxs-lookup"><span data-stu-id="b8464-141">Name of database</span></span> | <span data-ttu-id="b8464-142">Unikt ID för hello Cosmos-DB-databas</span><span class="sxs-lookup"><span data-stu-id="b8464-142">Unique ID for hello Cosmos DB database</span></span>  |
    | <span data-ttu-id="b8464-143">**API**</span><span class="sxs-lookup"><span data-stu-id="b8464-143">**API**</span></span> | <span data-ttu-id="b8464-144">SQL (DocumentDB)</span><span class="sxs-lookup"><span data-stu-id="b8464-144">SQL (DocumentDB)</span></span> | <span data-ttu-id="b8464-145">Välj hello dokumentet databasen API.</span><span class="sxs-lookup"><span data-stu-id="b8464-145">Select hello document database API.</span></span>  |
    | <span data-ttu-id="b8464-146">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="b8464-146">**Subscription**</span></span> | <span data-ttu-id="b8464-147">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b8464-147">Azure Subscription</span></span> | <span data-ttu-id="b8464-148">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b8464-148">Azure Subscription</span></span>  |
    | <span data-ttu-id="b8464-149">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="b8464-149">**Resource Group**</span></span> | <span data-ttu-id="b8464-150">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b8464-150">myResourceGroup</span></span> |  <span data-ttu-id="b8464-151">Använd hello befintlig resursgrupp som innehåller funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="b8464-151">Use hello existing resource group that contains your function app.</span></span> |
    | <span data-ttu-id="b8464-152">**Plats**</span><span class="sxs-lookup"><span data-stu-id="b8464-152">**Location**</span></span>  | <span data-ttu-id="b8464-153">Västeuropa</span><span class="sxs-lookup"><span data-stu-id="b8464-153">WestEurope</span></span> | <span data-ttu-id="b8464-154">Välj en plats nära tooeither funktionsapp eller tooother appar som använder hello lagrade dokument.</span><span class="sxs-lookup"><span data-stu-id="b8464-154">Select a location near tooeither your function app or tooother apps that use hello stored documents.</span></span>  |

6. <span data-ttu-id="b8464-155">Klicka på **OK** toocreate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="b8464-155">Click **OK** toocreate hello database.</span></span> <span data-ttu-id="b8464-156">Det kan ta några minuter toocreate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="b8464-156">It may take a few minutes toocreate hello database.</span></span> <span data-ttu-id="b8464-157">När hello-databasen har skapats lagras hello databasanslutningssträng som en funktionen app-inställning.</span><span class="sxs-lookup"><span data-stu-id="b8464-157">After hello database is created, hello database connection string is stored as a function app setting.</span></span> <span data-ttu-id="b8464-158">hello namnet på den här appinställningen infogas i **Cosmos konto databasanslutningen**.</span><span class="sxs-lookup"><span data-stu-id="b8464-158">hello name of this app setting is inserted in **Cosmos DB account connection**.</span></span> 
 
8. <span data-ttu-id="b8464-159">När hello anslutningssträngen har angetts, Välj **spara** toocreate hello bindning.</span><span class="sxs-lookup"><span data-stu-id="b8464-159">After hello connection string is set, select **Save** toocreate hello binding.</span></span>

## <a name="update-hello-function-code"></a><span data-ttu-id="b8464-160">Uppdatera Funktionskoden hello</span><span class="sxs-lookup"><span data-stu-id="b8464-160">Update hello function code</span></span>

<span data-ttu-id="b8464-161">Ersätt hello befintliga C# Funktionskoden med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b8464-161">Replace hello existing C# function code with hello following code:</span></span>

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
<span data-ttu-id="b8464-162">Det här kodexemplet läser hello HTTP-begäran frågesträngar och tilldelar dem toofields i hello `taskDocument` objekt.</span><span class="sxs-lookup"><span data-stu-id="b8464-162">This code sample reads hello HTTP Request query strings and assigns them toofields in hello `taskDocument` object.</span></span> <span data-ttu-id="b8464-163">Hej `taskDocument` bindning skickar hello objektdata från den här bindningen parametern toobe som lagras i hello bundet dokument-databasen.</span><span class="sxs-lookup"><span data-stu-id="b8464-163">hello `taskDocument` binding sends hello object data from this binding parameter toobe stored in hello bound document database.</span></span> <span data-ttu-id="b8464-164">hello-databas skapas hello första gången Hej funktionen körs.</span><span class="sxs-lookup"><span data-stu-id="b8464-164">hello database is created hello first time hello function runs.</span></span>

## <a name="test-hello-function-and-database"></a><span data-ttu-id="b8464-165">Testa hello funktionen och databas</span><span class="sxs-lookup"><span data-stu-id="b8464-165">Test hello function and database</span></span>

1. <span data-ttu-id="b8464-166">Expandera hello högra fönstret och välj **Test**.</span><span class="sxs-lookup"><span data-stu-id="b8464-166">Expand hello right window and select **Test**.</span></span> <span data-ttu-id="b8464-167">Under **frågan**, klickar du på **+ Lägg till parametern** och Lägg till följande parametrar toohello frågesträngen hello:</span><span class="sxs-lookup"><span data-stu-id="b8464-167">Under **Query**, click **+ Add parameter** and add hello following parameters toohello query string:</span></span>

    + `name`
    + `task`
    + `duedate`

2. <span data-ttu-id="b8464-168">Klicka på **Kör** och kontrollera att statusen 200 returneras.</span><span class="sxs-lookup"><span data-stu-id="b8464-168">Click **Run** and verify that a 200 status is returned.</span></span>

    ![Konfigurera Cosmos-DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. <span data-ttu-id="b8464-170">Expandera hello ikonen stapel på vänster sida av hello Azure-portalen hello, typen `cosmos` i hello söka och markerar **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="b8464-170">On hello left side of hello Azure portal, expand hello icon bar, type `cosmos` in hello search field, and select **Azure Cosmos DB**.</span></span>

    ![Sök efter hello Cosmos-DB-tjänsten](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. <span data-ttu-id="b8464-172">Välj hello-databasen som du skapade, välj sedan **Data Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b8464-172">Select hello database you created, then select **Data Explorer**.</span></span> <span data-ttu-id="b8464-173">Expandera hello **samlingar** noder, Välj hello nytt dokument och bekräfta att hello-dokumentet innehåller din fråga strängvärden, tillsammans med vissa ytterligare metadata.</span><span class="sxs-lookup"><span data-stu-id="b8464-173">Expand hello **Collections** nodes, select hello new document, and confirm that hello document contains your query string values, along with some additional metadata.</span></span> 

    ![Kontrollera Cosmos-DB-posten](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

<span data-ttu-id="b8464-175">Du har lagt en bindning tooyour HTTP-utlösare som lagrar Ostrukturerade data i en Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="b8464-175">You have successfully added a binding tooyour HTTP trigger that stores unstructured data in a Cosmos DB database.</span></span>

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="b8464-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8464-176">Next steps</span></span>

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="b8464-177">Mer information om bindningen tooa Cosmos-DB-databasen finns [Azure Functions Cosmos DB bindningar](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="b8464-177">For more information about binding tooa Cosmos DB database, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>
