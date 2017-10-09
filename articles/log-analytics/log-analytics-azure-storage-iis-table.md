---
title: "aaaUse blob-lagring för IIS- och lagring för händelser i Azure Log Analytics | Microsoft Docs"
description: "Logganalys kan läsa hello loggar för Azure-tjänster som skriver diagnostik tootable lagring eller IIS-loggar som skrivs tooblob lagring."
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
ms.openlocfilehash: ff3de04dc8cb6729c1443372ec31a0e8dc47f273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a><span data-ttu-id="8e685-103">Använda Azure blob storage för IIS och Azure-tabellagring för händelser med logganalys</span><span class="sxs-lookup"><span data-stu-id="8e685-103">Use Azure blob storage for IIS and Azure table storage for events with Log Analytics</span></span>

<span data-ttu-id="8e685-104">Logganalys kan läsa hello loggar för hello följande tjänster som skriver diagnostik tootable lagrings- eller IIS loggar du skriftlig tooblob lagring:</span><span class="sxs-lookup"><span data-stu-id="8e685-104">Log Analytics can read hello logs for hello following services that write diagnostics tootable storage or IIS logs written tooblob storage:</span></span>

* <span data-ttu-id="8e685-105">Service Fabric-kluster (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="8e685-105">Service Fabric clusters (Preview)</span></span>
* <span data-ttu-id="8e685-106">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8e685-106">Virtual Machines</span></span>
* <span data-ttu-id="8e685-107">Web/Worker-roller</span><span class="sxs-lookup"><span data-stu-id="8e685-107">Web/Worker Roles</span></span>

<span data-ttu-id="8e685-108">Innan logganalys kan samla in data för dessa resurser, måste Azure diagnostics aktiveras.</span><span class="sxs-lookup"><span data-stu-id="8e685-108">Before Log Analytics can collect data for these resources, Azure diagnostics must be enabled.</span></span>

<span data-ttu-id="8e685-109">När diagnostik är aktiverade, kan du använda hello Azure-portalen eller PowerShell konfigurera logganalys toocollect hello loggar.</span><span class="sxs-lookup"><span data-stu-id="8e685-109">Once diagnostics are enabled, you can use hello Azure portal or PowerShell configure Log Analytics toocollect hello logs.</span></span>

<span data-ttu-id="8e685-110">Azure Diagnostics är en Azure-tillägget som du kan använda toocollect diagnostikdata från arbetsrollen, webbroll eller virtuell dator som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="8e685-110">Azure Diagnostics is an Azure extension that enables you toocollect diagnostic data from a worker role, web role, or virtual machine running in Azure.</span></span> <span data-ttu-id="8e685-111">hello data lagras i ett Azure storage-konto och sedan ska samlas in av logganalys.</span><span class="sxs-lookup"><span data-stu-id="8e685-111">hello data is stored in an Azure storage account and can then be collected by Log Analytics.</span></span>

<span data-ttu-id="8e685-112">Logganalys toocollect loggarna Azure Diagnostics hello loggar måste ha hello följande platser:</span><span class="sxs-lookup"><span data-stu-id="8e685-112">For Log Analytics toocollect these Azure Diagnostics logs, hello logs must be in hello following locations:</span></span>

| <span data-ttu-id="8e685-113">Loggtyp</span><span class="sxs-lookup"><span data-stu-id="8e685-113">Log Type</span></span> | <span data-ttu-id="8e685-114">Resurstyp</span><span class="sxs-lookup"><span data-stu-id="8e685-114">Resource Type</span></span> | <span data-ttu-id="8e685-115">Plats</span><span class="sxs-lookup"><span data-stu-id="8e685-115">Location</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8e685-116">IIS-loggar</span><span class="sxs-lookup"><span data-stu-id="8e685-116">IIS logs</span></span> |<span data-ttu-id="8e685-117">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8e685-117">Virtual Machines</span></span> <br> <span data-ttu-id="8e685-118">Webbroller</span><span class="sxs-lookup"><span data-stu-id="8e685-118">Web roles</span></span> <br> <span data-ttu-id="8e685-119">Worker-roller</span><span class="sxs-lookup"><span data-stu-id="8e685-119">Worker roles</span></span> |<span data-ttu-id="8e685-120">bomullstuss-iis-loggfiler (Blob Storage)</span><span class="sxs-lookup"><span data-stu-id="8e685-120">wad-iis-logfiles (Blob Storage)</span></span> |
| <span data-ttu-id="8e685-121">Syslog</span><span class="sxs-lookup"><span data-stu-id="8e685-121">Syslog</span></span> |<span data-ttu-id="8e685-122">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8e685-122">Virtual Machines</span></span> |<span data-ttu-id="8e685-123">LinuxsyslogVer2v0 (tabell lagring)</span><span class="sxs-lookup"><span data-stu-id="8e685-123">LinuxsyslogVer2v0 (Table Storage)</span></span> |
| <span data-ttu-id="8e685-124">Service Fabric operativa händelser</span><span class="sxs-lookup"><span data-stu-id="8e685-124">Service Fabric Operational Events</span></span> |<span data-ttu-id="8e685-125">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="8e685-125">Service Fabric nodes</span></span> |<span data-ttu-id="8e685-126">WADServiceFabricSystemEventTable</span><span class="sxs-lookup"><span data-stu-id="8e685-126">WADServiceFabricSystemEventTable</span></span> |
| <span data-ttu-id="8e685-127">Service Fabric tillförlitliga aktören händelser</span><span class="sxs-lookup"><span data-stu-id="8e685-127">Service Fabric Reliable Actor Events</span></span> |<span data-ttu-id="8e685-128">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="8e685-128">Service Fabric nodes</span></span> |<span data-ttu-id="8e685-129">WADServiceFabricReliableActorEventTable</span><span class="sxs-lookup"><span data-stu-id="8e685-129">WADServiceFabricReliableActorEventTable</span></span> |
| <span data-ttu-id="8e685-130">Händelser för Service Fabric tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="8e685-130">Service Fabric Reliable Service Events</span></span> |<span data-ttu-id="8e685-131">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="8e685-131">Service Fabric nodes</span></span> |<span data-ttu-id="8e685-132">WADServiceFabricReliableServiceEventTable</span><span class="sxs-lookup"><span data-stu-id="8e685-132">WADServiceFabricReliableServiceEventTable</span></span> |
| <span data-ttu-id="8e685-133">Windows-händelseloggar</span><span class="sxs-lookup"><span data-stu-id="8e685-133">Windows Event logs</span></span> |<span data-ttu-id="8e685-134">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="8e685-134">Service Fabric nodes</span></span> <br> <span data-ttu-id="8e685-135">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8e685-135">Virtual Machines</span></span> <br> <span data-ttu-id="8e685-136">Webbroller</span><span class="sxs-lookup"><span data-stu-id="8e685-136">Web roles</span></span> <br> <span data-ttu-id="8e685-137">Worker-roller</span><span class="sxs-lookup"><span data-stu-id="8e685-137">Worker roles</span></span> |<span data-ttu-id="8e685-138">WADWindowsEventLogsTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="8e685-138">WADWindowsEventLogsTable (Table Storage)</span></span> |
| <span data-ttu-id="8e685-139">ETW-Windows-loggar</span><span class="sxs-lookup"><span data-stu-id="8e685-139">Windows ETW logs</span></span> |<span data-ttu-id="8e685-140">Service Fabric-noder</span><span class="sxs-lookup"><span data-stu-id="8e685-140">Service Fabric nodes</span></span> <br> <span data-ttu-id="8e685-141">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8e685-141">Virtual Machines</span></span> <br> <span data-ttu-id="8e685-142">Webbroller</span><span class="sxs-lookup"><span data-stu-id="8e685-142">Web roles</span></span> <br> <span data-ttu-id="8e685-143">Worker-roller</span><span class="sxs-lookup"><span data-stu-id="8e685-143">Worker roles</span></span> |<span data-ttu-id="8e685-144">WADETWEventTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="8e685-144">WADETWEventTable (Table Storage)</span></span> |

> [!NOTE]
> <span data-ttu-id="8e685-145">IIS-loggar från Azure-webbplatser stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="8e685-145">IIS logs from Azure Websites are not currently supported.</span></span>
>
>

<span data-ttu-id="8e685-146">För virtuella datorer har hello möjlighet att installera hello [logganalys agent](log-analytics-azure-vm-extension.md) till din virtuella tooenable ytterligare insikter.</span><span class="sxs-lookup"><span data-stu-id="8e685-146">For virtual machines, you have hello option of installing hello [Log Analytics agent](log-analytics-azure-vm-extension.md) into your virtual machine tooenable additional insights.</span></span> <span data-ttu-id="8e685-147">Dessutom kan du göra ytterligare analys, inklusive konfiguration ändringsspårning, SQL-bedömning och utvärdering av uppdateringar toobeing kan tooanalyze IIS-loggar och händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="8e685-147">In addition toobeing able tooanalyze IIS logs and Event Logs, you can perform additional analysis including configuration change tracking, SQL assessment, and update assessment.</span></span>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a><span data-ttu-id="8e685-148">Aktivera Azure-diagnostik i en virtuell dator för insamling av webbloggar händelseloggen och IIS</span><span class="sxs-lookup"><span data-stu-id="8e685-148">Enable Azure diagnostics in a virtual machine for event log and IIS log collection</span></span>
<span data-ttu-id="8e685-149">Använd hello följa proceduren tooenable Azure-diagnostik i en virtuell dator för händelseloggen och IIS logg med hello Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8e685-149">Use hello following procedure tooenable Azure diagnostics in a virtual machine for Event Log and IIS log collection using hello Microsoft Azure portal.</span></span>

### <a name="tooenable-azure-diagnostics-in-a-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="8e685-150">tooenable Azure-diagnostik i en virtuell dator med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8e685-150">tooenable Azure diagnostics in a virtual machine with hello Azure portal</span></span>
1. <span data-ttu-id="8e685-151">Installera hello VM-agenten när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8e685-151">Install hello VM Agent when you create a virtual machine.</span></span> <span data-ttu-id="8e685-152">Om hello virtuella datorn redan finns kontrollerar du att hello VM-agenten redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="8e685-152">If hello virtual machine already exists, verify that hello VM Agent is already installed.</span></span>

   * <span data-ttu-id="8e685-153">I hello Azure-portalen, navigera toohello virtuell dator, väljer **valfri konfiguration**, sedan **diagnostik** och ange **Status** för**på**.</span><span class="sxs-lookup"><span data-stu-id="8e685-153">In hello Azure portal, navigate toohello virtual machine, select **Optional Configuration**, then **Diagnostics** and set **Status** too**On**.</span></span>

     <span data-ttu-id="8e685-154">När åtgärden har slutförts har hello VM hello Azure Diagnostics installerade och körs.</span><span class="sxs-lookup"><span data-stu-id="8e685-154">Upon completion, hello VM has hello Azure Diagnostics extension installed and running.</span></span> <span data-ttu-id="8e685-155">Det här tillägget är ansvarig för att samla in diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="8e685-155">This extension is responsible for collecting your diagnostics data.</span></span>
2. <span data-ttu-id="8e685-156">Aktivera övervakning och konfigurera händelseloggning på en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8e685-156">Enable monitoring and configure event logging on an existing VM.</span></span> <span data-ttu-id="8e685-157">Du kan aktivera diagnostik på hello VM-nivå.</span><span class="sxs-lookup"><span data-stu-id="8e685-157">You can enable diagnostics at hello VM level.</span></span> <span data-ttu-id="8e685-158">tooenable diagnostik och sedan konfigurera händelseloggning, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8e685-158">tooenable diagnostics and then configure event logging, perform hello following steps:</span></span>

   1. <span data-ttu-id="8e685-159">Välj hello VM.</span><span class="sxs-lookup"><span data-stu-id="8e685-159">Select hello VM.</span></span>
   2. <span data-ttu-id="8e685-160">Klicka på **övervakning**.</span><span class="sxs-lookup"><span data-stu-id="8e685-160">Click **Monitoring**.</span></span>
   3. <span data-ttu-id="8e685-161">Klicka på **diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="8e685-161">Click **Diagnostics**.</span></span>
   4. <span data-ttu-id="8e685-162">Ange hello **Status** för**på**.</span><span class="sxs-lookup"><span data-stu-id="8e685-162">Set hello **Status** too**ON**.</span></span>
   5. <span data-ttu-id="8e685-163">Markera varje diagnostik-logg som du vill toocollect.</span><span class="sxs-lookup"><span data-stu-id="8e685-163">Select each diagnostics log that you want toocollect.</span></span>
   6. <span data-ttu-id="8e685-164">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e685-164">Click **OK**.</span></span>

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a><span data-ttu-id="8e685-165">Aktivera Azure-diagnostik i en webbroll för IIS-loggen och händelsen samling</span><span class="sxs-lookup"><span data-stu-id="8e685-165">Enable Azure diagnostics in a Web role for IIS log and event collection</span></span>
<span data-ttu-id="8e685-166">Se för[hur tooEnable diagnostik i en molntjänst](../cloud-services/cloud-services-dotnet-diagnostics.md) allmänna anvisningar om hur du aktiverar Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="8e685-166">Refer too[How tooEnable Diagnostics in a Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) for general steps on enabling Azure diagnostics.</span></span> <span data-ttu-id="8e685-167">hello instruktionerna nedan använder den här informationen och anpassa den för användning med logganalys.</span><span class="sxs-lookup"><span data-stu-id="8e685-167">hello instructions below use this information and customize it for use with Log Analytics.</span></span>

<span data-ttu-id="8e685-168">Med Azure diagnostics aktiverad:</span><span class="sxs-lookup"><span data-stu-id="8e685-168">With Azure diagnostics enabled:</span></span>

* <span data-ttu-id="8e685-169">IIS-loggar lagras som standard med loggdata överförs med hello scheduledTransferPeriod överföring intervall.</span><span class="sxs-lookup"><span data-stu-id="8e685-169">IIS logs are stored by default, with log data transferred at hello scheduledTransferPeriod transfer interval.</span></span>
* <span data-ttu-id="8e685-170">Windows-händelseloggar överförs inte som standard.</span><span class="sxs-lookup"><span data-stu-id="8e685-170">Windows Event Logs are not transferred by default.</span></span>

### <a name="tooenable-diagnostics"></a><span data-ttu-id="8e685-171">tooenable diagnostik</span><span class="sxs-lookup"><span data-stu-id="8e685-171">tooenable diagnostics</span></span>
<span data-ttu-id="8e685-172">tooenable händelseloggarna i Windows eller toochange Hej scheduledTransferPeriod, konfigurera Azure-diagnostik använder hello XML-konfigurationsfil (diagnostics.wadcfg) enligt [steg 4: skapa konfigurationsfilen diagnostik och installera hello tillägg](../cloud-services/cloud-services-dotnet-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="8e685-172">tooenable Windows Event Logs, or toochange hello scheduledTransferPeriod, configure Azure Diagnostics using hello XML configuration file (diagnostics.wadcfg), as shown in [Step 4: Create your Diagnostics configuration file and install hello extension](../cloud-services/cloud-services-dotnet-diagnostics.md)</span></span>

<span data-ttu-id="8e685-173">hello följande exempel konfigurationsfil samlar in IIS-loggar och alla händelser från hello program och systemloggarna:</span><span class="sxs-lookup"><span data-stu-id="8e685-173">hello following example configuration file collects IIS Logs and all Events from hello Application and System logs:</span></span>

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant tooWeb roles -->
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

<span data-ttu-id="8e685-174">Se till att din ConfigurationSettings anger ett lagringskonto, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8e685-174">Ensure that your ConfigurationSettings specifies a storage account, as in hello following example:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

<span data-ttu-id="8e685-175">Hej **AccountName** och **AccountKey** värden finns i hello Azure-portalen i hello storage-konto instrumentpanelen under hantera åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="8e685-175">hello **AccountName** and **AccountKey** values are found in hello Azure portal in hello storage account dashboard, under Manage Access Keys.</span></span> <span data-ttu-id="8e685-176">hello-protokollet för hello anslutningssträngen måste vara **https**.</span><span class="sxs-lookup"><span data-stu-id="8e685-176">hello protocol for hello connection string must be **https**.</span></span>

<span data-ttu-id="8e685-177">När hello uppdaterade diagnostikkonfiguration används skriver tooyour Molntjänsten och diagnostik tooAzure lagring, och du är redo tooconfigure logganalys.</span><span class="sxs-lookup"><span data-stu-id="8e685-177">Once hello updated diagnostic configuration is applied tooyour cloud service and it is writing diagnostics tooAzure Storage, then you are ready tooconfigure Log Analytics.</span></span>

## <a name="use-hello-azure-portal-toocollect-logs-from-azure-storage"></a><span data-ttu-id="8e685-178">Använda hello Azure portal toocollect loggar från Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8e685-178">Use hello Azure portal toocollect logs from Azure Storage</span></span>
<span data-ttu-id="8e685-179">Du kan använda hello Azure portal tooconfigure logganalys toocollect hello loggar för hello följande Azure-tjänster:</span><span class="sxs-lookup"><span data-stu-id="8e685-179">You can use hello Azure portal tooconfigure Log Analytics toocollect hello logs for hello following Azure services:</span></span>

* <span data-ttu-id="8e685-180">Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="8e685-180">Service Fabric clusters</span></span>
* <span data-ttu-id="8e685-181">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8e685-181">Virtual Machines</span></span>
* <span data-ttu-id="8e685-182">Web/Worker-roller</span><span class="sxs-lookup"><span data-stu-id="8e685-182">Web/Worker Roles</span></span>

<span data-ttu-id="8e685-183">Navigera tooyour logganalys-arbetsytan i hello Azure-portalen och utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8e685-183">In hello Azure portal, navigate tooyour Log Analytics workspace and perform hello following tasks:</span></span>

1. <span data-ttu-id="8e685-184">Klicka på *lagringskonton loggar*</span><span class="sxs-lookup"><span data-stu-id="8e685-184">Click *Storage accounts logs*</span></span>
2. <span data-ttu-id="8e685-185">Klicka på hello *Lägg till* aktivitet</span><span class="sxs-lookup"><span data-stu-id="8e685-185">Click hello *Add* task</span></span>
3. <span data-ttu-id="8e685-186">Välj hello Storage-konto som innehåller hello diagnostik loggar</span><span class="sxs-lookup"><span data-stu-id="8e685-186">Select hello Storage account that contains hello diagnostics logs</span></span>
   * <span data-ttu-id="8e685-187">Det här kontot kan vara ett klassiska storage-konto eller ett lagringskonto i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8e685-187">This account can be either a classic storage account or an Azure Resource Manager storage account</span></span>
4. <span data-ttu-id="8e685-188">Välj hello datatyp du vill använda toocollect loggar för</span><span class="sxs-lookup"><span data-stu-id="8e685-188">Select hello Data Type you want toocollect logs for</span></span>
   * <span data-ttu-id="8e685-189">hello alternativ är IIS-loggar. Händelser. Syslog (Linux) ETW-loggar. Service Fabric-händelser</span><span class="sxs-lookup"><span data-stu-id="8e685-189">hello choices are IIS Logs; Events; Syslog (Linux); ETW Logs; Service Fabric Events</span></span>
5. <span data-ttu-id="8e685-190">hello värdet fylls i automatiskt baserat på hello datatypen och kan inte ändras</span><span class="sxs-lookup"><span data-stu-id="8e685-190">hello value for Source is automatically populated based on hello data type and cannot be changed</span></span>
6. <span data-ttu-id="8e685-191">Klicka på OK toosave hello konfiguration</span><span class="sxs-lookup"><span data-stu-id="8e685-191">Click OK toosave hello configuration</span></span>

<span data-ttu-id="8e685-192">Upprepa steg 2 till 6 för ytterligare lagringsutrymme konton och datatyper som du vill logganalys toocollect.</span><span class="sxs-lookup"><span data-stu-id="8e685-192">Repeat steps 2-6 for additional storage accounts and data types that you want Log Analytics toocollect.</span></span>

<span data-ttu-id="8e685-193">Du är kan toosee data från hello storage-konto i logganalys cirka 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="8e685-193">In approximately 30 minutes, you are able toosee data from hello storage account in Log Analytics.</span></span> <span data-ttu-id="8e685-194">Data som skrivs toostorage efter hello konfiguration används visas bara.</span><span class="sxs-lookup"><span data-stu-id="8e685-194">You will only see data that is written toostorage after hello configuration is applied.</span></span> <span data-ttu-id="8e685-195">Logganalys läser inte hello befintlig data från hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8e685-195">Log Analytics does not read hello pre-existing data from hello storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="8e685-196">hello portal inte validerar den hello-källan finns i hello storage-konto eller om nya data skrivs.</span><span class="sxs-lookup"><span data-stu-id="8e685-196">hello portal does not validate that hello Source exists in hello storage account or if new data is being written.</span></span>
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a><span data-ttu-id="8e685-197">Aktivera Azure-diagnostik i en virtuell dator för händelseloggen och IIS logg med PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e685-197">Enable Azure diagnostics in a virtual machine for event log and IIS log collection using PowerShell</span></span>
<span data-ttu-id="8e685-198">Använd hello stegen i [konfigurera logganalys tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread från Azure-diagnostik som skrivs tootable lagring.</span><span class="sxs-lookup"><span data-stu-id="8e685-198">Use hello steps in [Configuring Log Analytics tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread from Azure diagnostics that are written tootable storage.</span></span>

<span data-ttu-id="8e685-199">Med hjälp av Azure PowerShell kan du mer exakt ange hello händelser som skrivs tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="8e685-199">Using Azure PowerShell you can more precisely specify hello events that are written tooAzure Storage.</span></span>
<span data-ttu-id="8e685-200">Mer information finns i [aktiverar diagnostik i Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="8e685-200">For more information, see [Enabling Diagnostics in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="8e685-201">Du kan aktivera och uppdatera Azure diagnostics med hello följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="8e685-201">You can enable and update Azure diagnostics using hello following PowerShell script.</span></span>
<span data-ttu-id="8e685-202">Du kan också använda det här skriptet till en konfiguration för anpassad loggning.</span><span class="sxs-lookup"><span data-stu-id="8e685-202">You can also use this script with a custom logging configuration.</span></span>
<span data-ttu-id="8e685-203">Ändra hello skriptet tooset hello storage-konto, tjänstnamn och namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8e685-203">Modify hello script tooset hello storage account, service name, and virtual machine name.</span></span>
<span data-ttu-id="8e685-204">hello-skript som använder cmdlets för klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8e685-204">hello script uses cmdlets for classic virtual machines.</span></span>

<span data-ttu-id="8e685-205">Granska följande skriptexempel hello, kopierar den, ändra det efter behov, spara hello exempel som en PowerShell-skriptfil och kör sedan hello skript.</span><span class="sxs-lookup"><span data-stu-id="8e685-205">Review hello following script sample, copy it, modify it as needed, save hello sample as a PowerShell script file, and then run hello script.</span></span>

```
    #Connect tooAzure
    Add-AzureAccount

    # settings toochange:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert tooconfig format

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
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of hello extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a><span data-ttu-id="8e685-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e685-206">Next steps</span></span>
* <span data-ttu-id="8e685-207">[Samla in loggar och mått för Azure-tjänster](log-analytics-azure-storage.md) för Azure-tjänster som stöds.</span><span class="sxs-lookup"><span data-stu-id="8e685-207">[Collect logs and metrics for Azure services](log-analytics-azure-storage.md) for supported Azure services.</span></span>
* <span data-ttu-id="8e685-208">[Aktivera lösningar](log-analytics-add-solutions.md) tooprovide inblick i hello data.</span><span class="sxs-lookup"><span data-stu-id="8e685-208">[Enable Solutions](log-analytics-add-solutions.md) tooprovide insight into hello data.</span></span>
* <span data-ttu-id="8e685-209">[Använd sökfrågor](log-analytics-log-searches.md) tooanalyze hello data.</span><span class="sxs-lookup"><span data-stu-id="8e685-209">[Use search queries](log-analytics-log-searches.md) tooanalyze hello data.</span></span>
