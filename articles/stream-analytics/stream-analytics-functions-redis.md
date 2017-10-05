---
title: "Strömma Analytics realtid bearbetning för Azure Functions | Microsoft Docs"
description: "Lär dig hur du använder en Azure-funktion anslutna en Service Bus-kö, för att fylla i en Azure Redis-Cache från utdata från ett Stream Analytics-jobb."
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
ms.openlocfilehash: ad14cc858ff513573e2718a26a9ab5c524e1adc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Hur du lagrar data från Azure Stream Analytics i en Azure Redis-Cache med hjälp av Azure-funktioner
Azure Stream Analytics kan du snabbt utveckla och distribuera prisvärda lösningar för att få realtidsinsikter från enheter, sensorer, infrastruktur, och program eller en dataström. Den gör olika användningsfall, till exempel realtid hantering och övervakning kommandot och, att upptäcka bedrägerier, anslutna bilar och mycket mer. I många sådana scenarier kanske du vill lagra data i en distribuerad datalager, till exempel en Azure Redis-cache för utdata av Azure Stream Analytics.

Anta att du är en del av en telekommunikation företag. Du försöker identifiera SIM bedrägeri där flera anrop som kommer från samma identitet, på samma gång, men i olika geografiskt platser. Du har gett med att lagra alla potentiella bedrägliga telefonsamtal i en Azure Redis-cache. I den här bloggen ger vi riktlinjer för hur du lätt kan utföra uppgiften. 

## <a name="prerequisites"></a>Krav
Slutför den [realtid att upptäcka bedrägerier] [ fraud-detection] hanteringspaketen för ASA

## <a name="architecture-overview"></a>Arkitektur, översikt
![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/architecture-overview.png)

I föregående bild visas kan Stream Analytics strömning indata för att fråga och skickas till utdata. Baserat på resultatet kan Azure Functions sedan utlösa någon typ av händelse. 

I den här bloggen fokusera på Azure Functions en del av denna pipeline eller mer specifikt utlösa en händelse som lagrar falska data till cachen.
När du har slutfört den [realtid att upptäcka bedrägerier] [ fraud-detection] kursen har du indata (en händelsehubb), en fråga och utdata (blob storage) redan konfigurerad och körs. I den här bloggen ändra vi utdata om du vill använda en Service Bus-kö i stället. Sedan kan ansluta vi en Azure-funktion för kön. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Skapa och koppla en Service Bus-kö-utdata
Om du vill skapa en Service Bus-kö följa steg 1 och 2 i avsnittet .NET i [komma igång med Service Bus-köer][servicebus-getstarted].
Nu ska vi ansluta kön till Stream Analytics-jobbet har skapats i tidigare bedrägeri identifiering genomgången.

1. I Azure-portalen går du till den **utdata** bladet för ditt projekt och välj **Lägg till** överst på sidan.
   
    ![Lägger till utdata](./media/stream-analytics-functions-redis/adding-outputs.png)
2. Välj **Service Bus-kö** som den **Sink** och följ anvisningarna på skärmen. Kontrollera att namnområdet för Service Bus-kö som du skapade i [komma igång med Service Bus-köer][servicebus-getstarted]. Klicka på knappen ”höger” när du är klar.
3. Ange följande värden:
   
   * **Händelsen serialiseraren Format**: JSON
   * **Kodning**: UTF8
   * **FORMATET**: Radseparerade
4. Klicka på den **skapa** knappen för att lägga till den här datakällan och kontrollera att Stream Analytics kan ansluta till lagringskontot.
5. I den **frågan** fliken ersätter den aktuella frågan med följande. Ersätt * [YOUR SERVICE BUS NAME] * med utdatanamnet som du skapade i steg 3. 
   
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
Skapa ett Azure Redis-cache genom att följa avsnittet .NET i [så Använd Azure Redis-Cache] [ use-rediscache] tills avsnittet kallas ***konfigurera cache-klienter***.
När borttagningen är klar har du en ny Redis-Cache. Under **alla inställningar**väljer **åtkomstnycklar** och notera den ***primära anslutningssträngen***.

![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion
Följ [skapa din första Azure-funktion] [ functions-getstarted] kursen och kom igång med Azure Functions. Om du redan har en Azure funktion som du vill använda sedan gå vidare till [skrivning till Redis-Cache](#Writing-to-Redis-Cache)

1. Välj Apptjänster från det vänstra navigeringsfönstret i portalen och sedan på ditt Azure funktionsnamn app att hämta funktionens app webbplats.
    ![Skärmbild av listan över appar services-funktionen](./media/stream-analytics-functions-redis/app-services-function-list.png)
2. Klicka på **nya funktionen > ServiceBusQueueTrigger – C#**. Följ dessa instruktioner för följande fält:
   
   * **Könamnet**: samma namn som det namn du angav när du skapade i kön i [komma igång med Service Bus-köer] [ servicebus-getstarted] (inte namnet på service bus). Kontrollera att du använder den kö som är ansluten till Stream Analytics-utdata.
   * **Service Bus-anslutning**: Välj **lägga till en anslutningssträng**. För att hitta anslutningssträngen, gå till den klassiska portalen väljer **Service Bus**, service bus som du skapade och **ANSLUTNINGSINFORMATIONEN** längst ned på skärmen. Kontrollera att du är i huvudfönstret på den här sidan. Kopiera och klistra in anslutningssträngen. Du kan ange ett anslutningsnamn.
     
       ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * **AccessRights**: Välj **hantera**
3. Klicka på **Skapa**

## <a name="writing-to-redis-cache"></a>Skrivning till Redis-Cache
Vi har nu skapat en Azure-funktion som läser från en Service Bus-kö. Återstår gör är vår funktionen för att skriva data till Redis-Cache. 

1. Markera den nyligen skapade **ServiceBusQueueTrigger**, och klicka på **fungerar appinställningar** på övre högra hörnet. Välj **går du till App Service-Inställningar > Inställningar > programinställningar**
2. I avsnittet anslutning strängar skapar du ett namn i den **namn** avsnitt. Klistra in den primära anslutningssträng som du hittade i den **skapa Redis-Cache** steg i den **värdet** avsnitt. Välj **anpassad** står det **SQL-databas**.
3. Klicka på **spara** längst upp.
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/function-connection-string.png)
4. Nu gå tillbaka till App Service-inställningar och välj **Verktyg > App Service Editor (förhandsgranskning) > på > Gå**.
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/app-service-editor.png)
5. I ett redigeringsprogram, skapar du en JSON-fil med namnet **project.json** med följande och spara den till den lokala hårddisken.
   
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
6. Ladda upp den här filen till rotkatalogen för din funktion (inte WWWROOT). Du bör se en fil med namnet **project.lock.json** visas automatiskt bekräftar att Nuget-paketen ”StackExchange.Redis” och ”Newtonsoft.Json” har importerats.
7. I den **run.csx** fil, Ersätt förgenererade koden med följande kod. Ersätt ”ansluten NAME” i lazyConnection-funktionen med namnet som du skapade i steg 2 i **lagra data i Redis-cache**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
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

## <a name="start-the-stream-analytics-job"></a>Starta Stream Analytics-jobbet
1. Starta programmet telcodatagen.exe. Användningen är följande:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````
2. I bladet Stream Analytics-jobbet i portalen klickar du på **starta** överst på sidan.
   
    ![Skärmbild av startjobb](./media/stream-analytics-functions-redis/starting-job.png)
3. I den **startjobb** bladet som visas, väljer **nu** och klicka sedan på den **starta** längst ned på skärmen. Jobbet status ändras till Start och efter vissa tidsändringar körs.
   
    ![Skärmbild av valet av tid för start jobb](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Kör lösningen och kontrollera resultaten
Gå tillbaka till din **ServiceBusQueueTrigger** sidan du bör nu se logga instruktioner. De här loggarna visar att du har fått något från Service Bus-kö, placera den i databasen och hämtas ut på tid som nyckel!

Kontrollera att dina data är i Redis-cache, gå till sidan Redis-cache i den nya portalen (som visas i den föregående [skapa ett Azure Redis-Cache](#Create-an-Azure-Redis-Cache) steg) och välj konsolen.

Nu kan du skriva Redis-kommandon för att bekräfta att data är i själva verket i cacheminnet.

![Skärmbild av Redis-konsolen](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Nästa steg
Vi är nöjda med nya saker Azure Functions Stream analytics kan göra tillsammans och vi hoppas det här låser upp nya möjligheter för dig. Om du har feedback på vad du vill nu passa på att använda den [Azure UserVoice-webbplatsen](https://feedback.azure.com/forums/270577-stream-analytics).

Om du är ny Microsoft Azure kan vi bjuda in dig på detta genom att registrera dig för en [kostnadsfria Azure utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/). Om du är nybörjare på Stream Analytics och gärna [skapa ditt första Stream Analytics-jobb](stream-analytics-create-a-job.md).

Om du behöver hjälp eller har frågor, gör ett inlägg i [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) eller [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forum. 

Du kan också se följande resurser:

* [Azure Functions, info för utvecklare](../azure-functions/functions-reference.md)
* [Azure Functions C# för utvecklare](../azure-functions/functions-reference-csharp.md)
* [Azure Functions F # för utvecklare](../azure-functions/functions-reference-fsharp.md)
* [Azure Functions NodeJS för utvecklare](../azure-functions/functions-reference.md)
* [Azure Functions-utlösare och bindningar](../azure-functions/functions-triggers-bindings.md)
* [Hur du övervakar Azure Redis-Cache](../redis-cache/cache-how-to-monitor.md)

Om du vill hålla dig uppdaterad på alla de senaste nyheterna och funktioner, Följ [ @AzureStreaming ](https://twitter.com/AzureStreaming) på Twitter.

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
