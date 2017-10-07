---
title: "aaaAdd en händelse källa tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "I kursen får ansluter du en händelse källa tooyour tid serien insikter miljö"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 817df5e81cb4dc3d7376914a4651aabebadbcc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a>Skapa en händelsekälla för tid serien insikter miljön med hjälp av hello Ibiza-portalen

Händelsekällan Time Series Insights härleds från en asynkron meddelandekö för händelser, som Azure Event Hubs. Tid serien Insights ansluter direkt tooEvent källor, vill föra in hello dataströmmen utan användare toowrite en enda rad kod. Tidserieinsikter stöder för närvarande, Azure Event Hubs och Azure IoT Hubs. I framtida hello läggs mer händelsekällor till.

## <a name="steps-tooadd-an-event-source-tooyour-environment"></a>Steg tooadd en händelse tooyour källmiljön

1.  Logga in toohello [Ibiza-portalen](https://portal.azure.com).
2.  Klicka på ”alla resurser” hello menyn hello vänster på hello Ibiza-portalen.
3.  Välj Time Series Insights-miljö.

  ![Skapa hello tid serien insikter händelsekälla](media/add-event-source/getstarted-create-event-source-1.png)

4.  Välj ”händelsekällor” och klicka på ”+ Lägg till”.

  ![Skapa hello tid serien insikter händelsekälla - information](media/add-event-source/getstarted-create-event-source-2.png)

5.  Ange hello namn hello händelsekälla. Det här namnet är associerat med alla händelser som kommer från den här händelsekällan och är tillgänglig när en fråga körs.
6.  Välj en händelsehubb hello listan över Event Hub resurser i hello nuvarande prenumeration. Annars väljer du alternativ ”ange Event Hub-inställningar manuellt” toospecify en händelsehubb i en annan prenumeration. Händelser måste publiceras i JSON-format.
7.  Välj princip som har läsbehörighet i hello händelsehubb.
8.  Välja konsumentgrupp för Event Hub.

  > [!IMPORTANT]
  > Kontrollera att den här konsumentgruppen inte används av andra tjänster (t.ex Stream Analytics-jobb eller en annan Time Series Insights-miljö). Om konsumentgrupp används av andra tjänster, Läs åtgärden påverkas negativt för den här miljön och hello andra tjänster. Om du använder ”$Default” som hello konsumentgrupp leda toopotential återanvändning av andra läsare.

9.  Klicka på ”Skapa”.

Efter generering av hello händelsekälla startar automatiskt tid serien insikter strömmande data i din miljö.

## <a name="next-steps"></a>Nästa steg

* [Skicka händelser](time-series-insights-send-events.md) toohello händelsekälla
* Visa din miljö i [Time Series Insights-portalen](https://insights.timeseries.azure.com)
