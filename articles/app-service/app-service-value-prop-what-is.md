---
title: "aaaAzure Apptjänst för webb-, mobil- och API apps | Microsoft Docs"
description: "Lär dig hur Azure Apptjänst hjälper dig att utveckla, distribuera och hantera webb- och mobilappar."
keywords: app service, azure app service, app service cost, scale, scalable, app deployment, azure app deployment, paas, platform-as-a-service, website, web site, web, azure mobile
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: 22f414c7d79092d87406a8d3538b946881fb4580
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-app-service"></a>Vad är Azure App Service?
*Apptjänst* är en [plattform som en tjänst](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) som erbjuds med Microsoft Azure. Skapa webb- och mobilappar för alla plattformar och enheter. Integrera dina appar med SaaS-lösningar, anslut till lokala program och automatisera affärsprocesserna. Azure kör dina appar på helt hanterade virtuella datorer (VM) med de delade VM-resurser eller dedikerade VM:er som du väljer.

Apptjänst innehåller hello webb- och mobilfunktioner som vi tidigare levererat separat som Azure Websites och Azure Mobile Services. Det finns nya funktioner för att automatisera affärsprocesser och hantera moln-API:er. Som en enda integrerad tjänst gör Apptjänst att du kan sätta ihop olika komponenter – webbplatser, mobilapp-servrar, RESTful-API:er och affärsprocesser – i en enda lösning.

hello följande 4-minuters videoklipp innehåller en kort beskrivning av hur Apptjänst relaterar tooearlier Azure erbjudanden och vad är nytt i den.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Varför använda Azure Apptjänst?
Här följer några viktiga funktioner och funktioner i Apptjänst:

* **Flera språk och ramverk** – Apptjänst har förstklassigt stöd för ASP.NET, Node.js, Java, PHP och Python. Du kan också köra [Windows PowerShell och andra skript och körbara filer](../app-service-web/web-sites-create-web-jobs.md) på Apptjänstens virtuella datorer.
* **DevOps-optimering** – Konfigurera [kontinuerlig integrering och distribution](../app-service-web/app-service-continuous-deployment.md) med Visual Studio Team Services, GitHub eller BitBucket. Flytta upp uppdateringar via [test- och mellanlagringsmiljöer](../app-service-web/web-sites-staged-publishing.md). Utför [A/B-test](../app-service-web/app-service-web-test-in-production-get-start.md). Hantera dina appar i App Service med hjälp av [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller hello [plattformsoberoende kommandoradsgränssnittet (CLI)](../cli-install-nodejs.md).
* **Global skala med hög tillgänglighet** – skala [upp](../app-service-web/web-sites-scale.md) och [ned](../monitoring-and-diagnostics/insights-how-to-scale.md) manuellt och automatiskt. Värd för dina appar var som helst i Microsofts globala infrastruktur och hello Apptjänst [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) lovar hög tillgänglighet.
* **Anslutningar tooSaaS plattformar och lokala data** – Välj bland fler än 50 [kopplingar](../connectors/apis-list.md) för företagssystem (till exempel SAP, Siebel och Oracle), SaaS-tjänster (till exempel Salesforce och Office 365) och internet tjänster (till exempel Facebook och Twitter). Åtkomst till lokala data via [hybridanslutningar ](../biztalk-services/integration-hybrid-connection-overview.md) och [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md).
* **Säkerhet och efterlevnad** – App Service [uppfyller ISO, SOC och PCI](https://www.microsoft.com/TrustCenter/).
* **Programmallar** -Välj en omfattande lista med mallar i hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) där du kan använda en guiden tooinstall populära med öppen källkod som WordPress, Joomla och Drupal.
* **Visual Studio-integrationen** – dedikerade verktyg i Visual Studio effektiviserar hello arbetet med att skapa, distribuera och felsökning.

## <a name="app-types-in-app-service"></a>Apptyper i Apptjänst
Apptjänst erbjuder flera *apptyper*måste alla som är avsedda toohost en viss arbetsbelastning:

* [**Webbappar**](../app-service-web/app-service-web-overview.md) – för att hantera webbplatser och webbprogram.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) – för att hantera mobilappservrar.
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) – För att hantera RESTful-API:er.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) – För att automatisera affärsprocesser och integrera system och data mellan moln utan att skriva kod.

Hej word *app* här refererar toohello värd resurser dedikerade toorunning en arbetsbelastning. Tar ”webbapp” som exempel kan du befinner dig förmodligen bekant toothinking för en webbapp som både beräkningsresurser hello- och kod som tillsammans ger funktioner tooa standardwebbläsare. Men i App Service en *webbapp* är hello beräkningsresurser som Azure tillhandahåller för hantering av programkoden. 

Programmet kan bestå av flera Apptjänst-appar av olika typer. Om ditt program till exempel består av en frontwebb och en REST-API-serverdel kan du:

- Distribuera både (klientdelen och api) tooa enda webbprogram  
- Distribuera din frontend kod tooa webbapp och din serverdelskoden tooan API-app. 



## <a name="app-service-plans"></a>App Service-planer
[Apptjänstplaner](azure-web-sites-web-hosting-plans-in-depth-overview.md) representerar hello samling av fysiska resurser som används toohost dina appar.

App Service-planer definierar följande:

- **Region** (USA, västra, USA, östra osv.)
- **Skalningsantal** (en, två, tre instanser osv.)
- **Instansstorlek** (liten, medel, stor)
- **SKU** (Kostnadsfri, Delad, Basic, Standard, Premium)

Alla program som tilldelats tooan **programtjänstplanen** dela hello resurser som definieras av den så att du toosave kostnad när värd för flera appar.

Din **programtjänstplanen** kan skala från **lediga** och **delade** SKU: er för**grundläggande**, **Standard**, och **Premium** SKU: er som ger dig åtkomst till toomore resurser och funktioner längs hello sätt. När din App Service-Plan har angetts för**grundläggande** eller högre kan du också styra hello **storlek** och skala antalet hello virtuella datorer.

Hej **SKU** och **skala** av hello App Service plan avgör hello kostnad och inte hello många appar i den. 

Om du behöver mer skalbarhet och nätverksisolering kan du köra dina appar i en [Apptjänstmiljö](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Priser
Information om hur mycket Apptjänst kostar finns i [Priser för Apptjänst](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Testköra App Service
[Skapa en exempelwebbapp, exempelmobilapp eller exempellogikapp](https://azure.microsoft.com/try/app-service/) och testa appen i en timme – inget kreditkort behövs, inga åtaganden, inget krångel.

Eller öppna ett [kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/) och prova en av våra självstudier för att komma igång:

* [Självstudier: Skapa en webbapp](../app-service-web/app-service-web-get-started.md)
* [Självstudier: Skapa en mobilapp](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Självstudier: Skapa en API-app](../app-service-api/app-service-api-dotnet-get-started.md)
* [Självstudier: Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)

