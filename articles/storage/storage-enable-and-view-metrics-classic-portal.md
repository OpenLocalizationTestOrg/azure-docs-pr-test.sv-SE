---
title: "aaaEnabling storage-mätvärden i hello Azure-portalen | Microsoft Docs"
description: "Hur tooenable storage-mätvärden för hello Blob, kön, tabell, och Filtjänster"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 2fb5b229-f099-4334-92be-4e0e7dd257d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 4c990371e08a6586d935b0535149eabd4960cfaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="781a5-103">Aktivera Storage-mätvärden och visa mått data</span><span class="sxs-lookup"><span data-stu-id="781a5-103">Enabling Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="781a5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="781a5-104">Overview</span></span>
<span data-ttu-id="781a5-105">Storage-mätvärden är aktiverad som standard när du skapar ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="781a5-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="781a5-106">Du kan konfigurera övervakning med hjälp av antingen hello [klassiska Azure-portalen](https://manage.windowsazure.com), Windows PowerShell eller via programmering genom en API-lagring.</span><span class="sxs-lookup"><span data-stu-id="781a5-106">You can configure monitoring using either hello [Azure Classic Portal](https://manage.windowsazure.com), Windows PowerShell, or programmatically through a storage API.</span></span>

<span data-ttu-id="781a5-107">När du aktiverar Storage-mätvärden, måste du välja en kvarhållningsperiod för hello data: den här perioden avgör för hur länge hello lagring tjänsten håller hello mått och kostnader för hello utrymme krävs toostore dem.</span><span class="sxs-lookup"><span data-stu-id="781a5-107">When you enable Storage Metrics, you must choose a retention period for hello data: this period determines for how long hello storage service keeps hello metrics and charges you for hello space required toostore them.</span></span> <span data-ttu-id="781a5-108">Du bör normalt använda en kortare period för minut mått än timvis mått på grund av hello betydande extra utrymme krävs för minut mått.</span><span class="sxs-lookup"><span data-stu-id="781a5-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of hello significant extra space required for minute metrics.</span></span> <span data-ttu-id="781a5-109">Du bör välja en kvarhållningsperiod så att du har tillräcklig tid tooanalyze hello data och hämta några mått som du vill tookeep för offline-analys och rapportering.</span><span class="sxs-lookup"><span data-stu-id="781a5-109">You should choose a retention period such that you have sufficient time tooanalyze hello data and download any metrics you wish tookeep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="781a5-110">Kom ihåg att du också kommer att debiteras för att ladda ned mått data från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="781a5-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-tooenable-storage-metrics-using-hello-azure-classic-portal"></a><span data-ttu-id="781a5-111">Hur tooenable lagring mått med hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="781a5-111">How tooenable Storage metrics using hello Azure Classic Portal</span></span>
<span data-ttu-id="781a5-112">I hello [klassiska Azure-portalen](https://manage.windowsazure.com), du använder hello konfigurationssidan för en storage-konto toocontrol lagring mått.</span><span class="sxs-lookup"><span data-stu-id="781a5-112">In hello [Azure Classic Portal](https://manage.windowsazure.com), you use hello Configure page for a storage account toocontrol Storage Metrics.</span></span> <span data-ttu-id="781a5-113">För övervakning, kan du ange en nivå och en Bevarandeperiod i dagar för alla blobbar, tabeller och köer.</span><span class="sxs-lookup"><span data-stu-id="781a5-113">For monitoring, you can set a level and a retention period in days for each of blobs, tables, and queues.</span></span> <span data-ttu-id="781a5-114">I varje fall är hello nivån en av följande hello:</span><span class="sxs-lookup"><span data-stu-id="781a5-114">In each case, hello level is one of hello following:</span></span>

* <span data-ttu-id="781a5-115">Ut – Samlas inga mått.</span><span class="sxs-lookup"><span data-stu-id="781a5-115">Off — No metrics are collected.</span></span>
* <span data-ttu-id="781a5-116">Minimalt-Storage-mätvärden samlar in en grundläggande uppsättning mått som ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal som aggregeras för hello Blob-, tabell- och kön.</span><span class="sxs-lookup"><span data-stu-id="781a5-116">Minimal — Storage Metrics collects a basic set of metrics such as ingress/egress, availability, latency, and success percentages, which are aggregated for hello Blob, Table, and Queue services.</span></span>
* <span data-ttu-id="781a5-117">Utförlig – Storage-mätvärden samlar in en fullständig uppsättning mått som innehåller Hej samma mätvärden för varje storage API-åtgärd, dessutom toohello servicenivåer mått.</span><span class="sxs-lookup"><span data-stu-id="781a5-117">Verbose — Storage Metrics collects a full set of metrics that includes hello same metrics for each storage API operation, in addition toohello service-level metrics.</span></span> <span data-ttu-id="781a5-118">Utförlig mått aktivera närmare analys av problem som uppstår under program-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="781a5-118">Verbose metrics enable closer analysis of issues that occur during application operations.</span></span>

<span data-ttu-id="781a5-119">Observera att hello klassiska Azure-portalen kan för närvarande du inte tooconfigure minut mått i ditt lagringskonto; Du måste aktivera minut mått med PowerShell eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="781a5-119">Note that hello Azure Classic Portal does not currently enable you tooconfigure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-tooenable-storage-metrics-using-powershell"></a><span data-ttu-id="781a5-120">Hur tooenable lagring mått med PowerShell</span><span class="sxs-lookup"><span data-stu-id="781a5-120">How tooenable Storage metrics using PowerShell</span></span>
<span data-ttu-id="781a5-121">Du kan använda PowerShell på din lokala dator tooconfigure Storage-mätvärden i ditt lagringskonto med hjälp av hello Azure PowerShell-cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello aktuella inställningar och hello cmdlet Ange AzureStorageServiceMetricsProperty toochange hello aktuella inställningar.</span><span class="sxs-lookup"><span data-stu-id="781a5-121">You can use PowerShell on your local machine tooconfigure Storage Metrics in your storage account by using hello Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello current settings, and hello cmdlet Set-AzureStorageServiceMetricsProperty toochange hello current settings.</span></span>

<span data-ttu-id="781a5-122">hello-cmdletar som styr Storage-mätvärden använder hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="781a5-122">hello cmdlets that control Storage Metrics use hello following parameters:</span></span>

* <span data-ttu-id="781a5-123">MetricsType möjliga värden är timmar och minuter.</span><span class="sxs-lookup"><span data-stu-id="781a5-123">MetricsType possible values are Hour and Minute.</span></span>
* <span data-ttu-id="781a5-124">ServiceType möjliga värden är Blob, köer och tabellen.</span><span class="sxs-lookup"><span data-stu-id="781a5-124">ServiceType possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="781a5-125">MetricsLevel möjliga värden är ingen (motsvarande tooOff i hello klassiska Azure-portalen)-tjänsten (motsvarande tooMinimal i hello klassiska Azure-portalen) och ServiceAndApi (motsvarande tooVerbose i hello klassiska Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="781a5-125">MetricsLevel possible values are None (equivalent tooOff in hello Azure Classic Portal), Service (equivalent tooMinimal in hello Azure Classic Portal), and ServiceAndApi (equivalent tooVerbose in hello Azure Classic Portal).</span></span>

<span data-ttu-id="781a5-126">Till exempel växlar hello följande kommando på minut mått hello blob-tjänst i din standardkontot för lagring med hello Bevarandeperiod ställa in toofive dagar:</span><span class="sxs-lookup"><span data-stu-id="781a5-126">For example, hello following command switches on minute metrics for hello blob service in your default storage account with hello retention period set toofive days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
```
<span data-ttu-id="781a5-127">hello hämtar följande kommando hello aktuella timvis mått nivå och kvarhållning dagar hello blob-tjänst i standardkontot för lagring:</span><span class="sxs-lookup"><span data-stu-id="781a5-127">hello following command retrieves hello current hourly metrics level and retention days for hello blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```
<span data-ttu-id="781a5-128">Information om hur tooconfigure hello Azure PowerShell-cmdlets toowork med din Azure-prenumeration och hur tooselect hello standardlagring kontot toouse finns: [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="781a5-128">For information about how tooconfigure hello Azure PowerShell cmdlets toowork with your Azure subscription and how tooselect hello default storage account toouse, see: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-tooenable-storage-metrics-programmatically"></a><span data-ttu-id="781a5-129">Hur tooenable Storage-mätvärden via programmering</span><span class="sxs-lookup"><span data-stu-id="781a5-129">How tooenable Storage metrics programmatically</span></span>
<span data-ttu-id="781a5-130">hello följande C# fragment visar hur tooenable mått och loggning för hello Blob-tjänsten använder hello storage-klientbibliotek för .NET:</span><span class="sxs-lookup"><span data-stu-id="781a5-130">hello following C# snippet shows how tooenable metrics and logging for hello Blob service using hello storage client library for .NET:</span></span>

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy too10 days. 
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at hello same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set hello default service version toobe used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set hello service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a><span data-ttu-id="781a5-131">Visa Storage-mätvärden</span><span class="sxs-lookup"><span data-stu-id="781a5-131">Viewing Storage metrics</span></span>
<span data-ttu-id="781a5-132">När du har konfigurerat lagring mått toomonitor ditt lagringskonto, registrerar hello mått i en uppsättning välkända tabeller i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="781a5-132">When you have configured Storage Metrics toomonitor your storage account, it records hello metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="781a5-133">Du kan använda hello övervakaren sidan för ditt lagringskonto i hello Azure klassiska Portal tooview hello timvis mått när de blir tillgängliga i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="781a5-133">You can use hello Monitor page for your storage account in hello Azure Classic Portal tooview hello hourly metrics as they become available on a chart.</span></span> <span data-ttu-id="781a5-134">På den här sidan i hello klassiska Azure-portalen kan du:</span><span class="sxs-lookup"><span data-stu-id="781a5-134">On this page in hello Azure Classic Portal, you can:</span></span>

* <span data-ttu-id="781a5-135">Välj vilka mått tooplot i hello diagram (hello valet av tillgängliga mått beror på om du har valt utförlig eller minimalt övervakningen hello-tjänsten på konfigurationssidan hello).</span><span class="sxs-lookup"><span data-stu-id="781a5-135">Select which metrics tooplot on hello chart (hello choice of available metrics will depend on whether you chose verbose or minimal monitoring for hello service on hello Configure page).</span></span>
* <span data-ttu-id="781a5-136">Välj hello tidsintervallet för hello mått som visas på hello diagram.</span><span class="sxs-lookup"><span data-stu-id="781a5-136">Select hello time range for hello metrics displayed on hello chart.</span></span>
* <span data-ttu-id="781a5-137">Välj toouse en absolut eller relativ skala tooplot hello mått.</span><span class="sxs-lookup"><span data-stu-id="781a5-137">Choose toouse an absolute or relative scale tooplot hello metrics.</span></span>
* <span data-ttu-id="781a5-138">Konfigurera e-aviseringar toonotify när ett specifikt mått når ett visst värde.</span><span class="sxs-lookup"><span data-stu-id="781a5-138">Configure email alerts toonotify you when a specific metric reaches a certain value.</span></span>

<span data-ttu-id="781a5-139">Om du vill toodownload hello mått för långsiktig lagring eller tooanalyze dem lokalt, du behöver toouse ett verktyg eller skriva kod tooread hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="781a5-139">If you want toodownload hello metrics for long-term storage or tooanalyze them locally, you will need toouse a tool or write some code tooread hello tables.</span></span> <span data-ttu-id="781a5-140">Du måste hämta hello minut mått för analys.</span><span class="sxs-lookup"><span data-stu-id="781a5-140">You must download hello minute metrics for analysis.</span></span> <span data-ttu-id="781a5-141">hello tabeller visas inte om du lista alla hello tabeller i ditt lagringskonto, men du kan komma åt dem direkt efter namn.</span><span class="sxs-lookup"><span data-stu-id="781a5-141">hello tables do not appear if you list all hello tables in your storage account, but you can access them directly by name.</span></span> <span data-ttu-id="781a5-142">Många tredjeparts-lagring – bläddring verktyg är medveten om dessa tabeller och aktivera tooview dem direkt (finns i blogginlägget hello [lagringsutforskare för Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) en lista över tillgängliga verktyg).</span><span class="sxs-lookup"><span data-stu-id="781a5-142">Many third-party storage-browsing tools are aware of these tables and enable you tooview them directly (see hello blog post [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) for a list of available tools).</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="781a5-143">Varje timme mått</span><span class="sxs-lookup"><span data-stu-id="781a5-143">Hourly metrics</span></span>
* <span data-ttu-id="781a5-144">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="781a5-144">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="781a5-145">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="781a5-145">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="781a5-146">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="781a5-146">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="781a5-147">Minut mått</span><span class="sxs-lookup"><span data-stu-id="781a5-147">Minute metrics</span></span>
* <span data-ttu-id="781a5-148">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="781a5-148">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="781a5-149">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="781a5-149">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="781a5-150">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="781a5-150">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="781a5-151">Kapacitet</span><span class="sxs-lookup"><span data-stu-id="781a5-151">Capacity</span></span>
* <span data-ttu-id="781a5-152">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="781a5-152">$MetricsCapacityBlob</span></span>

<span data-ttu-id="781a5-153">Du kan hitta fullständig information om hello scheman för dessa tabeller på [Storage Analytics mätvärden tabellschemat](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="781a5-153">You can find full details of hello schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="781a5-154">hello exempel raderna nedan visar endast en delmängd av hello tillgängliga kolumner, men illustrera vissa viktiga funktioner i hello sätt Storage-mätvärden sparar de här måtten:</span><span class="sxs-lookup"><span data-stu-id="781a5-154">hello sample rows below show only a subset of hello columns available, but illustrate some important features of hello way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="781a5-155">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="781a5-155">PartitionKey</span></span> | <span data-ttu-id="781a5-156">RowKey</span><span class="sxs-lookup"><span data-stu-id="781a5-156">RowKey</span></span> | <span data-ttu-id="781a5-157">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="781a5-157">Timestamp</span></span> | <span data-ttu-id="781a5-158">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="781a5-158">TotalRequests</span></span> | <span data-ttu-id="781a5-159">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="781a5-159">TotalBillableRequests</span></span> | <span data-ttu-id="781a5-160">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="781a5-160">TotalIngress</span></span> | <span data-ttu-id="781a5-161">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="781a5-161">TotalEgress</span></span> | <span data-ttu-id="781a5-162">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="781a5-162">Availability</span></span> | <span data-ttu-id="781a5-163">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="781a5-163">AverageE2ELatency</span></span> | <span data-ttu-id="781a5-164">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="781a5-164">AverageServerLatency</span></span> | <span data-ttu-id="781a5-165">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="781a5-165">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="781a5-166">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="781a5-166">20140522T1100</span></span> |<span data-ttu-id="781a5-167">användaren. Alla</span><span class="sxs-lookup"><span data-stu-id="781a5-167">user;All</span></span> |<span data-ttu-id="781a5-168">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="781a5-168">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="781a5-169">7</span><span class="sxs-lookup"><span data-stu-id="781a5-169">7</span></span> |<span data-ttu-id="781a5-170">7</span><span class="sxs-lookup"><span data-stu-id="781a5-170">7</span></span> |<span data-ttu-id="781a5-171">4003</span><span class="sxs-lookup"><span data-stu-id="781a5-171">4003</span></span> |<span data-ttu-id="781a5-172">46801</span><span class="sxs-lookup"><span data-stu-id="781a5-172">46801</span></span> |<span data-ttu-id="781a5-173">100</span><span class="sxs-lookup"><span data-stu-id="781a5-173">100</span></span> |<span data-ttu-id="781a5-174">104.4286</span><span class="sxs-lookup"><span data-stu-id="781a5-174">104.4286</span></span> |<span data-ttu-id="781a5-175">6.857143</span><span class="sxs-lookup"><span data-stu-id="781a5-175">6.857143</span></span> |<span data-ttu-id="781a5-176">100</span><span class="sxs-lookup"><span data-stu-id="781a5-176">100</span></span> |
| <span data-ttu-id="781a5-177">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="781a5-177">20140522T1100</span></span> |<span data-ttu-id="781a5-178">användaren. QueryEntities</span><span class="sxs-lookup"><span data-stu-id="781a5-178">user;QueryEntities</span></span> |<span data-ttu-id="781a5-179">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="781a5-179">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="781a5-180">5</span><span class="sxs-lookup"><span data-stu-id="781a5-180">5</span></span> |<span data-ttu-id="781a5-181">5</span><span class="sxs-lookup"><span data-stu-id="781a5-181">5</span></span> |<span data-ttu-id="781a5-182">2694</span><span class="sxs-lookup"><span data-stu-id="781a5-182">2694</span></span> |<span data-ttu-id="781a5-183">45951</span><span class="sxs-lookup"><span data-stu-id="781a5-183">45951</span></span> |<span data-ttu-id="781a5-184">100</span><span class="sxs-lookup"><span data-stu-id="781a5-184">100</span></span> |<span data-ttu-id="781a5-185">143.8</span><span class="sxs-lookup"><span data-stu-id="781a5-185">143.8</span></span> |<span data-ttu-id="781a5-186">7.8</span><span class="sxs-lookup"><span data-stu-id="781a5-186">7.8</span></span> |<span data-ttu-id="781a5-187">100</span><span class="sxs-lookup"><span data-stu-id="781a5-187">100</span></span> |
| <span data-ttu-id="781a5-188">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="781a5-188">20140522T1100</span></span> |<span data-ttu-id="781a5-189">användaren. QueryEntity</span><span class="sxs-lookup"><span data-stu-id="781a5-189">user;QueryEntity</span></span> |<span data-ttu-id="781a5-190">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="781a5-190">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="781a5-191">1</span><span class="sxs-lookup"><span data-stu-id="781a5-191">1</span></span> |<span data-ttu-id="781a5-192">1</span><span class="sxs-lookup"><span data-stu-id="781a5-192">1</span></span> |<span data-ttu-id="781a5-193">538</span><span class="sxs-lookup"><span data-stu-id="781a5-193">538</span></span> |<span data-ttu-id="781a5-194">633</span><span class="sxs-lookup"><span data-stu-id="781a5-194">633</span></span> |<span data-ttu-id="781a5-195">100</span><span class="sxs-lookup"><span data-stu-id="781a5-195">100</span></span> |<span data-ttu-id="781a5-196">3</span><span class="sxs-lookup"><span data-stu-id="781a5-196">3</span></span> |<span data-ttu-id="781a5-197">3</span><span class="sxs-lookup"><span data-stu-id="781a5-197">3</span></span> |<span data-ttu-id="781a5-198">100</span><span class="sxs-lookup"><span data-stu-id="781a5-198">100</span></span> |
| <span data-ttu-id="781a5-199">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="781a5-199">20140522T1100</span></span> |<span data-ttu-id="781a5-200">användaren. UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="781a5-200">user;UpdateEntity</span></span> |<span data-ttu-id="781a5-201">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="781a5-201">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="781a5-202">1</span><span class="sxs-lookup"><span data-stu-id="781a5-202">1</span></span> |<span data-ttu-id="781a5-203">1</span><span class="sxs-lookup"><span data-stu-id="781a5-203">1</span></span> |<span data-ttu-id="781a5-204">771</span><span class="sxs-lookup"><span data-stu-id="781a5-204">771</span></span> |<span data-ttu-id="781a5-205">217</span><span class="sxs-lookup"><span data-stu-id="781a5-205">217</span></span> |<span data-ttu-id="781a5-206">100</span><span class="sxs-lookup"><span data-stu-id="781a5-206">100</span></span> |<span data-ttu-id="781a5-207">9</span><span class="sxs-lookup"><span data-stu-id="781a5-207">9</span></span> |<span data-ttu-id="781a5-208">6</span><span class="sxs-lookup"><span data-stu-id="781a5-208">6</span></span> |<span data-ttu-id="781a5-209">100</span><span class="sxs-lookup"><span data-stu-id="781a5-209">100</span></span> |

<span data-ttu-id="781a5-210">I det här exemplet minut mätvärdesdata kan använder hello partitionsnyckel hello tid i minuter upplösning.</span><span class="sxs-lookup"><span data-stu-id="781a5-210">In this example minute metrics data, hello partition key uses hello time at minute resolution.</span></span> <span data-ttu-id="781a5-211">Hej radnyckel identifierar hello typ av information som lagras i hello raden och detta består av två delar av information, hello åtkomsttyp och hello Begärandetypen:</span><span class="sxs-lookup"><span data-stu-id="781a5-211">hello row key identifies hello type of information that is stored in hello row and this is composed of two pieces of information, hello access type, and hello request type:</span></span>

* <span data-ttu-id="781a5-212">hello åtkomsttypen är användaren eller systemet, där användaren refererar tooall användaren begäranden toohello storage-tjänsten och system refererar toorequests som gjorts av Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="781a5-212">hello access type is either user or system, where user refers tooall user requests toohello storage service, and system refers toorequests made by Storage Analytics.</span></span>
* <span data-ttu-id="781a5-213">hello Begärandetypen är alla i så fall är det en sammanfattande rad eller identifierar hello specifika API: et, till exempel QueryEntity eller UpdateEntity.</span><span class="sxs-lookup"><span data-stu-id="781a5-213">hello request type is either all in which case it is a summary line, or it identifies hello specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="781a5-214">hello exempeldata ovan visas alla hello registrerar för en minut (börjar på 11:00:00), så hello antalet QueryEntities förfrågningar plus hello antal QueryEntity begäranden plus hello antalet UpdateEntity begäranden summera tooseven som är hello totala visas på hello användare: All raden.</span><span class="sxs-lookup"><span data-stu-id="781a5-214">hello sample data above shows all hello records for a single minute (starting at 11:00AM), so hello number of QueryEntities requests plus hello number of QueryEntity requests plus hello number of UpdateEntity requests add up tooseven, which is hello total shown on hello user:All row.</span></span> <span data-ttu-id="781a5-215">Du kan dessutom härleda hello slutpunkt till slutpunkt Medelsvarstid 104.4286 på hello användare: All rad genom en beräkning ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="781a5-215">Similarly, you can derive hello average end-to-end latency 104.4286 on hello user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

<span data-ttu-id="781a5-216">Bör du ställa in aviseringar i hello klassiska Azure-portalen på hello övervakaren sidan så att Storage-mätvärden automatiskt meddela dig om några viktiga ändringar i hello beteendet för storage-tjänster. Om du använder en lagring explorer verktyget toodownload informationen mått i en avgränsad format kan du använda Microsoft Excel tooanalyze hello-data.</span><span class="sxs-lookup"><span data-stu-id="781a5-216">You should consider setting up alerts in hello Azure Classic Portal on hello Monitor page so that Storage Metrics can automatically notify you of any important changes in hello behavior of your storage services.If you use a storage explorer tool toodownload this metrics data in a delimited format, you can use Microsoft Excel tooanalyze hello data.</span></span> <span data-ttu-id="781a5-217">Finns i blogginlägget hello [lagringsutforskare för Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) för en lista över tillgängliga lagringsutrymmet explorer verktyg.</span><span class="sxs-lookup"><span data-stu-id="781a5-217">See hello blog post [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) for a list of available storage explorer tools.</span></span>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="781a5-218">Dataåtkomst mätvärden via programmering</span><span class="sxs-lookup"><span data-stu-id="781a5-218">Accessing metrics data programmatically</span></span>
<span data-ttu-id="781a5-219">hello visar nedan exempel C#-kod som har åtkomst till hello minut mått för ett antal minuter och visar hello resultaten i ett konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="781a5-219">hello following listing shows sample C# code that accesses hello minute metrics for a range of minutes and displays hello results in a console Window.</span></span> <span data-ttu-id="781a5-220">Den använder hello Azure Storage-biblioteket version 4 som innehåller hello CloudAnalyticsClient klass som förenklar använder hello mått tabeller i lagringen.</span><span class="sxs-lookup"><span data-stu-id="781a5-220">It uses hello Azure Storage Library version 4 that includes hello CloudAnalyticsClient class that simplifies accessing hello metrics tables in storage.</span></span>

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert hello dates toohello format used in hello PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} too{2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using hello entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in hello MetricsEntity class.
          // hello PartitionKey identifies hello DataTime of hello metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
        select entity;

        // Filter on "user" transactions after fetching hello metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="781a5-221">Vilka avgifter gör uppkommer när du aktiverar lagring mått?</span><span class="sxs-lookup"><span data-stu-id="781a5-221">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="781a5-222">Skriva begäranden toocreate tabellentiteter för mätvärden debiteras hello standardpriser gäller tooall Azure Storage-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="781a5-222">Write requests toocreate table entities for metrics are charged at hello standard rates applicable tooall Azure Storage operations.</span></span>

<span data-ttu-id="781a5-223">Förfrågningar om läsning och ta bort en klient toometrics data är också fakturerbar på standardpriser.</span><span class="sxs-lookup"><span data-stu-id="781a5-223">Read and delete requests by a client toometrics data are also billable at standard rates.</span></span> <span data-ttu-id="781a5-224">Om du har konfigurerat en databevarandeprincip debiteras du inte när Azure Storage tar bort den gamla mått data.</span><span class="sxs-lookup"><span data-stu-id="781a5-224">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="781a5-225">Om du tar bort analysdata debiteras ditt konto för hello delete-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="781a5-225">However, if you delete analytics data, your account is charged for hello delete operations.</span></span>

<span data-ttu-id="781a5-226">hello-kapaciteten som används av hello mått tabeller är också fakturerbar: du kan använda hello följande tooestimate hello storleken på den kapacitet som används för att lagra data för mått:</span><span class="sxs-lookup"><span data-stu-id="781a5-226">hello capacity used by hello metrics tables is also billable: you can use hello following tooestimate hello amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="781a5-227">Om varje timme en tjänst använder varje API i varje tjänst, är ungefär 148KB data lagrade varje timme i hello mått transaktion tabellerna om du har aktiverat både tjänsten och översikt över API-nivå.</span><span class="sxs-lookup"><span data-stu-id="781a5-227">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in hello metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="781a5-228">Om varje timme en tjänst använder varje API i varje tjänst, är cirka 12KB data lagrade varje timme i hello mått transaktion tabellerna om du har aktiverat bara servicenivå som är sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="781a5-228">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in hello metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="781a5-229">hello kapacitet för BLOB har två rader läggs varje dag (förutsatt att användaren har valt i loggar): Detta innebär att varje dag hello ökar storleken på den här tabellen genom in tooapproximately 300 byte.</span><span class="sxs-lookup"><span data-stu-id="781a5-229">hello capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day hello size of this table increases by up tooapproximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="781a5-230">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="781a5-230">Next steps:</span></span>
[<span data-ttu-id="781a5-231">Aktivera Storage Analytics loggning och åtkomst till loggdata</span><span class="sxs-lookup"><span data-stu-id="781a5-231">Enabling Storage Analytics Logging and Accessing Log Data</span></span>](https://msdn.microsoft.com/library/dn782840.aspx)
