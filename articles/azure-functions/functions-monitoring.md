---
title: "Övervaka Azure Functions | Microsoft Docs"
description: "Lär dig hur du övervakar dina Azure-funktioner."
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
ms.openlocfilehash: b70214593b1417265387f42306a633bb0df2920e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-functions"></a><span data-ttu-id="2686d-104">Övervaka Azure Functions</span><span class="sxs-lookup"><span data-stu-id="2686d-104">Monitoring Azure Functions</span></span>

## <a name="overview"></a><span data-ttu-id="2686d-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="2686d-105">Overview</span></span> 


<span data-ttu-id="2686d-106">Den **övervakaren** för varje funktion kan du granska varje körning av en funktion.</span><span class="sxs-lookup"><span data-stu-id="2686d-106">The **Monitor** tab for each function allows you to review each execution of a function.</span></span>

![Azure fliken för övervakning av funktioner](./media/functions-monitoring/monitor-tab.png) 

<span data-ttu-id="2686d-108">Klicka på en körning kan du granska varaktighet, indata, fel och associerade loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="2686d-108">Clicking an execution allows you to review the duration, input data, errors, and associated log files.</span></span> <span data-ttu-id="2686d-109">Detta är användbart felsökning och prestandajustering dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="2686d-109">This is useful debugging and performance tuning your functions.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="2686d-110">När du använder den [förbrukning som värd för planen](functions-overview.md#pricing) för Azure Functions i **övervakning** panelen i bladet Funktionsapp översikt visas inte några data.</span><span class="sxs-lookup"><span data-stu-id="2686d-110">When using the [Consumption hosting plan](functions-overview.md#pricing) for Azure Functions, the **Monitoring** tile in the Function App overview blade will not show any data.</span></span> <span data-ttu-id="2686d-111">Detta beror på att plattformen dynamiskt skalar och hanterar compute-instanser för dig, så de här måtten inte meningsfull på en plan för användning.</span><span class="sxs-lookup"><span data-stu-id="2686d-111">This is because the platform dynamically scales and manages compute instances for you, so these metrics are not meaningful on a Consumption plan.</span></span> <span data-ttu-id="2686d-112">För att övervaka användningen av funktionen-appar, bör du istället använda riktlinjerna i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2686d-112">To monitor the usage of your Function Apps, you should instead use the guidance in this article.</span></span>
> 
> <span data-ttu-id="2686d-113">Följande skärmbild visar ett exempel:</span><span class="sxs-lookup"><span data-stu-id="2686d-113">The following screen-shot shows an example:</span></span>
> 
> ![Övervakning på huvudsakliga resursbladet](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a><span data-ttu-id="2686d-115">Realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="2686d-115">Real-time monitoring</span></span>

<span data-ttu-id="2686d-116">Realtidsövervakning är tillgänglig genom att klicka på **live händelseströmmen** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="2686d-116">Real-time monitoring is available by clicking **live event stream** as shown below.</span></span> 

![Direktsänd händelse dataströmmen alternativ för övervakning](./media/functions-monitoring/monitor-tab-live-event-stream.png)

<span data-ttu-id="2686d-118">Dataströmmen direktsänd händelse ska visas i diagram registreringen i en ny webbläsarflik enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="2686d-118">The live event stream will be graphed in a new browser tab as shown below.</span></span> 

![Direktsänd händelse stream-exempel](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> <span data-ttu-id="2686d-120">Det finns ett känt problem som kan orsaka att data ska kunna fyllas i.</span><span class="sxs-lookup"><span data-stu-id="2686d-120">There is a known issue that may cause your data to fail to be populated.</span></span> <span data-ttu-id="2686d-121">Om du får detta kan du behöva Stäng fliken som innehåller dataströmmen direktsänd händelse och klicka sedan på **live händelseströmmen** igen för att göra det möjligt att fylla i din händelsedata dataströmmen korrekt.</span><span class="sxs-lookup"><span data-stu-id="2686d-121">If you experience this, you may need to close the browser tab containing the live event stream and then click **live event stream** again to allow it to properly populate your event stream data.</span></span> 

<span data-ttu-id="2686d-122">Live händelseströmmen kommer kurva följande statistik för din funktion:</span><span class="sxs-lookup"><span data-stu-id="2686d-122">The live event stream will graph the following statistics for your function:</span></span>

* <span data-ttu-id="2686d-123">Körningar som startas per sekund</span><span class="sxs-lookup"><span data-stu-id="2686d-123">Executions started per second</span></span>
* <span data-ttu-id="2686d-124">Körningar som slutförs varje sekund</span><span class="sxs-lookup"><span data-stu-id="2686d-124">Executions completed per second</span></span>
* <span data-ttu-id="2686d-125">Körningar som misslyckas per sekund</span><span class="sxs-lookup"><span data-stu-id="2686d-125">Executions failed per second</span></span>
* <span data-ttu-id="2686d-126">Genomsnittlig körningstid i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="2686d-126">Average execution time in milliseconds.</span></span>

<span data-ttu-id="2686d-127">Statistiken är realtid men den faktiska grafiska Körningsdata kanske cirka 10 sekunder svarstid.</span><span class="sxs-lookup"><span data-stu-id="2686d-127">These statistics are real-time but the actual graphing of the execution data may have around 10 seconds of latency.</span></span>






## <a name="monitoring-log-files-from-a-command-line"></a><span data-ttu-id="2686d-128">Övervaka loggfilerna från en kommandorad</span><span class="sxs-lookup"><span data-stu-id="2686d-128">Monitoring log files from a command line</span></span>


<span data-ttu-id="2686d-129">Du kan strömma loggfiler till en session på kommandoraden på en lokal arbetsstation med Azure-kommandoradsgränssnittet (CLI) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2686d-129">You can stream log files to a command line session on a local workstation using the Azure Command Line Interface (CLI) or PowerShell.</span></span>

### <a name="monitoring-function-app-log-files-with-the-azure-cli"></a><span data-ttu-id="2686d-130">Övervakning av funktionen app-loggfiler med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2686d-130">Monitoring function app log files with the Azure CLI</span></span>

<span data-ttu-id="2686d-131">Du kommer igång [installerar Azure CLI](../cli-install-nodejs.md)</span><span class="sxs-lookup"><span data-stu-id="2686d-131">To get started, [install the Azure CLI](../cli-install-nodejs.md)</span></span>

<span data-ttu-id="2686d-132">Logga in på ditt Azure-konto med hjälp av följande kommando, eller några av de alternativ som beskrivs i, [logga in till Azure från Azure CLI](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="2686d-132">Log into your Azure account using the following command, or any of the other options covered in, [Log in to Azure from the Azure CLI](../xplat-cli-connect.md).</span></span>

    azure login

<span data-ttu-id="2686d-133">Använd följande kommando för att aktivera Azure CLI Service Management (ASM) läge:.</span><span class="sxs-lookup"><span data-stu-id="2686d-133">Use the following command to enable Azure CLI Service Management (ASM) mode:.</span></span>

    azure config mode asm

<span data-ttu-id="2686d-134">Om du har flera prenumerationer, använder du följande kommandon lista dina prenumerationer och ange den aktuella prenumerationen till den prenumeration som innehåller funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="2686d-134">If you have multiple subscriptions, use the following commands to list your subscriptions and set the current subscription to the subscription that contains your function app.</span></span>

    azure account list
    azure account set <subscriptionNameOrId>

<span data-ttu-id="2686d-135">Följande kommando direktuppspelas loggfilerna för funktionen appen till kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="2686d-135">The following command will stream the log files of your function app to the command line:</span></span>

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a><span data-ttu-id="2686d-136">Övervakning av funktionen app-loggfiler med PowerShell</span><span class="sxs-lookup"><span data-stu-id="2686d-136">Monitoring function app log files with PowerShell</span></span>

<span data-ttu-id="2686d-137">Du kommer igång [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2686d-137">To get started, [install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="2686d-138">Lägg till ditt Azure-konto genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2686d-138">Add your Azure account by running the following command:</span></span>

    PS C:\> Add-AzureAccount

<span data-ttu-id="2686d-139">Om du har flera prenumerationer, kan du visa dem med namn med följande kommando för att se om korrekt prenumeration är den markerade baserat på `IsCurrent` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="2686d-139">If you have multiple subscriptions, you can list them by name with the following command to see if the correct subscription is the currently selected based on `IsCurrent` property:</span></span>

    PS C:\> Get-AzureSubscription

<span data-ttu-id="2686d-140">Om du behöver ange aktiv prenumeration till den som innehåller appen funktionen använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2686d-140">If you need to set the active subscription to the one containing your function app, use the following command:</span></span>

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

<span data-ttu-id="2686d-141">Strömma loggar i PowerShell-sessionen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2686d-141">Stream the logs to your PowerShell session with the following command:</span></span>

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

<span data-ttu-id="2686d-142">Mer information finns i [så här: strömma loggar för web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span><span class="sxs-lookup"><span data-stu-id="2686d-142">For more information refer to [How to: Stream logs for web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2686d-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2686d-143">Next steps</span></span>
<span data-ttu-id="2686d-144">Mer information finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="2686d-144">For more information, see the following resources:</span></span>

* [<span data-ttu-id="2686d-145">Testa en funktion</span><span class="sxs-lookup"><span data-stu-id="2686d-145">Testing a function</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="2686d-146">Skala en funktion</span><span class="sxs-lookup"><span data-stu-id="2686d-146">Scale a function</span></span>](functions-scale.md)

