---
title: "Skapa aviseringar för Azure-tjänster - PowerShell | Microsoft Docs"
description: "Utlösaren e-postmeddelanden meddelanden, anropa webbplatser URL: er (webhooks) eller automation när angivna villkor uppfylls."
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
ms.openlocfilehash: 50127242cdf156771d0610e58cf2fc41281adae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a><span data-ttu-id="ccde1-103">Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - PowerShell</span><span class="sxs-lookup"><span data-stu-id="ccde1-103">Create metric alerts in Azure Monitor for Azure services - PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ccde1-104">Portal</span><span class="sxs-lookup"><span data-stu-id="ccde1-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="ccde1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ccde1-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="ccde1-106">CLI</span><span class="sxs-lookup"><span data-stu-id="ccde1-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="ccde1-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="ccde1-107">Overview</span></span>
<span data-ttu-id="ccde1-108">Den här artikeln visar hur du ställer in Azure mått aviseringar med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccde1-108">This article shows you how to set up Azure metric alerts using PowerShell.</span></span>  

<span data-ttu-id="ccde1-109">Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="ccde1-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="ccde1-110">**Måttvärden** -aviseringen utlöses när värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="ccde1-110">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="ccde1-111">Det vill säga den utlöser både när villkoret uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="ccde1-111">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="ccde1-112">**Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="ccde1-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="ccde1-113">Mer information om aktiviteten loggen aviseringar [Klicka här](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="ccde1-113">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="ccde1-114">Du kan konfigurera en mått avisering när den utlöser gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ccde1-114">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="ccde1-115">Skicka e-postmeddelanden till tjänstadministratören och medadministratörer</span><span class="sxs-lookup"><span data-stu-id="ccde1-115">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="ccde1-116">Skicka e-post till ytterligare e-postmeddelanden som du anger.</span><span class="sxs-lookup"><span data-stu-id="ccde1-116">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="ccde1-117">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="ccde1-117">call a webhook</span></span>
* <span data-ttu-id="ccde1-118">Starta körning av en Azure-runbook (endast från Azure portal)</span><span class="sxs-lookup"><span data-stu-id="ccde1-118">start execution of an Azure runbook (only from the Azure portal)</span></span>

<span data-ttu-id="ccde1-119">Du kan konfigurera och få information om aviseringen regler med hjälp av</span><span class="sxs-lookup"><span data-stu-id="ccde1-119">You can configure and get information about alert rules using</span></span>

* [<span data-ttu-id="ccde1-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ccde1-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="ccde1-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ccde1-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="ccde1-122">kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="ccde1-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="ccde1-123">Azure-Monitor REST API</span><span class="sxs-lookup"><span data-stu-id="ccde1-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="ccde1-124">För ytterligare information, kan du alltid skriva ```Get-Help``` och sedan av PowerShell-kommando som du vill ha hjälp med.</span><span class="sxs-lookup"><span data-stu-id="ccde1-124">For additional information, you can always type ```Get-Help``` and then the PowerShell command you want help on.</span></span>

## <a name="create-alert-rules-in-powershell"></a><span data-ttu-id="ccde1-125">Skapa Varningsregler i PowerShell</span><span class="sxs-lookup"><span data-stu-id="ccde1-125">Create alert rules in PowerShell</span></span>
1. <span data-ttu-id="ccde1-126">Logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="ccde1-126">Log in to Azure.</span></span>   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. <span data-ttu-id="ccde1-127">Hämta en lista över prenumerationerna som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="ccde1-127">Get a list of the subscriptions you have available.</span></span> <span data-ttu-id="ccde1-128">Kontrollera att du arbetar med rätt prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ccde1-128">Verify that you are working with the right subscription.</span></span> <span data-ttu-id="ccde1-129">Om inte, anger du det högra använda utdata från `Get-AzureRmSubscription`.</span><span class="sxs-lookup"><span data-stu-id="ccde1-129">If not, set it to the right one using the output from `Get-AzureRmSubscription`.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. <span data-ttu-id="ccde1-130">Om du vill visa en lista med befintliga regler på en resursgrupp, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ccde1-130">To list existing rules on a resource group, use the following command:</span></span>

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. <span data-ttu-id="ccde1-131">Om du vill skapa en regel som du behöver ha flera viktiga uppgifter för först.</span><span class="sxs-lookup"><span data-stu-id="ccde1-131">To create a rule, you need to have several important pieces of information first.</span></span>

  * <span data-ttu-id="ccde1-132">Den **resurs-ID** för den resurs som du vill aktivera en avisering för</span><span class="sxs-lookup"><span data-stu-id="ccde1-132">The **Resource ID** for the resource you want to set an alert for</span></span>
  * <span data-ttu-id="ccde1-133">Den **måttdefinitioner** tillgänglig för den här resursen</span><span class="sxs-lookup"><span data-stu-id="ccde1-133">The **metric definitions** available for that resource</span></span>

     <span data-ttu-id="ccde1-134">Ett sätt att hämta resurs-ID är att använda Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ccde1-134">One way to get the Resource ID is to use the Azure portal.</span></span> <span data-ttu-id="ccde1-135">Under förutsättning att resursen har redan skapats, väljer du den i portalen.</span><span class="sxs-lookup"><span data-stu-id="ccde1-135">Assuming the resource is already created, select it in the portal.</span></span> <span data-ttu-id="ccde1-136">Välj sedan i bladet nästa *egenskaper* under den *inställningar* avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ccde1-136">Then in the next blade, select *Properties* under the *Settings* section.</span></span> <span data-ttu-id="ccde1-137">**RESURS-ID** är ett fält i bladet nästa.</span><span class="sxs-lookup"><span data-stu-id="ccde1-137">**RESOURCE ID** is a field in the next blade.</span></span> <span data-ttu-id="ccde1-138">Ett annat sätt är att använda den [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ccde1-138">Another way is to use the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="ccde1-139">Är ett exempel resurs-ID för en webbapp</span><span class="sxs-lookup"><span data-stu-id="ccde1-139">An example Resource ID for a web app is</span></span>

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="ccde1-140">Du kan använda `Get-AzureRmMetricDefinition` att visa en lista över alla måttdefinitioner för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="ccde1-140">You can use `Get-AzureRmMetricDefinition` to view the list of all metric definitions for a specific resource.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     <span data-ttu-id="ccde1-141">I följande exempel genererar en tabell med måttet namn och enhet för att mått.</span><span class="sxs-lookup"><span data-stu-id="ccde1-141">The following example generates a table with the metric Name and the Unit for that metric.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     <span data-ttu-id="ccde1-142">En fullständig lista över tillgängliga alternativ för Get-AzureRmMetricDefinition är tillgänglig genom att köra Get-MetricDefinitions.</span><span class="sxs-lookup"><span data-stu-id="ccde1-142">A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.</span></span>
5. <span data-ttu-id="ccde1-143">I följande exempel ställer in en avisering för en resurs för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="ccde1-143">The following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="ccde1-144">Aviseringen utlösare när den får konsekvent all trafik för 5 minuter och igen när den tar emot någon trafik i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="ccde1-144">The alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. <span data-ttu-id="ccde1-145">För att skapa webhooken eller skicka e-postmeddelande när en avisering utlöser, först skapa e-post och/eller webhooks.</span><span class="sxs-lookup"><span data-stu-id="ccde1-145">To create webhook or send email when an alert triggers, first create the email and/or webhooks.</span></span> <span data-ttu-id="ccde1-146">Sedan omedelbart skapa regel efteråt med taggen - åtgärder och som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="ccde1-146">Then immediately create the rule afterwards with the -Actions tag and as shown in the following example.</span></span> <span data-ttu-id="ccde1-147">Du kan inte associera webhook eller e-post med redan skapat regler via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccde1-147">You cannot associate webhook or emails with already created rules via PowerShell.</span></span>

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. <span data-ttu-id="ccde1-148">Kontrollera att dina aviseringar genom att titta på enskilda regler har skapats korrekt.</span><span class="sxs-lookup"><span data-stu-id="ccde1-148">To verify that your alerts have been created properly by looking at the individual rules.</span></span>

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. <span data-ttu-id="ccde1-149">Ta bort aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="ccde1-149">Delete your alerts.</span></span> <span data-ttu-id="ccde1-150">Dessa kommandon för att ta bort regler som har skapats tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ccde1-150">These commands delete the rules created previously in this article.</span></span>

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a><span data-ttu-id="ccde1-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ccde1-151">Next steps</span></span>
* <span data-ttu-id="ccde1-152">[Få en översikt över Azure övervakning](monitoring-overview.md) inklusive typerna av information som du kan samla in och övervaka.</span><span class="sxs-lookup"><span data-stu-id="ccde1-152">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="ccde1-153">Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="ccde1-153">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="ccde1-154">Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="ccde1-154">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="ccde1-155">Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="ccde1-155">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="ccde1-156">Hämta en [översikt över att samla in diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) att samla in detaljerade hög frekvens mått på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="ccde1-156">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) to collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="ccde1-157">Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) att kontrollera att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="ccde1-157">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
