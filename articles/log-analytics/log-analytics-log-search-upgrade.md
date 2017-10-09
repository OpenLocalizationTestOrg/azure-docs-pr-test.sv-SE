---
title: "aaaUpgrading Azure logganalys toonew loggen Sök | Microsoft Docs"
description: "Du kan delta i hello förhandsversion hello nya Log Analytics-frågespråket är nästan här.  Den här artikeln beskriver hello fördelarna med hello nytt språk och hur tooconvert din arbetsyta."
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
ms.date: 08/08/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 7659c9d1467cab79d3a16e73b52b507ed281b002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-azure-log-analytics-workspace-toonew-log-search"></a>Uppgradera din Azure logganalys arbetsytan toonew loggen sökning

> [!NOTE]
> Uppgradera toohello nya Log Analytics-frågespråket är för närvarande valfria ger dig tid för[snabbt skala upp hello nytt språk](https://go.microsoft.com/fwlink/?linkid=856078).  

hello nya Log Analytics-frågespråket är här, och du behöver tooupgrade din arbetsyta tootake nytta av den.  Den här artikeln beskriver hello fördelarna med hello nytt språk och hur tooconvert din arbetsyta.  Om du inte väljer tooupgrade, fortsätter arbetsytan toooperate precis som har alltid, men den konverteras automatiskt vid ett senare tillfälle.  Du får mycket tid och meddelande när det datumet har angetts.

Den här artikeln innehåller information om hello nytt språk och hello uppgraderingsprocessen.

## <a name="why-hello-new-language"></a>Varför hello nytt språk?
Vi förstår att det finns smärta i alla övergångar och vi inte ändra hello frågespråk för hello roliga med den.  Det finns flera orsaker som den här ändringen ger betydande värdet tooour logganalys kunder.

- **Enkel men kraftfull.** hello nytt språk är enklare toounderstand och liknande tooSQL med konstruktioner mer som naturligt språk än hello äldre språk.
- **Fullständig rörnät språk.**  hello nytt språk har mer omfattande rörnät funktioner än hello äldre språk.  Praktiskt taget eventuella utdata kan vara via rörledningar tooanother kommandot toocreate mer komplexa frågor än var möjligt tidigare.
- **Sök tidsfält extractions.**  hello nya språket stöder mer avancerade runtime beräknade fält än hello äldre språk.  Du kan använda komplexa beräkningar för utökade fält och sedan använda hello beräknade fält för ytterligare kommandon, inklusive kopplingar och aggregeringar.
- **Avancerade kopplingar.**  hello nytt språk ger mer avancerade kopplingar än hello äldre språk, inklusive hello möjlighet toojoin tabeller på flera fält, använda inre och yttre kopplingar och delta i utökade fält.
- **Tidsvärdet funktioner.**  hello nytt språk har mer avancerade funktioner för datum/tid än hello äldre språk.
- **Smart Analytics.**  hello nytt språk har avancerade algoritmer tooevaluate mönster i datauppsättningar och jämför olika datauppsättningar.
- **Avancerade Analytics-portalen.**  hello avancerade analyser portal erbjuder analysfunktioner som är inte tillgängligt i hello logganalys-portalen inklusive multiline redigering av frågor, ytterligare grafik och avancerad diagnostik.
- **Konsekvens med andra program.**  Hej nytt språk och hello avancerade Analytics-portalen används redan för analyser i Application Insights.  Implementera för Log Analytics ger konsekvens över Azure-tjänster.
- **Bättre integration med Power BI.** Frågorna i hello nya språk kan vara exporterade tooPower BI Desktop så att du kan använda dess omfattande data transformation funktioner.
- **Mycket mer.** Se toohello [Azure Log Analytics-frågespråket](https://docs.loganalytics.io) plats för fullständig information och om hello nya språket.


## <a name="when-can-i-upgrade"></a>När kan jag uppgradera?
hello uppgraderingen lyfts över alla Azure-regioner så det kan vara tillgängliga i vissa regioner före andra.  Du vet när din arbetsyta är tillgänglig toobe uppgraderas när du ser hello lila banderoll överst hello arbetsytans uppmana tooupgrade.

![Uppgradering 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

## <a name="what-happens-when-i-upgrade"></a>Vad händer när jag uppgraderar?
När du konverterar din arbetsyta, är alla sparade sökningar, Varningsregler och vyer som du har skapat med hello Vydesigner automatiskt konverterade toohello nytt språk.  Söker i lösningar konverteras inte automatiskt, men de är i stället konverteras på hello direkt när du öppnar dem..  Detta är helt transparent tooyou.

## <a name="can-i-go-back-after-i-upgrade"></a>Kan jag gå tillbaka efter uppgraderingen?
När du uppgraderar utförs en fullständig säkerhetskopiering av din arbetsyta som innehåller en ögonblicksbild av alla sparade sökningar, varningsregeln och vyer.  Detta gör att du toorestore gamla arbetsytan om du senare ska önskar.

toorestore arbetsytans äldre gå för**inställningar** i din arbetsyta och sedan **uppgradera sammanfattning**.  Du kan sedan välja hello alternativ för**Återställ äldre arbetsyta**.  

![Återställa äldre](media/log-analytics-log-search-upgrade/restore-legacy-b.png)

## <a name="how-do-i-perform-hello-upgrade"></a>Hur utför hello uppgraderingen?
Du kan uppgradera din arbetsyta när du ser hello lila banderoll överst hello i hello-portalen.  

1.  Starta hello uppgraderingen genom att klicka på hello lila banderoll som säger **mer och uppgradera**.<br>![Uppgradera 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>
2.  Läs igenom hello ytterligare information om hello uppgraderingen på hello uppgraderingsinformation sida.<br>![Uppgradera 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>
3.  Klicka på **uppgradera nu** toostart hello uppgraderingen.<br>![Uppgradera 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Ett meddelande i hello övre högra hörnet visar hello status.<br>![Uppgradera 5](media/log-analytics-log-search-upgrade/upgrade-05.png)
4.  Klart!  Gå igenom toohello loggen Sök sidan toohave en titt på uppgraderade arbetsytan.<br>![Uppgradera 6](media/log-analytics-log-search-upgrade/upgrade-06.png)<br>

Om det uppstår ett problem som orsakar hello uppgradera toofail går du toohello [diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) och ställa din fråga eller [skapa en supportbegäran](../azure-supportability/how-to-create-azure-support-request.md) från hello Azure-portalen.

## <a name="how-do-i-learn-hello-new-language"></a>Hur kan jag ta reda hello nytt språk
Eftersom den används av flera tjänster har vi skapat ett [extern webbplats toohost hello dokumentationen](https://docs.loganalytics.io/) för hello nytt språk.  Detta inkluderar självstudier, exempel och en fullständig referens toohelp du kommer på toospeed. Du kan gå igenom självstudiekursen hello nya språk på [komma igång med frågor](https://go.microsoft.com/fwlink/?linkid=856078) och komma åt hello Språkreferens på [logganalys frågespråk](https://go.microsoft.com/fwlink/?linkid=856079).  

Om du redan är bekant med hello äldre Log Analytics-frågespråket dock och du kan använda hello språk konverterare som läggs till tooyour arbetsytan som en del av hello uppgraderingen.

Skriv i äldre frågan och sedan på **konvertera** toosee hello översatt version.  Därefter kan du antingen på hello Sök knappen toorun hello Sök eller kopiera och klistra in hello konverteras frågan toouse någon annanstans, till exempel en aviseringsregel.

![Språk konverterare](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Nästa steg
- Checka ut en [självstudiekurs om hello nya språket](https://go.microsoft.com/fwlink/?linkid=856078).
- Gå igenom en [självstudier om hur du använder hello loggen Sök portal](log-analytics-log-search-log-search-portal.md) med hello nya frågespråk.
- Få en introduktion toohello nya [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587).
