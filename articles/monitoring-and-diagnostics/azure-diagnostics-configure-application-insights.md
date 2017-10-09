---
title: aaaConfigure Azure Diagnostics toosend data tooApplication insikter | Microsoft Docs
description: Uppdatera hello Azure Diagnostics offentliga konfiguration toosend data tooApplication insikter.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: f9e12c3e-c307-435e-a149-ef0fef20513a
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2016
ms.author: robb
ms.openlocfilehash: 7c36f29da8fdc12fa58c17458348a311b900b0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-tooapplication-insights"></a><span data-ttu-id="e6faa-103">Skicka Cloud Service, virtuell dator eller Service Fabric diagnostikdata tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="e6faa-103">Send Cloud Service, Virtual Machine, or Service Fabric diagnostic data tooApplication Insights</span></span>
<span data-ttu-id="e6faa-104">Molntjänster, virtuella datorer, virtuella datorer och Service Fabric alla använda hello Azure Diagnostics tillägget toocollect data.</span><span class="sxs-lookup"><span data-stu-id="e6faa-104">Cloud services, Virtual Machines, Virtual Machine Scale Sets and Service Fabric all use hello Azure Diagnostics extension toocollect data.</span></span>  <span data-ttu-id="e6faa-105">Azure diagnostics skickar data tooAzure Storage-tabeller.</span><span class="sxs-lookup"><span data-stu-id="e6faa-105">Azure diagnostics sends data tooAzure Storage tables.</span></span>  <span data-ttu-id="e6faa-106">Du kan dock också pipe alla eller en delmängd av hello data tooother platser med hjälp av Azure-diagnostik tillägget 1.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e6faa-106">However, you can also pipe all or a subset of hello data tooother locations using Azure Diagnostics extension 1.5 or later.</span></span>

<span data-ttu-id="e6faa-107">Den här artikeln beskriver hur toosend data från hello Azure Diagnostics tillägget tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="e6faa-107">This article describes how toosend data from hello Azure Diagnostics extension tooApplication Insights.</span></span>

## <a name="diagnostics-configuration-explained"></a><span data-ttu-id="e6faa-108">Diagnostik-konfiguration beskrivs</span><span class="sxs-lookup"><span data-stu-id="e6faa-108">Diagnostics configuration explained</span></span>
<span data-ttu-id="e6faa-109">hello Azure diagnostics tillägget 1.5 introduceras sänkor, vilket ytterligare platser där du kan skicka diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="e6faa-109">hello Azure diagnostics extension 1.5 introduced sinks, which are additional locations where you can send diagnostic data.</span></span>

<span data-ttu-id="e6faa-110">Exempel konfigurationen av en mottagare för Application Insights:</span><span class="sxs-lookup"><span data-stu-id="e6faa-110">Example configuration of a sink for Application Insights:</span></span>

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- <span data-ttu-id="e6faa-111">Hej **Sink** *namn* attributet är ett strängvärde som unikt identifierar hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="e6faa-111">hello **Sink** *name* attribute is a string value that uniquely identifies hello sink.</span></span>

- <span data-ttu-id="e6faa-112">Hej **ApplicationInsights** element anger instrumentation nyckeln för hello Application insights-resurs där hello Azure diagnostikdata skickas.</span><span class="sxs-lookup"><span data-stu-id="e6faa-112">hello **ApplicationInsights** element specifies instrumentation key of hello Application insights resource where hello Azure diagnostics data is sent.</span></span>
    - <span data-ttu-id="e6faa-113">Om du inte har en befintlig Application Insights-resurs, se [skapar en ny Application Insights-resurs](../application-insights/app-insights-create-new-resource.md) mer information om att skapa en resurs och hämtar hello instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="e6faa-113">If you don't have an existing Application Insights resource, see [Create a new Application Insights resource](../application-insights/app-insights-create-new-resource.md) for more information on creating a resource and getting hello instrumentation key.</span></span>
    - <span data-ttu-id="e6faa-114">Om du utvecklar en tjänst i molnet med Azure SDK 2.8 och senare fylls instrumentation nyckeln i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e6faa-114">If you are developing a Cloud Service with Azure SDK 2.8 and later, this instrumentation key is automatically populated.</span></span> <span data-ttu-id="e6faa-115">hello värdet baseras på hello **APPINSIGHTS_INSTRUMENTATIONKEY** konfigurationsinställning för tjänst när paketera hello Cloud Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="e6faa-115">hello value is based on hello **APPINSIGHTS_INSTRUMENTATIONKEY** service configuration setting when packaging hello Cloud Service project.</span></span> <span data-ttu-id="e6faa-116">Se [Programinsikter för användning med Azure-diagnostik tootroubleshoot Molntjänsten utfärdar](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span><span class="sxs-lookup"><span data-stu-id="e6faa-116">See [Use Application Insights with Azure Diagnostics tootroubleshoot Cloud Service issues](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span></span>

- <span data-ttu-id="e6faa-117">Hej **kanaler** elementet innehåller ett eller flera **kanal** element.</span><span class="sxs-lookup"><span data-stu-id="e6faa-117">hello **Channels** element contains one or more **Channel** elements.</span></span>
    - <span data-ttu-id="e6faa-118">Hej *namn* attribut refererar unikt toothat kanal.</span><span class="sxs-lookup"><span data-stu-id="e6faa-118">hello *name* attribute uniquely refers toothat channel.</span></span>
    - <span data-ttu-id="e6faa-119">Hej *loglevel* attribut kan du ange hello Loggnivå som hello kanalen tillåter.</span><span class="sxs-lookup"><span data-stu-id="e6faa-119">hello *loglevel* attribute lets you specify hello log level that hello channel allows.</span></span> <span data-ttu-id="e6faa-120">hello tillgängliga loggningsnivåer i ordningen för de flesta tooleast information är:</span><span class="sxs-lookup"><span data-stu-id="e6faa-120">hello available log levels in order of most tooleast information are:</span></span>
        - <span data-ttu-id="e6faa-121">Utförlig</span><span class="sxs-lookup"><span data-stu-id="e6faa-121">Verbose</span></span>
        - <span data-ttu-id="e6faa-122">Information</span><span class="sxs-lookup"><span data-stu-id="e6faa-122">Information</span></span>
        - <span data-ttu-id="e6faa-123">Varning</span><span class="sxs-lookup"><span data-stu-id="e6faa-123">Warning</span></span>
        - <span data-ttu-id="e6faa-124">Fel</span><span class="sxs-lookup"><span data-stu-id="e6faa-124">Error</span></span>
        - <span data-ttu-id="e6faa-125">kritiska</span><span class="sxs-lookup"><span data-stu-id="e6faa-125">Critical</span></span>

<span data-ttu-id="e6faa-126">En kanal som fungerar som ett filter och låter dig tooselect viss loggning nivåer toosend toohello mål mottagare.</span><span class="sxs-lookup"><span data-stu-id="e6faa-126">A channel acts like a filter and allows you tooselect specific log levels toosend toohello target sink.</span></span> <span data-ttu-id="e6faa-127">Du kan till exempel samla in detaljerade loggarna och skicka dem toostorage, men skicka endast fel toohello mottagare.</span><span class="sxs-lookup"><span data-stu-id="e6faa-127">For example, you could collect verbose logs and send them toostorage, but send only Errors toohello sink.</span></span>

<span data-ttu-id="e6faa-128">hello visar följande bild den här relationen.</span><span class="sxs-lookup"><span data-stu-id="e6faa-128">hello following graphic shows this relationship.</span></span>

![Diagnostik offentliga konfiguration](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

<span data-ttu-id="e6faa-130">Följande bild hello sammanfattas hello konfigurationsvärden och hur de fungerar.</span><span class="sxs-lookup"><span data-stu-id="e6faa-130">hello following graphic summarizes hello configuration values and how they work.</span></span> <span data-ttu-id="e6faa-131">Du kan inkludera flera handfat i hello konfiguration på olika nivåer i hello hierarki.</span><span class="sxs-lookup"><span data-stu-id="e6faa-131">You can include multiple sinks in hello configuration at different levels in hello hierarchy.</span></span> <span data-ttu-id="e6faa-132">hello sink toppnivå hello fungerar som en global inställning och hello något har angetts på hello enskilda element fungerar som en åsidosättning toothat global inställning.</span><span class="sxs-lookup"><span data-stu-id="e6faa-132">hello sink at hello top level acts as a global setting and hello one specified at hello individual element acts like an override toothat global setting.</span></span>

![Diagnostik egenskaperna med Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a><span data-ttu-id="e6faa-134">Fullständig sink Konfigurationsexempel</span><span class="sxs-lookup"><span data-stu-id="e6faa-134">Complete sink configuration example</span></span>
<span data-ttu-id="e6faa-135">Här är en komplett exempel på hello offentliga konfiguration fil som</span><span class="sxs-lookup"><span data-stu-id="e6faa-135">Here is a complete example of hello public configuration file that</span></span>
1. <span data-ttu-id="e6faa-136">skickar alla fel tooApplication insikter (anges på hello **DiagnosticMonitorConfiguration** nod)</span><span class="sxs-lookup"><span data-stu-id="e6faa-136">sends all errors tooApplication Insights (specified at hello **DiagnosticMonitorConfiguration** node)</span></span>
2. <span data-ttu-id="e6faa-137">skickar också utförlig nivå loggar för hello programloggarna (anges på hello **loggar** nod).</span><span class="sxs-lookup"><span data-stu-id="e6faa-137">also sends Verbose level logs for hello Application Logs (specified at hello **Logs** node).</span></span>

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent toothis channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent toothis channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent toothis channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "DataSource": [
                {
                    "name": "Application!*"
                }
            ]
        },
        "Logs": {
            "scheduledTransferPeriod": "PT1M",
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent toothis channel"
        }
    },
    "SinksConfig": {
        "Sink": [
            {
                "name": "ApplicationInsights",
                "ApplicationInsights": "{Insert InstrumentationKey}",
                "Channels": {
                    "Channel": [
                        {
                            "logLevel": "Error",
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
<span data-ttu-id="e6faa-138">I tidigare hello-konfigurationen har hello följande raderna hello följande innebörd:</span><span class="sxs-lookup"><span data-stu-id="e6faa-138">In hello previous configuration, hello following lines have hello following meanings:</span></span>

### <a name="send-all-hello-data-that-is-being-collected-by-azure-diagnostics"></a><span data-ttu-id="e6faa-139">Skicka alla hello-data som samlas in av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="e6faa-139">Send all hello data that is being collected by Azure diagnostics</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-toohello-application-insights-sink"></a><span data-ttu-id="e6faa-140">Skicka bara fel loggar toohello Application Insights mottagare</span><span class="sxs-lookup"><span data-stu-id="e6faa-140">Send only error logs toohello Application Insights sink</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-tooapplication-insights"></a><span data-ttu-id="e6faa-141">Skicka utförliga programloggar tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="e6faa-141">Send Verbose application logs tooApplication Insights</span></span>

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a><span data-ttu-id="e6faa-142">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="e6faa-142">Limitations</span></span>

- <span data-ttu-id="e6faa-143">**Kanaler bara logga typ och inte prestanda räknare.**</span><span class="sxs-lookup"><span data-stu-id="e6faa-143">**Channels only log type and not performance counters.**</span></span> <span data-ttu-id="e6faa-144">Om du anger en kanal med elementet prestandaräknaren ignoreras.</span><span class="sxs-lookup"><span data-stu-id="e6faa-144">If you specify a channel with a performance counter element, it is ignored.</span></span>
- <span data-ttu-id="e6faa-145">**hello loggningsnivån för en kanal får inte överskrida hello loggningsnivån för vad som samlas in av Azure-diagnostik.**</span><span class="sxs-lookup"><span data-stu-id="e6faa-145">**hello log level for a channel cannot exceed hello log level for what is being collected by Azure diagnostics.**</span></span> <span data-ttu-id="e6faa-146">Till exempel inte du samla in program logga fel i hello loggar element och försök toosend detaljerade loggarna toohello Application Insights mottagare.</span><span class="sxs-lookup"><span data-stu-id="e6faa-146">For example, you cannot collect Application Log errors in hello Logs element and try toosend Verbose logs toohello Application Insight sink.</span></span> <span data-ttu-id="e6faa-147">Hej *scheduledTransferLogLevelFilter* attributet måste alltid samla in lika eller fler loggar än hello loggar du försöker toosend tooa mottagare.</span><span class="sxs-lookup"><span data-stu-id="e6faa-147">hello *scheduledTransferLogLevelFilter* attribute must always collect equal or more logs than hello logs you are trying toosend tooa sink.</span></span>
- <span data-ttu-id="e6faa-148">**Du kan inte skicka blob-data som samlas in av Azure-diagnostik tillägget tooApplication insikter.**</span><span class="sxs-lookup"><span data-stu-id="e6faa-148">**You cannot send blob data collected by Azure diagnostics extension tooApplication Insights.**</span></span> <span data-ttu-id="e6faa-149">Till exempel allt som anges under hello *kataloger* nod.</span><span class="sxs-lookup"><span data-stu-id="e6faa-149">For example, anything specified under hello *Directories* node.</span></span> <span data-ttu-id="e6faa-150">För kraschdumpar hello faktiska krasch-dump skickas tooblob lagring och ett meddelande som hello krasch-dump genererades skickas tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="e6faa-150">For Crash Dumps hello actual crash dump is sent tooblob storage and only a notification that hello crash dump was generated is sent tooApplication Insights.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6faa-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6faa-151">Next Steps</span></span>
* <span data-ttu-id="e6faa-152">Lär dig hur för[se information om din Azure-diagnostik](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e6faa-152">Learn how too[view your Azure diagnostics information](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.</span></span>
* <span data-ttu-id="e6faa-153">Använd [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics tillägget för ditt program.</span><span class="sxs-lookup"><span data-stu-id="e6faa-153">Use [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics extension for your application.</span></span>
* <span data-ttu-id="e6faa-154">Använd [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics tillägget för ditt program</span><span class="sxs-lookup"><span data-stu-id="e6faa-154">Use [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics extension for your application</span></span>
