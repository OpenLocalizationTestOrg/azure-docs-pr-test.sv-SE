---
title: "aaaGetting igång med OpenAPI Metadata i Azure Functions | Microsoft Docs"
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
ms.openlocfilehash: fee3464c9a0a11b6d3891ccd0e9c5343d6bedcad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a><span data-ttu-id="dd394-103">Skapa OpenAPI 2.0 (Swagger) Metadata för en Funktionsapp (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="dd394-103">Creating OpenAPI 2.0 (Swagger) Metadata for a Function App (Preview)</span></span>

<span data-ttu-id="dd394-104">Det här dokumentet hjälper dig att hello steg för steg att skapa en OpenAPI Definition för ett enkelt API som finns på Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="dd394-104">This document guides you through hello step by step process of creating an OpenAPI Definition for a simple API hosted on Azure Functions.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a><span data-ttu-id="dd394-105">Vad är OpenAPI (Swagger)?</span><span class="sxs-lookup"><span data-stu-id="dd394-105">What is OpenAPI (Swagger)?</span></span>
<span data-ttu-id="dd394-106">[Swagger-Metadata](http://swagger.io/) är en fil som definierar hello funktioner och drift av din API och gör en funktion som värd för en REST API-toobe som används av en mängd annan programvara.</span><span class="sxs-lookup"><span data-stu-id="dd394-106">[Swagger Metadata](http://swagger.io/) is a file that defines hello functionality and operating modes of your API, and allows a function hosting a REST API toobe consumed by a wide variety of other software.</span></span> <span data-ttu-id="dd394-107">Microsoft-erbjudanden som PowerApps och [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), samt 3 part developer tooling som [Postman](https://www.getpostman.com/docs/importing_swagger) och [många fler paket](http://swagger.io/tools/) alla Tillåt din API-toobe som används med en Swagger-definition.</span><span class="sxs-lookup"><span data-stu-id="dd394-107">Microsoft offerings like PowerApps and [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), as well as 3rd party developer tooling like [Postman](https://www.getpostman.com/docs/importing_swagger) and [many more packages](http://swagger.io/tools/) all allow your API toobe consumed with a Swagger definition.</span></span>

## <span data-ttu-id="dd394-108"><a name="prepare-function"></a>Skapa en funktion med ett enkelt API</span><span class="sxs-lookup"><span data-stu-id="dd394-108"><a name="prepare-function"></a>Creating a Function with a simple API</span></span>
  <span data-ttu-id="dd394-109">toocreate en OpenAPI definition vi måste först toocreate en funktion med ett enkelt API.</span><span class="sxs-lookup"><span data-stu-id="dd394-109">toocreate an OpenAPI definition, we first need toocreate a Function with a simple API.</span></span> <span data-ttu-id="dd394-110">Om du redan har en API som finns på en Funktionsapp, kan du hoppa över raka toohello nästa avsnitt</span><span class="sxs-lookup"><span data-stu-id="dd394-110">If you already have an API hosted on a Function App, you can skip straight toohello next section</span></span>
1. <span data-ttu-id="dd394-111">Skapa en ny Funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="dd394-111">Create a new Function App.</span></span>
    1. <span data-ttu-id="dd394-112">[Azure-portalen](https://portal.azure.com)  >  `+ New` > Sök efter ”Funktionsapp”</span><span class="sxs-lookup"><span data-stu-id="dd394-112">[Azure portal](https://portal.azure.com) > `+ New` > Search for "Function App"</span></span>
1. <span data-ttu-id="dd394-113">Skapa en ny funktion i din nya Funktionsapp gäller HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="dd394-113">Create a new HTTP trigger function inside your new Function App</span></span>
    1. <span data-ttu-id="dd394-114">Funktionen är i förväg med koden som definierar ett väldigt enkelt REST-API.</span><span class="sxs-lookup"><span data-stu-id="dd394-114">Your function is pre-populated with code defining a very simple REST API.</span></span>
    1. <span data-ttu-id="dd394-115">Valfri sträng skickades toohello funktion som en frågeparameter eller i meddelandetexten hello returneras som ”Hello {indata}”</span><span class="sxs-lookup"><span data-stu-id="dd394-115">Any string passed toohello Function as a query parameter or in hello body is returned as "Hello {input}"</span></span>
1. <span data-ttu-id="dd394-116">Gå toohello `Integrate` för den nya funktionen för HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="dd394-116">Go toohello `Integrate` tab of your new HTTP Trigger function</span></span>
    1. <span data-ttu-id="dd394-117">Växla `Allowed HTTP methods` för`Selected methods`</span><span class="sxs-lookup"><span data-stu-id="dd394-117">Toggle `Allowed HTTP methods` too`Selected methods`</span></span>
    1. <span data-ttu-id="dd394-118">I `Selected HTTP methods` avmarkera alla verb utom POST.</span><span class="sxs-lookup"><span data-stu-id="dd394-118">In `Selected HTTP methods` uncheck every verb except POST.</span></span>
    <span data-ttu-id="dd394-119">![Valda http-metoder](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-119">![Selected HTTP Methods](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span></span>
    1. <span data-ttu-id="dd394-120">Det här steget förenklar API-definition vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="dd394-120">This step will simplify your API definition later on.</span></span>

## <span data-ttu-id="dd394-121"><a name="enable"></a>Aktivera stöd för API-Definition</span><span class="sxs-lookup"><span data-stu-id="dd394-121"><a name="enable"></a>Enabling API Definition Support</span></span>
1. <span data-ttu-id="dd394-122">Navigera för`your function name` > `Platform Features` > `API Definition`
![fliken Definition](./media/functions-api-definition-getting-started/definitiontab.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-122">Navigate too`your function name` > `Platform Features` > `API Definition`
![Definition Tab](./media/functions-api-definition-getting-started/definitiontab.png)</span></span>
1. <span data-ttu-id="dd394-123">Ange `API Definition Source` för`Function (preview)`</span><span class="sxs-lookup"><span data-stu-id="dd394-123">Set `API Definition Source` too`Function (preview)`</span></span>
    1. <span data-ttu-id="dd394-124">Det här steget gör det möjligt för en uppsättning OpenAPI alternativ för funktionen appen, inklusive en slutpunkt toohost en OpenAPI-fil från din Funktionsapp domän, en infogad kopia av hello [OpenAPI Editor](http://editor.swagger.io), och en Snabbstart definition generator.</span><span class="sxs-lookup"><span data-stu-id="dd394-124">This step enables a suite of OpenAPI options for your Function App, including an endpoint toohost an OpenAPI file from your Function App's domain, an inline copy of hello [OpenAPI Editor](http://editor.swagger.io), and a quickstart definition generator.</span></span>
<span data-ttu-id="dd394-125">![Aktiverade Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-125">![Enabled Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span></span>

## <span data-ttu-id="dd394-126"><a name="create-definition"></a>Skapa din API-Definition från en mall</span><span class="sxs-lookup"><span data-stu-id="dd394-126"><a name="create-definition"></a>Creating your API Definition from a template</span></span>
1. <span data-ttu-id="dd394-127">Klicka på `Generate API Definition template`</span><span class="sxs-lookup"><span data-stu-id="dd394-127">Click `Generate API Definition template`</span></span>
    1. <span data-ttu-id="dd394-128">Det här steget söker igenom appen funktionen för HTTP-utlösaren funktioner och använder hello info i functions.json toogenerate ett OpenAPI dokument.</span><span class="sxs-lookup"><span data-stu-id="dd394-128">This step scans your Function App for HTTP Trigger functions and uses hello info in functions.json toogenerate an OpenAPI document.</span></span>
1. <span data-ttu-id="dd394-129">Lägga till ett objekt för åtgärden för`paths: /api/yourfunctionroute post:`</span><span class="sxs-lookup"><span data-stu-id="dd394-129">Add an operation object too`paths: /api/yourfunctionroute post:`</span></span>
    1. <span data-ttu-id="dd394-130">hello quickstart OpenAPI dokumentet är en sammanfattning av en fullständig OpenAPI-dokument. Det krävs mer metadata toobe en fullständig OpenAPI definition, till exempel åtgärden objekt och svar mallar.</span><span class="sxs-lookup"><span data-stu-id="dd394-130">hello quickstart OpenAPI document is an outline of a full OpenAPI doc. It requires more metadata toobe a full OpenAPI definition, such as operation objects and response templates.</span></span>
    1. <span data-ttu-id="dd394-131">hello exempel åtgärden objekt nedan har en fylld ger/förbrukar avsnitt, ett parameter-objekt och ett svarsobjekt.</span><span class="sxs-lookup"><span data-stu-id="dd394-131">hello sample operation object below has a filled out produces/consumes section, a parameter object, and a response object.</span></span>
    
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
    >  <span data-ttu-id="dd394-132">x-ms-sammanfattning ger ett namn i Logikappar, flödesreglering och PowerApps.</span><span class="sxs-lookup"><span data-stu-id="dd394-132">x-ms-summary provides a display name in Logic Apps, Flow, and PowerApps.</span></span>
    >
    > <span data-ttu-id="dd394-133">Checka ut [anpassa din Swagger-definition för PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="dd394-133">Check out [customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn more.</span></span>

1. <span data-ttu-id="dd394-134">Klicka på `save` toosave ändringarna ![definitionen i mallen för att lägga till](./media/functions-api-definition-getting-started/addingtemplate.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-134">Click `save` toosave your changes ![Adding Template Definition](./media/functions-api-definition-getting-started/addingtemplate.png)</span></span>

## <span data-ttu-id="dd394-135"><a name="use-definition"></a>Med API-Definition</span><span class="sxs-lookup"><span data-stu-id="dd394-135"><a name="use-definition"></a>Using Your API Definition</span></span>
1. <span data-ttu-id="dd394-136">Kopiera ditt `API definition URL` och klistra in den i en ny webbläsare fliken tooview raw OpenAPI dokumentet.</span><span class="sxs-lookup"><span data-stu-id="dd394-136">Copy your `API definition URL` and paste it into a new browser tab tooview your raw OpenAPI document.</span></span>
1. <span data-ttu-id="dd394-137">Du kan importera OpenAPI dokument tooany antal verktyg för att testa och integration med URL: en.</span><span class="sxs-lookup"><span data-stu-id="dd394-137">You can import your OpenAPI document tooany number of tools for testing and integration using that URL.</span></span>
    1. <span data-ttu-id="dd394-138">Många Azure-resurser som kan tooautomatically importera OpenAPI dokumentet med hjälp av hello URL för API-Definition som sparas i funktionen programinställningarna.</span><span class="sxs-lookup"><span data-stu-id="dd394-138">Many Azure resources are able tooautomatically import your OpenAPI doc using hello API Definition URL that is saved in your Function application settings.</span></span> <span data-ttu-id="dd394-139">Som en del av hello `Function` källa för API-definition, vi uppdatera den url du.</span><span class="sxs-lookup"><span data-stu-id="dd394-139">As a part of hello `Function` API definition source, we update that url for you.</span></span>


## <span data-ttu-id="dd394-140"><a name="test-definition"></a>Med hjälp av hello Swagger-användargränssnitt tootest API-definition</span><span class="sxs-lookup"><span data-stu-id="dd394-140"><a name="test-definition"></a>Using hello Swagger UI tootest your API definition</span></span>
<span data-ttu-id="dd394-141">Det testar verktyg som är inbyggda i toohello UI vy i Redigeraren för hello inbäddad API-definition.</span><span class="sxs-lookup"><span data-stu-id="dd394-141">There are testing tools built in toohello UI view of hello imbedded API definition editor.</span></span> <span data-ttu-id="dd394-142">Lägga till din API-nyckel och sedan använda hello `Try this operation` knappen under varje metod.</span><span class="sxs-lookup"><span data-stu-id="dd394-142">Add your API key, and then use hello `Try this operation` button under each method.</span></span> <span data-ttu-id="dd394-143">dina API-Definition tooformat hello-begäranden används hello-verktyget och lyckade svar visar att din definition är korrekt.</span><span class="sxs-lookup"><span data-stu-id="dd394-143">hello tool will use your API Definition tooformat hello requests and successful responses will indicate that your definition is correct.</span></span>

### <a name="steps"></a><span data-ttu-id="dd394-144">Så här:</span><span class="sxs-lookup"><span data-stu-id="dd394-144">Steps:</span></span>

1. <span data-ttu-id="dd394-145">Kopiera din funktion API-nyckel</span><span class="sxs-lookup"><span data-stu-id="dd394-145">Copy your function API key</span></span>
    1. <span data-ttu-id="dd394-146">hello API-nyckel kan hittas i HTTP-utlösaren-funktionen under `function name` > `Keys` > `Function Keys` 
   ![funktionen nyckel](./media/functions-api-definition-getting-started/functionkey.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-146">hello API key can be found in your HTTP Trigger Function under `function name` > `Keys` > `Function Keys`
![Function key](./media/functions-api-definition-getting-started/functionkey.png)</span></span>
1. <span data-ttu-id="dd394-147">Navigera toohello `API Definition` sidan.</span><span class="sxs-lookup"><span data-stu-id="dd394-147">Navigate toohello `API Definition` page.</span></span>
    1. <span data-ttu-id="dd394-148">Klicka på `Authenticate` och Lägg till din API-funktionen viktiga toohello säkerhetsobjekt hello överst.</span><span class="sxs-lookup"><span data-stu-id="dd394-148">Click `Authenticate` and add your Function API key toohello security object at hello top.</span></span>
  <span data-ttu-id="dd394-149">![OpenAPI nyckel](./media/functions-api-definition-getting-started/definitionTest auth.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-149">![OpenAPI key](./media/functions-api-definition-getting-started/definitionTest auth.png)</span></span>
1. <span data-ttu-id="dd394-150">Välj`/api/yourfunctionroute` > `POST`</span><span class="sxs-lookup"><span data-stu-id="dd394-150">select `/api/yourfunctionroute` > `POST`</span></span>
1. <span data-ttu-id="dd394-151">Klicka på `Try it out` och ange ett namn på tootest</span><span class="sxs-lookup"><span data-stu-id="dd394-151">Click `Try it out` and enter a name tootest</span></span>
1. <span data-ttu-id="dd394-152">Du bör se ett lyckade resultat under `Pretty` 
 ![API-definitions-test](./media/functions-api-definition-getting-started/definitionTest.png)</span><span class="sxs-lookup"><span data-stu-id="dd394-152">You should see a SUCCESS result under `Pretty`
![API Definition test](./media/functions-api-definition-getting-started/definitionTest.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd394-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd394-153">Next steps</span></span>
* [<span data-ttu-id="dd394-154">OpenAPI Definition i funktioner – översikt</span><span class="sxs-lookup"><span data-stu-id="dd394-154">OpenAPI Definition in Functions Overview</span></span>](functions-api-definition.md)
  * <span data-ttu-id="dd394-155">Läsa hello hela dokumentationen för mer information om OpenAPI som stöds.</span><span class="sxs-lookup"><span data-stu-id="dd394-155">Read hello full documentation for more info on OpenAPI support.</span></span>
* [<span data-ttu-id="dd394-156">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="dd394-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  * <span data-ttu-id="dd394-157">Info för programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="dd394-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="dd394-158">Azure Functions GitHub-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="dd394-158">Azure Functions GitHub repository</span></span>](https://github.com/Azure/Azure-Functions/)
  * <span data-ttu-id="dd394-159">Ta en titt hello funktioner GitHub toogive oss feedback på hello API definition stöd preview!</span><span class="sxs-lookup"><span data-stu-id="dd394-159">Check out hello Functions GitHub toogive us feedback on hello API definition support preview!</span></span> <span data-ttu-id="dd394-160">Skapa ett GitHub-problem döpas toosee uppdateras.</span><span class="sxs-lookup"><span data-stu-id="dd394-160">Make a GitHub issue for anything you'd like toosee updated.</span></span>
