---
title: aaaOverview av Azure Event Hubs avbilda | Microsoft Docs
description: Samla in telemetridata med Event Hubs avbilda
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: sethm;darosa
ms.openlocfilehash: 0238cae712a0ed7bdf3e87ee93a069a553cb65df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-event-hubs-capture"></a><span data-ttu-id="933cb-103">Azure Event Hubs avbildning</span><span class="sxs-lookup"><span data-stu-id="933cb-103">Azure Event Hubs Capture</span></span>

<span data-ttu-id="933cb-104">Azure Event Hubs avbilda kan du tooautomatically leverera hello strömmande data i Händelsehubbar tooan [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) eller [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) konto önskat med hello extra flexibilitet Ange ett intervall för tid, eller storlek.</span><span class="sxs-lookup"><span data-stu-id="933cb-104">Azure Event Hubs Capture enables you tooautomatically deliver hello streaming data in Event Hubs tooan [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) or [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account of your choice, with hello added flexibility of specifying a time or size interval.</span></span> <span data-ttu-id="933cb-105">Ställa in avbildningen är snabb, det finns inga administrativa kostnader toorun den och skalar automatiskt med Händelsehubbar [genomflödesenheter](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="933cb-105">Setting up Capture is fast, there are no administrative costs toorun it, and it scales automatically with Event Hubs [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="933cb-106">Event Hubs avbilda är hello enklaste sättet tooload strömmande data till Azure och aktiverar toofocus databehandling i stället för datainsamling.</span><span class="sxs-lookup"><span data-stu-id="933cb-106">Event Hubs Capture is hello easiest way tooload streaming data into Azure, and enables you toofocus on data processing rather than on data capture.</span></span>

<span data-ttu-id="933cb-107">Event Hubs avbilda kan du tooprocess i realtid och batch-baserade pipelines på hello samma dataström.</span><span class="sxs-lookup"><span data-stu-id="933cb-107">Event Hubs Capture enables you tooprocess real-time and batch-based pipelines on hello same stream.</span></span> <span data-ttu-id="933cb-108">Det innebär att du kan skapa lösningar som växa med dina behov över tid.</span><span class="sxs-lookup"><span data-stu-id="933cb-108">This means you can build solutions that grow with your needs over time.</span></span> <span data-ttu-id="933cb-109">Om du utvecklar batch-baserade system idag med ett öga mot framtida realtidsbearbetning eller om du vill tooadd en effektiv cold sökvägen tooan befintlig realtid lösning, gör Event Hubs avbilda arbeta med strömmande data enklare.</span><span class="sxs-lookup"><span data-stu-id="933cb-109">Whether you're building batch-based systems today with an eye towards future real-time processing, or you want tooadd an efficient cold path tooan existing real-time solution, Event Hubs Capture makes working with streaming data easier.</span></span>

## <a name="how-event-hubs-capture-works"></a><span data-ttu-id="933cb-110">Så här fungerar Event Hubs avbilda</span><span class="sxs-lookup"><span data-stu-id="933cb-110">How Event Hubs Capture works</span></span>

<span data-ttu-id="933cb-111">Händelsehubbar är en tid kvarhållning varaktiga buffert för telemetriingång, liknande tooa distribuerade loggen.</span><span class="sxs-lookup"><span data-stu-id="933cb-111">Event Hubs is a time-retention durable buffer for telemetry ingress, similar tooa distributed log.</span></span> <span data-ttu-id="933cb-112">hello viktiga tooscaling i Händelsehubbar är hello [konsumentmönster indelat modellen](event-hubs-features.md#partitions).</span><span class="sxs-lookup"><span data-stu-id="933cb-112">hello key tooscaling in Event Hubs is hello [partitioned consumer model](event-hubs-features.md#partitions).</span></span> <span data-ttu-id="933cb-113">Varje partition är en oberoende segment av data och används oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="933cb-113">Each partition is an independent segment of data and is consumed independently.</span></span> <span data-ttu-id="933cb-114">Över tiden data åldras ut, baserat på hello konfigurerbara kvarhållningsperioden.</span><span class="sxs-lookup"><span data-stu-id="933cb-114">Over time this data ages off, based on hello configurable retention period.</span></span> <span data-ttu-id="933cb-115">Därför det finns en given händelsehubb för aldrig hämtar ”full”.</span><span class="sxs-lookup"><span data-stu-id="933cb-115">As a result, a given event hub never gets "too full."</span></span>

<span data-ttu-id="933cb-116">Event Hubs avbilda kan du toospecify din egen Azure Blob storage-konto och en behållare eller ett Azure Data Lake Store-konto, vilket används toostore hello infångade data.</span><span class="sxs-lookup"><span data-stu-id="933cb-116">Event Hubs Capture enables you toospecify your own Azure Blob storage account and container, or Azure Data Lake Store account, which are used toostore hello captured data.</span></span> <span data-ttu-id="933cb-117">Dessa konton kan ha hello samma region som din händelsehubb eller i en annan region, lägger till toohello flexibilitet för hello Event Hubs avbilda funktion.</span><span class="sxs-lookup"><span data-stu-id="933cb-117">These accounts can be in hello same region as your event hub or in another region, adding toohello flexibility of hello Event Hubs Capture feature.</span></span>

<span data-ttu-id="933cb-118">Insamlade data skrivs i [Apache Avro] [ Apache Avro] format: ett compact, snabb, binära format som ger omfattande datastrukturer inlineschema.</span><span class="sxs-lookup"><span data-stu-id="933cb-118">Captured data is written in [Apache Avro][Apache Avro] format: a compact, fast, binary format that provides rich data structures with inline schema.</span></span> <span data-ttu-id="933cb-119">Det här formatet används ofta i hello Hadoop-ekosystemet, Stream Analytics och Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="933cb-119">This format is widely used in hello Hadoop ecosystem, Stream Analytics, and Azure Data Factory.</span></span> <span data-ttu-id="933cb-120">Mer information om hur du arbetar med Avro finns senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="933cb-120">More information about working with Avro is available later in this article.</span></span>

### <a name="capture-windowing"></a><span data-ttu-id="933cb-121">Avbilda fönsterhantering</span><span class="sxs-lookup"><span data-stu-id="933cb-121">Capture windowing</span></span>

<span data-ttu-id="933cb-122">Event Hubs avbilda gör tooset upp en avbildning av fönstret toocontrol.</span><span class="sxs-lookup"><span data-stu-id="933cb-122">Event Hubs Capture enables you tooset up a window toocontrol capturing.</span></span> <span data-ttu-id="933cb-123">Det här fönstret är en minsta storlek och konfiguration med en ”första WINS-princip”, vilket innebär en capture-åtgärd som hello första utlösaren påträffade orsaker.</span><span class="sxs-lookup"><span data-stu-id="933cb-123">This window is a minimum size and time configuration with a "first wins policy," meaning that hello first trigger encountered causes a capture operation.</span></span> <span data-ttu-id="933cb-124">Om du har en femton minuter fönstret capture 100 MB och skicka 1 MB per sekund, hello storlek fönstret utlösare innan hello tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="933cb-124">If you have a fifteen-minute, 100 MB capture window and send 1 MB per second, hello size window triggers before hello time window.</span></span> <span data-ttu-id="933cb-125">Varje partition in separat och skriver slutförda blockblob när hello avbildning för hello tid vid vilken hello avbilda intervall påträffades.</span><span class="sxs-lookup"><span data-stu-id="933cb-125">Each partition captures independently and writes a completed block blob at hello time of capture, named for hello time at which hello capture interval was encountered.</span></span> <span data-ttu-id="933cb-126">hello namngivningskonvention för lagring är följande:</span><span class="sxs-lookup"><span data-stu-id="933cb-126">hello storage naming convention is as follows:</span></span>

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-toothroughput-units"></a><span data-ttu-id="933cb-127">Skalning toothroughput enheter</span><span class="sxs-lookup"><span data-stu-id="933cb-127">Scaling toothroughput units</span></span>

<span data-ttu-id="933cb-128">Event Hubs trafik styrs av [genomflödesenheter](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="933cb-128">Event Hubs traffic is controlled by [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="933cb-129">En genomflödesenhet kan 1 MB per sekund eller 1 000 händelser per sekund på inkommande trafik och två gånger att mängden utgång.</span><span class="sxs-lookup"><span data-stu-id="933cb-129">A single throughput unit allows 1 MB per second or 1000 events per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="933cb-130">Standard Händelsehubbar kan konfigureras med 1-20 genomflödesenheter och du kan köpa fler med en kvot öka [supportbegäran][support request].</span><span class="sxs-lookup"><span data-stu-id="933cb-130">Standard Event Hubs can be configured with 1-20 throughput units, and you can purchase more with a quota increase [support request][support request].</span></span> <span data-ttu-id="933cb-131">Användning utöver din inköpta genomflödesenheter begränsas.</span><span class="sxs-lookup"><span data-stu-id="933cb-131">Usage beyond your purchased throughput units is throttled.</span></span> <span data-ttu-id="933cb-132">Event Hubs avbilda kopierar data direkt från hello intern Händelsehubbar lagring, kringgå genomströmning enhet utgång kvoter och spara din utgång för andra bearbetning, till exempel Stream Analytics eller Spark.</span><span class="sxs-lookup"><span data-stu-id="933cb-132">Event Hubs Capture copies data directly from hello internal Event Hubs storage, bypassing throughput unit egress quotas and saving your egress for other processing readers, such as Stream Analytics or Spark.</span></span>

<span data-ttu-id="933cb-133">När du konfigurerat Event Hubs avbilda körs automatiskt när du skickar din första händelsen och fortsätter att köras.</span><span class="sxs-lookup"><span data-stu-id="933cb-133">Once configured, Event Hubs Capture runs automatically when you send your first event, and continues running.</span></span> <span data-ttu-id="933cb-134">toomake underlättar för dina bearbetningen nedströms tooknow som hello processen fungerar Händelsehubbar skriver tomma filer när det finns inga data.</span><span class="sxs-lookup"><span data-stu-id="933cb-134">toomake it easier for your downstream processing tooknow that hello process is working, Event Hubs writes empty files when there is no data.</span></span> <span data-ttu-id="933cb-135">Den här processen ger en förutsägbar takt och markör som kan mata batch-processorer.</span><span class="sxs-lookup"><span data-stu-id="933cb-135">This process provides a predictable cadence and marker that can feed your batch processors.</span></span>

## <a name="setting-up-event-hubs-capture"></a><span data-ttu-id="933cb-136">Konfigurera Event Hubs avbilda</span><span class="sxs-lookup"><span data-stu-id="933cb-136">Setting up Event Hubs Capture</span></span>

<span data-ttu-id="933cb-137">Du kan konfigurera avbilda vid hello event hub skapandet med hello [Azure-portalen](https://portal.azure.com), eller med hjälp av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="933cb-137">You can configure Capture at hello event hub creation time using hello [Azure portal](https://portal.azure.com), or using Azure Resource Manager templates.</span></span> <span data-ttu-id="933cb-138">Mer information finns i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="933cb-138">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="933cb-139">Aktivera Event Hubs hämtar med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="933cb-139">Enable Event Hubs Capture using hello Azure portal</span></span>](event-hubs-capture-enable-through-portal.md)
- [<span data-ttu-id="933cb-140">Skapa ett namnområde för Händelsehubbar med en händelsehubb och aktivera avbildning med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="933cb-140">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-hello-captured-files-and-working-with-avro"></a><span data-ttu-id="933cb-141">Utforska hello avbildas filer och arbeta med Avro</span><span class="sxs-lookup"><span data-stu-id="933cb-141">Exploring hello captured files and working with Avro</span></span>

<span data-ttu-id="933cb-142">Event Hubs avbilda skapar filer i Avro-format, som anges på hello konfigurerade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="933cb-142">Event Hubs Capture creates files in Avro format, as specified on hello configured time window.</span></span> <span data-ttu-id="933cb-143">Du kan visa dessa filer i ett verktyg som [Azure Lagringsutforskaren][Azure Storage Explorer].</span><span class="sxs-lookup"><span data-stu-id="933cb-143">You can view these files in any tool such as [Azure Storage Explorer][Azure Storage Explorer].</span></span> <span data-ttu-id="933cb-144">Du kan hämta hello filer lokalt toowork på dem.</span><span class="sxs-lookup"><span data-stu-id="933cb-144">You can download hello files locally toowork on them.</span></span>

<span data-ttu-id="933cb-145">hello-filer som skapas av Event Hubs avbilda har hello följande Avro-schemat:</span><span class="sxs-lookup"><span data-stu-id="933cb-145">hello files produced by Event Hubs Capture have hello following Avro schema:</span></span>

![][3]

<span data-ttu-id="933cb-146">Ett enkelt sätt tooexplore Avro-filernas är med hjälp av hello [Avro verktyg] [ Avro Tools] jar från Apache.</span><span class="sxs-lookup"><span data-stu-id="933cb-146">An easy way tooexplore Avro files is by using hello [Avro Tools][Avro Tools] jar from Apache.</span></span> <span data-ttu-id="933cb-147">Du kan se hello schemat för en viss Avro-fil genom att köra följande kommando hello när du har hämtat den här jar:</span><span class="sxs-lookup"><span data-stu-id="933cb-147">After downloading this jar, you can see hello schema of a specific Avro file by running hello following command:</span></span>

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

<span data-ttu-id="933cb-148">Det här kommandot returnerar</span><span class="sxs-lookup"><span data-stu-id="933cb-148">This command returns</span></span>

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

<span data-ttu-id="933cb-149">Du kan också använder Avro verktyg tooconvert hello loggfilsformat tooJSON och utföra andra bearbetning.</span><span class="sxs-lookup"><span data-stu-id="933cb-149">You can also use Avro Tools tooconvert hello file tooJSON format and perform other processing.</span></span>

<span data-ttu-id="933cb-150">tooperform mer avancerade bearbetning, hämta och installera Avro för valet av plattform.</span><span class="sxs-lookup"><span data-stu-id="933cb-150">tooperform more advanced processing, download and install Avro for your choice of platform.</span></span> <span data-ttu-id="933cb-151">När hello detta skrivs, det finns implementeringar för C, C++, C\#, Java, NodeJS, Perl, PHP, Python eller Ruby.</span><span class="sxs-lookup"><span data-stu-id="933cb-151">At hello time of this writing, there are implementations available for C, C++, C\#, Java, NodeJS, Perl, PHP, Python, and Ruby.</span></span>

<span data-ttu-id="933cb-152">Apache Avro har slutförts komma igång guider för [Java] [ Java] och [Python][Python].</span><span class="sxs-lookup"><span data-stu-id="933cb-152">Apache Avro has complete Getting Started guides for [Java][Java] and [Python][Python].</span></span> <span data-ttu-id="933cb-153">Du kan också läsa hello [komma igång med Event Hubs avbilda](event-hubs-capture-python.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="933cb-153">You can also read hello [Getting started with Event Hubs Capture](event-hubs-capture-python.md) article.</span></span>

## <a name="how-event-hubs-capture-is-charged"></a><span data-ttu-id="933cb-154">Hur Event Hubs avbilda debiteras</span><span class="sxs-lookup"><span data-stu-id="933cb-154">How Event Hubs Capture is charged</span></span>

<span data-ttu-id="933cb-155">Event Hubs avbilda mäts på samma sätt toothroughput enheter: som timkostnaden.</span><span class="sxs-lookup"><span data-stu-id="933cb-155">Event Hubs Capture is metered similarly toothroughput units: as an hourly charge.</span></span> <span data-ttu-id="933cb-156">hello är proportionell toohello antalet genomflödesenheter för hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="933cb-156">hello charge is directly proportional toohello number of throughput units purchased for hello namespace.</span></span> <span data-ttu-id="933cb-157">Genomflödesenheter ökas och minskas, Event Hubs avbilda mätare öka och minska tooprovide matchar prestanda.</span><span class="sxs-lookup"><span data-stu-id="933cb-157">As throughput units are increased and decreased, Event Hubs Capture meters increase and decrease tooprovide matching performance.</span></span> <span data-ttu-id="933cb-158">hello mätare uppstå tillsammans.</span><span class="sxs-lookup"><span data-stu-id="933cb-158">hello meters occur in tandem.</span></span> <span data-ttu-id="933cb-159">Information om priser, se [priser för Händelsehubbar](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="933cb-159">For pricing details, see [Event Hubs pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="933cb-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="933cb-160">Next steps</span></span>

<span data-ttu-id="933cb-161">Event Hubs avbilda är hello enklaste sättet tooget data till Azure.</span><span class="sxs-lookup"><span data-stu-id="933cb-161">Event Hubs Capture is hello easiest way tooget data into Azure.</span></span> <span data-ttu-id="933cb-162">Med Azure Data Lake och Azure Data Factory Azure HDInsight kan du utföra batchbearbetning och andra analytics med hjälp av välbekanta verktyg och plattformar du väljer, skaländras du behöver.</span><span class="sxs-lookup"><span data-stu-id="933cb-162">Using Azure Data Lake, Azure Data Factory, and Azure HDInsight, you can perform batch processing and other analytics using familiar tools and platforms of your choosing, at any scale you need.</span></span>

<span data-ttu-id="933cb-163">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="933cb-163">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="933cb-164">Börja skicka och ta emot händelser</span><span class="sxs-lookup"><span data-stu-id="933cb-164">Get started sending and receiving events</span></span>](event-hubs-dotnet-framework-getstarted-send.md)
* <span data-ttu-id="933cb-165">Ett komplett [exempelprogram som använder händelsehubbar][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="933cb-165">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs]</span></span>
* <span data-ttu-id="933cb-166">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="933cb-166">[Event Hubs overview][Event Hubs overview]</span></span>

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
