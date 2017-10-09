---
title: "Översikt över aaaWeb-appar | Microsoft Docs"
description: "Ta reda på hur Azure App Service hjälper dig att utveckla och hantera webbappar."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ce27519bddd62a7fca6ba1fb23c763d0fc378c2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="web-apps-overview"></a>Översikt över Web Apps
*App Service Web Apps* är en helt hanterad beräkningsplattform som är optimerad för hantering av webbplatser och webbappar. Detta [plattform som en tjänst](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) som erbjuds av Microsoft Azure kan du fokusera på affärslogiken medan Azure tar hand om hello infrastruktur toorun och skala appar.

hello efter 5 minuter långa videon introducerar Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

## <a name="what-is-a-web-app-in-app-service"></a>Vad är en webbapp i App Service för något?
I App Service är en *webbapp* är hello beräkningsresurser som Azure tillhandahåller för värd för webbplatsen eller programmet.  

Hej beräkningsresurserna kan finnas på delade eller dedikerade virtuella datorer (VM), beroende på hello prisnivån som du väljer. Programkoden körs på en hanterad virtuell dator som är isolerad från andra kunder.

Koden kan vara i valfritt språk eller ramverk som hanteras av [Azure App Service](../app-service/app-service-value-prop-what-is.md), till exempel ASP.NET, Node.js, Java, PHP och Python. Du kan också köra skript som använder [PowerShell och andra skriptspråk](web-sites-create-web-jobs.md#acceptablefiles) i en webbapp.

Exempel på vanliga Programscenarier som du kan använda Web Apps för finns i avsnitten [Webbapp-scenarier](https://azure.microsoft.com/documentation/scenarios/web-app/) och hello **scenarier och rekommendationer** avsnitt i [Azure App Service, Virtual Jämförelse mellan datorer, Service Fabric och Cloud Services](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Fördelar med att använda Web Apps
Här följer några funktioner i App Service som gäller tooWeb appar:

* **Flera språk och ramverk** – App Service har förstklassigt stöd för ASP.NET, Node.js, Java, PHP och Python. Du kan också köra [PowerShell och andra skript och körbara filer](web-sites-create-web-jobs.md) på virtuella datorer i App Service.
* **DevOps-optimering** – Konfigurera [kontinuerlig integrering och distribution](app-service-continuous-deployment.md) med Visual Studio Team Services, GitHub eller BitBucket. Flytta upp uppdateringar via [test- och mellanlagringsmiljöer](web-sites-staged-publishing.md). Utför [A/B-test](app-service-web-test-in-production-get-start.md). Hantera dina appar i App Service med hjälp av [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller hello [plattformsoberoende kommandoradsgränssnittet (CLI)](../cli-install-nodejs.md).
* **Global skala med hög tillgänglighet** – skala [upp](web-sites-scale.md) och [ned](../monitoring-and-diagnostics/insights-how-to-scale.md) manuellt och automatiskt. Värd för dina appar var som helst i Microsofts globala infrastruktur och hello Apptjänst [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) lovar hög tillgänglighet.
* **Anslutningar tooSaaS plattformar och lokala data** – Välj bland fler än 50 [kopplingar](../connectors/apis-list.md) för företagssystem (till exempel SAP, Siebel och Oracle), SaaS-tjänster (till exempel Salesforce och Office 365) och internet tjänster (till exempel Facebook och Twitter). Åtkomst till lokala data via [hybridanslutningar ](../biztalk-services/integration-hybrid-connection-overview.md) och [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Säkerhet och efterlevnad** – App Service [uppfyller ISO, SOC och PCI](https://www.microsoft.com/TrustCenter/).
* **Programmallar** -Välj en omfattande lista med programmallar i hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) där du kan använda en guiden tooinstall populära med öppen källkod som WordPress, Joomla och Drupal.
* **Visual Studio-integrationen** – dedikerade verktyg i Visual Studio effektiviserar hello arbetet med att skapa, distribuera och felsökning.

Dessutom kan en webbapp dra nytta av funktioner i [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (t.ex. stöd för CORS) och [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (t.ex. push-meddelanden). Mer information om apptyperna i App Service finns i [Översikt över Azure App Service](../app-service/app-service-value-prop-what-is.md).

Förutom Web Apps i App Service erbjuder Azure andra tjänster som kan användas till att hantera webbplatser och webbappar. De flesta fall är Web Apps hello bästa valet.  Arkitektur för mikrotjänster överväga [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric), och om du behöver mer kontroll över hello virtuella datorer koden körs på du [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Mer information om hur toochoose mellan dessa Azure-tjänster finns [jämförelse mellan Azure App Service, virtuella datorer, Service Fabric och Cloud Services](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Komma igång
tooget igång genom att distribuera koden tooa nya exempelwebbapp i App Service, Följ någon av hello självstudiekurser i hello följande listrutan. Du behöver ett kostnadsfritt Azure-konto.

> [!div class="op_single_selector"]
> * [Distribuera din första ASP.NET web app tooAzure på 5 minuter](app-service-web-get-started-dotnet.md)
> * [Distribuera din första PHP web app tooAzure på 5 minuter](app-service-web-get-started-php.md)
> * [Distribuera din första Node.js web app tooAzure på 5 minuter](app-service-web-get-started-nodejs.md)
> * [Distribuera din första Java web app tooAzure på 5 minuter](app-service-web-get-started-java.md)
> * [Distribuera din första Python web app tooAzure på 5 minuter](app-service-web-get-started-python.md)
> * [Distribuera din första HTML plats tooAzure på fem minuter](app-service-web-get-started-html.md)
> 
> 

> [!NOTE]
> Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto. Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.
> 
> 
