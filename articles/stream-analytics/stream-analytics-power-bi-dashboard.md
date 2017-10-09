---
title: "aaaPower BI-instrumentpanelen på Azure Stream Analytics | Microsoft Docs"
description: "Använd en realtid strömmande Power BI dashboard toogather affärsinformation och analysera stora volymer data från ett Stream Analytics-jobb."
keywords: instrumentpanelen, realtid instrumentpanelen
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: jeffstok
ms.openlocfilehash: cb9127576230e9d327b437b674f31cc23869bfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Strömma analyser och Power BI: en analys i realtid instrumentpanel för strömmande data
Azure Stream Analytics gör tootake fördel av hello inledande verktyg för business intelligence [Microsoft Power BI](https://powerbi.com/). I den här artikeln får du lära dig hur skapa business intelligence-verktyg med hjälp av Power BI som utdata för Azure Stream Analytics-jobb. Du också lära dig hur toocreate och använda en realtid instrumentpanel.

Den här artikeln fortsätter från hello Stream Analytics [att upptäcka bedrägerier realtid](stream-analytics-real-time-fraud-detection.md) kursen. Den bygger på hello arbetsflöde som skapats i denna självstudiekurs och lägger till en Power BI utdata så att du kan visualisera bedrägliga telefonsamtal som identifieras av en Streaming Analytics-jobbet. 

Du kan titta på [en video](https://www.youtube.com/watch?v=SGUpT-a99MA) som visar det här scenariot.


## <a name="prerequisites"></a>Krav

Innan du börjar bör du kontrollera att du har hello följande:

* Ett Azure-konto.
* Ett konto för Power BI. Du kan använda ett arbetskonto eller ett skolkonto.
* En fullständig version av hello [att upptäcka bedrägerier realtid](stream-analytics-real-time-fraud-detection.md) kursen. hello självstudier innehåller en app som genererar fiktiva telefonsamtal metadata. I hello självstudien du skapar en händelsehubb och skickar hello strömning telefonsamtal data toohello händelsehubb. Du kan skriva en fråga som identifierar bedrägliga anrop (anrop från samma tal på hello hello, samma tid på olika platser). 


## <a name="add-power-bi-output"></a>Lägga till Power BI-utdata
I hello realtid bedrägeri identifiering självstudien skickas hello utdata tooAzure Blob storage. I det här avsnittet kan du lägga till utdata som skickar information tooPower BI.

1. Öppna hello Streaming Analytics-jobbet som du skapade tidigare i hello Azure-portalen. Om du har använt hello föreslagna namn hello jobbet heter `sa_frauddetection_job_demo`.

2. Välj hello **utdata** hello mitten av hello jobbet instrumentpanelen och välj sedan **+ Lägg till**.

3. För **Utdataalias**, ange `CallStream-PowerBI`. Du kan använda ett annat namn. Om du vill anteckna, eftersom du måste ha namnet hello senare. 

4. Under **Sink**väljer **Power BI**.

   ![Skapa ett utgående för Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. Klicka på **auktorisera**.

    En öppnas där du kan ange dina Azure-autentiseringsuppgifter för ett konto för arbetet eller skolan. 

    ![Ange autentiseringsuppgifter för åtkomst tooPower BI](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. Ange dina autentiseringsuppgifter. Tänk sedan när du anger dina autentiseringsuppgifter måste du också håller behörighet toohello Streaming Analytics-jobbet tooaccess Power BI-område.

7. När du kommer tillbaka toohello **nya utdata** bladet ange hello följande information:

    * **Gruppera arbetsytan**: Välj en arbetsyta i Power BI-klienten där du vill att toocreate hello dataset.
    * **DataSet-namnet**: Ange `sa-dataset`. Du kan använda ett annat namn. Om du vill anteckna den för senare.
    * **Tabellnamn**: Ange `fraudulent-calls`. Power BI-utdata från Stream Analytics-jobb kan för närvarande har endast en tabell i en dataset.

    ![PBI-arbetsytan](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Om Power BI har en datauppsättning och tabell som har hello samma namn som hello som du anger i hello Stream Analytics-jobbet, skrivs hello befintliga över.
    > Vi rekommenderar att du inte uttryckligen skapar den här datauppsättningen och tabellen i Power BI-konto. De skapas automatiskt när du startar Stream Analytics-jobbet och hello jobbet startar pumpande utdata till Power BI. Om jobbet frågan inte returnerar något resultat, skapas inte hello dataset och tabell.
    >

8. Klicka på **Skapa**.

hello datauppsättningen har skapats med hello följande inställningar:

* **defaultRetentionPolicy: BasicFIFO**: Data är FIFO, med högst 200 000 rader.
* **defaultMode: pushStreaming**: hello dataset stöder både strömmande paneler och traditionella rapport-baserade visuell information (kallas även push).

För närvarande kan skapa du inte datauppsättningar med andra flaggor.

Mer information om Power BI-datauppsättningar finns hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) referens.


## <a name="write-hello-query"></a>Skriva hello fråga

1. Stäng hello **utdata** bladet och returnera toohello jobb-bladet.

2. Klicka på hello **frågan** rutan. 

3. Ange hello följande fråga. Den här frågan är liknande toohello självkoppling frågan du skapade i hello upptäckt av bedrägerier kursen. hello skillnaden är att den här frågan skickar resultaten toohello nya utdata som du skapade (`CallStream-PowerBI`). 

    >[!NOTE]
    >Om du inte name hello indata `CallStream` i hello upptäckt av bedrägerier självstudien ersätta ditt namn för `CallStream` i hello **från** och **ansluta** satser i hello fråga.

        /* Our criteria for fraud:
        Calls made from hello same caller tootwo phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where hello caller is hello same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where hello switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. Klicka på **Spara**.


## <a name="test-hello-query"></a>Testa hello fråga
Det här avsnittet är valfritt men rekommenderas. 

1. Om hello TelcoStreaming appen inte körs startar du den genom att följa dessa steg:

    * Öppna ett kommandofönster.
    * Gå toohello mapp där hello telcogenerator.exe och ändrade telcodatagen.exe.config filer.
    * Kör följande kommando hello:

            telcodatagen.exe 1000 .2 2

2. I hello **frågan** bladet, klickar du på hello punkter nästa toohello `CallStream` indata och välj sedan **exempeldata från indata**.

3. Du vill att tre minuter kan du se data och klicka på **OK**. Vänta tills du meddelas att hello data har samlats in.

4. Klicka på **Test** och se till att du får resultat.


## <a name="run-hello-job"></a>Kör jobb för hello

1. Se till att hello TelcoStreaming appen körs.

2. Stäng hello **frågan** bladet.

3. I hello jobb-bladet, klickar du på **starta**.

    ![Starta hello Stream Analytics-jobbet](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Streaming Analytics-jobbet startar söker efter bedrägliga anrop i hello inkommande dataström. hello jobbet också hello dataset och tabell skapas i Power BI och börjar skicka data om hello bedrägliga anrop toothem.


## <a name="create-hello-dashboard-in-power-bi"></a>Skapa hello instrumentpanel i Power BI

1. Gå för[Powerbi.com](https://powerbi.com) och logga in med ditt arbets- eller skolkonto. Om hello Stream Analytics-jobbet frågan överför resultaten, ser du att dina dataset redan har skapats:

    ![Strömmande datauppsättning i Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. I arbetsytan, klickar du på  **+ &nbsp;skapa**.

    ![hello skapa knappen i Power BI-arbetsyta](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Skapa en ny instrumentpanel och ger den namnet `Fraudulent Calls`.

    ![Skapa en instrumentpanel och ge det ett namn i Power BI-arbetsyta](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Hello överkant hello-fönstret klickar du på **Lägg till panelen**väljer **anpassade STRÖMMANDE DATA**, och klicka sedan på **nästa**.

    ![Anpassad strömning dataset](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. Under **din DATSETS**, Välj din datauppsättning och sedan på **nästa**.

    ![Din strömmande datauppsättning](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. Under **visualiseringen typen**väljer **kort**, och klicka sedan på hello **fält** väljer **fraudulentcalls**.

    ![Visualiseringen information för den nya panelen](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. Klicka på **Nästa**.

8. Fyll i panelen detaljer som en rubrik och underrubrik.

    ![Rubrik och underrubrik för den nya panelen](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Klicka på **Använd**.

    Nu har du en bedrägeri räknare!

    ![Bedrägeri räknare](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. Följ hello steg igen tooadd en panel (startar med steg 4). Den här tiden kan hello följande:

    * När du får för**visualiseringen typen**väljer **linjediagram**. 
    * Lägg till en axel och välj **windowend**. 
    * Lägg till ett värde och välj **fraudulentcalls**.
    * För **tid fönstret toodisplay**väljer hello sista 10 minuter.

    ![Skapa panelen för linjediagram](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Klicka på **nästa**, lägga till en rubrik och underrubrik och på **tillämpa**.

    hello Power BI-instrumentpanel nu får du två vyer av data om falska samtal som identifierats i hello strömmande data.

    ![Färdig Power BI-instrumentpanelen visar två paneler för bedrägliga anrop](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Ta reda på med om Power BI

Den här kursen visar hur toocreate endast några typer av visualiseringar för en dataset. Med hjälp av Powerbi kan du skapa andra kunden business intelligence-verktyg för din organisation. Fler idéer finns hello följande resurser:

* Ett annat exempel på en Power BI-instrumentpanel titta i hello [komma igång med Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.
* Mer information om hur du konfigurerar Streaming Analytics-jobbet utdata tooPower BI och använder Power BI-grupper granska hello [Power BI](stream-analytics-define-outputs.md#power-bi) avsnitt i hello [Stream Analytics matar ut](stream-analytics-define-outputs.md) artikel. 
* Information om hur du använder Power BI vanligtvis finns [instrumentpaneler i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Lär dig mer om metodtips och begränsningar
Power BI kan för närvarande anropas ungefär en gång per sekund. Strömmande visuell information stöder paket på 15 KB. Strömmande visuell information misslyckas utöver det kan (men push fortsätter toowork). På grund av dessa begränsningar lämpar Power BI sig bäst naturligt toocases där Azure Stream Analytics har viktiga data belastningen minska. Vi rekommenderar att du använder en rullande fönster eller Hopping fönstret tooensure som data-push är som mest en push per sekund och som frågan hamnar inom hello genomströmning krav.

Du kan använda följande formel toocompute hello värdet toogive hello fönstret i sekunder:

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Exempel:

* Du har 1 000 enheter som skickar data med en sekunds intervall.
* Du använder hello Power BI Pro SKU som har stöd för 1 000 000 rader per timme.
* Vill du toopublish hello mängden genomsnittlig data per enhet tooPower BI.

Därför blir hello formel:

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Med den här konfigurationen kan du ändra hello ursprungliga frågan toohello följande:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a>Förnya auktorisering
Om hello lösenord har ändrats sedan jobbet skapades eller senast autentiserad, måste tooreauthenticate Power BI-konto. Om Azure Multi-Factor Authentication har konfigurerats på din Azure Active Directory (Azure AD)-klient, måste du också toorenew Power BI-auktorisering varannan vecka. Om du inte förnyar kan du se problem, till exempel brist på jobbutdata eller en `Authenticate user error` i hello åtgärdsloggar.

Om ett jobb startar efter hello token har upphört att gälla, uppstår ett fel och hello jobb misslyckas. tooresolve det här problemet stoppa hello jobb som körs och gå tooyour Power BI-utdata. tooavoid dataförlust, Välj hello **förnya auktorisering** länka och starta sedan om jobbet från hello **stoppats senast**.

När hello auktorisering har uppdaterats med Power BI, visas en grön avisering i hello auktorisering området tooreflect att hello problemet har lösts.

## <a name="get-help"></a>Få hjälp
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language-referens](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
