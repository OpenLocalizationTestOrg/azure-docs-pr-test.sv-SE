---
title: "aaaUsing hello loggen Sök portal i Azure Log Analytics | Microsoft Docs"
description: "Den här artikeln innehåller en genomgång som beskriver hur toocreate logga sökningar och analysera data som lagras i logganalys-arbetsytan med hjälp av hello loggen Sök portal.  hello självstudier innehåller och köra några enkla frågor tooreturn olika typer av data och analysera resultat."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 2e6633d548bb508edc0c650d11d2c32fc6ee536c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-hello-log-search-portal"></a>Skapa loggen sökningar i Azure Log Analytics med hjälp av hello loggen Sök portal

> [!NOTE]
> Den här artikeln beskriver hello loggen Sök portal i Azure Log Analytics med hjälp av hello nya frågespråk.  Du lär dig mer om hello nytt språk och få hello proceduren tooupgrade ditt arbetsområde på [uppgradera sökningen Azure logganalys arbetsytan toonew loggen](log-analytics-log-search-upgrade.md).  
>
> Om ditt arbetsområde inte har varit uppgraderade toohello nya frågespråk, bör du läsa för[söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md) information om hello nuvarande version av hello loggen Sök-portalen.

Den här artikeln innehåller en genomgång som beskriver hur toocreate logga sökningar och analysera data som lagras i logganalys-arbetsytan med hjälp av hello loggen Sök portal.  hello självstudier innehåller och köra några enkla frågor tooreturn olika typer av data och analysera resultat.  Den fokuserar på funktioner i hello loggen Sök portal för att ändra hello frågan i stället för att ändra direkt.  Mer information om direkt redigera hello frågan finns hello [Query Language referens](https://go.microsoft.com/fwlink/?linkid=856079).

toocreate söker i hello Advanced Analytics-portalen i stället för hello loggen Sök portal finns [komma igång med hello Analytics-portalen](https://go.microsoft.com/fwlink/?linkid=856587).  Båda portaler använda hello samma fråga språk tooaccess hello samma data i hello logganalys-arbetsytan.

## <a name="prerequisites"></a>Krav
Den här kursen förutsätter att du redan har en logganalys-arbetsytan med minst en ansluten datakälla som genererar data för hello frågor tooanalyze.  

- Om du inte har en arbetsyta, du kan skapa en kostnadsfri hello sätt på [komma igång med en logganalys-arbetsytan](log-analytics-get-started.md).
- Ansluta minst ett [Windows-agenten](log-analytics-windows-agents.md) eller en [Linux-agenten](log-analytics-linux-agents.md) toohello arbetsytan.  

## <a name="open-hello-log-search-portal"></a>Öppna hello loggen Sök portal
Starta genom att öppna hello loggen Sök-portalen.  Du kan komma åt den i hello Azure-portalen eller hello OMS-portalen.

1. Öppna hello Azure-portalen.
2. Navigera tooLog Analytics och markera arbetsytan.
3. Välj antingen **loggen Sök** toostay i hello Azure-portalen eller starta hello OMS-portalen genom att välja **OMS-portalen** och sedan klicka på sökknappen för hello loggen.

![Logga sökknappen](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Skapa en enkel sökning
hello snabbaste sättet tooretrieve vissa data toowork med är en enkel fråga som returnerar alla poster i tabellen.  Om du har en Windows- eller Linux-klienter anslutna tooyour arbetsyta har du data i antingen hello händelse (Windows) eller Syslog (Linux) tabell.

Skriv en hello följande frågor i hello sökrutan och klicka hello sökning.  

```
Event
```
```
Syslog
```

Data returneras i listvyn för hello standard och du kan se hur många Totalt antal poster returnerades.

![Enkel fråga](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Endast visas hello första några egenskaper för varje post.  Klicka på **visa fler** toodisplay alla egenskaper för en viss post.

![Registrera information](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-hello-time-scope"></a>Ange hello tid scope
Alla poster som samlas in av logganalys har en **TimeGenerated** egenskap som innehåller hello datum och tid hello posten skapades.  En fråga i hello loggen Sök portal returnerar bara poster med en **TimeGenerated** omfattas hello tid som visas på vänster sida av skärmen hello hello.  

Du kan ändra hello tidsfiltret genom att välja hello dropdown eller genom att ändra hello skjutreglaget.  hello skjutreglaget visar ett stapeldiagram som visar hello relativa antal poster för varje gång segment inom hello intervallet.  Det här segmentet varierar beroende på hello intervall.

hello standard tid omfånget är **1 dag**.  Ändra värdet för**7 dagar**, och öka hello Totalt antal poster.

![Datum tid omfång](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-hello-query"></a>Filtrera resultatet av hello fråga
På hello är vänster hello-skärmen hello filterfönstret där du tooadd filtrering toohello fråga utan att ändra den direkt.  Flera egenskaper för hello poster returneras visas med deras översta tio värden med antal poster.

Om du arbetar med **händelse**, Välj hello för kryssrutan bredvid**fel** under **EVENTLEVELNAME**.   Om du arbetar med **Syslog**, Välj hello för kryssrutan bredvid**err** under **SEVERITYLEVEL**.  Detta ändrar hello fråga tooone av hello följande toolimit hello resulterar tooerror händelser.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filter](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Lägg till egenskaper toohello filterfönstret genom att välja **lägga till toofilters** hello egenskapen-menyn på en av hello poster.

![Toofilter menyn Lägg till](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Du kan ange samma filter genom att välja hello **Filter** hello egenskapen menyn för en post med hello värde du vill toofilter.  

Du har bara hello **Filter** alternativ för egenskaper med deras namn i blått.  Dessa är *sökbara* fält som indexeras för sökvillkor.  Fält i grått *fritext sökbara* fält som endast har hello **visa referenser** alternativet.  Det här alternativet returnerar poster som har detta värde i en egenskap.

![Filter-menyn](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Du kan gruppera hello resultat på en enskild egenskap genom att välja hello **Gruppera efter** alternativ i hello post-menyn.  Detta lägger till en [sammanfatta](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operatorn tooyour fråga som visar hello resultatet i ett diagram.  Du kan gruppera på mer än en egenskap, men du behöver tooedit hello frågan direkt.  Välj hello poster menyn nästa hello hello **datorn** egenskapen och välj **Gruppera efter ”dator”**.  

![Gruppera efter dator](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Arbeta med resultat
hello loggen Sök portalen innehåller en mängd funktioner för att arbeta med hello resultatet av en fråga.  Du kan sortera, filtrera och gruppera resulterar tooanalyze hello data utan att ändra hello faktiska fråga.  Resultatet av en fråga sorteras inte som standard.

tooview hello data i tabellformat som tillhandahåller ytterligare alternativ för att filtrera och sortera, klicka på **tabellen**.  

![Tabellvy](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Klicka på pilen för hello genom ett poster tooview hello information för den posten.

![Sortera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Sortera efter något fält genom att klicka på dess kolumnrubriken.

![Sortera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtrera hello resultaten på ett specifikt värde i kolumnen hello genom att klicka hello filter och tillhandahåller ett filtervillkor.

![Filtrera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Gruppera efter en kolumn genom att dra dess kolumnen huvud toohello överkant hello resultat.  Du kan gruppera på flera fält genom att dra flera kolumner toohello upp.

![Gruppera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Arbeta med prestandadata
Prestandadata för både Windows och Linux-agenter lagras i hello logganalys-arbetsytan i hello **Perf** tabell.  Uppgifter som ser ut precis som andra poster och vi kan skriva en enkel fråga som returnerar alla uppgifter på samma sätt som med händelser.

```
Perf
```

![Prestandadata](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Returnerar miljontals poster för alla prestandaobjekt och räknare men är inte användbar.  Du kan använda samma metoder som du använt ovanför toofilter hello data eller bara skriver hello följande fråga hello direkt i sökrutan för hello loggen.  Detta returnerar endast processor användning poster för både Windows och Linux-datorer.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processorbelastning](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Detta begränsar hello data tooa räknaren, men det fortfarande Placera inte den i ett formulär som är särskilt användbart.  Du kan visa hello data i ett linjediagram, men måste först toogroup den av dator- och TimeGenerated.  toogroup på flera fält måste toomodify hello frågan direkt, så att ändra hello frågan toohello följande.  Här används hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) fungerar på hello **CounterValue** egenskapen toocalculate hello genomsnittligt värde över varje timme.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data prestandadiagrammet](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Nu när hello data är lämpligt grupperade kan du visa det i ett visual diagram genom att lägga till hello [återge](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Linjediagram](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Nästa steg

- Mer information om hello Log Analytics-frågespråket i [komma igång med hello Analytics-portalen](https://go.microsoft.com/fwlink/?linkid=856079).
- Gå igenom en självstudiekurs med hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) vilket gör att du toorun hello samma frågor och åtkomst hello samma data som hello loggen Sök-portalen.
