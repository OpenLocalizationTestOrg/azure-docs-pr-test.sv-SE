---
title: "aaaAzure SQL Analytics lösning i Log Analytics | Microsoft Docs"
description: "hello Azure SQL Analytics lösningen hjälper dig att hantera dina Azure SQL-databaser."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: fe228bb3cb3f9d578a84707c3917f02fbeb8a627
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a><span data-ttu-id="5dac1-103">Övervaka Azure SQL Database med Azure SQL Analytics (förhandsgranskning) i logganalys</span><span class="sxs-lookup"><span data-stu-id="5dac1-103">Monitor Azure SQL Database using Azure SQL Analytics (Preview) in Log Analytics</span></span>

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

<span data-ttu-id="5dac1-105">hello Azure SQL Analytics lösning i Azure Log Analytics samlar in och visualizes viktiga mått för prestanda för SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="5dac1-105">hello Azure SQL Analytics solution in Azure Log Analytics collects and visualizes important SQL Azure performance metrics.</span></span> <span data-ttu-id="5dac1-106">Använder hello mått som du samlar in hello-lösning kan skapa du anpassade regler för övervakning och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="5dac1-106">By using hello metrics that you collect with hello solution, you can create custom monitoring rules and alerts.</span></span> <span data-ttu-id="5dac1-107">Du kan övervaka Azure SQL Database och elastisk pool mått över flera Azure-prenumerationer och elastiska pooler och visualisera dem.</span><span class="sxs-lookup"><span data-stu-id="5dac1-107">And, you can monitor Azure SQL Database and elastic pool metrics across multiple Azure subscriptions and elastic pools and visualize them.</span></span> <span data-ttu-id="5dac1-108">hello lösningen hjälper dig också tooidentify problem på varje nivå i program-stacken.</span><span class="sxs-lookup"><span data-stu-id="5dac1-108">hello solution also helps you tooidentify issues at each layer of your application stack.</span></span>  <span data-ttu-id="5dac1-109">Den använder [Azure diagnostisk mått](log-analytics-azure-storage.md) tillsammans med logganalys-vyer toopresent data om alla Azure SQL-databaser och elastiska pooler i en enda logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="5dac1-109">It uses [Azure Diagnostic metrics](log-analytics-azure-storage.md) together with Log Analytics views toopresent data about all your Azure SQL databases and elastic pools in a single Log Analytics workspace.</span></span>

<span data-ttu-id="5dac1-110">Den här preview-lösningen stöder för närvarande upp too150 000 Azure SQL-databaser och 5 000 SQL elastiska pooler per arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="5dac1-110">Currently, this preview solution supports up too150,000 Azure SQL Databases and 5,000 SQL Elastic Pools per workspace.</span></span>

<span data-ttu-id="5dac1-111">hello Azure SQL Analytics lösning, precis som andra tillgängliga för Log Analytics hjälper dig att övervaka och ta emot meddelanden om hello hälsotillstånd resurserna i Azure – i det här fallet, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5dac1-111">hello Azure SQL Analytics solution, like others available for Log Analytics, helps you monitor and receive notifications about hello health of your Azure resources—in this case, Azure SQL Database.</span></span> <span data-ttu-id="5dac1-112">Microsoft Azure SQL Database är en skalbar relationsdatabas-tjänst som tillhandahåller välbekanta SQL-Server-liknande funktioner tooapplications körs i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="5dac1-112">Microsoft Azure SQL Database is a scalable relational database service that provides familiar SQL-Server-like capabilities tooapplications running in hello Azure cloud.</span></span> <span data-ttu-id="5dac1-113">Logganalys hjälper dig att toocollect, korrelera och visualisera strukturerade och Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="5dac1-113">Log Analytics helps you toocollect, correlate, and visualize structured and unstructured data.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="5dac1-114">Anslutna källor</span><span class="sxs-lookup"><span data-stu-id="5dac1-114">Connected sources</span></span>

<span data-ttu-id="5dac1-115">hello Azure SQL Analytics lösningen använder inte agenter tooconnect toohello logganalys-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5dac1-115">hello Azure SQL Analytics solution doesn't use agents tooconnect toohello Log Analytics service.</span></span>

<span data-ttu-id="5dac1-116">hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="5dac1-116">hello following table describes hello connected sources that are supported by this solution.</span></span>

| <span data-ttu-id="5dac1-117">Ansluten källa</span><span class="sxs-lookup"><span data-stu-id="5dac1-117">Connected Source</span></span> | <span data-ttu-id="5dac1-118">Support</span><span class="sxs-lookup"><span data-stu-id="5dac1-118">Support</span></span> | <span data-ttu-id="5dac1-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5dac1-119">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="5dac1-120">Windows-agenter</span><span class="sxs-lookup"><span data-stu-id="5dac1-120">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="5dac1-121">Nej</span><span class="sxs-lookup"><span data-stu-id="5dac1-121">No</span></span> | <span data-ttu-id="5dac1-122">Direkt Windows-agenter som inte används av hello lösning.</span><span class="sxs-lookup"><span data-stu-id="5dac1-122">Direct Windows agents are not used by hello solution.</span></span> |
| [<span data-ttu-id="5dac1-123">Linux-agenter</span><span class="sxs-lookup"><span data-stu-id="5dac1-123">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="5dac1-124">Nej</span><span class="sxs-lookup"><span data-stu-id="5dac1-124">No</span></span> | <span data-ttu-id="5dac1-125">Direkt Linux-agenter som inte används av hello lösning.</span><span class="sxs-lookup"><span data-stu-id="5dac1-125">Direct Linux agents are not used by hello solution.</span></span> |
| [<span data-ttu-id="5dac1-126">SCOM-hanteringsgrupp</span><span class="sxs-lookup"><span data-stu-id="5dac1-126">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="5dac1-127">Nej</span><span class="sxs-lookup"><span data-stu-id="5dac1-127">No</span></span> | <span data-ttu-id="5dac1-128">En direkt anslutning från hello SCOM-agent tooLog Analytics används inte av hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="5dac1-128">A direct connection from hello SCOM agent tooLog Analytics is not used by hello solution.</span></span> |
| [<span data-ttu-id="5dac1-129">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="5dac1-129">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="5dac1-130">Nej</span><span class="sxs-lookup"><span data-stu-id="5dac1-130">No</span></span> | <span data-ttu-id="5dac1-131">Logganalys läser inte hello data från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5dac1-131">Log Analytics does not read hello data from a storage account.</span></span> |
| [<span data-ttu-id="5dac1-132">Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="5dac1-132">Azure diagnostics</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="5dac1-133">Ja</span><span class="sxs-lookup"><span data-stu-id="5dac1-133">Yes</span></span> | <span data-ttu-id="5dac1-134">Azure mått data skickas tooLog Analytics direkt av Azure.</span><span class="sxs-lookup"><span data-stu-id="5dac1-134">Azure metric data is sent tooLog Analytics directly by Azure.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="5dac1-135">Krav</span><span class="sxs-lookup"><span data-stu-id="5dac1-135">Prerequisites</span></span>

- <span data-ttu-id="5dac1-136">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5dac1-136">An Azure Subscription.</span></span> <span data-ttu-id="5dac1-137">Om du inte har någon, kan du skapa en för [ledigt](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="5dac1-137">If you don't have one, you can create one for [free](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="5dac1-138">Logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="5dac1-138">A Log Analytics workspace.</span></span> <span data-ttu-id="5dac1-139">Du kan använda en befintlig eller kan du [skapa en ny](log-analytics-get-started.md) innan du börjar använda den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="5dac1-139">You can use an existing one, or you can [create a new one](log-analytics-get-started.md) before you start using this solution.</span></span>
- <span data-ttu-id="5dac1-140">Aktivera Azure-diagnostik för din Azure SQL-databaser och elastiska pooler och [konfigurera dem toosend sina data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="5dac1-140">Enable Azure Diagnostics for your Azure SQL databases and elastic pools and [configure them toosend their data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span>

## <a name="configuration"></a><span data-ttu-id="5dac1-141">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="5dac1-141">Configuration</span></span>

<span data-ttu-id="5dac1-142">Utför följande steg tooadd hello Azure SQL Analytics lösning tooyour arbetsytan hello.</span><span class="sxs-lookup"><span data-stu-id="5dac1-142">Perform hello following steps tooadd hello Azure SQL Analytics solution tooyour workspace.</span></span>

1. <span data-ttu-id="5dac1-143">Lägg till hello Azure SQL Analytics lösning tooyour arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="5dac1-143">Add hello Azure SQL Analytics solution tooyour workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="5dac1-144">I hello Azure-portalen klickar du på **ny** (hello symbolen +), i hello lista över resurser, väljer du **övervakning + Management**.</span><span class="sxs-lookup"><span data-stu-id="5dac1-144">In hello Azure portal, click **New** (hello + symbol), then in hello list of resources, select **Monitoring + Management**.</span></span>  
    <span data-ttu-id="5dac1-145">![Övervakning och hantering](./media/log-analytics-azure-sql/monitoring-management.png)</span><span class="sxs-lookup"><span data-stu-id="5dac1-145">![Monitoring + Management](./media/log-analytics-azure-sql/monitoring-management.png)</span></span>
3. <span data-ttu-id="5dac1-146">I hello **övervakning + Management** lista Klicka **se alla**.</span><span class="sxs-lookup"><span data-stu-id="5dac1-146">In hello **Monitoring + Management** list click **See all**.</span></span>
4. <span data-ttu-id="5dac1-147">I hello **rekommenderas** klickar du på **mer** , och sedan i hello ny lista, **Azure SQL Analytics (förhandsgranskning)** och markera den.</span><span class="sxs-lookup"><span data-stu-id="5dac1-147">In hello **Recommended** list, click **More** , and then in hello new list, find **Azure SQL Analytics (Preview)** and then select it.</span></span>  
    <span data-ttu-id="5dac1-148">![Azure SQL Analytics-lösning](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span><span class="sxs-lookup"><span data-stu-id="5dac1-148">![Azure SQL Analytics solution](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span></span>
5. <span data-ttu-id="5dac1-149">I hello **Azure SQL Analytics (förhandsgranskning)** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5dac1-149">In hello **Azure SQL Analytics (Preview)** blade, click **Create**.</span></span>  
    <span data-ttu-id="5dac1-150">![Skapa](./media/log-analytics-azure-sql/portal-create.png)</span><span class="sxs-lookup"><span data-stu-id="5dac1-150">![Create](./media/log-analytics-azure-sql/portal-create.png)</span></span>
6. <span data-ttu-id="5dac1-151">I hello **Skapa ny lösning** bladet, Välj hello arbetsytan som du vill tooadd hello lösning tooand och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5dac1-151">In hello **Create new solution** blade, select hello workspace that you want tooadd hello solution tooand then click **Create**.</span></span>  
    <span data-ttu-id="5dac1-152">![Lägg till tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="5dac1-152">![add tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span></span>


### <a name="tooconfigure-multiple-azure-subscriptions"></a><span data-ttu-id="5dac1-153">tooconfigure flera Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="5dac1-153">tooconfigure multiple Azure subscriptions</span></span>

<span data-ttu-id="5dac1-154">toosupport flera prenumerationer använder hello PowerShell-skriptet från [aktivera Azure resource mått loggning med hjälp av PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="5dac1-154">toosupport multiple subscriptions, use hello PowerShell script from [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span> <span data-ttu-id="5dac1-155">Ange hello arbetsytan resurs-ID som en parameter när du kör skriptet hello toosend diagnostikdata från resurser i en Azure-prenumeration tooa arbetsyta i en annan Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5dac1-155">Provide hello workspace resource ID as a parameter when executing hello script toosend diagnostic data from resources in one Azure subscription tooa workspace in another Azure subscription.</span></span>

<span data-ttu-id="5dac1-156">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="5dac1-156">**Example**</span></span>

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-hello-solution"></a><span data-ttu-id="5dac1-157">Med hello-lösning</span><span class="sxs-lookup"><span data-stu-id="5dac1-157">Using hello solution</span></span>

<span data-ttu-id="5dac1-158">När du lägger till hello lösning tooyour arbetsytan hello Azure SQL Analytics panelen läggs tooyour arbetsytan och det visas i Översikt.</span><span class="sxs-lookup"><span data-stu-id="5dac1-158">When you add hello solution tooyour workspace, hello Azure SQL Analytics tile is added tooyour workspace, and it appears in Overview.</span></span> <span data-ttu-id="5dac1-159">hello panelen visar hello antalet Azure SQL-databaser och elastiska pooler i Azure SQL som hello lösningen är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="5dac1-159">hello tile shows hello number of Azure SQL databases and Azure SQL elastic pools that hello solution is connected to.</span></span>

![Azure SQL Analytics sida vid sida](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a><span data-ttu-id="5dac1-161">Visa analysdata för Azure SQL</span><span class="sxs-lookup"><span data-stu-id="5dac1-161">Viewing Azure SQL Analytics data</span></span>

<span data-ttu-id="5dac1-162">Klicka på hello **Azure SQL Analytics** panelen tooopen hello Azure SQL instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5dac1-162">Click on hello **Azure SQL Analytics** tile tooopen hello Azure SQL Analytics dashboard.</span></span> <span data-ttu-id="5dac1-163">hello instrumentpanelen innehåller hello blad som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="5dac1-163">hello dashboard includes hello blades defined below.</span></span> <span data-ttu-id="5dac1-164">Varje bladet visar too15 resurser (prenumeration, server, elastisk pool och databas).</span><span class="sxs-lookup"><span data-stu-id="5dac1-164">Each blade lists up too15 resources (subscription, server, elastic pool, and database).</span></span> <span data-ttu-id="5dac1-165">Klicka på någon av hello resurser tooopen hello instrumentpanelen för den särskilda resursen.</span><span class="sxs-lookup"><span data-stu-id="5dac1-165">Click any of hello resources tooopen hello dashboard for that specific resource.</span></span> <span data-ttu-id="5dac1-166">Elastisk Pool eller databasen innehåller hello diagram med mått för en markerad resurs.</span><span class="sxs-lookup"><span data-stu-id="5dac1-166">Elastic Pool or Database contains hello charts with metrics for a selected resource.</span></span> <span data-ttu-id="5dac1-167">Klicka på ett diagram tooopen hello loggen Sök dialogruta.</span><span class="sxs-lookup"><span data-stu-id="5dac1-167">Click a chart tooopen hello Log Search dialog.</span></span>

| <span data-ttu-id="5dac1-168">Bladet</span><span class="sxs-lookup"><span data-stu-id="5dac1-168">Blade</span></span> | <span data-ttu-id="5dac1-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5dac1-169">Description</span></span> |
|---|---|
| <span data-ttu-id="5dac1-170">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="5dac1-170">Subscriptions</span></span> | <span data-ttu-id="5dac1-171">Listan över prenumerationer med antal anslutna servrar, pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="5dac1-171">List of subscriptions with number of connected servers, pools, and databases.</span></span> |
| <span data-ttu-id="5dac1-172">Servrar</span><span class="sxs-lookup"><span data-stu-id="5dac1-172">Servers</span></span> | <span data-ttu-id="5dac1-173">Listan över servrar med antalet anslutna pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="5dac1-173">List of servers with number of connected pools and databases.</span></span> |
| <span data-ttu-id="5dac1-174">Elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="5dac1-174">Elastic Pools</span></span> | <span data-ttu-id="5dac1-175">Lista över anslutna elastiska pooler med högsta GB och eDTU i hello observerade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="5dac1-175">List of connected elastic pools with maximum GB and eDTU in hello observed period.</span></span> |
|<span data-ttu-id="5dac1-176">Databaser</span><span class="sxs-lookup"><span data-stu-id="5dac1-176">Databases</span></span> | <span data-ttu-id="5dac1-177">Lista över anslutna databaser med högsta GB och DTU i hello observerade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="5dac1-177">List of connected databases with maximum GB and DTU in hello observed period.</span></span>|


### <a name="analyze-data-and-create-alerts"></a><span data-ttu-id="5dac1-178">Analysera data och skapa varningar</span><span class="sxs-lookup"><span data-stu-id="5dac1-178">Analyze data and create alerts</span></span>

<span data-ttu-id="5dac1-179">Du kan enkelt skapa aviseringar med hello data från Azure SQL Database-resurser.</span><span class="sxs-lookup"><span data-stu-id="5dac1-179">You can easily create alerts with hello data coming from Azure SQL Database resources.</span></span> <span data-ttu-id="5dac1-180">Här följer några användbara [loggen Sök](log-analytics-log-searches.md) frågor som du kan använda för aviseringar:</span><span class="sxs-lookup"><span data-stu-id="5dac1-180">Here are a couple of useful [log search](log-analytics-log-searches.md) queries that you can use for alerting:</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


<span data-ttu-id="5dac1-181">*Hög DTU på Azure SQL-databas*</span><span class="sxs-lookup"><span data-stu-id="5dac1-181">*High DTU on Azure SQL Database*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="5dac1-182">*Hög DTU på Azure SQL Database-elastisk Pool*</span><span class="sxs-lookup"><span data-stu-id="5dac1-182">*High DTU on Azure SQL Database Elastic Pool*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="5dac1-183">Du kan använda dessa avisering-baserade frågor tooalert på specifika tröskelvärden för både Azure SQL Database och elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="5dac1-183">You can use these alert-based queries tooalert on specific thresholds for both Azure SQL Database and elastic pools.</span></span> <span data-ttu-id="5dac1-184">tooconfigure en avisering för din OMS-arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="5dac1-184">tooconfigure an alert for your OMS workspace:</span></span>

#### <a name="tooconfigure-an-alert-for-your-workspace"></a><span data-ttu-id="5dac1-185">tooconfigure en avisering för din arbetsyta</span><span class="sxs-lookup"><span data-stu-id="5dac1-185">tooconfigure an alert for your workspace</span></span>

1. <span data-ttu-id="5dac1-186">Gå toohello [OMS-portalen](http://mms.microsoft.com/) och logga in.</span><span class="sxs-lookup"><span data-stu-id="5dac1-186">Go toohello [OMS portal](http://mms.microsoft.com/) and sign in.</span></span>
2. <span data-ttu-id="5dac1-187">Öppna hello-arbetsyta som du har konfigurerat för hello lösning.</span><span class="sxs-lookup"><span data-stu-id="5dac1-187">Open hello workspace that you have configured for hello solution.</span></span>
3. <span data-ttu-id="5dac1-188">Klicka på översiktssidan för hello hello **Azure SQL Analytics (förhandsgranskning)** panelen.</span><span class="sxs-lookup"><span data-stu-id="5dac1-188">On hello Overview page, click hello **Azure SQL Analytics (Preview)** tile.</span></span>
4. <span data-ttu-id="5dac1-189">Kör något av hello exempelfrågor.</span><span class="sxs-lookup"><span data-stu-id="5dac1-189">Run one of hello example queries.</span></span>
5. <span data-ttu-id="5dac1-190">I loggen sökning, klickar du på **avisering**.</span><span class="sxs-lookup"><span data-stu-id="5dac1-190">In Log Search, click **Alert**.</span></span>  
<span data-ttu-id="5dac1-191">![Skapa en avisering i sökningen](./media/log-analytics-azure-sql/create-alert01.png)</span><span class="sxs-lookup"><span data-stu-id="5dac1-191">![create alert in search](./media/log-analytics-azure-sql/create-alert01.png)</span></span>
6. <span data-ttu-id="5dac1-192">På hello **lägga till Varningsregeln** konfigurerar hello lämpliga egenskaper och hello specifika tröskelvärden som du vill använda och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5dac1-192">On hello **Add Alert Rule** page, configure hello appropriate properties and hello specific thresholds that you want and then click **Save**.</span></span>  
<span data-ttu-id="5dac1-193">![lägga till varningsregel](./media/log-analytics-azure-sql/create-alert02.png)</span><span class="sxs-lookup"><span data-stu-id="5dac1-193">![add alert rule](./media/log-analytics-azure-sql/create-alert02.png)</span></span>

### <a name="act-on-azure-sql-analytics-data"></a><span data-ttu-id="5dac1-194">Arbeta med Azure SQL analysdata</span><span class="sxs-lookup"><span data-stu-id="5dac1-194">Act on Azure SQL Analytics data</span></span>

<span data-ttu-id="5dac1-195">Exempelvis är en hello mest användbara frågor som du kan utföra toocompare hello DTU-användningen för alla Azure SQL elastiska pooler över alla dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="5dac1-195">As an example, one of hello most useful queries that you can perform is toocompare hello DTU utilization for all Azure SQL Elastic Pools across all your Azure subscriptions.</span></span> <span data-ttu-id="5dac1-196">Databasen genomströmning Units (DTU) ger ett sätt toodescribe hello relativa kapacitet för nivåerna Basic, Standard och Premium-databaser och pooler.</span><span class="sxs-lookup"><span data-stu-id="5dac1-196">Database Throughput Unit (DTU) provides a way toodescribe hello relative capacity of a performance level of Basic, Standard, and Premium databases and pools.</span></span> <span data-ttu-id="5dac1-197">Dtu: er baseras på ett blandat mått av CPU, minne, läser och skriver.</span><span class="sxs-lookup"><span data-stu-id="5dac1-197">DTUs are based on a blended measure of CPU, memory, reads, and writes.</span></span> <span data-ttu-id="5dac1-198">Dtu: er ökar hello power erbjuds av hello prestanda ökar.</span><span class="sxs-lookup"><span data-stu-id="5dac1-198">As DTUs increase, hello power offered by hello performance level increases.</span></span> <span data-ttu-id="5dac1-199">Till exempel har prestandanivå med 5 dtu: er fem gånger mer än en prestandanivå med 1 DTU.</span><span class="sxs-lookup"><span data-stu-id="5dac1-199">For example, a performance level with 5 DTUs has five times more power than a performance level with 1 DTU.</span></span> <span data-ttu-id="5dac1-200">En högsta DTU-kvot gäller tooeach server och elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="5dac1-200">A maximum DTU quota applies tooeach server and elastic pool.</span></span>

<span data-ttu-id="5dac1-201">Genom att köra hello efter logg sökfråga kan se du lätt om du modellfunktionerna eller över använder din SQL Azure elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="5dac1-201">By running hello following Log Search query, you can easily tell if you are underutilizing or over utilizing your SQL Azure elastic pools.</span></span>

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> <span data-ttu-id="5dac1-202">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello ovan frågan skulle ändra toohello följande.</span><span class="sxs-lookup"><span data-stu-id="5dac1-202">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above query would change toohello following.</span></span>
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

<span data-ttu-id="5dac1-203">I följande exempel hello, ser du att en elastisk pool har ett hårt nära 100% DTU medan andra har mycket lite användning.</span><span class="sxs-lookup"><span data-stu-id="5dac1-203">In hello following example, you can see that one elastic pool has a high usage near 100% DTU while others have very little usage.</span></span> <span data-ttu-id="5dac1-204">Du kan undersöka ytterligare tootroubleshoot eventuella ändringar i din miljö som använder Azure aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="5dac1-204">You can investigate further tootroubleshoot potential recent changes in your environment using Azure Activity logs.</span></span>

![Sökresultat för logg - hög belastning](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a><span data-ttu-id="5dac1-206">Se även</span><span class="sxs-lookup"><span data-stu-id="5dac1-206">See also</span></span>

- <span data-ttu-id="5dac1-207">Använd [loggen sökningar](log-analytics-log-searches.md) i logganalys tooview detaljerade Azure SQL-data.</span><span class="sxs-lookup"><span data-stu-id="5dac1-207">Use [Log Searches](log-analytics-log-searches.md) in Log Analytics tooview detailed Azure SQL data.</span></span>
- <span data-ttu-id="5dac1-208">[Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) Azure SQL data visas.</span><span class="sxs-lookup"><span data-stu-id="5dac1-208">[Create your own dashboards](log-analytics-dashboards.md) showing Azure SQL data.</span></span>
- <span data-ttu-id="5dac1-209">[Skapa aviseringar](log-analytics-alerts.md) när specifika Azure SQL-händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="5dac1-209">[Create alerts](log-analytics-alerts.md) when specific Azure SQL events occur.</span></span>
