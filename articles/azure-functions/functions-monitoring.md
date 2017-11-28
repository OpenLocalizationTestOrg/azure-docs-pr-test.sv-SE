---
title: aaaMonitoring Azure Functions | Microsoft Docs
description: "Lär dig hur toomonitor Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2016
ms.author: wesmc
ms.openlocfilehash: 254348d1cefce925654bd9532715b6def571e0ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-functions"></a><span data-ttu-id="94ab3-104">Övervaka Azure Functions</span><span class="sxs-lookup"><span data-stu-id="94ab3-104">Monitoring Azure Functions</span></span>

## <a name="overview"></a><span data-ttu-id="94ab3-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="94ab3-105">Overview</span></span> 


<span data-ttu-id="94ab3-106">Hej **övervakaren** för varje funktion kan du tooreview varje körning av en funktion.</span><span class="sxs-lookup"><span data-stu-id="94ab3-106">hello **Monitor** tab for each function allows you tooreview each execution of a function.</span></span>

![Azure fliken för övervakning av funktioner](./media/functions-monitoring/monitor-tab.png) 

<span data-ttu-id="94ab3-108">Om du klickar på en körning kan du tooreview hello varaktighet, indata, fel och associerade loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="94ab3-108">Clicking an execution allows you tooreview hello duration, input data, errors, and associated log files.</span></span> <span data-ttu-id="94ab3-109">Detta är användbart felsökning och prestandajustering dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="94ab3-109">This is useful debugging and performance tuning your functions.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="94ab3-110">När du använder hello [förbrukning som värd för planen](functions-overview.md#pricing) Azure Functions hello **övervakning** panelen i hello Funktionsapp översikt bladet visas inte några data.</span><span class="sxs-lookup"><span data-stu-id="94ab3-110">When using hello [Consumption hosting plan](functions-overview.md#pricing) for Azure Functions, hello **Monitoring** tile in hello Function App overview blade will not show any data.</span></span> <span data-ttu-id="94ab3-111">Detta beror på att hello plattform dynamiskt skalar och hanterar compute-instanser för dig, så de här måtten inte meningsfull på en plan för användning.</span><span class="sxs-lookup"><span data-stu-id="94ab3-111">This is because hello platform dynamically scales and manages compute instances for you, so these metrics are not meaningful on a Consumption plan.</span></span> <span data-ttu-id="94ab3-112">toomonitor hello användning av funktionen-appar, bör du istället använda hello vägledning i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="94ab3-112">toomonitor hello usage of your Function Apps, you should instead use hello guidance in this article.</span></span>
> 
> <span data-ttu-id="94ab3-113">hello följande skärmbild som visar ett exempel:</span><span class="sxs-lookup"><span data-stu-id="94ab3-113">hello following screen-shot shows an example:</span></span>
> 
> ![Övervakning på hello huvudsakliga resursbladet](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a><span data-ttu-id="94ab3-115">Realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="94ab3-115">Real-time monitoring</span></span>

<span data-ttu-id="94ab3-116">Realtidsövervakning är tillgänglig genom att klicka på **live händelseströmmen** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="94ab3-116">Real-time monitoring is available by clicking **live event stream** as shown below.</span></span> 

![Live händelse dataströmmen alternativ för hello övervakningsfliken](./media/functions-monitoring/monitor-tab-live-event-stream.png)

<span data-ttu-id="94ab3-118">hello direktsänd händelse dataströmmen ska visas i diagram registreringen i en ny webbläsarflik enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="94ab3-118">hello live event stream will be graphed in a new browser tab as shown below.</span></span> 

![Direktsänd händelse stream-exempel](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> <span data-ttu-id="94ab3-120">Det finns ett känt problem som kan medföra att dina data toofail toobe fylls i.</span><span class="sxs-lookup"><span data-stu-id="94ab3-120">There is a known issue that may cause your data toofail toobe populated.</span></span> <span data-ttu-id="94ab3-121">Om du får detta kan du behöva tooclose hello webbläsare fliken som innehåller hello live händelseströmmen och klicka sedan på **live händelseströmmen** igen tooallow den tooproperly fylla din händelsedata för dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="94ab3-121">If you experience this, you may need tooclose hello browser tab containing hello live event stream and then click **live event stream** again tooallow it tooproperly populate your event stream data.</span></span> 

<span data-ttu-id="94ab3-122">hello direktsänd händelse dataströmmen kommer kurva hello följande statistik för din funktion:</span><span class="sxs-lookup"><span data-stu-id="94ab3-122">hello live event stream will graph hello following statistics for your function:</span></span>

* <span data-ttu-id="94ab3-123">Körningar som startas per sekund</span><span class="sxs-lookup"><span data-stu-id="94ab3-123">Executions started per second</span></span>
* <span data-ttu-id="94ab3-124">Körningar som slutförs varje sekund</span><span class="sxs-lookup"><span data-stu-id="94ab3-124">Executions completed per second</span></span>
* <span data-ttu-id="94ab3-125">Körningar som misslyckas per sekund</span><span class="sxs-lookup"><span data-stu-id="94ab3-125">Executions failed per second</span></span>
* <span data-ttu-id="94ab3-126">Genomsnittlig körningstid i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="94ab3-126">Average execution time in milliseconds.</span></span>

<span data-ttu-id="94ab3-127">Statistiken är realtid men hello faktiska rita in av hello Körningsdata kanske cirka 10 sekunder svarstid.</span><span class="sxs-lookup"><span data-stu-id="94ab3-127">These statistics are real-time but hello actual graphing of hello execution data may have around 10 seconds of latency.</span></span>






## <a name="monitoring-log-files-from-a-command-line"></a><span data-ttu-id="94ab3-128">Övervaka loggfilerna från en kommandorad</span><span class="sxs-lookup"><span data-stu-id="94ab3-128">Monitoring log files from a command line</span></span>


<span data-ttu-id="94ab3-129">Du kan strömma loggen filer tooa kommandoraden session på en lokal arbetsstation med hello Azure-kommandoradsgränssnittet (CLI) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="94ab3-129">You can stream log files tooa command line session on a local workstation using hello Azure Command Line Interface (CLI) or PowerShell.</span></span>

### <a name="monitoring-function-app-log-files-with-hello-azure-cli"></a><span data-ttu-id="94ab3-130">Övervakning av funktionen app-loggfiler med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="94ab3-130">Monitoring function app log files with hello Azure CLI</span></span>

<span data-ttu-id="94ab3-131">tooget har startats [installera hello Azure CLI](../cli-install-nodejs.md)</span><span class="sxs-lookup"><span data-stu-id="94ab3-131">tooget started, [install hello Azure CLI](../cli-install-nodejs.md)</span></span>

<span data-ttu-id="94ab3-132">Logga in på ditt Azure-konto med hjälp av hello följande kommando, eller andra hello andra alternativ som beskrivs i, [logga in tooAzure från hello Azure CLI](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="94ab3-132">Log into your Azure account using hello following command, or any of hello other options covered in, [Log in tooAzure from hello Azure CLI](../xplat-cli-connect.md).</span></span>

    azure login

<span data-ttu-id="94ab3-133">Använd hello följande kommando tooenable Azure CLI Service Management (ASM) läge:.</span><span class="sxs-lookup"><span data-stu-id="94ab3-133">Use hello following command tooenable Azure CLI Service Management (ASM) mode:.</span></span>

    azure config mode asm

<span data-ttu-id="94ab3-134">Om du har flera prenumerationer använder du följande kommandon toolist hello dina prenumerationer och ange hello aktuell prenumeration toohello prenumeration som innehåller appen funktionen.</span><span class="sxs-lookup"><span data-stu-id="94ab3-134">If you have multiple subscriptions, use hello following commands toolist your subscriptions and set hello current subscription toohello subscription that contains your function app.</span></span>

    azure account list
    azure account set <subscriptionNameOrId>

<span data-ttu-id="94ab3-135">hello direktuppspelas följande kommando hello loggfiler funktionen app toohello kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="94ab3-135">hello following command will stream hello log files of your function app toohello command line:</span></span>

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a><span data-ttu-id="94ab3-136">Övervakning av funktionen app-loggfiler med PowerShell</span><span class="sxs-lookup"><span data-stu-id="94ab3-136">Monitoring function app log files with PowerShell</span></span>

<span data-ttu-id="94ab3-137">tooget har startats [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="94ab3-137">tooget started, [install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="94ab3-138">Lägg till ditt Azure-konto genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="94ab3-138">Add your Azure account by running hello following command:</span></span>

    PS C:\> Add-AzureAccount

<span data-ttu-id="94ab3-139">Om du har flera prenumerationer, kan du visa dem efter namn med hello efter kommandot toosee om hello rätt prenumerationen är hello markerade baserat på `IsCurrent` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="94ab3-139">If you have multiple subscriptions, you can list them by name with hello following command toosee if hello correct subscription is hello currently selected based on `IsCurrent` property:</span></span>

    PS C:\> Get-AzureSubscription

<span data-ttu-id="94ab3-140">Om du behöver tooset hello aktiv prenumeration toohello ett som innehåller appen funktionen använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="94ab3-140">If you need tooset hello active subscription toohello one containing your function app, use hello following command:</span></span>

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

<span data-ttu-id="94ab3-141">Dataströmmen hello loggar tooyour PowerShell-session med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="94ab3-141">Stream hello logs tooyour PowerShell session with hello following command:</span></span>

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

<span data-ttu-id="94ab3-142">Mer information finns för[så här: strömma loggar för web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span><span class="sxs-lookup"><span data-stu-id="94ab3-142">For more information refer too[How to: Stream logs for web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="94ab3-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94ab3-143">Next steps</span></span>
<span data-ttu-id="94ab3-144">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="94ab3-144">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="94ab3-145">Testa en funktion</span><span class="sxs-lookup"><span data-stu-id="94ab3-145">Testing a function</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="94ab3-146">Skala en funktion</span><span class="sxs-lookup"><span data-stu-id="94ab3-146">Scale a function</span></span>](functions-scale.md)

