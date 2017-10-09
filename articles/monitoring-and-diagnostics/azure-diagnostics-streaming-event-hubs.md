---
title: "aaaStreaming data i Azure-diagnostik i hello varm sökväg med Händelsehubbar | Microsoft Docs"
description: "Konfigurera Azure-diagnostik med Händelsehubbar avslutas tooend, inklusive anvisningar för vanliga scenarier."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: a2528ddd0688d1c23a8631e769ca016dd79e4159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-azure-diagnostics-data-in-hello-hot-path-by-using-event-hubs"></a><span data-ttu-id="73efa-103">Strömmande data i Azure-diagnostik i hello varm sökväg med hjälp av Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="73efa-103">Streaming Azure Diagnostics data in hello hot path by using Event Hubs</span></span>
<span data-ttu-id="73efa-104">Azure-diagnostik och ger flexibla sätt toocollect mått och loggar från cloud services virtuella maskiner (VMs) och överför resultat tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="73efa-104">Azure Diagnostics provides flexible ways toocollect metrics and logs from cloud services virtual machines (VMs) and transfer results tooAzure Storage.</span></span> <span data-ttu-id="73efa-105">Från och med tidsintervall för hello mars 2016 (SDK 2.9), kan du skicka diagnostik toocustom datakällor och dataöverföring varm sökväg i sekunder med hjälp av [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="73efa-105">Starting in hello March 2016 (SDK 2.9) time frame, you can send Diagnostics toocustom data sources and transfer hot path data in seconds by using [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span></span>

<span data-ttu-id="73efa-106">Datatyper som stöds är:</span><span class="sxs-lookup"><span data-stu-id="73efa-106">Supported data types include:</span></span>

* <span data-ttu-id="73efa-107">ETW-händelser (Event Tracing for Windows)</span><span class="sxs-lookup"><span data-stu-id="73efa-107">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="73efa-108">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="73efa-108">Performance counters</span></span>
* <span data-ttu-id="73efa-109">Windows-händelseloggar</span><span class="sxs-lookup"><span data-stu-id="73efa-109">Windows event logs</span></span>
* <span data-ttu-id="73efa-110">Programloggar</span><span class="sxs-lookup"><span data-stu-id="73efa-110">Application logs</span></span>
* <span data-ttu-id="73efa-111">Azure Diagnostics infrastruktur loggar</span><span class="sxs-lookup"><span data-stu-id="73efa-111">Azure Diagnostics infrastructure logs</span></span>

<span data-ttu-id="73efa-112">Den här artikeln visar hur tooconfigure Azure Diagnostics med Händelsehubbar från avslutas tooend.</span><span class="sxs-lookup"><span data-stu-id="73efa-112">This article shows you how tooconfigure Azure Diagnostics with Event Hubs from end tooend.</span></span> <span data-ttu-id="73efa-113">Vägledning ges också för hello följande vanliga scenarier:</span><span class="sxs-lookup"><span data-stu-id="73efa-113">Guidance is also provided for hello following common scenarios:</span></span>

* <span data-ttu-id="73efa-114">Hur toocustomize hello loggar och mått som skickas tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="73efa-114">How toocustomize hello logs and metrics that get sent tooEvent Hubs</span></span>
* <span data-ttu-id="73efa-115">Hur toochange konfigurationer i varje miljö</span><span class="sxs-lookup"><span data-stu-id="73efa-115">How toochange configurations in each environment</span></span>
* <span data-ttu-id="73efa-116">Hur tooview Händelsehubbar strömma data</span><span class="sxs-lookup"><span data-stu-id="73efa-116">How tooview Event Hubs stream data</span></span>
* <span data-ttu-id="73efa-117">Hur tootroubleshoot hello anslutning</span><span class="sxs-lookup"><span data-stu-id="73efa-117">How tootroubleshoot hello connection</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="73efa-118">Krav</span><span class="sxs-lookup"><span data-stu-id="73efa-118">Prerequisites</span></span>
<span data-ttu-id="73efa-119">Event Hubs receieving data från Azure-diagnostik stöds i molntjänster, virtuella datorer, virtuella datorer och Service Fabric från och med hello Azure SDK 2.9 och hello motsvarande Azure-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73efa-119">Event Hubs receieving data from Azure Diagnostics is supported in Cloud Services, VMs, Virtual Machine Scale Sets, and Service Fabric starting in hello Azure SDK 2.9 and hello corresponding Azure Tools for Visual Studio.</span></span>

* <span data-ttu-id="73efa-120">Azure Diagnostics tillägget 1.6 ([Azure SDK för .NET 2.9 eller senare](https://azure.microsoft.com/downloads/) riktar sig mot detta som standard)</span><span class="sxs-lookup"><span data-stu-id="73efa-120">Azure Diagnostics extension 1.6 ([Azure SDK for .NET 2.9 or later](https://azure.microsoft.com/downloads/) targets this by default)</span></span>
* [<span data-ttu-id="73efa-121">Visual Studio 2013 eller senare</span><span class="sxs-lookup"><span data-stu-id="73efa-121">Visual Studio 2013 or later</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="73efa-122">Befintliga konfigurationer av Azure-diagnostik i ett program med hjälp av en *.wadcfgx* fil och en av hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="73efa-122">Existing configurations of Azure Diagnostics in an application by using a *.wadcfgx* file and one of hello following methods:</span></span>
  * <span data-ttu-id="73efa-123">Visual Studio: [konfigurera diagnostik för Azure-molntjänster och virtuella datorer](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span><span class="sxs-lookup"><span data-stu-id="73efa-123">Visual Studio: [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span></span>
  * <span data-ttu-id="73efa-124">Windows PowerShell: [aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="73efa-124">Windows PowerShell: [Enable diagnostics in Azure Cloud Services using PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span></span>
* <span data-ttu-id="73efa-125">Hubbar händelsenamnrymden etablerats per hello artikel [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="73efa-125">Event Hubs namespace provisioned per hello article, [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span></span>

## <a name="connect-azure-diagnostics-tooevent-hubs-sink"></a><span data-ttu-id="73efa-126">Anslut Azure Diagnostics tooEvent hubbar sink</span><span class="sxs-lookup"><span data-stu-id="73efa-126">Connect Azure Diagnostics tooEvent Hubs sink</span></span>
<span data-ttu-id="73efa-127">Som standard skickar Azure-diagnostik alltid loggar och mått tooan Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="73efa-127">By default, Azure Diagnostics always sends logs and metrics tooan Azure Storage account.</span></span> <span data-ttu-id="73efa-128">Ett program kan även skicka data tooEvent Hubs genom att lägga till en ny **egenskaperna** avsnitt under hello **PublicConfig** / **WadCfg** element av hello *.wadcfgx* fil.</span><span class="sxs-lookup"><span data-stu-id="73efa-128">An application may also send data tooEvent Hubs by adding a new **Sinks** section under hello **PublicConfig** / **WadCfg** element of hello *.wadcfgx* file.</span></span> <span data-ttu-id="73efa-129">I Visual Studio hello *.wadcfgx* lagras i hello följande sökväg: **Molntjänstprojekt** > **roller** > **() RoleName)** > **diagnostics.wadcfgx** fil.</span><span class="sxs-lookup"><span data-stu-id="73efa-129">In Visual Studio, hello *.wadcfgx* file is stored in hello following path: **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx** file.</span></span>

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

<span data-ttu-id="73efa-130">I det här exemplet hello händelsehubb URL anges toohello fullständigt kvalificerade namnområdet för hello händelsehubb: Händelsehubbar namnområde + ”/” + händelsehubbens namn.</span><span class="sxs-lookup"><span data-stu-id="73efa-130">In this example, hello event hub URL is set toohello fully qualified namespace of hello event hub: Event Hubs namespace  + "/" + event hub name.</span></span>  

<span data-ttu-id="73efa-131">Hej händelsehubb URL visas i hello [Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) på hello Händelsehubbar instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="73efa-131">hello event hub URL is displayed in hello [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) on hello Event Hubs dashboard.</span></span>  

<span data-ttu-id="73efa-132">Hej **Sink** namn kan ställas in tooany giltig sträng så länge hello samma värde används konsekvent i hela hello config-fil.</span><span class="sxs-lookup"><span data-stu-id="73efa-132">hello **Sink** name can be set tooany valid string as long as hello same value is used consistently throughout hello config file.</span></span>

> [!NOTE]
> <span data-ttu-id="73efa-133">Det kan finnas ytterligare sänkor som *applicationInsights* konfigurerats i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="73efa-133">There may be additional sinks, such as *applicationInsights* configured in this section.</span></span> <span data-ttu-id="73efa-134">Azure Diagnostics tillåter en eller flera egenskaperna toobe definieras om varje mottagare deklareras även i hello **PrivateConfig** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="73efa-134">Azure Diagnostics allows one or more sinks toobe defined if each sink is also declared in hello **PrivateConfig** section.</span></span>  
>
>

<span data-ttu-id="73efa-135">Hej Händelsehubbar sink måste också deklareras och definieras i hello **PrivateConfig** avsnitt i hello *.wadcfgx* config-fil.</span><span class="sxs-lookup"><span data-stu-id="73efa-135">hello Event Hubs sink must also be declared and defined in hello **PrivateConfig** section of hello *.wadcfgx* config file.</span></span>

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

<span data-ttu-id="73efa-136">Hej `SharedAccessKeyName` värdet måste matcha en delad signatur åtkomst (SAS) nyckel och en princip som har definierats i hello **Händelsehubbar** namnområde.</span><span class="sxs-lookup"><span data-stu-id="73efa-136">hello `SharedAccessKeyName` value must match a Shared Access Signature (SAS) key and policy that has been defined in hello **Event Hubs** namespace.</span></span> <span data-ttu-id="73efa-137">Bläddra toohello Händelsehubbar instrumentpanelen i hello [Azure-portalen](https://manage.windowsazure.com), klicka på hello **konfigurera** fliken och skapa en namngiven princip (till exempel ”SendRule”) som har *skicka* behörigheter.</span><span class="sxs-lookup"><span data-stu-id="73efa-137">Browse toohello Event Hubs dashboard in hello [Azure portal](https://manage.windowsazure.com), click hello **Configure** tab, and set up a named policy (for example, "SendRule") that has *Send* permissions.</span></span> <span data-ttu-id="73efa-138">Hej **StorageAccount** också har deklarerats i **PrivateConfig**.</span><span class="sxs-lookup"><span data-stu-id="73efa-138">hello **StorageAccount** is also declared in **PrivateConfig**.</span></span> <span data-ttu-id="73efa-139">Det behövs ingen här toochange värden om de fungerar.</span><span class="sxs-lookup"><span data-stu-id="73efa-139">There is no need toochange values here if they are working.</span></span> <span data-ttu-id="73efa-140">I det här exemplet lämnar vi hello värden tomt, vilket är ett tecken som att den underordnade resursen kommer att ange hello värden.</span><span class="sxs-lookup"><span data-stu-id="73efa-140">In this example, we leave hello values empty, which is a sign that a downstream asset will set hello values.</span></span> <span data-ttu-id="73efa-141">Till exempel hello *ServiceConfiguration.Cloud.cscfg* miljö konfigurationsfil anger hello miljö som är lämpliga namn och nycklar.</span><span class="sxs-lookup"><span data-stu-id="73efa-141">For example, hello *ServiceConfiguration.Cloud.cscfg* environment configuration file sets hello environment-appropriate names and keys.</span></span>  

> [!WARNING]
> <span data-ttu-id="73efa-142">hello Event Hubs SAS-nyckeln lagras i klartext i hello *.wadcfgx* fil.</span><span class="sxs-lookup"><span data-stu-id="73efa-142">hello Event Hubs SAS key is stored in plain text in hello *.wadcfgx* file.</span></span> <span data-ttu-id="73efa-143">Ofta den här nyckeln är markerad i toosource för källkodskontroll eller är tillgänglig som en tillgång i build-servern så bör du skydda den efter behov.</span><span class="sxs-lookup"><span data-stu-id="73efa-143">Often, this key is checked in toosource code control or is available as an asset in your build server, so you should protect it as appropriate.</span></span> <span data-ttu-id="73efa-144">Vi rekommenderar att du använder SAS-nyckeln här med *Skicka bara* behörigheter så att en obehörig användare inte kan skriva toohello händelsehubb, men lyssna tooit eller hantera den.</span><span class="sxs-lookup"><span data-stu-id="73efa-144">We recommend that you use a SAS key here with *Send only* permissions so that a malicious user can write toohello event hub, but not listen tooit or manage it.</span></span>
>
>

## <a name="configure-azure-diagnostics-toosend-logs-and-metrics-tooevent-hubs"></a><span data-ttu-id="73efa-145">Konfigurera Azure-diagnostik toosend loggar och mått tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="73efa-145">Configure Azure Diagnostics toosend logs and metrics tooEvent Hubs</span></span>
<span data-ttu-id="73efa-146">Som diskuterats tidigare alla standard- och anpassade diagnostikdata, det vill säga skickas loggar, mått och automatiskt tooAzure lagring i hello konfigurerade intervallen.</span><span class="sxs-lookup"><span data-stu-id="73efa-146">As discussed previously, all default and custom diagnostics data, that is, metrics and logs, is automatically sent tooAzure Storage in hello configured intervals.</span></span> <span data-ttu-id="73efa-147">Du kan ange vilken nod som rot eller löv i hello hierarkin toobe skickas toohello händelsehubb med Händelsehubbar och eventuella ytterligare mottagare.</span><span class="sxs-lookup"><span data-stu-id="73efa-147">With Event Hubs and any additional sink, you can specify any root or leaf node in hello hierarchy toobe sent toohello event hub.</span></span> <span data-ttu-id="73efa-148">Detta inkluderar ETW-händelser, prestandaräknare, Windows-händelseloggar och programloggarna.</span><span class="sxs-lookup"><span data-stu-id="73efa-148">This includes ETW events, performance counters, Windows event logs, and application logs.</span></span>   

<span data-ttu-id="73efa-149">Det är viktigt tooconsider hur många datapunkter ska verkligen vara överförs tooEvent Hubs.</span><span class="sxs-lookup"><span data-stu-id="73efa-149">It is important tooconsider how many data points should actually be transferred tooEvent Hubs.</span></span> <span data-ttu-id="73efa-150">Vanligtvis dataöverföring utvecklare låg latens hot sökvägen som används och tolkas snabbt.</span><span class="sxs-lookup"><span data-stu-id="73efa-150">Typically, developers transfer low-latency hot-path data that must be consumed and interpreted quickly.</span></span> <span data-ttu-id="73efa-151">System som övervakar aviseringar eller Autoskala regler är exempel.</span><span class="sxs-lookup"><span data-stu-id="73efa-151">Systems that monitor alerts or autoscale rules are examples.</span></span> <span data-ttu-id="73efa-152">En utvecklare kan också konfigurera en alternativ analysis-butik eller Sök store – till exempel Azure Stream Analytics Elasticsearch, ett anpassat övervakningssystem eller en favorit övervakningssystemet från andra.</span><span class="sxs-lookup"><span data-stu-id="73efa-152">A developer might also configure an alternate analysis store or search store -- for example, Azure Stream Analytics, Elasticsearch, a custom monitoring system, or a favorite monitoring system from others.</span></span>

<span data-ttu-id="73efa-153">hello följande är några exempelkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="73efa-153">hello following are some example configurations.</span></span>

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

<span data-ttu-id="73efa-154">I hello ovan exempel hello sink är tillämpade toohello överordnade **PerformanceCounters** nod i hello-hierarkin, vilket innebär att alla underordnade **prestandaräknarna** skickas tooEvent Hubs.</span><span class="sxs-lookup"><span data-stu-id="73efa-154">In hello above example, hello sink is applied toohello parent **PerformanceCounters** node in hello hierarchy, which means all child **PerformanceCounters** will be sent tooEvent Hubs.</span></span>  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

<span data-ttu-id="73efa-155">I föregående exempel hello hello sink är tillämpade tooonly tre räknare: **begäranden i kö**, **begäranden avvisas**, och **% processortid**.</span><span class="sxs-lookup"><span data-stu-id="73efa-155">In hello previous example, hello sink is applied tooonly three counters: **Requests Queued**, **Requests Rejected**, and **% Processor time**.</span></span>  

<span data-ttu-id="73efa-156">hello följande exempel visas hur en utvecklare kan begränsa hello mängden data som skickats toobe hello kritiska mått som används för den här tjänstens hälsa.</span><span class="sxs-lookup"><span data-stu-id="73efa-156">hello following example shows how a developer can limit hello amount of sent data toobe hello critical metrics that are used for this service’s health.</span></span>  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

<span data-ttu-id="73efa-157">I det här exemplet hello sink är tillämpade toologs och filtrerad endast tooerror nivå trace.</span><span class="sxs-lookup"><span data-stu-id="73efa-157">In this example, hello sink is applied toologs and is filtered only tooerror level trace.</span></span>

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a><span data-ttu-id="73efa-158">Distribuera och uppdatera en molntjänster program och diagnostik config</span><span class="sxs-lookup"><span data-stu-id="73efa-158">Deploy and update a Cloud Services application and diagnostics config</span></span>
<span data-ttu-id="73efa-159">Visual Studio ger hello enklaste sökvägen toodeploy hello program och Händelsehubbar sink-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73efa-159">Visual Studio provides hello easiest path toodeploy hello application and Event Hubs sink configuration.</span></span> <span data-ttu-id="73efa-160">tooview och redigera hello-fil, öppna hello *.wadcfgx* filen i Visual Studio, redigera och spara den.</span><span class="sxs-lookup"><span data-stu-id="73efa-160">tooview and edit hello file, open hello *.wadcfgx* file in Visual Studio, edit it, and save it.</span></span> <span data-ttu-id="73efa-161">hello sökvägen är **Molntjänstprojekt** > **roller** > **(RoleName)** > **diagnostics.wadcfgx**.</span><span class="sxs-lookup"><span data-stu-id="73efa-161">hello path is **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx**.</span></span>  

<span data-ttu-id="73efa-162">Nu är alla och distributionen uppdatera åtgärder i Visual Studio, Visual Studio Team System och alla kommandon eller skript som är baserade på MSBuild och använda hello **/t: publicera** mål inkluderar hello *.wadcfgx*  hello paketering pågår.</span><span class="sxs-lookup"><span data-stu-id="73efa-162">At this point, all deployment and deployment update actions in Visual Studio, Visual Studio Team System, and all commands or scripts that are based on MSBuild and use hello **/t:publish** target include hello *.wadcfgx* in hello packaging process.</span></span> <span data-ttu-id="73efa-163">Dessutom distribuera distributioner och uppdateringar hello filen tooAzure med hello Azure Diagnostics agent filtillägg på dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="73efa-163">In addition, deployments and updates deploy hello file tooAzure by using hello appropriate Azure Diagnostics agent extension on your VMs.</span></span>

<span data-ttu-id="73efa-164">Aktiviteten i hello instrumentpanelen hello händelsehubbens visas direkt när du har distribuerat hello program och konfiguration av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="73efa-164">After you deploy hello application and Azure Diagnostics configuration, you will immediately see activity in hello dashboard of hello event hub.</span></span> <span data-ttu-id="73efa-165">Detta anger att du är klar toomove på tooviewing hello hot sökväg data i hello lyssnare klient- eller verktyg som helst.</span><span class="sxs-lookup"><span data-stu-id="73efa-165">This indicates that you're ready toomove on tooviewing hello hot-path data in hello listener client or analysis tool of your choice.</span></span>  

<span data-ttu-id="73efa-166">I följande bild hello, visar hello Händelsehubbar instrumentpanelen felfria skickar diagnostik data toohello event hub börjar stund efter kl.</span><span class="sxs-lookup"><span data-stu-id="73efa-166">In hello following figure, hello Event Hubs dashboard shows healthy sending of diagnostics data toohello event hub starting sometime after 11 PM.</span></span> <span data-ttu-id="73efa-167">Det är då hello programmet distribuerades med en uppdaterad *.wadcfgx* filen och hello mottagare har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="73efa-167">That's when hello application was deployed with an updated *.wadcfgx* file, and hello sink was configured properly.</span></span>

![][0]  

> [!NOTE]
> <span data-ttu-id="73efa-168">När du gör uppdateringar toohello Azure Diagnostics config-fil (.wadcfgx), bör du push hello uppdateringar toohello hela programmet samt hello konfiguration med hjälp av Visual Studio-publicering eller en Windows PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="73efa-168">When you make updates toohello Azure Diagnostics config file (.wadcfgx), it's recommended that you push hello updates toohello entire application as well as hello configuration by using either Visual Studio publishing, or a Windows PowerShell script.</span></span>  
>
>

## <a name="view-hot-path-data"></a><span data-ttu-id="73efa-169">Visa hot sökväg data</span><span class="sxs-lookup"><span data-stu-id="73efa-169">View hot-path data</span></span>
<span data-ttu-id="73efa-170">Som tidigare diskuterats, finns det många användningsområden för lyssnande tooand Händelsehubbar data bearbetas.</span><span class="sxs-lookup"><span data-stu-id="73efa-170">As discussed previously, there are many use cases for listening tooand processing Event Hubs data.</span></span>

<span data-ttu-id="73efa-171">En enkel metod är toocreate en test små konsolen programmet toolisten toohello händelsehubb och skriva ut hello utdataströmmen.</span><span class="sxs-lookup"><span data-stu-id="73efa-171">One simple approach is toocreate a small test console application toolisten toohello event hub and print hello output stream.</span></span> <span data-ttu-id="73efa-172">Du kan placera hello följande kod, som beskrivs i detalj i [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), i ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="73efa-172">You can place hello following code, which is explained in more detail in [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), in a console application.</span></span>  

<span data-ttu-id="73efa-173">Observera att hello konsolprogram måste innehålla hello [händelse Processor värden NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="73efa-173">Note that hello console application must include hello [Event Processor Host NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>  

<span data-ttu-id="73efa-174">Kom ihåg tooreplace hello värden hakparenteser i hello **Main** funktion med värden för dina resurser.</span><span class="sxs-lookup"><span data-stu-id="73efa-174">Remember tooreplace hello values in angle brackets in hello **Main** function with values for your resources.</span></span>   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key toostop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a><span data-ttu-id="73efa-175">Felsöka Händelsehubbar handfat</span><span class="sxs-lookup"><span data-stu-id="73efa-175">Troubleshoot Event Hubs sinks</span></span>
* <span data-ttu-id="73efa-176">Hej händelsehubb visar inte inkommande eller utgående händelseaktivitet som förväntat.</span><span class="sxs-lookup"><span data-stu-id="73efa-176">hello event hub does not show incoming or outgoing event activity as expected.</span></span>

    <span data-ttu-id="73efa-177">Kontrollera att din händelsehubb har etablerats.</span><span class="sxs-lookup"><span data-stu-id="73efa-177">Check that your event hub is successfully provisioned.</span></span> <span data-ttu-id="73efa-178">Alla anslutningsinformation i hello **PrivateConfig** avsnitt i *.wadcfgx* måste matcha hello värdena för din resurs som visas i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="73efa-178">All connection info in hello **PrivateConfig** section of *.wadcfgx* must match hello values of your resource as seen in hello portal.</span></span> <span data-ttu-id="73efa-179">Se till att du har en SAS princip definierad (”SendRule” hello exempel) i hello-portalen och som *skicka* behörigheten.</span><span class="sxs-lookup"><span data-stu-id="73efa-179">Make sure that you have a SAS policy defined ("SendRule" in hello example) in hello portal and that *Send* permission is granted.</span></span>  
* <span data-ttu-id="73efa-180">När en uppdatering visar inte längre inkommande eller utgående händelseaktivitet hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="73efa-180">After an update, hello event hub no longer shows incoming or outgoing event activity.</span></span>

    <span data-ttu-id="73efa-181">Kontrollera först att namnet är rätt som tidigare förklarats hello NAV- och händelseinformation.</span><span class="sxs-lookup"><span data-stu-id="73efa-181">First, make sure that hello event hub and configuration info is correct as explained previously.</span></span> <span data-ttu-id="73efa-182">Ibland hello **PrivateConfig** återställs i en distributionsuppdatering.</span><span class="sxs-lookup"><span data-stu-id="73efa-182">Sometimes hello **PrivateConfig** is reset in a deployment update.</span></span> <span data-ttu-id="73efa-183">hello rekommenderas korrigering är toomake alla ändringar för*.wadcfgx* i hello projektet och sedan push-installera en uppdatering för hela programmet.</span><span class="sxs-lookup"><span data-stu-id="73efa-183">hello recommended fix is toomake all changes too*.wadcfgx* in hello project and then push a complete application update.</span></span> <span data-ttu-id="73efa-184">Om det inte är möjligt, kontrollera att hello diagnostik update skickar en fullständig **PrivateConfig** som innehåller hello SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="73efa-184">If this is not possible, make sure that hello diagnostics update pushes a complete **PrivateConfig** that includes hello SAS key.</span></span>  
* <span data-ttu-id="73efa-185">Jag har försökt hello förslag och hello händelsehubb fortfarande fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="73efa-185">I tried hello suggestions, and hello event hub is still not working.</span></span>

    <span data-ttu-id="73efa-186">Titta i hello Azure Storage-tabell som innehåller loggarna och fel för Azure-diagnostik själva: **WADDiagnosticInfrastructureLogsTable**.</span><span class="sxs-lookup"><span data-stu-id="73efa-186">Try looking in hello Azure Storage table that contains logs and errors for Azure Diagnostics itself: **WADDiagnosticInfrastructureLogsTable**.</span></span> <span data-ttu-id="73efa-187">Ett alternativ är toouse ett verktyg som [Azure Lagringsutforskaren](http://www.storageexplorer.com) tooconnect toothis storage-konto, visa den här tabellen och Lägg till en fråga för tidsstämplar i hello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="73efa-187">One option is toouse a tool such as [Azure Storage Explorer](http://www.storageexplorer.com) tooconnect toothis storage account, view this table, and add a query for TimeStamp in hello last 24 hours.</span></span> <span data-ttu-id="73efa-188">Du kan använda hello verktyget tooexport en CSV-fil och öppna den i ett program, till exempel Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="73efa-188">You can use hello tool tooexport a .csv file and open it in an application such as Microsoft Excel.</span></span> <span data-ttu-id="73efa-189">Excel gör det enkelt toosearch för kort strängar som **EventHubs**, toosee vilka fel rapporteras.</span><span class="sxs-lookup"><span data-stu-id="73efa-189">Excel makes it easy toosearch for calling-card strings, such as **EventHubs**, toosee what error is reported.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="73efa-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73efa-190">Next steps</span></span>
<span data-ttu-id="73efa-191">• [Mer information om Händelsehubbar](https://azure.microsoft.com/services/event-hubs/)</span><span class="sxs-lookup"><span data-stu-id="73efa-191">•    [Learn more about Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span></span>

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a><span data-ttu-id="73efa-192">Bilaga: Slutföra exempel på Azure-diagnostik en konfigurationsfil (.wadcfgx)</span><span class="sxs-lookup"><span data-stu-id="73efa-192">Appendix: Complete Azure Diagnostics configuration file (.wadcfgx) example</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

<span data-ttu-id="73efa-193">hello kompletterande *ServiceConfiguration.Cloud.cscfg* för det här exemplet ser ut som följande hello.</span><span class="sxs-lookup"><span data-stu-id="73efa-193">hello complementary *ServiceConfiguration.Cloud.cscfg* for this example looks like hello following.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<span data-ttu-id="73efa-194">Motsvarande Json-baserade inställningar för virtuella datorer är följande:</span><span class="sxs-lookup"><span data-stu-id="73efa-194">Equivalent Json based settings for virtual machines is as follows:</span></span>
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
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
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="73efa-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73efa-195">Next steps</span></span>
<span data-ttu-id="73efa-196">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="73efa-196">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="73efa-197">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="73efa-197">Event Hubs overview</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="73efa-198">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="73efa-198">Create an event hub</span></span>](../event-hubs/event-hubs-create.md)
* [<span data-ttu-id="73efa-199">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="73efa-199">Event Hubs FAQ</span></span>](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
