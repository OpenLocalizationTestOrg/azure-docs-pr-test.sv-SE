---
title: "aaaOverview av mått i Microsoft Azure | Microsoft Docs"
description: "Lär dig hur toocustomize övervakning diagram i Azure."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c36031eb-4df5-4cd5-9479-311d493a40d2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: 4196b2e9bda713e9a0abc349eed78685abcaf194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Översikt över mått i Microsoft Azure
Alla Azure-tjänster spåra nyckelvärden som gör att du toomonitor hello hälsa, prestanda, tillgänglighet och användning av dina tjänster. Du kan visa de här måtten i hello Azure-portalen och du kan också använda hello [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) eller [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooaccess hello fullständig uppsättning mätvärden via programmering.

För vissa tjänster kanske du måste tooturn på diagnostik i ordning toosee några mått. Du får en grundläggande uppsättning mått för övriga, till exempel virtuella datorer, men behöver tooenable hello fullständiga hög frekvens mått. Se [aktivera övervakning och diagnostik](insights-how-to-use-diagnostics.md) toolearn mer.

## <a name="using-monitoring-charts"></a>Med hjälp av övervakning diagram
Du kan skapa diagram över alla hello mått dem under en tidsperiod som du väljer.

1. I hello [Azure Portal](https://portal.azure.com/), klickar du på **Bläddra**, och sedan en resurs du vill övervaka.
2. Hej **övervakning** avsnittet innehåller hello viktigaste mätdata för varje Azure-resurs. Ett webbprogram har till exempel **begäranden och fel**, där som en virtuell dator skulle ha **prosessorprocent** och **Disk läsning och skrivning**: ![övervakning lins](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Klicka på diagram visar du hello **mått** bladet. På bladet hello är dessutom toohello diagrammet en tabell som visar mängdfunktioner av hello mått (till exempel genomsnittlig lägsta och högsta över hello tidsintervall du väljer). Nedan som är hello Varningsregler för hello resurs.
    ![Mått bladet](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. toocustomize hello rader som visas, klicka på hello **redigera** knappen i hello diagram eller hello **redigera diagram** på hello mått bladet.
5. På bladet för hello Redigera fråga kan du göra tre saker:
   
   * Ändra hello tidsintervall
   * Växla hello utseende mellan stapel och linje
   * Välj olika metics ![Redigera fråga](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Ändra hello tidsintervallet är lika enkelt som att välja ett annat område (t.ex **senaste timmen**) och klicka på **spara** längst hello hello-bladet. Du kan också välja **anpassade**, vilket gör att du toochoose tid över hello senaste 2 veckorna. Du kan till exempel se hello hela två veckor eller bara 1 timme från igår. Skriv i hello text rutan tooenter en annan timme.
    ![Anpassat tidsintervall](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Nedan hello tidsintervall väljer du kanal valfritt antal mått tooshow i hello diagram.
8. När du klickar på Spara kommer dina ändringar att sparas i just den resursen. Om du har två virtuella datorer och du ändrar ett diagram på en, kommer det inte påverkar hello andra.

## <a name="creating-side-by-side-charts"></a>Skapa sida-vid-sida-diagram
Du kan använda hello kraftfulla anpassning hello-portalen för att lägga till så många diagram som du vill.

1. I hello **...**  menyn hello överst på bladet och klickar på hello **Lägg till paneler**:  
    ![Menyn Lägg till](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Sedan kan du kan markera välja ett diagram hello **galleriet** hello höger på skärmen: ![galleri](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Om du inte ser hello mått som du vill att du kan alltid lägga till en hello förinställningen statistik, och **redigera** hello diagram tooshow hello mått som du behöver.

## <a name="monitoring-usage-quotas"></a>Övervakning av användning kvoter
De flesta mått visar trender över tid, men vissa data, till exempel användning kvoter finns i tidpunkt information med ett tröskelvärde.

Du kan också se användning kvoter på hello bladet för resurser som har kvoter:

![Användning](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Som med statistik och, du kan använda hello [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) eller [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooaccess hello fullständig uppsättning användning kvoter programmässigt.

## <a name="next-steps"></a>Nästa steg
* [Få aviseringar](insights-receive-alert-notifications.md) när en måttet överskrider ett tröskelvärde.
* [Aktivera övervakning och diagnostik](insights-how-to-use-diagnostics.md) toocollect detaljerad hög frekvens mått på din tjänst.
* [Skala instansantalet automatiskt](insights-how-to-scale.md) toomake att tjänsten är tillgänglig och svarstid.
* [Övervaka programprestanda](../application-insights/app-insights-azure-web-apps.md) om du vill toounderstand exakt hur din kod genomför i hello molnet.
* Använd [Programinsikter för JavaScript-appar och webbsidor](../application-insights/app-insights-web-track-usage.md) tooget klienten analytics om hello webbläsare som besöker en webbsida.
* [Övervaka tillgänglighet och svarstider på en webbsida](../application-insights/app-insights-monitor-web-app-availability.md) med Application Insights så att du kan läsa mer om sidan inte är tillgänglig.

