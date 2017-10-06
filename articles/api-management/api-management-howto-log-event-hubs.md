---
title: "aaaHow toolog händelser tooAzure Händelsehubbar i Azure API Management | Microsoft Docs"
description: "Lär dig hur toolog händelser tooAzure Händelsehubbar i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 09ca65fc48a874467c6662858f7594e9b19fcdb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toolog-events-tooazure-event-hubs-in-azure-api-management"></a>Hur toolog händelser tooAzure Händelsehubbar i Azure API Management
Händelsehubbar i Azure är en mycket skalbar tjänst för dataingång som kan mata in miljontals händelser per sekund så att du kan bearbeta och analysera hello stora mängder data som produceras av dina anslutna enheter och program. Händelsehubbar fungerar som hello ”ytterdörren” för en händelsepipeline, och när data har samlats in i en händelsehubb, det kan omvandlas och lagras med hjälp av en leverantör av realtidsanalys eller adaptrar för batchbearbetning/lagring. Händelsehubbar frikopplar hello framställning av en dataström med händelser från hello användningen av dessa händelser så att händelsekonsumenterna kan komma åt hello händelser på sitt eget schema.

Den här artikeln är en tillhörande toohello [integrera Azure API Management med Händelsehubbar](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video och beskriver hur toolog API Management händelser med hjälp av Händelsehubbar i Azure.

## <a name="create-an-azure-event-hub"></a>Skapa ett Azure Event Hub
toocreate en ny Händelsehubb inloggning toohello [klassiska Azure-portalen](https://manage.windowsazure.com) och på **ny**->**Apptjänster**->**Service Bus**  -> **Händelsehubb**->**Snabbregistrering**. Ange namnet på en Händelsehubb region, väljer en prenumeration och ett namnområde. Om du inte tidigare har skapat ett namnområde kan du skapa en genom att skriva ett namn i hello **Namespace** textruta. När alla egenskaper har konfigurerats, klickar du på **skapa en ny Händelsehubb** toocreate hello Event Hub.

![Skapa händelsehubb][create-event-hub]

Nu ska du navigera toohello **konfigurera** för din nya Event Hub och skapa två **delade åtkomstprinciper**. Namnge hello första **sändande** och ge den **skicka** behörigheter.

![Skicka princip][sending-policy]

Namnge hello andra **tar emot**, ger det **lyssna** behörigheter och klickar på **spara**.

![Emot princip][receiving-policy]

Varje princip för delad åtkomst kan program toosend och ta emot händelser tooand från hello Event Hub. tooaccess hello-anslutningssträngar för dessa principer navigera toohello **instrumentpanelen** hello Event Hub och klicka på fliken **anslutningsinformationen**.

![Anslutningssträng][event-hub-dashboard]

Hej **sändande** anslutningssträngen som används när du loggar händelser, och hello **tar emot** anslutningssträngen som används när du hämtar händelser från hello Event Hub.

![Anslutningssträng][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Skapa en API Management-logg
Nu när du har en Händelsehubb hello nästa steg är tooconfigure en [loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) i API Management-tjänsten så att den kan logga händelser toohello Event Hub.

API Management loggare konfigureras med hjälp av hello [API Management REST API](http://aka.ms/smapi). Innan du använder hello REST API för hello första gången, granska hello [krav](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) och se till att du har [aktiverat åtkomst toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

toocreate loggaren, gör ett HTTP PUT-begäran som använder hello följande URL: en mall.

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Ersätt `{your service}` med hello namnet på din API Management service-instans.
* Ersätt `{new logger name}` med hello önskat namn för din nya meddelandeloggfiler. Du sedan hänvisar till det här namnet när du konfigurerar hello [loggen till eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) princip

Lägg till hello efter huvuden toohello begäran.

* Content-Type: application/json
* Auktorisering: SharedAccessSignature 58...
  * Anvisningar för att generera hello `SharedAccessSignature` finns [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Ange hello begärandetexten med hello följande mall.

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of hello Event Hub from hello Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `type`måste anges för`AzureEventHub`.
* `description`ger en valfri beskrivning av hello loggaren och kan vara en tom sträng om så önskas.
* `credentials`innehåller hello `name` och `connectionString` för din Azure-Händelsehubb.

När du gör hello begäran om hello loggaren skapas en statuskod för `201 Created` returneras.

> [!NOTE]
> Andra möjliga returkoder och skälen finns [skapa en loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). toosee hur utföra andra åtgärder som finns i listan, uppdatera och ta bort, hello [loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentationen för enheten.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurera principer för logg-eventhubs
När din loggaren har konfigurerats i API-hantering kan konfigurera du din loggen till eventhubs principer toolog hello önskad händelser. hello loggen till eventhubs principen kan användas antingen hello inkommande principavsnitt eller hello utgående principavsnitt.

tooconfigure principer, logga in toohello [Azure-portalen](https://portal.azure.com), navigera tooyour API Management-tjänsten och klicka på **Publisher portal** tooaccess hello publisher-portalen.

![Utgivarportalen][publisher-portal]

Klicka på **principer** hello API Management menyn hello vänster, Välj önskade hello-produkten och API och klicka **Lägg till princip**. I det här exemplet lägger vi till en princip toohello **Echo API** i hello **obegränsad** produkten.

![Lägg till princip][add-policy]

Placera markören i hello `inbound` princip avsnittet och klicka på hello **loggen tooEventHub** princip tooinsert hello `log-to-eventhub` Principmall för instruktionen.

![Principredigerare][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

Ersätt `logger-id` med hello namnet hello API Management-logg som du konfigurerade i hello föregående steg.

Du kan använda ett uttryck som returnerar en sträng som värde hello hello `log-to-eventhub` element. En sträng som innehåller hello datum och klockslag, tjänstnamn, förfrågnings-id, ip-adressen för begäran och åtgärdsnamn loggas i det här exemplet.

Klicka på **spara** toosave hello uppdateras principkonfigurationen. Så snart den är sparad hello principen är aktiv och händelser är loggade toohello avses Event Hub.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om Azure Event Hubs
  * [Kom igång med Händelsehubbar i Azure](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Ta emot meddelanden med EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmeringsguide för Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Mer information om API-hantering och Händelsehubbar-integrering
  * [Loggaren enhetsreferens](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [principreferens för loggen till eventhub](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Övervaka dina API: er med Azure API Management, Händelsehubbar och Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Titta på en videogenomgång
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
