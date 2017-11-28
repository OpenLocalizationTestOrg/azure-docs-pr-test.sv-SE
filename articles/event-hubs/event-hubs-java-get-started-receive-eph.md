---
title: "aaaReceive händelser från Azure Event Hubs använder Java | Microsoft Docs"
description: "Börja ta emot från Event Hubs använder Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 05414a22e6616296752c678bb0af887d6f070c12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a><span data-ttu-id="d8f21-103">Ta emot händelser från Azure Event Hubs använder Java</span><span class="sxs-lookup"><span data-stu-id="d8f21-103">Receive events from Azure Event Hubs using Java</span></span>


## <a name="introduction"></a><span data-ttu-id="d8f21-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d8f21-104">Introduction</span></span>
<span data-ttu-id="d8f21-105">Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund som aktiverar ett program tooprocess och analysera hello stora mängder data som produceras av dina anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="d8f21-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="d8f21-106">När uppgifterna väl samlats i Event Hubs, kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalyser eller lagringskluster.</span><span class="sxs-lookup"><span data-stu-id="d8f21-106">Once collected into Event Hubs, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="d8f21-107">Mer information finns i hello [översikt av Händelsehubbar][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="d8f21-107">For more information, see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="d8f21-108">Den här kursen visar hur tooreceive händelser i en händelsehubb med hjälp av ett konsolprogram som skrivits i Java.</span><span class="sxs-lookup"><span data-stu-id="d8f21-108">This tutorial shows how tooreceive events into an event hub using a console application written in Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8f21-109">Krav</span><span class="sxs-lookup"><span data-stu-id="d8f21-109">Prerequisites</span></span>

<span data-ttu-id="d8f21-110">I ordning toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="d8f21-110">In order toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="d8f21-111">Java development environment.</span><span class="sxs-lookup"><span data-stu-id="d8f21-111">A Java development environment.</span></span> <span data-ttu-id="d8f21-112">Den här självstudiekursen förutsätter vi att [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="d8f21-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="d8f21-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d8f21-113">An active Azure account.</span></span> <br/><span data-ttu-id="d8f21-114">Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d8f21-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="d8f21-115">Mer information om den <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="d8f21-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="receive-messages-with-eventprocessorhost-in-java"></a><span data-ttu-id="d8f21-116">Ta emot meddelanden med EventProcessorHost i Java</span><span class="sxs-lookup"><span data-stu-id="d8f21-116">Receive messages with EventProcessorHost in Java</span></span>

<span data-ttu-id="d8f21-117">**EventProcessorHost** är en javaklass som förenklar mottagandet av händelser från Event Hubs genom att hantera permanenta kontrollpunkter och parallella mottaganden från Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="d8f21-117">**EventProcessorHost** is a Java class that simplifies receiving events from Event Hubs by managing persistent checkpoints and parallel receives from those Event Hubs.</span></span> <span data-ttu-id="d8f21-118">Med EventProcessorHost kan dela du händelser på flera olika mottagare, även när de ligger på olika noder.</span><span class="sxs-lookup"><span data-stu-id="d8f21-118">Using EventProcessorHost, you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="d8f21-119">Det här exemplet illustrerar hur toouse EventProcessorHost för en enda mottagare.</span><span class="sxs-lookup"><span data-stu-id="d8f21-119">This example shows how toouse EventProcessorHost for a single receiver.</span></span>

### <a name="create-a-storage-account"></a><span data-ttu-id="d8f21-120">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="d8f21-120">Create a storage account</span></span>
<span data-ttu-id="d8f21-121">toouse EventProcessorHost, måste du ha en [Azure Storage-konto][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="d8f21-121">toouse EventProcessorHost, you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="d8f21-122">Logga in toohello [Azure-portalen][Azure portal], och klicka på **+ ny** hello hello skärmens vänstra sida.</span><span class="sxs-lookup"><span data-stu-id="d8f21-122">Log on toohello [Azure portal][Azure portal], and click **+ New** on hello left-hand side of hello screen.</span></span>
2. <span data-ttu-id="d8f21-123">Klicka på **Lagring** och sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="d8f21-123">Click **Storage**, then click **Storage account**.</span></span> <span data-ttu-id="d8f21-124">I hello **skapa lagringskonto** bladet, ange ett namn för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d8f21-124">In hello **Create storage account** blade, type a name for hello storage account.</span></span> <span data-ttu-id="d8f21-125">Fyll hello resten av hello fält, Välj önskad region och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d8f21-125">Complete hello rest of hello fields, select your desired region, and then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. <span data-ttu-id="d8f21-126">Klicka på hello nyligen skapade lagringskontot och klicka sedan på **hantera åtkomstnycklar**:</span><span class="sxs-lookup"><span data-stu-id="d8f21-126">Click hello newly created storage account, and then click **Manage Access Keys**:</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    <span data-ttu-id="d8f21-127">Kopiera hello primära nyckel tooa tillfällig plats, toouse senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d8f21-127">Copy hello primary access key tooa temporary location, toouse later in this tutorial.</span></span>

### <a name="create-a-java-project-using-hello-eventprocessor-host"></a><span data-ttu-id="d8f21-128">Skapa en Java-projekt med hello EventProcessor värden</span><span class="sxs-lookup"><span data-stu-id="d8f21-128">Create a Java project using hello EventProcessor Host</span></span>
<span data-ttu-id="d8f21-129">hello Java-klientbibliotek för Händelsehubbar är tillgänglig för användning i Maven-projekt från hello [Maven centrallager][Maven Package], och kan refereras i hello följande beroende deklarationen i din Maven-projekt-filen:</span><span class="sxs-lookup"><span data-stu-id="d8f21-129">hello Java client library for Event Hubs is available for use in Maven projects from hello [Maven Central Repository][Maven Package], and can be referenced using hello following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-eventhubs-eph</artifactId>
  <version>0.14.0</version>
</dependency>
```

<span data-ttu-id="d8f21-130">För olika typer av versionsmiljöer du explicit kan hämta hello senaste publicerat JAR-filer från hello [Maven centrallager] [ Maven Package] eller från [hello versionen distributionsplatsen på GitHub](https://github.com/Azure/azure-event-hubs/releases).</span><span class="sxs-lookup"><span data-stu-id="d8f21-130">For different types of build environments, you can explicitly obtain hello latest released JAR files from hello [Maven Central Repository][Maven Package] or from [hello release distribution point on GitHub](https://github.com/Azure/azure-event-hubs/releases).</span></span>  

1. <span data-ttu-id="d8f21-131">För hello följande exempel, skapa ett nytt Maven-projekt för ett program på konsolen-gränssnittet i din favorit Java-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8f21-131">For hello following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="d8f21-132">hello klassen kallas `ErrorNotificationHandler`.</span><span class="sxs-lookup"><span data-stu-id="d8f21-132">hello class is called `ErrorNotificationHandler`.</span></span>     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. <span data-ttu-id="d8f21-133">Använd hello följande kod toocreate kallas för en ny klass `EventProcessor`.</span><span class="sxs-lookup"><span data-stu-id="d8f21-133">Use hello following code toocreate a new class called `EventProcessor`.</span></span>
   
    ```java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;
   
    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;
   
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }
   
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
   
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }
   
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
   
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```
3. <span data-ttu-id="d8f21-134">Skapa en mer klass med namnet `EventProcessorSample`med hjälp av hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="d8f21-134">Create one more class called `EventProcessorSample`, using hello following code.</span></span>
   
    ```java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;
   
    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";
   
            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
   
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
   
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
   
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }
   
            System.out.println("Press enter toostop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
   
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
   
            System.out.println("End of sample");
        }
    }
    ```
4. <span data-ttu-id="d8f21-135">Ersätt hello följande fält med hello värden som ska användas när du skapade hello händelse NAV- och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d8f21-135">Replace hello following fields with hello values used when you created hello event hub and storage account.</span></span>
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> <span data-ttu-id="d8f21-136">Den här guiden använder en enda instans av EventProcessorHost.</span><span class="sxs-lookup"><span data-stu-id="d8f21-136">This tutorial uses a single instance of EventProcessorHost.</span></span> <span data-ttu-id="d8f21-137">tooincrease genomströmning rekommenderas att du kör flera instanser av EventProcessorHost, helst på separata datorer.</span><span class="sxs-lookup"><span data-stu-id="d8f21-137">tooincrease throughput, it is recommended that you run multiple instances of EventProcessorHost, preferably on separate machines.</span></span>  <span data-ttu-id="d8f21-138">Detta ger redundans samt.</span><span class="sxs-lookup"><span data-stu-id="d8f21-138">This provides redundancy as well.</span></span> <span data-ttu-id="d8f21-139">I sådana fall hello olika instanserna automatiskt sinsemellan i ordning tooload saldo hello emot händelser.</span><span class="sxs-lookup"><span data-stu-id="d8f21-139">In those cases, hello various instances automatically coordinate with each other in order tooload balance hello received events.</span></span> <span data-ttu-id="d8f21-140">Om du vill att flera mottagare tooeach processen *alla* hello händelser, måste du använda hello **ConsumerGroup** begrepp.</span><span class="sxs-lookup"><span data-stu-id="d8f21-140">If you want multiple receivers tooeach process *all* hello events, you must use hello **ConsumerGroup** concept.</span></span> <span data-ttu-id="d8f21-141">När du tar emot händelser från olika datorer, kanske användbart toospecify namn för EventProcessorHost instanser, baserat på hello datorer (eller roller) i som de har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="d8f21-141">When receiving events from different machines, it might be useful toospecify names for EventProcessorHost instances based on hello machines (or roles) in which they are deployed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d8f21-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8f21-142">Next steps</span></span>
<span data-ttu-id="d8f21-143">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="d8f21-143">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="d8f21-144">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="d8f21-144">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="d8f21-145">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="d8f21-145">Create an Event Hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="d8f21-146">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d8f21-146">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
