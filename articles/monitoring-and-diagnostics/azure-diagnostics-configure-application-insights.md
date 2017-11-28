---
title: "Konfigurera Azure-diagnostik för att skicka data till Application Insights | Microsoft Docs"
description: "Uppdatera Azure-diagnostik offentliga konfiguration för att skicka data till Application Insights."
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
ms.openlocfilehash: 67dc2d5bbfa2012e4e098616edda593d023c4c1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a><span data-ttu-id="d2511-103">Skicka Cloud Service, virtuell dator eller Service Fabric diagnostikdata till Application Insights</span><span class="sxs-lookup"><span data-stu-id="d2511-103">Send Cloud Service, Virtual Machine, or Service Fabric diagnostic data to Application Insights</span></span>
<span data-ttu-id="d2511-104">Molntjänster, virtuella datorer, virtuella datorer och Service Fabric alla filnamnstillägget Azure Diagnostics att samla in data.</span><span class="sxs-lookup"><span data-stu-id="d2511-104">Cloud services, Virtual Machines, Virtual Machine Scale Sets and Service Fabric all use the Azure Diagnostics extension to collect data.</span></span>  <span data-ttu-id="d2511-105">Azure diagnostics skickar data till Azure Storage-tabeller.</span><span class="sxs-lookup"><span data-stu-id="d2511-105">Azure diagnostics sends data to Azure Storage tables.</span></span>  <span data-ttu-id="d2511-106">Du kan dock också pipe alla eller en delmängd av data till andra platser med hjälp av Azure-diagnostik tillägget 1.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d2511-106">However, you can also pipe all or a subset of the data to other locations using Azure Diagnostics extension 1.5 or later.</span></span>

<span data-ttu-id="d2511-107">Den här artikeln beskriver hur du skickar data från Azure-diagnostik-tillägg till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d2511-107">This article describes how to send data from the Azure Diagnostics extension to Application Insights.</span></span>

## <a name="diagnostics-configuration-explained"></a><span data-ttu-id="d2511-108">Diagnostik-konfiguration beskrivs</span><span class="sxs-lookup"><span data-stu-id="d2511-108">Diagnostics configuration explained</span></span>
<span data-ttu-id="d2511-109">Azure diagnostics tillägget 1.5 introduceras egenskaperna som är ytterligare platser där du kan skicka diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="d2511-109">The Azure diagnostics extension 1.5 introduced sinks, which are additional locations where you can send diagnostic data.</span></span>

<span data-ttu-id="d2511-110">Exempel konfigurationen av en mottagare för Application Insights:</span><span class="sxs-lookup"><span data-stu-id="d2511-110">Example configuration of a sink for Application Insights:</span></span>

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
- <span data-ttu-id="d2511-111">Den **Sink** *namn* attributet är ett strängvärde som unikt identifierar sink.</span><span class="sxs-lookup"><span data-stu-id="d2511-111">The **Sink** *name* attribute is a string value that uniquely identifies the sink.</span></span>

- <span data-ttu-id="d2511-112">Den **ApplicationInsights** element anger instrumentation nyckeln för Application insights-resurs där Azure diagnostikdata skickas.</span><span class="sxs-lookup"><span data-stu-id="d2511-112">The **ApplicationInsights** element specifies instrumentation key of the Application insights resource where the Azure diagnostics data is sent.</span></span>
    - <span data-ttu-id="d2511-113">Om du inte har en befintlig Application Insights-resurs, se [skapar en ny Application Insights-resurs](../application-insights/app-insights-create-new-resource.md) mer information om att skapa en resurs och hämtar nyckeln instrumentation.</span><span class="sxs-lookup"><span data-stu-id="d2511-113">If you don't have an existing Application Insights resource, see [Create a new Application Insights resource](../application-insights/app-insights-create-new-resource.md) for more information on creating a resource and getting the instrumentation key.</span></span>
    - <span data-ttu-id="d2511-114">Om du utvecklar en tjänst i molnet med Azure SDK 2.8 och senare fylls instrumentation nyckeln i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d2511-114">If you are developing a Cloud Service with Azure SDK 2.8 and later, this instrumentation key is automatically populated.</span></span> <span data-ttu-id="d2511-115">Värdet är baserad på den **APPINSIGHTS_INSTRUMENTATIONKEY** konfigurationsinställning för tjänst när paketera Cloud Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="d2511-115">The value is based on the **APPINSIGHTS_INSTRUMENTATIONKEY** service configuration setting when packaging the Cloud Service project.</span></span> <span data-ttu-id="d2511-116">Se [Programinsikter för användning med Azure-diagnostik för att felsöka problem med Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span><span class="sxs-lookup"><span data-stu-id="d2511-116">See [Use Application Insights with Azure Diagnostics to troubleshoot Cloud Service issues](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span></span>

- <span data-ttu-id="d2511-117">Den **kanaler** elementet innehåller ett eller flera **kanal** element.</span><span class="sxs-lookup"><span data-stu-id="d2511-117">The **Channels** element contains one or more **Channel** elements.</span></span>
    - <span data-ttu-id="d2511-118">Den *namn* attributet unikt refererar till kanalen.</span><span class="sxs-lookup"><span data-stu-id="d2511-118">The *name* attribute uniquely refers to that channel.</span></span>
    - <span data-ttu-id="d2511-119">Den *loglevel* attribut kan du ange den Loggnivå som kanalen tillåter.</span><span class="sxs-lookup"><span data-stu-id="d2511-119">The *loglevel* attribute lets you specify the log level that the channel allows.</span></span> <span data-ttu-id="d2511-120">Tillgängliga nivåer i den ordning de mest till minst information är:</span><span class="sxs-lookup"><span data-stu-id="d2511-120">The available log levels in order of most to least information are:</span></span>
        - <span data-ttu-id="d2511-121">Utförlig</span><span class="sxs-lookup"><span data-stu-id="d2511-121">Verbose</span></span>
        - <span data-ttu-id="d2511-122">Information</span><span class="sxs-lookup"><span data-stu-id="d2511-122">Information</span></span>
        - <span data-ttu-id="d2511-123">Varning</span><span class="sxs-lookup"><span data-stu-id="d2511-123">Warning</span></span>
        - <span data-ttu-id="d2511-124">Fel</span><span class="sxs-lookup"><span data-stu-id="d2511-124">Error</span></span>
        - <span data-ttu-id="d2511-125">kritiska</span><span class="sxs-lookup"><span data-stu-id="d2511-125">Critical</span></span>

<span data-ttu-id="d2511-126">En kanal som fungerar som ett filter och kan du välja specifika loggningsnivåer att skicka till mål-mottagare.</span><span class="sxs-lookup"><span data-stu-id="d2511-126">A channel acts like a filter and allows you to select specific log levels to send to the target sink.</span></span> <span data-ttu-id="d2511-127">Du kan till exempel samla in detaljerade loggarna och skicka dem till lagring, men skicka endast fel till sink.</span><span class="sxs-lookup"><span data-stu-id="d2511-127">For example, you could collect verbose logs and send them to storage, but send only Errors to the sink.</span></span>

<span data-ttu-id="d2511-128">Följande bild visar den här relationen.</span><span class="sxs-lookup"><span data-stu-id="d2511-128">The following graphic shows this relationship.</span></span>

![Diagnostik offentliga konfiguration](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

<span data-ttu-id="d2511-130">Bilden nedan sammanfattas konfigurationsvärdena och hur de fungerar.</span><span class="sxs-lookup"><span data-stu-id="d2511-130">The following graphic summarizes the configuration values and how they work.</span></span> <span data-ttu-id="d2511-131">Du kan inkludera flera handfat i konfigurationen på olika nivåer i hierarkin.</span><span class="sxs-lookup"><span data-stu-id="d2511-131">You can include multiple sinks in the configuration at different levels in the hierarchy.</span></span> <span data-ttu-id="d2511-132">Sink på den översta nivån som fungerar som en global inställning och den som anges i enskilda element fungerar som en åsidosättning av den globala inställningen.</span><span class="sxs-lookup"><span data-stu-id="d2511-132">The sink at the top level acts as a global setting and the one specified at the individual element acts like an override to that global setting.</span></span>

![Diagnostik egenskaperna med Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a><span data-ttu-id="d2511-134">Fullständig sink Konfigurationsexempel</span><span class="sxs-lookup"><span data-stu-id="d2511-134">Complete sink configuration example</span></span>
<span data-ttu-id="d2511-135">Här är en komplett exempel på offentliga konfiguration fil som</span><span class="sxs-lookup"><span data-stu-id="d2511-135">Here is a complete example of the public configuration file that</span></span>
1. <span data-ttu-id="d2511-136">skickar alla fel till Application Insights (angivna på den **DiagnosticMonitorConfiguration** nod)</span><span class="sxs-lookup"><span data-stu-id="d2511-136">sends all errors to Application Insights (specified at the **DiagnosticMonitorConfiguration** node)</span></span>
2. <span data-ttu-id="d2511-137">skickar också utförlig nivå loggar för programloggarna (angivna på den **loggar** nod).</span><span class="sxs-lookup"><span data-stu-id="d2511-137">also sends Verbose level logs for the Application Logs (specified at the **Logs** node).</span></span>

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
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
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent to this channel",
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
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent to this channel"
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
<span data-ttu-id="d2511-138">I den tidigare konfigurationen har följande rader följande:</span><span class="sxs-lookup"><span data-stu-id="d2511-138">In the previous configuration, the following lines have the following meanings:</span></span>

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a><span data-ttu-id="d2511-139">Skicka alla data som samlas in av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="d2511-139">Send all the data that is being collected by Azure diagnostics</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-to-the-application-insights-sink"></a><span data-ttu-id="d2511-140">Skicka bara felloggar till Application Insights sink</span><span class="sxs-lookup"><span data-stu-id="d2511-140">Send only error logs to the Application Insights sink</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-to-application-insights"></a><span data-ttu-id="d2511-141">Skicka utförliga programloggar till Application Insights</span><span class="sxs-lookup"><span data-stu-id="d2511-141">Send Verbose application logs to Application Insights</span></span>

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a><span data-ttu-id="d2511-142">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="d2511-142">Limitations</span></span>

- <span data-ttu-id="d2511-143">**Kanaler bara logga typ och inte prestanda räknare.**</span><span class="sxs-lookup"><span data-stu-id="d2511-143">**Channels only log type and not performance counters.**</span></span> <span data-ttu-id="d2511-144">Om du anger en kanal med elementet prestandaräknaren ignoreras.</span><span class="sxs-lookup"><span data-stu-id="d2511-144">If you specify a channel with a performance counter element, it is ignored.</span></span>
- <span data-ttu-id="d2511-145">**Loggningsnivån för en kanal får inte överskrida loggningsnivån för vad som samlas in av Azure-diagnostik.**</span><span class="sxs-lookup"><span data-stu-id="d2511-145">**The log level for a channel cannot exceed the log level for what is being collected by Azure diagnostics.**</span></span> <span data-ttu-id="d2511-146">T.ex, du kan inte samla in program logga fel i loggarna elementet och försök att skicka utförlig loggar till Application Insights sink.</span><span class="sxs-lookup"><span data-stu-id="d2511-146">For example, you cannot collect Application Log errors in the Logs element and try to send Verbose logs to the Application Insight sink.</span></span> <span data-ttu-id="d2511-147">Den *scheduledTransferLogLevelFilter* attributet måste alltid samla in lika eller fler loggar än loggarna som du försöker skicka till en mottagare.</span><span class="sxs-lookup"><span data-stu-id="d2511-147">The *scheduledTransferLogLevelFilter* attribute must always collect equal or more logs than the logs you are trying to send to a sink.</span></span>
- <span data-ttu-id="d2511-148">**Du kan inte skicka blob-data som samlas in av Azure-diagnostik tillägget till Application Insights.**</span><span class="sxs-lookup"><span data-stu-id="d2511-148">**You cannot send blob data collected by Azure diagnostics extension to Application Insights.**</span></span> <span data-ttu-id="d2511-149">Till exempel allt som anges under den *kataloger* nod.</span><span class="sxs-lookup"><span data-stu-id="d2511-149">For example, anything specified under the *Directories* node.</span></span> <span data-ttu-id="d2511-150">För kraschdumpar faktiska krasch-dump skickas till blob storage och endast ett meddelande om att krasch-dump genererades skickas till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d2511-150">For Crash Dumps the actual crash dump is sent to blob storage and only a notification that the crash dump was generated is sent to Application Insights.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2511-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2511-151">Next Steps</span></span>
* <span data-ttu-id="d2511-152">Lär dig hur du [se information om din Azure-diagnostik](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d2511-152">Learn how to [view your Azure diagnostics information](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.</span></span>
* <span data-ttu-id="d2511-153">Använd [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) aktivera tillägget för Azure-diagnostik för ditt program.</span><span class="sxs-lookup"><span data-stu-id="d2511-153">Use [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) to enable the Azure diagnostics extension for your application.</span></span>
* <span data-ttu-id="d2511-154">Använd [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) aktivera tillägget för Azure-diagnostik för ditt program</span><span class="sxs-lookup"><span data-stu-id="d2511-154">Use [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) to enable the Azure diagnostics extension for your application</span></span>
