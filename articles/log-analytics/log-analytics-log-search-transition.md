---
title: aaaAzure Log Analytics query language fusklapp | Microsoft Docs
description: "Den här artikeln innehåller hjälp om övergång toohello nya frågespråk för Log Analytics om du redan är bekant med hello äldre språk."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 8b4ee3d0b5e1ec8a9f95a09e0ad9835615ad1342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transitioning-tooazure-log-analytics-new-query-language"></a>Övergång tooAzure logganalys nya frågespråket

> [!NOTE]
> Du kan läsa mer om hello nya Log Analytics-frågespråket och få hello proceduren tooupgrade arbetsytan vid uppgraderingen din [Azure logganalys arbetsytan toonew loggen Sök](log-analytics-log-search-upgrade.md).

Den här artikeln innehåller hjälp om övergång toohello nya frågespråk för Log Analytics om du redan är bekant med hello äldre språk.

## <a name="language-converter"></a>Språk konverterare

Om du är bekant med hello äldre Log Analytics-frågespråket enklast hello toocreate hello samma fråga på nytt språk för hello toouse hello språk konverterare som är installerad i hello loggen Sök portal när din arbetsyta konverteras.  Hello konverteraren är lika enkelt som att skriva i en äldre fråga i hello översta textrutan och klicka sedan på **konvertera**.  Du kan klicka på hello knappen toorun hello sökfråga eller kopiera och klistra in den toouse den någon annanstans.

![Språk konverterare](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a>Fusklapp

hello följande tabell innehåller en jämförelse mellan olika vanliga frågor tooequivalent kommandon mellan hello nya och gamla frågespråket i Azure logganalys.

| Beskrivning | Äldre | Ny |
|:--|:--|:--|
| Sök alla tabeller      | fel | Sök ”error” (inte skiftlägeskänsliga) |
| Välj data från tabellen | Typ = händelse |  Händelse |
|                        | Typ = händelse &#124; Välj källa, EventLog, händelse-ID | Händelsen &#124; projektet källa, EventLog, händelse-ID |
|                        | Typ = händelse &#124; de 100 främsta | Händelsen &#124; ta 100 |
| Strängjämförelse      | Typ = händelsen Computer=srv01.contoso.com   | Händelsen &#124; Om datorn == ”srv01.contoso.com” |
|                        | Typ = händelsen Computer=contains("contoso") | Händelsen &#124; Om datorn innehåller ”contoso” (inte skiftlägeskänsliga)<br>Händelsen &#124; Om datorn contains_cs ”Contoso” (skiftlägeskänslig) |
|                        | Typ = händelse datorn = RegEx (”@contoso@”)  | Händelsen &#124; Om datorn matchar regex ”. *contoso*” |
| Jämförelse av datum        | Typ av händelse TimeGenerated = > nu 1DAYS | Händelsen &#124; där TimeGenerated > ago(1d) |
|                        | Typ av händelse TimeGenerated = > 2017-05-01 TimeGenerated < 2017-05-31 | Händelsen &#124; där TimeGenerated mellan (datetime(2017-05-01)... datetime(2017-05-31)) |
| Booleskt jämförelse     | Typ = pulsslag IsGatewayInstalled = false  | Pulsslag | där IsGatewayInstalled == false |
| Sortera                   | Typ = händelse &#124; Sortera datorn asc, EventLog desc, EventLevelName asc | Händelsen \| Sortera efter dator asc, EventLog desc, EventLevelName asc |
| Distinkta               | Typ = händelse &#124; dedupliceringen datorn \| Välj dator | Händelsen &#124; Sammanfatta per dator, EventLog |
| Utöka kolumner         | Typ = Perf CounterName = ”% processortid” &#124; Utöka if(map(CounterValue,0,50,0,1),"HIGH","LOW") som användning | Perf &#124; Om CounterName == ”% processortid” \| Utöka användningen = iff (CounterValue > 50, ”hög”, ”lågt”) |
| Aggregeringen            | Typ = händelse &#124; måttet count() som antal per dator | Händelsen &#124; Sammanfatta Count = count() per dator |
|                                | Typ = Perf ObjectName = Processor CounterName = ”% processortid” &#124; måttet avg(CounterValue) datorn intervall 5 minut | Perf &#124; där ObjectName == ”-Processor” och CounterName == ”% processortid” &#124; Sammanfatta avg(CounterValue) per dator, bin (TimeGenerated 5 minuter) |
| Aggregeringen med gräns | Typ = händelse &#124; måttet count() av datorn &#124; Topp 10 | Händelsen &#124; Sammanfatta AggregatedValue = count() av datorn &#124; begränsa 10 |
| Union                  | Typ = händelse eller typ = Syslog | Union händelse Syslog |
| Slå ihop                   | Typ = NetworkMonitoring &#124; Anslut inre AgentIP (typ = pulsslag) ComputerIP | NetworkMonitoring &#124; Anslut typ = internt (Sök typen == ”pulsslag”) på $left. AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Nästa steg
- Checka ut en [självstudier om hur du skriver frågor](https://go.microsoft.com/fwlink/?linkid=856078) med hello nya frågespråk.
- Se toohello [Språkreferens för frågan](https://go.microsoft.com/fwlink/?linkid=856079) detaljer om alla kommandot operatorer och funktioner för hello nya frågespråk.  
