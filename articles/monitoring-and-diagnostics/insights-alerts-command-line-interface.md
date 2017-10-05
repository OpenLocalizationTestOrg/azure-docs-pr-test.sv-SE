---
title: "Skapa aviseringar för Azure-tjänster - plattformar CLI | Microsoft Docs"
description: "Utlösaren e-postmeddelanden meddelanden, anropa webbplatser URL: er (webhooks) eller automation när angivna villkor uppfylls."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: 92246a8da73a244a1c9a924bed55711d71a20fd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a><span data-ttu-id="4585b-103">Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - plattformar CLI</span><span class="sxs-lookup"><span data-stu-id="4585b-103">Create metric alerts in Azure Monitor for Azure services - Cross-platform CLI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4585b-104">Portal</span><span class="sxs-lookup"><span data-stu-id="4585b-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="4585b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4585b-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="4585b-106">CLI</span><span class="sxs-lookup"><span data-stu-id="4585b-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="4585b-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="4585b-107">Overview</span></span>
<span data-ttu-id="4585b-108">Den här artikeln visar hur du ställer in Azure mått aviseringar via plattformsoberoende kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="4585b-108">This article shows you how to set up Azure metric alerts using the cross-platform Command Line Interface (CLI).</span></span>

> [!NOTE]
> <span data-ttu-id="4585b-109">Azure övervakaren är det nya namnet för vad anropades ”Azure Insights” förrän den 25 september 2016.</span><span class="sxs-lookup"><span data-stu-id="4585b-109">Azure Monitor is the new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="4585b-110">Namnområden och därmed nedanstående kommandon fortfarande innehåller dock ”insikter”.</span><span class="sxs-lookup"><span data-stu-id="4585b-110">However, the namespaces and thus the commands below still contain the "insights".</span></span>
>
>

<span data-ttu-id="4585b-111">Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4585b-111">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="4585b-112">**Måttvärden** -aviseringen utlöses när värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="4585b-112">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="4585b-113">Det vill säga den utlöser både när villkoret uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="4585b-113">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="4585b-114">**Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="4585b-114">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="4585b-115">Mer information om aktiviteten loggen aviseringar [Klicka här](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="4585b-115">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="4585b-116">Du kan konfigurera en mått avisering när den utlöser gör du följande:</span><span class="sxs-lookup"><span data-stu-id="4585b-116">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="4585b-117">Skicka e-postmeddelanden till tjänstadministratören och medadministratörer</span><span class="sxs-lookup"><span data-stu-id="4585b-117">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="4585b-118">Skicka e-post till ytterligare e-postmeddelanden som du anger.</span><span class="sxs-lookup"><span data-stu-id="4585b-118">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="4585b-119">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="4585b-119">call a webhook</span></span>
* <span data-ttu-id="4585b-120">Starta körning av en Azure-runbook (endast från Azure-portalen just nu)</span><span class="sxs-lookup"><span data-stu-id="4585b-120">start execution of an Azure runbook (only from the Azure portal at this time)</span></span>

<span data-ttu-id="4585b-121">Du kan konfigurera och få information om mått Varningsregler med</span><span class="sxs-lookup"><span data-stu-id="4585b-121">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="4585b-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4585b-122">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="4585b-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4585b-123">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="4585b-124">kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="4585b-124">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="4585b-125">Azure-Monitor REST API</span><span class="sxs-lookup"><span data-stu-id="4585b-125">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="4585b-126">Du kan alltid få hjälp för kommandon genom att skriva ett kommando och tas - hjälp i slutet.</span><span class="sxs-lookup"><span data-stu-id="4585b-126">You can always receive help for commands by typing a command and putting -help at the end.</span></span> <span data-ttu-id="4585b-127">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4585b-127">For example:</span></span>

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a><span data-ttu-id="4585b-128">Skapa Varningsregler med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="4585b-128">Create alert rules using the CLI</span></span>
1. <span data-ttu-id="4585b-129">Utföra förutsättningarna och logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="4585b-129">Perform the Prerequisites and login to Azure.</span></span> <span data-ttu-id="4585b-130">Se [Azure övervakaren CLI exempel](insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4585b-130">See [Azure Monitor CLI samples](insights-cli-samples.md).</span></span> <span data-ttu-id="4585b-131">Kort sagt: Installera CLI och köra dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="4585b-131">In short, install the CLI and run these commands.</span></span> <span data-ttu-id="4585b-132">De får du loggade in, visa vilken prenumeration som du använder och förbereda dig för att köra Azure-Monitor-kommandon.</span><span class="sxs-lookup"><span data-stu-id="4585b-132">They get you logged in, show what subscription you are using, and prepare you to run Azure Monitor commands.</span></span>

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. <span data-ttu-id="4585b-133">Använd följande syntax om du vill visa en lista med befintliga regler på en resursgrupp, **azure insikter aviseringar regel listan** *[alternativ] &lt;resourceGroup&gt;*</span><span class="sxs-lookup"><span data-stu-id="4585b-133">To list existing rules on a resource group, use the following form **azure insights alerts rule list** *[options] &lt;resourceGroup&gt;*</span></span>

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. <span data-ttu-id="4585b-134">Om du vill skapa en regel som du behöver ha flera viktiga uppgifter för först.</span><span class="sxs-lookup"><span data-stu-id="4585b-134">To create a rule, you need to have several important pieces of information first.</span></span>
  * <span data-ttu-id="4585b-135">Den **resurs-ID** för den resurs som du vill aktivera en avisering för</span><span class="sxs-lookup"><span data-stu-id="4585b-135">The **Resource ID** for the resource you want to set an alert for</span></span>
  * <span data-ttu-id="4585b-136">Den **måttdefinitioner** tillgänglig för den här resursen</span><span class="sxs-lookup"><span data-stu-id="4585b-136">The **metric definitions** available for that resource</span></span>

     <span data-ttu-id="4585b-137">Ett sätt att hämta resurs-ID är att använda Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4585b-137">One way to get the Resource ID is to use the Azure portal.</span></span> <span data-ttu-id="4585b-138">Under förutsättning att resursen har redan skapats, väljer du den i portalen.</span><span class="sxs-lookup"><span data-stu-id="4585b-138">Assuming the resource is already created, select it in the portal.</span></span> <span data-ttu-id="4585b-139">Välj sedan i bladet nästa *egenskaper* under den *inställningar* avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4585b-139">Then in the next blade, select *Properties* under the *Settings* section.</span></span> <span data-ttu-id="4585b-140">Den *resurs-ID* är ett fält i bladet nästa.</span><span class="sxs-lookup"><span data-stu-id="4585b-140">The *RESOURCE ID* is a field in the next blade.</span></span> <span data-ttu-id="4585b-141">Ett annat sätt är att använda den [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4585b-141">Another way is to use the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="4585b-142">Är ett exempel resurs-id för en webbapp</span><span class="sxs-lookup"><span data-stu-id="4585b-142">An example resource id for a web app is</span></span>

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="4585b-143">Om du vill hämta en lista över tillgängliga mått och enheter för dessa mätvärden för exemplet ovan resurs, kan du använda kommandot CLI:</span><span class="sxs-lookup"><span data-stu-id="4585b-143">To get a list of the available metrics and units for those metrics for the previous resource example, use the following CLI command:</span></span>  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     <span data-ttu-id="4585b-144">*PT1M* är Granulariteten för tillgängliga mått (1 minut).</span><span class="sxs-lookup"><span data-stu-id="4585b-144">*PT1M* is the granularity of the available measurement (1-minute intervals).</span></span> <span data-ttu-id="4585b-145">Olika granulariteter får du använder olika alternativ för mått.</span><span class="sxs-lookup"><span data-stu-id="4585b-145">Using different granularities gives you different metric options.</span></span>
4. <span data-ttu-id="4585b-146">Om du vill skapa ett mått baserat varningsregeln använder du kommandot i följande format:</span><span class="sxs-lookup"><span data-stu-id="4585b-146">To create a metric-based alert rule, use a command of the following form:</span></span>

    <span data-ttu-id="4585b-147">**Azure insikter aviseringar regeluppsättning mått** *[alternativ] &lt;ruleName&gt; &lt;plats&gt; &lt;resourceGroup&gt; &lt;fönsterstorlek&gt; &lt;operatorn&gt; &lt;tröskelvärdet&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span><span class="sxs-lookup"><span data-stu-id="4585b-147">**azure insights alerts rule metric set** *[options] &lt;ruleName&gt; &lt;location&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;threshold&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span></span>

    <span data-ttu-id="4585b-148">I följande exempel ställer in en avisering för en resurs för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="4585b-148">The following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="4585b-149">Aviseringen utlösare när den får konsekvent all trafik för 5 minuter och igen när den tar emot någon trafik i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="4585b-149">The alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. <span data-ttu-id="4585b-150">För att skapa webhooken eller skicka e-postmeddelande när en avisering om mått utlöses, först skapa e-post och/eller webhooks.</span><span class="sxs-lookup"><span data-stu-id="4585b-150">To create webhook or send email when a metric alert fires, first create the email and/or webhooks.</span></span> <span data-ttu-id="4585b-151">Skapa regel omedelbart efteråt.</span><span class="sxs-lookup"><span data-stu-id="4585b-151">Then create the rule immediately afterwards.</span></span> <span data-ttu-id="4585b-152">Du kan inte associera webhook eller e-post med redan skapat regler med hjälp av CLI.</span><span class="sxs-lookup"><span data-stu-id="4585b-152">You cannot associate webhook or emails with already created rules using the CLI.</span></span>

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. <span data-ttu-id="4585b-153">Du kan kontrollera att aviseringar har skapats korrekt genom att titta på en enskild regel.</span><span class="sxs-lookup"><span data-stu-id="4585b-153">You can verify that your alerts have been created properly by looking at an individual rule.</span></span>

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. <span data-ttu-id="4585b-154">Kommandot formuläret för att ta bort regler:</span><span class="sxs-lookup"><span data-stu-id="4585b-154">To delete rules, use a command of the form:</span></span>

    <span data-ttu-id="4585b-155">**insikter aviseringar regel delete** [alternativ] &lt;resourceGroup&gt; &lt;ruleName&gt;</span><span class="sxs-lookup"><span data-stu-id="4585b-155">**insights alerts rule delete** [options] &lt;resourceGroup&gt; &lt;ruleName&gt;</span></span>

    <span data-ttu-id="4585b-156">Dessa kommandon ta bort regler som skapats tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4585b-156">These commands delete the rules previously created in this article.</span></span>

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a><span data-ttu-id="4585b-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4585b-157">Next steps</span></span>
* <span data-ttu-id="4585b-158">[Få en översikt över Azure övervakning](monitoring-overview.md) inklusive typerna av information som du kan samla in och övervaka.</span><span class="sxs-lookup"><span data-stu-id="4585b-158">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="4585b-159">Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="4585b-159">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="4585b-160">Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="4585b-160">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="4585b-161">Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="4585b-161">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="4585b-162">Hämta en [översikt över att samla in diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) att samla in detaljerade hög frekvens mått på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="4585b-162">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) to collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="4585b-163">Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) att kontrollera att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="4585b-163">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
