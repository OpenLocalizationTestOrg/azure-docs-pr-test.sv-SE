---
title: "Använd Azure-kölagring med WebJobs SDK:n"
description: "Lär dig mer om att använda Azure queue storage med WebJobs SDK. Skapa och ta bort köer. Infoga, granska, hämta och ta bort Kömeddelanden och mycket mer."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: dbfac5d9-f4a0-4e3e-9ecc-af3d7bf80463
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 63b987a2c9471f2929b8d2dd605323910d2ad43b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a>Använd Azure-kölagring med WebJobs SDK:n
## <a name="overview"></a>Översikt
Den här guiden innehåller C#-kodexempel som visar hur du använder Azure WebJobs SDK version 1.x med tjänsten Azure queue storage.

Handboken förutsätter att du vet [hur du skapar ett Webbjobb-projekt i Visual Studio med anslutning strängar som pekar på ditt lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller [flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

De flesta av kodavsnitten Visa endast funktioner, inte koden som skapar den `JobHost` objekt som i följande exempel:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

Guiden innehåller följande ämnen:

* [Hur du utlöser en funktion när ett kömeddelande tas emot](#trigger)
  * Sträng Kömeddelanden
  * POCO Kömeddelanden
  * Async-funktion
  * Attributet QueueTrigger fungerar med typer
  * Avsökningen algoritm
  * Flera instanser
  * Parallell körning
  * Hämta kön eller kön meddelandet metadata
  * Korrekt avslutning
* [Så här skapar du ett kömeddelande under bearbetning av ett meddelande i kön](#createqueue)
  * Sträng Kömeddelanden
  * POCO Kömeddelanden
  * Skapa flera meddelanden eller i async-funktioner
  * Attributet kön fungerar med typer
  * Använda WebJobs-SDK-attribut i brödtexten för en funktion
* [Hur kan läsa och skriva BLOB under bearbetning av ett meddelande i kön](#blobs)
  * Sträng Kömeddelanden
  * POCO Kömeddelanden
  * Blob-attributet som fungerar med typer
* [Hur du hanterar förgiftade meddelanden](#poison)
  * Hantering av automatisk skadligt meddelande
  * Hantering av manuell skadligt meddelande
* [Hur du ställer in konfigurationsalternativ](#config)
  * Ange SDK anslutningssträngar i kod
  * Konfigurera inställningar för QueueTrigger
  * Ange värden för WebJobs SDK konstruktorparametrarna i koden
* [Hur du utlöser en funktion manuellt](#manual)
* [Hur du skriver loggar](#logs)
* [Hantera fel och konfigurera tidsgränser](#errors)
* [Nästa steg](#nextsteps)

## <a id="trigger"></a>Hur du utlöser en funktion när ett kömeddelande tas emot
Om du vill skriva en funktion som WebJobs SDK anropar när ett kömeddelande tas emot, använder den `QueueTrigger` attribut. Attributkonstruktorn använder en strängparameter som anger namnet på kön avsöker. Du kan också [ange könamnet dynamiskt](#config).

### <a name="string-queue-messages"></a>Sträng Kömeddelanden
I följande exempel kön innehåller ett strängmeddelande, så `QueueTrigger` tillämpas på en som strängparameter med namnet `logMessage` som innehåller innehållet i kön meddelandet. Funktionen [skriver ett loggmeddelande till instrumentpanelen](#logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Förutom `string`, parametern kan vara en bytematris en `CloudQueueMessage` objekt eller en POCO som du definierar.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö
I följande exempel innehåller kömeddelandet JSON för en `BlobInformation` objekt som innehåller en `BlobName` egenskap. SDK deserializes automatiskt objektet.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

SDK använder den [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) serialisera och deserialisera meddelanden. Om du skapar meddelanden i kö i ett program som inte använder WebJobs SDK kan du skriva kod som i följande exempel för att skapa ett kömeddelande för POCO som SDK kan parsa.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Async-funktion
Följande async-funktion [skriver en logg till instrumentpanelen](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynkrona funktioner kan ta en [annullering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)som visas i följande exempel som kopierar en blob. (En förklaring av den `queueTrigger` platshållare, finns det [Blobbar](#blobs) avsnitt.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Attributet QueueTrigger fungerar med typer
Du kan använda `QueueTrigger` med följande typer:

* `string`
* En POCO-typen som serialiseras som JSON
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Avsökningen algoritm
SDK implementerar en exponentiell tillbaka av algoritmen för att minska effekten av inaktiv-kön avsökning på transaktion lagringskostnader.  När ett meddelande hittas SDK väntar två sekunder och söker efter en annan message; När det finns inget meddelande väntar på fyra sekunder innan du försöker igen. Väntetiden fortsätter att öka tills väntetiden, där standardinställningen är en minut efter efterföljande misslyckade försök att få ett meddelande i kön. [Väntetiden kan konfigureras](#config).

### <a id="instances"></a>Flera instanser
Om ditt webbprogram som körs på flera instanser är ett kontinuerligt Webbjobb som körs på varje dator och varje dator ska vänta tills utlösare och försök att köra funktioner. WebJobs SDK kö utlösaren förhindrar automatiskt en funktion från att bearbeta ett kömeddelande flera gånger. funktioner har inte att skrivas till vara idempotent. Men om du vill se till att endast en instans av en funktion körs även när det finns flera instanser av värden webbprogram, kan du använda den `Singleton` attribut.

### <a id="parallel"></a>Parallell körning
Om du har flera funktioner som lyssnar på olika köer anropar SDK dem parallellt när meddelanden tas emot samtidigt.

Samma sak gäller när flera meddelanden tas emot för en enskild kö. Som standard SDK hämtar en batch med 16 Kömeddelanden i taget och kör den funktion som behandlar dem parallellt. [Batchstorleken konfigureras](#config). När antalet bearbetas hämtar till hälften av batchstorleken, hämtar en annan batch SDK och påbörjar bearbetningen av dessa meddelanden. Därför är det maximala antalet samtidiga meddelanden som bearbetas per funktion en och en halv gånger batchstorlek. Den här begränsningen gäller separat för varje funktion som har en `QueueTrigger` attribut.

Om du inte vill parallell körning för meddelanden som tas emot i en kö kan ange du batchstorleken till 1. Se även **mer kontroll över kön bearbetning** i [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>Hämta kön eller kön meddelandet metadata
Du kan få följande meddelandeegenskaper genom att lägga till parametrar Metodsignaturen:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (innehåller meddelandetext)
* `string`ID
* `string`popReceipt
* `int`dequeueCount

Om du vill arbeta direkt med Azure storage API, du kan också lägga till en `CloudStorageAccount` parameter.

I följande exempel skriver alla dessa metadata till en INFO programloggen. Både logMessage och queueTrigger innehåller innehållet i kön meddelandet i det här exemplet.

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

Här är en exempellogg som skrivits av exempelkoden:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <a id="graceful"></a>Korrekt avslutning
En funktion som körs i ett kontinuerligt Webbjobb kan acceptera en `CancellationToken` parameter som gör operativsystemet för att meddela funktionen när Webbjobbet håller på att avslutas. Du kan använda det här meddelandet för att kontrollera att funktionen inte avslutas oväntat på ett sätt som lämnar data i ett inkonsekvent tillstånd.

I följande exempel visas hur du kontrollerar för nära förestående Webbjobb avslutning i en funktion.

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

**Obs:** instrumentpanelen kanske inte korrekt visar status och utdata för funktioner som har stängts av.

Mer information finns i [WebJobs korrekt avslutning](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a id="createqueue"></a>Så här skapar du ett kömeddelande under bearbetning av ett meddelande i kön
Om du vill skriva en funktion som skapar ett nytt meddelande i kön, använder den `Queue` attribut. Som `QueueTrigger`kan du skicka in könamnet som en sträng eller [ange könamnet dynamiskt](#config).

### <a name="string-queue-messages"></a>Sträng Kömeddelanden
Följande kodexempel icke asynkron skapar ett nytt meddelande i kön i kön med namnet ”outputqueue” med samma innehåll som kön meddelandet som togs emot i kön med namnet ”inputqueue”. (För asynkrona funktioner använder `IAsyncCollector<T>` enligt senare i det här avsnittet.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö
Skicka POCO-typ för att skapa ett meddelande i kön som innehåller en POCO i stället för en sträng som en utdataparameter till den `Queue` Attributkonstruktorn.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

SDK Serialiserar automatiskt objektet till JSON. Ett kömeddelande skapas alltid, även om objektet är null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Skapa flera meddelanden eller i async-funktioner
Om du vill skapa flera meddelanden gör parametertypen för utgående kö `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande exempel.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Varje meddelande i kön skapas omedelbart när den `Add` metoden anropas.

### <a name="types-that-the-queue-attribute-works-with"></a>Typer som attributet kön fungerar med
Du kan använda den `Queue` -attributet på följande parameter:

* `out string`(skapar kömeddelande om parametervärdet är icke-null när funktionen avslutas)
* `out byte[]`(fungerar som `string`)
* `out CloudQueueMessage`(fungerar som `string`)
* `out POCO`(en serialiserbar typ. skapar ett meddelande med ett null-objekt om parametern är null när funktionen avslutas)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(för att skapa meddelanden manuellt med hjälp av Azure Storage-API direkt)

### <a id="ibinder"></a>Använda WebJobs-SDK-attribut i brödtexten för en funktion
Om du behöver göra några resurser i din funktion innan du använder ett WebJobs-SDK-attribut som `Queue`, `Blob`, eller `Table`, du kan använda den `IBinder` gränssnitt.

I följande exempel tar ett inkommande kö-meddelande och skapar ett nytt meddelande med samma innehåll i en utgående kö. Könamnet utdata anges av koden i själva funktionen.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

Den `IBinder` gränssnitt kan även användas med den `Table` och `Blob` attribut.

## <a id="blobs"></a>Hur kan läsa och skriva BLOB och tabeller under bearbetning av ett meddelande i kön
Den `Blob` och `Table` attribut kan du läsa och skriva BLOB och tabeller. Exemplen i det här avsnittet gäller för blobbar. Kodexempel som visar hur du utlöser processer när blobbar har skapats eller uppdaterats, se [använda Azure blob storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), och kodexempel som läsa och skriva tabeller, se [hur du använder Azure-tabellen Storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Sträng Kömeddelanden utlösa blob-åtgärder
För ett meddelande i kön som innehåller en sträng, `queueTrigger` är en platshållare som du kan använda i den `Blob` attributets `blobPath` parametrar som innehåller innehållet i meddelandet.

I följande exempel används `Stream` objekt att läsa och skriva BLOB. Kömeddelandet är namnet på en blob som finns i behållaren textblobs. En kopia av blobben med ”-nya” läggs till namnet skapas i samma behållare.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Den `Blob` attributet konstruktorn tar en `blobPath` parameter som anger namnet på behållaren och blob. Mer information om den här platshållaren finns [använda Azure blob storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),

När attributet decorates en `Stream` objekt, en annan konstruktorparametern anger det `FileAccess` läge som Läs-, Skriv- eller läsning och skrivning.

I följande exempel används en `CloudBlockBlob` objekt att ta bort en blob. Kömeddelandet är namnet på blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) meddelanden i kö
För en POCO lagras som JSON i kön meddelandet, kan du använda platshållare som namn egenskaperna för objektet i den `Queue` attributets `blobPath` parameter. Du kan också använda [kö metadata egenskapsnamn](#queuemetadata) som platshållare.

I följande exempel kopierar en blobb till en ny blob med ett annat tillägg. Kön meddelandet är ett `BlobInformation` -objekt som innehåller `BlobName` och `BlobNameWithoutExtension` egenskaper. Egenskapsnamnen används som platshållare i blobbsökvägen för den `Blob` attribut.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

SDK använder den [Newtonsoft.Json NuGet-paketet](http://www.nuget.org/packages/Newtonsoft.Json) serialisera och deserialisera meddelanden. Om du skapar meddelanden i kö i ett program som inte använder WebJobs SDK kan du skriva kod som i följande exempel för att skapa ett kömeddelande för POCO som SDK kan parsa.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Om du behöver göra vissa arbetet i din funktion innan du binder en blobb till ett objekt kan du använda attributet i brödtexten i funktion, [enligt tidigare för attributet kön](#ibinder).

### <a id="blobattributetypes"></a>Du kan använda Blob-attribut med
Den `Blob` attributet kan användas med följande typer:

* `Stream`(läsa eller skriva anges med hjälp av parametern FileAccess konstruktor)
* `TextReader`
* `TextWriter`
* `string`(Läs)
* `out string`(skriva; skapar en blob endast om strängparametern är icke-null när returnerar funktionen)
* POCO (läsa)
* ut POCO (skriva; alltid skapar en blob, skapar som null-objekt om POCO-parametern är null när returnerar funktionen)
* `CloudBlobStream`(skriva)
* `ICloudBlob`(läsa eller skriva)
* `CloudBlockBlob`(läsa eller skriva)
* `CloudPageBlob`(läsa eller skriva)

## <a id="poison"></a>Hur du hanterar förgiftade meddelanden
Meddelanden vars innehåll gör en funktionen misslyckas kallas *förgiftade meddelanden*. När funktionen misslyckas, tas inte bort kömeddelandet och slutligen hämtas igen och orsakar cykeln ska upprepas. SDK kan avbryta cykeln efter ett begränsat antal upprepningar eller så kan du göra det manuellt.

### <a name="automatic-poison-message-handling"></a>Hantering av automatisk skadligt meddelande
SDK: N ska anropa en funktion upp till 5 gånger för att bearbeta ett meddelande i kön. Om den femte försök misslyckas, flyttas meddelandet till en skadligt kö. [Det maximala antalet nya försök kan konfigureras](#config).

Skadligt kön heter *{originalqueuename}*-skadligt. Du kan skriva en funktion för att bearbeta meddelanden från skadligt kön av loggning av dem eller skicka ett meddelande till den manuella åtgärder krävs.

I följande exempel på `CopyBlob` funktionen kommer att misslyckas när ett kömeddelande innehåller namnet på en blob som inte finns. När det händer kan flyttas meddelandet från kön copyblobqueue till copyblobqueue poison kön. Den `ProcessPoisonMessage` loggar skadligt meddelande.

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
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

Följande bild visar konsolens utdata från de här funktionerna när ett skadligt meddelande bearbetas.

![Konsolens utdata för hantering av skadligt meddelande](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Hantering av manuell skadligt meddelande
Du kan hämta antalet gånger som ett meddelande har plockats för bearbetning genom att lägga till en `int` parameter med namnet `dequeueCount` till funktionen. Du kan kontrollera antalet dequeue i Funktionskoden och utföra egna skadligt meddelandehantering när antalet överskrider ett tröskelvärde som visas i följande exempel.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a id="config"></a>Hur du ställer in konfigurationsalternativ
Du kan använda den `JobHostConfiguration` typen för att ange följande konfigurationsalternativ:

* Ange anslutningssträngar SDK i koden.
* Konfigurera `QueueTrigger` inställningar, till exempel maximalt antal har status Created.
* Hämta könamn från konfigurationen.

### <a id="setconnstr"></a>Ange SDK anslutningssträngar i kod
Ställa in anslutningssträngar SDK i koden kan du använda din egen anslutning sträng namn i konfigurationsfiler eller miljövariabler som visas i följande exempel.

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

### <a id="configqueue"></a>Konfigurera inställningar för QueueTrigger
Du kan konfigurera följande inställningar som gäller för meddelandebehandling kö:

* Det maximala antalet Kömeddelanden som fångas upp samtidigt kan köras parallellt (standard är 16).
* Det maximala antalet försök innan ett kömeddelande skickas till ett skadligt kö (standardvärdet är 5).
* Maximal väntetid innan avsökning igen när en kö är tomt (standard är 1 minut).

I följande exempel visas hur du konfigurerar dessa inställningar:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="setnamesincode"></a>Ange värden för WebJobs SDK konstruktorparametrarna i koden
Du vill ibland ange en ny kö, ett blob-namn eller en behållare eller en tabell i stället för att hårdkoda ge den namnet. Du kan till exempel vill ange namnet på kön för `QueueTrigger` i en konfiguration av fil- eller miljö variabel.

Du kan göra det genom att passera i en `NameResolver` objekt till den `JobHostConfiguration` typen. Du inkludera särskilda platshållare omges av procenttecken (%) i WebJobs SDK attributet konstruktorn parametrar och dina `NameResolver` koden anger de faktiska värdena som ska användas i stället för dessa platshållare.

Anta att du vill använda en kö med namnet logqueuetest i testmiljön och en namngiven logqueueprod i produktion. I stället för en hårdkodad kö du vill ange namnet på en post i den `appSettings` samling som skulle ha faktiska könamnet. Om den `appSettings` nyckeln är logqueue, din funktion kan se ut som följande exempel.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Din `NameResolver` klassen kan sedan hämta könamnet från `appSettings` som visas i följande exempel:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Du skickar den `NameResolver` klassen i att den `JobHost` objekt som visas i följande exempel.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Obs:** kön tabell och blobbnamnen är löst varje gång som en funktion kallas men blob-behållaren namnmatchning bara när programmet startas. Du kan inte ändra namnet för blob-behållare medan jobbet körs.

## <a id="manual"></a>Hur du utlöser en funktion manuellt
Utlös en funktion manuellt genom att använda den `Call` eller `CallAsync` -metoden i den `JobHost` objekt och `NoAutomaticTrigger` attributet för funktionen, som visas i följande exempel.

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

## <a id="logs"></a>Hur du skriver loggar
Instrumentpanelen visar loggar på två platser: sidan för Webbjobbet, och på sidan för ett särskilt Webbjobb-anrop.

![Loggar Webbjobb på sidan](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Loggar i funktionen anrops-sida](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Utdata från konsolen metoder som anropas i en funktion eller i den `Main()` metoden visas på sidan instrumentpanelen för Webbjobbet, inte på sidan för en viss metodanropet. Utdata från TextWriter-objekt som du får från en parameter i Metodsignaturen visas på sidan instrumentpanelen för ett metodanrop.

Konsolens utdata kan inte länkas till en viss metodanropet eftersom konsolen är enkeltrådad, men många jobbfunktioner kan köras samtidigt. Det är därför SDK innehåller varje funktionsanrop med sin egen unika loggen skrivarobjekt.

Att skriva [spårning programloggarna](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), använda `Console.Out` (skapar loggar som är märkta som INFO) och `Console.Error` (skapar loggar som är märkta som fel). Ett alternativ är att använda [spårningen eller TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), som innehåller utförlig, varning, och kritiska nivåer utöver Info och fel. Spårningsloggar för programmet visas i web app loggfilerna Azure-tabeller eller Azure BLOB-objekt beroende på hur du konfigurerar din Azure-webbapp. Som gäller för alla konsolens utdata, visas senaste 100 programloggarna även på sidan instrumentpanelen för Webbjobb, inte sidan för ett funktionsanrop.

Konsolens utdata visas i instrumentpanelen bara om programmet körs i en Azure-Webbjobb inte om programmet körs lokalt eller i en annan miljö.

Inaktivera loggning för instrumentpanelen för scenarier med hög genomströmning. Som standard SDK skriver loggar till lagring och den här aktiviteten kan försämra prestanda vid bearbetning av många meddelanden. Om du vill inaktivera loggning, ange anslutningssträngen instrumentpanelen null som visas i följande exempel.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

I följande exempel visar flera olika sätt att skriva loggar:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

I WebJobs SDK instrumentpanelen för utdata från den `TextWriter` objekt ser ut när du gå till sidan för en viss fungera anrop och klicka på **växla utdata**:

![Klicka på länken för anrop av funktionen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Loggar i funktionen anrops-sida](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

I instrumentpanelen för WebJobs SDK senaste 100 raderna i konsolen utdata visar dig när du gå till sidan för Webbjobb (inte för funktionsanrop) och klicka på **växla utdata**.

![Klicka på Visa/Dölj utdata](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

I ett kontinuerligt Webbjobb programloggarna visas i/data/jobb/kontinuerlig/*{webjobname}*/job_log.txt i filsystemet web app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

I ett Azure blob-program loggar ser ut så här: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, fel, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Och i en Azure-tabellen i `Console.Out` och `Console.Error` loggar ut så här:

![Loggen för information i tabellen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Felloggen i tabellen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Om du vill ansluta din egen loggaren finns [det här exemplet](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Hantera fel och konfigurera tidsgränser
WebJobs SDK innehåller också en [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribut som du kan använda för att en funktion som ska avbrytas om inte slutföra inom en angiven tidsperiod. Och om du vill aktivera en varning när för många fel som inträffar inom en angiven tidsperiod, kan du använda den `ErrorTrigger` attribut. Här är en [ErrorTrigger exempel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Du kan också dynamiskt inaktiverar och aktiverar funktioner för att kontrollera om de kan utlösas, med hjälp av en konfigurationsväxel som kan vara en appinställning eller Miljövariabelns namn. Exempelkod finns i `Disable` attribut i [WebJobs SDK exempel databasen](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a>Nästa steg
Den här guiden tillhandahåller kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure köer. Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).
