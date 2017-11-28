---
title: "Använda blob storage för IIS- och lagring för händelser i Azure Log Analytics | Microsoft Docs"
description: "Logganalys kan läsa loggfiler för Azure-tjänster som skriver diagnostik till tabellagring eller IIS-loggar som skrivs till blob storage."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 459ef90ca1d76bada6565bfefd7b4bd1086197d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a><span data-ttu-id="13cb1-103">Använda Azure blob storage för IIS och Azure-tabellagring för händelser med logganalys</span><span class="sxs-lookup"><span data-stu-id="13cb1-103">Use Azure blob storage for IIS and Azure table storage for events with Log Analytics</span></span>

<span data-ttu-id="13cb1-104">Logganalys kan läsa loggfiler för följande tjänster som skriver diagnostik till tabellagring eller IIS-loggar som skrivs till blob storage:</span><span class="sxs-lookup"><span data-stu-id="13cb1-104">Log Analytics can read the logs for the following services that write diagnostics to table storage or IIS logs written to blob storage:</span></span>

* <span data-ttu-id="13cb1-105">Service Fabric-kluster (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="13cb1-105">Service Fabric clusters (Preview)</span></span>
* <span data-ttu-id="13cb1-106">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="13cb1-106">Virtual Machines</span></span>
* <span data-ttu-id="13cb1-107">Web/Worker-roller</span><span class="sxs-lookup"><span data-stu-id="13cb1-107">Web/Worker Roles</span></span>

<span data-ttu-id="13cb1-108">Innan logganalys kan samla in data för dessa resurser, måste Azure diagnostics aktiveras.</span><span class="sxs-lookup"><span data-stu-id="13cb1-108">Before Log Analytics can collect data for these resources, Azure diagnostics must be enabled.</span></span>

<span data-ttu-id="13cb1-109">När diagnostik är aktiverade, kan du använda Azure portal eller PowerShell konfigurera Log Analytics för att samla in loggarna.</span><span class="sxs-lookup"><span data-stu-id="13cb1-109">Once diagnostics are enabled, you can use the Azure portal or PowerShell configure Log Analytics to collect the logs.</span></span>

<span data-ttu-id="13cb1-110">Azure Diagnostics är en Azure-tillägget som gör det möjligt att samla in diagnostikdata från arbetsrollen, webbroll eller virtuell dator som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="13cb1-110">Azure Diagnostics is an Azure extension that enables you to collect diagnostic data from a worker role, web role, or virtual machine running in Azure.</span></span> <span data-ttu-id="13cb1-111">Data lagras i ett Azure storage-konto och sedan ska samlas in av logganalys.</span><span class="sxs-lookup"><span data-stu-id="13cb1-111">The data is stored in an Azure storage account and can then be collected by Log Analytics.</span></span>

<span data-ttu-id="13cb1-112">Log Analytics att samla in loggarna Azure-diagnostik måste loggarna ha följande platser:</span><span class="sxs-lookup"><span data-stu-id="13cb1-112">For Log Analytics to collect these Azure Diagnostics logs, the logs must be in the following locations:</span></span>

| <span data-ttu-id="13cb1-113">Loggtyp</span><span class="sxs-lookup"><span data-stu-id="13cb1-113">Log Type</span></span> | <span data-ttu-id="13cb1-114">Resurstyp</span><span class="sxs-lookup"><span data-stu-id="13cb1-114">Resource Type</span></span> | <span data-ttu-id="13cb1-115">Plats</span><span class="sxs-lookup"><span data-stu-id="13cb1-115">Location</span></span> |
| --- | --- | --- |
| <span data-ttu-id="13cb1-116">IIS-loggar</span><span class="sxs-lookup"><span data-stu-id="13cb1-116">IIS logs</span></span> |<span data-ttu-id="13cb1-117">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="13cb1-117">Virtual Machines</span></span> <br> <span data-ttu-id="13cb1-118">Webbroller</span><span class="sxs-lookup"><span data-stu-id="13cb1-118">Web roles</span></span> <br> <span data-ttu-id="13cb1-119">Worker-roller</span><span class="sxs-lookup"><span data-stu-id="13cb1-119">Worker roles</span></span> |<span data-ttu-id="13cb1-120">bomullstuss-iis-loggfiler (Blob Storage)</span><span class="sxs-lookup"><span data-stu-id="13cb1-120">wad-iis-logfiles (Blob Storage)</span></span> |
| <span data-ttu-id="13cb1-121">Syslog</span><span class="sxs-lookup"><span data-stu-id="13cb1-121">Syslog</span></span> |<span data-ttu-id="13cb1-122">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="13cb1-122">Virtual Machines</span></span> |<span data-ttu-id="13cb1-123">LinuxsyslogVer2v0 (tabell lagring)</span><span class="sxs-lookup"><span data-stu-id="13cb1-123">LinuxsyslogVer2v0 (Table Storage)</span></span> |
| <span data-ttu-id="13cb1-124">Service Fabric operativa händelser</span><span class="sxs-lookup"><span data-stu-id="13cb1-124">Service Fabric Operational Events</span></span> |<span data-ttu-id="13cb1-125">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="13cb1-125">Service Fabric nodes</span></span> |<span data-ttu-id="13cb1-126">WADServiceFabricSystemEventTable</span><span class="sxs-lookup"><span data-stu-id="13cb1-126">WADServiceFabricSystemEventTable</span></span> |
| <span data-ttu-id="13cb1-127">Service Fabric tillförlitliga aktören händelser</span><span class="sxs-lookup"><span data-stu-id="13cb1-127">Service Fabric Reliable Actor Events</span></span> |<span data-ttu-id="13cb1-128">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="13cb1-128">Service Fabric nodes</span></span> |<span data-ttu-id="13cb1-129">WADServiceFabricReliableActorEventTable</span><span class="sxs-lookup"><span data-stu-id="13cb1-129">WADServiceFabricReliableActorEventTable</span></span> |
| <span data-ttu-id="13cb1-130">Händelser för Service Fabric tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="13cb1-130">Service Fabric Reliable Service Events</span></span> |<span data-ttu-id="13cb1-131">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="13cb1-131">Service Fabric nodes</span></span> |<span data-ttu-id="13cb1-132">WADServiceFabricReliableServiceEventTable</span><span class="sxs-lookup"><span data-stu-id="13cb1-132">WADServiceFabricReliableServiceEventTable</span></span> |
| <span data-ttu-id="13cb1-133">Windows-händelseloggar</span><span class="sxs-lookup"><span data-stu-id="13cb1-133">Windows Event logs</span></span> |<span data-ttu-id="13cb1-134">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="13cb1-134">Service Fabric nodes</span></span> <br> <span data-ttu-id="13cb1-135">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="13cb1-135">Virtual Machines</span></span> <br> <span data-ttu-id="13cb1-136">Webbroller</span><span class="sxs-lookup"><span data-stu-id="13cb1-136">Web roles</span></span> <br> <span data-ttu-id="13cb1-137">Worker-roller</span><span class="sxs-lookup"><span data-stu-id="13cb1-137">Worker roles</span></span> |<span data-ttu-id="13cb1-138">WADWindowsEventLogsTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="13cb1-138">WADWindowsEventLogsTable (Table Storage)</span></span> |
| <span data-ttu-id="13cb1-139">ETW-Windows-loggar</span><span class="sxs-lookup"><span data-stu-id="13cb1-139">Windows ETW logs</span></span> |<span data-ttu-id="13cb1-140">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="13cb1-140">Service Fabric nodes</span></span> <br> <span data-ttu-id="13cb1-141">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="13cb1-141">Virtual Machines</span></span> <br> <span data-ttu-id="13cb1-142">Webbroller</span><span class="sxs-lookup"><span data-stu-id="13cb1-142">Web roles</span></span> <br> <span data-ttu-id="13cb1-143">Worker-roller</span><span class="sxs-lookup"><span data-stu-id="13cb1-143">Worker roles</span></span> |<span data-ttu-id="13cb1-144">WADETWEventTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="13cb1-144">WADETWEventTable (Table Storage)</span></span> |

> [!NOTE]
> <span data-ttu-id="13cb1-145">IIS-loggar från Azure-webbplatser stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="13cb1-145">IIS logs from Azure Websites are not currently supported.</span></span>
>
>

<span data-ttu-id="13cb1-146">För virtuella datorer, har du möjlighet att installera den [logganalys agent](log-analytics-azure-vm-extension.md) till den virtuella datorn att aktivera ytterligare insikter.</span><span class="sxs-lookup"><span data-stu-id="13cb1-146">For virtual machines, you have the option of installing the [Log Analytics agent](log-analytics-azure-vm-extension.md) into your virtual machine to enable additional insights.</span></span> <span data-ttu-id="13cb1-147">Förutom att analysera IIS-loggar och händelseloggar kan utföra du ytterligare analys, inklusive konfiguration ändringsspårning, SQL-bedömning och utvärdering av uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="13cb1-147">In addition to being able to analyze IIS logs and Event Logs, you can perform additional analysis including configuration change tracking, SQL assessment, and update assessment.</span></span>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a><span data-ttu-id="13cb1-148">Aktivera Azure-diagnostik i en virtuell dator för insamling av webbloggar händelseloggen och IIS</span><span class="sxs-lookup"><span data-stu-id="13cb1-148">Enable Azure diagnostics in a virtual machine for event log and IIS log collection</span></span>
<span data-ttu-id="13cb1-149">Använd följande procedur för att aktivera Azure-diagnostik i en virtuell dator för händelseloggen och IIS Logginsamling med hjälp av Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="13cb1-149">Use the following procedure to enable Azure diagnostics in a virtual machine for Event Log and IIS log collection using the Microsoft Azure portal.</span></span>

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a><span data-ttu-id="13cb1-150">Så här aktiverar du Azure-diagnostik i en virtuell dator med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="13cb1-150">To enable Azure diagnostics in a virtual machine with the Azure portal</span></span>
1. <span data-ttu-id="13cb1-151">Installera den Virtuella Datoragenten när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="13cb1-151">Install the VM Agent when you create a virtual machine.</span></span> <span data-ttu-id="13cb1-152">Om den virtuella datorn redan finns kontrollerar du att den Virtuella Datoragenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="13cb1-152">If the virtual machine already exists, verify that the VM Agent is already installed.</span></span>

   * <span data-ttu-id="13cb1-153">I Azure portal, navigerar du till den virtuella datorn, Välj **valfri konfiguration**, sedan **diagnostik** och ange **Status** till **på** .</span><span class="sxs-lookup"><span data-stu-id="13cb1-153">In the Azure portal, navigate to the virtual machine, select **Optional Configuration**, then **Diagnostics** and set **Status** to **On**.</span></span>

     <span data-ttu-id="13cb1-154">Den virtuella datorn har filnamnstillägget Azure Diagnostics installerade och körs när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="13cb1-154">Upon completion, the VM has the Azure Diagnostics extension installed and running.</span></span> <span data-ttu-id="13cb1-155">Det här tillägget är ansvarig för att samla in diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="13cb1-155">This extension is responsible for collecting your diagnostics data.</span></span>
2. <span data-ttu-id="13cb1-156">Aktivera övervakning och konfigurera händelseloggning på en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="13cb1-156">Enable monitoring and configure event logging on an existing VM.</span></span> <span data-ttu-id="13cb1-157">Du kan aktivera diagnostik på VM-nivå.</span><span class="sxs-lookup"><span data-stu-id="13cb1-157">You can enable diagnostics at the VM level.</span></span> <span data-ttu-id="13cb1-158">Om du vill aktivera diagnostik och sedan konfigurera händelseloggning, utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="13cb1-158">To enable diagnostics and then configure event logging, perform the following steps:</span></span>

   1. <span data-ttu-id="13cb1-159">Välj den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="13cb1-159">Select the VM.</span></span>
   2. <span data-ttu-id="13cb1-160">Klicka på **övervakning**.</span><span class="sxs-lookup"><span data-stu-id="13cb1-160">Click **Monitoring**.</span></span>
   3. <span data-ttu-id="13cb1-161">Klicka på **diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="13cb1-161">Click **Diagnostics**.</span></span>
   4. <span data-ttu-id="13cb1-162">Ange den **Status** till **på**.</span><span class="sxs-lookup"><span data-stu-id="13cb1-162">Set the **Status** to **ON**.</span></span>
   5. <span data-ttu-id="13cb1-163">Markera varje diagnostik-logg som du vill samla in.</span><span class="sxs-lookup"><span data-stu-id="13cb1-163">Select each diagnostics log that you want to collect.</span></span>
   6. <span data-ttu-id="13cb1-164">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="13cb1-164">Click **OK**.</span></span>

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a><span data-ttu-id="13cb1-165">Aktivera Azure-diagnostik i en webbroll för IIS-loggen och händelsen samling</span><span class="sxs-lookup"><span data-stu-id="13cb1-165">Enable Azure diagnostics in a Web role for IIS log and event collection</span></span>
<span data-ttu-id="13cb1-166">Referera till [hur att aktivera diagnostik i en molntjänst](../cloud-services/cloud-services-dotnet-diagnostics.md) allmänna anvisningar om hur du aktiverar Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="13cb1-166">Refer to [How To Enable Diagnostics in a Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) for general steps on enabling Azure diagnostics.</span></span> <span data-ttu-id="13cb1-167">Anvisningarna nedan använder den här informationen och anpassa den för användning med logganalys.</span><span class="sxs-lookup"><span data-stu-id="13cb1-167">The instructions below use this information and customize it for use with Log Analytics.</span></span>

<span data-ttu-id="13cb1-168">Med Azure diagnostics aktiverad:</span><span class="sxs-lookup"><span data-stu-id="13cb1-168">With Azure diagnostics enabled:</span></span>

* <span data-ttu-id="13cb1-169">IIS-loggar lagras som standard med loggdata överförs vid intervallet som scheduledTransferPeriod överföring.</span><span class="sxs-lookup"><span data-stu-id="13cb1-169">IIS logs are stored by default, with log data transferred at the scheduledTransferPeriod transfer interval.</span></span>
* <span data-ttu-id="13cb1-170">Windows-händelseloggar överförs inte som standard.</span><span class="sxs-lookup"><span data-stu-id="13cb1-170">Windows Event Logs are not transferred by default.</span></span>

### <a name="to-enable-diagnostics"></a><span data-ttu-id="13cb1-171">Aktivera diagnostik</span><span class="sxs-lookup"><span data-stu-id="13cb1-171">To enable diagnostics</span></span>
<span data-ttu-id="13cb1-172">Aktivera Windows-händelseloggar, eller ändra scheduledTransferPeriod, konfigurera Azure-diagnostik med XML-konfigurationsfilen (diagnostics.wadcfg) enligt [steg 4: skapa konfigurationsfilen diagnostik och installera tillägget](../cloud-services/cloud-services-dotnet-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="13cb1-172">To enable Windows Event Logs, or to change the scheduledTransferPeriod, configure Azure Diagnostics using the XML configuration file (diagnostics.wadcfg), as shown in [Step 4: Create your Diagnostics configuration file and install the extension](../cloud-services/cloud-services-dotnet-diagnostics.md)</span></span>

<span data-ttu-id="13cb1-173">Följande exempel konfigurationsfil samlar in IIS-loggar och alla händelser från program- och systemloggarna:</span><span class="sxs-lookup"><span data-stu-id="13cb1-173">The following example configuration file collects IIS Logs and all Events from the Application and System logs:</span></span>

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

<span data-ttu-id="13cb1-174">Se till att din ConfigurationSettings anger ett lagringskonto, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="13cb1-174">Ensure that your ConfigurationSettings specifies a storage account, as in the following example:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

<span data-ttu-id="13cb1-175">Den **AccountName** och **AccountKey** värden finns i Azure-portalen på instrumentpanelen för storage-konto, under hantera åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="13cb1-175">The **AccountName** and **AccountKey** values are found in the Azure portal in the storage account dashboard, under Manage Access Keys.</span></span> <span data-ttu-id="13cb1-176">Protokollet för anslutningssträngen måste vara **https**.</span><span class="sxs-lookup"><span data-stu-id="13cb1-176">The protocol for the connection string must be **https**.</span></span>

<span data-ttu-id="13cb1-177">När den uppdaterade diagnostiska konfigurationen tillämpas på Molntjänsten och det skriver diagnostik till Azure Storage, är du redo att konfigurera logganalys.</span><span class="sxs-lookup"><span data-stu-id="13cb1-177">Once the updated diagnostic configuration is applied to your cloud service and it is writing diagnostics to Azure Storage, then you are ready to configure Log Analytics.</span></span>

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a><span data-ttu-id="13cb1-178">Använda Azure portal för att samla in loggar från Azure Storage</span><span class="sxs-lookup"><span data-stu-id="13cb1-178">Use the Azure portal to collect logs from Azure Storage</span></span>
<span data-ttu-id="13cb1-179">Du kan använda Azure-portalen för att konfigurera Log Analytics för att samla in loggar för följande Azure-tjänster:</span><span class="sxs-lookup"><span data-stu-id="13cb1-179">You can use the Azure portal to configure Log Analytics to collect the logs for the following Azure services:</span></span>

* <span data-ttu-id="13cb1-180">Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="13cb1-180">Service Fabric clusters</span></span>
* <span data-ttu-id="13cb1-181">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="13cb1-181">Virtual Machines</span></span>
* <span data-ttu-id="13cb1-182">Web/Worker-roller</span><span class="sxs-lookup"><span data-stu-id="13cb1-182">Web/Worker Roles</span></span>

<span data-ttu-id="13cb1-183">Navigera till logganalys-arbetsytan i Azure-portalen och utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="13cb1-183">In the Azure portal, navigate to your Log Analytics workspace and perform the following tasks:</span></span>

1. <span data-ttu-id="13cb1-184">Klicka på *lagringskonton loggar*</span><span class="sxs-lookup"><span data-stu-id="13cb1-184">Click *Storage accounts logs*</span></span>
2. <span data-ttu-id="13cb1-185">Klicka på den *Lägg till* aktivitet</span><span class="sxs-lookup"><span data-stu-id="13cb1-185">Click the *Add* task</span></span>
3. <span data-ttu-id="13cb1-186">Välj lagringskonto som innehåller diagnostik-loggar</span><span class="sxs-lookup"><span data-stu-id="13cb1-186">Select the Storage account that contains the diagnostics logs</span></span>
   * <span data-ttu-id="13cb1-187">Det här kontot kan vara ett klassiska storage-konto eller ett lagringskonto i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="13cb1-187">This account can be either a classic storage account or an Azure Resource Manager storage account</span></span>
4. <span data-ttu-id="13cb1-188">Välj en datatyp som du vill samla in loggar för</span><span class="sxs-lookup"><span data-stu-id="13cb1-188">Select the Data Type you want to collect logs for</span></span>
   * <span data-ttu-id="13cb1-189">Alternativen är IIS-loggar. Händelser. Syslog (Linux) ETW-loggar. Service Fabric-händelser</span><span class="sxs-lookup"><span data-stu-id="13cb1-189">The choices are IIS Logs; Events; Syslog (Linux); ETW Logs; Service Fabric Events</span></span>
5. <span data-ttu-id="13cb1-190">Värdet för källa fylls i automatiskt baserat på datatyp och kan inte ändras</span><span class="sxs-lookup"><span data-stu-id="13cb1-190">The value for Source is automatically populated based on the data type and cannot be changed</span></span>
6. <span data-ttu-id="13cb1-191">Klicka på OK om du vill spara konfigurationen</span><span class="sxs-lookup"><span data-stu-id="13cb1-191">Click OK to save the configuration</span></span>

<span data-ttu-id="13cb1-192">Upprepa steg 2 till 6 för ytterligare lagringskonton och datatyper som du vill använda Log Analytics för att samla in.</span><span class="sxs-lookup"><span data-stu-id="13cb1-192">Repeat steps 2-6 for additional storage accounts and data types that you want Log Analytics to collect.</span></span>

<span data-ttu-id="13cb1-193">Du ska kunna se data från storage-konto i logganalys cirka 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="13cb1-193">In approximately 30 minutes, you are able to see data from the storage account in Log Analytics.</span></span> <span data-ttu-id="13cb1-194">Data som skrivs till lagring när konfigurationen tillämpas visas bara.</span><span class="sxs-lookup"><span data-stu-id="13cb1-194">You will only see data that is written to storage after the configuration is applied.</span></span> <span data-ttu-id="13cb1-195">Logganalys läser inte befintliga data från lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="13cb1-195">Log Analytics does not read the pre-existing data from the storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="13cb1-196">Portalen kontrollerar inte att källan finns i lagringskontot eller om nya data skrivs.</span><span class="sxs-lookup"><span data-stu-id="13cb1-196">The portal does not validate that the Source exists in the storage account or if new data is being written.</span></span>
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a><span data-ttu-id="13cb1-197">Aktivera Azure-diagnostik i en virtuell dator för händelseloggen och IIS logg med PowerShell</span><span class="sxs-lookup"><span data-stu-id="13cb1-197">Enable Azure diagnostics in a virtual machine for event log and IIS log collection using PowerShell</span></span>
<span data-ttu-id="13cb1-198">Följ stegen i [konfigurera logganalys att indexera Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) du använder PowerShell för att läsa från Azure-diagnostik som skrivs till table storage.</span><span class="sxs-lookup"><span data-stu-id="13cb1-198">Use the steps in [Configuring Log Analytics to index Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) to use PowerShell to read from Azure diagnostics that are written to table storage.</span></span>

<span data-ttu-id="13cb1-199">Med hjälp av Azure PowerShell kan du mer exakt ange de händelser som skrivs till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="13cb1-199">Using Azure PowerShell you can more precisely specify the events that are written to Azure Storage.</span></span>
<span data-ttu-id="13cb1-200">Mer information finns i [aktiverar diagnostik i Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="13cb1-200">For more information, see [Enabling Diagnostics in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="13cb1-201">Du kan aktivera och uppdatera Azure diagnostics med följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="13cb1-201">You can enable and update Azure diagnostics using the following PowerShell script.</span></span>
<span data-ttu-id="13cb1-202">Du kan också använda det här skriptet till en konfiguration för anpassad loggning.</span><span class="sxs-lookup"><span data-stu-id="13cb1-202">You can also use this script with a custom logging configuration.</span></span>
<span data-ttu-id="13cb1-203">Ändra skriptet för att ange storage-konto, tjänstnamn och namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="13cb1-203">Modify the script to set the storage account, service name, and virtual machine name.</span></span>
<span data-ttu-id="13cb1-204">Skriptet använder cmdlets för klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="13cb1-204">The script uses cmdlets for classic virtual machines.</span></span>

<span data-ttu-id="13cb1-205">Granska följande skriptexempel, kopierar den, ändra det efter behov, spara exemplet som en PowerShell-skriptfil och kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="13cb1-205">Review the following script sample, copy it, modify it as needed, save the sample as a PowerShell script file, and then run the script.</span></span>

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a><span data-ttu-id="13cb1-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13cb1-206">Next steps</span></span>
* <span data-ttu-id="13cb1-207">[Samla in loggar och mått för Azure-tjänster](log-analytics-azure-storage.md) för Azure-tjänster som stöds.</span><span class="sxs-lookup"><span data-stu-id="13cb1-207">[Collect logs and metrics for Azure services](log-analytics-azure-storage.md) for supported Azure services.</span></span>
* <span data-ttu-id="13cb1-208">[Aktivera lösningar](log-analytics-add-solutions.md) att ge insikt om data.</span><span class="sxs-lookup"><span data-stu-id="13cb1-208">[Enable Solutions](log-analytics-add-solutions.md) to provide insight into the data.</span></span>
* <span data-ttu-id="13cb1-209">[Använd sökfrågor](log-analytics-log-searches.md) att analysera data.</span><span class="sxs-lookup"><span data-stu-id="13cb1-209">[Use search queries](log-analytics-log-searches.md) to analyze the data.</span></span>
