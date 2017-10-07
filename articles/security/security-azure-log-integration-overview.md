---
title: "aaaIntegrate loggar från Azure-resurser i din SIEM-system | Microsoft Docs"
description: "Läs mer om Azure logganalys integration, de viktigaste funktionerna och hur det fungerar."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 4a59ce625702e5266a7c8eb020473cfeaf6b1964
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toomicrosoft-azure-log-integration"></a>Introduktion tooMicrosoft Azure logganalys-integrering
Läs mer om Azure logganalys integration, de viktigaste funktionerna och hur det fungerar.

## <a name="overview"></a>Översikt

Azure logganalys-integrering är en kostnadsfri lösning som gör att du toointegrate loggarna från Azure-resurser i tooyour lokala Security Information and Event Management SIEM ()-system.

Azure logganalys integration samlar in Windows-händelser från Windows Loggboken, [Azure aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Security Center-aviseringar](../security-center/security-center-intro.md), och [Azure diagnostiska loggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) från Azure-resurser. Den här integreringen hjälper din SIEM-lösning som ger en enhetlig instrumentpanel för alla tillgångar, lokalt eller i hello molnet så att du kan aggregera korrelera, analysera och varna för säkerhetshändelser.

>[!NOTE]
För tillfället är hello stöds endast moln kommersiella Azure och Azure Government. Andra moln stöds inte.

![Azure-loggintegrering][1]

## <a name="what-logs-can-i-integrate"></a>Vilka loggar kan integrera?
Azure ger utförlig loggning för varje Azure-tjänst. Dessa loggar representerar tre typer av loggar:

* **Kontroll och hantering loggar** ger inblick i hello [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) skapa, uppdatera och ta bort åtgärder. Azure aktivitetsloggar är ett exempel på den här typen av loggen.
* **Data plan loggar** ger inblick i hello händelser aktiveras som en del av hello användning av en Azure-resurs. Ett exempel på den här typen av loggen är hello Windows Loggboken- **System**, **säkerhet**, och **programmet** kanaler i en Windows-dator. Ett annat exempel är diagnostik loggning konfigureras via Azure-Monitor
* **Bearbetade händelser** ange analyserade händelse och aviseringsinformation bearbetas för din räkning. Ett exempel på den här typen av händelse är Azure Security Center-aviseringar, där Azure Security Center har bearbetats och analyserade din prenumeration tooprovide aviseringar relevanta tooyour aktuella säkerhetstillståndet.

Azure Log-Integration stöder ArcSight och QRadar Splunk. I samtliga fall Kontakta din SIEM leverantör tooassess om de har en intern anslutning. I vissa fall kan behöver du inte toouse Azure Log-integrering när interna kopplingar är tillgängliga. Mer information om stöds loggen typer finns hello vanliga frågor och svar.

>[!NOTE]
Azure Log-integrering är en kostnadsfri lösning, finns men det kostnader för Azure storage som uppstår till följd av lagring för hello loggfiler information.

Community-hjälp är tillgänglig via hello [Azure Log Integration MSDN-Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). hello forum ger hello AzLog community hello möjlighet toosupport varandra frågor, svar, tips och råd om hur tooget hello mest utanför Azure Log-integrering. Dessutom övervakar det här forumet hello Azure Log-integrering team och hjälper när vi kan.

Du kan även öppna en [supportbegäran](../azure-supportability/how-to-create-azure-support-request.md). toodo detta, Välj **loggen Integration** som hello-tjänst som du begär support.

## <a name="next-steps"></a>Nästa steg
I det här dokumentet har introducerades tooAzure loggen integrering. toolearn mer om Azure logga integrering och hello loggtyper som stöds, finns följande hello:

* [Microsoft Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center för ytterligare information, systemkrav, och installera instruktioner på Azure logganalys-integration.
* [Kom igång med Azure logganalys-integration](security-azure-log-integration-get-started.md) – den här självstudiekursen vägleder dig genom installationen av Azure logganalys-integrering och integrera loggar från Azure BOMULLSTUSS lagring, Azure aktivitetsloggar Azure Security Center-aviseringar och Azure Active Directory granskningsloggar.
* [Samarbeta konfigurationssteg](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – det här blogginlägget visar hur tooconfigure Azure logga toowork integrering med partnerlösningar Splunk, HP ArcSight och IBM QRadar. Den här bloggen representerar vår aktuella position om hur du konfigurerar hello partnerlösningar. I samtliga fall Se toopartner lösning dokumentationen först.
* [Aktiviteten och ASC aviseringar över syslog tooQRadar](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – det här blogginlägget innehåller hello steg toosend aktivitet och Azure Security Center aviseringar via syslog tooQRadar
* [Azure logganalys Integration vanliga frågor (FAQ)](security-azure-log-integration-faq.md) – det här avsnittet får du svar frågor om Azure logganalys-integration.
* [Integrera Security Center aviseringar med Azure logga Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – det här dokumentet beskrivs hur toosync Azure Security Center aviseringar med Azure Log-integrering.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
