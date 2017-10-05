---
title: "Välj mellan flödet, Logikappar, funktioner och WebJobs | Microsoft Docs"
description: "Jämför och jämför den för molnet integration tjänster från Microsoft och bestämma vilka tjänster som du ska använda."
services: functions,app-service\logic
documentationcenter: na
author: cephalin
manager: wpickett
tags: 
keywords: "Microsoft flödet, flöde, logikappar, azure-funktioner, funktioner, azure webjobs, webjobs, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: da2ff16b5bdd7a0c171451930ce10427fe5bbda7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Välj mellan Flow, Logic Apps, Functions och WebJobs
Den här artikeln Jämför och förklarar följande tjänster i Microsoft-moln, vilket kan alla lösa problem med integration och automatisering av affärsprocesser:

* [Microsoft-flöde](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure Apptjänst WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Dessa tjänster är användbara när ”fäster” ihop olika system. De kan alla definiera indata, åtgärder, villkor och utdata. Du kan köra dem på ett schema eller utlösare. Varje tjänst lägger till en unik uppsättning värdet och jämföra dem är inte en fråga av ”vilken tjänst är bäst”? men ”som bäst passar i den här situationen”? En kombination av dessa tjänster är ofta det bästa sättet att snabbt skapa en skalbar, fullständig funktionalitet integrationslösning.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flödet vs. Logic Apps
Vi kan diskutera Microsoft Flow och Azure Logic Apps tillsammans eftersom de båda *konfigurationen första* integration services, vilket gör det enkelt att skapa processer och arbetsflöden och integrera med olika SaaS- och enterprise program. 

* Flödet bygger på Logic Apps
* De har samma Arbetsflödesdesignern
* [Kopplingar](../connectors/apis-list.md) arbete i ett kan också fungera i den andra

Flöden ger alla office worker att utföra enkla integreringar (t.ex. hämta SMS för viktiga e-postmeddelanden) utan att gå via utvecklare eller IT. Logic Apps kan å andra sidan Aktivera avancerade eller verksamhetskritiska integreringar (t.ex. B2B processer) där företagsnivå DevOps och säkerhet praxis krävs. Det är vanligt för ett företagsarbetsflöde att växa i komplexitet övertid. Därför kan du börja med ett flöde först och sedan konvertera den till en logikapp efter behov.

Tabellen nedan hjälper dig att avgöra om flödet eller Logic Apps är bäst för en viss integration.

|  | Flöde | Logic Apps |
| --- | --- | --- |
| Målgrupp |kontorsarbetarna företagsanvändare |IT-proffs utvecklare |
| Scenarier |Självbetjäning |Verksamhetskritiska |
| Designverktyg |I webbläsaren och mobila appen, endast användargränssnitt |I webbläsaren och [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [vyn kod](../logic-apps/logic-apps-author-definitions.md) tillgängliga |
| DevOps |Ad hoc-utveckla i produktion |käll-kontroll, testa, support och automation och hanterbarhet i [Azure Resource Manager](../logic-apps/logic-apps-arm-provision.md) |
| Administratörsupplevelse |[https://Flow.microsoft.com](https://flow.microsoft.com) |[https://Portal.Azure.com](https://portal.azure.com) |
| Säkerhet |Standard praxis: [data suveränitet](https://wikipedia.org/wiki/Technological_Sovereignty), [kryptering i vila](https://wikipedia.org/wiki/Data_at_rest#Encryption) känsliga data, osv. |Säkerhet försäkran för Azure: [Azure-säkerhet](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [granskningsloggar](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/), med mera. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Jämfört med funktioner Webbjobb
Vi kan diskutera Azure Functions och Azure App Service WebJobs tillsammans eftersom de båda *kod första* integration services och avsedd för utvecklare. Du kan köra ett skript eller en typ av kod som svar på olika händelser som [nya Storage-Blobbar](functions-bindings-storage.md) eller [en WebHook-begäran](functions-bindings-http-webhook.md). Här följer sina likheter: 

* Både bygger på [Azure App Service](../app-service/app-service-value-prop-what-is.md) och njut av funktioner som [kontroll](../app-service-web/app-service-continuous-deployment.md), [autentisering](../app-service/app-service-authentication-overview.md), och [övervakning](../app-service-web/web-sites-monitor.md).
* Båda är utvecklare tjänster.
* Både stöd standard skript- och programmeringsspråk.
* Har båda NuGet och NPM stöd.

Funktioner är naturlig utvecklingen av WebJobs eftersom det tar WebJobs fördelarna och förbättrar på den. Förbättringarna innefattar: 

* Effektiviserad dev, testa och körning av koden direkt i webbläsaren.
* Inbyggd integrering med flera Azure-tjänster och 3-tjänster som [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
* Betala per tillfälle, behöver du inte betalar för en [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatisk [dynamisk skalning](functions-scale.md).
* För befintliga kunder av App Service körs på App Service-plan fortfarande möjligt (för att dra nytta av outnyttjad resurser).
* Integrering med Logic Apps.

I följande tabell sammanfattas skillnaderna mellan funktioner och WebJobs:

|  | Funktioner | Webbjobb |
| --- | --- | --- |
| Skalning |Configurationless skalning |skala med App Service-plan |
| Prissättning |Betala per användning eller del av App Service-plan |En del av App Service-plan |
| Kör-typ |Utlöses, schemalagda (med timer som utlösare) |utlösta, kontinuerlig schemalagda |
| Utlösaren händelser |[Tidsinställt](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-documentdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md), [Azure Apptjänst Mobilappar](functions-bindings-mobile-apps.md), [Azure Notification Hubs](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure Storage](functions-bindings-storage.md) |[Azure Storage](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](../app-service-web/websites-dotnet-webjobs-sdk-service-bus.md) |
| Utveckling i webbläsare |Stöds | Stöds inte |
| Fönstret skript |experiment |Stöds |
| PowerShell |experiment |Stöds |
| C# |Stöds |Stöds |
| F# |Stöds |Stöds inte |
| Bash |experiment |Stöds |
| PHP |experiment |Stöds |
| Python |experiment |Stöds |
| JavaScript |Stöds |Stöds |

Om du vill använda funktioner eller WebJobs slutligen beror på vad du redan gör med App Service. Om du har en Apptjänst-app som du vill köra kodstycken och du vill hantera dem tillsammans i samma DevOps-miljö, bör du använda WebJobs. Om du vill köra kodfragment för andra Azure-tjänster eller även 3 parts appar, eller om du vill hantera din integrering kodstycken separat från din Apptjänst-appar, eller om du vill anropa kodavsnitt från en logikapp, bör du dra nytta av alla i mprovements i funktioner.  

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Flöda, Logic Apps och fungerar tillsammans
Som tidigare nämnts beror vilken tjänst som är bäst för dig på din situation. 

* Enkel affärsoptimering sedan använda flödet.
* Om ditt integration scenario för avancerade för flöde eller måste DevOps-funktioner och säkerheten compliances, använder du Logic Apps.
* Om ett steg i ditt scenario integration kräver hög anpassade omvandling eller särskilda kod kan skriva en funktionsapp och sedan utlösa en funktion som en åtgärd i din logikapp.

Du kan anropa en logikapp i ett flöde. Du kan också anropa en funktion i en logikapp och en logikapp i en funktion. Integration mellan flödet och Logic Apps funktioner fortsätta att förbättra övertid. Du kan skapa något i en tjänst och använda den i de andra tjänsterna. Andra investeringar som du gör i dessa tre tekniker är därför lönar.

## <a name="next-steps"></a>Nästa steg
Kom igång med varje tjänst genom att skapa din första flödet, logikapp, funktionsapp eller Webbjobb. Klicka på någon av följande länkar:

* [Kom igång med Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)
* [Skapa din första Azure-funktion](functions-create-first-azure-function.md)
* [Distribuera WebJobs med hjälp av Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Eller skaffa mer information om dessa integreringstjänster med följande länkar:

* [Utnyttja Azure Functions & Azure App Service för integrationsscenarier av Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integreringar enklare av Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Live webbsändningar](http://aka.ms/logicappslive)
* [Microsoft flöda vanliga frågor och svar](https://flow.microsoft.com/documentation/frequently-asked-questions/)
* [Azure WebJobs-dokumentation](../app-service-web/websites-webjobs-resources.md)

