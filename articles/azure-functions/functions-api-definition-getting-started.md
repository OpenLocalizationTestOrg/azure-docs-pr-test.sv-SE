---
title: "Komma igång med OpenAPI Metadata i Azure Functions | Microsoft Docs"
description: "Komma igång med OpenAPI stöd i Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: 9b861aacf31e17866293732dc2323f56014c1877
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a><span data-ttu-id="e1c73-103">Skapa OpenAPI 2.0 (Swagger) Metadata för en Funktionsapp (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="e1c73-103">Creating OpenAPI 2.0 (Swagger) Metadata for a Function App (Preview)</span></span>

<span data-ttu-id="e1c73-104">Det här dokumentet hjälper dig att steg för steg att skapa en OpenAPI Definition för ett enkelt API som finns på Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e1c73-104">This document guides you through the step by step process of creating an OpenAPI Definition for a simple API hosted on Azure Functions.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a><span data-ttu-id="e1c73-105">Vad är OpenAPI (Swagger)?</span><span class="sxs-lookup"><span data-stu-id="e1c73-105">What is OpenAPI (Swagger)?</span></span>
<span data-ttu-id="e1c73-106">[Swagger-Metadata](http://swagger.io/) är en fil som definierar funktioner och -lägena av din API och gör en funktion som värd för en REST-API för att användas av en mängd annan programvara.</span><span class="sxs-lookup"><span data-stu-id="e1c73-106">[Swagger Metadata](http://swagger.io/) is a file that defines the functionality and operating modes of your API, and allows a function hosting a REST API to be consumed by a wide variety of other software.</span></span> <span data-ttu-id="e1c73-107">Microsoft-erbjudanden som PowerApps och [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), samt 3 part developer tooling som [Postman](https://www.getpostman.com/docs/importing_swagger) och [många fler paket](http://swagger.io/tools/) alla Tillåt ditt API för att användas med en Swagger-definition.</span><span class="sxs-lookup"><span data-stu-id="e1c73-107">Microsoft offerings like PowerApps and [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), as well as 3rd party developer tooling like [Postman](https://www.getpostman.com/docs/importing_swagger) and [many more packages](http://swagger.io/tools/) all allow your API to be consumed with a Swagger definition.</span></span>

## <span data-ttu-id="e1c73-108"><a name="prepare-function"></a>Skapa en funktion med ett enkelt API</span><span class="sxs-lookup"><span data-stu-id="e1c73-108"><a name="prepare-function"></a>Creating a Function with a simple API</span></span>
  <span data-ttu-id="e1c73-109">Om du vill skapa en OpenAPI definition måste vi först skapa en funktion med ett enkelt API.</span><span class="sxs-lookup"><span data-stu-id="e1c73-109">To create an OpenAPI definition, we first need to create a Function with a simple API.</span></span> <span data-ttu-id="e1c73-110">Om du redan har en API som finns på en Funktionsapp kan gå du direkt till nästa avsnitt</span><span class="sxs-lookup"><span data-stu-id="e1c73-110">If you already have an API hosted on a Function App, you can skip straight to the next section</span></span>
1. <span data-ttu-id="e1c73-111">Skapa en ny Funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="e1c73-111">Create a new Function App.</span></span>
    1. <span data-ttu-id="e1c73-112">[Azure-portalen](https://portal.azure.com)  >  `+ New` > Sök efter ”Funktionsapp”</span><span class="sxs-lookup"><span data-stu-id="e1c73-112">[Azure portal](https://portal.azure.com) > `+ New` > Search for "Function App"</span></span>
1. <span data-ttu-id="e1c73-113">Skapa en ny funktion i din nya Funktionsapp gäller HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="e1c73-113">Create a new HTTP trigger function inside your new Function App</span></span>
    1. <span data-ttu-id="e1c73-114">Funktionen är i förväg med koden som definierar ett väldigt enkelt REST-API.</span><span class="sxs-lookup"><span data-stu-id="e1c73-114">Your function is pre-populated with code defining a very simple REST API.</span></span>
    1. <span data-ttu-id="e1c73-115">Valfri sträng som skickas till funktionen som en frågeparameter eller i meddelandetexten returneras som ”Hello {indata}”</span><span class="sxs-lookup"><span data-stu-id="e1c73-115">Any string passed to the Function as a query parameter or in the body is returned as "Hello {input}"</span></span>
1. <span data-ttu-id="e1c73-116">Gå till den `Integrate` för den nya funktionen för HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="e1c73-116">Go to the `Integrate` tab of your new HTTP Trigger function</span></span>
    1. <span data-ttu-id="e1c73-117">Växla `Allowed HTTP methods` till`Selected methods`</span><span class="sxs-lookup"><span data-stu-id="e1c73-117">Toggle `Allowed HTTP methods` to `Selected methods`</span></span>
    1. <span data-ttu-id="e1c73-118">I `Selected HTTP methods` avmarkera alla verb utom POST.</span><span class="sxs-lookup"><span data-stu-id="e1c73-118">In `Selected HTTP methods` uncheck every verb except POST.</span></span>
    <span data-ttu-id="e1c73-119">![Valda http-metoder](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-119">![Selected HTTP Methods](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span></span>
    1. <span data-ttu-id="e1c73-120">Det här steget förenklar API-definition vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="e1c73-120">This step will simplify your API definition later on.</span></span>

## <span data-ttu-id="e1c73-121"><a name="enable"></a>Aktivera stöd för API-Definition</span><span class="sxs-lookup"><span data-stu-id="e1c73-121"><a name="enable"></a>Enabling API Definition Support</span></span>
1. <span data-ttu-id="e1c73-122">Gå till `your function name`  >  `Platform Features`  >  `API Definition` 
 ![fliken Definition](./media/functions-api-definition-getting-started/definitiontab.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-122">Navigate to `your function name` > `Platform Features` > `API Definition`
![Definition Tab](./media/functions-api-definition-getting-started/definitiontab.png)</span></span>
1. <span data-ttu-id="e1c73-123">Ange `API Definition Source` till`Function (preview)`</span><span class="sxs-lookup"><span data-stu-id="e1c73-123">Set `API Definition Source` to `Function (preview)`</span></span>
    1. <span data-ttu-id="e1c73-124">Det här steget gör det möjligt för en uppsättning OpenAPI alternativ för funktionen appen, inklusive en slutpunkt som värd för en OpenAPI-fil från din Funktionsapp domän, en infogad kopia av den [OpenAPI Editor](http://editor.swagger.io), och en Snabbstart definition generator.</span><span class="sxs-lookup"><span data-stu-id="e1c73-124">This step enables a suite of OpenAPI options for your Function App, including an endpoint to host an OpenAPI file from your Function App's domain, an inline copy of the [OpenAPI Editor](http://editor.swagger.io), and a quickstart definition generator.</span></span>
<span data-ttu-id="e1c73-125">![Aktiverade Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-125">![Enabled Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span></span>

## <span data-ttu-id="e1c73-126"><a name="create-definition"></a>Skapa din API-Definition från en mall</span><span class="sxs-lookup"><span data-stu-id="e1c73-126"><a name="create-definition"></a>Creating your API Definition from a template</span></span>
1. <span data-ttu-id="e1c73-127">Klicka på `Generate API Definition template`</span><span class="sxs-lookup"><span data-stu-id="e1c73-127">Click `Generate API Definition template`</span></span>
    1. <span data-ttu-id="e1c73-128">Det här steget söker igenom appen funktionen för HTTP-utlösaren funktioner och använder informationen i functions.json för att generera ett OpenAPI dokument.</span><span class="sxs-lookup"><span data-stu-id="e1c73-128">This step scans your Function App for HTTP Trigger functions and uses the info in functions.json to generate an OpenAPI document.</span></span>
1. <span data-ttu-id="e1c73-129">Lägg till ett objekt för åtgärden att`paths: /api/yourfunctionroute post:`</span><span class="sxs-lookup"><span data-stu-id="e1c73-129">Add an operation object to `paths: /api/yourfunctionroute post:`</span></span>
    1. <span data-ttu-id="e1c73-130">Snabbstart OpenAPI dokumentet är en sammanfattning av en fullständig OpenAPI-dokument. Det krävs mer metadata för att vara en fullständig OpenAPI definition, till exempel åtgärden objekt och svar mallar.</span><span class="sxs-lookup"><span data-stu-id="e1c73-130">The quickstart OpenAPI document is an outline of a full OpenAPI doc. It requires more metadata to be a full OpenAPI definition, such as operation objects and response templates.</span></span>
    1. <span data-ttu-id="e1c73-131">Exempel åtgärden objekt nedan har en fylld ger/förbrukar avsnitt, ett parameter-objekt och ett svarsobjekt.</span><span class="sxs-lookup"><span data-stu-id="e1c73-131">The sample operation object below has a filled out produces/consumes section, a parameter object, and a response object.</span></span>
    
    ```yaml
      post:
        operationId: /api/yourfunctionroute/post
        consumes: [application/json]
        produces: [application/json]
        parameters:
          - name: name
            in: body
            description: Your name
            x-ms-summary: Your name
            required: true
            schema:
              type: object
              properties:
                name:
                  type: string
        description: >-
          Replace with Operation Object
          #http://swagger.io/specification/#operationObject
        responses:
          '200':
            description: A Greeting
            x-ms-summary: Your name
            schema:
              type: string
        security:
          - apikeyQuery: []
    ```
    
    > [!NOTE]
    >  <span data-ttu-id="e1c73-132">x-ms-sammanfattning ger ett namn i Logikappar, flödesreglering och PowerApps.</span><span class="sxs-lookup"><span data-stu-id="e1c73-132">x-ms-summary provides a display name in Logic Apps, Flow, and PowerApps.</span></span>
    >
    > <span data-ttu-id="e1c73-133">Checka ut [anpassa din Swagger-definition för PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="e1c73-133">Check out [customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) to learn more.</span></span>

1. <span data-ttu-id="e1c73-134">Klicka på `save` spara dina ändringar ![definitionen i mallen för att lägga till](./media/functions-api-definition-getting-started/addingtemplate.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-134">Click `save` to save your changes ![Adding Template Definition](./media/functions-api-definition-getting-started/addingtemplate.png)</span></span>

## <span data-ttu-id="e1c73-135"><a name="use-definition"></a>Med API-Definition</span><span class="sxs-lookup"><span data-stu-id="e1c73-135"><a name="use-definition"></a>Using Your API Definition</span></span>
1. <span data-ttu-id="e1c73-136">Kopiera ditt `API definition URL` och klistra in den i en ny webbläsarflik att visa dokumentet rådata OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="e1c73-136">Copy your `API definition URL` and paste it into a new browser tab to view your raw OpenAPI document.</span></span>
1. <span data-ttu-id="e1c73-137">Du kan importera dokumentet OpenAPI till valfritt antal verktyg för att testa och integration med URL: en.</span><span class="sxs-lookup"><span data-stu-id="e1c73-137">You can import your OpenAPI document to any number of tools for testing and integration using that URL.</span></span>
    1. <span data-ttu-id="e1c73-138">Många Azure-resurser kan importera OpenAPI dokumentet med hjälp av URL för API-Definition som sparas i programinställningarna funktionen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e1c73-138">Many Azure resources are able to automatically import your OpenAPI doc using the API Definition URL that is saved in your Function application settings.</span></span> <span data-ttu-id="e1c73-139">Som en del av den `Function` källa för API-definition, vi uppdatera den url du.</span><span class="sxs-lookup"><span data-stu-id="e1c73-139">As a part of the `Function` API definition source, we update that url for you.</span></span>


## <span data-ttu-id="e1c73-140"><a name="test-definition"></a>Använd Swagger-användargränssnitt för att testa din API-definition</span><span class="sxs-lookup"><span data-stu-id="e1c73-140"><a name="test-definition"></a>Using the Swagger UI to test your API definition</span></span>
<span data-ttu-id="e1c73-141">Det testar verktyg som är inbyggd i vyn Användargränssnittet i Redigeraren för inbäddad API-definition.</span><span class="sxs-lookup"><span data-stu-id="e1c73-141">There are testing tools built in to the UI view of the imbedded API definition editor.</span></span> <span data-ttu-id="e1c73-142">Lägg till din API-nyckel och sedan använda den `Try this operation` knappen under varje metod.</span><span class="sxs-lookup"><span data-stu-id="e1c73-142">Add your API key, and then use the `Try this operation` button under each method.</span></span> <span data-ttu-id="e1c73-143">Verktyget använder API-Definition för att formatera begäranden och lyckade svar visar att din definition är korrekt.</span><span class="sxs-lookup"><span data-stu-id="e1c73-143">The tool will use your API Definition to format the requests and successful responses will indicate that your definition is correct.</span></span>

### <a name="steps"></a><span data-ttu-id="e1c73-144">Så här:</span><span class="sxs-lookup"><span data-stu-id="e1c73-144">Steps:</span></span>

1. <span data-ttu-id="e1c73-145">Kopiera din funktion API-nyckel</span><span class="sxs-lookup"><span data-stu-id="e1c73-145">Copy your function API key</span></span>
    1. <span data-ttu-id="e1c73-146">API-nyckeln finns i din HTTP-Utlösarfunktion under `function name` > `Keys` > `Function Keys` 
   ![funktionen nyckel](./media/functions-api-definition-getting-started/functionkey.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-146">The API key can be found in your HTTP Trigger Function under `function name` > `Keys` > `Function Keys`
![Function key](./media/functions-api-definition-getting-started/functionkey.png)</span></span>
1. <span data-ttu-id="e1c73-147">Navigera till den `API Definition` sidan.</span><span class="sxs-lookup"><span data-stu-id="e1c73-147">Navigate to the `API Definition` page.</span></span>
    1. <span data-ttu-id="e1c73-148">Klicka på `Authenticate` och lägga till funktionen API-nyckel i säkerhetsobjekt längst upp.</span><span class="sxs-lookup"><span data-stu-id="e1c73-148">Click `Authenticate` and add your Function API key to the security object at the top.</span></span>
  <span data-ttu-id="e1c73-149">![OpenAPI nyckel](./media/functions-api-definition-getting-started/definitionTest auth.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-149">![OpenAPI key](./media/functions-api-definition-getting-started/definitionTest auth.png)</span></span>
1. <span data-ttu-id="e1c73-150">Välj`/api/yourfunctionroute` > `POST`</span><span class="sxs-lookup"><span data-stu-id="e1c73-150">select `/api/yourfunctionroute` > `POST`</span></span>
1. <span data-ttu-id="e1c73-151">Klicka på `Try it out` och ange ett namn för att testa</span><span class="sxs-lookup"><span data-stu-id="e1c73-151">Click `Try it out` and enter a name to test</span></span>
1. <span data-ttu-id="e1c73-152">Du bör se ett lyckade resultat under `Pretty` 
 ![API-definitions-test](./media/functions-api-definition-getting-started/definitionTest.png)</span><span class="sxs-lookup"><span data-stu-id="e1c73-152">You should see a SUCCESS result under `Pretty`
![API Definition test](./media/functions-api-definition-getting-started/definitionTest.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1c73-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e1c73-153">Next steps</span></span>
* [<span data-ttu-id="e1c73-154">OpenAPI Definition i funktioner – översikt</span><span class="sxs-lookup"><span data-stu-id="e1c73-154">OpenAPI Definition in Functions Overview</span></span>](functions-api-definition.md)
  * <span data-ttu-id="e1c73-155">Läs hela dokumentationen för mer information om OpenAPI som stöds.</span><span class="sxs-lookup"><span data-stu-id="e1c73-155">Read the full documentation for more info on OpenAPI support.</span></span>
* [<span data-ttu-id="e1c73-156">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="e1c73-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  * <span data-ttu-id="e1c73-157">Info för programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="e1c73-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="e1c73-158">Azure Functions GitHub-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="e1c73-158">Azure Functions GitHub repository</span></span>](https://github.com/Azure/Azure-Functions/)
  * <span data-ttu-id="e1c73-159">Ta en titt funktioner GitHub och ger oss feedback på API-definitionen stöd preview!</span><span class="sxs-lookup"><span data-stu-id="e1c73-159">Check out the Functions GitHub to give us feedback on the API definition support preview!</span></span> <span data-ttu-id="e1c73-160">Skapa ett GitHub-problem vad du skulle vilja se uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="e1c73-160">Make a GitHub issue for anything you'd like to see updated.</span></span>
