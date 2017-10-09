---
title: "aaaUse Azure Stream Analytics Tools för Visual Studio | Microsoft Docs"
description: "Komma igång-kursen för hello Azure Stream Analytics Tools för Visual Studio"
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: 
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: 
ms.openlocfilehash: bda8e548040509a6f29f1b713268bc38f73228fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Använda Azure Stream Analytics-verktyg för Visual Studio
## <a name="introduction"></a>Introduktion
I den här kursen lär du dig hur toouse Azure Stream Analytics-verktyg för Visual Studio toocreate, skapa, testa lokalt, hantera och felsöka Stream Analytics-jobb. 

När du har slutfört den här självstudiekursen kommer du att kunna:
* Bekanta dig med Stream Analytics-verktyg för Visual Studio.
* Konfigurera och distribuera ett Stream Analytics-jobb.
* TESTJOBBET lokalt med lokala exempeldata.
* Använd övervakning tootroubleshoot problem.
* Exportera befintliga jobb tooprojects.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande krav:
* Slutför hello steg som föregår ”skapa ett Stream Analytics-jobb” i hello [skapar en IoT-lösning med Stream Analytics-självstudier](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Använd Visual Studio 2015, Visual Studio 2013 update 4 eller Visual Studio 2012. Enterprise (Ultimate/Premium), Professional och Community-versioner stöds. Express edition stöds inte. Visual Studio-2017 stöds inte. 
* Använd hello Azure SDK för .NET version 2.7.1 eller senare. Installera den med hjälp av hello [installationsprogram för webbplattform](http://www.microsoft.com/web/downloads/platform.aspx).
* Installera hello [Stream Analytics Tools för Visual Studio](http://aka.ms/asatoolsvs).

## <a name="create-a-stream-analytics-project"></a>Skapa ett Stream Analytics-projekt
1. I Visual Studio klickar du på hello **filen** -menyn och välj **nytt projekt**. 

2. Hello mallar hello vänster Markera i **Stream Analytics** och klicka sedan på **Azure Stream Analytics-programmet**.

3. Ange hello projekt **namn**, **plats**, och **lösningsnamn** som du gör för andra projekt.

    ![Nytt projekt-fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    En **avgift** projekt har genererats i **Solution Explorer**.

    ![Avgift projektet genereras i Solution Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-hello-correct-subscription"></a>Välj rätt hello-prenumeration
1. I Visual Studio klickar du på hello **visa** -menyn och öppna **Server Explorer**.

2. Logga in med ditt Azure-konto. 

## <a name="define-hello-input-sources"></a>Definiera hello indatakällor
1.  I **Solution Explorer**, expandera hello **indata** nod och Byt namn på **Input.json** för**EntryStream.json**. Dubbelklicka på **EntryStream.json**.
2.  Hej **indata Alias** är nu **EntryStream**. Ange Hello-alias används i hello Frågeskript. 
3.  I **källtypen**väljer **dataströmmen**.
4.  I **källa**väljer **Händelsehubb**.
5.  I **Service Bus Namespace**väljer hello **TollData** alternativet.
6.  I **Händelsehubbens namn**väljer **post**.
7.  I **Event Hub principnamn**väljer **RootManageSharedAccessKey** (hello standardvärdet).
8.  I **händelse serialiseringsformat**väljer **Json**. 
9.  I **kodning**väljer **UTF8**. Inställningarna bör se ut som följande skärmbild hello:

    ![Inkommande fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. toofinish hello guiden klickar du på **spara**. Nu kan du lägga till en annan indatakälla toocreate hello avsluta dataströmmen. Högerklicka på hello **indata** och väljer **nytt objekt**.

    ![Nytt objekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. Välj hello fönstret i **Azure Stream Analytics indata**, och ändra hello **namn** för**ExitStream.json**. Klicka på **Lägg till**.

    ![Lägg till nytt objekt-fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. Dubbelklicka på **ExitStream.json** i hello projektet och följ hello samma steg som du gjorde för hello post dataströmmen. Vara säker på att tooenter **avsluta** för hello **Händelsehubbens namn** som visas i följande skärmbild hello:

    ![ExitStream fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    Du har nu definierat två inkommande dataströmmar:

    ![Ingång och utgång inkommande strömmar](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    Lägg till referensindata för hello blob-fil som innehåller bil registreringsdata.

13. Högerklicka på hello **indata** i hello projektet och följ hello samma steg som du gjorde för hello dataströmmen indata. I **indata Alias**, ange **registrering**, och i **källtypen**väljer **referensdata**.

    ![Fönster för registrering](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. I **Lagringskonto**väljer hello **tolldata** alternativet. I **behållare**väljer **tolldata**, och i **sökvägar**, ange **registration.json**. Det här namnet är skiftlägeskänsligt och måste vara gemener.
15. toofinish hello guiden klickar du på **spara**.

Nu har alla hello indata definierats.

## <a name="define-hello-output"></a>Definiera hello-utdata
1.  I **Solution Explorer**, expandera hello **indata** nod och dubbelklickar på **Output.json**.

2.  I **Utdataalias**, ange **utdata**. 
3.  I **Sink**väljer **SQL-databas**.
4.  I **databasen**väljer **TollDataDB**.
5.  I **användarnamn**, ange **tolladmin**. 
6.  I **lösenord**, ange **123toll!**.
7.  I **tabell**, ange **TollDataRefJoin**.
8.  Klicka på **Spara**.

    ![Utdatafönstret](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a>Skapa en Stream Analytics-fråga
Den här självstudiekursen försöker tooanswer flera verksamhetsrelaterade frågor som är relaterade tootoll data. Det skapar också Stream Analytics-frågor som kan användas i Stream Analytics tooprovide relevanta svar.
Innan du börjar din första Stream Analytics-jobbet ska vi titta närmare ett enkelt scenario och hello frågesyntaxen.

### <a name="introduction-toohello-stream-analytics-query-language"></a>Introduktion toohello Stream Analytics-frågespråket
Anta att du behöver toocount hello antalet fordon som anger en avgift monter. Eftersom det här exemplet är en ständig ström med händelser kan ha toodefine en tid. Ändra hello fråga toobe ”hur många fordon anger en avgift monter alla tre minuter”? Det här sättet toocount data är ofta kallas tooas hello rullande count.

Titta på hello Stream Analytics-fråga som svar på den här frågan:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Stream Analytics använder ett frågespråk som som SQL och lägger till några tillägg toospecify tidsrelaterade aspekter av hello frågan.

Mer information finns i [tidshantering](https://msdn.microsoft.com/library/azure/mt582045.aspx) och [fönsterhantering](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstruktioner som används i hello frågan från MSDN.

Nu när du har skrivit din första Stream Analytics-fråga, är det tid tootest den. Använd hello exempeldatafiler finns i mappen TollApp i hello följande sökväg:

.. \TollApp\TollApp\Data

Den här mappen innehåller hello följande filer:
*   Entry.JSON
*   Exit.JSON
*   Registration.JSON

## <a name="count-hello-number-of-vehicles-entering-a-toll-booth"></a>Antal hello fordon att ange en avgift monter
Dubbelklicka i hello-projektet **Script.asaql** tooopen hello skriptet i hello **frågeredigeraren**. Kopiera och klistra in hello skriptet i föregående avsnitt i hello hello-redigeraren. Hej frågeredigeraren stöder IntelliSense, syntax färgläggning och hello fel markör.

![Frågeredigeraren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Testa lokalt Stream Analytics-frågor

1. toocompile hello fråga toosee om det finns ett syntaxfel, högerklicka på hello projektet och välj **skapa**. 

2. toovalidate den här frågan mot exempeldata, kan du använda lokala exempeldata. Högerklicka på hello indata och välj **lägga till lokala indata**.

    ![Lägga till lokala indata](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. I hello popup-fönster, Välj hello exempeldata från din lokala sökväg. Klicka på **Spara**.

    ![Lägg till lokala inkommande fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    En fil med namnet **local_EntryStream.json** läggs till automatiskt tooyour indata mapp.

    ![Tillagda tooinputs mapp](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. I hello **frågeredigeraren**, klickar du på **kör lokalt**. Eller du kan trycka på F5 för hello.

    ![Kör lokalt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Lokal körning utdata](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    Tryck på några viktiga tooview hello utdata i hello **ASA lokala kör resultatet** fönstret i Visual Studio. 

    ![ASA lokala kör resultatfönstret](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. Klicka på **öppna resultatet mappen** toocheck hello utdatafiler både i csv- och JSON-format.

    ![Öppna mappen resultatet utdata](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-hello-input-data"></a>Exemplet hello-indata
Du kan också inkommande exempeldata från indatakällor tooa lokal fil. 
1. Högerklicka på hello inkommande konfigurationsfilen och välj **exempeldata**. 

   ![Exempeldata](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    Du kan prova endast händelsehubb eller IoT-hubb för tillfället. Andra indatakällor stöds inte.

2. Ange hello lokal sökväg används toosave hello exempeldata i hello popup-fönster. Klicka på **exempel**.

    ![Exempel på fönstret](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    Du kan se hello förlopp i hello **utdata** fönster. 

    ![Utdatafönstret](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-tooazure"></a>Skicka ett Stream Analytics query tooAzure
1. I hello **frågeredigeraren**, klickar du på **skicka tooAzure** i hello Skriptredigeraren.

    ![Skicka tooAzure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Välj **skapa en ny Azure Stream Analytics-jobbet**. Ange hello **jobbnamn**, och välj rätt hello **prenumeration**. Klicka på **skicka**.

    ![Skicka jobb](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a>Starta ett jobb
Nu när jobbet har skapats, öppnas automatiskt hello jobbet vyn. 
1. toostart hello jobbet, klickar du på hello **grön pil** knappen.

    ![Starta ett jobb](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Välj hello standardinställningen och klicka på **starta**.
 
    ![Starta jobb](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    hello jobbet **Status** ändras för**kör**, och **indata händelser** och **Utdatahändelserna** visas.

    ![Kör status i jobbsammanfattning](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-hello-results-in-visual-studio"></a>Resultat av hello i Visual Studio
1. Öppna i Visual Studio **Server Explorer** och högerklicka på hello **TollDataRefJoin** tabell.
2. Välj **visa tabelldata** toosee hello utdata för jobbet.
   
    ![Val av Visa tabelldata i Server Explorer](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-hello-job-metrics"></a>Visa hello jobbet mått
Vissa grundläggande jobbet statistik finns i **jobbet mått**. 

![Jobbet mått fönster](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-hello-job-in-server-explorer"></a>Lista hello jobbet i Server Explorer
I **Server Explorer**, klickar du på **Stream Analytics-jobb** och klicka sedan på **uppdatera**. hello jobb visas under **Stream Analytics-jobb**.

![Stream Analytics-jobb som finns i Server Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-hello-job-view"></a>Öppna hello jobbet vy
tooopen hello jobbet vyn, expandera jobbnoden och dubbelklicka på hello **jobbet** nod.

![Jobbet nod](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-tooa-project"></a>Exportera ett befintligt jobb tooa projekt
Det finns två sätt som du kan exportera ett befintligt jobb tooa projekt.

I **Server Explorer**, högerklicka på hello jobbet nod i hello **Stream Analytics-jobb** och välj **exportera tooNew Stream Analytics projekt**.

![Exportera tooNew Stream Analytics-projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

hello-projektet har genererats i **Solution Explorer**.

![Projektet har skapats i Solution Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
Du också kan använda hello jobbet vyn och klicka på **generera projekt**.

![Skapa projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a>Kända problem och begränsningar
 
- Det finns inget stöd för Power BI-utdata och Azure datum Datasjölager utdata.
- Det finns inget stöd för Redigeraren för att lägga till eller ändra JavaScript användardefinierade funktioner.

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Kom igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
