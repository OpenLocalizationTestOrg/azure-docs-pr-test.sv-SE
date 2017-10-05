---
title: "Utvärdera Service Fabric-program med Log Analytics med hjälp av Azure portal | Microsoft Docs"
description: "Du kan använda Service Fabric-lösning i logganalys med Azure-portalen för att bedöma risken och hälsotillståndet för Service Fabric program, micro-tjänster, noder och kluster."
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
ms.openlocfilehash: 8c564c0dcbb2f9be286917b2f4d8a40da5406fae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a><span data-ttu-id="16b16-103">Utvärdera Service Fabric-program och micro-tjänster med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="16b16-103">Assess Service Fabric applications and micro-services with the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="16b16-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="16b16-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="16b16-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="16b16-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>

![Service Fabric-symbolen](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="16b16-107">Den här artikeln beskriver hur du använder Service Fabric-lösning i Log Analytics för att identifiera och felsöka problem i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="16b16-107">This article describes how to use the Service Fabric solution in Log Analytics to help identify and troubleshoot issues across your Service Fabric cluster.</span></span>

<span data-ttu-id="16b16-108">Service Fabric-lösningen använder Azure-diagnostik data från din Service Fabric virtuella datorer genom att samla in data från Azure BOMULLSTUSS tabellerna.</span><span class="sxs-lookup"><span data-stu-id="16b16-108">The Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="16b16-109">Logganalys läser sedan Service Fabric framework händelser, inklusive **händelser för tillförlitlig tjänst**, **aktören händelser**, **drifthändelser**, och **anpassad ETW-händelser**.</span><span class="sxs-lookup"><span data-stu-id="16b16-109">Log Analytics then reads Service Fabric framework events, including **Reliable Service Events**, **Actor Events**, **Operational Events**, and **Custom ETW events**.</span></span> <span data-ttu-id="16b16-110">Med instrumentpanelen lösning kan du visa anmärkningsvärda problem och relevanta händelser i Service Fabric-miljö.</span><span class="sxs-lookup"><span data-stu-id="16b16-110">With the solution dashboard, you are able to view notable issues and relevant events in your Service Fabric environment.</span></span>

<span data-ttu-id="16b16-111">Du måste ansluta Service Fabric-klustret till logganalys-arbetsytan för att komma igång med lösningen.</span><span class="sxs-lookup"><span data-stu-id="16b16-111">To get started with the solution, you need to connect your Service Fabric cluster to a Log Analytics workspace.</span></span> <span data-ttu-id="16b16-112">Här följer tre scenarier att tänka på:</span><span class="sxs-lookup"><span data-stu-id="16b16-112">Here are three scenarios to consider:</span></span>

1. <span data-ttu-id="16b16-113">Om du inte har distribuerat Service Fabric-kluster använder du stegen i ***distribuera ett Service Fabric-kluster som är ansluten till logganalys-arbetsytan*** att distribuera ett nytt kluster och den är konfigurerad för att rapportera till logganalys.</span><span class="sxs-lookup"><span data-stu-id="16b16-113">If you have not deployed your Service Fabric cluster, use the steps in ***Deploy a Service Fabric Cluster connected to a Log Analytics workspace*** to deploy a new cluster and have it configured to report to Log Analytics.</span></span>
2. <span data-ttu-id="16b16-114">Om du behöver samla in prestandaräknare från värdar att använda andra OMS-lösningar, till exempel säkerhet på Service Fabric-kluster, följer du stegen i ***distribuera ett Service Fabric-kluster som är ansluten till logganalys-arbetsytan med VM-tillägg som är installerad.***</span><span class="sxs-lookup"><span data-stu-id="16b16-114">If you need to collect performance counters from your hosts to use other OMS solutions such as Security on your Service Fabric Cluster, follow the steps in ***Deploy a Service Fabric Cluster connected to a Log Analytics workspace with VM Extension installed.***</span></span>
3. <span data-ttu-id="16b16-115">Om du redan har distribuerat din Service Fabric-kluster och vill ansluta till logganalys följer du stegen i ***att lägga till ett befintligt lagringskonto till logganalys.***</span><span class="sxs-lookup"><span data-stu-id="16b16-115">If you have already deployed your Service Fabric cluster and want to connect it to Log Analytics, follow the steps in ***Adding an existing storage account to Log Analytics.***</span></span>

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a><span data-ttu-id="16b16-116">Distribuera ett Service Fabric-kluster som ansluten till logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-116">Deploy a Service Fabric Cluster connected to a Log Analytics workspace.</span></span>
<span data-ttu-id="16b16-117">Den här mallen gör följande:</span><span class="sxs-lookup"><span data-stu-id="16b16-117">This template does the following:</span></span>

1. <span data-ttu-id="16b16-118">Distribuerar ett Azure Service Fabric-kluster som redan är ansluten till logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-118">Deploys an Azure Service Fabric cluster already connected to a Log Analytics workspace.</span></span> <span data-ttu-id="16b16-119">Du har möjlighet att skapa en ny arbetsyta när mallen distribueras eller ange namnet på en redan befintlig logganalys-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="16b16-119">You have the option to create a new workspace while deploying the template, or input the name of an already existing Log Analytics workspace.</span></span>
2. <span data-ttu-id="16b16-120">Lägger till diagnostik lagringskontot logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-120">Adds the diagnostic storage account to the Log Analytics workspace.</span></span>
3. <span data-ttu-id="16b16-121">Gör det möjligt för Service Fabric-lösningen i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-121">Enables the Service Fabric solution in your Log Analytics workspace.</span></span>

<span data-ttu-id="16b16-122">[![Distribuera till Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="16b16-122">[![Deploy to Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="16b16-123">När du har valt knappen distribuera öppnas Azure-portalen med parametrar för att redigera.</span><span class="sxs-lookup"><span data-stu-id="16b16-123">Once you select the deploy button above, the Azure portal opens with parameters for you to edit.</span></span> <span data-ttu-id="16b16-124">Glöm inte att skapa en ny resursgrupp om du ange ett nytt namn för logganalys-arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="16b16-124">Be sure to create a new resource group if you input a new Log Analytics workspace name:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

<span data-ttu-id="16b16-127">Acceptera de juridiska villkoren och klicka på **skapa** att starta distributionen.</span><span class="sxs-lookup"><span data-stu-id="16b16-127">Accept the legal terms and click **Create** to start the deployment.</span></span> <span data-ttu-id="16b16-128">När distributionen är klar visas den nya arbetsytan och kluster skapas och WADServiceFabric * händelse, WADWindowsEventLogs och WADETWEvent tabeller som har lagts till:</span><span class="sxs-lookup"><span data-stu-id="16b16-128">Once the deployment is completed, you should see the new workspace and cluster created, and the WADServiceFabric*Event, WADWindowsEventLogs and WADETWEvent tables added:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a><span data-ttu-id="16b16-130">Distribuera ett Service Fabric-kluster som ansluten till logganalys-arbetsytan med VM-tillägg som är installerad.</span><span class="sxs-lookup"><span data-stu-id="16b16-130">Deploy a Service Fabric Cluster connected to a Log Analytics workspace with VM Extension installed.</span></span>

<span data-ttu-id="16b16-131">Den här mallen gör följande:</span><span class="sxs-lookup"><span data-stu-id="16b16-131">This template does the following:</span></span>

1. <span data-ttu-id="16b16-132">Distribuerar ett Azure Service Fabric-kluster som redan är ansluten till logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-132">Deploys an Azure Service Fabric cluster already connected to a Log Analytics workspace.</span></span> <span data-ttu-id="16b16-133">Du kan skapa en ny arbetsyta eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="16b16-133">You can create a new workspace or use an existing one.</span></span>
2. <span data-ttu-id="16b16-134">Lägger till diagnostik storage-konton till logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-134">Adds the diagnostic storage accounts to the Log Analytics workspace.</span></span>
3. <span data-ttu-id="16b16-135">Gör det möjligt för Service Fabric-lösningen i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-135">Enables the Service Fabric solution in the Log Analytics workspace.</span></span>
4. <span data-ttu-id="16b16-136">Installerar tillägget MMA agent i varje virtuell dator skala i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="16b16-136">Installs the MMA agent extension in each virtual machine scale set in your Service Fabric cluster.</span></span> <span data-ttu-id="16b16-137">Du ska kunna visa prestandamått om noderna med MMA-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="16b16-137">With the MMA agent installed, you are able to view performance metrics about your nodes.</span></span>

<span data-ttu-id="16b16-138">[![Distribuera till Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="16b16-138">[![Deploy to Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="16b16-139">Ange nödvändiga parametrar stegen ovan och startar en distribution.</span><span class="sxs-lookup"><span data-stu-id="16b16-139">Following the same steps above, input the necessary parameters, and kick off a deployment.</span></span> <span data-ttu-id="16b16-140">Igen bör du se den nya arbetsytan, kluster och BOMULLSTUSS tabeller alla skapade:</span><span class="sxs-lookup"><span data-stu-id="16b16-140">Once again you should see the new workspace, cluster and WAD tables all created:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a><span data-ttu-id="16b16-142">Visar prestandadata</span><span class="sxs-lookup"><span data-stu-id="16b16-142">Viewing Performance Data</span></span>

<span data-ttu-id="16b16-143">Visa prestandadata från din noder:</span><span class="sxs-lookup"><span data-stu-id="16b16-143">To view Perf Data from your nodes:</span></span>


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- <span data-ttu-id="16b16-144">Starta logganalys-arbetsytan i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="16b16-144">Launch the Log Analytics workspace from the Azure portal.</span></span>
  <span data-ttu-id="16b16-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span><span class="sxs-lookup"><span data-stu-id="16b16-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span></span>
- <span data-ttu-id="16b16-146">Gå till inställningar i det vänstra fönstret och välj Data >> Windows prestandaräknare >> ”Lägg till valda prestandaräknare”: ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span><span class="sxs-lookup"><span data-stu-id="16b16-146">Go to Settings on the left pane, and select Data >> Windows Performance Counters >> "Add the selected performance counters": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span></span>
- <span data-ttu-id="16b16-147">Använd följande frågor att detaljerad information om viktiga mått om noderna i loggen sökning:</span><span class="sxs-lookup"><span data-stu-id="16b16-147">In Log Search, use the following queries to delve into key metrics about your nodes:</span></span>

    <span data-ttu-id="16b16-148">a.</span><span class="sxs-lookup"><span data-stu-id="16b16-148">a.</span></span> <span data-ttu-id="16b16-149">Jämför den genomsnittliga processoranvändningen mellan alla noder under den senaste timmen för en om du vill se vilka noder har problem och med vilka tidsintervall en nod har en topp:</span><span class="sxs-lookup"><span data-stu-id="16b16-149">Compare the average CPU Utilization across all your nodes in the last one hour to see which nodes are having issues and at what time interval a node had a spike:</span></span>

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    <span data-ttu-id="16b16-151">b.</span><span class="sxs-lookup"><span data-stu-id="16b16-151">b.</span></span> <span data-ttu-id="16b16-152">Visa liknande linjediagram för tillgängligt minne på varje nod med den här frågan:</span><span class="sxs-lookup"><span data-stu-id="16b16-152">View similar line charts for available memory on each node with this query:</span></span>

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    <span data-ttu-id="16b16-153">Använd den här frågan om du vill visa en lista över alla noder, visar exakt medelvärdet för tillgängliga megabyte för varje nod:</span><span class="sxs-lookup"><span data-stu-id="16b16-153">To view a listing of all your nodes, showing the exact average value for Available MBytes for each node, use this query:</span></span>

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    <span data-ttu-id="16b16-155">c.</span><span class="sxs-lookup"><span data-stu-id="16b16-155">c.</span></span> <span data-ttu-id="16b16-156">Du är i de fall som du vill öka detaljnivån till en viss nod genom att undersöka timvis medelvärde, lägsta, högsta och 75 percentil CPU-användning kan göra detta med hjälp av den här frågan (Ersätt datorn fältet):</span><span class="sxs-lookup"><span data-stu-id="16b16-156">In the case that you want to drill down into a specific node by examining the hourly average, minimum, maximum and 75-percentile CPU usage, you're able to do this by using this query (replace Computer field):</span></span>

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

<span data-ttu-id="16b16-158">Mer information om prestandamått i logganalys på den [Operations Management Suite-blogg](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span><span class="sxs-lookup"><span data-stu-id="16b16-158">Read more information about performance metrics in Log Analytics at the [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span></span>


## <a name="adding-an-existing-storage-account-to-log-analytics"></a><span data-ttu-id="16b16-159">Lägga till ett befintligt lagringskonto till logganalys</span><span class="sxs-lookup"><span data-stu-id="16b16-159">Adding an existing storage account to Log Analytics</span></span>

<span data-ttu-id="16b16-160">Den här mallen lägger bara till dina befintliga lagringskonton till en ny eller befintlig logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-160">This template simply adds your existing storage accounts to a new or existing Log Analytics workspace.</span></span>

<span data-ttu-id="16b16-161">[![Distribuera till Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="16b16-161">[![Deploy to Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span></span>

> [!NOTE]
> <span data-ttu-id="16b16-162">Att välja en resursgrupp, Välj om du arbetar med en redan befintlig logganalys-arbetsyta ”Använd befintligt” och Sök efter den resursgrupp som innehåller logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-162">In selecting a Resource Group, if you're working with an already existing Log Analytics workspace, select "Use Existing" and search for the resource group containing the Log Analytics workspace.</span></span> <span data-ttu-id="16b16-163">Skapa en ny en om annars.</span><span class="sxs-lookup"><span data-stu-id="16b16-163">Create a new one if otherwise.</span></span>
> <span data-ttu-id="16b16-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span><span class="sxs-lookup"><span data-stu-id="16b16-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span></span>
>
>

<span data-ttu-id="16b16-165">När den här mallen har distribuerats, kommer du att kunna finns det lagringskonto som är ansluten till logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="16b16-165">After this template has been deployed, you will be able to see the storage account connected to your Log Analytics workspace.</span></span> <span data-ttu-id="16b16-166">I den här instansen jag har lagt till ett mer lagringskonto till arbetsytan Exchange jag skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="16b16-166">In this instance, I added one more storage account to the Exchange workspace I created above.</span></span>
<span data-ttu-id="16b16-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span><span class="sxs-lookup"><span data-stu-id="16b16-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span></span>

## <a name="view-service-fabric-events"></a><span data-ttu-id="16b16-168">Visa händelser för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="16b16-168">View Service Fabric events</span></span>

<span data-ttu-id="16b16-169">När distributioner har slutförts och Service Fabric-lösningen har aktiverats på arbetsytan, Välj den **Service Fabric** panelen i logganalys-portalen för att starta Service Fabric-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="16b16-169">Once the deployments are completed and the Service Fabric solution has been enabled in your workspace, select the **Service Fabric** tile in the Log Analytics portal to launch the Service Fabric dashboard.</span></span> <span data-ttu-id="16b16-170">Instrumentpanelen innehåller kolumnerna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="16b16-170">The dashboard includes the columns in the following table.</span></span> <span data-ttu-id="16b16-171">Varje kolumnen visas de översta 10 händelserna efter antal som matchar den kolumnen villkoren för det angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="16b16-171">Each column lists the top 10 events by count matching that column's criteria for the specified time range.</span></span> <span data-ttu-id="16b16-172">Du kan köra en sökning i logg som innehåller hela listan genom att klicka på **se alla** längst ned rätt i varje kolumn, eller genom att klicka på kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="16b16-172">You can run a log search that provides the entire list by clicking **See all** at the right bottom of each column, or by clicking the column header.</span></span>

| <span data-ttu-id="16b16-173">**Service Fabric-händelse**</span><span class="sxs-lookup"><span data-stu-id="16b16-173">**Service Fabric event**</span></span> | <span data-ttu-id="16b16-174">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="16b16-174">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="16b16-175">Anmärkningsvärda problem</span><span class="sxs-lookup"><span data-stu-id="16b16-175">Notable Issues</span></span> |<span data-ttu-id="16b16-176">En översikt över problem som till exempel RunAsyncFailures RunAsynCancellations och noden används.</span><span class="sxs-lookup"><span data-stu-id="16b16-176">A Display of issues such as RunAsyncFailures RunAsynCancellations and Node Downs.</span></span> |
| <span data-ttu-id="16b16-177">Operativa händelser</span><span class="sxs-lookup"><span data-stu-id="16b16-177">Operational Events</span></span> |<span data-ttu-id="16b16-178">Viktiga operativa händelser, till exempel distributioner och uppgradering av programmet.</span><span class="sxs-lookup"><span data-stu-id="16b16-178">Notable operational events such as application upgrade and deployments.</span></span> |
| <span data-ttu-id="16b16-179">Händelser för tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="16b16-179">Reliable Service Events</span></span> |<span data-ttu-id="16b16-180">Viktiga tillförlitlig tjänsthändelser sådana Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="16b16-180">Notable reliable service events such a Runasyncinvocations.</span></span> |
| <span data-ttu-id="16b16-181">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="16b16-181">Actor Events</span></span> |<span data-ttu-id="16b16-182">Viktiga aktören händelser som genererats av din micro-tjänster, till exempel undantag som utlöses av en aktörsmetod, aktören aktiveringar och avaktiveringar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="16b16-182">Notable actor events generated by your micro-services, such as exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="16b16-183">Programhändelser</span><span class="sxs-lookup"><span data-stu-id="16b16-183">Application Events</span></span> |<span data-ttu-id="16b16-184">Alla anpassade ETW händelser som genererats av dina program.</span><span class="sxs-lookup"><span data-stu-id="16b16-184">All custom ETW events generated by your applications.</span></span> |

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="16b16-187">I följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="16b16-187">The following table shows data collection methods and other details about how data is collected for Service Fabric.</span></span>

| <span data-ttu-id="16b16-188">Plattform</span><span class="sxs-lookup"><span data-stu-id="16b16-188">platform</span></span> | <span data-ttu-id="16b16-189">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="16b16-189">Direct Agent</span></span> | <span data-ttu-id="16b16-190">Operations Manager-agent</span><span class="sxs-lookup"><span data-stu-id="16b16-190">Operations Manager agent</span></span> | <span data-ttu-id="16b16-191">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="16b16-191">Azure Storage</span></span> | <span data-ttu-id="16b16-192">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="16b16-192">Operations Manager required?</span></span> | <span data-ttu-id="16b16-193">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="16b16-193">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="16b16-194">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="16b16-194">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="16b16-195">Windows</span><span class="sxs-lookup"><span data-stu-id="16b16-195">Windows</span></span> |  |  | <span data-ttu-id="16b16-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="16b16-196">&#8226;</span></span> |  |  |<span data-ttu-id="16b16-197">10 minuter</span><span class="sxs-lookup"><span data-stu-id="16b16-197">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="16b16-198">Du kan ändra omfånget för dessa händelser i Service Fabric-lösningen genom att klicka på **Data baserat på senaste 7 dagarna** överst på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="16b16-198">You can change the scope of these events in the Service Fabric solution by clicking **Data based on last 7 days** at the top of the dashboard.</span></span> <span data-ttu-id="16b16-199">Du kan också visa händelser som genererats under de senaste sju dagarna, en dag eller sex timmar.</span><span class="sxs-lookup"><span data-stu-id="16b16-199">You can also show events generated within the last seven days, one day, or six hours.</span></span> <span data-ttu-id="16b16-200">Du kan också välja **anpassade** att ange ett anpassat datumintervall.</span><span class="sxs-lookup"><span data-stu-id="16b16-200">Or, you can select **Custom** to specify a custom date range.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="16b16-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16b16-201">Next steps</span></span>

* <span data-ttu-id="16b16-202">Använd [loggen söker i logganalys](log-analytics-log-searches.md) att visa detaljerad information för Service Fabric-händelse.</span><span class="sxs-lookup"><span data-stu-id="16b16-202">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) to view detailed Service Fabric event data.</span></span>
