---
title: "Azure SQL Analytics lösning i Log Analytics | Microsoft Docs"
description: "Azure SQL Analytics lösningen hjälper dig att hantera dina Azure SQL-databaser."
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
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a><span data-ttu-id="c1eb6-103">Övervaka Azure SQL Database med Azure SQL Analytics (förhandsgranskning) i logganalys</span><span class="sxs-lookup"><span data-stu-id="c1eb6-103">Monitor Azure SQL Database using Azure SQL Analytics (Preview) in Log Analytics</span></span>

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

<span data-ttu-id="c1eb6-105">Azure SQL Analytics lösningen i Azure Log Analytics samlar in och visualizes viktiga mått för prestanda för SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-105">The Azure SQL Analytics solution in Azure Log Analytics collects and visualizes important SQL Azure performance metrics.</span></span> <span data-ttu-id="c1eb6-106">Med det mått som du samlar in med lösningen kan skapa du anpassade regler för övervakning och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-106">By using the metrics that you collect with the solution, you can create custom monitoring rules and alerts.</span></span> <span data-ttu-id="c1eb6-107">Du kan övervaka Azure SQL Database och elastisk pool mått över flera Azure-prenumerationer och elastiska pooler och visualisera dem.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-107">And, you can monitor Azure SQL Database and elastic pool metrics across multiple Azure subscriptions and elastic pools and visualize them.</span></span> <span data-ttu-id="c1eb6-108">Lösningen hjälper dig att identifiera problem på varje nivå i program-stacken.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-108">The solution also helps you to identify issues at each layer of your application stack.</span></span>  <span data-ttu-id="c1eb6-109">Den använder [Azure diagnostisk mått](log-analytics-azure-storage.md) tillsammans med logganalys vyer presentera data om dina Azure SQL-databaser och elastiska pooler i en enda logganalys-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-109">It uses [Azure Diagnostic metrics](log-analytics-azure-storage.md) together with Log Analytics views to present data about all your Azure SQL databases and elastic pools in a single Log Analytics workspace.</span></span>

<span data-ttu-id="c1eb6-110">Den här preview-lösningen stöder för närvarande, upp till 150 000 Azure SQL-databaser och 5 000 SQL elastiska pooler per arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-110">Currently, this preview solution supports up to 150,000 Azure SQL Databases and 5,000 SQL Elastic Pools per workspace.</span></span>

<span data-ttu-id="c1eb6-111">Azure SQL Analytics-lösningen, precis som andra tillgängliga för Log Analytics hjälper dig att övervaka och ta emot meddelanden om hälsotillståndet för dina Azure-resurser – i det här fallet, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-111">The Azure SQL Analytics solution, like others available for Log Analytics, helps you monitor and receive notifications about the health of your Azure resources—in this case, Azure SQL Database.</span></span> <span data-ttu-id="c1eb6-112">Microsoft Azure SQL Database är en skalbar relationsdatabas-tjänst som tillhandahåller välbekanta SQL-Server-liknande funktioner till program som körs i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-112">Microsoft Azure SQL Database is a scalable relational database service that provides familiar SQL-Server-like capabilities to applications running in the Azure cloud.</span></span> <span data-ttu-id="c1eb6-113">Logganalys hjälper dig att samla in, korrelera och visualisera strukturerade och Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-113">Log Analytics helps you to collect, correlate, and visualize structured and unstructured data.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="c1eb6-114">Anslutna källor</span><span class="sxs-lookup"><span data-stu-id="c1eb6-114">Connected sources</span></span>

<span data-ttu-id="c1eb6-115">Azure SQL Analytics lösningen använder inte agenter för att ansluta till Log Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-115">The Azure SQL Analytics solution doesn't use agents to connect to the Log Analytics service.</span></span>

<span data-ttu-id="c1eb6-116">I följande tabell beskrivs de anslutna källor som stöds av den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-116">The following table describes the connected sources that are supported by this solution.</span></span>

| <span data-ttu-id="c1eb6-117">Ansluten källa</span><span class="sxs-lookup"><span data-stu-id="c1eb6-117">Connected Source</span></span> | <span data-ttu-id="c1eb6-118">Support</span><span class="sxs-lookup"><span data-stu-id="c1eb6-118">Support</span></span> | <span data-ttu-id="c1eb6-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1eb6-119">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="c1eb6-120">Windows-agenter</span><span class="sxs-lookup"><span data-stu-id="c1eb6-120">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="c1eb6-121">Nej</span><span class="sxs-lookup"><span data-stu-id="c1eb6-121">No</span></span> | <span data-ttu-id="c1eb6-122">Direkt Windows-agenter som inte används av lösningen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-122">Direct Windows agents are not used by the solution.</span></span> |
| [<span data-ttu-id="c1eb6-123">Linux-agenter</span><span class="sxs-lookup"><span data-stu-id="c1eb6-123">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="c1eb6-124">Nej</span><span class="sxs-lookup"><span data-stu-id="c1eb6-124">No</span></span> | <span data-ttu-id="c1eb6-125">Direkt Linux-agenter som inte används av lösningen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-125">Direct Linux agents are not used by the solution.</span></span> |
| [<span data-ttu-id="c1eb6-126">SCOM-hanteringsgrupp</span><span class="sxs-lookup"><span data-stu-id="c1eb6-126">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="c1eb6-127">Nej</span><span class="sxs-lookup"><span data-stu-id="c1eb6-127">No</span></span> | <span data-ttu-id="c1eb6-128">En direkt anslutning från SCOM-agent till logganalys används inte av lösningen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-128">A direct connection from the SCOM agent to Log Analytics is not used by the solution.</span></span> |
| [<span data-ttu-id="c1eb6-129">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="c1eb6-129">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="c1eb6-130">Nej</span><span class="sxs-lookup"><span data-stu-id="c1eb6-130">No</span></span> | <span data-ttu-id="c1eb6-131">Logganalys inte att läsa data från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-131">Log Analytics does not read the data from a storage account.</span></span> |
| [<span data-ttu-id="c1eb6-132">Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="c1eb6-132">Azure diagnostics</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="c1eb6-133">Ja</span><span class="sxs-lookup"><span data-stu-id="c1eb6-133">Yes</span></span> | <span data-ttu-id="c1eb6-134">Azure mått data skickas till logganalys direkt av Azure.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-134">Azure metric data is sent to Log Analytics directly by Azure.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="c1eb6-135">Krav</span><span class="sxs-lookup"><span data-stu-id="c1eb6-135">Prerequisites</span></span>

- <span data-ttu-id="c1eb6-136">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-136">An Azure Subscription.</span></span> <span data-ttu-id="c1eb6-137">Om du inte har någon, kan du skapa en för [ledigt](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c1eb6-137">If you don't have one, you can create one for [free](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="c1eb6-138">Logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-138">A Log Analytics workspace.</span></span> <span data-ttu-id="c1eb6-139">Du kan använda en befintlig eller kan du [skapa en ny](log-analytics-get-started.md) innan du börjar använda den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-139">You can use an existing one, or you can [create a new one](log-analytics-get-started.md) before you start using this solution.</span></span>
- <span data-ttu-id="c1eb6-140">Aktivera Azure-diagnostik för din Azure SQL-databaser och elastiska pooler och [konfigurera dem att skicka data till logganalys](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="c1eb6-140">Enable Azure Diagnostics for your Azure SQL databases and elastic pools and [configure them to send their data to Log Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span>

## <a name="configuration"></a><span data-ttu-id="c1eb6-141">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c1eb6-141">Configuration</span></span>

<span data-ttu-id="c1eb6-142">Utför följande steg för att lägga till Azure SQL Analytics lösningen till din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-142">Perform the following steps to add the Azure SQL Analytics solution to your workspace.</span></span>

1. <span data-ttu-id="c1eb6-143">Lägg till Azure SQL Analytics-lösning till arbetsytan från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) eller genom att använda processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="c1eb6-143">Add the Azure SQL Analytics solution to your workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="c1eb6-144">I Azure-portalen klickar du på **ny** (den symbolen +), Välj i listan över resurser, **övervakning + Management**.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-144">In the Azure portal, click **New** (the + symbol), then in the list of resources, select **Monitoring + Management**.</span></span>  
    <span data-ttu-id="c1eb6-145">![Övervakning och hantering](./media/log-analytics-azure-sql/monitoring-management.png)</span><span class="sxs-lookup"><span data-stu-id="c1eb6-145">![Monitoring + Management](./media/log-analytics-azure-sql/monitoring-management.png)</span></span>
3. <span data-ttu-id="c1eb6-146">I den **övervakning + Management** lista Klicka **se alla**.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-146">In the **Monitoring + Management** list click **See all**.</span></span>
4. <span data-ttu-id="c1eb6-147">I den **rekommenderas** klickar du på **mer** , och sedan i den nya listan **Azure SQL Analytics (förhandsgranskning)** och markera den.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-147">In the **Recommended** list, click **More** , and then in the new list, find **Azure SQL Analytics (Preview)** and then select it.</span></span>  
    <span data-ttu-id="c1eb6-148">![Azure SQL Analytics-lösning](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span><span class="sxs-lookup"><span data-stu-id="c1eb6-148">![Azure SQL Analytics solution](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span></span>
5. <span data-ttu-id="c1eb6-149">I den **Azure SQL Analytics (förhandsgranskning)** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-149">In the **Azure SQL Analytics (Preview)** blade, click **Create**.</span></span>  
    <span data-ttu-id="c1eb6-150">![Skapa](./media/log-analytics-azure-sql/portal-create.png)</span><span class="sxs-lookup"><span data-stu-id="c1eb6-150">![Create](./media/log-analytics-azure-sql/portal-create.png)</span></span>
6. <span data-ttu-id="c1eb6-151">I den **Skapa ny lösning** bladet, väljer arbetsytan som du vill lägga till lösningen på och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-151">In the **Create new solution** blade, select the workspace that you want to add the solution to and then click **Create**.</span></span>  
    <span data-ttu-id="c1eb6-152">![Lägg till arbetsytan](./media/log-analytics-azure-sql/add-to-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="c1eb6-152">![add to workspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span></span>


### <a name="to-configure-multiple-azure-subscriptions"></a><span data-ttu-id="c1eb6-153">Så här konfigurerar du Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="c1eb6-153">To configure multiple Azure subscriptions</span></span>

<span data-ttu-id="c1eb6-154">För att stödja flera prenumerationer, använder du PowerShell-skriptet från [aktivera Azure resource mått loggning med hjälp av PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="c1eb6-154">To support multiple subscriptions, use the PowerShell script from [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span> <span data-ttu-id="c1eb6-155">Ange arbetsytan resurs-ID som en parameter när du kör skriptet för att skicka diagnostikdata från resurser i en Azure-prenumeration till en arbetsyta i en annan Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-155">Provide the workspace resource ID as a parameter when executing the script to send diagnostic data from resources in one Azure subscription to a workspace in another Azure subscription.</span></span>

<span data-ttu-id="c1eb6-156">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="c1eb6-156">**Example**</span></span>

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a><span data-ttu-id="c1eb6-157">Använda lösningen</span><span class="sxs-lookup"><span data-stu-id="c1eb6-157">Using the solution</span></span>

<span data-ttu-id="c1eb6-158">När du lägger till lösningen till din arbetsyta Azure SQL Analytics panel har lagts till din arbetsyta och det visas i Översikt.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-158">When you add the solution to your workspace, the Azure SQL Analytics tile is added to your workspace, and it appears in Overview.</span></span> <span data-ttu-id="c1eb6-159">Panelen visar antalet Azure SQL-databaser och elastiska pooler i Azure SQL som lösningen är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-159">The tile shows the number of Azure SQL databases and Azure SQL elastic pools that the solution is connected to.</span></span>

![Azure SQL Analytics sida vid sida](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a><span data-ttu-id="c1eb6-161">Visa analysdata för Azure SQL</span><span class="sxs-lookup"><span data-stu-id="c1eb6-161">Viewing Azure SQL Analytics data</span></span>

<span data-ttu-id="c1eb6-162">Klicka på den **Azure SQL Analytics** öppna Azure SQL Analytics-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-162">Click on the **Azure SQL Analytics** tile to open the Azure SQL Analytics dashboard.</span></span> <span data-ttu-id="c1eb6-163">Instrumentpanelen innehåller blad som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-163">The dashboard includes the blades defined below.</span></span> <span data-ttu-id="c1eb6-164">Varje bladet visar upp till 15 resurser (prenumeration, server, elastisk pool och databas).</span><span class="sxs-lookup"><span data-stu-id="c1eb6-164">Each blade lists up to 15 resources (subscription, server, elastic pool, and database).</span></span> <span data-ttu-id="c1eb6-165">Klicka på någon av resurser för att öppna instrumentpanelen för den särskilda resursen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-165">Click any of the resources to open the dashboard for that specific resource.</span></span> <span data-ttu-id="c1eb6-166">Elastisk Pool eller databasen innehåller diagram med mått för en markerad resurs.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-166">Elastic Pool or Database contains the charts with metrics for a selected resource.</span></span> <span data-ttu-id="c1eb6-167">Klicka på ett diagram om du vill öppna dialogrutan Sök i loggfilen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-167">Click a chart to open the Log Search dialog.</span></span>

| <span data-ttu-id="c1eb6-168">Bladet</span><span class="sxs-lookup"><span data-stu-id="c1eb6-168">Blade</span></span> | <span data-ttu-id="c1eb6-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1eb6-169">Description</span></span> |
|---|---|
| <span data-ttu-id="c1eb6-170">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="c1eb6-170">Subscriptions</span></span> | <span data-ttu-id="c1eb6-171">Listan över prenumerationer med antal anslutna servrar, pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-171">List of subscriptions with number of connected servers, pools, and databases.</span></span> |
| <span data-ttu-id="c1eb6-172">Servrar</span><span class="sxs-lookup"><span data-stu-id="c1eb6-172">Servers</span></span> | <span data-ttu-id="c1eb6-173">Listan över servrar med antalet anslutna pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-173">List of servers with number of connected pools and databases.</span></span> |
| <span data-ttu-id="c1eb6-174">Elastiska pooler</span><span class="sxs-lookup"><span data-stu-id="c1eb6-174">Elastic Pools</span></span> | <span data-ttu-id="c1eb6-175">Lista över anslutna elastiska pooler med högsta GB och eDTU i den observerade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-175">List of connected elastic pools with maximum GB and eDTU in the observed period.</span></span> |
|<span data-ttu-id="c1eb6-176">Databaser</span><span class="sxs-lookup"><span data-stu-id="c1eb6-176">Databases</span></span> | <span data-ttu-id="c1eb6-177">Lista över anslutna databaser med högsta GB och DTU i den observerade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-177">List of connected databases with maximum GB and DTU in the observed period.</span></span>|


### <a name="analyze-data-and-create-alerts"></a><span data-ttu-id="c1eb6-178">Analysera data och skapa varningar</span><span class="sxs-lookup"><span data-stu-id="c1eb6-178">Analyze data and create alerts</span></span>

<span data-ttu-id="c1eb6-179">Du kan enkelt skapa aviseringar med data från Azure SQL Database-resurser.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-179">You can easily create alerts with the data coming from Azure SQL Database resources.</span></span> <span data-ttu-id="c1eb6-180">Här följer några användbara [loggen Sök](log-analytics-log-searches.md) frågor som du kan använda för aviseringar:</span><span class="sxs-lookup"><span data-stu-id="c1eb6-180">Here are a couple of useful [log search](log-analytics-log-searches.md) queries that you can use for alerting:</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


<span data-ttu-id="c1eb6-181">*Hög DTU på Azure SQL-databas*</span><span class="sxs-lookup"><span data-stu-id="c1eb6-181">*High DTU on Azure SQL Database*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="c1eb6-182">*Hög DTU på Azure SQL Database-elastisk Pool*</span><span class="sxs-lookup"><span data-stu-id="c1eb6-182">*High DTU on Azure SQL Database Elastic Pool*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="c1eb6-183">Du kan använda dessa avisering-baserade frågor för att Avisera om specifika tröskelvärden för både Azure SQL Database och elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-183">You can use these alert-based queries to alert on specific thresholds for both Azure SQL Database and elastic pools.</span></span> <span data-ttu-id="c1eb6-184">Konfigurera en avisering för din OMS-arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="c1eb6-184">To configure an alert for your OMS workspace:</span></span>

#### <a name="to-configure-an-alert-for-your-workspace"></a><span data-ttu-id="c1eb6-185">Så här konfigurerar du en avisering för din arbetsyta</span><span class="sxs-lookup"><span data-stu-id="c1eb6-185">To configure an alert for your workspace</span></span>

1. <span data-ttu-id="c1eb6-186">Gå till den [OMS-portalen](http://mms.microsoft.com/) och logga in.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-186">Go to the [OMS portal](http://mms.microsoft.com/) and sign in.</span></span>
2. <span data-ttu-id="c1eb6-187">Öppna arbetsytan som du har konfigurerat för lösningen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-187">Open the workspace that you have configured for the solution.</span></span>
3. <span data-ttu-id="c1eb6-188">På sidan Översikt över den **Azure SQL Analytics (förhandsgranskning)** panelen.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-188">On the Overview page, click the **Azure SQL Analytics (Preview)** tile.</span></span>
4. <span data-ttu-id="c1eb6-189">Kör något av de exempel på frågorna.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-189">Run one of the example queries.</span></span>
5. <span data-ttu-id="c1eb6-190">I loggen sökning, klickar du på **avisering**.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-190">In Log Search, click **Alert**.</span></span>  
<span data-ttu-id="c1eb6-191">![Skapa en avisering i sökningen](./media/log-analytics-azure-sql/create-alert01.png)</span><span class="sxs-lookup"><span data-stu-id="c1eb6-191">![create alert in search](./media/log-analytics-azure-sql/create-alert01.png)</span></span>
6. <span data-ttu-id="c1eb6-192">På den **lägga till Varningsregeln** konfigurerar lämpliga egenskaper och specifika tröskelvärden som du vill använda och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-192">On the **Add Alert Rule** page, configure the appropriate properties and the specific thresholds that you want and then click **Save**.</span></span>  
<span data-ttu-id="c1eb6-193">![lägga till varningsregel](./media/log-analytics-azure-sql/create-alert02.png)</span><span class="sxs-lookup"><span data-stu-id="c1eb6-193">![add alert rule](./media/log-analytics-azure-sql/create-alert02.png)</span></span>

### <a name="act-on-azure-sql-analytics-data"></a><span data-ttu-id="c1eb6-194">Arbeta med Azure SQL analysdata</span><span class="sxs-lookup"><span data-stu-id="c1eb6-194">Act on Azure SQL Analytics data</span></span>

<span data-ttu-id="c1eb6-195">Exempelvis är en av de mest användbara frågor som du kan utföra att jämföra DTU-användningen för alla Azure SQL elastiska pooler över alla dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-195">As an example, one of the most useful queries that you can perform is to compare the DTU utilization for all Azure SQL Elastic Pools across all your Azure subscriptions.</span></span> <span data-ttu-id="c1eb6-196">Databasen genomströmning Units (DTU) är ett sätt att beskriva relativa kapacitet för nivåerna Basic, Standard och Premium-databaser och pooler.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-196">Database Throughput Unit (DTU) provides a way to describe the relative capacity of a performance level of Basic, Standard, and Premium databases and pools.</span></span> <span data-ttu-id="c1eb6-197">Dtu: er baseras på ett blandat mått av CPU, minne, läser och skriver.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-197">DTUs are based on a blended measure of CPU, memory, reads, and writes.</span></span> <span data-ttu-id="c1eb6-198">Dtu: er ökar, ökar den effekt som erbjuds av prestandanivå.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-198">As DTUs increase, the power offered by the performance level increases.</span></span> <span data-ttu-id="c1eb6-199">Till exempel har prestandanivå med 5 dtu: er fem gånger mer än en prestandanivå med 1 DTU.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-199">For example, a performance level with 5 DTUs has five times more power than a performance level with 1 DTU.</span></span> <span data-ttu-id="c1eb6-200">En högsta DTU-kvot gäller för varje server och en elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-200">A maximum DTU quota applies to each server and elastic pool.</span></span>

<span data-ttu-id="c1eb6-201">Genom att köra följande loggen frågan kan se du lätt om du modellfunktionerna eller via använder din SQL Azure elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-201">By running the following Log Search query, you can easily tell if you are underutilizing or over utilizing your SQL Azure elastic pools.</span></span>

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> <span data-ttu-id="c1eb6-202">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), sedan frågan ovan skulle ändra till följande.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-202">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

<span data-ttu-id="c1eb6-203">I följande exempel visas att en elastisk pool har ett hårt nära 100% DTU medan andra har mycket lite användning.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-203">In the following example, you can see that one elastic pool has a high usage near 100% DTU while others have very little usage.</span></span> <span data-ttu-id="c1eb6-204">Du kan undersöka vidare för att felsöka eventuella ändringar i din miljö som använder Azure aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-204">You can investigate further to troubleshoot potential recent changes in your environment using Azure Activity logs.</span></span>

![Sökresultat för logg - hög belastning](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a><span data-ttu-id="c1eb6-206">Se även</span><span class="sxs-lookup"><span data-stu-id="c1eb6-206">See also</span></span>

- <span data-ttu-id="c1eb6-207">Använd [loggen sökningar](log-analytics-log-searches.md) i logganalys att visa detaljerad Azure SQL-data.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-207">Use [Log Searches](log-analytics-log-searches.md) in Log Analytics to view detailed Azure SQL data.</span></span>
- <span data-ttu-id="c1eb6-208">[Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) Azure SQL data visas.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-208">[Create your own dashboards](log-analytics-dashboards.md) showing Azure SQL data.</span></span>
- <span data-ttu-id="c1eb6-209">[Skapa aviseringar](log-analytics-alerts.md) när specifika Azure SQL-händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="c1eb6-209">[Create alerts](log-analytics-alerts.md) when specific Azure SQL events occur.</span></span>
