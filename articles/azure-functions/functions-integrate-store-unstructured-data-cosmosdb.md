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
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a>Lagra ostrukturerade data i Azure Cosmos-databasen med hjälp av funktioner

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) är ett väldigt bra sätt att lagra ostrukturerade och JSON-data. I kombination med Azure Functions, gör Cosmos DB lagring av data snabbt och enkelt med mycket mindre kod än vad som krävs för att lagra data i en relationsdatabas.

I Azure Functions kan du använda indata- och utdatabindningar för att ansluta till data i en extern tjänst från din funktion på ett deklarativt sätt. I det här avsnittet lär du dig hur du uppdaterar en befintlig C#-funktion för att lägga till en utdatabindning som lagrar ostrukturerade data i ett Cosmos DB-dokument. 

![Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a>Krav

För att slutföra den här kursen behöver du:

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a>Lägg till en utdatabindning

1. Expandera både din funktionsapp och din funktion.

1. Välj **Integrera** och **+ Nya utdata**, som du hittar längst upp till höger på sidan. Välj **Azure Cosmos DB** och klicka på **Välj**.

    ![Lägg till en Cosmos DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. Använd inställningarna för **Azure Cosmos DB-utdata** på det sätt som beskrivs i tabellen: 

    ![Konfigurera Cosmos-DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | Inställning      | Föreslaget värde  | Beskrivning                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Dokumentparameternamn** | taskDocument | Namn som refererar till Cosmos DB-objektet i koden. |
    | **Databasnamn** | taskDatabase | Namnet på databasen där dokumenten ska sparas. |
    | **Samlingsnamn** | TaskCollection | Namnet på en Cosmos DB-databassamling. |
    | **Om värdet är true skapas Cosmos DB-databasen och -samlingen** | Markerad | Samlingen finns inte redan, så du måste skapa den. |

4. Välj **Nytt** bredvid etiketten **Cosmos DB document connection** (Cosmos DB-dokumentanslutning) och välj **+ Skapa nytt**. 

5. Använd inställningarna för **Nytt konto** på det sätt som beskrivs i tabellen: 

    ![Konfigurera Cosmos-DB-anslutning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | Inställning      | Föreslaget värde  | Beskrivning                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **ID** | Namnet på databasen | Unikt ID för Cosmos-DB-databas  |
    | **API** | SQL (DocumentDB) | Välj API:et för dokumentdatabasen.  |
    | **Prenumeration** | Azure-prenumeration | Azure-prenumeration  |
    | **Resursgrupp** | myResourceGroup |  Använd den befintliga resursgruppen som innehåller din funktionsapp. |
    | **Plats**  | Västeuropa | Välj en plats nära funktionsappen eller nära andra appar som använder de lagrade dokumenten.  |

6. Skapa databasen genom att klicka på **OK**. Det kan ta några minuter att skapa databasen. När databasen har skapats lagras databasanslutningssträngen som en funktionsappsinställning. Namnet på den här appinställningen infogas i **Cosmos DB-kontoanslutningen**. 
 
8. När du har angett anslutningssträngen väljer du **Spara** för att skapa bindningen.

## <a name="update-the-function-code"></a>Uppdatera funktionskoden

Ersätt den befintliga C#-funktionskoden med följande kod:

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
Det här kodexemplet läser frågesträngarna i HTTP-begäran och tilldelar dem till fält i `taskDocument`-objektet. `taskDocument`-bindningen skickar objektdata från den här bindningsparametern för lagring i den bundna dokumentdatabasen. Databasen skapas första gången funktionen körs.

## <a name="test-the-function-and-database"></a>Testa funktionen och databasen

1. Expandera det högra fönstret och välj **Test**. Under **Fråga** klickar du på **+ Lägg till parameter** och lägger till följande parametrar i frågesträngen:

    + `name`
    + `task`
    + `duedate`

2. Klicka på **Kör** och kontrollera att statusen 200 returneras.

    ![Konfigurera Cosmos-DB-utdatabindning](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. Expandera ikonfältet till vänster på Azure-portalen, skriv `cosmos` i sökfältet och välj **Azure Cosmos DB**.

    ![Sök efter Cosmos DB-tjänsten](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. Välj den databas du skapade och välj sedan **Datautforskaren**. Expandera **Samlingar**-noderna, välj det nya dokumentet och kontrollera att dokumentet innehåller dina frågesträngsvärden, samt en del ytterligare metadata. 

    ![Kontrollera Cosmos-DB-posten](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

Du har lagt till en bindning till din HTTP-utlösare som lagrar ostrukturerade data i en Cosmos DB-databas.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

Mer information om att binda till en Cosmos DB-databas finns i [Azure Functions Cosmos DB-bindningar](functions-bindings-documentdb.md).
