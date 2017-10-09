---
title: "aaaAdd referens datauppsättning tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "Lägg till referens datauppsättning tooyour tid serien insikter miljö i den här självstudiekursen"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 05e626ed81a22f2a8710b23a931ccd17c0f38ca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a>Skapa en referens för datauppsättning för tid serien insikter miljön med hjälp av hello Ibiza-portalen

En referens datauppsättning är en samling objekt som är förstärkta med hello händelser från din händelsekälla. Bearbetningsmotorn för tidsserieinsikter kopplar en händelse från din händelsekälla till ett objekt i referensdatauppsättningen. Den här förhöjda händelsen är sedan tillgängliga för frågor. Den här kopplingen baseras på hello nycklar som definierats i datauppsättningen referens.

## <a name="steps-tooadd-a-reference-data-set-tooyour-environment"></a>Steg tooadd en referens datauppsättning tooyour miljö

1. Logga in toohello [Ibiza-portalen](https://portal.azure.com).
2. Klicka på ”alla resurser” hello menyn hello vänster på hello Ibiza-portalen.
3. Välj Time Series Insights-miljö.

    ![Skapa hello tid serien insikter referens datauppsättning](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Välj ”referensdatauppsättningar”, klicka på ”+ Lägg till”.

    ![Skapa datauppsättning för hello tid serien insikter referens - information](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Ange hello namn hello referens datauppsättning.
6. Ange hello nyckelnamn och dess. Den här namn och typ är används toopick hello rätt egenskap från hello händelse i din händelsekälla. Till exempel om du tillhandahåller nyckelnamn som ”DeviceId” och typ som ”sträng” sedan hello tid serien insikter ingång letar efter en egenskap med namnet hello ”DeviceId” av typen ”sträng” i hello inkommande händelse. Du kan ange flera viktiga toojoin med hello-händelse. hello egenskapen matchar är skiftlägeskänslig.

     ![Skapa datauppsättning för hello tid serien insikter referens - information](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. Klicka på ”Skapa”.

## <a name="next-steps"></a>Nästa steg

* [Hantera referensdata](time-series-insights-manage-reference-data-csharp.md) programmässigt.
* Hello fullständiga API-referens, se [referens Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentet.
