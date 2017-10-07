---
title: "aaaGetting igång med queue storage- och Visual Studio anslutna tjänster (Webbjobb projekt) | Microsoft Docs"
description: "Hur tooget igång med Azure Queue storage i ett Webbjobb projekt när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 720e96fda734c31e1b1d68d4f95aa9531a20a3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Komma igång med Azure Queue storage och Visual Studio anslutna tjänster (Webbjobb projekt)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur få igång med Azure Queue storage i Visual Studio Azure Webjobs-projekt när du har skapat eller refererar till ett Azure storage-konto med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan. När du lägger till ett storage-konto tooa Webbjobb projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan hello lämplig Azure Storage NuGet-paket installeras, hello lämpliga .NET-referenserna är tillagda toohello projektet och anslutningssträngar för hello storage-konto uppdateras i hello App.config-fil.  

Den här artikeln innehåller C#-kodexempel som visar hur toouse hello Azure WebJobs SDK version 1.x med hello Azure Queue storage-tjänst.

Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS. Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto. Se [Kom igång med Azure Queue Storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md) för mer information. Läs mer om ASP.NET [ASP.NET](http://www.asp.net).

## <a name="how-tootrigger-a-function-when-a-queue-message-is-received"></a>Hur tootrigger en funktion när ett kömeddelande tas emot
toowrite anropar en funktion som hello WebJobs SDK när ett kömeddelande tas emot använder hello **QueueTrigger** attribut. hello Attributkonstruktorn använder en strängparameter som anger hello kön toopoll hello namn. toosee hur tooset hello könamnet dynamiskt, kolla [hur tooset konfigurationsalternativ](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Sträng Kömeddelanden
I följande exempel hello, hello kön innehåller ett strängmeddelande, så **QueueTrigger** är tillämpade tooa strängparameter med namnet **logMessage** som innehåller hello innehållet hello kön meddelandet. Hej funktionen [skriver en logg meddelandet toohello instrumentpanelen](#how-to-write-logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Förutom **sträng**, hello kan vara en bytematris en **CloudQueueMessage** objekt eller en POCO som du definierar.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö
I följande exempel hello, hello kön meddelandet innehåller JSON för en **BlobInformation** objekt som innehåller en **BlobName** egenskapen. hello SDK deserializes automatiskt hello-objektet.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

hello SDK använder hello [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize och avbryta serialiseringen för meddelanden. Om du skapar meddelanden i kö i ett program som inte använder hello WebJobs SDK skriva du kod som hello följande exempel toocreate en POCO kömeddelande som hello SDK kan parsa.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Async-funktion
hello följande async-funktionen [skriver en logg toohello instrumentpanelen](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynkrona funktioner kan ta en [annullering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)som visas i följande exempel som kopierar en blobb hello. (En förklaring av hello **queueTrigger** platshållare finns hello [Blobbar](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) avsnitt.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-hello-queuetrigger-attribute-works-with"></a>Typer hello QueueTrigger attributet fungerar med
Du kan använda **QueueTrigger** med hello följande typer:

* **sträng**
* En POCO-typen som serialiseras som JSON
* **byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Avsökningen algoritm
hello SDK implementerar en exponentiell tillbaka av algoritmen tooreduce hello effekten av inaktiv-kön avsökning på transaktion lagringskostnader.  När ett meddelande hittas hello SDK väntar två sekunder och söker efter en annan message; När det finns inget meddelande väntar på fyra sekunder innan du försöker igen. När efterföljande misslyckade försök tooget ett kömeddelande hello väntetiden fortsätter tooincrease tills den når maximal väntetid för hello, vilka standardvärden tooone minut. [hello väntetiden konfigureras](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Flera instanser
Om ditt webbprogram som körs på flera instanser, kontinuerliga Webbjobb som körs på varje dator och varje dator ska vänta för utlösare och försök toorun funktioner. I vissa fall kan detta leda toosome funktioner bearbetning hello samma data två gånger så funktioner ska vara idempotent (skrivs så som anropar dem flera gånger med samma indata inte ger hello duplicera resultat).  

## <a name="parallel-execution"></a>Parallell körning
Om du har flera funktioner som lyssnar på olika köer anropar hello SDK dem parallellt när meddelanden tas emot samtidigt.

hello sak samma gäller när flera meddelanden tas emot för en enskild kö. Som standard hello SDK hämtar en batch med 16 Kömeddelanden i taget och utför hello-funktion som behandlar dem parallellt. [hello storlek är konfigurerbar](#how-to-set-configuration-options). När hello nummer bearbetas hämtar ned toohalf av hello batchstorlek, hämtar en annan batch hello SDK och påbörjar bearbetningen av dessa meddelanden. Hello maximalt antal samtidiga meddelanden som bearbetas per funktion är därför en och en halv gånger hello batchstorlek. Den här begränsningen gäller separat tooeach funktion som har en **QueueTrigger** attribut. Om du inte vill parallell körning för meddelanden som tas emot i en kö kan du ange hello batch storlek too1.

## <a name="get-queue-or-queue-message-metadata"></a>Hämta kön eller kön meddelandet metadata
Du kan få följande meddelandeegenskaper genom att lägga till parametrar toohello Metodsignaturen hello:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **strängen** queueTrigger (innehåller meddelandetext)
* **strängen** id
* **strängen** popReceipt
* **int** dequeueCount

Om du vill toowork direkt med hello Azure storage-API, du kan också lägga till en **CloudStorageAccount** parameter.

hello skriver följande exempel alla denna metadata tooan INFO programloggen. I exemplet hello innehåll både logMessage och queueTrigger hello av hälsningsmeddelande för kön.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Här är en exempellogg som skrivits av hello exempelkod:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Korrekt avslutning
En funktion som körs i ett kontinuerligt Webbjobb kan acceptera en **CancellationToken** parameter som aktiverar hello operativsystemet toonotify hello fungerar när hello Webbjobb kommer toobe avslutades. Du kan använda det här meddelandet toomake att hello funktionen inte avslutas oväntat på ett sätt som lämnar data i ett inkonsekvent tillstånd.

följande exempel visar hur hello toocheck för nära förestående Webbjobb avslutning i en funktion.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Obs:** hello instrumentpanelen kanske inte korrekt visar hello status och utdata för funktioner som har stängts av.

Mer information finns i [WebJobs korrekt avslutning](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-toocreate-a-queue-message-while-processing-a-queue-message"></a>Hur toocreate en kö meddelande vid bearbetning av ett meddelande i kön
toowrite en funktion som skapar ett nytt meddelande i kön, Använd hello **kön** attribut. Som **QueueTrigger**kan du skicka in hello könamnet som en sträng eller [ange hello könamnet dynamiskt](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Sträng Kömeddelanden
hello följande kodexempel icke asynkron skapar ett nytt meddelande i kön i hello kö med namnet ”outputqueue” med hello samma innehåll som kön hello-meddelande togs emot i hello kö med namnet ”inputqueue”. (För asynkrona funktioner använder **IAsyncCollector<T>**  enligt senare i det här avsnittet.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö
toocreate ett kömeddelande som innehåller en POCO i stället för en sträng, pass hello POCO skriver som en output-parameter toohello **kön** Attributkonstruktorn.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

hello SDK Serialiserar automatiskt hello objektet tooJSON. Ett kömeddelande skapas alltid, även om hello-objektet är null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Skapa flera meddelanden eller i async-funktioner
toocreate flera meddelanden gör hello parametertypen för hello utgående kö **ICollector<T>**  eller **IAsyncCollector<T>**som visas i följande exempel hello.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Varje meddelande i kön skapas omedelbart när hello **Lägg till** -metoden anropas.

### <a name="types-that-hello-queue-attribute-works-with"></a>Typer hello kön attributet fungerar med
Du kan använda hello **kön** attributet på hello följande parametertyper:

* **i strängen** (skapar kömeddelande om parametervärdet är icke-null när hello funktionen avslutas)
* **ut byte []** (fungerar som **sträng**)
* **ut CloudQueueMessage** (fungerar som **sträng**)
* **ut POCO** (en serialiserbar typ. skapar ett meddelande med ett null-objekt om hello parameter är null när hello funktionen avslutas)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (för att skapa meddelanden manuellt med hjälp av hello Azure Storage-API direkt)

### <a name="use-webjobs-sdk-attributes-in-hello-body-of-a-function"></a>Använda WebJobs-SDK-attribut i hello brödtext
Om du behöver toodo vissa fungerar i din funktion innan du använder ett WebJobs-SDK-attribut som **kön**, **Blob**, eller **tabell**, du kan använda hello **IBinder** gränssnitt.

hello följande exempel tar ett inkommande kö-meddelande och skapar ett nytt meddelande med hello samma innehåll i en utgående kö. hello utgående kö namn anges av koden i hello brödtexten i hello-funktion.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

Hej **IBinder** gränssnitt kan även användas med hello **tabell** och **Blob** attribut.

## <a name="how-tooread-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Hur tooread och skriva BLOB-objekt och tabeller under bearbetning av ett meddelande i kön
Hej **Blob** och **tabell** attribut aktivera tooread och skriva BLOB och tabeller. hello prover i det här avsnittet gäller tooblobs. Kodexempel som visar hur tootrigger bearbetar när blobbar har skapats eller uppdaterats, se [hur toouse Azure blob storage med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), och kodexempel som läsa och skriva tabeller, se [hur toouse Azure-tabellen lagring med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Sträng Kömeddelanden utlösa blob-åtgärder
För ett meddelande i kön som innehåller en sträng, **queueTrigger** är en platshållare som du kan använda i hello **Blob** attributets **blobPath** parametrar som innehåller hello innehållet i hello-meddelande.

hello följande exempel används **dataströmmen** tooread och skriva BLOB-objekt. hälsningsmeddelande för kön är en blob som finns i hello textblobs behållare hello namn. En kopia av hello blob med ”-nya” tillagda toohello namnet skapas i hello samma behållare.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Hej **Blob** attributet konstruktorn tar en **blobPath** parameter som anger hello-behållaren och blobbnamnet. Mer information om den här platshållaren finns [hur toouse Azure blob storage med hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).

När attributet hello decorates en **dataströmmen** objekt, en annan konstruktorparametern anger hello **FileAccess** läge som Läs-, Skriv- eller läsning och skrivning.

hello följande exempel används en **CloudBlockBlob** objekt toodelete en blob. Hej kömeddelande är hello namn hello-blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö
Du kan använda platshållare att egenskaperna för hello objekt i hello-namnet för en POCO som lagras som JSON i kön hälsningsmeddelande **kön** attributets **blobPath** parameter. Du kan också använda kön metadata egenskapsnamn som platshållare. Se [hämta kön eller kön meddelandet metadata](#get-queue-or-queue-message-metadata).

hello kopieras följande exempel en blob tooa nya blob med ett annat tillägg. hälsningsmeddelande för kön är en **BlobInformation** -objekt som innehåller **BlobName** och **BlobNameWithoutExtension** egenskaper. hello egenskapsnamn används som platshållare i hello blob sökväg för hello **Blob** attribut.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

hello SDK använder hello [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize och avbryta serialiseringen för meddelanden. Om du skapar meddelanden i kö i ett program som inte använder hello WebJobs SDK skriva du kod som hello följande exempel toocreate en POCO kömeddelande som hello SDK kan parsa.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Om du behöver toodo vissa fungerar i din funktion innan du binder en tooan blob-objektet, du kan använda hello-attributet i hello brödtext hello funktionen enligt [använder WebJobs-SDK-attribut i hello brödtext](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-hello-blob-attribute-with"></a>Du kan använda hello Blob-attribut med
Hej **Blob** attributet kan användas med hello följande typer:

* **Dataströmmen** (läsa eller skriva som anges av parametern hello FileAccess konstruktor)
* **TextReader**
* **TextWriter**
* **strängen** (läsa)
* **i strängen** (skriva; skapar en blob endast om hello strängparameter är icke-null om hello funktionen returnerar)
* POCO (läsa)
* ut POCO (skriva; alltid skapar en blob, skapar som null-objekt om POCO-parametern är null när hello funktionen returnerar)
* **CloudBlobStream** (Skriv)
* **ICloudBlob** (läsa eller skriva)
* **CloudBlockBlob** (läsa eller skriva)
* **CloudPageBlob** (läsa eller skriva)

## <a name="how-toohandle-poison-messages"></a>Hur toohandle förgiftade meddelanden
Meddelanden vars innehåll orsakar en funktionen toofail kallas *förgiftade meddelanden*. När hello funktionen misslyckas hello kön meddelandet tas inte bort och slutligen hämtas igen, vilket ledde hello cykel toobe upprepas. hello SDK kan avbryta hello cykel efter ett begränsat antal upprepningar eller så kan du göra det manuellt.

### <a name="automatic-poison-message-handling"></a>Hantering av automatisk skadligt meddelande
hello SDK ska anropa en funktion in too5 gånger tooprocess ett kömeddelande. Om hello femte försök misslyckas är hello-meddelande har flyttats tooa skadligt kö. Du kan se hur tooconfigure hello maximalt antal försök i [hur tooset konfigurationsalternativ](#how-to-set-configuration-options).

hello skadligt kön heter *{originalqueuename}*-skadligt. Du kan skriva en funktion tooprocess meddelanden från hello skadligt kö med loggning av dem eller skicka ett meddelande om att manuella åtgärder krävs.

I följande exempel hello hello **CopyBlob** funktionen kommer att misslyckas när ett kömeddelande innehåller hello namnet på en blob som inte finns. När det händer kan flyttas hello-meddelande från hello copyblobqueue kön toohello copyblobqueue poison kö. Hej **ProcessPoisonMessage** och sedan loggar hello skadligt meddelande.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed toocopy blob, name=" + blobName);
        }

hello följande bild visar konsolens utdata från de här funktionerna när ett skadligt meddelande bearbetas.

![Konsolens utdata för hantering av skadligt meddelande](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Hantering av manuell skadligt meddelande
Du kan hämta hello många gånger meddelandet har plockats för bearbetning genom att lägga till en **int** parameter med namnet **dequeueCount** tooyour funktion. Därefter kan du kontrollera hello status Created antalet i Funktionskoden och utföra egna skadligt meddelandehantering när hello antalet överskrider ett tröskelvärde som visas i följande exempel hello.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed toocopy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-tooset-configuration-options"></a>Hur tooset konfigurationsalternativ
Du kan använda hello **JobHostConfiguration** typen tooset hello följande konfigurationsalternativ:

* Ange hello SDK-anslutningssträngar i koden.
* Konfigurera **QueueTrigger** inställningar, till exempel maximalt antal har status Created.
* Hämta könamn från konfigurationen.

### <a name="set-sdk-connection-strings-in-code"></a>Ange SDK anslutningssträngar i kod
Hello SDK-anslutningssträngar kan i koden du toouse egna namn på anslutning sträng i configuration-filer eller miljövariabler som visas i följande exempel hello.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a>Konfigurera inställningar för QueueTrigger
Du kan konfigurera följande inställningar som gäller toohello kön meddelandebehandling hello:

* Maximalt antal meddelanden i kö som tas upp samtidigt toobe utförs parallellt hello (standard är 16).
* Hej maximalt antal försök innan ett kömeddelande skickas tooa skadligt kön (standardvärdet är 5).
* hello väntetiden innan avsökning igen när en kö är tomt (standard är 1 minut).

följande exempel visar hur hello tooconfigure dessa inställningar:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Ange värden för WebJobs SDK konstruktorparametrarna i koden
Vill ibland du toospecify en kö, ett blob-namn eller en behållare eller en tabell i stället för att hårdkoda ge den namnet. Du kan exempelvis toospecify hello könamnet för **QueueTrigger** i en konfiguration av fil- eller miljö variabel.

Du kan göra det genom att passera i en **NameResolver** objekt toohello **JobHostConfiguration** typen. Du inkludera särskilda platshållare omges av procenttecken (%) i WebJobs SDK attributet konstruktorn parametrar och **NameResolver** koden anger hello faktiska värden toobe används i stället för dessa platshållare.

Till exempel anta att du vill toouse en kö med namnet logqueuetest i hello testmiljö och en namngiven logqueueprod i produktion. I stället för en hårdkodad könamnet önskade toospecify hello namnet på en post i hello **appSettings** samling som skulle ha hello faktiska könamnet. Om hello **appSettings** nyckeln är logqueue, din funktion kan se ut så hello följande exempel.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Din **NameResolver** klassen kan sedan hämta hello könamnet från **appSettings** som visas i följande exempel hello:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Du skickar hello **NameResolver** klassen i toohello **JobHost** objekt som visas i följande exempel hello.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Obs:** kön tabell och blobbnamnen är löst varje gång som en funktion kallas, men blob-behållaren namnmatchning bara när hello programmet startas. Du kan inte ändra namnet för blob-behållare medan hello jobbet körs.

## <a name="how-tootrigger-a-function-manually"></a>Hur tootrigger en funktion manuellt
tootrigger en funktion manuellt, använda hello **anropa** eller **CallAsync** metod på hello **JobHost** objektet och hello **NoAutomaticTrigger** attribut i hello funktion, som visas i följande exempel hello.

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-toowrite-logs"></a>Hur toowrite loggar
hello instrumentpanelen visar loggar på två platser: hello sidan för hello Webbjobb och hello-sidan för ett särskilt Webbjobb-anrop.

![Loggar Webbjobb på sidan](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Loggar i funktionen anrops-sida](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Utdata från konsolen metoderna i en funktion eller hello **Main()** metoden visas i hello instrumentpanelssidan för hello Webbjobb, inte i hello-sida för en viss metodanropet. Utdata från hello TextWriter-objekt som du får från en parameter i Metodsignaturen visas i hello instrumentpanelssidan för ett metodanrop.

Konsolens utdata kan inte länkade tooa viss metodanropet eftersom hello konsolen är enkeltrådad, medan många jobbfunktioner hello kan köras samtidigt. Det är därför hello SDK innehåller varje funktionsanrop med sin egen unika loggen skrivarobjekt.

toowrite [spårning programloggarna](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), använda **Console.Out** (skapar loggar som är märkta som INFO) och **Console.Error** (skapar loggar som är märkta som fel). Ett alternativ är toouse [spårningen eller TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), vilket möjliggör utförlig, varning, och kritiska nivåer i tillägget tooInfo och fel. Spårningsloggar för programmet visas i hello web app-loggfiler, Azure-tabeller eller Azure BLOB-objekt beroende på hur du konfigurerar din Azure-webbapp. Som gäller för alla konsolens utdata visas också hello senaste 100 programloggarna i hello instrumentpanelssidan för hello Webbjobb, inte hello sidan för ett funktionsanrop.

Konsolens utdata visas i hello instrumentpanelen endast om hello programmet körs i en Azure-Webbjobb inte om hello program körs lokalt eller i en annan miljö.

Du kan inaktivera loggning genom att ange hello instrumentpanelen anslutning sträng toonull. Mer information finns i [hur tooset konfigurationsalternativ](#how-to-set-configuration-options).

hello följande exempel visar flera olika sätt toowrite loggar:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

I hello WebJobs SDK-instrumentpanelen, hello utdata från hello **TextWriter** objekt visar upp går toohello sida för en viss fungera anrop och välj **växla utdata**:

![Anrops-länk](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Loggar i funktionen anrops-sida](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

I hello WebJobs SDK-instrumentpanelen, hello senaste 100 rader i konsolen utdata visar dig när du gå toohello sidan för hello Webbjobb (inte för hello funktionsanrop) och väljer **växla utdata**.

![Växla utdata](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

I ett kontinuerligt Webbjobb programloggarna visas i/data/jobb/kontinuerlig/*{webjobname}*/job_log.txt i filsystemet för hello web app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

I ett Azure blob hello program loggar ut så här: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, fel, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Och i en Azure-tabellen hello **Console.Out** och **Console.Error** loggar ut så här:

![Loggen för information i tabellen](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Felloggen i tabellen](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Nästa steg
Den här artikeln har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure köer. Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).

