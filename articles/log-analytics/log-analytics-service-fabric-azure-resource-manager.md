---
title: "aaaAssess Service Fabric-program med Log Analytics med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Du kan använda hello Service Fabric-lösning i Log Analytics med hjälp av hello Azure portal tooassess hello risk och hälsotillståndet hos Service Fabric-program micro-tjänster, noder och kluster."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 891c7f6e5ed511ac18599bdc280ab3dc09700fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-hello-azure-portal"></a><span data-ttu-id="0c07f-103">Utvärdera Service Fabric-program och micro-tjänster med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0c07f-103">Assess Service Fabric applications and micro-services with hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0c07f-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0c07f-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="0c07f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c07f-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>

![Service Fabric-symbolen](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="0c07f-107">Den här artikeln beskrivs hur toouse hello Service Fabric-lösning i logganalys toohelp identifiera och felsöka problem i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0c07f-107">This article describes how toouse hello Service Fabric solution in Log Analytics toohelp identify and troubleshoot issues across your Service Fabric cluster.</span></span>

<span data-ttu-id="0c07f-108">hello Service Fabric-lösningen använder Azure-diagnostik data från din Service Fabric virtuella datorer genom att samla in data från Azure BOMULLSTUSS tabellerna.</span><span class="sxs-lookup"><span data-stu-id="0c07f-108">hello Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="0c07f-109">Logganalys läser sedan Service Fabric framework händelser, inklusive **händelser för tillförlitlig tjänst**, **aktören händelser**, **drifthändelser**, och **anpassad ETW-händelser**.</span><span class="sxs-lookup"><span data-stu-id="0c07f-109">Log Analytics then reads Service Fabric framework events, including **Reliable Service Events**, **Actor Events**, **Operational Events**, and **Custom ETW events**.</span></span> <span data-ttu-id="0c07f-110">Med hello lösning instrumentpanel är du kan tooview anmärkningsvärda problem och relevanta händelser i Service Fabric-miljö.</span><span class="sxs-lookup"><span data-stu-id="0c07f-110">With hello solution dashboard, you are able tooview notable issues and relevant events in your Service Fabric environment.</span></span>

<span data-ttu-id="0c07f-111">tooget igång med hello lösning, behöver du tooconnect Service Fabric-kluster tooa logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-111">tooget started with hello solution, you need tooconnect your Service Fabric cluster tooa Log Analytics workspace.</span></span> <span data-ttu-id="0c07f-112">Här följer tre scenarier tooconsider:</span><span class="sxs-lookup"><span data-stu-id="0c07f-112">Here are three scenarios tooconsider:</span></span>

1. <span data-ttu-id="0c07f-113">Om du inte har distribuerat Service Fabric-kluster använder hello stegen i ***distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan*** toodeploy ett nytt kluster och har konfigurerat tooreport tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="0c07f-113">If you have not deployed your Service Fabric cluster, use hello steps in ***Deploy a Service Fabric Cluster connected tooa Log Analytics workspace*** toodeploy a new cluster and have it configured tooreport tooLog Analytics.</span></span>
2. <span data-ttu-id="0c07f-114">Om du behöver toocollect prestandaräknare från värdar-toouse andra OMS-lösningar, till exempel säkerhet på Service Fabric-kluster, gör hello i ***distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan med VM-tillägg installerad.***</span><span class="sxs-lookup"><span data-stu-id="0c07f-114">If you need toocollect performance counters from your hosts toouse other OMS solutions such as Security on your Service Fabric Cluster, follow hello steps in ***Deploy a Service Fabric Cluster connected tooa Log Analytics workspace with VM Extension installed.***</span></span>
3. <span data-ttu-id="0c07f-115">Om du redan har distribuerat Service Fabric-kluster och vill tooconnect den tooLog Analytics, gör hello i ***att lägga till en befintlig lagring konto tooLog Analytics.***</span><span class="sxs-lookup"><span data-stu-id="0c07f-115">If you have already deployed your Service Fabric cluster and want tooconnect it tooLog Analytics, follow hello steps in ***Adding an existing storage account tooLog Analytics.***</span></span>

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace"></a><span data-ttu-id="0c07f-116">Distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-116">Deploy a Service Fabric Cluster connected tooa Log Analytics workspace.</span></span>
<span data-ttu-id="0c07f-117">Den här mallen hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c07f-117">This template does hello following:</span></span>

1. <span data-ttu-id="0c07f-118">Distribuerar en Azure Service Fabric klustret redan ansluten tooa logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-118">Deploys an Azure Service Fabric cluster already connected tooa Log Analytics workspace.</span></span> <span data-ttu-id="0c07f-119">Du har hello alternativet toocreate arbetsyta vid distribution av hello mall eller inkommande hello namnet på en redan befintlig logganalys-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="0c07f-119">You have hello option toocreate a new workspace while deploying hello template, or input hello name of an already existing Log Analytics workspace.</span></span>
2. <span data-ttu-id="0c07f-120">Lägger till hello diagnostiska storage-konto toohello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-120">Adds hello diagnostic storage account toohello Log Analytics workspace.</span></span>
3. <span data-ttu-id="0c07f-121">Aktiverar hello Service Fabric-lösning i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-121">Enables hello Service Fabric solution in your Log Analytics workspace.</span></span>

<span data-ttu-id="0c07f-122">[![Distribuera tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0c07f-122">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="0c07f-123">När du har valt hello distribuera ovan, hello Azure portal öppnas med parametrar du tooedit.</span><span class="sxs-lookup"><span data-stu-id="0c07f-123">Once you select hello deploy button above, hello Azure portal opens with parameters for you tooedit.</span></span> <span data-ttu-id="0c07f-124">Vara säker på att toocreate en ny resursgrupp om du ange ett nytt namn för logganalys-arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="0c07f-124">Be sure toocreate a new resource group if you input a new Log Analytics workspace name:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

<span data-ttu-id="0c07f-127">Godkänn hello juridiska villkoren och klicka på **skapa** toostart hello distribution.</span><span class="sxs-lookup"><span data-stu-id="0c07f-127">Accept hello legal terms and click **Create** toostart hello deployment.</span></span> <span data-ttu-id="0c07f-128">När hello distributionen är klar du bör se hello ny arbetsyta och kluster skapas och hello WADServiceFabric * händelse, WADWindowsEventLogs och WADETWEvent tabeller som har lagts till:</span><span class="sxs-lookup"><span data-stu-id="0c07f-128">Once hello deployment is completed, you should see hello new workspace and cluster created, and hello WADServiceFabric*Event, WADWindowsEventLogs and WADETWEvent tables added:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace-with-vm-extension-installed"></a><span data-ttu-id="0c07f-130">Distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan med VM-tillägg som är installerad.</span><span class="sxs-lookup"><span data-stu-id="0c07f-130">Deploy a Service Fabric Cluster connected tooa Log Analytics workspace with VM Extension installed.</span></span>

<span data-ttu-id="0c07f-131">Den här mallen hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c07f-131">This template does hello following:</span></span>

1. <span data-ttu-id="0c07f-132">Distribuerar en Azure Service Fabric klustret redan ansluten tooa logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-132">Deploys an Azure Service Fabric cluster already connected tooa Log Analytics workspace.</span></span> <span data-ttu-id="0c07f-133">Du kan skapa en ny arbetsyta eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="0c07f-133">You can create a new workspace or use an existing one.</span></span>
2. <span data-ttu-id="0c07f-134">Lägger till hello diagnostiska storage-konton toohello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-134">Adds hello diagnostic storage accounts toohello Log Analytics workspace.</span></span>
3. <span data-ttu-id="0c07f-135">Aktiverar hello Service Fabric-lösning i hello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-135">Enables hello Service Fabric solution in hello Log Analytics workspace.</span></span>
4. <span data-ttu-id="0c07f-136">Installerar tillägget för hello MMA agent i varje virtuell dator skala i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="0c07f-136">Installs hello MMA agent extension in each virtual machine scale set in your Service Fabric cluster.</span></span> <span data-ttu-id="0c07f-137">Med hello MMA-agenten är installerad, är du kan tooview prestandamått om noderna.</span><span class="sxs-lookup"><span data-stu-id="0c07f-137">With hello MMA agent installed, you are able tooview performance metrics about your nodes.</span></span>

<span data-ttu-id="0c07f-138">[![Distribuera tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0c07f-138">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="0c07f-139">Följande hello samma stegen ovan inkommande hello nödvändiga parametrar och startar en distribution.</span><span class="sxs-lookup"><span data-stu-id="0c07f-139">Following hello same steps above, input hello necessary parameters, and kick off a deployment.</span></span> <span data-ttu-id="0c07f-140">Igen bör du se hello ny arbetsyta, kluster och BOMULLSTUSS tabeller alla skapade:</span><span class="sxs-lookup"><span data-stu-id="0c07f-140">Once again you should see hello new workspace, cluster and WAD tables all created:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a><span data-ttu-id="0c07f-142">Visar prestandadata</span><span class="sxs-lookup"><span data-stu-id="0c07f-142">Viewing Performance Data</span></span>

<span data-ttu-id="0c07f-143">tooview prestandadata från din noder:</span><span class="sxs-lookup"><span data-stu-id="0c07f-143">tooview Perf Data from your nodes:</span></span>


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- <span data-ttu-id="0c07f-144">Starta hello logganalys-arbetsytan från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c07f-144">Launch hello Log Analytics workspace from hello Azure portal.</span></span>
  <span data-ttu-id="0c07f-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span><span class="sxs-lookup"><span data-stu-id="0c07f-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span></span>
- <span data-ttu-id="0c07f-146">Gå tooSettings hello vänster och välj Data >> Windows prestandaräknare >> ”hello Lägg till valda prestandaräknare”: ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span><span class="sxs-lookup"><span data-stu-id="0c07f-146">Go tooSettings on hello left pane, and select Data >> Windows Performance Counters >> "Add hello selected performance counters": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span></span>
- <span data-ttu-id="0c07f-147">Använd hello följande frågor toodelve till nyckelvärden om noderna i loggen sökning:</span><span class="sxs-lookup"><span data-stu-id="0c07f-147">In Log Search, use hello following queries toodelve into key metrics about your nodes:</span></span>

    <span data-ttu-id="0c07f-148">a.</span><span class="sxs-lookup"><span data-stu-id="0c07f-148">a.</span></span> <span data-ttu-id="0c07f-149">Jämför hello Genomsnittlig CPU-användning för alla noder i hello senast en timme toosee vilka noder har problem och med vilka tidsintervall en nod har en topp:</span><span class="sxs-lookup"><span data-stu-id="0c07f-149">Compare hello average CPU Utilization across all your nodes in hello last one hour toosee which nodes are having issues and at what time interval a node had a spike:</span></span>

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    <span data-ttu-id="0c07f-151">b.</span><span class="sxs-lookup"><span data-stu-id="0c07f-151">b.</span></span> <span data-ttu-id="0c07f-152">Visa liknande linjediagram för tillgängligt minne på varje nod med den här frågan:</span><span class="sxs-lookup"><span data-stu-id="0c07f-152">View similar line charts for available memory on each node with this query:</span></span>

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    <span data-ttu-id="0c07f-153">tooview en lista över alla noder, visar hello exakt medelvärdet för tillgängliga megabyte för varje nod, Använd den här frågan:</span><span class="sxs-lookup"><span data-stu-id="0c07f-153">tooview a listing of all your nodes, showing hello exact average value for Available MBytes for each node, use this query:</span></span>

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    <span data-ttu-id="0c07f-155">c.</span><span class="sxs-lookup"><span data-stu-id="0c07f-155">c.</span></span> <span data-ttu-id="0c07f-156">I hello fall som du vill toodrill ned till en viss nod genom att undersöka hello timvis medelvärde, lägsta, högsta och 75 percentil CPU-användning, du är kan toodo detta genom att med hjälp av den här frågan (Ersätt datorn fältet):</span><span class="sxs-lookup"><span data-stu-id="0c07f-156">In hello case that you want toodrill down into a specific node by examining hello hourly average, minimum, maximum and 75-percentile CPU usage, you're able toodo this by using this query (replace Computer field):</span></span>

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

<span data-ttu-id="0c07f-158">Mer information om prestandamått i logganalys på hello [Operations Management Suite-blogg](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span><span class="sxs-lookup"><span data-stu-id="0c07f-158">Read more information about performance metrics in Log Analytics at hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span></span>


## <a name="adding-an-existing-storage-account-toolog-analytics"></a><span data-ttu-id="0c07f-159">Lägga till en befintlig lagring konto tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="0c07f-159">Adding an existing storage account tooLog Analytics</span></span>

<span data-ttu-id="0c07f-160">Den här mallen lägger bara till befintlig lagring konton tooa ny eller befintlig logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-160">This template simply adds your existing storage accounts tooa new or existing Log Analytics workspace.</span></span>

<span data-ttu-id="0c07f-161">[![Distribuera tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0c07f-161">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span></span>

> [!NOTE]
> <span data-ttu-id="0c07f-162">Att välja en resursgrupp, Välj om du arbetar med en redan befintlig logganalys-arbetsyta ”Använd befintligt” och Sök efter hello resursgruppen som innehåller hello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-162">In selecting a Resource Group, if you're working with an already existing Log Analytics workspace, select "Use Existing" and search for hello resource group containing hello Log Analytics workspace.</span></span> <span data-ttu-id="0c07f-163">Skapa en ny en om annars.</span><span class="sxs-lookup"><span data-stu-id="0c07f-163">Create a new one if otherwise.</span></span>
> <span data-ttu-id="0c07f-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span><span class="sxs-lookup"><span data-stu-id="0c07f-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span></span>
>
>

<span data-ttu-id="0c07f-165">När den här mallen har distribuerats, kommer du att kunna toosee hello lagring konto ansluten tooyour logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-165">After this template has been deployed, you will be able toosee hello storage account connected tooyour Log Analytics workspace.</span></span> <span data-ttu-id="0c07f-166">I den här instansen jag har lagt till en mer lagringsutrymme konto toohello Exchange arbetsytan jag skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="0c07f-166">In this instance, I added one more storage account toohello Exchange workspace I created above.</span></span>
<span data-ttu-id="0c07f-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span><span class="sxs-lookup"><span data-stu-id="0c07f-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span></span>

## <a name="view-service-fabric-events"></a><span data-ttu-id="0c07f-168">Visa händelser för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0c07f-168">View Service Fabric events</span></span>

<span data-ttu-id="0c07f-169">När hello distributioner har slutförts och hello Service Fabric-lösningen har aktiverats på arbetsytan, Välj hello **Service Fabric** panelen i hello logganalys portal toolaunch hello Service Fabric-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0c07f-169">Once hello deployments are completed and hello Service Fabric solution has been enabled in your workspace, select hello **Service Fabric** tile in hello Log Analytics portal toolaunch hello Service Fabric dashboard.</span></span> <span data-ttu-id="0c07f-170">hello instrumentpanelen innehåller hello kolumner i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0c07f-170">hello dashboard includes hello columns in hello following table.</span></span> <span data-ttu-id="0c07f-171">Varje kolumn visar hello översta 10 händelser efter antal matchande att kolumnens sökvillkor för hello angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="0c07f-171">Each column lists hello top 10 events by count matching that column's criteria for hello specified time range.</span></span> <span data-ttu-id="0c07f-172">Du kan köra en sökning i logg som innehåller hello hela listan genom att klicka på **se alla** längst ned hello rätt i varje kolumn, eller klicka på kolumnrubriken hello.</span><span class="sxs-lookup"><span data-stu-id="0c07f-172">You can run a log search that provides hello entire list by clicking **See all** at hello right bottom of each column, or by clicking hello column header.</span></span>

| <span data-ttu-id="0c07f-173">**Service Fabric-händelse**</span><span class="sxs-lookup"><span data-stu-id="0c07f-173">**Service Fabric event**</span></span> | <span data-ttu-id="0c07f-174">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="0c07f-174">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="0c07f-175">Anmärkningsvärda problem</span><span class="sxs-lookup"><span data-stu-id="0c07f-175">Notable Issues</span></span> |<span data-ttu-id="0c07f-176">En översikt över problem som till exempel RunAsyncFailures RunAsynCancellations och noden används.</span><span class="sxs-lookup"><span data-stu-id="0c07f-176">A Display of issues such as RunAsyncFailures RunAsynCancellations and Node Downs.</span></span> |
| <span data-ttu-id="0c07f-177">Operativa händelser</span><span class="sxs-lookup"><span data-stu-id="0c07f-177">Operational Events</span></span> |<span data-ttu-id="0c07f-178">Viktiga operativa händelser, till exempel distributioner och uppgradering av programmet.</span><span class="sxs-lookup"><span data-stu-id="0c07f-178">Notable operational events such as application upgrade and deployments.</span></span> |
| <span data-ttu-id="0c07f-179">Händelser för tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="0c07f-179">Reliable Service Events</span></span> |<span data-ttu-id="0c07f-180">Viktiga tillförlitlig tjänsthändelser sådana Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="0c07f-180">Notable reliable service events such a Runasyncinvocations.</span></span> |
| <span data-ttu-id="0c07f-181">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="0c07f-181">Actor Events</span></span> |<span data-ttu-id="0c07f-182">Viktiga aktören händelser som genererats av din micro-tjänster, till exempel undantag som utlöses av en aktörsmetod, aktören aktiveringar och avaktiveringar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="0c07f-182">Notable actor events generated by your micro-services, such as exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="0c07f-183">Programhändelser</span><span class="sxs-lookup"><span data-stu-id="0c07f-183">Application Events</span></span> |<span data-ttu-id="0c07f-184">Alla anpassade ETW händelser som genererats av dina program.</span><span class="sxs-lookup"><span data-stu-id="0c07f-184">All custom ETW events generated by your applications.</span></span> |

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="0c07f-187">hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0c07f-187">hello following table shows data collection methods and other details about how data is collected for Service Fabric.</span></span>

| <span data-ttu-id="0c07f-188">Plattform</span><span class="sxs-lookup"><span data-stu-id="0c07f-188">platform</span></span> | <span data-ttu-id="0c07f-189">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="0c07f-189">Direct Agent</span></span> | <span data-ttu-id="0c07f-190">Operations Manager-agent</span><span class="sxs-lookup"><span data-stu-id="0c07f-190">Operations Manager agent</span></span> | <span data-ttu-id="0c07f-191">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0c07f-191">Azure Storage</span></span> | <span data-ttu-id="0c07f-192">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="0c07f-192">Operations Manager required?</span></span> | <span data-ttu-id="0c07f-193">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="0c07f-193">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="0c07f-194">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="0c07f-194">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0c07f-195">Windows</span><span class="sxs-lookup"><span data-stu-id="0c07f-195">Windows</span></span> |  |  | <span data-ttu-id="0c07f-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="0c07f-196">&#8226;</span></span> |  |  |<span data-ttu-id="0c07f-197">10 minuter</span><span class="sxs-lookup"><span data-stu-id="0c07f-197">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="0c07f-198">Du kan ändra hello omfånget för dessa händelser i hello Service Fabric-lösningen genom att klicka på **Data baserat på senaste 7 dagarna** hello överst i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0c07f-198">You can change hello scope of these events in hello Service Fabric solution by clicking **Data based on last 7 days** at hello top of hello dashboard.</span></span> <span data-ttu-id="0c07f-199">Du kan också visa händelser som genereras inom hello senaste sju dagarna, en dag eller sex timmar.</span><span class="sxs-lookup"><span data-stu-id="0c07f-199">You can also show events generated within hello last seven days, one day, or six hours.</span></span> <span data-ttu-id="0c07f-200">Du kan också välja **anpassade** toospecify datumintervall.</span><span class="sxs-lookup"><span data-stu-id="0c07f-200">Or, you can select **Custom** toospecify a custom date range.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="0c07f-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c07f-201">Next steps</span></span>

* <span data-ttu-id="0c07f-202">Använd [loggen söker i logganalys](log-analytics-log-searches.md) tooview detaljerad Service Fabric händelsedata.</span><span class="sxs-lookup"><span data-stu-id="0c07f-202">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Service Fabric event data.</span></span>
