---
title: aaaJob skalning med Azure Stream Analytics & AzureML functions | Microsoft Docs
description: "Lär dig hur tooproperly skala Stream Analytics-jobb (partitionering, SU antal och mer) när du använder Azure Machine Learning-funktioner."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 47ce7c5e-1de1-41ca-9a26-b5ecce814743
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3fbdfaf7e8e86896c56f1d18bbde3a10bd3dca04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Skala Stream Analytics-jobbet med Azure Machine Learning-funktioner
Det är ofta ganska enkelt tooset upp ett Stream Analytics-jobb och kör exempeldata genom den. Vad gör vi när vi behöver toorun hello samma jobb med högre datavolym? Det krävs oss toounderstand hur tooconfigure hello Stream Analytics-jobbet så att det skalas. I det här dokumentet fokuseras på hello speciella aspekter av skalning Stream Analytics jobb med Machine Learning-funktioner. Mer information om hur tooscale Stream Analytics-jobb i allmänhet finns hello artikel [skalning jobb](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Vad är Azure Machine Learning-funktionen i Stream Analytics?
En Machine Learning i Stream Analytics kan användas som ett reguljärt funktionsanrop i hello Stream Analytics-frågespråket. Bakom hello scen emellertid hello funktionsanrop faktiskt Azure Machine Learning-webbtjänst begäranden. Machine Learning-webbtjänster stöd ”batchbearbetning” flera rader som kallas Mini batch, i hello samma web service API-anrop tooimprove totala genomflödet. Se följande artiklar för mer information, hello [Azure Machine Learning-funktioner i Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) och [Azure Machine Learning-webbtjänster](../machine-learning/machine-learning-consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfigurera ett Stream Analytics-jobb med Machine Learning-funktioner
När du konfigurerar en Machine Learning-funktionen för Stream Analytics-jobbet, finns det två parametrar tooconsider, hello batchstorlek hello Machine Learning-funktionsanrop och hello strömningsenheter (SUs) försetts hello Stream Analytics-jobbet. toodetermine hello lämpliga värden för dessa, först beslut måste göras mellan svarstid och genomströmning, det vill säga svarstiden för hello Stream Analytics-jobbet och dataflöde på varje SU. SUs kan alltid läggas tooa jobbet tooincrease genomströmningen i en väl partitionerade Stream Analytics-fråga, även om ytterligare SUs ökar hello kostnaden för hello jobb som körs.

Det är därför viktigt toodetermine hello *tolerans* av fördröjning i Stream Analytics-jobbet körs. Naturligtvis ökar ytterligare svarstiden från att köra Azure Machine Learning tjänstbegäranden med batchstorlek som kommer att förena hello svarstiden för hello Stream Analytics-jobbet. På hello kan däremot öka batchstorlek hello Stream Analytics-jobbet tooprocess * fler händelser med hello *samma nummer* av Machine Learning webbförfrågningar service. Ofta hello ökningen av svarstiden för Machine Learning web service är underordnad linjär toohello ökningen av batchstorlek så att det är viktigt tooconsider hello mest kostnadseffektiva batchstorlek för Machine Learning-webbtjänsten i en viss situation. hello standard batchstorlek för hello web service-begäranden är 1000 och kan ändras genom att använda hello [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") eller hello [PowerShell-klienten för Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell-klienten för Stream Analytics").

När en batchstorlek har fastställts hello mängden strömning enheter (SUs) kan bestämmas utifrån hello antalet händelser att hello-funktionen måste tooprocess per sekund. Mer information om enheter för strömning finns [Stream Analytics skala jobb](stream-analytics-scale-jobs.md).

I allmänhet finns 20 samtidiga anslutningar toohello Machine Learning-webbtjänst för varje 6 SUs, förutom att 1 SU jobb och 3 SU jobb får 20 samtidiga anslutningar också.  Om frekvensen för hello-indata är 200 000 händelser per sekund och hello batchstorlek lämnas är toohello standard av 1000 hello resulterande web service fördröjning med 1000 händelser Mini batch 200 MS. Det innebär att alla anslutningar kan begär 5 toohello Machine Learning-webbtjänst på en sekund. 20 anslutningar kan hello Stream Analytics-jobbet bearbeta 20 000 händelser i 200 MS och därför 100 000 händelser på en sekund. Så tooprocess 200 000 händelser per sekund, hello Stream Analytics-jobbet måste 40 samtidiga anslutningar som kommer ut too12 SUs. hello diagrammet nedan visar hello begäranden från webbtjänstslutpunkt för hello Stream Analytics-jobbet toohello Machine Learning – var 6 SUs har 20 samtidiga anslutningar tooMachine Learning-webbtjänst på max.

![Skala Stream Analytics med Machine Learning funktioner 2 jobbet exempel](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "skala Stream Analytics med Machine Learning funktioner 2 jobbet exemplet")

I allmänhet ***B*** för batchstorlek, ***L*** för hello web service svarstid batchstorlek B i millisekunder, hello genomflödet av ett Stream Analytics-jobb med ***N*** SUs är:

![Skala Stream Analytics med Machine Learning funktioner formel](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "skala Stream Analytics med Machine Learning funktioner formel")

En ytterligare eftertanke kanske hello 'högsta antal samtidiga anrop' på hello Machine Learning web service sida, rekommenderar vi tooset toohello högsta värdet (för närvarande 200).

Mer information om den här inställningen Granska hello [skalning artikel för Machine Learning-webbtjänster](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Exempel – Sentiment analys
hello följande exempel innehåller ett Stream Analytics-jobb med hello sentiment analys Machine Learning-funktionen, enligt beskrivningen i hello [Stream Analytics, Machine Learning integration kursen](stream-analytics-machine-learning-integration-tutorial.md).

hello frågan är en enkel fullständigt partitionerade fråga följt av hello **sentiment** fungerar enligt nedan:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Överväg att hello följande scenario; Stream Analytics-jobbet måste skapas tooperform sentiment analys av hello tweets (händelser) för med en genomströmning på 10 000 tweets per sekund. Med 1 SU Stream Analytics-jobbet kunde kan toohandle hello trafik? Med hjälp av hello batch standardstorlek 1000 hello jobbet ska vara kan tookeep upp med hello indata. Ytterligare hello lägga till Machine Learning-funktionen ska skapa mer än en sekund av latens, vilket är hello allmänna standard svarstiden för hello sentiment analys Machine Learning-webbtjänst (med en standard batchstorlek 1000). hello Stream Analytics jobbet **övergripande** eller svarstid för slutpunkt till slutpunkt kan vara några sekunder. Ta en närmare titt till Stream Analytics-jobbet *särskilt* hello Machine Learning-funktionsanrop. Med hello batchstorlek som 1000, tar en genomströmning på 10 000 händelser cirka 10 begäranden tooweb service. Det finns tillräckligt många samtidiga anslutningar tooaccommodate även med 1 SU detta inkommande trafik.

Men vad händer om hello inkommande händelsehastighet ökar med 100 x och hello Stream Analytics-jobbet måste tooprocess 1 000 000 tweets per sekund? Det finns två alternativ:

1. Öka hello batchstorlek eller
2. Partitionen hello Indataströmmen tooprocess hello händelser parallellt

Hej jobb med hello första alternativet, **svarstid** ökar.

Med andra alternativ för hello, skulle flera SUs måste toobe etablerad och därför generera flera samtidiga webbtjänstbegäranden för Machine Learning. Detta innebär hello jobbet **kostnaden** ökar.

Anta hello fördröjning hello sentiment analys Machine Learning-webbtjänsten är 200 MS för 1000-händelsen batchar eller nedan, 250ms för 5 000 händelse batchar 300ms för 10 000-händelsen batchar eller 500ms för 25 000 händelse batchar.

1. Med alternativet för hello första (**inte** flera SUs-etablering), ökas hello batchstorlek för**25 000**. Detta skulle i sin tur låta hello jobbet tooprocess 1 000 000 händelser med 20 samtidiga anslutningar toohello Machine Learning-webbtjänst (med en fördröjning på 500ms per anrop). Så hello ytterligare svarstiden för hello Stream Analytics-jobbet på grund av toohello sentiment funktionen förfrågningar mot hello Machine Learning webbtjänstbegäranden skulle ökas från **200 MS** för**500ms**. Observera dock att batchstorlek **kan** ökas oändligt som hello Machine Learning-webbtjänster kräver hello nyttolastens storlek för en begäran vara 4 MB eller mindre web service-timeout för begäranden efter 100 sekunder av åtgärden.
2. Med andra alternativ för hello hello batchstorlek lämnas på 1000, med 200 MS web service svarstid, webbtjänsten var 20 samtidiga anslutningar toohello att kan tooprocess 1000 * 20 * 5 händelser = 100 000 per sekund. Därför måste tooprocess 1 000 000 händelser per sekund, hello jobb 60 SUs. Jämfört med toohello första alternativet, Stream Analytics-jobbet skulle bli mer web service gruppbegäranden i sin tur genererar en ökad kostnad.

Nedan finns en tabell för hello genomflödet i hello Stream Analytics-jobbet för olika SUs och batch-storlek (i antal händelser per sekund).

| batchstorlek (ML svarstid) | 500 (200 MS) | 1 000 (200 MS) | 5 000 (250ms) | 10 000-tal (300ms) | 25 000 (500ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 SUs** |5,000 |10 000 |40,000 |60,000 |100,000 |
| **18 SUs** |7,500 |15,000 |60,000 |90,000 |150,000 |
| **24 SUs** |10 000 |20,000 |80,000 |120,000 |200 000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25,000 |50,000 |200 000 |300,000 |500,000 |

Nu, bör du redan har en god förståelse av hur Machine Learning-funktioner i Stream Analytics fungerar. Du förmodligen förstå också att Stream Analytics-jobb ”pull” data från datakällor och varje ”pull” returnerar en batch med händelser för hello Stream Analytics-jobbet tooprocess. Hur påverkar den här pull-modellen hello Machine Learning webbtjänstbegäranden?

Normalt sett vara inte hello batchstorlek som vi har angetts för Machine Learning funktioner exakt delbart med hello antalet händelser som returneras av varje Stream Analytics-jobbet ”pull”. Detta inträffar när hello Machine Learning-webbtjänst anropas med ”delar” batchar. Detta görs toonot innebära ytterligare jobb svarstid försämras under buffertsammanslagning händelser från pull toopull.

## <a name="new-function-related-monitoring-metrics"></a>Ny funktion-relaterade övervakning mått
Tre ytterligare funktion-relaterade mått har lagts till i hello övervakaren del av ett Stream Analytics-jobb. De är funktionen BEGÄRANDEN, FUNKTIONSHÄNDELSER och misslyckade BEGÄRANDEN att funktionen enligt hello bilden nedan.

![Skala Stream Analytics med Machine Learning funktioner](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "skala Stream Analytics med Machine Learning-funktioner")

hello definieras enligt följande:

**FUNKTIONEN BEGÄRANDEN**: hello antal begäranden för funktionen.

**FUNKTIONSHÄNDELSER**: hello antalet händelser i hello fungera begäranden.

**MISSLYCKADE BEGÄRANDEN om funktionen**: hello antal funktionsfel begäranden.

## <a name="key-takeaways"></a>Viktiga Takeaways
toosummarize hello huvudsakliga punkter, i ordning tooscale ett Stream Analytics-jobb med Machine Learning-funktioner, hello följande objekt måste beaktas:

1. hello inkommande händelsefrekvens
2. hello tolereras svarstid för hello Stream Analytics-jobbet körs (och därmed hello batchstorlek för hello Machine Learning-webbtjänst begäranden)
3. hello etablerats Stream Analytics SUs och hello antal Machine Learning webbtjänstbegäranden (hello ytterligare funktion-relaterade kostnader)

En fullständigt partitionerade Stream Analytics-fråga används som exempel. Om det behövs en mer komplex fråga hello [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) är en utmärkt resurs hello Stream Analytics-teamet för att få ytterligare hjälp.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Stream Analytics, se:

* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
