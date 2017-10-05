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
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Skapa OpenAPI 2.0 (Swagger) Metadata för en Funktionsapp (förhandsgranskning)

Det här dokumentet hjälper dig att steg för steg att skapa en OpenAPI Definition för ett enkelt API som finns på Azure Functions.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Vad är OpenAPI (Swagger)?
[Swagger-Metadata](http://swagger.io/) är en fil som definierar funktioner och -lägena av din API och gör en funktion som värd för en REST-API för att användas av en mängd annan programvara. Microsoft-erbjudanden som PowerApps och [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), samt 3 part developer tooling som [Postman](https://www.getpostman.com/docs/importing_swagger) och [många fler paket](http://swagger.io/tools/) alla Tillåt ditt API för att användas med en Swagger-definition.

## <a name="prepare-function"></a>Skapa en funktion med ett enkelt API
  Om du vill skapa en OpenAPI definition måste vi först skapa en funktion med ett enkelt API. Om du redan har en API som finns på en Funktionsapp kan gå du direkt till nästa avsnitt
1. Skapa en ny Funktionsapp.
    1. [Azure-portalen](https://portal.azure.com)  >  `+ New` > Sök efter ”Funktionsapp”
1. Skapa en ny funktion i din nya Funktionsapp gäller HTTP-utlösare
    1. Funktionen är i förväg med koden som definierar ett väldigt enkelt REST-API.
    1. Valfri sträng som skickas till funktionen som en frågeparameter eller i meddelandetexten returneras som ”Hello {indata}”
1. Gå till den `Integrate` för den nya funktionen för HTTP-utlösare
    1. Växla `Allowed HTTP methods` till`Selected methods`
    1. I `Selected HTTP methods` avmarkera alla verb utom POST.
    ![Valda http-metoder](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
    1. Det här steget förenklar API-definition vid ett senare tillfälle.

## <a name="enable"></a>Aktivera stöd för API-Definition
1. Gå till `your function name`  >  `Platform Features`  >  `API Definition` 
 ![fliken Definition](./media/functions-api-definition-getting-started/definitiontab.png)
1. Ange `API Definition Source` till`Function (preview)`
    1. Det här steget gör det möjligt för en uppsättning OpenAPI alternativ för funktionen appen, inklusive en slutpunkt som värd för en OpenAPI-fil från din Funktionsapp domän, en infogad kopia av den [OpenAPI Editor](http://editor.swagger.io), och en Snabbstart definition generator.
![Aktiverade Definition](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Skapa din API-Definition från en mall
1. Klicka på `Generate API Definition template`
    1. Det här steget söker igenom appen funktionen för HTTP-utlösaren funktioner och använder informationen i functions.json för att generera ett OpenAPI dokument.
1. Lägg till ett objekt för åtgärden att`paths: /api/yourfunctionroute post:`
    1. Snabbstart OpenAPI dokumentet är en sammanfattning av en fullständig OpenAPI-dokument. Det krävs mer metadata för att vara en fullständig OpenAPI definition, till exempel åtgärden objekt och svar mallar.
    1. Exempel åtgärden objekt nedan har en fylld ger/förbrukar avsnitt, ett parameter-objekt och ett svarsobjekt.
    
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
    > Checka ut [anpassa din Swagger-definition för PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) vill veta mer.

1. Klicka på `save` spara dina ändringar ![definitionen i mallen för att lägga till](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Med API-Definition
1. Kopiera ditt `API definition URL` och klistra in den i en ny webbläsarflik att visa dokumentet rådata OpenAPI.
1. Du kan importera dokumentet OpenAPI till valfritt antal verktyg för att testa och integration med URL: en.
    1. Många Azure-resurser kan importera OpenAPI dokumentet med hjälp av URL för API-Definition som sparas i programinställningarna funktionen automatiskt. Som en del av den `Function` källa för API-definition, vi uppdatera den url du.


## <a name="test-definition"></a>Använd Swagger-användargränssnitt för att testa din API-definition
Det testar verktyg som är inbyggd i vyn Användargränssnittet i Redigeraren för inbäddad API-definition. Lägg till din API-nyckel och sedan använda den `Try this operation` knappen under varje metod. Verktyget använder API-Definition för att formatera begäranden och lyckade svar visar att din definition är korrekt.

### <a name="steps"></a>Så här:

1. Kopiera din funktion API-nyckel
    1. API-nyckeln finns i din HTTP-Utlösarfunktion under `function name` > `Keys` > `Function Keys` 
   ![funktionen nyckel](./media/functions-api-definition-getting-started/functionkey.png)
1. Navigera till den `API Definition` sidan.
    1. Klicka på `Authenticate` och lägga till funktionen API-nyckel i säkerhetsobjekt längst upp.
  ![OpenAPI nyckel](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Välj`/api/yourfunctionroute` > `POST`
1. Klicka på `Try it out` och ange ett namn för att testa
1. Du bör se ett lyckade resultat under `Pretty` 
 ![API-definitions-test](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Nästa steg
* [OpenAPI Definition i funktioner – översikt](functions-api-definition.md)
  * Läs hela dokumentationen för mer information om OpenAPI som stöds.
* [Azure Functions, info för utvecklare](functions-reference.md)  
  * Info för programmerare om att koda funktioner och definiera utlösare och bindningar.
* [Azure Functions GitHub-lagringsplats](https://github.com/Azure/Azure-Functions/)
  * Ta en titt funktioner GitHub och ger oss feedback på API-definitionen stöd preview! Skapa ett GitHub-problem vad du skulle vilja se uppdaterade.
