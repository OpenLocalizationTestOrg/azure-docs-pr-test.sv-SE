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
# <a name="how-toouse-azure-service-bus-with-hello-webjobs-sdk"></a><span data-ttu-id="6f8f9-103">Hur toouse Azure Service Bus med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="6f8f9-103">How toouse Azure Service Bus with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="6f8f9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6f8f9-104">Overview</span></span>
<span data-ttu-id="6f8f9-105">Den här guiden innehåller C# kod exempel som visar hur tootrigger en process när ett Azure Service Bus-meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-105">This guide provides C# code samples that show how tootrigger a process when an Azure Service Bus message is received.</span></span> <span data-ttu-id="6f8f9-106">hello kod exempel Använd [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-106">hello code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="6f8f9-107">hello handboken förutsätts att du vet [hur toocreate ett Webbjobb projekt i Visual Studio med anslutning strängar som punkt tooyour lagringskonto](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md).</span></span>

<span data-ttu-id="6f8f9-108">hello kodstycken Visa endast funktioner, inte hello kod som skapar hello `JobHost` objekt som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-108">hello code snippets only show functions, not hello code that creates hello `JobHost` object as in this example:</span></span>

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

<span data-ttu-id="6f8f9-109">En [fullständig Service Bus-kodexempel](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) i hello azure-webjobs-sdk-exempel lagringsplats på GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-109">A [complete Service Bus code example](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in hello azure-webjobs-sdk-samples repository on GitHub.com.</span></span>

## <span data-ttu-id="6f8f9-110"><a id="prerequisites"></a>Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="6f8f9-110"><a id="prerequisites"></a> Prerequisites</span></span>
<span data-ttu-id="6f8f9-111">toowork med Service Bus har tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-paketet dessutom toohello andra WebJobs-SDK-paket.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-111">toowork with Service Bus you have tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet package in addition toohello other WebJobs SDK packages.</span></span> 

<span data-ttu-id="6f8f9-112">Du kan också ha tooset hello AzureWebJobsServiceBus anslutningssträngen i tillägg toohello storage-anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-112">You also have tooset hello AzureWebJobsServiceBus connection string in addition toohello storage connection strings.</span></span>  <span data-ttu-id="6f8f9-113">Du kan göra detta i hello `connectionStrings` avsnitt i hello App.config-filen som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-113">You can do this in hello `connectionStrings` section of hello App.config file, as shown in hello following example:</span></span>

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

<span data-ttu-id="6f8f9-114">En exempel-projekt som innehåller hello Service Bus inställningen för anslutningssträngen i hello App.config-fil, se [Service Bus-exempel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-114">For a sample project that includes hello Service Bus connection string setting in hello App.config file, see [Service Bus example](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span></span> 

<span data-ttu-id="6f8f9-115">hello anslutningssträngar kan också anges i hello Azure körningsmiljö, som sedan åsidosätter hello App.config inställningar när hello Webbjobbet körs i Azure; Mer information finns i [Kom igång med hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-115">hello connection strings can also be set in hello Azure runtime environment, which then overrides hello App.config settings when hello WebJob runs in Azure; for more information, see [Get Started with hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span></span>

## <span data-ttu-id="6f8f9-116"><a id="trigger"></a>Hur tootrigger en funktion när en Service Bus-kö meddelandet tas emot</span><span class="sxs-lookup"><span data-stu-id="6f8f9-116"><a id="trigger"></a> How tootrigger a function when a Service Bus queue message is received</span></span>
<span data-ttu-id="6f8f9-117">toowrite anropar en funktion som hello WebJobs SDK när ett kömeddelande tas emot använder hello `ServiceBusTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-117">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello `ServiceBusTrigger` attribute.</span></span> <span data-ttu-id="6f8f9-118">hello Attributkonstruktorn tar en parameter som anger hello kön toopoll hello namn.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-118">hello attribute constructor takes a parameter that specifies hello name of hello queue toopoll.</span></span>

### <a name="how-servicebustrigger-works"></a><span data-ttu-id="6f8f9-119">Så här fungerar ServiceBusTrigger</span><span class="sxs-lookup"><span data-stu-id="6f8f9-119">How ServiceBusTrigger works</span></span>
<span data-ttu-id="6f8f9-120">hello SDK tar emot ett meddelande i `PeekLock` läge och anrop `Complete` på hello-meddelande om hello funktionen slutförs eller samtal `Abandon` om hello funktionen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-120">hello SDK receives a message in `PeekLock` mode and calls `Complete` on hello message if hello function finishes successfully, or calls `Abandon` if hello function fails.</span></span> <span data-ttu-id="6f8f9-121">Om Hej funktionen körs längre än hello `PeekLock` timeout hello Lås förnyas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-121">If hello function runs longer than hello `PeekLock` timeout, hello lock is automatically renewed.</span></span>

<span data-ttu-id="6f8f9-122">Service Bus har sin egen hantering av skadligt kön som inte styrs eller konfigurerats av hello WebJobs-SDK.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-122">Service Bus does its own poison queue handling which cannot be controlled or configured by hello WebJobs SDK.</span></span> 

### <a name="string-queue-message"></a><span data-ttu-id="6f8f9-123">Strängen kömeddelande</span><span class="sxs-lookup"><span data-stu-id="6f8f9-123">String queue message</span></span>
<span data-ttu-id="6f8f9-124">hello läser följande kodexempel ett meddelande i kön som innehåller en sträng och skriver hello sträng toohello WebJobs-SDK-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-124">hello following code sample reads a queue message that contains a string and writes hello string toohello WebJobs SDK dashboard.</span></span>

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

<span data-ttu-id="6f8f9-125">**Obs:** om du skapar hello Kömeddelanden i ett program som inte använder hello WebJobs SDK, se till att tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) för ”text/plain”.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-125">**Note:** If you are creating hello queue messages in an application that doesn't use hello WebJobs SDK, make sure tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) too"text/plain".</span></span>

### <a name="poco-queue-message"></a><span data-ttu-id="6f8f9-126">POCO kömeddelande</span><span class="sxs-lookup"><span data-stu-id="6f8f9-126">POCO queue message</span></span>
<span data-ttu-id="6f8f9-127">hello SDK kommer automatiskt att deserialisera ett kömeddelande som innehåller JSON för en POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typen.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-127">hello SDK will automatically deserialize a queue message that contains JSON for a POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type.</span></span> <span data-ttu-id="6f8f9-128">hello följande kodexempel läser ett meddelande i kön som innehåller en `BlobInformation` objekt som har en `BlobName` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-128">hello following code sample reads a queue message that contains a `BlobInformation` object which has a `BlobName` property:</span></span>

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="6f8f9-129">Kodexempel visar hur hello toouse egenskaper för hello POCO toowork med blobbar och tabeller i samma fungerar finns i avsnittet hello [lagring köer versionen av den här artikeln](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-129">For code samples showing how toouse properties of hello POCO toowork with blobs and tables in hello same function, see hello [storage queues version of this article](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span></span>

<span data-ttu-id="6f8f9-130">Om koden som skapar kön hälsningsmeddelande inte använda hello WebJobs SDK Använd kod liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-130">If your code that creates hello queue message doesn't use hello WebJobs SDK, use code similar toohello following example:</span></span>

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a><span data-ttu-id="6f8f9-131">Typer ServiceBusTrigger fungerar med</span><span class="sxs-lookup"><span data-stu-id="6f8f9-131">Types ServiceBusTrigger works with</span></span>
<span data-ttu-id="6f8f9-132">Förutom `string` POCO typerna och du kan använda hello `ServiceBusTrigger` attribut med en bytematris eller en `BrokeredMessage` objektet.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-132">Besides `string` and POCO  types, you can use hello `ServiceBusTrigger` attribute with a byte array or a `BrokeredMessage` object.</span></span>

## <span data-ttu-id="6f8f9-133"><a id="create"></a>Hur toocreate Service Bus kö meddelanden</span><span class="sxs-lookup"><span data-stu-id="6f8f9-133"><a id="create"></a> How toocreate Service Bus queue messages</span></span>
<span data-ttu-id="6f8f9-134">toowrite en funktion som skapar ett nytt meddelande i kön använder hello `ServiceBus` attributet och skicka in hello kön namn toohello Attributkonstruktorn.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-134">toowrite a function that creates a new queue message use hello `ServiceBus` attribute and pass in hello queue name toohello attribute constructor.</span></span> 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a><span data-ttu-id="6f8f9-135">Skapa ett enda kömeddelande i en icke-async-funktion</span><span class="sxs-lookup"><span data-stu-id="6f8f9-135">Create a single queue message in a non-async function</span></span>
<span data-ttu-id="6f8f9-136">hello följande kodexempel används en toocreate för utdata-parametern som ett nytt meddelande i hello kö med namnet ”outputqueue” med samma innehåll som hello meddelande togs emot i hello kö med namnet ”inputqueue” Hej.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-136">hello following code sample uses an output parameter toocreate a new message in hello queue named "outputqueue" with hello same content as hello message received in hello queue named "inputqueue".</span></span>

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

<span data-ttu-id="6f8f9-137">hello output-parameter för att skapa ett enda kömeddelande kan vara något av följande typer hello:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-137">hello output parameter for creating a single queue message can be any of hello following types:</span></span>

* `string`
* `byte[]`
* `BrokeredMessage`
* <span data-ttu-id="6f8f9-138">En serialiserbar POCO-typ som du definierar.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-138">A serializable POCO type that you define.</span></span> <span data-ttu-id="6f8f9-139">Serialiserad automatiskt som JSON.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-139">Automatically serialized as JSON.</span></span>

<span data-ttu-id="6f8f9-140">För POCO typparametrar skapas ett kömeddelande alltid när hello funktionen avslutas; Om hello-parametern är null, skapar ett meddelande i kön som kommer att returnera null när hello-meddelande tas emot och avserialiseras hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-140">For POCO type parameters, a queue message is always created when hello function ends; if hello parameter is null, hello SDK creates a queue message that will return null when hello message is received and deserialized.</span></span> <span data-ttu-id="6f8f9-141">Hej för andra typer, inget kömeddelande skapas om hello-parametern är null.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-141">For hello other types, if hello parameter is null no queue message is created.</span></span>

### <a name="create-multiple-queue-messages-or-in-async-functions"></a><span data-ttu-id="6f8f9-142">Skapa flera Kömeddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="6f8f9-142">Create multiple queue messages or in async functions</span></span>
<span data-ttu-id="6f8f9-143">toocreate flera meddelanden använder hello `ServiceBus` attributet med `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-143">toocreate multiple messages, use  hello `ServiceBus` attribute with `ICollector<T>` or `IAsyncCollector<T>`, as shown in hello following code sample:</span></span>

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="6f8f9-144">Varje meddelande i kön skapas omedelbart när hello `Add` metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-144">Each queue message is created immediately when hello `Add` method is called.</span></span>

## <span data-ttu-id="6f8f9-145"><a id="topics"></a>Hur toowork med Service Bus-ämnen</span><span class="sxs-lookup"><span data-stu-id="6f8f9-145"><a id="topics"></a>How toowork with Service Bus topics</span></span>
<span data-ttu-id="6f8f9-146">toowrite anropar en funktion som hello SDK när ett meddelande tas emot på Service Bus-ämne använder hello `ServiceBusTrigger` attribut med hello-konstruktor som tar ämnet och prenumerationsnamn, som visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-146">toowrite a function that hello SDK calls when a message is received on a Service Bus topic, use hello `ServiceBusTrigger` attribute with hello constructor that takes topic name and subscription name, as shown in hello following code sample:</span></span>

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

<span data-ttu-id="6f8f9-147">toocreate ett meddelande om ett ämne, Använd hello `ServiceBus` attribut med en avsnittet namn hello samma sätt som du använder det med ett namn för kön.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-147">toocreate a message on a topic, use hello `ServiceBus` attribute with a topic name hello same way you use it with a queue name.</span></span>

## <a name="features-added-in-release-11"></a><span data-ttu-id="6f8f9-148">Funktioner i version 1.1</span><span class="sxs-lookup"><span data-stu-id="6f8f9-148">Features added in release 1.1</span></span>
<span data-ttu-id="6f8f9-149">hello följande funktioner har lagts till i version 1.1:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-149">hello following features were added in release 1.1:</span></span>

* <span data-ttu-id="6f8f9-150">Tillåt djup anpassning av meddelandebehandling `ServiceBusConfiguration.MessagingProvider`.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-150">Allow deep customization of message processing via `ServiceBusConfiguration.MessagingProvider`.</span></span>
* <span data-ttu-id="6f8f9-151">`MessagingProvider`har stöd för anpassning av hello Service Bus `MessagingFactory` och `NamespaceManager`.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-151">`MessagingProvider` supports customization of hello Service Bus `MessagingFactory` and `NamespaceManager`.</span></span>
* <span data-ttu-id="6f8f9-152">En `MessageProcessor` strategimönster kan du toospecify en processor per kö/avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-152">A `MessageProcessor` strategy pattern allows you toospecify a processor per queue/topic.</span></span>
* <span data-ttu-id="6f8f9-153">Meddelandet bearbetning samtidighet stöds som standard.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-153">Message processing concurrency is supported by default.</span></span> 
* <span data-ttu-id="6f8f9-154">Enkelt anpassning av `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-154">Easy customization of `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span></span>
* <span data-ttu-id="6f8f9-155">Tillåt [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe som angetts i `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (för scenarier där du kan inte hantera rättigheterna).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-155">Allow [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe specified on `ServiceBusTriggerAttribute`/`ServiceBusAttribute` (for scenarios where you might not have Manage rights).</span></span> <span data-ttu-id="6f8f9-156">Observera att Azure WebJobs är tooautomatically etablera obefintlig köer och ämnen utan att hantera AccessRights.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-156">Note that Azure WebJobs is unable tooautomatically provision non-existent queues and topics without Manage AccessRights.</span></span>

## <span data-ttu-id="6f8f9-157"><a id="queues"></a>Närliggande ämnen som omfattas av hello lagring köer hur-tooarticle</span><span class="sxs-lookup"><span data-stu-id="6f8f9-157"><a id="queues"></a>Related topics covered by hello storage queues how-tooarticle</span></span>
<span data-ttu-id="6f8f9-158">Information om WebJobs SDK scenarier inte specifika tooService Bus, finns [hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-158">For information about WebJobs SDK scenarios not specific tooService Bus, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="6f8f9-159">Avsnitt som beskrivs i artikeln inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="6f8f9-159">Topics covered in that article include hello following:</span></span>

* <span data-ttu-id="6f8f9-160">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="6f8f9-160">Async functions</span></span>
* <span data-ttu-id="6f8f9-161">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="6f8f9-161">Multiple instances</span></span>
* <span data-ttu-id="6f8f9-162">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="6f8f9-162">Graceful shutdown</span></span>
* <span data-ttu-id="6f8f9-163">Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="6f8f9-163">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="6f8f9-164">Ange hello SDK-anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="6f8f9-164">Set hello SDK connection strings in code</span></span>
* <span data-ttu-id="6f8f9-165">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="6f8f9-165">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="6f8f9-166">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="6f8f9-166">Trigger a function manually</span></span>
* <span data-ttu-id="6f8f9-167">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="6f8f9-167">Write logs</span></span>

## <span data-ttu-id="6f8f9-168"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f8f9-168"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="6f8f9-169">Den här guiden har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6f8f9-169">This guide has provided code samples that show how toohandle common scenarios for working with Azure Service Bus.</span></span> <span data-ttu-id="6f8f9-170">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="6f8f9-170">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

