---
title: "aaaIntroduction tooAzure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren tjänsten för övervakning och visualisering av nätverk anslutet resurser i Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 14bc2266-99e3-42a2-8d19-bd7257fec35e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: 283b3fa6add05d9bad6d5dbdae1524344d1bfc7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-monitoring-overview"></a>Azure-nätverk övervakning-översikt

Kunder kan du skapa ett nätverk i Azure genom att samordna och skapa olika enskilda nätverksresurser, till exempel virtuella nätverk, ExpressRoute, Programgateway, belastningsutjämnare och flera. Övervakning är tillgängligt på varje hello nätverksresurser. Det finns toothis övervakning som nivån Resursövervakning.

Hej tooend nätverk kan ha komplexa konfigurationer och samverkan mellan resurser, skapa komplicerade scenarier som behöver scenariobaserade övervakning via Nätverksbevakaren.

Den här artikeln beskriver scenariot och nivå Resursövervakning. Nätverksövervakning i Azure är omfattande och omfattar två kategorier:

* [**Nätverk Watcher** ](#network-watcher) -scenariot-baserad övervakning medföljer hello funktioner i Nätverksbevakaren. Den här tjänsten innehåller paketinsamling, nästa hopp, IP-flöde Kontrollera NSG flödet loggar i gruppvyn säkerhet. Scenariot nivån övervakning innehåller en end tooend för nätverksresurser i kontrast tooindividual resurs nätverksövervakning.
* [**Resursövervakning** ](#network-resource-level-monitoring) -nivå Resursövervakning består av fyra funktioner, diagnostiska loggar, mått, felsökning och resurshälsa. Alla dessa funktioner är byggda på hello nätverksnivån resurs.

## <a name="network-watcher"></a>Network Watcher

Nätverksbevakaren är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på nätverket scenariot nivå, till och från Azure. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure.

Nätverksbevakaren har för närvarande hello följande funktioner:

* **[Topologi](network-watcher-topology-overview.md)**  -tillhandahåller ett nätverk nivån som visar hello olika anslutningarna och associationer mellan nätverksresurser i en resursgrupp.
* **[Variabeln paketinsamling](network-watcher-packet-capture-overview.md)**  -samlar in paketdata till och från en virtuell dator. Avancerade filter alternativ och finjustera kontroller, till exempel som kan tooset tid och storleksbegränsningar ger flexibilitet. hello paketdata kan lagras i en blobstore eller på hello lokal disk i CAP-format.
* **[Kontrollera IP-flöde](network-watcher-ip-flow-verify-overview.md)**  -kontrollerar om ett paket tillåts eller nekas baserat på flödet information 5-tuppel paket parametrar (mål-IP, käll-IP, målport, källport och Protocol). Om hello paket nekas av en säkerhetsgrupp returneras hello regeln och grupp som nekas hello paket.
* **[Nästa hopp](network-watcher-next-hop-overview.md)**  -avgör hello next hop för paket som vidarebefordras i hello Azure nätverksinfrastruktur, aktiverar du toodiagnose alla felkonfigurerad användardefinierade vägar.
* **[Säkerhet gruppvyn](network-watcher-security-group-view-overview.md)**  -hämtar hello effektiva och tillämpade säkerhetsregler som tillämpas på en virtuell dator.
* **[NSG flöda loggning](network-watcher-nsg-flow-logging-overview.md)**  -flöde Nätverkssäkerhetsgrupper loggarna aktivera toocapture loggar relaterade tootraffic som tillåts eller nekas av hello säkerhetsregler i hello-gruppen. hello flödet definieras av en 5-tuppel information – käll-IP, mål-IP, källport, mål Port och protokoll.
* **[Virtuella Nätverksgatewayen och anslutningen felsökning](network-watcher-troubleshoot-manage-rest.md)**  -ger hello möjlighet tootroubleshoot virtuella Nätverksgatewayer och anslutningar.
* **[Nätverk prenumerationsbegränsningar](#network-subscription-limits)**  -aktiverar tooview nätverksresursanvändning mot gränser.
* **[Konfigurera diagnostik loggen](#diagnostic-logs)**  – ger en enda tooenable eller inaktivera diagnostik loggar för nätverksresurser i en resursgrupp.
* **[Anslutningen (förhandsgranskning)](network-watcher-connectivity-overview.md)**  -verifierar hello möjligheten att upprätta en direkt TCP-anslutning från en virtuell dator tooa anges slutpunkt.

### <a name="role-based-access-control-rbac-in-network-watcher"></a>Rollbaserad åtkomstkontroll (RBAC) i Nätverksbevakaren

Nätverksbevakaren använder hello [rollbaserad åtkomstkontroll (RBAC) modellen](../active-directory/role-based-access-control-what-is.md). hello följande behörigheter krävs av hello Nätverksbevakaren. Det är viktigt att hello rollen används för att initiera nätverket Watcher API: er eller använda Nätverksbevakaren från hello-portalen har hello krävs åtkomst toomake.

|Resurs| Behörighet|
|---|---| 
|Microsoft.Storage/ |Läsa|
|Microsoft.Authorization/| Läsa| 
|Microsoft.Resources/subscriptions/resourceGroups/| Läsa|
|Microsoft.Storage/storageAccounts/listServiceSas/ | Åtgärd|
|Microsoft.Storage/storageAccounts/listAccountSas/ |Åtgärd|
|Microsoft.Storage/storageAccounts/listKeys/ | Åtgärd|
|Microsoft.Compute/virtualMachines/ |Läsa|
|Microsoft.Compute/virtualMachines/ |Skriva|
|Microsoft.Compute/virtualMachineScaleSets/ |Läsa|
|Microsoft.Compute/virtualMachineScaleSets/ |Skriva|
|Microsoft.Network/networkWatchers/packetCaptures/ |Läsa|
|Microsoft.Network/networkWatchers/packetCaptures/| Skriva|
|Microsoft.Network/networkWatchers/packetCaptures/| Ta bort|
|Microsoft.Network/networkWatchers/ |Skriva |
|Microsoft.Network/networkWatchers/| Läsa |
|Microsoft.Insights/alertRules/ |*|
|Microsoft.Support/ | *|

### <a name="network-subscription-limits"></a>Nätverket prenumerationsbegränsningar

Nätverket prenumerationsbegränsningar ger dig information om hello användning av varje hello nätverksresurs i en prenumeration i en region mot hello maximalt antal tillgängliga resurser.

![prenumerationsgränsen för nätverk][nsl]

## <a name="network-resource-level-monitoring"></a>Resursen nivån nätverksövervakning

hello följande funktioner är tillgängliga för nivån Resursövervakning:

### <a name="audit-log"></a>granskningslogg

Åtgärder som utförs som en del av hello konfiguration av nätverk loggas. Dessa loggar kan visas i hello Azure-portalen eller hämtas med hjälp av Microsoft-verktyg, till exempel Power BI eller verktyg från tredje part. Granskningsloggar är tillgängliga via hello portal, PowerShell, CLI och Rest-API. Mer information om granskningsloggarna finns [granskningsåtgärder med Resource Manager](../resource-group-audit.md)

Granskningsloggar är tillgängliga för åtgärder som utförs på alla nätverksresurser.

### <a name="metrics"></a>Mått

Mått är prestandamått och prestandaräknare som samlats in under en viss tidsperiod. Mått är tillgängliga för Programgateway. Mått kan vara används tootrigger varningar baserat på tröskelvärdet. Se [Gateway Programdiagnostik](../application-gateway/application-gateway-diagnostics.md) tooview hur mått kan vara används toocreate aviseringar.

![Visa mått][metrics]

### <a name="diagnostic-logs"></a>Diagnostikloggar

Periodiska och eget initiativ händelser skapas av nätverksresurser och inloggad storage-konton, skickas tooan Event Hub eller logganalys. Dessa loggar ger insikter om hello hälsotillståndet för en resurs. Dessa loggar kan visas i verktyg som Power BI och logganalys. hur tooview diagnostikloggar, besök toolearn [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md).

Diagnostikloggar är tillgängliga för [belastningsutjämnaren](../load-balancer/load-balancer-monitor-log.md), [Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-nsg-manage-log.md), vägar och [Programgateway](../application-gateway/application-gateway-diagnostics.md).

Nätverksbevakaren ger diagnostikloggar vyn. Den här vyn innehåller alla nätverksresurser som stöder diagnostikloggning. I den här vyn kan du aktivera och inaktivera nätverksresurser lätt och snabbt.

![loggar][logs]

### <a name="troubleshooting"></a>Felsökning

hello felsökning bladet en upplevelse i hello portal finns på nätverksresurser idag toodiagnose vanliga problem som är associerade med en enskild resurs. Det här upplevelsen är tillgänglig för hello efter nätverksresurser - ExpressRoute, VPN-Gateway, Programgateway, säkerhetsloggar i nätverket, vägar, DNS, belastningsutjämnare och Traffic Manager. toolearn mer om resursen nivån felsökning finns [diagnostisera och lösa problem med Azure-felsökning](https://azure.microsoft.com/blog/azure-troubleshoot-diagonse-resolve-issues/)

![felsökningsinformation][TS]

### <a name="resource-health"></a>Resurshälsa

hello hälsotillståndet för en nätverksresurs tillhandahålls på regelbunden basis. Dessa resurser inkluderar VPN-Gateway och VPN-tunnel. Resurshälsotillståndet är tillgänglig på hello Azure-portalen. toolearn mer om resource health finns [Resource Health översikt](../resource-health/resource-health-overview.md)

## <a name="next-steps"></a>Nästa steg

När du lära dig mer om Nätverksbevakaren dig du att:

Gör en paketinsamling på den virtuella datorn genom att besöka [variabeln paketinsamling i hello Azure-portalen](network-watcher-packet-capture-manage-portal.md)

Utföra Proaktiv övervakning och diagnostik använder [aviseringen utlöses paketinsamling](network-watcher-alert-triggered-packet-capture.md).

Identifiera säkerhetsproblem med [analysera paketinsamling med Wireshark](network-watcher-deep-packet-inspection.md), med hjälp av verktyg för öppen källkod.

Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure.

<!--Image references-->
[TS]: ./media/network-watcher-monitoring-overview/troubleshooting.png
[logs]: ./media/network-watcher-monitoring-overview/logs.png
[metrics]: ./media/network-watcher-monitoring-overview/metrics.png
[nsl]: ./media/network-watcher-monitoring-overview/nsl.png











