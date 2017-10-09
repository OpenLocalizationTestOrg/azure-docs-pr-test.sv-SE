---
title: "aaaGet startas med automatisk skalning av anpassade mått i Azure | Microsoft Docs"
description: "Lär dig hur tooscale resurs efter anpassade mått i Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: d3e268ec322698d0d367361cd9c156b21e0fb6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Kom igång med Autoskala efter anpassade mått i Azure
Den här artikeln beskriver hur tooscale resurs av ett anpassat mått i Azure-portalen.

Azure övervakaren Autoskala gäller endast tooVirtual datorn skala uppsättningar (VMSS), cloud services, apptjänstplaner och apptjänstmiljöer. 

# <a name="lets-get-started"></a>Kan komma igång
Den här artikeln förutsätter att du har ett webbprogram med application insights konfigureras. Om du inte har någon redan, kan du [konfigurera Application Insights för ASP.NET-webbplats][1]

- Öppna [Azure-portalen][2]
- Klicka på ikonen för Azure i hello vänstra navigeringsfönstret.
  ![Starta Azure-övervakning][3]
- Klicka på Autoskalningsinställning tooview alla hello resurser för vilka automatisk skalning är tillämpligt, tillsammans med dess aktuella status Autoskala ![identifiera Autoskala i Azure-monitor][4]
- Öppna 'Autoskala-bladet i Azure-Monitor och välj en resurs du vill tooscale
> Obs: hello stegen nedan använder en app service-plan som är associerade med ett webbprogram som har app insights konfigureras.
- Observera att hello aktuella standardinstansantalet är 1 i hello skala inställningen bladet för hello resurs. Klicka på ”Aktivera Autoskala'.
  ![Skalinställningen för ny webbapp][5]
- Ange ett namn för hello skala inställningen och hello klickar du på ”Lägg till en regel”. Observera hello regeln skalningsalternativ som öppnas som en kontext panel i hello höger. Som standard anges hello alternativet tooscale din instans räkna med 1 om hello CPU percetage hello resurs överskrider 70%. Ändra hello mått källa hello överst för ”Application Insights”, Välj hello app insights-resurs i hello-resursen' listrutan och välj sedan hello anpassat mått baserade som du vill använda tooscale.
  ![Skala med anpassade mått][6]
- Liknande toohello ovanstående steg, lägga till en scale-regel som ska skalas i och minska hello skalningsantalet med 1 om hello anpassat mått ligger under ett tröskelvärde.
  ![Skala baserat på cpu][7]
- Ange hello instansen av gränser. Till exempel om du vill tooscale mellan 2 och 5 instanser beroende på hello anpassade mått variationer, ange 'minsta' för '2', 'maximalt' för '5' och 'default' för 2
> Obs: om det inte går att läsa hello resurs mått och hello aktuella kapaciteten understiger hello standardkapaciteten sedan tooensure hello tillgängligheten för hello resurs Autoskala kommer skala ut toohello standardvärdet. Om hello aktuella kapacitet redan är högre än standardkapaciteten, skalar inte Autoskala i.
- Klicka på ”Save”

Grattis! Du kan nu nu har skapat din skala inställningen tooauto skala ditt webbprogram baserat på ett anpassat mått.

> Obs: hello likadant är tillämpliga tooget igång med en rolltjänst för VMSS eller molnet.

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
