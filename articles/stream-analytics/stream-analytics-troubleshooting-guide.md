---
title: "aaaTroubleshooting guide för Azure Stream Analytics | Microsoft Docs"
description: Hur tootroubleshoot Stream Analytics-jobbet
keywords: "Felsöka programguiden"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 4add48054e06a7d8eb617a08595c1be4555c59d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Felsökningsguide för Azure Stream Analytics

Felsökning av Azure Stream Analytics kan toobe besvärligt visas vid en första titt. När du arbetar med många användare kan vi har skapat den här guiden toohelp förenklad hello processen och ta bort hello gissningar om indata, utdata, frågor och funktioner.

## <a name="troubleshoot-your-stream-analytics-job"></a>Felsöka Stream Analytics-jobbet

För bästa resultat vid felsökning Stream Analytics-jobbet Använd hello följande riktlinjer:

1.  Testa anslutningen:
    - Verifiera anslutningsbarhet tooinputs och utdata med hello **Testanslutningen** knappen för varje indata och utdata.

2.  Granska dina indata:
    - tooverify som indata för flödar till Händelsehubben använder [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) tooconnect tooAzure Händelsehubb (om Event Hub indata används).  
    - Använd hello [ **exempeldata** ](stream-analytics-sample-data-input.md) knappen för varje indata och hämta hello inkommande exempeldata.
    - Inspektera hello exempel data toounderstand hello form av hello data: hello schema och hello [datatyper](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  Testa din fråga:
    - På hello **frågan** använder hello **testa** knappen tootest hello fråga och använda hello hämtas exempeldata för[testa hello fråga](stream-analytics-test-query.md). Granska eventuella fel och försök toocorrect dem.
    - Om du använder [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), kontrollera att hello händelser har tidsstämplar som är större än hello [jobbet starttid](stream-analytics-out-of-order-and-late-events.md).

4.  Felsöka din fråga:
    - Återskapa hello frågan progressivt från enkla select-instruktion toomore komplexa mängder med hjälp av stegen. Använd toobuild in hello frågan logik steg för steg [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) satser.
    - Använd [SELECT INTO](stream-analytics-select-into.md) toodebug frågesteg.

5.  Undvika vanliga fallgropar som:
    - En [ **där** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) -sats i frågan hello filtreras bort alla händelser som förhindrar att inga utdata som genereras.
    - När du använder Windows-funktioner, vänta tills hello hela fönstret varaktighet toosee utdata från hello fråga.
    - hello tidsstämpel för händelser som föregår hello jobbets starttid och därför händelser släpps.

6.  Använd händelsen ordning:
    - Om alla hello föregående steg som fungerade bra går toohello **inställningar** och välj [ **händelse ordning**](stream-analytics-out-of-order-and-late-events.md). Kontrollera att den här principen är konfigurerad för vad klokt i ditt scenario. hello policy *inte* tillämpas när du använder hello **Test** knappen tootest hello frågan. Resultatet är en skillnaden mellan testning i webbläsaren och köra hello jobb i produktion.

7.  Felsöka med hjälp av mätvärden:
    - Om du har fått inga utdata när hello förväntad varaktighet (baserat på hello fråga), försök hello följande:
        - Titta på [ **övervakningen mätvärdena** ](stream-analytics-monitoring.md) på hello **övervakaren** fliken. Eftersom hello värdena aggregeras, fördröjs hello mått med ett par minuter.
            - Om inkommande händelser > 0, hello jobbet är kan tooread indata. Om händelser för indata inte är > 0:
                - toosee om hello datakälla har giltiga data, kontrollera den med hjälp av [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Den här kontrollen utförs om hello jobbet använder Event Hub som indata.
                - Kontrollera toosee om hello data serialiseringsformat och data kodning som förväntat.
                - Om hello jobbet använder en Händelsehubb, kontrollera toosee om hello brödtext hello-meddelande är *Null*.
            - Om Data Konverteringsfel > 0 och skapa hello följande kan vara sant:
                - hello jobbet kanske inte kan toode-serialisera hello händelser.
                - hello Händelseschema kanske inte stämmer med hello definierats eller förväntade schemat för hello händelser i hello-frågan.
                - hello datatyper på några av hello fält i hello händelsen kanske inte stämmer förväntningar.
            - Om körningsfel > 0, innebär det hello jobbet kan ta emot hello data men det uppstår fel under bearbetning av hello frågan.
                - toofind hello fel, gå toohello [granskningsloggar](../azure-resource-manager/resource-group-audit.md) och filtrera på *misslyckades* status.
            - Om InputEvents > 0 och OutputEvents = 0, betyder det att en av hello följande är sant:
                - Frågebearbetningen resulterade i noll utdata-händelser.
                - Händelser eller dess fält kan vara felaktig, vilket resulterar i noll utdata efter bearbetning.
                - hello jobbet var toopush data toohello [utdatamottagaren](stream-analytics-select-into.md) av anslutning eller autentisering skäl.
        - I alla hello som tidigare nämnts fel fall operations loggmeddelanden beskrivs ytterligare information (inklusive vad som händer), förutom i fall där hello frågan logik filtreras bort alla händelser. Om hello bearbetning av flera händelser genererar fel, loggar Stream Analytics loggar hello först tre felmeddelanden av samma typ inom 10 minuter tooOperations hello. Sedan undertrycks ytterligare identiska fel med meddelandet ”fel sker för snabbt, dessa är dolt”.

8. Felsöka med hjälp av gransknings- och diagnostikloggar:
    - Använd [granskningsloggar](../azure-resource-manager/resource-group-audit.md), och tooidentify och felsökningsloggar filterfel.
    - Använd [jobbet diagnostikloggar](stream-analytics-job-diagnostic-logs.md) tooidentify och Felsök felen.

9. Undersök hello utdata:
    - När hello jobbets status är *kör*, beroende på hello varaktighet som anges i hello fråga, du kan se hello utdata i hello sink-datakällan.
    - Om du inte kan se utdata som ska tooa specifika utdatatypen omdirigera dem tooan utdatatypen som är mindre komplex, till exempel en Azure-Blob. Med Lagringsutforskaren kan kontrollera toosee om hello utdata visas. Kontrollera om bandbreddsbegränsning gränser med hello utdata förhindrar data tas emot.

10. Använd dataflöde analys med jobbet diagram:
    - tooanalyze data flödar och identifiera problem, Använd hello [jobbet diagram med](stream-analytics-job-diagram-with-metrics.md).

11. Öppna ett supportärende:
    - Slutligen, om allt annat misslyckas, öppna ett supportärende för Microsoft med hello prenumerations-ID som innehåller ditt jobb.

## <a name="get-help"></a>Få hjälp

För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
