---
title: aaaStore och visa diagnostiska Data i Azure Storage | Microsoft Docs
description: "Hämta Azure diagnostikdata till Azure Storage och visa den"
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: 18e0780d-43e7-41e4-b8e9-f1fb9a36eb03
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: robb
ms.openlocfilehash: dd47a2ef6d6488c80c102c72b2ebf6ca6d2e473f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a><span data-ttu-id="fdc7b-103">Lagra och visa diagnostiska data i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="fdc7b-103">Store and view diagnostic data in Azure Storage</span></span>
<span data-ttu-id="fdc7b-104">Diagnostikdata lagras inte permanent om du överför det toohello Microsoft Azure storage-emulatorn eller tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-104">Diagnostic data is not permanently stored unless you transfer it toohello Microsoft Azure storage emulator or tooAzure storage.</span></span> <span data-ttu-id="fdc7b-105">En gång i lagring, den kan visas med en av flera tillgängliga verktyg.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-105">Once in storage, it can be viewed with one of several available tools.</span></span>

## <a name="specify-a-storage-account"></a><span data-ttu-id="fdc7b-106">Ange ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="fdc7b-106">Specify a storage account</span></span>
<span data-ttu-id="fdc7b-107">Du kan ange hello storage-konto som du vill toouse i hello ServiceConfiguration.cscfg-filen.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-107">You specify hello storage account that you want toouse in hello ServiceConfiguration.cscfg file.</span></span> <span data-ttu-id="fdc7b-108">hello kontoinformation definieras som en anslutningssträng i en konfigurationsinställning.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-108">hello account information is defined as a connection string in a configuration setting.</span></span> <span data-ttu-id="fdc7b-109">hello visar följande exempel hello standardanslutningssträngen som skapats för ett nytt Cloud Service-projekt i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fdc7b-109">hello following example shows hello default connection string created for a new Cloud Service project in  Visual Studio:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

<span data-ttu-id="fdc7b-110">Du kan ändra den här tooprovide kontoinformationen i anslutningssträngen för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-110">You can change this connection string tooprovide account information for an Azure storage account.</span></span>

<span data-ttu-id="fdc7b-111">Beroende på hello typ av diagnostiska data som samlas in, använder Azure-diagnostik hello Blob-tjänsten eller hello tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-111">Depending on hello type of diagnostic data that is being collected, Azure Diagnostics uses either hello Blob service or hello Table service.</span></span> <span data-ttu-id="fdc7b-112">hello följande tabell visar hello datakällor som sparas och format.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-112">hello following table shows hello data sources that are persisted and their format.</span></span>

| <span data-ttu-id="fdc7b-113">Datakälla</span><span class="sxs-lookup"><span data-stu-id="fdc7b-113">Data source</span></span> | <span data-ttu-id="fdc7b-114">Lagringsformat</span><span class="sxs-lookup"><span data-stu-id="fdc7b-114">Storage format</span></span> |
| --- | --- |
| <span data-ttu-id="fdc7b-115">Azure-loggar</span><span class="sxs-lookup"><span data-stu-id="fdc7b-115">Azure logs</span></span> |<span data-ttu-id="fdc7b-116">Tabell</span><span class="sxs-lookup"><span data-stu-id="fdc7b-116">Table</span></span> |
| <span data-ttu-id="fdc7b-117">IIS 7.0-loggar</span><span class="sxs-lookup"><span data-stu-id="fdc7b-117">IIS 7.0 logs</span></span> |<span data-ttu-id="fdc7b-118">Blob</span><span class="sxs-lookup"><span data-stu-id="fdc7b-118">Blob</span></span> |
| <span data-ttu-id="fdc7b-119">Azure Diagnostics infrastruktur loggar</span><span class="sxs-lookup"><span data-stu-id="fdc7b-119">Azure Diagnostics infrastructure logs</span></span> |<span data-ttu-id="fdc7b-120">Tabell</span><span class="sxs-lookup"><span data-stu-id="fdc7b-120">Table</span></span> |
| <span data-ttu-id="fdc7b-121">Kunde inte begära spårningsloggar</span><span class="sxs-lookup"><span data-stu-id="fdc7b-121">Failed Request Trace logs</span></span> |<span data-ttu-id="fdc7b-122">Blob</span><span class="sxs-lookup"><span data-stu-id="fdc7b-122">Blob</span></span> |
| <span data-ttu-id="fdc7b-123">Windows-händelseloggar</span><span class="sxs-lookup"><span data-stu-id="fdc7b-123">Windows Event logs</span></span> |<span data-ttu-id="fdc7b-124">Tabell</span><span class="sxs-lookup"><span data-stu-id="fdc7b-124">Table</span></span> |
| <span data-ttu-id="fdc7b-125">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="fdc7b-125">Performance counters</span></span> |<span data-ttu-id="fdc7b-126">Tabell</span><span class="sxs-lookup"><span data-stu-id="fdc7b-126">Table</span></span> |
| <span data-ttu-id="fdc7b-127">Krasch minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="fdc7b-127">Crash dumps</span></span> |<span data-ttu-id="fdc7b-128">Blob</span><span class="sxs-lookup"><span data-stu-id="fdc7b-128">Blob</span></span> |
| <span data-ttu-id="fdc7b-129">Anpassade fel</span><span class="sxs-lookup"><span data-stu-id="fdc7b-129">Custom error logs</span></span> |<span data-ttu-id="fdc7b-130">Blob</span><span class="sxs-lookup"><span data-stu-id="fdc7b-130">Blob</span></span> |

## <a name="transfer-diagnostic-data"></a><span data-ttu-id="fdc7b-131">Överför diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="fdc7b-131">Transfer diagnostic data</span></span>
<span data-ttu-id="fdc7b-132">För SDK-2.5 och senare, kan det uppstå hello begäran tootransfer diagnostikdata via hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-132">For SDK 2.5 and later, hello request tootransfer diagnostic data can occur through hello configuration file.</span></span> <span data-ttu-id="fdc7b-133">Du kan överföra diagnostikdata med schemalagda intervall som anges i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-133">You can transfer diagnostic data at scheduled intervals as specified in hello configuration.</span></span>

<span data-ttu-id="fdc7b-134">För SDK-2.4 och tidigare kan du begära tootransfer hello diagnostikdata via hello samt i konfigurationsfilen som programmässigt.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-134">For SDK 2.4 and previous you can request tootransfer hello diagnostic data through hello configuration file as well as programmatically.</span></span> <span data-ttu-id="fdc7b-135">hello programmässiga metod kan du också toodo på begäran överföringar.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-135">hello programmatic approach also allows you toodo on-demand transfers.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdc7b-136">När du överför diagnostikdata tooan Azure storage-konto, medföra kostnader för hello lagringsresurser som använder din diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-136">When you transfer diagnostic data tooan Azure storage account, you incur costs for hello storage resources that your diagnostic data uses.</span></span>
> 
> 

## <a name="store-diagnostic-data"></a><span data-ttu-id="fdc7b-137">Lagra diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="fdc7b-137">Store diagnostic data</span></span>
<span data-ttu-id="fdc7b-138">Loggdata lagras i Blob eller tabell med hello följande namn:</span><span class="sxs-lookup"><span data-stu-id="fdc7b-138">Log data is stored in either Blob or Table storage with hello following names:</span></span>

<span data-ttu-id="fdc7b-139">**Tabeller**</span><span class="sxs-lookup"><span data-stu-id="fdc7b-139">**Tables**</span></span>

* <span data-ttu-id="fdc7b-140">**WadLogsTable** - loggar som skrivs i kod med hello spårningslyssnaren.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-140">**WadLogsTable** - Logs written in code using hello trace listener.</span></span>
* <span data-ttu-id="fdc7b-141">**WADDiagnosticInfrastructureLogsTable** -diagnostik ändringar av Övervakare och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-141">**WADDiagnosticInfrastructureLogsTable** - Diagnostic monitor and configuration changes.</span></span>
* <span data-ttu-id="fdc7b-142">**WADDirectoriesTable** – kataloger som hello diagnostikövervakare övervakning.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-142">**WADDirectoriesTable** – Directories that hello diagnostic monitor is monitoring.</span></span>  <span data-ttu-id="fdc7b-143">Detta inkluderar IIS-loggar, IIS kunde inte loggar begäran och egna kataloger.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-143">This includes IIS logs, IIS failed request logs, and custom directories.</span></span>  <span data-ttu-id="fdc7b-144">hello plats för hello blob-loggfilen anges i hello behållaren fältet och hello hello blob heter hello RelativePath fältet.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-144">hello location of hello blob log file is specified in hello Container field and hello name of hello blob is in hello RelativePath field.</span></span>  <span data-ttu-id="fdc7b-145">Hej AbsolutePath fältet anger hello plats och namn på hello som den såg ut hello virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-145">hello AbsolutePath field indicates hello location and name of hello file as it existed on hello Azure virtual machine.</span></span>
* <span data-ttu-id="fdc7b-146">**WADPerformanceCountersTable** – prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-146">**WADPerformanceCountersTable** – Performance counters.</span></span>
* <span data-ttu-id="fdc7b-147">**WADWindowsEventLogsTable** – Windows-händelseloggarna.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-147">**WADWindowsEventLogsTable** – Windows Event logs.</span></span>

<span data-ttu-id="fdc7b-148">**Blobbar**</span><span class="sxs-lookup"><span data-stu-id="fdc7b-148">**Blobs**</span></span>

* <span data-ttu-id="fdc7b-149">**bomullstuss-kontroll-container** – (endast för SDK-2.4 och tidigare) innehåller hello XML-konfigurationsfiler som styr hello Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-149">**wad-control-container** – (Only for SDK 2.4 and previous) Contains hello XML configuration files that controls hello Azure diagnostics .</span></span>
* <span data-ttu-id="fdc7b-150">**bomullstuss-iis-failedreqlogfiles** – innehåller information från IIS kunde inte begära loggar.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-150">**wad-iis-failedreqlogfiles** – Contains information from IIS Failed Request logs.</span></span>
* <span data-ttu-id="fdc7b-151">**bomullstuss-iis-loggfiler** – innehåller information om IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-151">**wad-iis-logfiles** – Contains information about IIS logs.</span></span>
* <span data-ttu-id="fdc7b-152">**”anpassad”** – en anpassade container baserat på hur du konfigurerar kataloger som övervakas av hello diagnostikövervakare.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-152">**"custom"** – A custom container based on configuring directories that are monitored by hello diagnostic monitor.</span></span>  <span data-ttu-id="fdc7b-153">hello namn för den här blobbehållaren ska anges i WADDirectoriesTable.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-153">hello name of this blob container will be specified in WADDirectoriesTable.</span></span>

## <a name="tools-tooview-diagnostic-data"></a><span data-ttu-id="fdc7b-154">Verktyg tooview diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="fdc7b-154">Tools tooview diagnostic data</span></span>
<span data-ttu-id="fdc7b-155">Flera verktyg är tillgängliga tooview hello data efter att den är överförda toostorage.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-155">Several tools are available tooview hello data after it is transferred toostorage.</span></span> <span data-ttu-id="fdc7b-156">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fdc7b-156">For example:</span></span>

* <span data-ttu-id="fdc7b-157">Server Explorer i Visual Studio - om du har installerat hello Azure Tools för Microsoft Visual Studio, du kan använda hello Azure Storage-nod i Server Explorer tooview skrivskyddade blob och tabellen data från Azure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-157">Server Explorer in Visual Studio - If you have installed hello Azure Tools for Microsoft Visual Studio, you can use hello Azure Storage node in Server Explorer tooview read-only blob and table data from your Azure storage accounts.</span></span> <span data-ttu-id="fdc7b-158">Du kan visa data från ditt konto för lokal lagring emulatorn och även från storage-konton du har skapat för Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-158">You can display data from your local storage emulator account and also from storage accounts you have created for Azure.</span></span> <span data-ttu-id="fdc7b-159">Mer information finns i [webbsökning och hantera lagringsresurser med Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span><span class="sxs-lookup"><span data-stu-id="fdc7b-159">For more information, see [Browsing and Managing Storage Resources with Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span></span>
* <span data-ttu-id="fdc7b-160">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, OSX och Linux.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, OSX, and Linux.</span></span>
* <span data-ttu-id="fdc7b-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) innehåller Azure Diagnostics Manager där du tooview, hämta och hantera hello diagnostikdata som samlas in av hello-program som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc7b-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) includes Azure Diagnostics Manager which allows you tooview, download and manage hello diagnostics data collected by hello applications running on Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdc7b-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fdc7b-162">Next Steps</span></span>
[<span data-ttu-id="fdc7b-163">Spåra hello flödet i Cloud Services-program med Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="fdc7b-163">Trace hello flow in a Cloud Services application with Azure Diagnostics</span></span>](cloud-services-dotnet-diagnostics-trace-flow.md)

