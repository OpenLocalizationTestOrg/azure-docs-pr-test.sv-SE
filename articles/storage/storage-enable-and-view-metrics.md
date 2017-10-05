---
title: "Aktivera storage-mätvärden i Azure portal | Microsoft Docs"
description: "Så här aktiverar du storage-mätvärden för Blob, kön, tabell och filen tjänster"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0407adfc-2a41-4126-922d-b76e90b74563
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: robinsh
ms.openlocfilehash: 2906f808482d0b990e3ddd31ef5368e9fdd9f646
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="e89d6-103">Aktivera Azure Storage-mätvärden och visa mått data</span><span class="sxs-lookup"><span data-stu-id="e89d6-103">Enabling Azure Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="e89d6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e89d6-104">Overview</span></span>
<span data-ttu-id="e89d6-105">Storage-mätvärden är aktiverad som standard när du skapar ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e89d6-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="e89d6-106">Du kan konfigurera övervakning den [Azure-portalen](https://portal.azure.com) eller Windows PowerShell eller programmässigt via en lagringsklientbiblioteken.</span><span class="sxs-lookup"><span data-stu-id="e89d6-106">You can configure monitoring via the [Azure portal](https://portal.azure.com) or Windows PowerShell, or programmatically via one of the storage client libraries.</span></span>

<span data-ttu-id="e89d6-107">Du kan konfigurera en kvarhållningsperiod för metrics data: den här perioden anger hur länge lagring tjänsten håller mått och avgifter som du för utrymmet krävs för att lagra dem.</span><span class="sxs-lookup"><span data-stu-id="e89d6-107">You can configure a retention period for the metrics data: this period determines for how long the storage service keeps the metrics and charges you for the space required to store them.</span></span> <span data-ttu-id="e89d6-108">Du bör normalt använda en kortare period för minut mått än timvis mått på grund av betydande extra utrymme som krävs för minut mått.</span><span class="sxs-lookup"><span data-stu-id="e89d6-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of the significant extra space required for minute metrics.</span></span> <span data-ttu-id="e89d6-109">Du bör välja en kvarhållningsperiod så att du har tillräckligt med tid att analysera data och hämta några mått som du vill behålla för offline-analys och rapportering.</span><span class="sxs-lookup"><span data-stu-id="e89d6-109">You should choose a retention period such that you have sufficient time to analyze the data and download any metrics you wish to keep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="e89d6-110">Kom ihåg att du också kommer att debiteras för att ladda ned mått data från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e89d6-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-to-enable-metrics-using-the-azure-portal"></a><span data-ttu-id="e89d6-111">Så här aktiverar du mått med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e89d6-111">How to enable metrics using the Azure portal</span></span>
<span data-ttu-id="e89d6-112">Följ dessa steg om du vill aktivera mätvärden i den [Azure-portalen](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="e89d6-112">Follow these steps to enable metrics in the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="e89d6-113">Navigera till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e89d6-113">Navigate to your storage account.</span></span>
1. <span data-ttu-id="e89d6-114">Välj **diagnostik** på den **menyn** bladet</span><span class="sxs-lookup"><span data-stu-id="e89d6-114">Select **Diagnostics** on the **Menu** blade</span></span>
1. <span data-ttu-id="e89d6-115">Se till att **Status** är inställd på **på**.</span><span class="sxs-lookup"><span data-stu-id="e89d6-115">Ensure that **Status** is set to **On**.</span></span>
1. <span data-ttu-id="e89d6-116">Välj mått för de tjänster som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="e89d6-116">Select the metrics for the services you wish to monitor.</span></span>
1. <span data-ttu-id="e89d6-117">Ange en bevarandeprincip som anger hur lång tid att behålla mått och logga data.</span><span class="sxs-lookup"><span data-stu-id="e89d6-117">Specify a retention policy to indicate how long to retain metrics and log data.</span></span>
1. <span data-ttu-id="e89d6-118">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e89d6-118">Select **Save**.</span></span>

<span data-ttu-id="e89d6-119">Observera att den [Azure-portalen](https://portal.azure.com) för närvarande kan du inte konfigurera minut mått i ditt lagringskonto, måste du aktivera minut mått med PowerShell eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="e89d6-119">Note that the [Azure portal](https://portal.azure.com) does not currently enable you to configure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-to-enable-metrics-using-powershell"></a><span data-ttu-id="e89d6-120">Så här aktiverar du mått med PowerShell</span><span class="sxs-lookup"><span data-stu-id="e89d6-120">How to enable metrics using PowerShell</span></span>
<span data-ttu-id="e89d6-121">Du kan använda PowerShell på den lokala datorn för att konfigurera måtten för lagring i ditt lagringskonto med hjälp av Azure PowerShell-cmdleten Get-AzureStorageServiceMetricsProperty för att hämta de aktuella inställningarna och cmdlet Set-AzureStorageServiceMetricsProperty för att ändra de aktuella inställningarna.</span><span class="sxs-lookup"><span data-stu-id="e89d6-121">You can use PowerShell on your local machine to configure Storage Metrics in your storage account by using the Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty to retrieve the current settings, and the cmdlet Set-AzureStorageServiceMetricsProperty to change the current settings.</span></span>

<span data-ttu-id="e89d6-122">Cmdletar som styr Storage-mätvärden använder följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="e89d6-122">The cmdlets that control Storage Metrics use the following parameters:</span></span>

* <span data-ttu-id="e89d6-123">MetricsType: möjliga värden är timmar och minuter.</span><span class="sxs-lookup"><span data-stu-id="e89d6-123">MetricsType: possible values are Hour and Minute.</span></span>
* <span data-ttu-id="e89d6-124">ServiceType: möjliga värden är Blob, köer och tabellen.</span><span class="sxs-lookup"><span data-stu-id="e89d6-124">ServiceType: possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="e89d6-125">MetricsLevel: möjliga värden är None, tjänst och ServiceAndApi.</span><span class="sxs-lookup"><span data-stu-id="e89d6-125">MetricsLevel: possible values are None, Service, and ServiceAndApi.</span></span>

<span data-ttu-id="e89d6-126">Till exempel växlar följande kommando på minut mätvärden för Blob-tjänsten i standardkontot för lagring med kvarhållning tidsperiod som angetts till fem dagar:</span><span class="sxs-lookup"><span data-stu-id="e89d6-126">For example, the following command switches on minute metrics for the Blob service in your default storage account with the retention period set to five days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

<span data-ttu-id="e89d6-127">Följande kommando hämtar de aktuella timvis mått nivå och kvarhållning dagarna för blob-tjänsten i standardkontot för lagring:</span><span class="sxs-lookup"><span data-stu-id="e89d6-127">The following command retrieves the current hourly metrics level and retention days for the blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

<span data-ttu-id="e89d6-128">Information om hur du konfigurerar Azure PowerShell-cmdlets för att arbeta med din Azure-prenumeration och hur du väljer standardkontot för lagring ska användas, finns: [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e89d6-128">For information about how to configure the Azure PowerShell cmdlets to work with your Azure subscription and how to select the default storage account to use, see: [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-to-enable-storage-metrics-programmatically"></a><span data-ttu-id="e89d6-129">Så här aktiverar du Storage-mätvärden via programmering</span><span class="sxs-lookup"><span data-stu-id="e89d6-129">How to enable Storage metrics programmatically</span></span>
<span data-ttu-id="e89d6-130">Följande C# utdrag visar hur du aktiverar loggning för Blob-tjänsten med storage-klientbiblioteket för .NET- och mått:</span><span class="sxs-lookup"><span data-stu-id="e89d6-130">The following C# snippet shows how to enable metrics and logging for the Blob service using the storage client library for .NET:</span></span>

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy to 10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at the same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set the default service version to be used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set the service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a><span data-ttu-id="e89d6-131">Visa Storage-mätvärden</span><span class="sxs-lookup"><span data-stu-id="e89d6-131">Viewing Storage metrics</span></span>
<span data-ttu-id="e89d6-132">När du har konfigurerat Storage Analytics mätvärden för att övervaka ditt lagringskonto registrerar Storage Analytics mätvärden i en uppsättning välkända tabeller i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e89d6-132">After you configure Storage Analytics metrics to monitor your storage account, Storage Analytics records the metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="e89d6-133">Du kan konfigurera diagram om du vill visa timvis mått i den [Azure-portalen](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="e89d6-133">You can configure charts to view hourly metrics in the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="e89d6-134">Navigera till ditt lagringskonto i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e89d6-134">Navigate to your storage account in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="e89d6-135">Välj **mått** i den **menyn** bladet för tjänsten vars mått som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="e89d6-135">Select **Metrics** in the **Menu** blade for the service whose metrics you want to view.</span></span>
1. <span data-ttu-id="e89d6-136">Välj **redigera** på diagrammet som du vill konfigurera.</span><span class="sxs-lookup"><span data-stu-id="e89d6-136">Select **Edit** on the chart you want to configure.</span></span>
1. <span data-ttu-id="e89d6-137">I den **redigera diagram** bladet väljer den **tidsintervall**, **diagramtypen**, och det mått som ska visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="e89d6-137">In the **Edit Chart** blade, select the **Time Range**, **Chart type**, and the metrics you want displayed in the chart.</span></span>
1. <span data-ttu-id="e89d6-138">Välj **OK**</span><span class="sxs-lookup"><span data-stu-id="e89d6-138">Select **OK**</span></span>

<span data-ttu-id="e89d6-139">Om du vill hämta mätvärden för långsiktig lagring eller analysera dem lokalt, behöver du:</span><span class="sxs-lookup"><span data-stu-id="e89d6-139">If you want to download the metrics for long-term storage or to analyze them locally, you will need to:</span></span>

* <span data-ttu-id="e89d6-140">Använd ett verktyg som känner till dessa tabeller och gör att du kan visa och ladda ned dem.</span><span class="sxs-lookup"><span data-stu-id="e89d6-140">Use a tool that is aware of these tables and will allow you to view and download them.</span></span>
* <span data-ttu-id="e89d6-141">Skriv ett anpassat program eller skript för att läsa och lagra tabeller.</span><span class="sxs-lookup"><span data-stu-id="e89d6-141">Write a custom application or script to read and store the tables.</span></span>

<span data-ttu-id="e89d6-142">Surfa på storage-verktyg för många från tredje part är medveten om dessa tabeller och gör att du kan visa dem direkt.</span><span class="sxs-lookup"><span data-stu-id="e89d6-142">Many third-party storage-browsing tools are aware of these tables and enable you to view them directly.</span></span>
<span data-ttu-id="e89d6-143">Se [Azure Storage-klientverktyg](storage-explorers.md) en lista över tillgängliga verktyg.</span><span class="sxs-lookup"><span data-stu-id="e89d6-143">Please see [Azure Storage Client Tools](storage-explorers.md) for a list of available tools.</span></span>

> [!NOTE]
> <span data-ttu-id="e89d6-144">Från och med version 0.8.0 av den [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/), kan du visa och hämta analytics mätvärden tabeller.</span><span class="sxs-lookup"><span data-stu-id="e89d6-144">Starting with version 0.8.0 of the [Microsoft Azure Storage Explorer](http://storageexplorer.com/), you can view and download the analytics metrics tables.</span></span>
> 
> 

<span data-ttu-id="e89d6-145">För att komma åt tabellerna analytics programmässigt Observera analytics tabeller inte visas om du en lista över alla tabeller i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e89d6-145">In order to access the analytics tables programmatically, do note that the analytics tables do not appear if you list all the tables in your storage account.</span></span> <span data-ttu-id="e89d6-146">Du kan antingen komma åt dem direkt efter namn eller använda den [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) i .NET-klientbibliotek för att fråga tabellnamnen.</span><span class="sxs-lookup"><span data-stu-id="e89d6-146">You can either access them directly by name, or use the [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in the .NET client library to query the table names.</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="e89d6-147">Varje timme mått</span><span class="sxs-lookup"><span data-stu-id="e89d6-147">Hourly metrics</span></span>
* <span data-ttu-id="e89d6-148">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="e89d6-148">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="e89d6-149">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="e89d6-149">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="e89d6-150">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="e89d6-150">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="e89d6-151">Minut mått</span><span class="sxs-lookup"><span data-stu-id="e89d6-151">Minute metrics</span></span>
* <span data-ttu-id="e89d6-152">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="e89d6-152">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="e89d6-153">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="e89d6-153">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="e89d6-154">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="e89d6-154">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="e89d6-155">Kapacitet</span><span class="sxs-lookup"><span data-stu-id="e89d6-155">Capacity</span></span>
* <span data-ttu-id="e89d6-156">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="e89d6-156">$MetricsCapacityBlob</span></span>

<span data-ttu-id="e89d6-157">Du kan hitta fullständig information om scheman för dessa tabeller på [Storage Analytics mätvärden tabellschemat](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="e89d6-157">You can find full details of the schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="e89d6-158">Raderna exemplet nedan visar endast en delmängd av kolumnerna som är tillgängliga, men illustrera vissa viktiga funktioner av Storage-mätvärden sparar dessa mått:</span><span class="sxs-lookup"><span data-stu-id="e89d6-158">The sample rows below show only a subset of the columns available, but illustrate some important features of the way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="e89d6-159">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="e89d6-159">PartitionKey</span></span> | <span data-ttu-id="e89d6-160">RowKey</span><span class="sxs-lookup"><span data-stu-id="e89d6-160">RowKey</span></span> | <span data-ttu-id="e89d6-161">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="e89d6-161">Timestamp</span></span> | <span data-ttu-id="e89d6-162">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="e89d6-162">TotalRequests</span></span> | <span data-ttu-id="e89d6-163">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="e89d6-163">TotalBillableRequests</span></span> | <span data-ttu-id="e89d6-164">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="e89d6-164">TotalIngress</span></span> | <span data-ttu-id="e89d6-165">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="e89d6-165">TotalEgress</span></span> | <span data-ttu-id="e89d6-166">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="e89d6-166">Availability</span></span> | <span data-ttu-id="e89d6-167">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="e89d6-167">AverageE2ELatency</span></span> | <span data-ttu-id="e89d6-168">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="e89d6-168">AverageServerLatency</span></span> | <span data-ttu-id="e89d6-169">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="e89d6-169">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="e89d6-170">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e89d6-170">20140522T1100</span></span> |<span data-ttu-id="e89d6-171">användaren. Alla</span><span class="sxs-lookup"><span data-stu-id="e89d6-171">user;All</span></span> |<span data-ttu-id="e89d6-172">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="e89d6-172">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="e89d6-173">7</span><span class="sxs-lookup"><span data-stu-id="e89d6-173">7</span></span> |<span data-ttu-id="e89d6-174">7</span><span class="sxs-lookup"><span data-stu-id="e89d6-174">7</span></span> |<span data-ttu-id="e89d6-175">4003</span><span class="sxs-lookup"><span data-stu-id="e89d6-175">4003</span></span> |<span data-ttu-id="e89d6-176">46801</span><span class="sxs-lookup"><span data-stu-id="e89d6-176">46801</span></span> |<span data-ttu-id="e89d6-177">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-177">100</span></span> |<span data-ttu-id="e89d6-178">104.4286</span><span class="sxs-lookup"><span data-stu-id="e89d6-178">104.4286</span></span> |<span data-ttu-id="e89d6-179">6.857143</span><span class="sxs-lookup"><span data-stu-id="e89d6-179">6.857143</span></span> |<span data-ttu-id="e89d6-180">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-180">100</span></span> |
| <span data-ttu-id="e89d6-181">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e89d6-181">20140522T1100</span></span> |<span data-ttu-id="e89d6-182">användaren. QueryEntities</span><span class="sxs-lookup"><span data-stu-id="e89d6-182">user;QueryEntities</span></span> |<span data-ttu-id="e89d6-183">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="e89d6-183">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="e89d6-184">5</span><span class="sxs-lookup"><span data-stu-id="e89d6-184">5</span></span> |<span data-ttu-id="e89d6-185">5</span><span class="sxs-lookup"><span data-stu-id="e89d6-185">5</span></span> |<span data-ttu-id="e89d6-186">2694</span><span class="sxs-lookup"><span data-stu-id="e89d6-186">2694</span></span> |<span data-ttu-id="e89d6-187">45951</span><span class="sxs-lookup"><span data-stu-id="e89d6-187">45951</span></span> |<span data-ttu-id="e89d6-188">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-188">100</span></span> |<span data-ttu-id="e89d6-189">143.8</span><span class="sxs-lookup"><span data-stu-id="e89d6-189">143.8</span></span> |<span data-ttu-id="e89d6-190">7.8</span><span class="sxs-lookup"><span data-stu-id="e89d6-190">7.8</span></span> |<span data-ttu-id="e89d6-191">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-191">100</span></span> |
| <span data-ttu-id="e89d6-192">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e89d6-192">20140522T1100</span></span> |<span data-ttu-id="e89d6-193">användaren. QueryEntity</span><span class="sxs-lookup"><span data-stu-id="e89d6-193">user;QueryEntity</span></span> |<span data-ttu-id="e89d6-194">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="e89d6-194">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="e89d6-195">1</span><span class="sxs-lookup"><span data-stu-id="e89d6-195">1</span></span> |<span data-ttu-id="e89d6-196">1</span><span class="sxs-lookup"><span data-stu-id="e89d6-196">1</span></span> |<span data-ttu-id="e89d6-197">538</span><span class="sxs-lookup"><span data-stu-id="e89d6-197">538</span></span> |<span data-ttu-id="e89d6-198">633</span><span class="sxs-lookup"><span data-stu-id="e89d6-198">633</span></span> |<span data-ttu-id="e89d6-199">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-199">100</span></span> |<span data-ttu-id="e89d6-200">3</span><span class="sxs-lookup"><span data-stu-id="e89d6-200">3</span></span> |<span data-ttu-id="e89d6-201">3</span><span class="sxs-lookup"><span data-stu-id="e89d6-201">3</span></span> |<span data-ttu-id="e89d6-202">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-202">100</span></span> |
| <span data-ttu-id="e89d6-203">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e89d6-203">20140522T1100</span></span> |<span data-ttu-id="e89d6-204">användaren. UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="e89d6-204">user;UpdateEntity</span></span> |<span data-ttu-id="e89d6-205">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="e89d6-205">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="e89d6-206">1</span><span class="sxs-lookup"><span data-stu-id="e89d6-206">1</span></span> |<span data-ttu-id="e89d6-207">1</span><span class="sxs-lookup"><span data-stu-id="e89d6-207">1</span></span> |<span data-ttu-id="e89d6-208">771</span><span class="sxs-lookup"><span data-stu-id="e89d6-208">771</span></span> |<span data-ttu-id="e89d6-209">217</span><span class="sxs-lookup"><span data-stu-id="e89d6-209">217</span></span> |<span data-ttu-id="e89d6-210">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-210">100</span></span> |<span data-ttu-id="e89d6-211">9</span><span class="sxs-lookup"><span data-stu-id="e89d6-211">9</span></span> |<span data-ttu-id="e89d6-212">6</span><span class="sxs-lookup"><span data-stu-id="e89d6-212">6</span></span> |<span data-ttu-id="e89d6-213">100</span><span class="sxs-lookup"><span data-stu-id="e89d6-213">100</span></span> |

<span data-ttu-id="e89d6-214">I det här exemplet minut mätvärdesdata kan används Partitionsnyckeln vid matchning av minut.</span><span class="sxs-lookup"><span data-stu-id="e89d6-214">In this example minute metrics data, the partition key uses the time at minute resolution.</span></span> <span data-ttu-id="e89d6-215">Radnyckeln identifierar typ av information som lagras på raden och detta består av två delar av information, vilken åtkomsttyp och typ av begäran:</span><span class="sxs-lookup"><span data-stu-id="e89d6-215">The row key identifies the type of information that is stored in the row and this is composed of two pieces of information, the access type, and the request type:</span></span>

* <span data-ttu-id="e89d6-216">Åtkomsttypen är användaren eller systemet, där användaren avser alla användarförfrågningar till storage-tjänsten och system avser förfrågningar som görs av Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="e89d6-216">The access type is either user or system, where user refers to all user requests to the storage service, and system refers to requests made by Storage Analytics.</span></span>
* <span data-ttu-id="e89d6-217">Typ av begäran är antingen i vilket fall det är en sammanfattande rad eller identifierar det specifika API: et, till exempel QueryEntity eller UpdateEntity.</span><span class="sxs-lookup"><span data-stu-id="e89d6-217">The request type is either all in which case it is a summary line, or it identifies the specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="e89d6-218">Exempeldata ovan visar alla poster för en minut (börjar på 11:00:00), så att antalet begäranden som QueryEntities plus antalet QueryEntity begär plus antalet UpdateEntity manuellt lägga till upp till sju, vilket är det totala antalet visas på raden för användaren: All.</span><span class="sxs-lookup"><span data-stu-id="e89d6-218">The sample data above shows all the records for a single minute (starting at 11:00AM), so the number of QueryEntities requests plus the number of QueryEntity requests plus the number of UpdateEntity requests add up to seven, which is the total shown on the user:All row.</span></span> <span data-ttu-id="e89d6-219">Du kan dessutom härleda slutpunkt till slutpunkt genomsnittlig svarstid 104.4286 på raden för användaren: All genom en beräkning ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="e89d6-219">Similarly, you can derive the average end-to-end latency 104.4286 on the user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

## <a name="metrics-alerts"></a><span data-ttu-id="e89d6-220">Mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="e89d6-220">Metrics alerts</span></span>
<span data-ttu-id="e89d6-221">Du bör du överväga att konfigurera aviseringar i den [Azure-portalen](https://portal.azure.com) så Storage-mätvärden automatiskt meddela dig om viktiga ändringar i beteendet för storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e89d6-221">You should consider setting up alerts in the [Azure portal](https://portal.azure.com) so Storage Metrics can automatically notify you of important changes in the behavior of your storage services.</span></span> <span data-ttu-id="e89d6-222">Om du använder ett verktyg för lagring explorer för att hämta informationen mått i en avgränsad format kan du använda Microsoft Excel för att analysera data.</span><span class="sxs-lookup"><span data-stu-id="e89d6-222">If you use a storage explorer tool to download this metrics data in a delimited format, you can use Microsoft Excel to analyze the data.</span></span> <span data-ttu-id="e89d6-223">Se [Azure Storage-klientverktyg](storage-explorers.md) för en lista över tillgängliga lagringsutrymmet explorer verktyg.</span><span class="sxs-lookup"><span data-stu-id="e89d6-223">See [Azure Storage Client Tools](storage-explorers.md) for a list of available storage explorer tools.</span></span> <span data-ttu-id="e89d6-224">Du kan konfigurera aviseringar i den **Varna regler** bladet tillgänglig under **övervakning** i bladet Storage-konto-menyn.</span><span class="sxs-lookup"><span data-stu-id="e89d6-224">You can configure alerts in the **Alert rules** blade, accessible under **Monitoring** in the Storage account menu blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e89d6-225">Det kan finnas en fördröjning mellan en händelse för lagring och när motsvarande timvis eller minut mätvärdesdata registreras.</span><span class="sxs-lookup"><span data-stu-id="e89d6-225">There may be a delay between a storage event and when the corresponding hourly or minute metrics data is recorded.</span></span> <span data-ttu-id="e89d6-226">När det gäller minut mått kan flera minuter av data skrivas på samma gång.</span><span class="sxs-lookup"><span data-stu-id="e89d6-226">In the case of minute metrics, several minutes of data may be written at once.</span></span> <span data-ttu-id="e89d6-227">Detta kan leda till transaktioner från tidigare minuter som ska aggregeras i transaktionen för den aktuella minuten.</span><span class="sxs-lookup"><span data-stu-id="e89d6-227">This can lead to transactions from earlier minutes being aggregated into the transaction for the current minute.</span></span> <span data-ttu-id="e89d6-228">När detta inträffar kanske alert-tjänsten inte alla tillgängliga mått data för konfigurerade avisering intervall, vilket kan leda till oväntat startar aviseringar.</span><span class="sxs-lookup"><span data-stu-id="e89d6-228">When this happens, the alert service may not have all available metrics data for the configured alert interval, which may lead to alerts firing unexpectedly.</span></span>
>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="e89d6-229">Dataåtkomst mätvärden via programmering</span><span class="sxs-lookup"><span data-stu-id="e89d6-229">Accessing metrics data programmatically</span></span>
<span data-ttu-id="e89d6-230">Följande lista visar exempel C#-kod som har åtkomst till minut mått för ett antal minuter och visar resultatet i ett konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="e89d6-230">The following listing shows sample C# code that accesses the minute metrics for a range of minutes and displays the results in a console Window.</span></span> <span data-ttu-id="e89d6-231">Den använder Azure Storage-biblioteket version 4 som innehåller klassen CloudAnalyticsClient som gör det enklare att komma åt mått tabellerna i lagring.</span><span class="sxs-lookup"><span data-stu-id="e89d6-231">It uses the Azure Storage Library version 4 that includes the CloudAnalyticsClient class that simplifies accessing the metrics tables in storage.</span></span>

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in the MetricsEntity class.
          // The PartitionKey identifies the DataTime of the metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
        select entity;

        // Filter on "user" transactions after fetching the metrics from Table Storage.
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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="e89d6-232">Vilka avgifter gör uppkommer när du aktiverar lagring mått?</span><span class="sxs-lookup"><span data-stu-id="e89d6-232">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="e89d6-233">Skriva begäranden om att skapa tabellentiteter för mått debiteras enligt standardpriser gäller för alla Azure Storage-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e89d6-233">Write requests to create table entities for metrics are charged at the standard rates applicable to all Azure Storage operations.</span></span>

<span data-ttu-id="e89d6-234">Förfrågningar om läsning och borttagning av en klient till mätvärdesdata är också fakturerbar på standardpriser.</span><span class="sxs-lookup"><span data-stu-id="e89d6-234">Read and delete requests by a client to metrics data are also billable at standard rates.</span></span> <span data-ttu-id="e89d6-235">Om du har konfigurerat en databevarandeprincip debiteras du inte när Azure Storage tar bort den gamla mått data.</span><span class="sxs-lookup"><span data-stu-id="e89d6-235">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="e89d6-236">Om du tar bort analysdata debiteras ditt konto för delete-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e89d6-236">However, if you delete analytics data, your account is charged for the delete operations.</span></span>

<span data-ttu-id="e89d6-237">Den kapacitet som används av tabellerna mått är också fakturerbar: du kan använda följande för att beräkna storleken på den kapacitet som används för att lagra data för mått:</span><span class="sxs-lookup"><span data-stu-id="e89d6-237">The capacity used by the metrics tables is also billable: you can use the following to estimate the amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="e89d6-238">Om varje timme en tjänst använder varje API i varje tjänst, är cirka 148KB data lagrade i timmen i tabellerna mått transaktion om du har aktiverat både tjänsten och översikt över API-nivå.</span><span class="sxs-lookup"><span data-stu-id="e89d6-238">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in the metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="e89d6-239">Om varje timme en tjänst använder varje API i varje tjänst, är cirka 12KB data lagrade i timmen i tabellerna mått transaktion om du har aktiverat bara servicenivå som är sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="e89d6-239">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in the metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="e89d6-240">Tabellen kapacitet för BLOB har två rader läggs varje dag (förutsatt att användaren har valt i loggar): Detta innebär att varje dag ökar storleken på den här tabellen med upp till ungefär 300 byte.</span><span class="sxs-lookup"><span data-stu-id="e89d6-240">The capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day the size of this table increases by up to approximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e89d6-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e89d6-241">Next steps</span></span>
[<span data-ttu-id="e89d6-242">Aktivera loggning och åtkomst till loggdata</span><span class="sxs-lookup"><span data-stu-id="e89d6-242">Enabling Storage Logging and Accessing Log Data</span></span>](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
