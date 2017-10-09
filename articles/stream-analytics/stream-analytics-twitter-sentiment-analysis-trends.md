---
title: aaaReal tid Twitter sentiment analys med Azure Stream Analytics | Microsoft Docs
description: "Lär dig hur toouse Stream Analytics för analys av realtidsskyddet Twitter-sentiment. Stegvisa anvisningar från händelsen generation toodata på en levande instrumentpanel."
keywords: trendanalys i realtid twitter, sentiment analys, sociala media analys, trend analys exempel
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: jeffstok
ms.openlocfilehash: 157790caa7ea6f5570dd9c9d3bd9694d437eb4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Realtid Twitter-sentiment analys i Azure Stream Analytics

Lär dig hur toobuild en sentiment analys lösning för social media analytics genom att ta realtid Twitter händelser i Händelsehubbar i Azure. Du kan sedan skriva en Azure Stream Analytics query tooanalyze hello data och antingen store hello resultat för senare användning eller använda en instrumentpanel och [Power BI](https://powerbi.com/) tooprovide insikter i realtid.

Verktyg för sociala media analytics hjälper företag att förstå trender avsnitt. Trender avsnitt är ämnen och attityder som har en stor volym med inlägg i sociala medier. Sentiment analys, som också kallas *åsikt utvinningsmodellen*, använder sociala media analytics verktyg toodetermine attityder mot en produkt, idé och så vidare. 

Trendanalys för realtid Twitter är ett bra exempel på ett webbanalysverktyg för eftersom hello hashtaggar prenumeration kan du toolisten toospecific nyckelord (hash-taggar) och utveckla sentiment analys av hello feed.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Scenario: Sociala medier sentiment analys i realtid

Ett företag som har en webbplats för Nyheter media är intresserad av att få flera fördelar jämfört med dess konkurrenter med innehåll som är direkt tillämpliga tooits läsare. hello företaget använder analys av sociala medier på information som är relevanta tooreaders genom att göra realtid sentiment analys av Twitter-data.

tooidentify trender avsnitt i realtid på Twitter, hello företagets behov analys i realtid om hello tweet volym och sentiment för viktiga ämnen. Med andra ord är hello behöver en sentiment analys analytics-motor som är baserad på den här sociala medier feed.

## <a name="prerequisites"></a>Krav
I den här kursen använder du ett klientprogram som ansluter tooTwitter och söker efter tweets som har vissa hash-taggar (som du kan ange). I ordning toorun hello program och analysera hello tweets med hjälp av Azure Streaming Analytics, måste du ha hello följande:

* En Azure-prenumeration
* Ett Twitter-konto 
* Ett Twitter-program och hello [OAuth-åtkomsttoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) för programmet. Vi ger avancerade anvisningar för hur toocreate Twitter programmet senare.
* Hej TwitterWPFClient program som läser hello Twitter-feed. tooget detta program, hämta hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) från GitHub och sedan packa hello paketet till en mapp på datorn. Om du vill toosee hello källkoden och köra programmet hello i en felsökare, du kan hämta hello källkod från [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Skapa en händelsehubb för Streaming Analytics indata

hello exempelprogrammet genererar händelser och skickar dem tooan Azure-händelsehubb. Händelsehubbar är hello önskad metod för händelsen införandet för Stream Analytics. Mer information finns i hello [Händelsehubbar i Azure-dokumentationen](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Skapa en event hub namnområde och händelsehubb
I den här proceduren du först skapa en event hub-namnområde och sedan lägger du till ett event hub toothat namnområde. Event hub namnområden används toologically gruppera relaterade händelser-bussen instanser. 

1. Logga in toohello Azure-portalen och klicka på **ny** > **Sakernas Internet** > **Händelsehubb**. 

2. I hello **skapa namnområdet** bladet, ange ett namn för namnområdet som `<yourname>-socialtwitter-eh-ns`. Du kan använda valfritt namn för hello namnområde, men hello-namnet måste vara giltigt för en URL och det måste vara unikt i Azure. 
    
3. Välj en prenumeration och skapa eller välja en resursgrupp och sedan klicka på **skapa**. 

    ![Skapa ett namnområde för event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. Hitta hello event hub namnområde i din lista över Azure-resurser när distribution hello namnområde har slutförts. 

5. Klicka på hello Nytt namnområde och hello namnområde bladet på  **+ &nbsp;Händelsehubb**. 

    ![hello lägga till Event Hub-knappen för att skapa en ny händelsehubb ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Namnet hello ny händelsehubb `socialtwitter-eh`. Du kan använda ett annat namn. Om du vill anteckna, eftersom du måste ha namnet hello senare. Du behöver inte tooset andra alternativ för hello händelsehubb.

    ![Bladet för att skapa en ny händelsehubb](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. Klicka på **Skapa**.


### <a name="grant-access-toohello-event-hub"></a>Bevilja åtkomst toohello händelsehubb

Innan en process kan skicka data tooan händelsehubb, måste hello händelsehubb ha en princip som tillåter åtkomstbehörighet. hello åtkomstprincip producerar en anslutningssträng som innehåller auktoriseringsinformation om.

1.  I hello händelse namnområde bladet, klickar du på **Händelsehubbar** och klicka sedan på hello namnet på din nya event hub.

2.  I hello event hub-blad klickar du på **principer för delad åtkomst** och klicka sedan på  **+ &nbsp;Lägg till**.

    >[!NOTE]
    >Kontrollera att du arbetar med hello händelsehubb, inte hello event hub namnområde.

3.  Lägg till en princip med namnet `socialtwitter-access` och **anspråk**väljer **hantera**.

    ![Bladet för att skapa en ny händelse hubb princip](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  Klicka på **Skapa**.

5.  Efter hello principen har distribuerats, klickar du på hello lista över principer för delad åtkomst.

6.  Hitta hello rutan **sträng-primära ANSLUTNINGSNYCKEL** och klicka på hello Kopiera knappen Nästa toohello-anslutningssträng. 
    
    ![Kopiera hello primära sträng anslutningsnyckel från hello åtkomstprincipen](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  Klistra in hello anslutningssträngen i en textredigerare. Du behöver den här anslutningssträngen för hello nästa avsnitt, när du har gjort vissa små redigeringar tooit.

    hello anslutningssträngen ser ut så här:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    Observera att hello-anslutningssträngen innehåller flera nyckel-värdepar, avgränsade med semikolon: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, och `EntityPath`.  

    > [!NOTE]
    > Delar av hello anslutningssträngen i hello exempel för säkerhet, har tagits bort.

8.  Ta bort hello i hello textredigerare `EntityPath` par från hello anslutningssträng (Glöm inte tooremove hello semikolon som föregår den). När du är klar hello anslutningssträngen ser ut så här:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-hello-twitter-client-application"></a>Konfigurera och starta hello Twitter-klientprogrammet
hello klientprogrammet hämtar tweet händelser direkt från Twitter. I order toodo så, måste behörighet toocall hello Twitter Streaming API: er. tooconfigure att behörigheten kan du skapa ett program i Twitter, vilket genererar unika autentiseringsuppgifter (t.ex en OAuth-token). Du kan sedan konfigurera hello klienten programmet toouse dessa autentiseringsuppgifter när gör det API-anrop. 

### <a name="create-a-twitter-application"></a>Skapa ett Twitter-program
Om du inte redan har ett Twitter-program som du kan använda för den här kursen kan skapa du en. Du måste redan ha ett Twitter-konto.

> [!NOTE]
> hello exakt processen i Twitter för att skapa ett program och hämtar hello nycklar och hemligheter token kan ändras. Om dessa instruktioner inte matchar det som visas på hello Twitter plats läser du toohello Twitter utvecklardokumentation.

1. Gå toohello [sidan Twitter programhantering](https://apps.twitter.com/). 

2. Skapa ett nytt program. 

    * Ange en giltig URL för hello Webbadress. Det har inte toobe en live-webbplats. (Du kan inte ange bara `localhost`.)
    * Lämna hello återanrop fältet tomt. hello-klientprogram som du använder för den här kursen kräver inte återanrop.

    ![Skapa ett program i Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. Du kan ändra behörigheter för hello program bara tooread.

4. När programmet hello skapas gå toohello **nycklar och åtkomst-token** sidan.

5. Klicka på hello knappen toogenerate en åtkomst-token och åtkomst-token hemlighet.

Behåll den här informationen praktisk, eftersom du behöver i hello nästa procedur.

>[!NOTE]
>Ange åtkomstkonto tooyour Twitter hello nycklar och hemligheter för hello Twitter-programmet. Behandla den här informationen som känslig, hello samma som du har lösenordet Twitter. Till exempel inte att bädda in den här informationen i ett program som du ger tooothers. 


### <a name="configure-hello-client-application"></a>Konfigurera hello klientprogrammet
Vi har skapat ett klientprogram som ansluter tooTwitter data med hjälp av [Twitter's Streaming API: er](https://dev.twitter.com/streaming/overview) toocollect tweet händelser för en specifik uppsättning avsnitt. hello programmet använder hello [Sentiment140](http://help.sentiment140.com/) öppen källkod verktyg som tilldelar hello följande sentiment värdet tooeach tweet:

* 0 = negativt
* 2 = neutral
* 4 = positivt

När hello tweet händelser har tilldelats ett värde för sentiment, är de pushas toohello händelsehubb som du skapade tidigare.

Innan hello programmet körs, kräver vissa information från dig, t.ex. hello Twitter nycklar och anslutningssträngen för hello event hub. Du kan ange hello konfigurationsinformation på följande sätt:

* Kör hello programmet och sedan använda hello programmets användargränssnitt tooenter hello nycklar hemligheter och anslutningssträngen. Om du gör detta hello konfigurationsinformation används för den aktuella sessionen, men sparas inte.
* Redigera hello programmet .config-fil och ange hello värdena. Den här metoden kvarstår hello konfigurationsinformation, men det innebär också att potentiellt känslig information lagras i klartext på datorn.

hello dokument följande procedur båda metoderna. 

1. Kontrollera att du har hämtat och uppackade hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) program som anges i hello krav.

2. tooset hello värden vid körning (och endast för hello aktuell session), kör hello `TwitterWPFClient.exe` program. När du uppmanas av programmet hello ange hello följande värden:

    * hello Twitter konsumentnyckel (API-nyckel).
    * hello Twitter konsumenten hemligheten (hemliga API).
    * hello Twitter åtkomst-Token.
    * hello Twitter åtkomst-Token hemlighet.
    * hello informationen i anslutningssträngen som du sparade tidigare. Kontrollera att du använder hello anslutningssträngen som du tog bort hello `EntityPath` nyckel / värde-par från.
    * hello Twitter nyckelord som du vill toodetermine sentiment för.

   ![TwitterWpfClient programmet körs, visar dolda inställningarna](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. tooset hello värden beständigt, använda en redigerare tooopen hello TwitterWpfClient.exe.config textfil. I hello `<appSettings>` element, göra detta:

    * Ange `oauth_consumer_key` toohello Twitter konsumentnyckel (API-nyckel). 
    * Ange `oauth_consumer_secret` toohello Twitter konsumenten hemligheten (hemliga API).
    * Ange `oauth_token` toohello Twitter åtkomst-Token.
    * Ange `oauth_token_secret` toohello Twitter åtkomst-Token hemlighet.

    Senare i hello `<appSettings>` element, göra dessa ändringar:

    * Ange `EventHubName` toohello händelsehubbens namn (det vill säga toohello värdet hello entitet sökväg).
    * Ange `EventHubNameConnectionString` toohello anslutningssträngen. Kontrollera att du använder hello anslutningssträngen som du tog bort hello `EntityPath` nyckel / värde-par från.

    Hej `<appSettings>` avsnitt ser ut som följande exempel hello. (För tydlighetens skull och säkerhet, vi omsluten några rader och ta bort vissa tecken.)

    ![TwitterWpfClient programkonfigurationsfilen i en textredigerare som visar hello Twitter nycklar och hemligheter samt information om hello event hub anslutningssträngar](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Om du inte redan startade hello program, köra TwitterWpfClient.exe. 

5. Klicka på hello gröna start-knappen toocollect sociala sentiment. Du ser Tweet händelser med hello **CreatedAt**, **avsnittet**, och **SentimentScore** värden som skickas till tooyour händelsehubb.

    ![TwitterWpfClient program som körs, visar en lista över tweets](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Om du får felmeddelanden och du inte ser en dataström med tweets som visas i hello nedre delen av hello fönster, kontrollera hello nycklar och hemligheter. Kontrollera även hello anslutningssträngen (se till att den inte innehåller hello `EntityPath` nyckel och värde.)


## <a name="create-a-stream-analytics-job"></a>Skapa ett Stream Analytics-jobb

Nu när tweet händelser strömning i realtid från Twitter, kan du ställa in en Stream Analytics-jobbet tooanalyze dessa händelser i realtid.

1. I hello Azure-portalen klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.

2. Hello jobb `socialtwitter-sa-job` och ange en prenumeration, resursgrupp och plats.

    Det är en bra idé tooplace hello jobbet och hello händelsehubb i hello samma region för bästa prestanda och så att du inte betalar tootransfer data mellan regioner.

    ![Skapar ett nytt Stream Analytics-jobb](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. Klicka på **Skapa**.

    hello jobb skapas och hello portalen visar jobbinformation.


## <a name="specify-hello-job-input"></a>Ange hello jobbet indata

1. I Stream Analytics-jobbet under **jobbet topologi** hello mitten av hello jobb-bladet, klickar du på **indata**. 

2. I hello **indata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyller sedan hello bladet med dessa värden:

    * **Ett inmatat alias**: Använd hello namn `TwitterStream`. Om du använder ett annat namn, notera den eftersom du behöver den senare.
    * **Typ av datakälla**: Välj **dataströmmen**.
    * **Källan**: Välj **händelsehubb**.
    * **Importera alternativet**: Välj **Använd händelsehubb från aktuell prenumeration**. 
    * **Service bus-namnrymd**: Markera hello event hub namnområde som du skapade tidigare (`<yourname>-socialtwitter-eh-ns`).
    * **Händelsehubb**: Välj hello händelsehubb som du skapade tidigare (`socialtwitter-eh`).
    * **Namnet på händelsehubben princip**: Välj hello åtkomstprincip som du skapade tidigare (`socialtwitter-access`).

    ![Skapa nya indata för Streaming Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. Klicka på **Skapa**.


## <a name="specify-hello-job-query"></a>Ange hello jobbfråga

Stream Analytics stöder en enkel, deklarativ frågemodell som beskriver transformationer. toolearn mer om hello språk finns hello [referens för Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Den här kursen hjälper dig att skapa och testa flera frågor via Twitter-data.

toocompare hello antalet nämns mellan ämnen, som du kan använda en [rullande fönster](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello antal nämns i avsnittet var femte sekund.

1. Stäng hello **indata** bladet om du inte redan har gjort.

2. Hello jobb-bladet, klickar du på hello **frågan** rutan. Azure visar hello indata och utdata som är konfigurerade för hello jobbet och kan du skapa en fråga som kan du omvandla hello Indataströmmen som toohello utdata skickas.

3. Kontrollera att hello TwitterWpfClient program körs. 

3. I hello **frågan** bladet, klickar du på hello punkter nästa toohello `TwitterStream` indata och välj sedan **exempeldata från indata**.

    ![Menyn Alternativ toouse exempeldata för hello Streaming Analytics-jobbet post med ”exempeldata från indata” markerad](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Då öppnas ett blad som du kan ange hur mycket exempel data tooget som definierats i hur länge tooread hello inkommande dataström.

4. Ange **minuter** too3 och klicka sedan på **OK**. 
    
    ![Alternativ för provtagning hello Indataströmmen, med ”3 minuter” markerad.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure exempel 3 minuter kan du se från hello Indataströmmen och meddelar dig när hello exempeldata är klar. (Detta tar en stund.) 

    hello exempeldata lagras tillfälligt och är tillgänglig när du har hello frågefönster öppnas. Om du stänger hello frågefönstret hello exempeldata ignoreras och du har toocreate en ny uppsättning exempeldata. 

5. Ändra hello frågan i hello kod editor toohello följande:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Om inte `TwitterStream` som hello alias för hello indata ersätta ditt alias för `TwitterStream` i hello-frågan.  

    Den här frågan använder hello **TIMESTAMP BY** nyckelordet toospecify något tidsstämpelsfält i hello nyttolast toobe används i hello temporala beräkningar. Om det här fältet har inte angetts, utförs hello fönsterhantering åtgärden med hjälp av hello länge varje händelse som anlänt på hello händelsehubb. Läs mer i avsnittet hello ”ankomsttid vs programmet tid” i [referens för Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Den här frågan också har åtkomst till en tidstämpel för hello slutet av varje fönster med hjälp av hello **System.Timestamp** egenskapen.

5. Klicka på **Test**. hello frågan körs mot hello data som du tog prov.
    
6. Klicka på **Spara**. Detta sparar hello frågan som en del av hello Streaming Analytics-jobbet. (Den inte spara hello exempeldata.)


## <a name="experiment-using-different-fields-from-hello-stream"></a>Experimentera med olika fält från hello dataström 

hello visar följande tabell hello fält som ingår i hello JSON strömmande data. Känna sig fria tooexperiment i hello frågeredigeraren.

|JSON-egenskap | Definition|
|--- | ---|
|CreatedAt | hello tid att hello tweet skapades|
|Avsnitt | hello avsnitt som matchar hello angivna nyckelordet|
|SentimentScore | Hej sentiment poäng från Sentiment140|
|Skapa | hello Twitter-referensen som skickas hello tweet|
|Text | hello fullständig brödtext hello tweet|


## <a name="create-an-output-sink"></a>Skapa utdatamottagare

Du har nu definierat en händelseström, event hub inkommande tooingest händelser och en fråga tooperform en omvandling över hello dataströmmen. hello sista steget är toodefine utdatamottagare för hello jobbet.  

I den här självstudiekursen skriva hello samman tweet händelser från hello jobbet frågan tooAzure Blob storage.  Du kan också push dina resultat tooAzure SQL-databas, Azure Table storage Event Hubs eller Power BI, beroende på ditt program behöver.

## <a name="specify-hello-job-output"></a>Ange hello jobbutdata

1. I hello **jobbet topologi** klickar du på hello **utdata** rutan. 

2. I hello **utdata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyller sedan hello bladet med dessa värden:

    * **Ett utdataalias**: Använd hello namn `TwitterStream-Output`. 
    * **Sink**: Välj **Blob storage**.
    * **Importalternativ**: Välj **använda blob storage från aktuell prenumeration**.
    * **Lagringskontot**. Välj **skapa ett nytt lagringskonto.**
    * **Lagringskontot** (andra rutan). Ange `YOURNAMEsa`, där `YOURNAME` är ditt namn eller en annan unik sträng. hello namn kan använda bara gemena bokstäver och siffror, och den måste vara unikt i Azure. 
    * **Behållaren**. Ange `socialtwitter`.
    Hej lagringskontonamn och behållarnamn är används tillsammans tooprovide en URI för hello blob storage, så här: 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![”Nya utdata” bladet för Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. Klicka på **Skapa**. 

    Azure skapar hello storage-konto och skapar automatiskt en nyckel. 

5. Stäng hello **utdata** bladet. 


## <a name="start-hello-job"></a>Starta hello jobbet

Jobbet indata-, fråge- och utdata har angetts. Är du redo toostart hello Stream Analytics-jobbet.

1. Kontrollera att hello TwitterWpfClient program körs. 

2. I hello jobb-bladet, klickar du på **starta**.

    ![Starta hello Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. I hello **startjobb** bladet för **jobbutdata starttid**väljer **nu** och klicka sedan på **starta**. 

    ![”Starta jobbet” bladet för hello Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure meddelar dig när hello jobb har startat, och i hello jobb-bladet hello visas statusen **kör**.

    ![Köra jobb](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Visa utdata för sentiment analys

När jobbet har startats och bearbetar hello realtid Twitter-dataströmmen kan visa du hello utdata för sentiment analys.

Du kan använda ett verktyg som [Azure Lagringsutforskaren](https://http://storageexplorer.com/) eller [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview jobbet utdata i realtid. Härifrån kan du använda [Power BI](https://powerbi.com/) tooextend ditt program tooinclude en anpassade instrumentpanel som hello som visas i följande skärmbild hello:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-tooidentify-trending-topics"></a>Skapa en annan fråga tooidentify trender avsnitt

En annan fråga som du kan använda toounderstand Twitter-sentiment baseras på en [glidande fönstret](https://msdn.microsoft.com/library/azure/dn835051.aspx). tooidentify trender ämnen, du söka efter avsnitt som går via ett tröskelvärde för nämns i en angiven tidsperiod.

Hello enligt den här kursen kan du söka efter avsnitt som nämns fler än 20 gånger i hello sista 5 sekunder.

1. I hello jobb-bladet, klickar du på **stoppa** toostop hello jobb. 

2. I hello **jobbet topologi** klickar du på hello **frågan** rutan. 

3. Ändra hello frågan toohello följande:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. Klicka på **Spara**.

5. Kontrollera att hello TwitterWpfClient program körs. 

6. Klicka på **starta** toorestart hello jobb med hjälp av hello ny fråga.


## <a name="get-support"></a>Få support
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
