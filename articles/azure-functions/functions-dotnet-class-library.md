---
title: aaaUsing .NET klassen bibliotek med Azure Functions | Microsoft Docs
description: "Lär dig hur tooauthor .NET-klassbibliotek för hjälp med Azure Functions"
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelsebearbetning, dynamiska beräkning serverlösa arkitektur"
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/09/2017
ms.author: donnam
ms.openlocfilehash: 4e0fd954b554006ba1d8ecc47403a9fb1c67c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a>Med hjälp av .NET-klassbibliotek med Azure Functions

Dessutom stöder tooscript filer, Azure Functions publicerar en klassbiblioteket som hello implementering för en eller flera funktioner. Vi rekommenderar att du använder hello [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).

## <a name="prerequisites"></a>Krav 

Den här artikeln har hello följande krav:

- [Visual Studio 2017 15,3 Preview](https://www.visualstudio.com/vs/preview/). Installera hello arbetsbelastningar **ASP.NET och web development** och **Azure-utveckling**.
- [Azure funktionen Verktyg för Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a>Funktioner för klassbiblioteksprojektet

Skapa ett nytt Azure Functions-projekt från Visual Studio. hello ny projektmall skapar hello filer *host.json* och *local.settings.json*. Du kan [Anpassa inställningar för Azure Functions-runtime i host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json). 

hello filen *local.settings.json* lagrar app-inställningar, anslutningssträngar och inställningar för Azure Functions Core verktyg. toolearn mer om dess struktur finns [koden och testa Azure functions lokalt](functions-run-local.md#local-settings).

### <a name="functionname-attribute"></a>FunctionName attribut

hello attributet [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) markerar en metod som en startpunkt för funktionen. Den måste användas med en utlösare och 0 eller fler indata och utdata bindningar.

### <a name="conversion-toofunctionjson"></a>Konvertering toofunction.json

När du har skapat ett Azure Functions-projekt, ger en fil `function.json` i hello directory matchar hello funktionsnamn definieras av `[FunctionName]`. Det anger utlösare och bindningar och punkter toohello projektfilen sammansättningen.

Den här konverteringen utförs av hello NuGet-paketet [Microsoft\.NET\.Sdk\.funktioner](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). hello källan är tillgänglig i GitHub-repo-hello [azure\-funktioner\-vs\-skapa\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="triggers-and-bindings"></a>Utlösare och bindningar

hello följande tabell visas hello utlösare och bindningar som är tillgängliga i en Azure Functions klassbiblioteksprojektet. Alla attribut är i hello namnområdet `Microsoft.Azure.WebJobs`.

| Bindning | Attribut | NuGet-paketet |
|------   | ------    | ------        |
| [BLOB storage-utlösare, indata, utdata](#blob-storage) | [BlobAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | [Blobblagring] |
| [Cosmos DB indata och utdata bindning](#cosmos-db) | [DocumentDBAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] | 
| [Händelseutlösare NAV- och utdata](#event-hub) | [EventHubTriggerAttribute], [EventHubAttribute] | [Microsoft.Azure.WebJobs.ServiceBus] |
| [Den externa filen inkommande och utgående](#api-hub) | [ApiHubFileAttribute] | [Microsoft.Azure.WebJobs.Extensions.ApiHub] |
| [HTTP- och webhook-utlösare](#http) | [HttpTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.Http] |
| [Mobile Apps inkommande och utgående](#mobile-apps) | [MobileTableAttribute] | [Microsoft.Azure.WebJobs.Extensions.MobileApps] | 
| [Notification Hubs utdata](#nh) | [NotificationHubAttribute] | [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] | 
| [Queue storage utlösare och utdata](#queue) | [QueueAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [SendGrid-utdata](#sendgrid) | [SendGridAttribute] | [Microsoft.Azure.WebJobs.Extensions.SendGrid] | 
| [Service Bus-utlösare och utdata](#service-bus) | [ServiceBusAttribute], [ServiceBusAccountAttribute] | [Microsoft.Azure.WebJobs.ServiceBus]
| [Table storage inkommande och utgående](#table) | [TableAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Timerutlösare](#timer) | [TimerTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions] | 
| [Twilio-utdata](#twilio) | [TwilioSmsAttribute] | [Microsoft.Azure.WebJobs.Extensions.Twilio] | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a>BLOB storage utlösaren indata och utdata bindningar

Azure Functions stöder utlösaren indata och utdata bindningar för Azure Blob storage. Mer information om bindande uttryck och metadata finns [Azure Functions Blob storage bindningar](functions-bindings-storage-blob.md).

En blob-utlösare har definierats med hello `[BlobTrigger]` attribut. Du kan använda hello attributet `[StorageAccount]` toodefine hello storage-konto som används av ett hela funktion eller en klass.

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

BLOB som indata och utdata har definierats med hello `[Blob]` attributet tillsammans med en `FileAccess` parameter som anger att läsa eller skriva. Hej följande exempel används en blob-utlösare och blob utdata bindning.

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-input-and-output-bindings"></a>Cosmos DB indata och utdata bindningar

Azure Functions stöder indata och utdata bindningar för Cosmos DB. toolearn mer om hello funktioner för hello Cosmos DB bindning finns [Azure Functions Cosmos DB bindningar](functions-bindings-documentdb.md).

toobind tooa Cosmos-DB-dokumentet använder hello attributet `[DocumentDB]` i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. hello följande exempel har en kö-utlösare och ett DocumentDB-API-utdatabindning:

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", ConnectionStringSetting = "DocDBConnection")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a>Händelseutlösare NAV- och utdata

Azure Functions stöder utlösa och utgående bindningar för Händelsehubbar. Mer information finns i [Azure Functions Event Hub bindningar](functions-bindings-event-hubs.md).

Hej typer `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` och `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.ServiceBus]. 

hello används följande exempel en Event Hub-utlösare:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

hello har följande exempel en Händelsehubb utdata, med hjälp av hello metoden returnerade värdet som hello utdata:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a>Den externa filen inkommande och utgående

Azure Functions stöder utlösaren indata och utdata bindningar för externa filer, till exempel Google Drive, Dropbox och OneDrive. Det finns fler toolearn [Azure Functions extern fil bindningar](functions-bindings-external-file.md). Hej attribut `[ExternalFileTrigger]` och `[ExternalFile]` definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.ApiHub].

hello följande C#-exempel visar en extern fil indata och utdata bindning. hello kod kopior hello indatafilen toohello utdatafilen.

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a>HTTP och webhooks

Använd hello `HttpTrigger` attributet toodefine en HTTP-utlösare eller webhooken. Det här attributet definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.Http]. Du kan anpassa hello åtkomstnivå, webhook-typ, väg och metoder. hello följande exempel definierar en HTTP-utlösare med anonym autentisering och _genericJson_ webhook-typen.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a>Mobile Apps inkommande och utgående

Azure Functions stöder indata och utdata bindningar för Mobile Apps. Det finns fler toolearn [Azure Functions Mobile Apps bindningar](functions-bindings-mobile-apps.md). hello attributet `[MobileTable]` har definierats i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.MobileApps].

hello exemplet nedan visar en Mobile Apps-utdatabindning:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a>Notification Hubs utdata

Azure Functions stöder en output-bindning för Notification Hubs. Det finns fler toolearn [Azure Functions Notification Hub-utdatabindning](functions-bindings-notification-hubs.md). hello attributet `[NotificationHub]` har definierats i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a>Queue storage utlösare och utdata

Azure Functions stöder utlösa och utgående bindningar för Azure köer. Mer information finns i [Azure Functions Queue Storage bindningar](functions-bindings-storage-queue.md).

hello följande exempel visas hur toouse hello funktionen returtyp med en kö utdata bindning, med hello `[Queue]` attribut. toodefine en kö-utlösare, Använd hello `[QueueTrigger]` attribut.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }

    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```

<a name="sendgrid"></a>

### <a name="sendgrid-output"></a>SendGrid-utdata

Azure Functions stöder en SendGrid utdata bindning för att skicka e-post via programmering. Det finns fler toolearn [Azure Functions SendGrid bindningar](functions-bindings-sendgrid.md).

hello attributet `[SendGrid]` har definierats i hello NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.SendGrid].

hello följande är ett exempel på med en utlösare för Service Bus-kö och en SendGrid utdata bindning med `SendGridMessage`:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string too{ get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a>Service Bus-utlösare och utdata

Azure Functions stöder utlösa och utgående bindningar för Service Bus-köer och ämnen. Mer information om hur du konfigurerar bindningar finns [Azure Functions Service Bus-bindningar](functions-bindings-service-bus.md).

Hej attribut `[ServiceBusTrigger]` och `[ServiceBus]` definieras i hello NuGet-paketet [Microsoft.Azure.WebJobs.ServiceBus]. 

hello följande är ett exempel på en utlösare för Service Bus-kö:

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

hello följande är ett exempel på en Service Bus-utdata bindning, med hello-metodens returtyp som hello utdata:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a>Table storage inkommande och utgående

Azure Functions stöder indata och utdata bindningar för Azure Table storage. Det finns fler toolearn [Azure Functions Table storage bindningar](functions-bindings-storage-table.md).

hello är följande exempel en klass med två funktioner som visar tabellen lagring utgående och inkommande bindningar. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use hello metadata parameter "queueTrigger" toobind hello queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a>Timerutlösare

Azure Functions har en timer utlösaren bindning som låter dig köra din funktionskod baserat på ett definierat schema. toolearn mer om hello funktioner för hello bindning finns [schemalägga kodkörning med Azure Functions](functions-bindings-timer.md).

Hello förbrukning planen kan du definiera scheman med en [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression). Om du använder en App Service-Plan kan använda du också en TimeSpan-sträng. 

hello följande exempel definierar en timer som utlösare som körs var femte minut:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a>Twilio-utdata

Azure Functions stöder Twilio utdata bindningar tooenable funktioner för toosend SMS textmeddelanden. Det finns fler toolearn [skicka SMS-meddelanden från Azure-funktioner med hjälp av hello Twilio-utdatabindning](functions-bindings-twilio.md). 

hello attributet `[TwilioSms]` har definierats i hello paketet [Microsoft.Azure.WebJobs.Extensions.Twilio].

hello följande C# exempel används en kö utlösare och en Twilio-utdatabindning:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        too= order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder Azure Functions i C#-skript finns [Azure Functions C\# skript för utvecklare](functions-reference-csharp.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links toosource --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs
