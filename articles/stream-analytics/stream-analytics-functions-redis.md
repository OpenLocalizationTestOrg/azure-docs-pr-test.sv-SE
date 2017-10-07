---
title: "aaaStream Analytics realtidsbearbetning för Azure Functions | Microsoft Docs"
description: "Lär dig hur toouse en Azure-funktion anslutna en Service Bus-kö, toopopulate ett Azure Redis-Cache från hello utdata för ett Stream Analytics-jobb."
keywords: "dataströmmen, redis-cache, service bus-kö"
services: stream-analytics
author: ryancrawcour
manager: jhubbard
documentationcenter: 
ms.assetid: d428bb33-4244-4001-b93d-c77bed816527
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: ryancraw
ms.openlocfilehash: 5ef4fe76c2cadf896a80eeaf421f010c315918af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostore-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Hur toostore data från Azure Stream Analytics i en Azure Redis-Cache med hjälp av Azure-funktioner
Azure Stream Analytics kan du snabbt utveckla och distribuera lösningar för prisvärda toogain realtidsinsikter från enheter, sensorer, infrastruktur, och program eller en dataström. Den gör olika användningsfall, till exempel realtid hantering och övervakning kommandot och, att upptäcka bedrägerier, anslutna bilar och mycket mer. Du kanske vill toostore data i en distribuerad datalager, till exempel en Azure Redis-cache för utdata av Azure Stream Analytics i många sådana scenarier.

Anta att du är en del av en telekommunikation företag. Du försöker toodetect SIM bedrägeri där flera anrop från hello samma identitet, hello på samma gång, men i olika geografiskt platser. Du har gett med att lagra alla hello potentiella bedrägliga telefonsamtal i en Azure Redis-cache. I den här bloggen ger vi riktlinjer för hur du lätt kan utföra uppgiften. 

## <a name="prerequisites"></a>Krav
Fullständig hello [realtid att upptäcka bedrägerier] [ fraud-detection] hanteringspaketen för ASA

## <a name="architecture-overview"></a>Arkitektur, översikt
![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/architecture-overview.png)

I hello föregående bild visas kan Stream Analytics strömning indata toobe efterfrågas och skickas tooan utdata. Baserat på hello utdata kan Azure Functions sedan utlösa någon typ av händelse. 

I den här bloggen vi fokusera på hello Azure Functions en del av denna pipeline eller mer specifikt hello utlösa en händelse som lagrar falska data till hello-cachen.
När du har slutfört hello [realtid att upptäcka bedrägerier] [ fraud-detection] kursen har du indata (en händelsehubb), en fråga och utdata (blob storage) redan konfigurerad och körs. I den här bloggen ändra vi hello utdata toouse en Service Bus-kö i stället. Efter att ansluta vi en Azure-funktion toothis kö. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Skapa och koppla en Service Bus-kö-utdata
toocreate en Service Bus-kö följa steg 1 och 2 i hello .NET-avsnittet i [komma igång med Service Bus-köer][servicebus-getstarted].
Nu ska vi ansluta hello kön toohello Stream Analytics-jobbet som skapades i hello tidigare hanteringspaketen för upptäckt av bedrägerier.

1. I hello Azure-portalen, går toohello **utdata** bladet för ditt projekt och välj **Lägg till** hello överst på hello sidan.
   
    ![Lägger till utdata](./media/stream-analytics-functions-redis/adding-outputs.png)
2. Välj **Service Bus-kö** som hello **Sink** och följer instruktionerna för hello på hello-skärmen. Skapas till toochoose hello namnområdet för hello Service Bus-kö i [komma igång med Service Bus-köer][servicebus-getstarted]. Klicka hello ”höger” när du är klar.
3. Ange hello följande värden:
   
   * **Händelsen serialiseraren Format**: JSON
   * **Kodning**: UTF8
   * **FORMATET**: Radseparerade
4. Klicka på hello **skapa** knappen tooadd käll- och tooverify att Stream Analytics kan ansluta toohello storage-konto.
5. I hello **frågan** fliken ersätter hello aktuell fråga med följande hello. Ersätt * [YOUR SERVICE BUS NAME] * med hello utdatanamn som du skapade i steg 3. 
   
    ```    
   
        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
   
        INTO [YOUR SERVICE BUS NAME]
   
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   
        WHERE CS1.SwitchNum != CS2.SwitchNum
   
    ```

## <a name="create-an-azure-redis-cache"></a>Skapa en Azure Redis Cache
Skapa ett Azure Redis-cache genom att följa hello .NET-avsnittet i [hur tooUse Azure Redis-Cache] [ use-rediscache] tills hello avsnittet ***konfigurera hello cacheklienter***.
När borttagningen är klar har du en ny Redis-Cache. Under **alla inställningar**väljer **åtkomstnycklar** och Skriv ner hello ***primära anslutningssträngen***.

![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion
Följ [skapa din första Azure-funktion] [ functions-getstarted] självstudiekursen tooget igång med Azure Functions. Om du redan har en Azure-funktion du skulle t.ex. toouse och sedan gå vidare för[skriva tooRedis Cache](#Writing-to-Redis-Cache)

1. Välj Apptjänster hello vänstra navigeringsfönstret i hello-portalen och sedan på ditt Azure funktionen app name tooget toohello funktionens app webbplats.
    ![Skärmbild av listan över appar services-funktionen](./media/stream-analytics-functions-redis/app-services-function-list.png)
2. Klicka på **nya funktionen > ServiceBusQueueTrigger – C#**. För hello följer anvisningarna i följande fält:
   
   * **Könamnet**: hello samma namn som hello namn du angav när du skapade hello kö i [komma igång med Service Bus-köer] [ servicebus-getstarted] (inte hello namn hello service bus). Kontrollera att du använder hello kö som är anslutna toohello Stream Analytics utdata.
   * **Service Bus-anslutning**: Välj **lägga till en anslutningssträng**. toofind hello anslutningssträngen, gå toohello klassiska portal, Välj **Service Bus**, hello service bus som du skapade och **ANSLUTNINGSINFORMATIONEN** längst hello hello-skärmen. Kontrollera att hello huvudskärmen på den här sidan. Kopiera och klistra in hello anslutningssträng. Känna sig fria tooenter alla anslutningsnamn.
     
       ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * **AccessRights**: Välj **hantera**
3. Klicka på **Skapa**

## <a name="writing-tooredis-cache"></a>Skrivning tooRedis Cache
Vi har nu skapat en Azure-funktion som läser från en Service Bus-kö. Återstår toodo är att använda våra funktionen toowrite denna data toohello Redis-Cache. 

1. Markera den nyligen skapade **ServiceBusQueueTrigger**, och klicka på **fungerar appinställningar** på hello övre högra hörnet. Välj **gå tooApp Inställningar > Inställningar > programinställningar**
2. I hello anslutning strängar avsnitt, skapar du ett namn i hello **namn** avsnitt. Klistra in primära hello anslutningssträng som du hittade i hello **skapa Redis-Cache** steget till hello **värdet** avsnitt. Välj **anpassad** står det **SQL-databas**.
3. Klicka på **spara** hello överst.
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/function-connection-string.png)
4. Gå nu tillbaka toohello App Service-inställningar och välj **Verktyg > App Service Editor (förhandsgranskning) > på > Gå**.
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/app-service-editor.png)
5. I ett redigeringsprogram, skapar du en JSON-fil med namnet **project.json** med hello följa och spara den tooyour lokal disk.
   
        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }
6. Ladda upp den här filen till hello rotkatalogen för din funktion (inte WWWROOT). Du bör se en fil med namnet **project.lock.json** visas automatiskt, bekräfta att hello Nuget paket ”StackExchange.Redis” och ”Newtonsoft.Json” har importerats.
7. I hello **run.csx** fil, Ersätt hello förgenererade koden med följande kod hello. Ersätt ”ansluten NAME” i hello lazyConnection funktionen med hello-namnet som du skapade i steg 2 i **lagra data i hello Redis-cache**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers tooa property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract hello time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using hello cache object...
        // Simple put of integral data types into hello cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from hello cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect toohello Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-hello-stream-analytics-job"></a>Starta hello Stream Analytics-jobbet
1. Starta hello telcodatagen.exe programmet. hello syntax är följande:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````
2. Hello Stream Analytics-jobbet bladet hello-portalen klickar du på **starta** hello överst på hello sidan.
   
    ![Skärmbild av startjobb](./media/stream-analytics-functions-redis/starting-job.png)
3. I hello **startjobb** bladet som visas, väljer **nu** och klicka sedan på hello **starta** knappen längst ned hello hello-skärmen. hello jobbets status har ändrats tooStarting och när vissa tid ändringar tooRunning.
   
    ![Skärmbild av valet av tid för start jobb](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Kör lösningen och kontrollera resultaten
Gå tillbaka tooyour **ServiceBusQueueTrigger** sidan du bör nu se logga instruktioner. De här loggarna visar att du har fått något från hello Service Bus-kö, placera den i hello-databas och hämtas ut använda hello tid som hello nyckel!

tooverify som dina data i Redis-cache gå tooyour Redis cache-sidan i nya hello-portalen (som visas i föregående hello [skapa ett Azure Redis-Cache](#Create-an-Azure-Redis-Cache) steg) och välj konsolen.

Nu kan du skriva Redis kommandon tooconfirm att data är i själva verket i hello-cachen.

![Skärmbild av Redis-konsolen](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Nästa steg
Vi är nöjda med hello nya saker Azure Functions Stream analytics kan göra tillsammans och vi hoppas det här låser upp nya möjligheter för dig. Om du har feedback på vad du vill att nästa anser ledigt toouse hello [Azure UserVoice-webbplatsen](https://feedback.azure.com/forums/270577-stream-analytics).

Om du är ny Microsoft Azure kan vi ber dig tootry den ut genom att registrera dig för en [kostnadsfria Azure utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/). Om du är ny tooStream Analytics, så vi ber dig för[skapa ditt första Stream Analytics-jobb](stream-analytics-create-a-job.md).

Om du behöver hjälp eller har frågor, gör ett inlägg i [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) eller [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forum. 

Du kan också se hello följande resurser:

* [Azure Functions, info för utvecklare](../azure-functions/functions-reference.md)
* [Azure Functions C# för utvecklare](../azure-functions/functions-reference-csharp.md)
* [Azure Functions F # för utvecklare](../azure-functions/functions-reference-fsharp.md)
* [Azure Functions NodeJS för utvecklare](../azure-functions/functions-reference.md)
* [Azure Functions-utlösare och bindningar](../azure-functions/functions-triggers-bindings.md)
* [Hur toomonitor Azure Redis-Cache](../redis-cache/cache-how-to-monitor.md)

toostay uppdaterad på alla hello nyheter och funktioner, Följ [ @AzureStreaming ](https://twitter.com/AzureStreaming) på Twitter.

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
