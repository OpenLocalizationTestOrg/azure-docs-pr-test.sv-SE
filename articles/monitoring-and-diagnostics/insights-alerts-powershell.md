---
title: "aaaCreate aviseringar för Azure-tjänster - PowerShell | Microsoft Docs"
description: "Utlösa e-postmeddelanden, meddelanden, anrop webbplatser-URL: er (webhooks) eller automation när hello villkor är uppfyllda."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: 80d3a3f194fc6a5a09a81d04206ea7a1640bddb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a><span data-ttu-id="c8b69-103">Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8b69-103">Create metric alerts in Azure Monitor for Azure services - PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c8b69-104">Portal</span><span class="sxs-lookup"><span data-stu-id="c8b69-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="c8b69-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8b69-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="c8b69-106">CLI</span><span class="sxs-lookup"><span data-stu-id="c8b69-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="c8b69-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="c8b69-107">Overview</span></span>
<span data-ttu-id="c8b69-108">Den här artikeln visar hur tooset upp Azure mått aviseringar med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c8b69-108">This article shows you how tooset up Azure metric alerts using PowerShell.</span></span>  

<span data-ttu-id="c8b69-109">Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c8b69-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="c8b69-110">**Måttvärden** - hello meddela utlösare när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="c8b69-110">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="c8b69-111">Det vill säga den utlöser både när hello villkor uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="c8b69-111">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="c8b69-112">**Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="c8b69-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="c8b69-113">Mer om aktiviteten loggen aviseringar toolearn [Klicka här](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="c8b69-113">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="c8b69-114">Du kan konfigurera en mått avisering toodo hello efter när den utlöser:</span><span class="sxs-lookup"><span data-stu-id="c8b69-114">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="c8b69-115">Skicka e-postaviseringar toohello tjänstadministratören och medadministratörer</span><span class="sxs-lookup"><span data-stu-id="c8b69-115">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="c8b69-116">Skicka e-post tooadditional e-postmeddelanden som du anger.</span><span class="sxs-lookup"><span data-stu-id="c8b69-116">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="c8b69-117">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="c8b69-117">call a webhook</span></span>
* <span data-ttu-id="c8b69-118">Starta körning av en Azure-runbook (endast från hello Azure-portalen)</span><span class="sxs-lookup"><span data-stu-id="c8b69-118">start execution of an Azure runbook (only from hello Azure portal)</span></span>

<span data-ttu-id="c8b69-119">Du kan konfigurera och få information om aviseringen regler med hjälp av</span><span class="sxs-lookup"><span data-stu-id="c8b69-119">You can configure and get information about alert rules using</span></span>

* [<span data-ttu-id="c8b69-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c8b69-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="c8b69-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8b69-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="c8b69-122">kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="c8b69-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="c8b69-123">Azure-Monitor REST API</span><span class="sxs-lookup"><span data-stu-id="c8b69-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="c8b69-124">För ytterligare information, kan du alltid skriva ```Get-Help``` och hello PowerShell-kommando som du vill ha hjälp med.</span><span class="sxs-lookup"><span data-stu-id="c8b69-124">For additional information, you can always type ```Get-Help``` and then hello PowerShell command you want help on.</span></span>

## <a name="create-alert-rules-in-powershell"></a><span data-ttu-id="c8b69-125">Skapa Varningsregler i PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8b69-125">Create alert rules in PowerShell</span></span>
1. <span data-ttu-id="c8b69-126">Logga in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c8b69-126">Log in tooAzure.</span></span>   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. <span data-ttu-id="c8b69-127">Hämta en lista över hello prenumerationer som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="c8b69-127">Get a list of hello subscriptions you have available.</span></span> <span data-ttu-id="c8b69-128">Kontrollera att du arbetar med rätt hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c8b69-128">Verify that you are working with hello right subscription.</span></span> <span data-ttu-id="c8b69-129">Om inte, ange den toohello höger en med hjälp av hello utdata från `Get-AzureRmSubscription`.</span><span class="sxs-lookup"><span data-stu-id="c8b69-129">If not, set it toohello right one using hello output from `Get-AzureRmSubscription`.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. <span data-ttu-id="c8b69-130">toolist befintliga regler på en resursgrupp, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c8b69-130">toolist existing rules on a resource group, use hello following command:</span></span>

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. <span data-ttu-id="c8b69-131">toocreate en regel måste toohave flera viktiga uppgifter för först.</span><span class="sxs-lookup"><span data-stu-id="c8b69-131">toocreate a rule, you need toohave several important pieces of information first.</span></span>

  * <span data-ttu-id="c8b69-132">Hej **resurs-ID** hello resursen du vill tooset en avisering om</span><span class="sxs-lookup"><span data-stu-id="c8b69-132">hello **Resource ID** for hello resource you want tooset an alert for</span></span>
  * <span data-ttu-id="c8b69-133">Hej **måttdefinitioner** tillgänglig för den här resursen</span><span class="sxs-lookup"><span data-stu-id="c8b69-133">hello **metric definitions** available for that resource</span></span>

     <span data-ttu-id="c8b69-134">Enkelriktade tooget hello resurs-ID är toouse hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8b69-134">One way tooget hello Resource ID is toouse hello Azure portal.</span></span> <span data-ttu-id="c8b69-135">Under förutsättning att hello resursen har redan skapats, väljer du den i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8b69-135">Assuming hello resource is already created, select it in hello portal.</span></span> <span data-ttu-id="c8b69-136">Markera i hello nästa bladet *egenskaper* under hello *inställningar* avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c8b69-136">Then in hello next blade, select *Properties* under hello *Settings* section.</span></span> <span data-ttu-id="c8b69-137">**RESURS-ID** är ett fält i nästa hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="c8b69-137">**RESOURCE ID** is a field in hello next blade.</span></span> <span data-ttu-id="c8b69-138">Ett annat sätt är toouse hello [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c8b69-138">Another way is toouse hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="c8b69-139">Är ett exempel resurs-ID för en webbapp</span><span class="sxs-lookup"><span data-stu-id="c8b69-139">An example Resource ID for a web app is</span></span>

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="c8b69-140">Du kan använda `Get-AzureRmMetricDefinition` tooview hello lista över alla måttdefinitioner för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="c8b69-140">You can use `Get-AzureRmMetricDefinition` tooview hello list of all metric definitions for a specific resource.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     <span data-ttu-id="c8b69-141">hello skapar följande exempel en tabell med hello mått namn och hello enhet för att mått.</span><span class="sxs-lookup"><span data-stu-id="c8b69-141">hello following example generates a table with hello metric Name and hello Unit for that metric.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     <span data-ttu-id="c8b69-142">En fullständig lista över tillgängliga alternativ för Get-AzureRmMetricDefinition är tillgänglig genom att köra Get-MetricDefinitions.</span><span class="sxs-lookup"><span data-stu-id="c8b69-142">A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.</span></span>
5. <span data-ttu-id="c8b69-143">hello följande exempel ställer in en avisering för en resurs för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="c8b69-143">hello following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="c8b69-144">hello avisering utlösare när den får konsekvent all trafik för 5 minuter och igen när den tar emot någon trafik i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="c8b69-144">hello alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. <span data-ttu-id="c8b69-145">toocreate webhook eller skicka e-post när en avisering utlöser först skapa hello e-post och/eller webhooks.</span><span class="sxs-lookup"><span data-stu-id="c8b69-145">toocreate webhook or send email when an alert triggers, first create hello email and/or webhooks.</span></span> <span data-ttu-id="c8b69-146">Direkt skapa hello regeln efteråt med hello - åtgärder taggen och som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="c8b69-146">Then immediately create hello rule afterwards with hello -Actions tag and as shown in hello following example.</span></span> <span data-ttu-id="c8b69-147">Du kan inte associera webhook eller e-post med redan skapat regler via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c8b69-147">You cannot associate webhook or emails with already created rules via PowerShell.</span></span>

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. <span data-ttu-id="c8b69-148">tooverify att aviseringar har skapats korrekt genom att titta på hello enskilda regler.</span><span class="sxs-lookup"><span data-stu-id="c8b69-148">tooverify that your alerts have been created properly by looking at hello individual rules.</span></span>

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. <span data-ttu-id="c8b69-149">Ta bort aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="c8b69-149">Delete your alerts.</span></span> <span data-ttu-id="c8b69-150">Dessa kommandon för att ta bort hello regler som har skapats tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c8b69-150">These commands delete hello rules created previously in this article.</span></span>

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a><span data-ttu-id="c8b69-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8b69-151">Next steps</span></span>
* <span data-ttu-id="c8b69-152">[Få en översikt över Azure övervakning](monitoring-overview.md) inklusive hello typer av information som du kan samla in och övervaka.</span><span class="sxs-lookup"><span data-stu-id="c8b69-152">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="c8b69-153">Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="c8b69-153">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="c8b69-154">Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="c8b69-154">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="c8b69-155">Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="c8b69-155">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="c8b69-156">Hämta en [översikt över att samla in diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) toocollect detaljerad hög frekvens mått på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="c8b69-156">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) toocollect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="c8b69-157">Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="c8b69-157">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
