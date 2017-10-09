---
title: "aaaAzure översikt över Functions | Microsoft Docs"
description: "Förstå hur toouse Azure Functions toooptimize asynkrona arbetsbelastningar i minuter."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 668f9055a007fee662a4c7ec774d3c0a1d847800
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-functions"></a>En introduktion tooAzure fungerar  
Azure Functions är en lösning för att enkelt köra små delar av kod eller ”funktioner”, i hello molnet. Du kan skriva precis hello kod du behöver för hello problemet, utan att oroa en hela programmet eller hello infrastruktur toorun den. Functions kan göra utvecklingen ännu mer produktiv och du kan använda det programmeringsspråk du föredrar, till exempel C#, Node.js, Python eller PHP. Betalar bara för hello tiden koden körs och lita på Azure tooscale efter behov. Azure Functions låter dig utveckla utan program utan server på Microsoft Azure.

Det här ämnet innehåller en översikt över Azure Functions. Om du vill använda toojump höger och kom igång med Azure Functions, börja med [skapa din första Azure-funktion](functions-create-first-azure-function.md). Om du behöver mer teknisk information om funktioner finns hello [för utvecklare](functions-reference.md).

## <a name="features"></a>Funktioner
Här följer några funktioner i Azure Functions:

* **Val av språk** – Skriv funktioner med hjälp av C#, F#, Node.js, Python, PHP, batch, bash eller andra körbara filer.
* **Betala per tillfälle Prismodell** – betala endast för hello tid använt för att köra din kod. Se hello förbrukning värd planera alternativ i hello [prissättningsavsnittet](#pricing).  
* **Ta med dina egna beroenden** – Functions stöder NuGet och NPM, så du kan använda dina favoritbibliotek.  
* **Integrerad säkerhet** – Skydda HTTP-utlösta funktioner med OAuth-providrar, till exempel Azure Active Directory, Facebook, Google, Twitter och Microsoft Account.  
* **Förenklad integration** – Utnyttja Azure-tjänster och SaaS-erbjudanden (programvara som en tjänst) enkelt. Se hello [avsnittet integreringar](#integrations) för några exempel.  
* **Flexibel utveckling** – koda dina funktioner direkt i hello-portalen eller Ställ in kontinuerlig integration och distribuera din kod via GitHub, Visual Studio Team Services och andra [utvecklingsverktyg som stöds](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Öppen källkod** -hello Functions-runtime är öppen källkod och [finns på GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Vad kan jag göra med Functions?
Azure Functions är en bra lösning för bearbetning av data, integrera system, arbeta med hello-sakernas internet (IoT) och skapa enkla API: er och mikrotjänster. Överväg att funktioner för uppgifter som bild eller Orderbearbetning, filunderhåll, eller för alla aktiviteter som du vill toorun enligt ett schema. 

Functions innehåller mallar tooget du igång med viktiga scenarier, inklusive hello följande:

* **BlobTrigger** -processen Azure Storage-blobbar när de läggs toocontainers. Du kan använda den här funktionen för storleksändring.
* **EventHubTrigger** -svara tooevents levereras tooan Azure Event Hub. Särskilt användbart i programinstrumentation, bearbetning av användarupplevelser och arbetsflöden samt scenarier i Sakernas Internet (IoT).
* **Allmän webhook** – Bearbeta webhook-HTTP-begäranden från alla tjänster som stöder webhooks.
* **GitHub-webhook** -svara tooevents som förekommer i dina GitHub-databaser. Ett exempel finns i [Skapa en webhook eller API-funktion](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** -utlösa hello körning av din kod genom att använda en HTTP-begäran.
* **QueueTrigger** -svara toomessages som tas emot i en Azure Storage-kö. Ett exempel finns [skapa en Azure-funktion som binder tooan Azure-tjänsten](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** – ansluta din kod tooother Azure-tjänster eller lokala tjänster genom att lyssna toomessage köer. 
* **ServiceBusTopicTrigger** – ansluta din kod tooother Azure-tjänster eller lokala tjänster genom att prenumerera tootopics. 
* **TimerTrigger** – Köra rensning eller andra batchaktiviteter enligt ett fördefinierat schema. Ett exempel finns i [Skapa en funktion för händelsebearbetning](functions-create-an-event-processing-function.md).

Azure Functions stöder *utlösare*, vilket är sätt toostart körning av din kod och *bindningar*, vilket är sätt toosimplify kodning för indata och utdata. En detaljerad beskrivning av hello utlösare och bindningar som ger Azure Functions finns [Azure Functions-utlösare och bindningar utvecklare](functions-triggers-bindings.md).

## <a name="integrations"></a>Integreringar
Azure Functions kan integreras med en mängd Azure- och tredjepartstjänster. Du kan använda tjänsterna för att utlösa funktionen och starta körning, eller så kan de fungera som indata och utdata för din kod. hello följande tjänsteintegreringar stöds av Azure Functions. 

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Mobile Apps (tabeller)
* Azure Notification Hubs
* Azure Service Bus (köer och ämnen)
* Azure Storage (blob, köer och tabeller) 
* GitHub (webhooks)
* Lokal (med hjälp av Service Bus)
* Twilio (SMS-meddelanden)

## <a name="pricing"></a>Hur mycket kostar Functions?
Azure Functions har två typer av prissättningar, Välj hello som bäst passar dina behov: 

* **Planera förbrukningen** – när din funktion körs, tillhandahåller Azure alla hello nödvändiga beräkningsresurser. Du har inte tooworry om resurshantering och du betalar bara för hello tiden koden körs. 
* **Apptjänstplan** – Kör dina funktioner precis som dina webb-, mobil- och API Apps. När du redan använder App Service för dina andra program, kan du köra dina funktioner på hello samma plan utan extra kostnad. 

Fullständig prisinformation är tillgängliga på hello [prissättning för Functions](https://azure.microsoft.com/pricing/details/functions/). Mer information om att skala dina funktioner finns [hur tooscale Azure Functions](functions-scale.md).

## <a name="next-steps"></a>Nästa steg
* [Skapa din första Azure-funktion](functions-create-first-azure-function.md)  
  Komma igång snabbt och skapa din första funktion med hjälp av hello Azure Functions-Snabbstart. 
* [Azure Functions, info för utvecklare](functions-reference.md)  
  Ger mer teknisk information om hello Azure Functions-runtime och en referens för kodning av funktioner och definiering av utlösare och bindningar.
* [Testa Azure Functions](functions-test-a-function.md)  
  Beskriver olika verktyg och tekniker för att testa funktioner.
* [Hur tooscale Azure Functions](functions-scale.md)  
  Beskriver tillgängliga med Azure Functions, inklusive hello förbrukning värd planerings- och hur toochoose hello rätt plan serviceplaner. 
* [Läs mer om Azure App Service](../app-service/app-service-value-prop-what-is.md)  
  Azure Functions utnyttjar plattformen för hello Azure App Service för grundläggande funktioner som distributioner, miljövariabler och diagnostik. 

