---
title: Lagra och visa diagnostiska Data i Azure Storage | Microsoft Docs
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
ms.openlocfilehash: 374cc179e13c00e439415e3df16e0c6d5ccba5e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a><span data-ttu-id="2a118-103">Lagra och visa diagnostiska data i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2a118-103">Store and view diagnostic data in Azure Storage</span></span>
<span data-ttu-id="2a118-104">Diagnostikdata lagras inte permanent om du överför den till Microsoft Azure storage-emulatorn eller till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="2a118-104">Diagnostic data is not permanently stored unless you transfer it to the Microsoft Azure storage emulator or to Azure storage.</span></span> <span data-ttu-id="2a118-105">En gång i lagring, den kan visas med en av flera tillgängliga verktyg.</span><span class="sxs-lookup"><span data-stu-id="2a118-105">Once in storage, it can be viewed with one of several available tools.</span></span>

## <a name="specify-a-storage-account"></a><span data-ttu-id="2a118-106">Ange ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="2a118-106">Specify a storage account</span></span>
<span data-ttu-id="2a118-107">Du kan ange det lagringskonto som du vill använda i ServiceConfiguration.cscfg-filen.</span><span class="sxs-lookup"><span data-stu-id="2a118-107">You specify the storage account that you want to use in the ServiceConfiguration.cscfg file.</span></span> <span data-ttu-id="2a118-108">Kontoinformationen är definierad som en anslutningssträng i en konfigurationsinställning.</span><span class="sxs-lookup"><span data-stu-id="2a118-108">The account information is defined as a connection string in a configuration setting.</span></span> <span data-ttu-id="2a118-109">I följande exempel visas standardanslutningssträngen som skapats för ett nytt Cloud Service-projekt i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2a118-109">The following example shows the default connection string created for a new Cloud Service project in  Visual Studio:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

<span data-ttu-id="2a118-110">Du kan ändra den här anslutningssträngen för att ange kontoinformationen för ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2a118-110">You can change this connection string to provide account information for an Azure storage account.</span></span>

<span data-ttu-id="2a118-111">Beroende på vilken typ av diagnostiska data som samlas in, använder Azure-diagnostik Blob-tjänsten eller tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="2a118-111">Depending on the type of diagnostic data that is being collected, Azure Diagnostics uses either the Blob service or the Table service.</span></span> <span data-ttu-id="2a118-112">I följande tabell visas de datakällor som är beständiga och deras format.</span><span class="sxs-lookup"><span data-stu-id="2a118-112">The following table shows the data sources that are persisted and their format.</span></span>

| <span data-ttu-id="2a118-113">Datakälla</span><span class="sxs-lookup"><span data-stu-id="2a118-113">Data source</span></span> | <span data-ttu-id="2a118-114">Lagringsformat</span><span class="sxs-lookup"><span data-stu-id="2a118-114">Storage format</span></span> |
| --- | --- |
| <span data-ttu-id="2a118-115">Azure-loggar</span><span class="sxs-lookup"><span data-stu-id="2a118-115">Azure logs</span></span> |<span data-ttu-id="2a118-116">Tabell</span><span class="sxs-lookup"><span data-stu-id="2a118-116">Table</span></span> |
| <span data-ttu-id="2a118-117">IIS 7.0-loggar</span><span class="sxs-lookup"><span data-stu-id="2a118-117">IIS 7.0 logs</span></span> |<span data-ttu-id="2a118-118">Blob</span><span class="sxs-lookup"><span data-stu-id="2a118-118">Blob</span></span> |
| <span data-ttu-id="2a118-119">Azure Diagnostics infrastruktur loggar</span><span class="sxs-lookup"><span data-stu-id="2a118-119">Azure Diagnostics infrastructure logs</span></span> |<span data-ttu-id="2a118-120">Tabell</span><span class="sxs-lookup"><span data-stu-id="2a118-120">Table</span></span> |
| <span data-ttu-id="2a118-121">Kunde inte begära spårningsloggar</span><span class="sxs-lookup"><span data-stu-id="2a118-121">Failed Request Trace logs</span></span> |<span data-ttu-id="2a118-122">Blob</span><span class="sxs-lookup"><span data-stu-id="2a118-122">Blob</span></span> |
| <span data-ttu-id="2a118-123">Windows-händelseloggar</span><span class="sxs-lookup"><span data-stu-id="2a118-123">Windows Event logs</span></span> |<span data-ttu-id="2a118-124">Tabell</span><span class="sxs-lookup"><span data-stu-id="2a118-124">Table</span></span> |
| <span data-ttu-id="2a118-125">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="2a118-125">Performance counters</span></span> |<span data-ttu-id="2a118-126">Tabell</span><span class="sxs-lookup"><span data-stu-id="2a118-126">Table</span></span> |
| <span data-ttu-id="2a118-127">Krasch minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="2a118-127">Crash dumps</span></span> |<span data-ttu-id="2a118-128">Blob</span><span class="sxs-lookup"><span data-stu-id="2a118-128">Blob</span></span> |
| <span data-ttu-id="2a118-129">Anpassade fel</span><span class="sxs-lookup"><span data-stu-id="2a118-129">Custom error logs</span></span> |<span data-ttu-id="2a118-130">Blob</span><span class="sxs-lookup"><span data-stu-id="2a118-130">Blob</span></span> |

## <a name="transfer-diagnostic-data"></a><span data-ttu-id="2a118-131">Överför diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="2a118-131">Transfer diagnostic data</span></span>
<span data-ttu-id="2a118-132">För SDK-2.5 och senare, kan det uppstå begäran att överföra diagnostikdata via konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="2a118-132">For SDK 2.5 and later, the request to transfer diagnostic data can occur through the configuration file.</span></span> <span data-ttu-id="2a118-133">Du kan överföra diagnostikdata med schemalagda intervall som anges i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2a118-133">You can transfer diagnostic data at scheduled intervals as specified in the configuration.</span></span>

<span data-ttu-id="2a118-134">För SDK-2.4 och tidigare kan du begära för att överföra diagnostiska data via konfigurationsfilen som programmässigt.</span><span class="sxs-lookup"><span data-stu-id="2a118-134">For SDK 2.4 and previous you can request to transfer the diagnostic data through the configuration file as well as programmatically.</span></span> <span data-ttu-id="2a118-135">Den programmässiga metoden kan du göra på begäran-överföringar.</span><span class="sxs-lookup"><span data-stu-id="2a118-135">The programmatic approach also allows you to do on-demand transfers.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a118-136">När du överför diagnostikdata till ett Azure storage-konto innebära kostnader för lagringsresurser som använder din diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="2a118-136">When you transfer diagnostic data to an Azure storage account, you incur costs for the storage resources that your diagnostic data uses.</span></span>
> 
> 

## <a name="store-diagnostic-data"></a><span data-ttu-id="2a118-137">Lagra diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="2a118-137">Store diagnostic data</span></span>
<span data-ttu-id="2a118-138">Loggdata lagras i Blob eller tabell med följande namn:</span><span class="sxs-lookup"><span data-stu-id="2a118-138">Log data is stored in either Blob or Table storage with the following names:</span></span>

<span data-ttu-id="2a118-139">**Tabeller**</span><span class="sxs-lookup"><span data-stu-id="2a118-139">**Tables**</span></span>

* <span data-ttu-id="2a118-140">**WadLogsTable** - loggar som skrivs i kod med spårningslyssnaren.</span><span class="sxs-lookup"><span data-stu-id="2a118-140">**WadLogsTable** - Logs written in code using the trace listener.</span></span>
* <span data-ttu-id="2a118-141">**WADDiagnosticInfrastructureLogsTable** -diagnostik ändringar av Övervakare och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2a118-141">**WADDiagnosticInfrastructureLogsTable** - Diagnostic monitor and configuration changes.</span></span>
* <span data-ttu-id="2a118-142">**WADDirectoriesTable** – kataloger som övervakar diagnostikövervakare.</span><span class="sxs-lookup"><span data-stu-id="2a118-142">**WADDirectoriesTable** – Directories that the diagnostic monitor is monitoring.</span></span>  <span data-ttu-id="2a118-143">Detta inkluderar IIS-loggar, IIS kunde inte loggar begäran och egna kataloger.</span><span class="sxs-lookup"><span data-stu-id="2a118-143">This includes IIS logs, IIS failed request logs, and custom directories.</span></span>  <span data-ttu-id="2a118-144">Platsen för blob-loggfilen anges i fältet behållare och RelativePath fältet är namnet på blob.</span><span class="sxs-lookup"><span data-stu-id="2a118-144">The location of the blob log file is specified in the Container field and the name of the blob is in the RelativePath field.</span></span>  <span data-ttu-id="2a118-145">Fältet AbsolutePath indikerar platsen och namnet på filen som den fanns på den virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="2a118-145">The AbsolutePath field indicates the location and name of the file as it existed on the Azure virtual machine.</span></span>
* <span data-ttu-id="2a118-146">**WADPerformanceCountersTable** – prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="2a118-146">**WADPerformanceCountersTable** – Performance counters.</span></span>
* <span data-ttu-id="2a118-147">**WADWindowsEventLogsTable** – Windows-händelseloggarna.</span><span class="sxs-lookup"><span data-stu-id="2a118-147">**WADWindowsEventLogsTable** – Windows Event logs.</span></span>

<span data-ttu-id="2a118-148">**Blobbar**</span><span class="sxs-lookup"><span data-stu-id="2a118-148">**Blobs**</span></span>

* <span data-ttu-id="2a118-149">**bomullstuss-kontroll-container** – (endast för SDK-2.4 och tidigare) innehåller XML-konfigurationsfiler som styr Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="2a118-149">**wad-control-container** – (Only for SDK 2.4 and previous) Contains the XML configuration files that controls the Azure diagnostics .</span></span>
* <span data-ttu-id="2a118-150">**bomullstuss-iis-failedreqlogfiles** – innehåller information från IIS kunde inte begära loggar.</span><span class="sxs-lookup"><span data-stu-id="2a118-150">**wad-iis-failedreqlogfiles** – Contains information from IIS Failed Request logs.</span></span>
* <span data-ttu-id="2a118-151">**bomullstuss-iis-loggfiler** – innehåller information om IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="2a118-151">**wad-iis-logfiles** – Contains information about IIS logs.</span></span>
* <span data-ttu-id="2a118-152">**”anpassad”** – en anpassade container baserat på Konfigurera kataloger som övervakas av diagnostiska övervakaren.</span><span class="sxs-lookup"><span data-stu-id="2a118-152">**"custom"** – A custom container based on configuring directories that are monitored by the diagnostic monitor.</span></span>  <span data-ttu-id="2a118-153">Namnet på den här blobbehållaren ska anges i WADDirectoriesTable.</span><span class="sxs-lookup"><span data-stu-id="2a118-153">The name of this blob container will be specified in WADDirectoriesTable.</span></span>

## <a name="tools-to-view-diagnostic-data"></a><span data-ttu-id="2a118-154">Verktyg för att visa diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="2a118-154">Tools to view diagnostic data</span></span>
<span data-ttu-id="2a118-155">Det finns flera verktyg för att visa data när den överförs till lagring.</span><span class="sxs-lookup"><span data-stu-id="2a118-155">Several tools are available to view the data after it is transferred to storage.</span></span> <span data-ttu-id="2a118-156">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2a118-156">For example:</span></span>

* <span data-ttu-id="2a118-157">Server Explorer i Visual Studio - om du har installerat Azure-verktyg för Microsoft Visual Studio, du kan använda Azure Storage-noden i Server Explorer visa skrivskyddad blob- och tabelldata från Azure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="2a118-157">Server Explorer in Visual Studio - If you have installed the Azure Tools for Microsoft Visual Studio, you can use the Azure Storage node in Server Explorer to view read-only blob and table data from your Azure storage accounts.</span></span> <span data-ttu-id="2a118-158">Du kan visa data från ditt konto för lokal lagring emulatorn och även från storage-konton du har skapat för Azure.</span><span class="sxs-lookup"><span data-stu-id="2a118-158">You can display data from your local storage emulator account and also from storage accounts you have created for Azure.</span></span> <span data-ttu-id="2a118-159">Mer information finns i [webbsökning och hantera lagringsresurser med Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span><span class="sxs-lookup"><span data-stu-id="2a118-159">For more information, see [Browsing and Managing Storage Resources with Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span></span>
* <span data-ttu-id="2a118-160">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som gör det enkelt att arbeta med Azure Storage-data i Windows, OSX och Linux.</span><span class="sxs-lookup"><span data-stu-id="2a118-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you to easily work with Azure Storage data on Windows, OSX, and Linux.</span></span>
* <span data-ttu-id="2a118-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) innehåller Azure Diagnostics Manager där du kan visa, hämta och hantera diagnostikdata som samlas in av program som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="2a118-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) includes Azure Diagnostics Manager which allows you to view, download and manage the diagnostics data collected by the applications running on Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a118-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a118-162">Next Steps</span></span>
[<span data-ttu-id="2a118-163">Spåra flödet i Cloud Services-program med Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="2a118-163">Trace the flow in a Cloud Services application with Azure Diagnostics</span></span>](cloud-services-dotnet-diagnostics-trace-flow.md)

