---
title: "aaaAzure händelse rutnätet översikt"
description: "Beskriver Azure händelse rutnätet och dess begrepp."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 95dce22e9335df88e81b134143a6c14994c26b8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-event-grid"></a>En introduktion tooAzure händelse rutnätet

Azure händelse rutnätet kan tooeasily bygga program med händelsebaserat arkitekturerna. Du kan välja hello Azure-resurs du gillar toosubscribe till och ger hello händelsehanteraren eller WebHook endpoint toosend hello händelse. Händelsen rutnätet har inbyggt stöd för händelser som kommer från Azure-tjänster, som storage-blobbar och resursgrupper. Händelsen rutnätet har också anpassade stöd för program och händelser från tredje part, med hjälp av anpassade avsnitt och anpassade webhooks. 

Du kan använda filter tooroute specifika händelser toodifferent slutpunkter, multicast toomultiple slutpunkter och kontrollera att din händelser levereras på ett tillförlitligt sätt. Händelsen rutnätet har också inbyggt stöd för anpassade och tredjeparts-händelser.

Hello förhandsversionen händelse rutnätet stöder **westus2** och **westcentralus** platser. Kommer att läggas till andra regioner.

Den här artikeln innehåller en översikt över Azure händelse rutnätet. Om du vill tooget igång med händelsen rutnätet finns [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).

![Händelsen rutnätet funktionella modellen](./media/overview/event-grid-functional-model.png)

Blob Storage är för närvarande inte tillgänglig som en utgivare.

## <a name="concepts"></a>Koncept

Det finns fem koncept i Azure händelse rutnät som gör att du kan komma igång:

* **Händelser** -vad hände.
* **Källor/händelseutfärdare** - när hello händelsen inträffade.
* **Avsnitt om** -hello slutpunkt där utgivare skicka händelser.
* **Händelseprenumerationer** -hello slutpunkt eller inbyggda mekanism tooroute händelser, ibland toomultiple hanterare. Prenumerationer används också av hanterare toointelligently filtrera inkommande händelser.
* **Händelsehanterare** – hello app eller tjänst korsreagerande toohello händelse.

Mer information om de här koncepten finns [koncept i Azure händelse rutnät](concepts.md).

## <a name="capabilities"></a>Funktioner

Här följer några av hello viktiga funktioner i Azure händelse rutnätet:

* **Enkelhet** -punkt och tooaim händelsen från Azure-resurs tooany händelsehanteraren eller slutpunkt.
* **Avancerad filtrering** -Filter på händelsen typ eller händelse publicera sökvägen tooensure händelsehanterare får bara relevanta händelser.
* **FAN-out** -prenumerera på flera slutpunkter toohello samma händelse toosend kopior av hello händelse tooas många platser efter behov.
* **Tillförlitlighet** -använda 24-timmarsformat återförsök med exponentiell backoff tooensure händelser levereras.
* **Betala per händelse** – betala endast för hello mycket du använder händelsen rutnätet.
* **Hög genomströmning** -skapa omfattande arbetsbelastningar i händelse rutnät med stöd för miljontals händelser per sekund.
* **Inbyggda händelser** – komma igång snabbt och köra med resurs-definierade inbyggda händelser.
* **Anpassade händelser** -Använd händelse rutnätet flödet, filter och tillförlitligt leverera anpassade händelser i din app.

## <a name="built-in-publisher-and-handler-integration"></a>Inbyggd integrering av utgivare och hanterare

Azure erbjuder inbyggda händelsestöd med ett stort antal tjänster, inklusive både utgivare och hanterare.

### <a name="publishers"></a>Utgivare

För närvarande har hello följande Azure-tjänster inbyggda utgivarens support för händelsen rutnät:

* Resursgrupper (hanteringsåtgärder)
* Azure-prenumerationer (hanteringsåtgärder)
* Händelsehubbar
* Anpassade avsnitt

Andra Azure-tjänster kommer att adderas i år.

### <a name="handlers"></a>Hanterare

För närvarande har hello följande Azure-tjänster stöd för inbyggda hanterare för händelsen rutnätet: 

* Azure Functions
* Logic Apps
* Azure Automation
* WebHooks

Andra Azure-tjänster kommer att adderas i år.

## <a name="what-can-i-do-with-event-grid"></a>Vad kan jag göra med händelsen rutnät?

Azure händelse rutnätet innehåller flera funktioner som kan förbättrar frågeprestanda avsevärt serverlösa ops automation och integrering arbete: 

### <a name="serverless-application-architectures"></a>Arkitekturer för program utan server

![Program](./media/overview/serverless_web_app.png)

Event Grid kopplar samman datakällor och händelsehanterare. Till exempel använda händelsen rutnätet tooinstantly utlösaren serverlösa funktionen toorun avbildningen analys varje gång en ny är läggs tooa blob storage-behållare. 

### <a name="ops-automation"></a>OPS Automation

![Automatisering av åtgärder](./media/overview/Ops_automation.png)

Händelsen rutnätet kan du toospeed automation och förenkla tvingande principer. Event Grid kan exempelvis meddela Azure Automation när en virtuell dator skapas eller en SQL-databas startas. Dessa händelser kan användas för tooautomatically Kontrollera tjänstkonfiguration är kompatibel, placera metadata i operations-verktyg, taggen virtuella datorer eller filen arbetsobjekt.

### <a name="application-integration"></a>Integrering av program

![Integrering av program](./media/overview/app_integration.png)

Event Grid ansluter din app till andra tjänster. Exempelvis skapa en anpassad avsnittet toosend appens händelse data tooEvent rutnätet och dra nytta av dess tillförlitlig leverans, avancerad routning, och direktintegrering med Azure. Du kan också använda händelsen rutnät med Logic Apps tooprocess data var som helst, utan att skriva kod. 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>Hur skiljer händelse rutnätet från andra Azure integration services?

Händelsen rutnätet är en eventing bakplan som möjliggör händelsedriven, reaktiv programmering. Den är helt integrerat med Azure-tjänster och kan integreras med tjänster från tredje part. hello Händelsemeddelandet innehåller hello information du behöver tooreact toochanges i tjänster och program. Händelsen rutnätet är inte en data-pipeline och leverera inte hello faktiska objekt som har uppdaterats.

Service Bus passar bra för traditionella företagsprogram som kräver transaktioner, ordning, dubblettidentifiering och omedelbar konsekvenskontroll. Händelsen rutnätet är utformad för hastighet, skala, bredd och billig reaktiv modellen. Det är bra tooserverless arkitektur.

Händelsen rutnätet kompletterar andra Azure-tjänster som Logic Apps och Händelsehubbar. Händelseutlösare rutnätet hello logik app toobegin dess arbetsflöde. Händelsehubbar fungerar med händelsen rutnätet genom att aktivera du tooreact tooevents från Event Hubs avbilda och bygga data ingång och omvandling pipelines.

## <a name="how-much-does-event-grid-cost"></a>Hur mycket kostar händelse rutnätet?

Azure händelse rutnätet använder en modell med betalning per händelse prisnivå så att du betalar bara för att du använder.

Händelsen rutnätet kostnaden $0,60 per miljoner operations ($0,30 under förhandsgranskning) och hello först 100 000 åtgärden per månad är gratis. Åtgärder definieras som händelsen ingång, avancerad matchar, leverans försök och management-anrop.  Mer information finns på hello [sida med priser](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Nästa steg

* [Skapa och prenumerera toocustom händelser](custom-event-quickstart.md) igång direkt och börja skicka dina egna anpassade händelser tooany slutpunkt med hello Azure händelse rutnätet Snabbstart.
* [Med hjälp av Logic Apps som händelsehanterare](monitor-virtual-machine-changes-event-grid-logic-app.md) en självstudiekurs om hur du skapar en app med Logic Apps tooreact tooevents pushas efter händelsen rutnät.
* [Händelsen rutnätet REST API-referens](/rest/api/eventgrid)  
  Ger mer teknisk information om hello Azure händelse rutnätet och en referens för att hantera prenumerationer på händelser, Routning och filtrering.
