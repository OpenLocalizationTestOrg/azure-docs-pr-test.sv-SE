---
title: aaaOpenAPI metadata i Azure Functions | Microsoft Docs
description: "Översikt över OpenAPI stöd i Azure Functions"
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
ms.openlocfilehash: fff8f14110469a002a6c9dca03f641672003a3a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Stöd för OpenAPI 2.0-metadata i Azure Functions (förhandsgranskning)
OpenAPI 2.0 (tidigare Swagger) stöd för metadata i Azure Functions är en förhandsfunktion som du kan använda toowrite en OpenAPI 2.0 definition inuti en funktionsapp. Du kan sedan värd filen med hjälp av hello funktionsapp.

[OpenAPI metadata](http://swagger.io/) tillåter en funktion som är värd för en REST API-toobe används av en mängd annan programvara. Den här programvaran innehåller Microsoft-erbjudanden som PowerApps och hello [API Apps-funktionen i Azure App Service](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), utvecklingsverktyg från tredje part som [Postman](https://www.getpostman.com/docs/importing_swagger), och [många fler paket](http://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>Vi rekommenderar att börja med hello [komma igång-självstudiekurs](./functions-api-definition-getting-started.md) och sedan återställa toothis dokument toolearn mer om specifika funktioner.

## <a name="enable"></a>Aktivera stöd för OpenAPI definition
Du kan konfigurera alla inställningar för OpenAPI på hello **API-Definition** sida i appen funktionen **plattformsfunktioner**.

Ange tooenable hello generering av en värdbaserad OpenAPI definition och en definition av Snabbstart **källa för API-definition** för**funktionen (förhandsgranskning)**. **Externa URL: en** kan funktionen-toouse en definition av OpenAPI som har värdbaserad någon annanstans.

## <a name="generate-definition"></a>Generera en Swagger-stommen från din funktion metadata
Med hjälp av en mall kan du börja skriva din första OpenAPI definition. hello definition mallen funktionen skapar en sparse OpenAPI definition med hjälp av alla hello metadata i hello function.json-filen för var och en av dina funktioner för HTTP-utlösare. Du behöver toofill i mer information om din API från hello [OpenAPI specifikationen](http://swagger.io/specification/), till exempel mallar för förfrågan och svar.

Stegvisa instruktioner finns i hello [komma igång-självstudiekurs](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Tillgängliga mallar

|Namn| Beskrivning |
|:-----|:-----|
|Genererade Definition|En definition av OpenAPI med hello maximal mängd information som kan härledas från hello funktionen befintliga metadata.|

### <a name="quickstart-details"></a>Med metadata i hello genereras definition

följande tabell representerar hello hello Azure portalinställningar och motsvarande data i function.json eftersom den mappade toohello genererade Swagger stommen.

|Swagger.JSON|Portalens användargränssnitt|Function.JSON|
|:----|:-----|:-----|
|[Värden](http://swagger.io/specification/#fixed-fields-15)|**Funktionen appinställningar** > **Apptjänst inställningar** > **översikt** > **URL**|*Finns inte*
|[Sökvägar](http://swagger.io/specification/#paths-object-29)|**Integrera** > **valda http-metoder**|Bindningar: väg
|[Objektet i sökvägen](http://swagger.io/specification/#path-item-object-32)|**Integrera** > **flödesmallen**|Bindningar: metoder
|[Säkerhet](http://swagger.io/specification/#security-scheme-object-112)|**Nycklar**|*Finns inte*|
|Åtgärds-ID *|**Väg + tillåtna verb**|Väg + tillåtna verb|

\*hello åtgärds-ID krävs endast för integrering med PowerApps och flöde.
> [!NOTE]
> hello x-ms-sammanfattning av tillägget ger ett namn i Logikappar, PowerApps och flöde.
>
> Det finns fler toolearn [anpassa din Swagger-definition för PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>Använda CI/CD tooset en API-definition

 Du måste aktivera API definition värd i hello-portalen innan du aktiverar källa kontrollen toomodify API-definition från källkontroll. Följ dessa anvisningar:

1. Bläddra för**API-Definition (förhandsgranskning)** i funktionen app-inställningarna.
  1. Ange **källa för API-definition** för**funktionen**.
  1. Klicka på **generera API-definition mallen** och sedan **spara** toocreate definition av en mall för att ändra senare.
  1. Observera att din URL för API-definition och nyckel.
1. [Ställ in kontinuerlig integration/kontinuerlig distribution (CI/CD)](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Ändra swagger.json i källkontrollen på \site\wwwroot\.azurefunctions\swagger\swagger.json.

Ändringar som tooswagger.json i databasen hanteras av din funktionsapp på URL: en för hello API-definition och nyckel som du antecknade i steg nu 1.c.

## <a name="next-steps"></a>Nästa steg
* [Komma igång-självstudiekurs](functions-api-definition-getting-started.md). Prova vår genomgången toosee en OpenAPI definition i åtgärden.
* [Azure Functions GitHub-lagringsplatsen](https://github.com/Azure/Azure-Functions/). Checka ut hello funktioner databasen toogive oss feedback på hello API definition stöd preview. Skapa ett GitHub-problem vad du vill uppdatera toosee.
* [Azure Functions för utvecklare](functions-reference.md). Läs mer om att koda funktioner och definiera utlösare och bindningar.
