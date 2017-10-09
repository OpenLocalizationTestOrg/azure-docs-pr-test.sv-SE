---
title: "aaaCreate aviseringar för Azure-tjänster - plattformar CLI | Microsoft Docs"
description: "Utlösa e-postmeddelanden, meddelanden, anrop webbplatser-URL: er (webhooks) eller automation när hello villkor är uppfyllda."
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
ms.openlocfilehash: e53701e5377a415038a69fbd32f1e5fc5fe99be9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a><span data-ttu-id="f2253-103">Skapa mått aviseringar i Azure-Monitor för Azure-tjänster - plattformar CLI</span><span class="sxs-lookup"><span data-stu-id="f2253-103">Create metric alerts in Azure Monitor for Azure services - Cross-platform CLI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f2253-104">Portal</span><span class="sxs-lookup"><span data-stu-id="f2253-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="f2253-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2253-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="f2253-106">CLI</span><span class="sxs-lookup"><span data-stu-id="f2253-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="f2253-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="f2253-107">Overview</span></span>
<span data-ttu-id="f2253-108">Den här artikeln visar hur tooset in Azure mått aviseringar med hello plattformsoberoende kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="f2253-108">This article shows you how tooset up Azure metric alerts using hello cross-platform Command Line Interface (CLI).</span></span>

> [!NOTE]
> <span data-ttu-id="f2253-109">Azure övervakaren är hello nytt namn för vad anropades ”Azure Insights” förrän den 25 september 2016.</span><span class="sxs-lookup"><span data-stu-id="f2253-109">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="f2253-110">Dock innehåller hello namnområden och därmed hello-kommandona nedan fortfarande insikter ”hello”.</span><span class="sxs-lookup"><span data-stu-id="f2253-110">However, hello namespaces and thus hello commands below still contain hello "insights".</span></span>
>
>

<span data-ttu-id="f2253-111">Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f2253-111">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="f2253-112">**Måttvärden** - hello meddela utlösare när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="f2253-112">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="f2253-113">Det vill säga den utlöser både när hello villkor uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="f2253-113">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="f2253-114">**Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller bara när en vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="f2253-114">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="f2253-115">Mer om aktiviteten loggen aviseringar toolearn [Klicka här](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="f2253-115">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="f2253-116">Du kan konfigurera en mått avisering toodo hello efter när den utlöser:</span><span class="sxs-lookup"><span data-stu-id="f2253-116">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="f2253-117">Skicka e-postaviseringar toohello tjänstadministratören och medadministratörer</span><span class="sxs-lookup"><span data-stu-id="f2253-117">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="f2253-118">Skicka e-post tooadditional e-postmeddelanden som du anger.</span><span class="sxs-lookup"><span data-stu-id="f2253-118">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="f2253-119">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="f2253-119">call a webhook</span></span>
* <span data-ttu-id="f2253-120">Starta körning av en Azure-runbook (endast från hello Azure-portalen just nu)</span><span class="sxs-lookup"><span data-stu-id="f2253-120">start execution of an Azure runbook (only from hello Azure portal at this time)</span></span>

<span data-ttu-id="f2253-121">Du kan konfigurera och få information om mått Varningsregler med</span><span class="sxs-lookup"><span data-stu-id="f2253-121">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="f2253-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f2253-122">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="f2253-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2253-123">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="f2253-124">kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="f2253-124">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="f2253-125">Azure-Monitor REST API</span><span class="sxs-lookup"><span data-stu-id="f2253-125">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="f2253-126">Du kan alltid få hjälp för kommandon genom att skriva ett kommando och tas - hjälp hello slutet.</span><span class="sxs-lookup"><span data-stu-id="f2253-126">You can always receive help for commands by typing a command and putting -help at hello end.</span></span> <span data-ttu-id="f2253-127">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f2253-127">For example:</span></span>

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-hello-cli"></a><span data-ttu-id="f2253-128">Skapa Varningsregler med hello CLI</span><span class="sxs-lookup"><span data-stu-id="f2253-128">Create alert rules using hello CLI</span></span>
1. <span data-ttu-id="f2253-129">Utför hello krav och inloggningen tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f2253-129">Perform hello Prerequisites and login tooAzure.</span></span> <span data-ttu-id="f2253-130">Se [Azure övervakaren CLI exempel](insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f2253-130">See [Azure Monitor CLI samples](insights-cli-samples.md).</span></span> <span data-ttu-id="f2253-131">Kort sagt: Installera hello CLI och köra dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="f2253-131">In short, install hello CLI and run these commands.</span></span> <span data-ttu-id="f2253-132">De får du loggade in, visa vilken prenumeration som du använder och förbereda toorun Azure-Monitor-kommandon.</span><span class="sxs-lookup"><span data-stu-id="f2253-132">They get you logged in, show what subscription you are using, and prepare you toorun Azure Monitor commands.</span></span>

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. <span data-ttu-id="f2253-133">toolist befintliga regler på en resursgrupp, använda hello efter formuläret **azure insikter aviseringar regel listan** *[alternativ] &lt;resourceGroup&gt;*</span><span class="sxs-lookup"><span data-stu-id="f2253-133">toolist existing rules on a resource group, use hello following form **azure insights alerts rule list** *[options] &lt;resourceGroup&gt;*</span></span>

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. <span data-ttu-id="f2253-134">toocreate en regel måste toohave flera viktiga uppgifter för först.</span><span class="sxs-lookup"><span data-stu-id="f2253-134">toocreate a rule, you need toohave several important pieces of information first.</span></span>
  * <span data-ttu-id="f2253-135">Hej **resurs-ID** hello resursen du vill tooset en avisering om</span><span class="sxs-lookup"><span data-stu-id="f2253-135">hello **Resource ID** for hello resource you want tooset an alert for</span></span>
  * <span data-ttu-id="f2253-136">Hej **måttdefinitioner** tillgänglig för den här resursen</span><span class="sxs-lookup"><span data-stu-id="f2253-136">hello **metric definitions** available for that resource</span></span>

     <span data-ttu-id="f2253-137">Enkelriktade tooget hello resurs-ID är toouse hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2253-137">One way tooget hello Resource ID is toouse hello Azure portal.</span></span> <span data-ttu-id="f2253-138">Under förutsättning att hello resursen har redan skapats, väljer du den i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2253-138">Assuming hello resource is already created, select it in hello portal.</span></span> <span data-ttu-id="f2253-139">Markera i hello nästa bladet *egenskaper* under hello *inställningar* avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f2253-139">Then in hello next blade, select *Properties* under hello *Settings* section.</span></span> <span data-ttu-id="f2253-140">Hej *resurs-ID* är ett fält i nästa hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="f2253-140">hello *RESOURCE ID* is a field in hello next blade.</span></span> <span data-ttu-id="f2253-141">Ett annat sätt är toouse hello [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f2253-141">Another way is toouse hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="f2253-142">Är ett exempel resurs-id för en webbapp</span><span class="sxs-lookup"><span data-stu-id="f2253-142">An example resource id for a web app is</span></span>

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="f2253-143">tooget en lista över tillgängliga mått för hello och enheter för de mätvärdena som exempelvis hello tidigare resurs Använd hello följande CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="f2253-143">tooget a list of hello available metrics and units for those metrics for hello previous resource example, use hello following CLI command:</span></span>  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     <span data-ttu-id="f2253-144">*PT1M* är hello Granulariteten för hello tillgängliga mått (1 minut).</span><span class="sxs-lookup"><span data-stu-id="f2253-144">*PT1M* is hello granularity of hello available measurement (1-minute intervals).</span></span> <span data-ttu-id="f2253-145">Olika granulariteter får du använder olika alternativ för mått.</span><span class="sxs-lookup"><span data-stu-id="f2253-145">Using different granularities gives you different metric options.</span></span>
4. <span data-ttu-id="f2253-146">toocreate ett mått baserat varningsregeln, kommandot hello följande format:</span><span class="sxs-lookup"><span data-stu-id="f2253-146">toocreate a metric-based alert rule, use a command of hello following form:</span></span>

    <span data-ttu-id="f2253-147">**Azure insikter aviseringar regeluppsättning mått** *[alternativ] &lt;ruleName&gt; &lt;plats&gt; &lt;resourceGroup&gt; &lt;fönsterstorlek&gt; &lt;operatorn&gt; &lt;tröskelvärdet&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span><span class="sxs-lookup"><span data-stu-id="f2253-147">**azure insights alerts rule metric set** *[options] &lt;ruleName&gt; &lt;location&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;threshold&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span></span>

    <span data-ttu-id="f2253-148">hello följande exempel ställer in en avisering för en resurs för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="f2253-148">hello following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="f2253-149">hello avisering utlösare när den får konsekvent all trafik för 5 minuter och igen när den tar emot någon trafik i 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="f2253-149">hello alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. <span data-ttu-id="f2253-150">toocreate webhook eller skicka e-post när en avisering om mått utlöses först skapa hello e-post och/eller webhooks.</span><span class="sxs-lookup"><span data-stu-id="f2253-150">toocreate webhook or send email when a metric alert fires, first create hello email and/or webhooks.</span></span> <span data-ttu-id="f2253-151">Skapa sedan hello regel omedelbart efteråt.</span><span class="sxs-lookup"><span data-stu-id="f2253-151">Then create hello rule immediately afterwards.</span></span> <span data-ttu-id="f2253-152">Du kan inte associera webhook eller e-post med redan skapat regler med hjälp av hello CLI.</span><span class="sxs-lookup"><span data-stu-id="f2253-152">You cannot associate webhook or emails with already created rules using hello CLI.</span></span>

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. <span data-ttu-id="f2253-153">Du kan kontrollera att aviseringar har skapats korrekt genom att titta på en enskild regel.</span><span class="sxs-lookup"><span data-stu-id="f2253-153">You can verify that your alerts have been created properly by looking at an individual rule.</span></span>

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. <span data-ttu-id="f2253-154">toodelete regler kommandot hello formuläret:</span><span class="sxs-lookup"><span data-stu-id="f2253-154">toodelete rules, use a command of hello form:</span></span>

    <span data-ttu-id="f2253-155">**insikter aviseringar regel delete** [alternativ] &lt;resourceGroup&gt; &lt;ruleName&gt;</span><span class="sxs-lookup"><span data-stu-id="f2253-155">**insights alerts rule delete** [options] &lt;resourceGroup&gt; &lt;ruleName&gt;</span></span>

    <span data-ttu-id="f2253-156">Dessa kommandon för att ta bort hello regler som har skapats tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f2253-156">These commands delete hello rules previously created in this article.</span></span>

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a><span data-ttu-id="f2253-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2253-157">Next steps</span></span>
* <span data-ttu-id="f2253-158">[Få en översikt över Azure övervakning](monitoring-overview.md) inklusive hello typer av information som du kan samla in och övervaka.</span><span class="sxs-lookup"><span data-stu-id="f2253-158">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="f2253-159">Lär dig mer om [hur du konfigurerar webhooks i aviseringar](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f2253-159">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="f2253-160">Lär dig mer om [konfigurera aviseringar på aktiviteten logghändelser](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f2253-160">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="f2253-161">Lär dig mer om [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="f2253-161">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="f2253-162">Hämta en [översikt över att samla in diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) toocollect detaljerad hög frekvens mått på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="f2253-162">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) toocollect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="f2253-163">Hämta en [översikt över mått samling](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="f2253-163">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
