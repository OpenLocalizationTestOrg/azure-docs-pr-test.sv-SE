---
title: "aaaIoT dataströmmar i realtid och Azure Stream Analytics | Microsoft Docs"
description: "IoT-sensortaggar och dataströmmar med dataströmsanalys och realtidsbearbetning av data"
keywords: "iot-lösning, komma igång med iot"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 3e829055-75ed-469f-91f5-f0dc95046bdb
ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 422e6b719d0289880aa7f17fdc585e2b768c63d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-stream-analytics-tooprocess-data-from-iot-devices"></a>Komma igång med Azure Stream Analytics tooprocess data från IoT-enheter
I den här kursen får du lära dig hur toocreate stream-logik toogather databearbetning från Sakernas Internet (IoT) enheter. Vi använder en verkliga, Sakernas Internet (IoT) Använd case toodemonstrate hur toobuild din lösning snabbt och billigt kan.

## <a name="prerequisites"></a>Krav
* [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/)
* Du kan ladda ned exempel på frågefiler och datafiler från [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scenario
Contoso, vilket är ett företag i hello industriell automation utrymme har helt automatiserat sin produktionsprocess. hello maskiner i den här anläggningen har sensorer som sänder dataströmmar i realtid kan. I det här scenariot en av företagets fabrikschefer vill toohave realtidsinsikter från hello sensor data toolook för mönster och vidta lämpliga åtgärder utifrån dem. Vi använder hello Stream Analytics Query Language (saql) mot över hello sensor data toofind intressanta mönster från hello inkommande dataström.

Här genereras data från en enhet med Texas Instruments SensorTag.

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Hej datanyttolast hello finns i JSON-format och ser ut som följande hello:

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

I ett verkligt scenario har du flera hundra av dessa sensorer som genererar händelser som en dataström. Vi rekommenderar en gateway-enhet kör kod toopush dessa händelser för[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) eller [Azure IoT-hubbar](https://azure.microsoft.com/services/iot-hub/). Stream Analytics-jobbet skulle mata in dessa händelser från Event Hubs och köra analys i realtid frågor mot hello dataströmmar. Du kan sedan skicka hello resultat tooone av hello [stöds utdata](stream-analytics-define-outputs.md).

För att förenkla användningen av den här komma igång-guiden ingår en exempeldatafil med data från verkliga SensorTag-enheter. Du kan köra frågor på hello exempeldata och se resultaten. I efterföljande självstudiekurser får du lära dig hur tooconnect din jobbet tooinputs och utdata och distribuera dem toohello Azure-tjänsten.

## <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb
1. I hello [Azure-portalen](http://portal.azure.com), klicka hello plustecken och skriv sedan **STREAM ANALYTICS** hello text fönstret toohello till höger. Välj sedan **Stream Analytics-jobbet** i hello resultatlistan.
   
    ![Skapa ett nytt Stream Analytics-jobb](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Ange ett unikt namn och kontrollera hello prenumeration är hello rätt en för ditt jobb. Skapa sedan en ny resursgrupp eller välj en befintlig på din prenumeration.
3. Markera sedan en plats för jobbet. För snabb bearbetning och minskning av kostnaden i dataöverföring som du väljer hello rekommenderas samma plats som hello resursgrupp och avsedda storage-konto.
   
    ![Information om att skapa ett nytt Stream Analytics-jobb](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > Du bör endast skapa det här lagringskontot en gång per region. Den här lagringen delas mellan alla Stream Analytics-jobb som skapas i en region.
   > 
   > 
4. Kontrollera hello rutan tooplace ditt jobb på instrumentpanelen och klicka sedan på **skapa**.
   
    ![jobbskapande pågår](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. Du bör se en 'distributionen har startat... ”visas i hello upp till höger i webbläsarfönstret. Snart ändras tooa slutförts fönster som visas nedan.
   
    ![jobbskapande pågår](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Skapa en Azure Stream Analytics-fråga
När jobbet har skapats dess tid tooopen den och skapa en fråga. Du kan enkelt komma åt ditt jobb genom att klicka på panelen hello för den.

![Jobbpanel](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

I hello **jobbet topologi** klickar du på hello **FRÅGAN** rutan toogo toohello frågeredigeraren. Hej **FRÅGAN** redigeraren kan du tooenter T-SQL-fråga som utför omvandlingen hello över hello inkommande händelsedata.

![Frågeruta](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Fråga: Arkivera dina rådata
hello enklaste formen av frågan är en direktfråga som arkiverar alla indata tooits avses utdata. Hämta hello Exempeldatafilen från [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) tooa plats på datorn. 

1. Klistra in hello frågan från hello PassThrough.txt fil. 
   
    ![Testa indataströmmen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. Klicka på hello tre punkter nästa tooyour indata och välj **ladda upp exempeldata från filen** rutan.
   
    ![Testa indataströmmen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. Ett fönster som öppnas på hello som ett resultat, i den väljer hello HelloWorldASA-InputStream.json data från din hämtade plats och klicka på **OK** längst hello hello-fönstret.
   
    ![Testa indataströmmen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. Klicka på hello **testa** redskap i hello övre vänstra del av hello fönster och bearbeta testa frågan mot hello exempeldatamängd. En resultatfönstret öppnas under frågan som hello bearbetningen är klar.
   
    ![Testresultat](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-hello-data-based-on-a-condition"></a>Fråga: Hello filterdata baserat på ett villkor
Nu ska vi prova toofilter hello resultat baserat på ett villkor. Vi vill gärna tooshow resultat för de händelser som kommer från ”sensorA”. hello-frågan är i hello Filtering.txt-filen.

![Filtrera en dataström](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Observera att hello skiftlägeskänsliga frågan jämför ett strängvärde. Klicka på hello **Test** redskap igen tooexecute hello frågan. hello frågan ska returnera 389 rader från 1 860 händelser.

![Andra utdataresultat från frågetest](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-tootrigger-a-business-workflow"></a>Fråga: Varning tootrigger ett företagsarbetsflöde
Nu gör vi vår fråga mer detaljerad. För varje typ av sensor vi vill toomonitor medeltemperaturen under 30 sekunder fönster och endast visa resultatet om genomsnittstemperaturen hello är över 100 grader. Vi kommer att skriva hello följande fråga och klicka sedan på **Test** toosee hello resultat. hello-frågan är i hello ThresholdAlerting.txt-filen.

![30 sekunders-filterfråga](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Du bör nu se resultat som innehåller bara 245 rader och namnen på sensorer där medeltemperaturen hello är större än 100. Den här frågan grupper hello dataström med händelser efter **dspl**, vilket är hello sensornamnet över en **rullande fönster** på 30 sekunder. Temporala frågor måste ange hur vi vill tid tooprogress. Med hjälp av hello **TIMESTAMP BY** -sats har vi angett hello **OUTPUTTIME** kolumnen tooassociate gånger med alla temporala beräkningar. Detaljerad information finns i hello MSDN artiklar om [tidshantering](https://msdn.microsoft.com/library/azure/mt582045.aspx) och [fönsterhantering funktioner](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Fråga: Identifiera avsaknad av händelser
Hur kan vi skriva en fråga toofind bristande inkommande händelser? Hitta hello senast gör att en sensor skickade data och sedan inte skickade händelser för hello nästa minut. hello-frågan är i hello AbsenseOfEvent.txt-filen.

![Identifiera avsaknad av händelser](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Här exemplet använder vi en **LEFT OUTER** ansluta toohello samma dataström (självkoppling). För en **INRE** koppling returneras resultatet bara om en matchning hittas.  För en **LEFT OUTER** koppling om en händelse från vänster sida av hello koppling hello är omatchad en rad som har NULL för alla hello kolumner i hello höger returneras. Den här metoden är användbar toofind frånvaro av händelser. Mer information om [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) finns i MSDN-dokumentationen.

## <a name="conclusion"></a>Slutsats
hello syftet med den här kursen är toodemonstrate hur toowrite olika Stream Analytics-frågespråket frågor och se resulterar i hello webbläsare. Men det här är bara början. Du kan göra så mycket mer med Stream Analytics. Stream Analytics stöder en mängd olika in- och utdataenheter och kan även använda funktioner i Azure Machine Learning toomake den ett stabilt verktyg för analys av dataströmmar. Du kan starta tooexplore mer om Stream Analytics med hjälp av vår [Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Mer information om hur toowrite frågor, hello artikeln om [vanliga frågemönster](stream-analytics-stream-analytics-query-patterns.md).

