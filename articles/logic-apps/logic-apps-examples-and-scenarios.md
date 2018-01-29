---
title: Exempel & vanliga scenarier - Azure Logic Apps | Microsoft Docs
description: "Mer information om logikappar med exempel, scenarier, självstudier och genomgång"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 09/13/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: b88d0c1ccb7a729c95299bcdc3cba5fd73fcdeac
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Vanliga scenarier, exempel, självstudier och genomgång för Azure Logic Apps

[Med Azure Logikappar](../logic-apps/logic-apps-overview.md) hjälper dig att samordna och integrera olika tjänster genom att tillhandahålla [100 + färdiga att använda kopplingar](../connectors/apis-list.md), räckvidd från lokala SQLServer eller SAP på kognitiva Microsoft-tjänster. Tjänsten Logic Apps är ”serverlösa”, så du inte behöver bekymra dig om skala eller instanser. Allt du behöver göra är att definiera arbetsflödet med en utlösare och åtgärder som utförs av arbetsflödet. Den underliggande plattformen hanterar skala, tillgänglighet och prestanda. Logic Apps är särskilt användbart för användningsfall och scenarier där du behöver samordna flera åtgärder över flera system.

För att lära dig mer om många mönster och funktioner som [Azure Logikappar](../logic-apps/logic-apps-overview.md) stöder, här är vanliga exempel och scenarier.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Vanliga startpunkter för logik app arbetsflöden

Varje logikappen som börjar med en [ *utlösaren*](../logic-apps/logic-apps-overview.md#logic-app-concepts), och endast en utlösare som startar logik app arbetsflödet och godkänns i alla data som en del av denna utlösare. Vissa anslutningar innehåller utlösare som kommer i de här typerna:

* *Avsökningen utlösare*: regelbundet kontrollerar en tjänstslutpunkt för nya data. När det finns nya data, utlösaren skapar och kör en ny arbetsflödesinstans med data som indata.

* *Push-utlösare*: lyssnar efter data på en tjänstslutpunkt och väntar tills en viss händelse inträffar. När händelsen inträffar utlöses utlösaren omedelbart, skapa och köra en ny arbetsflödesinstans som använder alla tillgängliga data som indata.

Här följer några vanliga utlösare exempel:

* Avsökningsintervall: 

  * [**Schema - upprepning** utlösaren](../connectors/connectors-native-recurrence.md) kan du ange datumet och tid plus upprepningen för startar din logikapp. 
  Exempelvis kan du välja dagarna och tidpunkter på dagen för att utlösa logikappen.

  * Utlösaren ”när ett e-postmeddelande tas emot” låter din logikapp söka efter ny e-post från alla e-leverantör som stöds av Logikappar, till exempel [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/)och så vidare.

  * Den [ **HTTP** utlösaren](../connectors/connectors-native-http.md) kan din logikapp Kontrollera angivna tjänstslutpunkten av kommunikation över HTTP.
  
* Push:

  * Den [ **begäranden / svar - begäran** utlösaren](../connectors/connectors-native-reqres.md) kan din logikapp ta emot HTTP-begäranden och svara i realtid på händelser på något sätt.

  * Den [ **HTTP Webhook** utlösaren](../connectors/connectors-native-webhook.md) prenumererar på en tjänstslutpunkt genom att registrera en *motringning URL* med den tjänsten. 
  På så sätt kan tjänsten kan bara meddela utlösaren när den angivna händelsen sker, så att utlösaren inte behöver avsöka tjänsten.

När du har fått ett meddelande om nya data eller en händelse utlösaren utlöses, skapar en ny arbetsflödesinstans för logik-app och kör åtgärderna i arbetsflödet. Du kan komma åt alla data från utlösare i hela arbetsflödet. Exempelvis skickar utlösaren ”på en ny tweet” tweet innehållet i logikappen som kör. 

## <a name="respond-to-triggers-and-extend-actions"></a>Svara på utlösare och utöka åtgärder

Du kan utöka logikappar för datorer och tjänster som inte kanske har publicerats kopplingar.

* [Skapa anpassade utlösare eller åtgärder](../logic-apps/logic-apps-create-api-app.md)
* [Ange långvariga åtgärder för arbetsflödet körs](../logic-apps/logic-apps-create-api-app.md)
* [Svara på externa händelser och åtgärder med webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Anropa utlösare eller kapsla arbetsflöden med synkron svar på HTTP-begäranden](../logic-apps/logic-apps-http-endpoint.md)
* [Självstudier: Skapa en AI-påslagen sociala instrumentpanel i minuter med Logic Apps och Power BI](http://aka.ms/logicappsdemo)
* [Självstudier: Svara på SMS Twilio webhooks och skicka ett textsvar](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Kontrollflöde felhantering och funktioner för loggning

Logikappar innehåller omfattande funktioner för avancerad Kontrollflöde som villkor, växlar, slingor och omfång. För att säkerställa flexibla lösningar, kan du även implementera fel och undantagshantering i dina arbetsflöden. För meddelanden och diagnostikloggar som arbetsflödet körs status innehåller också Azure Logikappar övervakning och aviseringar.

* [Artiklar i matriser och samlingar med slingor och batchar i logikappar](../logic-apps/logic-apps-loops-and-scopes.md)
* [Utför olika åtgärder med växeln-instruktioner](../logic-apps/logic-apps-switch-case.md)
* [Författare fel och undantagshantering i ett arbetsflöde](../logic-apps/logic-apps-exception-handling.md)
* [Användningsfall: hur sjukvården företaget använder logik app undantagshantering för HL7 FHIR arbetsflöden](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Aktivera övervakning, loggning och aviseringar för befintliga logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Aktivera övervakning och diagnostikloggning när du skapar logikappar](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Distribuera och hantera logikappar

Du kan fullständigt utveckla och distribuera logikappar med Visual Studio, Visual Studio Team Services eller andra källkontrollen och automatiserade verktyg. För att stödja distribution för arbetsflöden och beroende anslutningar i en resursmall för, Använd logikappar mallar för distribution av Azure-resurs. Visual Studio tools generera automatiskt dessa mallar som du kan checka in till källkontroll för versionshantering.

* [Skapa och distribuera logikappar från Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Aktivera övervakning, loggning och aviseringar för befintliga logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Skapa en mall för automatisk distribution](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Typer av innehåll och konverteringar transformationer inom en körning

Du kan använda, konvertera och omvandla flera typer av innehåll med många funktioner i Azure Logikappar [språk i arbetsflödesdefinitionen](http://aka.ms/logicappsdocs). Exempelvis kan du konvertera mellan en sträng, JSON och XML med den `@json()` och `@xml()` arbetsflödesuttryck. Motorn för Logic Apps bevarar typer av innehåll för att stödja innehållsöverföring i en förlustfri sätt mellan tjänster.

* [Hur arbetsflödesuttryck fungerar i logikappar](../logic-apps/logic-apps-author-definitions.md)
* [Hantera icke JSON innehållstyper](../logic-apps/logic-apps-content-type.md), till exempel `application/xml`, `application/octet-stream`, och`multipart/formdata`
* [Referens: Språk i Arbetsflödesdefinitionen för Azure Logikappar](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Andra integreringar och funktioner

Logikappar erbjuder också integrering med många tjänster, till exempel Azure Functions, Azure API Management, Azure App Service och anpassade HTTP-slutpunkter, till exempel REST och SOAP.

* [Skapa en realtid sociala instrumentpanel med Azure serverlösa](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Anropa Azure Functions från logikappar](../logic-apps/logic-apps-azure-functions.md)
* [Scenario: Utlösa logikappar med Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Blogg: Anropa SOAP-slutpunkter från logikappar](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Slutpunkt-till-slutpunkt-scenarier

* [Whitepaper: Enterprise integration slutpunkt till slutpunkt ärendehantering med Azure-tjänster som Logic Apps](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="next-steps"></a>Nästa steg

* [Författare arbetsflödesdefinitioner med definitionsspråk för arbetsflöde](../logic-apps/logic-apps-author-definitions.md)
* [Hantera fel och undantag i logikappar](../logic-apps/logic-apps-exception-handling.md)
* [Skicka dina kommentarer, frågor, feedback och förslag för att vi kan förbättra Azure Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)