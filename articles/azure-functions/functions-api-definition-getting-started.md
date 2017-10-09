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
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Skapa OpenAPI 2.0 (Swagger) Metadata för en Funktionsapp (förhandsgranskning)

Det här dokumentet hjälper dig att hello steg för steg att skapa en OpenAPI Definition för ett enkelt API som finns på Azure Functions.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Vad är OpenAPI (Swagger)?
[Swagger-Metadata](http://swagger.io/) är en fil som definierar hello funktioner och drift av din API och gör en funktion som värd för en REST API-toobe som används av en mängd annan programvara. Microsoft-erbjudanden som PowerApps och [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), samt 3 part developer tooling som [Postman](https://www.getpostman.com/docs/importing_swagger) och [många fler paket](http://swagger.io/tools/) alla Tillåt din API-toobe som används med en Swagger-definition.

## <a name="prepare-function"></a>Skapa en funktion med ett enkelt API
  toocreate en OpenAPI definition vi måste först toocreate en funktion med ett enkelt API. Om du redan har en API som finns på en Funktionsapp, kan du hoppa över raka toohello nästa avsnitt
1. Skapa en ny Funktionsapp.
    1. [Azure-portalen](https://portal.azure.com)  >  `+ New` > Sök efter ”Funktionsapp”
1. Skapa en ny funktion i din nya Funktionsapp gäller HTTP-utlösare
    1. Funktionen är i förväg med koden som definierar ett väldigt enkelt REST-API.
    1. Valfri sträng skickades toohello funktion som en frågeparameter eller i meddelandetexten hello returneras som ”Hello {indata}”
1. Gå toohello `Integrate` för den nya funktionen för HTTP-utlösare
    1. Växla `Allowed HTTP methods` för`Selected methods`
    1. I `Selected HTTP methods` avmarkera alla verb utom POST.
    ![Valda http-metoder](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
    1. Det här steget förenklar API-definition vid ett senare tillfälle.

## <a name="enable"></a>Aktivera stöd för API-Definition
1. Navigera för`your function name` > `Platform Features` > `API Definition`
![fliken Definition](./media/functions-api-definition-getting-started/definitiontab.png)
1. Ange `API Definition Source` för`Function (preview)`
    1. Det här steget gör det möjligt för en uppsättning OpenAPI alternativ för funktionen appen, inklusive en slutpunkt toohost en OpenAPI-fil från din Funktionsapp domän, en infogad kopia av hello [OpenAPI Editor](http://editor.swagger.io), och en Snabbstart definition generator.
![Aktiverade Definition](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Skapa din API-Definition från en mall
1. Klicka på `Generate API Definition template`
    1. Det här steget söker igenom appen funktionen för HTTP-utlösaren funktioner och använder hello info i functions.json toogenerate ett OpenAPI dokument.
1. Lägga till ett objekt för åtgärden för`paths: /api/yourfunctionroute post:`
    1. hello quickstart OpenAPI dokumentet är en sammanfattning av en fullständig OpenAPI-dokument. Det krävs mer metadata toobe en fullständig OpenAPI definition, till exempel åtgärden objekt och svar mallar.
    1. hello exempel åtgärden objekt nedan har en fylld ger/förbrukar avsnitt, ett parameter-objekt och ett svarsobjekt.
    
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
    >  x-ms-sammanfattning ger ett namn i Logikappar, flödesreglering och PowerApps.
    >
    > Checka ut [anpassa din Swagger-definition för PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn mer.

1. Klicka på `save` toosave ändringarna ![definitionen i mallen för att lägga till](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Med API-Definition
1. Kopiera ditt `API definition URL` och klistra in den i en ny webbläsare fliken tooview raw OpenAPI dokumentet.
1. Du kan importera OpenAPI dokument tooany antal verktyg för att testa och integration med URL: en.
    1. Många Azure-resurser som kan tooautomatically importera OpenAPI dokumentet med hjälp av hello URL för API-Definition som sparas i funktionen programinställningarna. Som en del av hello `Function` källa för API-definition, vi uppdatera den url du.


## <a name="test-definition"></a>Med hjälp av hello Swagger-användargränssnitt tootest API-definition
Det testar verktyg som är inbyggda i toohello UI vy i Redigeraren för hello inbäddad API-definition. Lägga till din API-nyckel och sedan använda hello `Try this operation` knappen under varje metod. dina API-Definition tooformat hello-begäranden används hello-verktyget och lyckade svar visar att din definition är korrekt.

### <a name="steps"></a>Så här:

1. Kopiera din funktion API-nyckel
    1. hello API-nyckel kan hittas i HTTP-utlösaren-funktionen under `function name` > `Keys` > `Function Keys` 
   ![funktionen nyckel](./media/functions-api-definition-getting-started/functionkey.png)
1. Navigera toohello `API Definition` sidan.
    1. Klicka på `Authenticate` och Lägg till din API-funktionen viktiga toohello säkerhetsobjekt hello överst.
  ![OpenAPI nyckel](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Välj`/api/yourfunctionroute` > `POST`
1. Klicka på `Try it out` och ange ett namn på tootest
1. Du bör se ett lyckade resultat under `Pretty` 
 ![API-definitions-test](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Nästa steg
* [OpenAPI Definition i funktioner – översikt](functions-api-definition.md)
  * Läsa hello hela dokumentationen för mer information om OpenAPI som stöds.
* [Azure Functions, info för utvecklare](functions-reference.md)  
  * Info för programmerare om att koda funktioner och definiera utlösare och bindningar.
* [Azure Functions GitHub-lagringsplats](https://github.com/Azure/Azure-Functions/)
  * Ta en titt hello funktioner GitHub toogive oss feedback på hello API definition stöd preview! Skapa ett GitHub-problem döpas toosee uppdateras.
