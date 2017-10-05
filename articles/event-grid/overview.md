---
title: "Översikt över Azure händelse rutnätet"
description: "Beskriver Azure händelse rutnätet och dess begrepp."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 59a834f32793e349d5639baf3c80dbcba274dfa8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="an-introduction-to-azure-event-grid"></a>En introduktion till Azure händelse rutnätet

Azure händelse rutnätet kan du enkelt kan skapa program med händelsebaserat arkitekturerna. Du väljer Azure-resursen du vill prenumerera på och ge händelsehanteraren eller WebHook-slutpunkten för att skicka händelsen till. Händelsen rutnätet har inbyggt stöd för händelser som kommer från Azure-tjänster, som storage-blobbar och resursgrupper. Händelsen rutnätet har också anpassade stöd för program och händelser från tredje part, med hjälp av anpassade avsnitt och anpassade webhooks. 

Du kan använda filter för att vidarebefordra specifika händelser till olika slutpunkter samtidigt till flera slutpunkter och kontrollera att din händelser levereras på ett tillförlitligt sätt. Händelsen rutnätet har också inbyggt stöd för anpassade och tredjeparts-händelser.

I förhandsversionen har Event Grid stöd för platserna **westus2** och **westcentralus**. Kommer att läggas till andra regioner.

Den här artikeln innehåller en översikt över Azure händelse rutnätet. Om du vill komma igång med händelsen rutnätet finns [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).

![Händelsen rutnätet funktionella modellen](./media/overview/event-grid-functional-model.png)

Blob Storage är för närvarande inte tillgänglig som en utgivare.

## <a name="concepts"></a>Koncept

Det finns fem koncept i Azure händelse rutnät som gör att du kan komma igång:

* **Händelser** -vad hände.
* **Källor/händelseutfärdare** – där händelsen ägde rum.
* **Avsnitt om** -slutpunkten som utgivare för att skicka händelser.
* **Händelseprenumerationer** -slutpunkt eller inbyggda mekanism för vägen händelser, ibland till flera hanterarna. Prenumerationer används också av hanterare Intelligent filtrera inkommande händelser.
* **Händelsehanterare** -app eller tjänst reagera på händelsen.

Mer information om de här koncepten finns [koncept i Azure händelse rutnät](concepts.md).

## <a name="capabilities"></a>Funktioner

Här följer några viktiga funktioner i Azure händelse rutnätet:

* **Enkelhet** -plats och klicka på att sträva efter händelser från Azure-resurs till händelsehanteraren eller slutpunkt.
* **Avancerad filtrering** -Filter på händelsen typ eller händelse publiceringssökväg att säkerställa händelsehanterare får endast relevanta händelser.
* **FAN-out** -prenumerera på flera slutpunkter på samma händelse ska skicka kopior av händelsen till så många platser efter behov.
* **Tillförlitlighet** -använda 24-timmarsformat återförsök med exponentiell backoff för att säkerställa att händelser levereras.
* **Betala per händelse** – betala endast för hur du använder händelsen rutnätet.
* **Hög genomströmning** -skapa omfattande arbetsbelastningar i händelse rutnät med stöd för miljontals händelser per sekund.
* **Inbyggda händelser** – komma igång snabbt och köra med resurs-definierade inbyggda händelser.
* **Anpassade händelser** -Använd händelse rutnätet flödet, filter och tillförlitligt leverera anpassade händelser i din app.

## <a name="built-in-publisher-and-handler-integration"></a>Inbyggd integrering av utgivare och hanterare

Azure erbjuder inbyggda händelsestöd med ett stort antal tjänster, inklusive både utgivare och hanterare.

### <a name="publishers"></a>Utgivare

För närvarande har följande Azure-tjänster inbyggda utgivarens support för händelsen rutnät:

* Resursgrupper (hanteringsåtgärder)
* Azure-prenumerationer (hanteringsåtgärder)
* Händelsehubbar
* Anpassade avsnitt

Andra Azure-tjänster kommer att adderas i år.

### <a name="handlers"></a>Hanterare

Följande Azure-tjänster har för närvarande stöd för inbyggda hanterare för händelsen rutnätet: 

* Azure Functions
* Logic Apps
* Azure Automation
* WebHooks

Andra Azure-tjänster kommer att adderas i år.

## <a name="what-can-i-do-with-event-grid"></a>Vad kan jag göra med händelsen rutnät?

Azure händelse rutnätet innehåller flera funktioner som kan förbättrar frågeprestanda avsevärt serverlösa ops automation och integrering arbete: 

### <a name="serverless-application-architectures"></a>Arkitekturer för program utan server

![Program](./media/overview/serverless_web_app.png)

Event Grid kopplar samman datakällor och händelsehanterare. Använd exempelvis Event Grid för att direkt utlösa att en funktion utan server kör bildanalys varje gång ett nytt foto läggs till i en blobblagringsbehållare. 

### <a name="ops-automation"></a>OPS Automation

![Automatisering av åtgärder](./media/overview/Ops_automation.png)

Event Grid ger snabbare automatisering och enklare principtillämpning. Event Grid kan exempelvis meddela Azure Automation när en virtuell dator skapas eller en SQL-databas startas. De här händelserna kan användas för att automatiskt kontrollera att tjänstkonfigurationer följer standard, placera metadata i åtgärdsverktyg, tagga virtuella datorer eller arkivera arbetsobjekt.

### <a name="application-integration"></a>Integrering av program

![Integrering av program](./media/overview/app_integration.png)

Event Grid ansluter din app till andra tjänster. Till exempel skapa en anpassad avsnittet för att skicka data om din app till händelse rutnätet och dra nytta av dess tillförlitlig leverans, avancerad routning, och dirigera integrering med Azure. Du kan även använda Event Grid med Logic Apps för att bearbeta data var som helst, utan att skriva kod. 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>Hur skiljer händelse rutnätet från andra Azure integration services?

Händelsen rutnätet är en eventing bakplan som möjliggör händelsedriven, reaktiv programmering. Den är helt integrerat med Azure-tjänster och kan integreras med tjänster från tredje part. Händelsemeddelandet innehåller den information du behöver ta hänsyn till ändringar i tjänster och program. Händelsen rutnätet är inte en data-pipeline och leverera inte det faktiska objektet har uppdaterats.

Service Bus passar bra för traditionella företagsprogram som kräver transaktioner, ordning, dubblettidentifiering och omedelbar konsekvenskontroll. Händelsen rutnätet är utformad för hastighet, skala, bredd och billig reaktiv modellen. Det är bra att serverlösa arkitektur.

Händelsen rutnätet kompletterar andra Azure-tjänster som Logic Apps och Händelsehubbar. Händelsen rutnätet utlöser logikappen om du vill starta arbetsflödet. Händelsehubbar fungerar med händelsen rutnätet genom att du kan reagera på händelser från Event Hubs avbilda och skapa pipelines för ingång och omvandling av data.

## <a name="how-much-does-event-grid-cost"></a>Hur mycket kostar händelse rutnätet?

Azure händelse rutnätet använder en modell med betalning per händelse prisnivå så att du betalar bara för att du använder.

Händelsen rutnätet kostnaden $0,60 per miljoner operations ($0,30 under förhandsgranskning) och åtgärden först 100 000 per månad är gratis. Åtgärder definieras som händelsen ingång, avancerad matchar, leverans försök och management-anrop.  Mer information finns på den [sida med priser](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Nästa steg

* [Skapa och prenumerera på anpassade händelser](custom-event-quickstart.md) igång direkt och börja skicka dina egna anpassade händelser för valfri slutpunkt med hjälp av Azure händelse rutnätet Snabbstart.
* [Med hjälp av Logic Apps som händelsehanterare](monitor-virtual-machine-changes-event-grid-logic-app.md) en självstudiekurs om hur du skapar en app med Logic Apps för att reagera på händelser pushas efter händelsen rutnät.
* [Händelsen rutnätet REST API-referens](/rest/api/eventgrid)  
  Ger mer teknisk information om rutnätet Azure händelse och en referens för att hantera prenumerationer på händelser, Routning och filtrering.