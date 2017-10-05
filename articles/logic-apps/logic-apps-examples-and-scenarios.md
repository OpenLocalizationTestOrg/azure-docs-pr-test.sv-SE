---
title: Exempel & vanliga scenarier - Azure Logic Apps | Microsoft Docs
description: "Mer information om logikappar med exempel och scenarier självstudier"
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
ms.workload: integration
ms.date: 08/9/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 50df1e3db239a6aa34ac91bfbd582625c5b0041b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="examples-and-common-scenarios-for-azure-logic-apps"></a>Exempel och vanliga scenarier för Azure Logic Apps

För att lära dig mer om de många mönster och funktionerna i Logikappar i Azure kan du här är vanliga exempel och scenarier.

## <a name="key-scenarios-for-logic-apps"></a>Viktiga scenarier för logikappar

Med Azure Logikappar tillhandahåller flexibel orchestration och -integrering för olika tjänster. Logikappar tjänsten är ”serverlösa”, så du inte behöver bekymra dig om skala eller instanser - allt du behöver göra är att definiera arbetsflödet (utlösare och åtgärder). Den underliggande plattformen hanterar skala, tillgänglighet och prestanda. Scenarier där du behöver samordnar flera åtgärder, speciellt över flera system är ett bra användningsfall för Logikappar i Azure. Här följer några mönster och exempel.

## <a name="respond-to-triggers-and-extend-actions"></a>Svara på utlösare och utöka åtgärder

Varje logikapp börjar med en utlösare. Arbetsflödet kan till exempel starta med en schemahändelse, en manuell anrop eller en händelse från ett externt system, till exempel utlösaren ”när en fil har lagts till en FTP-server”. Med Azure Logikappar stöder för närvarande över 100 färdiga att använda kopplingar, allt från lokal SAP till kognitiva Microsoft-tjänster. Du kan utöka logikappar för datorer och tjänster som inte kanske har publicerats kopplingar.

* [Skapa anpassade utlösare eller åtgärder](../logic-apps/logic-apps-create-api-app.md)
* [Ange långvariga åtgärder för arbetsflödet körs](../logic-apps/logic-apps-create-api-app.md)
* [Svara på externa händelser och åtgärder med webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Anropa utlösare eller kapsla arbetsflöden med synkron svar på HTTP-begäranden](../logic-apps/logic-apps-http-endpoint.md)
* [Självstudier: Svara på SMS Twilio webhooks och skicka ett textsvar](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)
* [Självstudier: Skapa en AI-påslagen sociala instrumentpanel i minuter med Logic Apps och Power BI](http://aka.ms/logicappsdemo)

## <a name="error-handling-logging-and-control-flow-capabilities"></a>Felhantering, loggning och kontroll flödet funktioner

Logikappar innehåller omfattande funktioner för avancerad Kontrollflöde som villkor, växlar, slingor och omfång. För att säkerställa flexibla lösningar, kan du även implementera fel och undantagshantering i dina arbetsflöden. För meddelanden och diagnostikloggar som arbetsflödet körs status innehåller också Azure Logikappar övervakning och aviseringar.

* [Utför olika åtgärder med växeln-instruktioner](../logic-apps/logic-apps-switch-case.md)
* [Artiklar i matriser och samlingar med slingor och batchar i logikappar](../logic-apps/logic-apps-loops-and-scopes.md)
* [Författare fel och undantagshantering i ett arbetsflöde](../logic-apps/logic-apps-exception-handling.md)
* [Aktivera övervakning, loggning och aviseringar för befintliga logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Aktivera övervakning och diagnostikloggning när du skapar logikappar](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)
* [Användningsfall: hur sjukvården företaget använder logik app undantagshantering för HL7 FHIR arbetsflöden](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

## <a name="deploy-and-manage-logic-apps"></a>Distribuera och hantera logikappar

Du kan fullständigt utveckla och distribuera logikappar med Visual Studio, Visual Studio Team Services eller andra källkontrollen och automatiserade verktyg. För att stödja distribution för arbetsflöden och beroende anslutningar i en resursmall för, Använd logikappar mallar för distribution av Azure-resurs. Visual Studio tools generera automatiskt dessa mallar som du kan checka in till källkontroll för versionshantering.

* [Skapa en mall för automatisk distribution](../logic-apps/logic-apps-create-deploy-template.md)
* [Skapa och distribuera logikappar från Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Övervaka hälsotillståndet hos dina logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Typer av innehåll och konverteringar transformationer inom en körning

Du kan använda, konvertera och omvandla flera typer av innehåll med många funktioner i Azure Logikappar [språk i arbetsflödesdefinitionen](http://aka.ms/logicappsdocs). Exempelvis kan du konvertera mellan en sträng, JSON och XML med den `@json()` och `@xml()` arbetsflödesuttryck. Motorn för Logic Apps bevarar typer av innehåll för att stödja innehållsöverföring i en förlustfri sätt mellan tjänster.

* [Hantera icke JSON innehållstyper](../logic-apps/logic-apps-content-type.md), till exempel `application/xml`, `application/octet-stream`, och`multipart/formdata`
* [Hur arbetsflödesuttryck fungerar i logikappar](../logic-apps/logic-apps-author-definitions.md)
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

- [Hantera fel och undantag i logikappar](../logic-apps/logic-apps-exception-handling.md)
- [Författare arbetsflödesdefinitioner med definitionsspråk för arbetsflöde](../logic-apps/logic-apps-author-definitions.md)
- [Skicka dina kommentarer, frågor, feedback och förslag för att vi kan förbättra Azure Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)