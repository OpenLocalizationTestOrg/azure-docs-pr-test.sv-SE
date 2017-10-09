---
title: "aaaMonitor åtgärder, händelser och prestandaräknare för belastningsutjämnaren | Microsoft Docs"
description: "Lär dig hur tooenable avisering händelser och avsökning hälsa status loggning för Azure belastningsutjämnare"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac53c2254e06cad780ad6144c5c30f0085d12576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a><span data-ttu-id="2197c-103">Logganalys för Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="2197c-103">Log analytics for Azure Load Balancer</span></span>

<span data-ttu-id="2197c-104">Du kan använda olika typer av loggar i Azure toomanage och felsöka belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="2197c-104">You can use different types of logs in Azure toomanage and troubleshoot load balancers.</span></span> <span data-ttu-id="2197c-105">Vissa av dessa loggar kan nås via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2197c-105">Some of these logs can be accessed through hello portal.</span></span> <span data-ttu-id="2197c-106">Alla loggar kan extraheras från Azure blob storage och visas i olika verktyg som Excel och PowerBI.</span><span class="sxs-lookup"><span data-stu-id="2197c-106">All logs can be extracted from Azure blob storage, and viewed in different tools, such as Excel and PowerBI.</span></span> <span data-ttu-id="2197c-107">Mer information om hello olika typer av loggar hello listan nedan.</span><span class="sxs-lookup"><span data-stu-id="2197c-107">You can learn more about hello different types of logs from hello list below.</span></span>

* <span data-ttu-id="2197c-108">**Granskningsloggar:** du kan använda [Azure-granskningsloggarna](../monitoring-and-diagnostics/insights-debugging-with-events.md) (kallades tidigare operativa loggar) tooview alla åtgärder som skickats tooyour Azure-prenumeration(er) och deras status.</span><span class="sxs-lookup"><span data-stu-id="2197c-108">**Audit logs:** You can use [Azure Audit Logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as Operational Logs) tooview all operations being submitted tooyour Azure subscription(s), and their status.</span></span> <span data-ttu-id="2197c-109">Granskningsloggar är aktiverade som standard och kan visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2197c-109">Audit logs are enabled by default, and can be viewed in hello Azure portal.</span></span>
* <span data-ttu-id="2197c-110">**Varna händelseloggar:** kan du använda den här loggen tooview aviseringar rasied hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="2197c-110">**Alert event logs:** You can use this log tooview alerts rasied by hello load balancer.</span></span> <span data-ttu-id="2197c-111">hello status för hello belastningsutjämnaren samlas var femte minut.</span><span class="sxs-lookup"><span data-stu-id="2197c-111">hello status for hello load balancer is collected every five minutes.</span></span> <span data-ttu-id="2197c-112">Den här loggen skrivs endast om en belastningsutjämnare avisering händelsen utlöses.</span><span class="sxs-lookup"><span data-stu-id="2197c-112">This log is only written if a load balancer alert event is raised.</span></span>
* <span data-ttu-id="2197c-113">**Hälsotillstånd avsökningen loggar:** du kan använda den här loggen tooview problem som identifieras av din hälsoavsökningen, till exempel hello antalet instanser i en serverdelspool som inte tar emot förfrågningar från hello belastningsutjämnaren på grund av fel för avsökning av hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="2197c-113">**Health probe logs:** You can use this log tooview problems detected by your health probe, such as hello number of instances in your backend-pool that are not receiving requests from hello load balancer because of health probe failures.</span></span> <span data-ttu-id="2197c-114">Den här loggen skapas toowhen ändras i hello hälsostatus för avsökning.</span><span class="sxs-lookup"><span data-stu-id="2197c-114">This log is written toowhen there is a change in hello health probe status.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2197c-115">Logga analytics fungerar för närvarande endast för belastningsutjämnare mot Internet.</span><span class="sxs-lookup"><span data-stu-id="2197c-115">Log analytics currently works only for Internet facing load balancers.</span></span> <span data-ttu-id="2197c-116">Loggarna är bara tillgängliga för resurser som har distribuerats i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2197c-116">Logs are only available for resources deployed in hello Resource Manager deployment model.</span></span> <span data-ttu-id="2197c-117">Du kan inte använda loggar för resurser i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2197c-117">You cannot use logs for resources in hello classic deployment model.</span></span> <span data-ttu-id="2197c-118">Läs mer om hello distributionsmodeller [förstå Resource Manager distribution och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2197c-118">For more information about hello deployment models, see [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="enable-logging"></a><span data-ttu-id="2197c-119">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="2197c-119">Enable logging</span></span>

<span data-ttu-id="2197c-120">Granskningsloggning aktiveras automatiskt för varje resurs för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="2197c-120">Audit logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="2197c-121">Du behöver tooenable händelse och hälsotillstånd avsökningen loggning toostart samlar in hello-data som är tillgängliga via dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="2197c-121">You need tooenable event and health probe logging toostart collecting hello data available through those logs.</span></span> <span data-ttu-id="2197c-122">Använd följande steg tooenable loggning hello.</span><span class="sxs-lookup"><span data-stu-id="2197c-122">Use hello following steps tooenable logging.</span></span>

<span data-ttu-id="2197c-123">Logga in toohello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2197c-123">Sign-in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="2197c-124">Om du inte redan har en belastningsutjämnare [skapa en belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="2197c-124">If you don't already have a load balancer, [create a load balancer](load-balancer-get-started-internet-arm-ps.md) before you continue.</span></span>

1. <span data-ttu-id="2197c-125">I hello-portalen klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="2197c-125">In hello portal, click **Browse**.</span></span>
2. <span data-ttu-id="2197c-126">Välj **belastningsutjämnare**.</span><span class="sxs-lookup"><span data-stu-id="2197c-126">Select **Load Balancers**.</span></span>

    ![Portal - belastningsutjämnare](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. <span data-ttu-id="2197c-128">Välj en befintlig belastningsutjämnare >> **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="2197c-128">Select an existing load balancer >> **All Settings**.</span></span>
4. <span data-ttu-id="2197c-129">Hello höger sida av hello dialogrutan under hello namn av hello belastningsutjämnare rulla för**övervakning**, klickar du på **diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="2197c-129">On hello right side of hello dialog under hello name of hello load balancer, scroll too**Monitoring**, click **Diagnostics**.</span></span>

    ![Portal –--inställningarna för belastningsutjämnaren](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. <span data-ttu-id="2197c-131">I hello **diagnostik** rutan under **Status**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="2197c-131">In hello **Diagnostics** pane, under **Status**, select **On**.</span></span>
6. <span data-ttu-id="2197c-132">Klicka på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="2197c-132">Click **Storage Account**.</span></span>
7. <span data-ttu-id="2197c-133">Under **loggar**, Välj ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="2197c-133">Under **LOGS**, select an existing storage account, or create a new one.</span></span> <span data-ttu-id="2197c-134">Använd hello skjutreglaget toodetermine hur många dagar med händelsedata sparas i hello händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="2197c-134">Use hello slider toodetermine how many days worth of event data will be stored in hello event logs.</span></span> 
8. <span data-ttu-id="2197c-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2197c-135">Click **Save**.</span></span>

    ![Portal - diagnostik loggar](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> <span data-ttu-id="2197c-137">Granskningsloggar kräver inte ett separat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2197c-137">Audit logs do not require a separate storage account.</span></span> <span data-ttu-id="2197c-138">Hej användning av lagring för händelsen och hälsa avsökningen loggning kommer avgifter service.</span><span class="sxs-lookup"><span data-stu-id="2197c-138">hello use of storage for event and health probe logging will incur service charges.</span></span>

## <a name="audit-log"></a><span data-ttu-id="2197c-139">granskningslogg</span><span class="sxs-lookup"><span data-stu-id="2197c-139">Audit log</span></span>

<span data-ttu-id="2197c-140">hello granskningsloggen skapas som standard.</span><span class="sxs-lookup"><span data-stu-id="2197c-140">hello audit log is generated by default.</span></span> <span data-ttu-id="2197c-141">hello loggar bevaras under 90 dagar i Azures händelseloggar store.</span><span class="sxs-lookup"><span data-stu-id="2197c-141">hello logs are preserved for 90 days in Azure's Event Logs store.</span></span> <span data-ttu-id="2197c-142">Mer information om de här loggarna genom att läsa hello [visa händelser och granskningsloggar](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2197c-142">Learn more about these logs by reading hello [View events and audit logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

## <a name="alert-event-log"></a><span data-ttu-id="2197c-143">Aviseringen händelseloggen</span><span class="sxs-lookup"><span data-stu-id="2197c-143">Alert event log</span></span>

<span data-ttu-id="2197c-144">Den här loggen genereras bara om du har aktiverat på en per belastningen belastningsutjämnaren basis.</span><span class="sxs-lookup"><span data-stu-id="2197c-144">This log is only generated if you've enabled it on a per load balancer basis.</span></span> <span data-ttu-id="2197c-145">hello händelser loggas i JSON-format och lagras i hello storage-konto som du angav när du har aktiverat hello loggning.</span><span class="sxs-lookup"><span data-stu-id="2197c-145">hello events are logged in JSON format and stored in hello storage account you specified when you enabled hello logging.</span></span> <span data-ttu-id="2197c-146">hello följande är ett exempel på en händelse.</span><span class="sxs-lookup"><span data-stu-id="2197c-146">hello following is an example of an event.</span></span>

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

<span data-ttu-id="2197c-147">hello JSON utdata visar hello *eventname* -egenskap som beskriver hello orsak för hello belastningsutjämnaren skapas en avisering.</span><span class="sxs-lookup"><span data-stu-id="2197c-147">hello JSON output shows hello *eventname* property which will describe hello reason for hello load balancer created an alert.</span></span> <span data-ttu-id="2197c-148">I det här fallet kunde hello avisering som genererats på grund av tooTCP port resursuttömning på grund av käll-IP NAT begränsar (SNAT).</span><span class="sxs-lookup"><span data-stu-id="2197c-148">In this case, hello alert generated was due tooTCP port exhaustion caused by source IP NAT limits (SNAT).</span></span>

## <a name="health-probe-log"></a><span data-ttu-id="2197c-149">Hälsotillstånd avsökningen logg</span><span class="sxs-lookup"><span data-stu-id="2197c-149">Health probe log</span></span>

<span data-ttu-id="2197c-150">Den här loggen genereras bara om du har aktiverat på en per belastningen belastningsutjämnaren bas enligt anvisningarna ovan.</span><span class="sxs-lookup"><span data-stu-id="2197c-150">This log is only generated if you've enabled it on a per load balancer basis as detailed above.</span></span> <span data-ttu-id="2197c-151">hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning.</span><span class="sxs-lookup"><span data-stu-id="2197c-151">hello data is stored in hello storage account you specified when you enabled hello logging.</span></span> <span data-ttu-id="2197c-152">En behållare med namnet 'insikter-loggar loadbalancerprobehealthstatus' har skapats och hello följande data loggas:</span><span class="sxs-lookup"><span data-stu-id="2197c-152">A container named 'insights-logs-loadbalancerprobehealthstatus' is created and hello following data is logged:</span></span>

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

<span data-ttu-id="2197c-153">hello JSON-utdata visar hello egenskaper fältet hello grundläggande information om hälsostatus för hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="2197c-153">hello JSON output shows in hello properties field hello basic information for hello probe health status.</span></span> <span data-ttu-id="2197c-154">Hej *dipDownCount* egenskapen visar hello Totalt antal instanser på hello serverdel som inte tar emot nätverkstrafik på grund av toofailed avsökningen svar.</span><span class="sxs-lookup"><span data-stu-id="2197c-154">hello *dipDownCount* property shows hello total number of instances on hello back-end which are not receiving network traffic due toofailed probe responses.</span></span>

## <a name="view-and-analyze-hello-audit-log"></a><span data-ttu-id="2197c-155">Visa och analysera hello granskningsloggen</span><span class="sxs-lookup"><span data-stu-id="2197c-155">View and analyze hello audit log</span></span>

<span data-ttu-id="2197c-156">Du kan visa och analysera granskningsdata med hjälp av hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="2197c-156">You can view and analyze audit log data using any of hello following methods:</span></span>

* <span data-ttu-id="2197c-157">**Azure-verktyg:** hello granskningsloggar via Azure PowerShell, hello Azure kommandoradsgränssnittet (CLI), hello Azure REST API för att hämta information om eller hello Azure preview portal.</span><span class="sxs-lookup"><span data-stu-id="2197c-157">**Azure tools:** Retrieve information from hello audit logs through Azure PowerShell, hello Azure Command Line Interface (CLI), hello Azure REST API, or hello Azure preview portal.</span></span> <span data-ttu-id="2197c-158">Stegvisa instruktioner för varje metod beskrivs i hello [granskningsåtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2197c-158">Step-by-step instructions for each method are detailed in hello [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="2197c-159">**Powerbi:** om du inte redan har en [Power BI](https://powerbi.microsoft.com/pricing) konto, du kan testa det gratis.</span><span class="sxs-lookup"><span data-stu-id="2197c-159">**Power BI:** If you do not already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="2197c-160">Med hjälp av hello [Azure-granskningsloggarna content pack för Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), du kan analysera dina data med förkonfigurerade instrumentpaneler och du kan anpassa vyer toosuit dina krav.</span><span class="sxs-lookup"><span data-stu-id="2197c-160">Using hello [Azure Audit Logs content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), you can analyze your data with pre-configured dashboards, or you can customize views toosuit your requirements.</span></span>

## <a name="view-and-analyze-hello-health-probe-and-event-log"></a><span data-ttu-id="2197c-161">Visa och analysera hello hälsoavsökningen och händelseloggen</span><span class="sxs-lookup"><span data-stu-id="2197c-161">View and analyze hello health probe and event log</span></span>

<span data-ttu-id="2197c-162">Du behöver tooconnect tooyour storage-konto och hämta hello JSON-loggposter för händelsen och hälsa avsökningen loggar.</span><span class="sxs-lookup"><span data-stu-id="2197c-162">You need tooconnect tooyour storage account and retrieve hello JSON log entries for event and health probe logs.</span></span> <span data-ttu-id="2197c-163">När du hämtar hello JSON-filer, kan du omvandla dem tooCSV och visa i Excel, PowerBI eller andra data visualiseringen verktyg.</span><span class="sxs-lookup"><span data-stu-id="2197c-163">Once you download hello JSON files, you can convert them tooCSV and view in Excel, PowerBI, or any other data visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="2197c-164">Om du är bekant med Visual Studio och grundläggande begrepp för att ändra värdena för variabler i C# och konstanter, kan du använda hello [logga konverteraren verktyg](https://github.com/Azure-Samples/networking-dotnet-log-converter) tillgängliga från GitHub.</span><span class="sxs-lookup"><span data-stu-id="2197c-164">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use hello [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2197c-165">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2197c-165">Additional resources</span></span>

* <span data-ttu-id="2197c-166">[Visualisera dina Azure-granskningsloggarna med Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="2197c-166">[Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="2197c-167">[Visa och analysera Azure-granskningsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="2197c-167">[View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2197c-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2197c-168">Next steps</span></span>

[<span data-ttu-id="2197c-169">Förstå avsökningar av belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="2197c-169">Understand load balancer probes</span></span>](load-balancer-custom-probe-overview.md)
