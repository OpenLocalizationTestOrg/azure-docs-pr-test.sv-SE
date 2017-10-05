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
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a><span data-ttu-id="dd938-103">Använd Azure Service Bus med WebJobs SDK:n</span><span class="sxs-lookup"><span data-stu-id="dd938-103">How to use Azure Service Bus with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="dd938-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dd938-104">Overview</span></span>
<span data-ttu-id="dd938-105">Den här guiden innehåller C#-kodexempel som visar hur du utlöser en process när ett Azure Service Bus-meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="dd938-105">This guide provides C# code samples that show how to trigger a process when an Azure Service Bus message is received.</span></span> <span data-ttu-id="dd938-106">Koden exempel Använd [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="dd938-106">The code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="dd938-107">Handboken förutsätter att du vet [hur du skapar ett Webbjobb-projekt i Visual Studio med anslutning strängar som pekar på ditt lagringskonto](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="dd938-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md).</span></span>

<span data-ttu-id="dd938-108">Kodavsnitten Visa endast funktioner, inte koden som skapar den `JobHost` objekt som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dd938-108">The code snippets only show functions, not the code that creates the `JobHost` object as in this example:</span></span>

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

<span data-ttu-id="dd938-109">En [fullständig Service Bus-kodexempel](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) är lagringsplatsen för azure-webjobs-sdk-exempel på GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="dd938-109">A [complete Service Bus code example](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in the azure-webjobs-sdk-samples repository on GitHub.com.</span></span>

## <span data-ttu-id="dd938-110"><a id="prerequisites"></a>Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="dd938-110"><a id="prerequisites"></a> Prerequisites</span></span>
<span data-ttu-id="dd938-111">Arbeta med Service Bus som du måste installera den [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-paketet utöver de andra paketen i WebJobs-SDK.</span><span class="sxs-lookup"><span data-stu-id="dd938-111">To work with Service Bus you have to install the [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet package in addition to the other WebJobs SDK packages.</span></span> 

<span data-ttu-id="dd938-112">Du måste också ange anslutningssträngen AzureWebJobsServiceBus förutom storage-anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="dd938-112">You also have to set the AzureWebJobsServiceBus connection string in addition to the storage connection strings.</span></span>  <span data-ttu-id="dd938-113">Du kan göra detta den `connectionStrings` avsnitt i filen App.config som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dd938-113">You can do this in the `connectionStrings` section of the App.config file, as shown in the following example:</span></span>

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

<span data-ttu-id="dd938-114">En exempelprojektet som innehåller inställningen för anslutningssträngen i Service Bus i App.config-filen finns [Service Bus-exempel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="dd938-114">For a sample project that includes the Service Bus connection string setting in the App.config file, see [Service Bus example](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span></span> 

<span data-ttu-id="dd938-115">Anslutningssträngar kan också anges i Azure körningsmiljön, som sedan åsidosätter App.config inställningarna när Webbjobbet körs i Azure; Mer information finns i [Kom igång med WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span><span class="sxs-lookup"><span data-stu-id="dd938-115">The connection strings can also be set in the Azure runtime environment, which then overrides the App.config settings when the WebJob runs in Azure; for more information, see [Get Started with the WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span></span>

## <span data-ttu-id="dd938-116"><a id="trigger"></a>Hur du utlöser en funktion när en Service Bus-kö meddelande tas emot</span><span class="sxs-lookup"><span data-stu-id="dd938-116"><a id="trigger"></a> How to trigger a function when a Service Bus queue message is received</span></span>
<span data-ttu-id="dd938-117">Om du vill skriva en funktion som WebJobs SDK anropar när ett kömeddelande tas emot, använder den `ServiceBusTrigger` attribut.</span><span class="sxs-lookup"><span data-stu-id="dd938-117">To write a function that the WebJobs SDK calls when a queue message is received, use the `ServiceBusTrigger` attribute.</span></span> <span data-ttu-id="dd938-118">Attributkonstruktorn tar en parameter som anger namnet på kön avsöker.</span><span class="sxs-lookup"><span data-stu-id="dd938-118">The attribute constructor takes a parameter that specifies the name of the queue to poll.</span></span>

### <a name="how-servicebustrigger-works"></a><span data-ttu-id="dd938-119">Så här fungerar ServiceBusTrigger</span><span class="sxs-lookup"><span data-stu-id="dd938-119">How ServiceBusTrigger works</span></span>
<span data-ttu-id="dd938-120">SDK: N får ett meddelande i `PeekLock` läge och anrop `Complete` på meddelandet om funktionen slutförs eller samtal `Abandon` om misslyckas åtgärden.</span><span class="sxs-lookup"><span data-stu-id="dd938-120">The SDK receives a message in `PeekLock` mode and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails.</span></span> <span data-ttu-id="dd938-121">Om funktionen körs längre än den `PeekLock` timeout låset förnyas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="dd938-121">If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.</span></span>

<span data-ttu-id="dd938-122">Service Bus har sin egen hantering av skadligt kön som inte styrs eller konfigurerats av WebJobs-SDK.</span><span class="sxs-lookup"><span data-stu-id="dd938-122">Service Bus does its own poison queue handling which cannot be controlled or configured by the WebJobs SDK.</span></span> 

### <a name="string-queue-message"></a><span data-ttu-id="dd938-123">Strängen kömeddelande</span><span class="sxs-lookup"><span data-stu-id="dd938-123">String queue message</span></span>
<span data-ttu-id="dd938-124">Följande kodexempel läser ett meddelande i kön som innehåller en sträng och skriver strängen till WebJobs-SDK-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="dd938-124">The following code sample reads a queue message that contains a string and writes the string to the WebJobs SDK dashboard.</span></span>

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

<span data-ttu-id="dd938-125">**Obs:** om du skapar Kömeddelanden i ett program som inte använder WebJobs SDK, se till att ange [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) till ”text/plain”.</span><span class="sxs-lookup"><span data-stu-id="dd938-125">**Note:** If you are creating the queue messages in an application that doesn't use the WebJobs SDK, make sure to set [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) to "text/plain".</span></span>

### <a name="poco-queue-message"></a><span data-ttu-id="dd938-126">POCO kömeddelande</span><span class="sxs-lookup"><span data-stu-id="dd938-126">POCO queue message</span></span>
<span data-ttu-id="dd938-127">SDK: N kommer automatiskt att deserialisera ett kömeddelande som innehåller JSON för en POCO [(vanlig gamla CLR-objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typen.</span><span class="sxs-lookup"><span data-stu-id="dd938-127">The SDK will automatically deserialize a queue message that contains JSON for a POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type.</span></span> <span data-ttu-id="dd938-128">Följande kodexempel läser ett meddelande i kön som innehåller en `BlobInformation` objekt som har en `BlobName` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="dd938-128">The following code sample reads a queue message that contains a `BlobInformation` object which has a `BlobName` property:</span></span>

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="dd938-129">Kodexempel visar hur du använder egenskaper för POCO för att arbeta med blobbar och tabeller i samma funktion, finns det [lagring köer versionen av den här artikeln](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span><span class="sxs-lookup"><span data-stu-id="dd938-129">For code samples showing how to use properties of the POCO to work with blobs and tables in the same function, see the [storage queues version of this article](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span></span>

<span data-ttu-id="dd938-130">Om din kod som skapar kömeddelandet inte använda WebJobs-SDK använder du koden som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dd938-130">If your code that creates the queue message doesn't use the WebJobs SDK, use code similar to the following example:</span></span>

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a><span data-ttu-id="dd938-131">Typer ServiceBusTrigger fungerar med</span><span class="sxs-lookup"><span data-stu-id="dd938-131">Types ServiceBusTrigger works with</span></span>
<span data-ttu-id="dd938-132">Förutom `string` och POCO typer som du kan använda den `ServiceBusTrigger` attribut med en bytematris eller en `BrokeredMessage` objektet.</span><span class="sxs-lookup"><span data-stu-id="dd938-132">Besides `string` and POCO  types, you can use the `ServiceBusTrigger` attribute with a byte array or a `BrokeredMessage` object.</span></span>

## <span data-ttu-id="dd938-133"><a id="create"></a>Så här skapar du meddelanden i Service Bus-kö</span><span class="sxs-lookup"><span data-stu-id="dd938-133"><a id="create"></a> How to create Service Bus queue messages</span></span>
<span data-ttu-id="dd938-134">Att skriva en funktion som skapar en ny kö meddelandet använder den `ServiceBus` attributet och skicka in könamnet till Attributkonstruktorn.</span><span class="sxs-lookup"><span data-stu-id="dd938-134">To write a function that creates a new queue message use the `ServiceBus` attribute and pass in the queue name to the attribute constructor.</span></span> 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a><span data-ttu-id="dd938-135">Skapa ett enda kömeddelande i en icke-async-funktion</span><span class="sxs-lookup"><span data-stu-id="dd938-135">Create a single queue message in a non-async function</span></span>
<span data-ttu-id="dd938-136">Följande kodexempel används en output-parameter för att skapa ett nytt meddelande i kön med namnet ”outputqueue” med samma innehåll som meddelandet som togs emot i kön med namnet ”inputqueue”.</span><span class="sxs-lookup"><span data-stu-id="dd938-136">The following code sample uses an output parameter to create a new message in the queue named "outputqueue" with the same content as the message received in the queue named "inputqueue".</span></span>

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

<span data-ttu-id="dd938-137">Output-parameter för att skapa ett enda kömeddelande kan vara något av följande typer:</span><span class="sxs-lookup"><span data-stu-id="dd938-137">The output parameter for creating a single queue message can be any of the following types:</span></span>

* `string`
* `byte[]`
* `BrokeredMessage`
* <span data-ttu-id="dd938-138">En serialiserbar POCO-typ som du definierar.</span><span class="sxs-lookup"><span data-stu-id="dd938-138">A serializable POCO type that you define.</span></span> <span data-ttu-id="dd938-139">Serialiserad automatiskt som JSON.</span><span class="sxs-lookup"><span data-stu-id="dd938-139">Automatically serialized as JSON.</span></span>

<span data-ttu-id="dd938-140">För POCO typparametrar skapas ett kömeddelande alltid när funktionen avslutas; Om parametern är null, skapar ett meddelande i kön som kommer att returnera null när meddelandet tas emot och avserialiseras SDK.</span><span class="sxs-lookup"><span data-stu-id="dd938-140">For POCO type parameters, a queue message is always created when the function ends; if the parameter is null, the SDK creates a queue message that will return null when the message is received and deserialized.</span></span> <span data-ttu-id="dd938-141">För andra typer skapas inget kömeddelande om parametern är null.</span><span class="sxs-lookup"><span data-stu-id="dd938-141">For the other types, if the parameter is null no queue message is created.</span></span>

### <a name="create-multiple-queue-messages-or-in-async-functions"></a><span data-ttu-id="dd938-142">Skapa flera Kömeddelanden eller i async-funktioner</span><span class="sxs-lookup"><span data-stu-id="dd938-142">Create multiple queue messages or in async functions</span></span>
<span data-ttu-id="dd938-143">Du kan skapa flera meddelanden med den `ServiceBus` attributet med `ICollector<T>` eller `IAsyncCollector<T>`som visas i följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="dd938-143">To create multiple messages, use  the `ServiceBus` attribute with `ICollector<T>` or `IAsyncCollector<T>`, as shown in the following code sample:</span></span>

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="dd938-144">Varje meddelande i kön skapas omedelbart när den `Add` metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="dd938-144">Each queue message is created immediately when the `Add` method is called.</span></span>

## <span data-ttu-id="dd938-145"><a id="topics"></a>Hur du arbetar med Service Bus-ämnen</span><span class="sxs-lookup"><span data-stu-id="dd938-145"><a id="topics"></a>How to work with Service Bus topics</span></span>
<span data-ttu-id="dd938-146">Om du vill skriva en funktion som SDK anropar när ett meddelande tas emot på Service Bus-ämne, använda den `ServiceBusTrigger` attribut med konstruktorn som tar ämnet och prenumerationsnamn, som visas i följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="dd938-146">To write a function that the SDK calls when a message is received on a Service Bus topic, use the `ServiceBusTrigger` attribute with the constructor that takes topic name and subscription name, as shown in the following code sample:</span></span>

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

<span data-ttu-id="dd938-147">Så här skapar du ett meddelande om ett ämne på `ServiceBus` attribut med ett namn för ämnet på samma sätt som du kan använda den med en kö.</span><span class="sxs-lookup"><span data-stu-id="dd938-147">To create a message on a topic, use the `ServiceBus` attribute with a topic name the same way you use it with a queue name.</span></span>

## <a name="features-added-in-release-11"></a><span data-ttu-id="dd938-148">Funktioner i version 1.1</span><span class="sxs-lookup"><span data-stu-id="dd938-148">Features added in release 1.1</span></span>
<span data-ttu-id="dd938-149">Följande funktioner har lagts till i version 1.1:</span><span class="sxs-lookup"><span data-stu-id="dd938-149">The following features were added in release 1.1:</span></span>

* <span data-ttu-id="dd938-150">Tillåt djup anpassning av meddelandebehandling `ServiceBusConfiguration.MessagingProvider`.</span><span class="sxs-lookup"><span data-stu-id="dd938-150">Allow deep customization of message processing via `ServiceBusConfiguration.MessagingProvider`.</span></span>
* <span data-ttu-id="dd938-151">`MessagingProvider`har stöd för anpassning av Service Bus `MessagingFactory` och `NamespaceManager`.</span><span class="sxs-lookup"><span data-stu-id="dd938-151">`MessagingProvider` supports customization of the Service Bus `MessagingFactory` and `NamespaceManager`.</span></span>
* <span data-ttu-id="dd938-152">En `MessageProcessor` strategimönster kan du ange en processor per kö/avsnittet.</span><span class="sxs-lookup"><span data-stu-id="dd938-152">A `MessageProcessor` strategy pattern allows you to specify a processor per queue/topic.</span></span>
* <span data-ttu-id="dd938-153">Meddelandet bearbetning samtidighet stöds som standard.</span><span class="sxs-lookup"><span data-stu-id="dd938-153">Message processing concurrency is supported by default.</span></span> 
* <span data-ttu-id="dd938-154">Enkelt anpassning av `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span><span class="sxs-lookup"><span data-stu-id="dd938-154">Easy customization of `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span></span>
* <span data-ttu-id="dd938-155">Tillåt [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) anges på `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (för scenarier där du kan inte hantera rättigheterna).</span><span class="sxs-lookup"><span data-stu-id="dd938-155">Allow [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) to be specified on `ServiceBusTriggerAttribute`/`ServiceBusAttribute` (for scenarios where you might not have Manage rights).</span></span> <span data-ttu-id="dd938-156">Observera att det går inte att automatiskt etablera obefintlig köer och ämnen utan att hantera AccessRights Azure WebJobs.</span><span class="sxs-lookup"><span data-stu-id="dd938-156">Note that Azure WebJobs is unable to automatically provision non-existent queues and topics without Manage AccessRights.</span></span>

## <span data-ttu-id="dd938-157"><a id="queues"></a>Närliggande ämnen som omfattas av lagring köer anvisningar artikel</span><span class="sxs-lookup"><span data-stu-id="dd938-157"><a id="queues"></a>Related topics covered by the storage queues how-to article</span></span>
<span data-ttu-id="dd938-158">Information om WebJobs-SDK-scenarier som inte är specifika för Service Bus finns [använda Azure queue storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="dd938-158">For information about WebJobs SDK scenarios not specific to Service Bus, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="dd938-159">Följande: avsnitt beskrivs i artikeln</span><span class="sxs-lookup"><span data-stu-id="dd938-159">Topics covered in that article include the following:</span></span>

* <span data-ttu-id="dd938-160">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="dd938-160">Async functions</span></span>
* <span data-ttu-id="dd938-161">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="dd938-161">Multiple instances</span></span>
* <span data-ttu-id="dd938-162">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="dd938-162">Graceful shutdown</span></span>
* <span data-ttu-id="dd938-163">Använda WebJobs-SDK-attribut i brödtexten för en funktion</span><span class="sxs-lookup"><span data-stu-id="dd938-163">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="dd938-164">Ange anslutningssträngar SDK i kod</span><span class="sxs-lookup"><span data-stu-id="dd938-164">Set the SDK connection strings in code</span></span>
* <span data-ttu-id="dd938-165">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="dd938-165">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="dd938-166">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="dd938-166">Trigger a function manually</span></span>
* <span data-ttu-id="dd938-167">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="dd938-167">Write logs</span></span>

## <span data-ttu-id="dd938-168"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd938-168"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="dd938-169">Den här guiden tillhandahåller kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="dd938-169">This guide has provided code samples that show how to handle common scenarios for working with Azure Service Bus.</span></span> <span data-ttu-id="dd938-170">Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="dd938-170">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

