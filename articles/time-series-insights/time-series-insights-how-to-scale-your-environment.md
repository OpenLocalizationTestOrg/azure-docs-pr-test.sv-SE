---
title: "Så här skalar du Azure tid serien Insights miljön | Microsoft Docs"
description: "Den här artikeln beskriver hur du skalar Azure tid serien Insights miljön. Använda Azure portal för att lägga till eller ta bort inom en prisnivå SKU."
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: edcd9561778998c4df09cc5014f8b8ba81c0e369
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Så här skalar du tid serien insikter miljön

Den här artikeln beskriver hur du ändrar kapaciteten för din miljö tid serien insikter miljön med hjälp av Azure portal. Kapaciteten är multiplikatorn tillämpas på meddelanden om ingångs-hastighet, lagringskapacitet och kostnaden för din valda SKU. 

Du kan använda Azure-portalen för att öka eller minska inom en viss prisnivå SKU. 

Dock är ändra prisnivån SKU inte tillåtet. Till exempel kan inte en miljö med en S1 priser SKU konverteras till en S2 eller vice versa. 


## <a name="s1-sku-ingress-rates-and-capacities"></a>S1-SKU ingång priser och kapacitet

| S1 Artikelnummerkapaciteten | Ingång hastighet | Maximal lagringskapacitet
| --- | --- | --- |
| 1 | 1 GB (1 miljon händelser) | 30 GB (30 miljoner händelser) per månad |
| 10 | 10 GB (10 miljoner händelser) | 300 GB (300 miljoner händelser) per månad |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU ingång priser och kapacitet

| S2 Artikelnummerkapaciteten | Ingång hastighet | Maximal lagringskapacitet
| --- | --- | --- |
| 1 | 10 GB (10 miljoner händelser) | 300 GB (300 miljoner händelser) per månad |
| 10 | 100 GB (100 miljoner händelser) | 3 TB (3 miljarder händelser) per månad |

Kapaciteter skalas linjärt, så en S1 SKU med kapacitet 2 stöder 2 GB (2 miljoner) händelser per dag ingång hastighet och 60 GB (60 miljoner händelser) per månad.

## <a name="change-the-capacity-of-your-environment"></a>Ändra kapaciteten för din miljö
1. Leta upp och markera tid serien insikter miljön i Azure-portalen. 

2. Välj på menyn för din miljö för tid serien Insighs **konfigurera**.

   ![Configure.PNG](media/scale-your-environment/configure.png)

3. Justera det **kapacitet** skjutreglaget för att välja den kapacitet som uppfyller kraven för meddelanden om ingångs-priser och lagringskapacitet. Observera den **ingång hastighet**, **lagringskapacitet**, och **uppskattade kostnaden** uppdateras dynamiskt så att visa effekten av ändringen. 

   ![Skjutreglage](media/scale-your-environment/slider.png)

   Du kan också skriva antalet multiplikatorn kapacitet i rutan till höger om skjutreglaget. 

4. Välj **spara** att skala miljön. Förloppsindikatorn visas tills ändringen har utförts, tillfälligt. 

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Kontrollera att den nya kapaciteten är tillräckliga för att förhindra begränsning](time-series-insights-diagnose-and-solve-problems.md).
