---
title: "aaaAzure funktioner användnings- och App Service-planer | Microsoft Docs"
description: "Förstå hur Azure Functions skalas toomeet hello behoven för din händelsedriven arbetsbelastningar."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3826915b93328635d9295efe90ecc421e6c56af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-consumption-and-app-service-plans"></a>Azure Functions användnings- och Apptjänst-planer 

## <a name="introduction"></a>Introduktion

Du kan köra Azure Functions i två olika lägen: förbrukning planerings- och Azure App Service-plan. hello förbrukning plan tilldelar automatiskt datorkraft när koden körs skalas ut som behövs toohandle belastning och skalas när koden inte körs. Därför du har inte toopay för inaktiv virtuella datorer och har inte tooreserve kapacitet i förväg. Den här artikeln fokuserar på hello förbrukning plan. Mer information om hur hello programtjänstplan fungerar finns hello [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Om du inte är bekant med Azure Functions finns hello [översikt över Azure Functions](functions-overview.md).

När du skapar en funktionsapp, måste du konfigurera en plan för värd för funktioner hello appen innehåller. I båda lägena, en instans av hello *Azure Functions värden* kör hello funktioner. hello typ av planen kontroller:

* Hur värdinstanser skalas ut.
* hello-resurser som är tillgängliga tooeach värden.

För närvarande måste du välja hello typ under hello skapandet av hello funktionsapp. Du kan inte ändra efteråt. 

Du kan skala mellan nivåer på hello App Service-plan. På hello förbrukning plan hanterar Azure Functions automatiskt alla resursallokering.

## <a name="consumption-plan"></a>Förbrukningsplan

När du använder en plan för förbrukning läggs instanser av hello Azure Functions värden dynamiskt och tas bort baserat på hello antalet inkommande händelser. Den här planen skalar automatiskt och du debiteras för beräkningsresurser bara när din funktion körs. På en plan för förbrukning köra en funktion för högst 10 minuter. 

> [!NOTE]
> hello Standardtidsgräns för funktioner på en plan för förbrukning är 5 minuter. hello-värdet kan vara bättre too10 minuter för hello Funktionsapp genom att ändra egenskapen hello `functionTimeout` i [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

Fakturering baseras på körningstid och minne som används och den sammanställs över alla funktioner i en funktionsapp. Mer information finns i hello [Azure Functions sida med priser].

hello förbrukning plan är hello standard och ger hello följande fördelar:
- Betala endast när din funktion körs.
- Skala ut automatiskt, även under perioder med hög att läsa in.

## <a name="app-service-plan"></a>App Service-plan

I hello Apptjänst planerar, funktionen apparna körs på dedikerade virtuella datorer på Basic, Standard och Premium-SKU: er, liknande tooWeb appar. Dedikerade virtuella datorer är allokerade tooyour Apptjänst-appar, vilket innebär att hello funktioner värden körs alltid.

Överväg att en apptjänstplan i hello följande fall:
- Du har befintliga, underutnyttjade virtuella datorer som redan kör andra Apptjänst-instanser.
- Du förväntar dig din funktion appar toorun kontinuerligt eller nästan kontinuerligt.
- Du behöver fler alternativ för CPU eller minne än vad som tillhandahålls på hello förbrukning plan.
- Du måste toorun som är längre än hello maximal körningstid tillåts på hello förbrukning plan.

En virtuell dator frikopplar kostnad från både runtime och minne storlek. Därför kan betalar du inte mer än hello kostnaden för hello VM-instans som du allokera. Mer information om hur hello programtjänstplan fungerar finns hello [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Du kan skala ut manuellt genom att lägga till flera VM-instanser med en App Service-plan eller du kan aktivera Autoskala. Mer information finns i [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Du kan även skala upp genom att välja en annan programtjänstplan. Mer information finns i [skala upp en app i Azure](../app-service-web/web-sites-scale.md). Om du planerar toorun JavaScript-funktioner på en apptjänstplan, bör du välja en plan som har färre kärnor. Mer information finns i hello [JavaScript-referens för funktioner](functions-reference-node.md#choose-single-core-app-service-plans).  

<!-- Note: hello portal links toothis section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Always On

Om du kör på en apptjänstplan, bör du aktivera hello **alltid på** inställningen så att funktionen appen körs korrekt. På en apptjänstplan går hello functions-runtime inaktiv efter några minuter av inaktivitet, så att endast HTTP-utlösare kommer ”väcka” dina funktioner. Detta är liknande toohow WebJobs måste ha alltid aktiverat. 

Always On är bara tillgängliga på en App Service-plan. På en plan för förbrukning aktiverar hello plattform funktionen appar automatiskt.

## <a name="storage-account-requirements"></a>Lagringskraven för kontot

En funktionsapp kräver ett Azure Storage-konto som stöder Azure Blob, köer och tabellen lagring på en plan för förbrukning eller en App Service-plan. Azure Functions används internt, Azure Storage för åtgärder som hanterar utlösare och loggning funktionen körningar. Vissa storage-konton stöder inte köer och tabeller, t.ex endast blob storage-konton (inklusive premium-lagring) och allmänna lagringskonton med zonredundant lagringsreplikering. De här kontona filtrerat från hello **Lagringskonto** bladet när du skapar en funktionsapp.

toolearn mer om lagringskontotyper finns [introduktion till hello Azure Storage-tjänster](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-hello-consumption-plan-works"></a>Så här fungerar hello förbrukning plan

hello förbrukning plan skalas automatiskt Processortid och minnesresurser genom att lägga till ytterligare instanser av hello funktioner värd, baserat på hello antalet händelser som utlöses på dess funktioner. Varje instans av hello funktioner värden är begränsad too1.5 GB minne.

När du använder Hej förbrukning värd plan, funktionen kodfiler lagras på Azure-filresurser på hello huvudsakliga storage-konto. När du tar bort hello huvudsakliga storage-konto raderas innehållet och kan inte återställas.

> [!NOTE]
> När du använder en blob-utlösare på en plan för användning kan det finnas upp tooa 10 minuters fördröjning vid bearbetningen av nya blobbar om en funktionsapp är inaktiv. När hello funktionen appen körs bearbetas blobbar omedelbart. Det här första tooavoid fördröjning bör du överväga att något av följande alternativ för hello:
> - Använda en apptjänstplan med alltid på aktiverad.
> - Använd tootrigger hello av en annan mekanism-blob som bearbetning, till exempel ett kömeddelande som innehåller hello blob-namnet. Ett exempel finns [kön utlösare med blob inkommande bindningen](functions-bindings-storage-blob.md#input-sample).

### <a name="runtime-scaling"></a>Runtime skalning

Azure Functions använder en komponent som kallas hello *skala controller* toomonitor hello frekvensen för händelser och ta reda på om tooscale ut eller skala. hello skala domänkontrollanten använder heuristik för varje typ av utlösare. Till exempel när du använder en Azure Queue storage-utlösare skalningen baserat på hello kölängd och hello ålder hello äldsta meddelandet i kön.

hello är skala hello funktionsapp. När hello funktionsapp skalas ut mer resurser som är allokerade toorun flera instanser av hello Azure Functions värden. Däremot bort hello skala controller funktionen värddatorinstanser som compute begäran minskas. hello antal instanser för skalas till slut ned toozero när inga funktioner som körs inom en funktionsapp.

![Skala controller händelseövervakning och skapa instanser](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Faktureringsmodell

Faktureringen för hello förbrukning planen beskrivs i detalj på hello [Azure Functions sida med priser]. Användning sammanställs på hello funktionen app-nivå och räknar endast hello-tid som Funktionskoden körs. hello följande är enheter för fakturering: 
* **Resursförbrukning i GB-sekunder (GB-s)**. Beräknad som en kombination av minnesstorlek och körningstiden för alla funktioner i en funktionsapp. 
* **Körningar**. Räknas varje gång som en funktion som ska köras i svaret tooan-händelsen som utlöses av en bindning.

[Azure Functions sida med priser]: https://azure.microsoft.com/pricing/details/functions
