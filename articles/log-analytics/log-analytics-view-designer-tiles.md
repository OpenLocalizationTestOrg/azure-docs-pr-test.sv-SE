---
title: "aaaTile referens för Vydesigner i OMS Log Analytics | Microsoft Docs"
description: "Vydesigner i logganalys kan du toocreate anpassade vyer i hello OMS-konsolen som innehåller olika visuella data i hello OMS-databasen. Den här artikeln innehåller en referens för hello inställningar för varje hello paneler tillgängliga toouse i anpassade vyer."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 4706abb16b8a3719f5dbe8c89cd61739391ab8f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-view-designer-tile-reference"></a>Log Analytics Vydesigner panelen referens
hello Vydesigner i logganalys kan du toocreate anpassade vyer i hello OMS-konsolen som innehåller olika visuella data i hello OMS-databasen. Den här artikeln innehåller en referens för hello inställningar för varje hello paneler tillgängliga toouse i anpassade vyer.

Andra artiklar som är tillgängliga för Vydesigner är:

* [Visa Designer](log-analytics-view-designer.md) -översikt över hello Vydesigner och procedurer för att skapa och redigera anpassade vyer.
* [Referens för visualisering del](log-analytics-view-designer-parts.md) -referens för hello inställningar för varje hello panelerna tillgängliga toouse i anpassade vyer.

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och sedan frågor i alla visningslägen måste skrivas i hello [nya frågespråket](https://go.microsoft.com/fwlink/?linkid=856078).  Alla vyer som har skapats innan hello arbetsytan har uppgraderats kommer att automtically konverteras.

hello visas följande tabell hello olika typer av paneler som är tillgängliga i hello View Designer.  hello avsnitten nedan beskrivs varje typ av panelen i detalj och deras egenskaper.

| sida vid sida | Beskrivning |
|:--- |:--- |
| [Antal](#number-tile) |Tal visar antalet poster från en fråga. |
| [Två tal](#two-numbers-tile) |Två enda tal visar antalet poster från två olika frågor. |
| [Ring](#donut-tile) |Ringdiagram baserat på en fråga med ett summary-värde i hello center. |
| [Linjediagram och callout](#line-chart-amp-callout-tile) |Linjediagram baserat på en fråga och en kommentar med ett summary-värde. |
| [Linjediagram](#line-chart-tile) |Linjediagram baserat på en fråga. |
| [Två tidslinjer](#two-timelines-tile) |Stapeldiagram med två serier varje baserat på en separat fråga. |

## <a name="number-tile"></a>Antalet sida vid sida
Hej **nummer** visas ett tal visar hello antal poster från en fråga i loggen och en etikett.

![Antalet sida vid sida](media/log-analytics-view-designer/tile-number.png)

| Inställning | Beskrivning |
|:--- |:--- |
| Namn |Text toodisplay hello överst i hello panelen. |
| Beskrivning |Text toodisplay under hello panelnamn. |
| **Sida vid sida** | |
| Förklaring |Text toodisplay under hello värde. |
| Fråga |Fråga toorun.  hello antal hello antalet poster som returneras av frågan hello visas. |
| **Avancerade** |**> Verifiering av-dataflöde** |
| Enabled |Välj om dataflöde kontroll ska aktiveras för hello sida vid sida.  Detta ger ett alternativ visas om data inte är tillgänglig för hello bricka.  Detta är normalt används tooprovide ett meddelande under hello period när hello visa är installerade och data är tillgängliga. |
| Fråga |Fråga toorun toocheck om data är tillgängliga för hello vy.  Om hello frågan returnerar inga resultat, visas ett meddelande i stället för hello värdet från hello huvudsakliga fråga. |
| Meddelande |Meddelandet toodisplay om hello dataflöde verifiering frågan returnerar inga data.  Om du anger inget meddelande *utför Assessment* visas. |


## <a name="two-numbers-tile"></a>Panelen två tal
Hej **två tal** visas två tal som visar hello antal poster från två olika loggen frågor och en etikett för varje.

![Panelen två tal](media/log-analytics-view-designer/tile-two-numbers.png)

| Inställning | Beskrivning |
|:--- |:--- |
| Namn |Text toodisplay hello överst i hello panelen. |
| Beskrivning |Text toodisplay under hello panelnamn. |
| **Första sida vid sida** | |
| Förklaring |Text toodisplay under hello värde. |
| Fråga |Fråga toorun.  hello antal hello antalet poster som returneras av frågan hello visas. |
| **Andra sida vid sida** | |
| Förklaring |Text toodisplay under hello värde. |
| Fråga |Fråga toorun.  hello antal hello antalet poster som returneras av frågan hello visas. |
| **Avancerade** |**> Verifiering av-dataflöde** |
| Enabled |Välj om dataflöde kontroll ska aktiveras för hello sida vid sida.  Detta ger ett alternativ visas om data inte är tillgänglig för hello bricka.  Detta är normalt används tooprovide ett meddelande under hello period när hello visa är installerade och data är tillgängliga. |
| Fråga |Fråga toorun toocheck om data är tillgängliga för hello vy.  Om hello frågan returnerar inga resultat, visas ett meddelande i stället för hello värdet från hello huvudsakliga fråga. |
| Meddelande |Meddelandet toodisplay om hello dataflöde verifiering frågan returnerar inga data.  Om du anger inget meddelande *utför Assessment* visas. |


## <a name="donut-tile"></a>Ring sida vid sida
Hej **ringen** visas ett tal sammanfattats från en med en kolumn i en fråga i loggen.  hello ringen visar grafiskt resultatet av hello översta tre poster.

![Ring sida vid sida](media/log-analytics-view-designer/tile-donut.png)

| Inställning | Beskrivning |
|:--- |:--- |
| Namn |Text toodisplay hello överst i hello panelen. |
| Beskrivning |Text toodisplay under hello panelnamn. |
| **Ring** | |
| Fråga |Fråga toorun för hello ringen.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Detta är vanligtvis en fråga som använder hello **mått** nyckelordet toosummarize resultat. |
| **Ring** |**> Center** |
| Text |Text toodisplay under hello värde i hello ringen. |
| Åtgärd |hello åtgärden tooperform på hello värdet toosummarize tooa enda egenskapsvärde.<br><br>-Summan: Lägga till hello värden för alla poster med hello egenskapsvärde.<br>-Procent: Procentandelen hello summeras värden från poster med hello egenskapen värde jämfört med toohello summeras värdena för alla poster. |
| Resultatvärden som används i center åtgärd |Du kan också klicka på hello plustecknet tooadd ett eller flera värden.  hello resultaten av hello frågan kommer att vara begränsade toorecords med hello egenskapsvärden som du anger.  Om inga värden har lagts till med än alla poster i hello frågan. |
| **Ring** |**> Ytterligare alternativ** |
| Färger |hello färg toodisplay för varje hello tre översta egenskaper.  Om du vill toospecify alternativa färger för egenskapsvärden kan du använda avancerade mappning av färg. |
| Avancerade färgmappning |Visar en färg för egenskapsvärden.  Om hello-värdet som du anger i hello översta tre sedan hello alternativa färg visas i stället för hello standard färg.  Om hello-egenskapen inte är i hello översta tre sedan hello färg visas inte. |
| **Avancerade** |**> Verifiering av-dataflöde** |
| Enabled |Välj om dataflöde kontroll ska aktiveras för hello sida vid sida.  Detta ger ett alternativ visas om data inte är tillgänglig för hello bricka.  Detta är normalt används tooprovide ett meddelande under hello period när hello visa är installerade och data är tillgängliga. |
| Fråga |Fråga toorun toocheck om data är tillgängliga för hello vy.  Om hello frågan returnerar inga resultat, visas ett meddelande i stället för hello värdet från hello huvudsakliga fråga. |
| Meddelande |Meddelandet toodisplay om hello dataflöde verifiering frågan returnerar inga data.  Om du anger inget meddelande *utför Assessment* visas. |


## <a name="line-chart-tile"></a>Raden diagram sida vid sida
Hej **linjediagram** panelen visar ett linjediagram med flera serier från en fråga logg över tid.  

![Linjediagram och Callout sida vid sida](media/log-analytics-view-designer/tile-line-chart.png)

| Inställning | Beskrivning |
|:--- |:--- |
| Namn |Text toodisplay hello överst i hello panelen. |
| Beskrivning |Text toodisplay under hello panelnamn. |
| **Linjediagram** | |
| Fråga |Fråga toorun hello linjediagram.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Detta är vanligtvis en fråga som använder hello **mått** nyckelordet toosummarize resultat.  Om hello frågan använder hello **intervall** nyckelordet sedan hello x-axeln i diagrammet hello kommer att använda detta tidsintervall.  Om hello frågan inte innehåller hello **intervall** nyckelord och varje timme sedan intervall används för hello x-axeln. |
| **Linjediagram** |**> Y-axeln** |
| Använd en logaritmisk skala |Välj toouse en logaritmisk skala för hello y-axeln. |
| Enheter |Ange hello enheter för hello-värden som returneras av hello frågan.  Den här informationen kan använda toodisplay etiketter i hello diagram som visar hello värdetyper och eventuellt för konvertering av hello värden.  Hej **enhetstyp** anger hello hello enhet och definierar hello **aktuella enhetstypen** värden som är tillgängliga.  Om du väljer ett värde i **omvandla till** sedan hello numeriska värden konverteras från hello **aktuell enhet** skriver toohello **konvertera till** typen. |
| Anpassad etikett |Text toodisplay för hello Y-axeln nästa toohello etikett för hello enhetstyp.  Om ingen etikett anges visas endast hello enhetstyp. |
| **Avancerade** |**> Verifiering av-dataflöde** |
| Enabled |Välj om dataflöde kontroll ska aktiveras för hello sida vid sida.  Detta ger ett alternativ visas om data inte är tillgänglig för hello bricka.  Detta är normalt används tooprovide ett meddelande under hello period när hello visa är installerade och data är tillgängliga. |
| Fråga |Fråga toorun toocheck om data är tillgängliga för hello vy.  Om hello frågan returnerar inga resultat, visas ett meddelande i stället för hello värdet från hello huvudsakliga fråga. |
| Meddelande |Meddelandet toodisplay om hello dataflöde verifiering frågan returnerar inga data.  Om du anger inget meddelande *utför Assessment* visas. |


## <a name="line-chart--callout-tile"></a>Raden diagram & callout panelen
Hej **rad diagram & callout** panelen visar ett linjediagram med flera serier från en fråga logg över tid och en kommentar med ett summerat värde.  

![Linjediagram och Callout sida vid sida](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Inställning | Beskrivning |
|:--- |:--- |
| Namn |Text toodisplay hello överst i hello panelen. |
| Beskrivning |Text toodisplay under hello panelnamn. |
| **Linjediagram** | |
| Fråga |Fråga toorun hello linjediagram.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Detta är vanligtvis en fråga som använder hello **mått** nyckelordet toosummarize resultat.  Om hello frågan använder hello **intervall** nyckelordet sedan hello x-axeln i diagrammet hello kommer att använda detta tidsintervall.  Om hello frågan inte innehåller hello **intervall** nyckelord och varje timme sedan intervall används för hello x-axeln. |
| **Linjediagram** |**> Callout** |
| Kommentar |Rubrik Text toodisplay över hello callout värde. |
| Serienamn |Egenskapsvärdet för hello serien toouse för hello callout-värdet.  Om inga serier anges, används alla poster från hello fråga. |
| Åtgärd |hello åtgärden tooperform på hello värdet egenskapen toosummarize tooa enskilt värde för hello bildtexter.<br>-Genomsnittlig: Medelvärdet av hello värdet från alla poster.<br><br>-Antal: Antalet poster som returneras av hello frågan.<br>-Sista: Exempelvärde från hello senaste intervall som ingår i hello diagram.<br>-Max: Maximalt värde från hello-intervall som ingår i hello diagram.<br>-Min: Minsta värde från hello-intervall som ingår i hello diagram.<br>-Summan: Summan av hello värdet från alla poster. |
| **Linjediagram** |**> Y-axeln** |
| Använd en logaritmisk skala |Välj toouse en logaritmisk skala för hello y-axeln. |
| Enheter |Ange hello enheter för hello-värden som returneras av hello frågan.  Den här informationen kan använda toodisplay etiketter i hello diagram som visar hello värdetyper och eventuellt för konvertering av hello värden.  Hej **enhetstyp** anger hello hello enhet och definierar hello **aktuella enhetstypen** värden som är tillgängliga.  Om du väljer ett värde i **omvandla till** sedan hello numeriska värden konverteras från hello **aktuell enhet** skriver toohello **konvertera till** typen. |
| Anpassad etikett |Text toodisplay för hello Y-axeln nästa toohello etikett för hello enhetstyp.  Om ingen etikett anges visas endast hello enhetstyp. |
| **Avancerade** |**> Verifiering av-dataflöde** |
| Enabled |Välj om dataflöde kontroll ska aktiveras för hello sida vid sida.  Detta ger ett alternativ visas om data inte är tillgänglig för hello bricka.  Detta är normalt används tooprovide ett meddelande under hello period när hello visa är installerade och data är tillgängliga. |
| Fråga |Fråga toorun toocheck om data är tillgängliga för hello vy.  Om hello frågan returnerar inga resultat, visas ett meddelande i stället för hello värdet från hello huvudsakliga fråga. |
| Meddelande |Meddelandet toodisplay om hello dataflöde verifiering frågan returnerar inga data.  Om du anger inget meddelande *utför Assessment* visas. |


## <a name="two-timelines-tile"></a>Två tidslinjer sida vid sida
Hej **två tidslinjer** panelen visar hello resultatet av två loggen frågor när kolumndiagram.  En uppmaning visas för varje serie.  

![Två tidslinjer sida vid sida](media/log-analytics-view-designer/tile-two-timelines.png)

| Inställning | Beskrivning |
|:--- |:--- |
| Namn |Text toodisplay hello överst i hello panelen. |
| Beskrivning |Text toodisplay under hello panelnamn. |
| Första diagrammet | |
| Förklaring |Text toodisplay under hello callout för hello första serien. |
| Färg |Färg toouse för hello kolumner i hello första serien. |
| Diagram-fråga |Fråga toorun för hello första serien.  hello antal hello antalet poster under varje tidsintervall et representeras av hello diagram kolumner. |
| Åtgärd |hello åtgärden tooperform på hello värdet egenskapen toosummarize tooa enskilt värde för hello bildtexter.<br><br>-Genomsnittlig: Medelvärdet av hello värdet från alla poster.<br>-Antal: Antalet poster som returneras av hello frågan.<br>-Sista: Exempelvärde från hello senaste intervall som ingår i hello diagram.<br>-Max: Maximalt värde från hello-intervall som ingår i hello diagram. |
| **Det andra diagrammet** | |
| Förklaring |Text toodisplay under hello callout för andra hello-serien. |
| Färg |Färg toouse för hello kolumner i andra hello-serien. |
| Diagram-fråga |Fråga toorun för andra hello-serien.  hello antal hello antalet poster under varje tidsintervall et representeras av hello diagram kolumner. |
| Åtgärd |hello åtgärden tooperform på hello värdet egenskapen toosummarize tooa enskilt värde för hello bildtexter.<br><br>-Genomsnittlig: Medelvärdet av hello värdet från alla poster.<br>-Antal: Antalet poster som returneras av hello frågan.<br>-Sista: Exempelvärde från hello senaste intervall som ingår i hello diagram.<br>-Max: Maximalt värde från hello-intervall som ingår i hello diagram. |
| **Avancerade** |**> Verifiering av-dataflöde** |
| Enabled |Välj om dataflöde kontroll ska aktiveras för hello sida vid sida.  Detta ger ett alternativ visas om data inte är tillgänglig för hello bricka.  Detta är normalt används tooprovide ett meddelande under hello period när hello visa är installerade och data är tillgängliga. |
| Fråga |Fråga toorun toocheck om data är tillgängliga för hello vy.  Om hello frågan returnerar inga resultat, visas ett meddelande i stället för hello värdet från hello huvudsakliga fråga. |
| Meddelande |Meddelandet toodisplay om hello dataflöde verifiering frågan returnerar inga data.  Om du anger inget meddelande *utför Assessment* visas. |


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toosupport hello frågor i panelerna.
* Lägg till [visualiseringen delar](log-analytics-view-designer-parts.md) tooyour anpassad vy.
