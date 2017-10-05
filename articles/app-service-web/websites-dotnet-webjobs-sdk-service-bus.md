---
title: "Använd Azure Service Bus med WebJobs SDK:n"
description: "Lär dig hur du använder Azure Service Bus-köer och ämnen med WebJobs SDK."
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
ms.openlocfilehash: 7cec03cae5d20d1ead9eb24e99415c33d8b76f05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Använd Azure Service Bus med WebJobs SDK:n
## <a name="overview"></a>Översikt
Den här guiden innehåller C#-kodexempel som visar hur du utlöser en process när ett Azure Service Bus-meddelande tas emot. Koden exempel Använd [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.

Handboken förutsätter att du vet [hur du skapar ett Webbjobb-projekt i Visual Studio med anslutning strängar som pekar på ditt lagringskonto](websites-dotnet-webjobs-sdk-get-started.md).

Kodavsnitten Visa endast funktioner, inte koden som skapar den `JobHost` objekt som i följande exempel:

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

En [fullständig Service Bus-kodexempel](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) är lagringsplatsen för azure-webjobs-sdk-exempel på GitHub.com.

## <a id="prerequisites"></a>Nödvändiga komponenter
Arbeta med Service Bus som du måste installera den [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-paketet utöver de andra paketen i WebJobs-SDK. 

Du måste också ange anslutningssträngen AzureWebJobsServiceBus förutom storage-anslutningssträngar.  Du kan göra detta den `connectionStrings` avsnitt i filen App.config som visas i följande exempel:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

En exempelprojektet som innehåller inställningen för anslutningssträngen i Service Bus i App.config-filen finns [Service Bus-exempel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Anslutningssträngar kan också anges i Azure körningsmiljön, som sedan åsidosätter App.config inställningarna när Webbjobbet körs i Azure; Mer information finns i [Kom igång med WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Hur du utlöser en funktion när en Service Bus-kö meddelande tas emot
Om du vill skriva en funktion som WebJobs SDK anropar när ett kömeddelande tas emot, använder den `ServiceBusTrigger` attribut. Attributkonstruktorn tar en parameter som anger namnet på kön avsöker.

### <a name="how-servicebustrigger-works"></a>Så här fungerar ServiceBusTrigger
SDK: N får ett meddelande i `PeekLock` läge och anrop `Complete` på meddelandet om funktionen slutförs eller samtal `Abandon` om misslyckas åtgärden. Om funktionen körs längre än den `PeekLock` timeout låset förnyas automatiskt.

Service Bus har sin egen hantering av skadligt kön som inte styrs eller konfigurerats av WebJobs-SDK. 

### <a name="string-queue-message"></a>Strängen kömeddelande
Följande kodexempel läser ett meddelande i kön som innehåller en sträng och skriver strängen till WebJobs-SDK-instrumentpanelen.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Obs:** om du skapar Kömeddelanden i ett program som inte använder WebJobs SDK, se till att ange [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) till ”text/plain”.

### <a name="poco-queue-message"></a>POCO kömeddelande
SDK: N kommer automatiskt att deserialisera ett kömeddelande som innehåller JSON för en POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typen. Följande kodexempel läser ett meddelande i kön som innehåller en `BlobInformation` objekt som har en `BlobName` egenskapen:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Kodexempel visar hur du använder egenskaper för POCO för att arbeta med blobbar och tabeller i samma funktion, finns det [lagring köer versionen av den här artikeln](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Om din kod som skapar kömeddelandet inte använda WebJobs-SDK använder du koden som liknar följande exempel:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typer ServiceBusTrigger fungerar med
Förutom `string` och POCO typer som du kan använda den `ServiceBusTrigger` attribut med en bytematris eller en `BrokeredMessage` objektet.

## <a id="create"></a>Så här skapar du meddelanden i Service Bus-kö
Att skriva en funktion som skapar en ny kö meddelandet använder den `ServiceBus` attributet och skicka in könamnet till Attributkonstruktorn. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Skapa ett enda kömeddelande i en icke-async-funktion
Följande kodexempel används en output-parameter för att skapa ett nytt meddelande i kön med namnet ”outputqueue” med samma innehåll som meddelandet som togs emot i kön med namnet ”inputqueue”.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Output-parameter för att skapa ett enda kömeddelande kan vara något av följande typer:

* `string`
* `byte[]`
* `BrokeredMessage`
* En serialiserbar POCO-typ som du definierar. Serialiserad automatiskt som JSON.

För POCO typparametrar skapas ett kömeddelande alltid när funktionen avslutas; Om parametern är null, skapar ett meddelande i kön som kommer att returnera null när meddelandet tas emot och avserialiseras SDK. För andra typer skapas inget kömeddelande om parametern är null.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Skapa flera Kömeddelanden eller i async-funktioner
Du kan skapa flera meddelanden med den `ServiceBus` attributet med `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande kodexempel:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Varje meddelande i kön skapas omedelbart när den `Add` metoden anropas.

## <a id="topics"></a>Hur du arbetar med Service Bus-ämnen
Om du vill skriva en funktion som SDK anropar när ett meddelande tas emot på Service Bus-ämne, använda den `ServiceBusTrigger` attribut med konstruktorn som tar ämnet och prenumerationsnamn, som visas i följande kodexempel:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Så här skapar du ett meddelande om ett ämne på `ServiceBus` attribut med ett namn för ämnet på samma sätt som du kan använda den med en kö.

## <a name="features-added-in-release-11"></a>Funktioner i version 1.1
Följande funktioner har lagts till i version 1.1:

* Tillåt djup anpassning av meddelandebehandling `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`har stöd för anpassning av Service Bus `MessagingFactory` och `NamespaceManager`.
* En `MessageProcessor` strategimönster kan du ange en processor per kö/avsnittet.
* Meddelandet bearbetning samtidighet stöds som standard. 
* Enkelt anpassning av `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.
* Tillåt [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) anges på `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (för scenarier där du kan inte hantera rättigheterna). Observera att det går inte att automatiskt etablera obefintlig köer och ämnen utan att hantera AccessRights Azure WebJobs.

## <a id="queues"></a>Närliggande ämnen som omfattas av lagring köer anvisningar artikel
Information om WebJobs-SDK-scenarier som inte är specifika för Service Bus finns [använda Azure queue storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Följande: avsnitt beskrivs i artikeln

* Async-funktion
* Flera instanser
* Korrekt avslutning
* Använda WebJobs-SDK-attribut i brödtexten för en funktion
* Ange anslutningssträngar SDK i kod
* Ange värden för WebJobs SDK konstruktorparametrarna i koden
* Utlös en funktion manuellt
* Skriva loggar

## <a id="nextsteps"></a>Nästa steg
Den här guiden tillhandahåller kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure Service Bus. Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).

