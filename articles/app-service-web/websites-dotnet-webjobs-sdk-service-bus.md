---
title: aaaHow toouse Azure Service Bus med hello WebJobs SDK
description: "Lär dig hur toouse Azure Service Bus-köer och ämnen med hello WebJobs-SDK."
services: app-service\web, service-bus
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 2114a934-135b-42b8-871c-6cc040214e76
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: cb801a9320a20c276da4f48c8941c09d3f09bb1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-service-bus-with-hello-webjobs-sdk"></a>Hur toouse Azure Service Bus med hello WebJobs SDK
## <a name="overview"></a>Översikt
Den här guiden innehåller C# kod exempel som visar hur tootrigger en process när ett Azure Service Bus-meddelande tas emot. hello kod exempel Använd [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.

hello handboken förutsätts att du vet [hur toocreate ett Webbjobb projekt i Visual Studio med anslutning strängar som punkt tooyour lagringskonto](websites-dotnet-webjobs-sdk-get-started.md).

hello kodstycken Visa endast funktioner, inte hello kod som skapar hello `JobHost` objekt som i följande exempel:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

En [fullständig Service Bus-kodexempel](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) i hello azure-webjobs-sdk-exempel lagringsplats på GitHub.com.

## <a id="prerequisites"></a>Nödvändiga komponenter
toowork med Service Bus har tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-paketet dessutom toohello andra WebJobs-SDK-paket. 

Du kan också ha tooset hello AzureWebJobsServiceBus anslutningssträngen i tillägg toohello storage-anslutningssträngar.  Du kan göra detta i hello `connectionStrings` avsnitt i hello App.config-filen som visas i följande exempel hello:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

En exempel-projekt som innehåller hello Service Bus inställningen för anslutningssträngen i hello App.config-fil, se [Service Bus-exempel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

hello anslutningssträngar kan också anges i hello Azure körningsmiljö, som sedan åsidosätter hello App.config inställningar när hello Webbjobbet körs i Azure; Mer information finns i [Kom igång med hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Hur tootrigger en funktion när en Service Bus-kö meddelandet tas emot
toowrite anropar en funktion som hello WebJobs SDK när ett kömeddelande tas emot använder hello `ServiceBusTrigger` attribut. hello Attributkonstruktorn tar en parameter som anger hello kön toopoll hello namn.

### <a name="how-servicebustrigger-works"></a>Så här fungerar ServiceBusTrigger
hello SDK tar emot ett meddelande i `PeekLock` läge och anrop `Complete` på hello-meddelande om hello funktionen slutförs eller samtal `Abandon` om hello funktionen misslyckas. Om Hej funktionen körs längre än hello `PeekLock` timeout hello Lås förnyas automatiskt.

Service Bus har sin egen hantering av skadligt kön som inte styrs eller konfigurerats av hello WebJobs-SDK. 

### <a name="string-queue-message"></a>Strängen kömeddelande
hello läser följande kodexempel ett meddelande i kön som innehåller en sträng och skriver hello sträng toohello WebJobs-SDK-instrumentpanelen.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Obs:** om du skapar hello Kömeddelanden i ett program som inte använder hello WebJobs SDK, se till att tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) för ”text/plain”.

### <a name="poco-queue-message"></a>POCO kömeddelande
hello SDK kommer automatiskt att deserialisera ett kömeddelande som innehåller JSON för en POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typen. hello följande kodexempel läser ett meddelande i kön som innehåller en `BlobInformation` objekt som har en `BlobName` egenskapen:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

Kodexempel visar hur hello toouse egenskaper för hello POCO toowork med blobbar och tabeller i samma fungerar finns i avsnittet hello [lagring köer versionen av den här artikeln](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Om koden som skapar kön hälsningsmeddelande inte använda hello WebJobs SDK Använd kod liknande toohello följande exempel:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typer ServiceBusTrigger fungerar med
Förutom `string` POCO typerna och du kan använda hello `ServiceBusTrigger` attribut med en bytematris eller en `BrokeredMessage` objektet.

## <a id="create"></a>Hur toocreate Service Bus kö meddelanden
toowrite en funktion som skapar ett nytt meddelande i kön använder hello `ServiceBus` attributet och skicka in hello kön namn toohello Attributkonstruktorn. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Skapa ett enda kömeddelande i en icke-async-funktion
hello följande kodexempel används en toocreate för utdata-parametern som ett nytt meddelande i hello kö med namnet ”outputqueue” med samma innehåll som hello meddelande togs emot i hello kö med namnet ”inputqueue” Hej.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

hello output-parameter för att skapa ett enda kömeddelande kan vara något av följande typer hello:

* `string`
* `byte[]`
* `BrokeredMessage`
* En serialiserbar POCO-typ som du definierar. Serialiserad automatiskt som JSON.

För POCO typparametrar skapas ett kömeddelande alltid när hello funktionen avslutas; Om hello-parametern är null, skapar ett meddelande i kön som kommer att returnera null när hello-meddelande tas emot och avserialiseras hello SDK. Hej för andra typer, inget kömeddelande skapas om hello-parametern är null.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Skapa flera Kömeddelanden eller i async-funktioner
toocreate flera meddelanden använder hello `ServiceBus` attributet med `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande kodexempel hello:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Varje meddelande i kön skapas omedelbart när hello `Add` metoden anropas.

## <a id="topics"></a>Hur toowork med Service Bus-ämnen
toowrite anropar en funktion som hello SDK när ett meddelande tas emot på Service Bus-ämne använder hello `ServiceBusTrigger` attribut med hello-konstruktor som tar ämnet och prenumerationsnamn, som visas i följande kodexempel hello:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

toocreate ett meddelande om ett ämne, Använd hello `ServiceBus` attribut med en avsnittet namn hello samma sätt som du använder det med ett namn för kön.

## <a name="features-added-in-release-11"></a>Funktioner i version 1.1
hello följande funktioner har lagts till i version 1.1:

* Tillåt djup anpassning av meddelandebehandling `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`har stöd för anpassning av hello Service Bus `MessagingFactory` och `NamespaceManager`.
* En `MessageProcessor` strategimönster kan du toospecify en processor per kö/avsnittet.
* Meddelandet bearbetning samtidighet stöds som standard. 
* Enkelt anpassning av `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.
* Tillåt [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe som angetts i `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (för scenarier där du kan inte hantera rättigheterna). Observera att Azure WebJobs är tooautomatically etablera obefintlig köer och ämnen utan att hantera AccessRights.

## <a id="queues"></a>Närliggande ämnen som omfattas av hello lagring köer hur-tooarticle
Information om WebJobs SDK scenarier inte specifika tooService Bus, finns [hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Avsnitt som beskrivs i artikeln inkludera hello följande:

* Async-funktion
* Flera instanser
* Korrekt avslutning
* Använda WebJobs-SDK-attribut i hello brödtext
* Ange hello SDK-anslutningssträngar i kod
* Ange värden för WebJobs SDK konstruktorparametrarna i koden
* Utlös en funktion manuellt
* Skriva loggar

## <a id="nextsteps"></a>Nästa steg
Den här guiden har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure Service Bus. Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).

