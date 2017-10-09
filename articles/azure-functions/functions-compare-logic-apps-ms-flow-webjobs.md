---
title: "aaaChoose mellan flödet, Logic Apps, funktioner och WebJobs | Microsoft Docs"
description: "Jämför och kontrast hello för integrering av molntjänster från Microsoft och bestämma vilka tjänster som du ska använda."
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
ms.openlocfilehash: 6becc1e389698e517924b18295dac4375542d524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Välj mellan Flow, Logic Apps, Functions och WebJobs
Den här artikeln Jämför och förklarar hello följande tjänster i hello Microsoft moln, vilket kan alla lösa problem med integration och automatisering av affärsprocesser:

* [Microsoft-flöde](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure Apptjänst WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Dessa tjänster är användbara när ”fäster” ihop olika system. De kan alla definiera indata, åtgärder, villkor och utdata. Du kan köra dem på ett schema eller utlösare. Varje tjänst lägger till en unik uppsättning värdet och jämföra dem är inte en fråga av ”vilken tjänst är hello bästa”? men ”som bäst passar i den här situationen”? En kombination av dessa tjänster är ofta hello bästa sättet toorapidly skapa en skalbar, fullständig funktionalitet integrationslösning.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flödet vs. Logic Apps
Vi kan diskutera Microsoft Flow och Azure Logic Apps tillsammans eftersom de båda *konfigurationen första* integration services, vilket gör det enkelt toobuild processer och arbetsflöden och integrera med olika SaaS- och enterprise program. 

* Flödet bygger på Logic Apps
* De har hello samma arbetsflödesdesigner
* [Kopplingar](../connectors/apis-list.md) arbete i ett kan också fungera i hello andra

Flöden ger en enkel integrering med office worker tooperform (t.ex. hämta SMS för viktiga e-postmeddelanden) utan att gå via utvecklare eller IT. Hej på andra sidan, Logic Apps kan aktivera avancerade eller verksamhetskritiska integreringar (t.ex. B2B processer) där företagsnivå DevOps och säkerhet praxis krävs. Det är typiska för ett företag arbetsflöde toogrow komplexitet övertid. Därför kan du börja med ett flöde först och sedan konvertera den tooa logikapp efter behov.

hello hjälper nedan dig att avgöra om flödet eller Logic Apps är bäst för en viss integration.

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
Vi kan diskutera Azure Functions och Azure App Service WebJobs tillsammans eftersom de båda *kod första* integration services och avsedd för utvecklare. Kan du toorun ett skript eller en del av koden i svaret toovarious händelser, t.ex [nya Storage-Blobbar](functions-bindings-storage.md) eller [en WebHook-begäran](functions-bindings-http-webhook.md). Här följer sina likheter: 

* Både bygger på [Azure App Service](../app-service/app-service-value-prop-what-is.md) och njut av funktioner som [kontroll](../app-service-web/app-service-continuous-deployment.md), [autentisering](../app-service/app-service-authentication-overview.md), och [övervakning](../app-service-web/web-sites-monitor.md).
* Båda är utvecklare tjänster.
* Både stöd standard skript- och programmeringsspråk.
* Har båda NuGet och NPM stöd.

Funktioner är hello naturlig utvecklingen av WebJobs eftersom det tar WebJobs hello fördelarna och förbättrar på den. hello-förbättringar är: 

* Effektiviserad dev testa och köra kod direkt i hello webbläsare.
* Inbyggd integrering med flera Azure-tjänster och 3-tjänster som [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
* Betala per användning, utan behov av toopay för en [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatisk [dynamisk skalning](functions-scale.md).
* För befintliga kunder av App Service körs på programtjänstplanen fortfarande möjligt (tootake nytta av outnyttjad resurser).
* Integrering med Logic Apps.

hello följande tabell sammanfattas hello skillnaderna mellan funktioner och WebJobs:

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

Om toouse funktioner eller WebJobs beror i slutändan på vad du redan gör med App Service. Om du har en Apptjänst-app som du vill toorun kodstycken och du vill toomanage dem tillsammans i hello samma DevOps-miljö bör du använda WebJobs. Om du vill toorun kodfragment för andra Azure-tjänster eller även 3 parts appar eller om du vill hantera för integrering kodavsnitt separat från din Apptjänst-appar, eller om du vill toocall kodavsnitt från en logikapp, bör du dra nytta av alla hello förbättringar i funktioner.  

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Flöda, Logic Apps och fungerar tillsammans
Som tidigare nämnts beror vilken tjänst som är bäst lämpade tooyou på din situation. 

* Enkel affärsoptimering sedan använda flödet.
* Om ditt integration scenario för avancerade för flöde eller måste DevOps-funktioner och säkerheten compliances, använder du Logic Apps.
* Om ett steg i ditt scenario integration kräver hög anpassade omvandling eller särskilda kod kan skriva en funktionsapp och sedan utlösa en funktion som en åtgärd i din logikapp.

Du kan anropa en logikapp i ett flöde. Du kan också anropa en funktion i en logikapp och en logikapp i en funktion. hello integrering mellan flödet och Logic Apps funktioner fortsätta tooimprove övertid. Du kan skapa något i en tjänst och använda den i hello andra tjänster. Andra investeringar som du gör i dessa tre tekniker är därför lönar.

## <a name="next-steps"></a>Nästa steg
Kom igång med varje hello-tjänster genom att skapa din första flödet, logikapp, funktionsapp eller Webbjobb. Klicka på någon av följande länkar hello:

* [Kom igång med Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)
* [Skapa din första Azure-funktion](functions-create-first-azure-function.md)
* [Distribuera WebJobs med hjälp av Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Eller skaffa mer information om dessa integreringstjänster med hello följande länkar:

* [Utnyttja Azure Functions & Azure App Service för integrationsscenarier av Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integreringar enklare av Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Live webbsändningar](http://aka.ms/logicappslive)
* [Microsoft flöda vanliga frågor och svar](https://flow.microsoft.com/documentation/frequently-asked-questions/)
* [Azure WebJobs-dokumentation](../app-service-web/websites-webjobs-resources.md)

