---
title: "aaaConnect appar och integrera data med arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Skapa arbetsflöden och automatisera processer genom att ansluta appar och integrera data med Azure Logic Apps."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07765c05-72a6-4169-a8ab-f6420bfbaf07
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: klam
ms.openlocfilehash: 53d4e165bb2205ddd56c1950719389725267ddea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-logic-apps"></a>Vad är Logic Apps?
Logic Apps ge ett sätt toosimplify och genomföra skalbara integreringar och arbetsflöden i hello molnet. Den ger en visuell designer toomodel och automatisera processen som en serie steg som kallas arbetsflöde.  Det finns [många kopplingar](../connectors/apis-list.md) över hello molnet och lokala tooquickly integrera olika tjänster och protokoll.  En logikapp börjar med en utlösare (som ”när ett konto läggs tooDynamics CRM-) och efter att börja många kombinationer av åtgärder, konverteringar och logik för villkoret.

hello fördelarna med Logic Apps inkludera hello följande:  

* Spara tid genom att skapa komplexa processer med hjälp av enkel toounderstand designverktyg
* Implementera mönster och arbetsflöden sömlöst som annars skulle vara svårt tooimplement i koden
* Kom snabbt igång med hjälp av mallar.
* Anpassa din logikapp med egna anpassade API:er, kod och åtgärder.
* Ansluta och synkronisera olika system lokalt och hello moln
* Utveckla med BizTalk Server, API Management, Azure Functions och Azure Service Bus med förstklassigt integrationsstöd.

Logic Apps är en helt hanterad iPaaS (integrering plattform som en tjänst) innebär att utvecklare inte toohave tooworry om hur du skapar värd, skalbarhet, tillgänglighet och hantering.  Logic Apps skalar upp automatiskt toomeet begäran.

![Designer för flödesappar](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Som vi redan nämnt kan du automatisera affärsprocesser med Logic Apps. Här följer några exempel:  

* Flytta filer överförs tooan FTP-servern till Azure Storage
* Behandla och dirigera order i lokala system och molnsystem.
* Övervaka alla tweets om ett visst ämne, analysera hello sentiment och skapa aviseringar och uppgifter för objekt som behöver uppföljning.

Scenarier som dessa kan konfigureras från hello visuella designern och utan att skriva en enda rad kod. Kom igång och [skapa din logikapp nu][create].  När den har skrivits kan en logikapp [snabbt distribueras och konfigureras om](../logic-apps/logic-apps-create-deploy-template.md) i flera miljöer och områden.

## <a name="why-logic-apps"></a>Vad är Logic Apps till för?
Logic Apps ger hastighet och skalbarhet till hello enterprise integration utrymme.  hello användarvänlighet hello designer mängd tillgänglig utlösare och åtgärder och kraftfulla verktyg Se centralisera din API: er för enklare än någonsin tidigare.  När företag flyttar mot digitalization, kan Logic Apps du tooconnect äldre och senaste system tillsammans.

Dessutom med vår [Enterprise Integration konto] [ biztalk] du kan skala toomature integrationsscenarier med hello kraften hos en [XML-meddelanden] [ xml], [handelspartnerhantering][tpm], med mera.

* **Designverktyg för enkelt toouse** -Logic Apps kan vara utformad slutpunkt till slutpunkt i hello webbläsare eller med Visual Studio tools. Börja med en utlösare – från ett enkelt schema toowhen skapas ett GitHub-problem. Sedan kan du dirigera valfritt antal åtgärder med hjälp av hello utförliga galleriet med kopplingar.
* **Ansluta API: er enkelt** -även skrivuppgifter som är lätt toodescribe är svåra tooimplement i koden. Logic Apps gör det enkelt tooconnect olika system. Om du vill använda tooconnect din marknadsföringslösning i molnet tooyour lokala faktureringssystem? Vill du toocentralize via API: er och system med en Enterprise Service Bus-meddelanden? Logic apps är hello snabbaste och mest tillförlitliga sättet toodeliver lösningar toothese problem.
* **Kom igång snabbt med mallar** -toohelp du börjar vi tillhandahåller en [galleri med mallar] [ templates] som gör det möjligt toorapidly skapa ett antal vanliga lösningar. Avancerade B2B lösningar toosimple SaaS-anslutningsbarhet, från och med några finns 'för' – hello galleriet är hello snabbaste sättet tooget igång med hello kraftfulla Logic Apps.
* **Inbyggd utökningsbarhet** -inte ser hello connector som du behöver? Logic Apps är utformade toowork med dina egna API: er och kod. Du kan enkelt skapa egna API app toouse som en anpassad koppling eller som anropar en [Azure-funktion](https://functions.azure.com) tooexecute kodavsnitt för koden på begäran. 
* **Verklig integrationskraft** – Starta enkelt och utöka i takt med dina behov. Logic Apps kan enkelt utnyttja hello kraften i BizTalk, Microsofts branschen inledande integration lösning tooenable integrering proffs toobuild hello lösningar de behöver. Lär dig mer om hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logikappkoncept
hello följande är några av hello nyckeluppgifter som utgör hello logikappupplevelsen. 

* **Arbetsflödet** -Logic Apps ger ett grafiskt sätt toomodel dina affärsprocesser som en serie steg eller ett arbetsflöde.
* **Hanterade kopplingar** – dina logic apps behöver åtkomst till toodata och tjänster. Hanterade anslutningar har skapats specifikt tooaid dig när du ansluter tooand arbetar med dina data. Visa hello lista över kopplingar som är tillgängliga i [hanteras kopplingar][managedapis].
* **Utlösare** – Vissa hanterade anslutningsappar kan också fungera som utlösare. En utlösare startar en ny instans av ett arbetsflöde baserat på en händelse som hello ankomsten av ett e-postmeddelande eller en ändring i ditt Azure Storage-konto.
* **Åtgärder** – alla steg efter hello utlösaren i ett arbetsflöde kallas för en åtgärd. Alla åtgärder mappar vanligtvis tooan åtgärden på den hanterade koppling eller anpassade API apps.
* **Enterprise-integrationspaket** – Logic Apps innehåller funktioner från BizTalk för mer avancerade integrationsscenarier. BizTalk är Microsofts branschledande integrationsplattform. hello Enterprise-Integrationspaket kopplingar kan du tooeasily inkluderar verifiering, transformation och mer i tooyour logikappsarbetsflöden.

## <a name="getting-started"></a>Komma igång
* tooget igång med Logic Apps följer hello [skapa en Logikapp] [ create] kursen.  
* [Visa vanliga exempel och scenarier](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Du kan automatisera verksamhetsprocesser med Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Lär dig hur tooIntegrate dina system med Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md
