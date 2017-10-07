---
title: "aaaAzure SQL-databas mått & diagnostikloggning | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar Azure SQL Database Resursanvändning toostore resurs-, anslutnings- och statistik för körning av frågan."
services: sql-database
documentationcenter: 
author: vvasic
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: vvasic
ms.openlocfilehash: e6f9e24992ca4f84f701e1ef858e98dc7b481e28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a><span data-ttu-id="3ced3-103">Azure SQL Database-mätvärden och diagnostikloggning</span><span class="sxs-lookup"><span data-stu-id="3ced3-103">Azure SQL Database metrics and diagnostics logging</span></span> 
<span data-ttu-id="3ced3-104">Azure SQL Database kan sända mått och diagnostikloggar för lättare övervakning.</span><span class="sxs-lookup"><span data-stu-id="3ced3-104">Azure SQL Database can emit metrics and diagnostic logs for easier monitoring.</span></span> <span data-ttu-id="3ced3-105">Du kan konfigurera Azure SQL Database toostore Resursanvändning, personal och sessioner och anslutning till en av dessa Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="3ced3-105">You can configure Azure SQL Database toostore resource usage, workers and sessions, and connectivity into one of these Azure resources:</span></span>
- <span data-ttu-id="3ced3-106">**Azure Storage**: För arkivering av stora mängder telemetri till ett lågt pris</span><span class="sxs-lookup"><span data-stu-id="3ced3-106">**Azure Storage**: For archiving vast amounts of telemetry for a small price</span></span>
- <span data-ttu-id="3ced3-107">**Azure Event Hub**: för att integrera Azure SQL Database telemetri med anpassade övervakningslösning eller varm pipelines</span><span class="sxs-lookup"><span data-stu-id="3ced3-107">**Azure Event Hub**: For integrating Azure SQL Database telemetry with your custom monitoring solution or hot pipelines</span></span>
- <span data-ttu-id="3ced3-108">**Azure logganalys**: för out of box för hello övervakningslösning med reporting, varningar och minimera funktioner</span><span class="sxs-lookup"><span data-stu-id="3ced3-108">**Azure Log Analytics**: For out of hello box monitoring solution with reporting, alerting, and mitigating capabilities</span></span> 

    ![Arkitektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a><span data-ttu-id="3ced3-110">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="3ced3-110">Enable logging</span></span>

<span data-ttu-id="3ced3-111">Mätvärden och diagnostikfunktionerna loggning är inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="3ced3-111">Metrics and diagnostics logging is not enabled by default.</span></span> <span data-ttu-id="3ced3-112">Du kan aktivera och hantera mått och diagnostikloggning med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="3ced3-112">You can enable and manage metrics and diagnostics logging using one of hello following methods:</span></span>
- <span data-ttu-id="3ced3-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3ced3-113">Azure portal</span></span>
- <span data-ttu-id="3ced3-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3ced3-114">PowerShell</span></span>
- <span data-ttu-id="3ced3-115">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3ced3-115">Azure CLI</span></span>
- <span data-ttu-id="3ced3-116">REST API</span><span class="sxs-lookup"><span data-stu-id="3ced3-116">REST API</span></span> 
- <span data-ttu-id="3ced3-117">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3ced3-117">Resource Manager template</span></span>

<span data-ttu-id="3ced3-118">När du aktiverar mått och diagnostikloggning måste toospecify hello Azure-resurs där valda data samlas in.</span><span class="sxs-lookup"><span data-stu-id="3ced3-118">When you enable metrics and diagnostics logging, you need toospecify hello Azure resource where selected data is collected.</span></span> <span data-ttu-id="3ced3-119">Tillgängliga alternativ:</span><span class="sxs-lookup"><span data-stu-id="3ced3-119">Options available:</span></span>
- <span data-ttu-id="3ced3-120">Log Analytics</span><span class="sxs-lookup"><span data-stu-id="3ced3-120">Log analytics</span></span>
- <span data-ttu-id="3ced3-121">Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="3ced3-121">Event Hub</span></span>
- <span data-ttu-id="3ced3-122">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3ced3-122">Azure Storage</span></span> 

<span data-ttu-id="3ced3-123">Du kan etablera en ny resurs i Azure eller välj en befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="3ced3-123">You can provision a new Azure resource or select an existing resource.</span></span> <span data-ttu-id="3ced3-124">När du har valt hello lagringsresurs, måste du toospecify vilka data toocollect.</span><span class="sxs-lookup"><span data-stu-id="3ced3-124">After selecting hello storage resource, you need toospecify which data toocollect.</span></span> <span data-ttu-id="3ced3-125">Alternativen är:</span><span class="sxs-lookup"><span data-stu-id="3ced3-125">Options available include:</span></span>

- <span data-ttu-id="3ced3-126">**[1 minut mått](sql-database-metrics-diag-logging.md#1-minute-metrics)**  -innehåller DTU-procent, DTU gränsen, CPU-procent fysiska data läsa procent, Log skriva procent, lyckade/misslyckade/Blocked av brandväggens anslutningar, sessioner procent, arbetare procent lagring, lagringsprocent, XTP-lagringsprocent</span><span class="sxs-lookup"><span data-stu-id="3ced3-126">**[1-minute metrics](sql-database-metrics-diag-logging.md#1-minute-metrics)** - contains DTU percentage, DTU limit, CPU percentage, Physical data read percentage, Log write percentage, Successful/Failed/Blocked by firewall connections, sessions percentage, workers percentage, storage, storage percentage, XTP storage percentage</span></span>

<span data-ttu-id="3ced3-127">Om du anger Event Hub eller ett AzureStorage konto, kan du ange en kvarhållning princip toospecify de data som är äldre än en vald tidsperiod tas bort.</span><span class="sxs-lookup"><span data-stu-id="3ced3-127">If you specify Event Hub or an AzureStorage account, you can specify a retention policy toospecify that data that is older than a selected time period is deleted.</span></span> <span data-ttu-id="3ced3-128">Om du anger logganalys beroende hello bevarandeprincip hello valda prisnivån.</span><span class="sxs-lookup"><span data-stu-id="3ced3-128">If you specify Log Analytics, hello retention policy depends on hello selected pricing tier.</span></span> <span data-ttu-id="3ced3-129">Läs mer om [logganalys priser](https://azure.microsoft.com/pricing/details/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="3ced3-129">Read more about [Log Analytics pricing](https://azure.microsoft.com/pricing/details/log-analytics/).</span></span> 

<span data-ttu-id="3ced3-130">Vi rekommenderar att du läser båda hello [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) och [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artiklar toogain förstå inte bara hur tooenable loggning, men hello kategorier av mätvärden och loggfiler som stöds av hello olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3ced3-130">We recommend that you read both hello [Overview of metrics in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) and [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) articles toogain an understanding of not only how tooenable logging, but hello metrics and log categories supported by hello various Azure services.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="3ced3-131">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3ced3-131">Azure portal</span></span>

<span data-ttu-id="3ced3-132">tooenable mått diagnostikloggar samling i hello Azure-portalen navigera tooyour Azure SQL database eller elastisk pool-sidan och klicka sedan på **diagnostikinställningar**.</span><span class="sxs-lookup"><span data-stu-id="3ced3-132">tooenable metrics and diagnostic logs collection in hello Azure portal, navigate tooyour Azure SQL database or elastic pool page, and then click **Diagnostic settings**.</span></span>

   ![Aktivera i hello Azure-portalen](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a><span data-ttu-id="3ced3-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3ced3-134">PowerShell</span></span>

<span data-ttu-id="3ced3-135">tooenable mätvärden och diagnostikfunktionerna loggning Använd med hjälp av PowerShell, hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3ced3-135">tooenable metrics and diagnostics logging using PowerShell, use hello following commands:</span></span>

- <span data-ttu-id="3ced3-136">tooenable lagring av diagnostiska loggar i ett Lagringskonto, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-136">tooenable storage of Diagnostic Logs in a Storage Account, use this command:</span></span>

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   <span data-ttu-id="3ced3-137">hello Storage-konto-ID är hello resurs-id för hello storage-konto toowhich du vill toosend hello loggar.</span><span class="sxs-lookup"><span data-stu-id="3ced3-137">hello Storage Account ID is hello resource id for hello storage account toowhich you want toosend hello logs.</span></span>

- <span data-ttu-id="3ced3-138">tooenable strömning av diagnostikloggar tooan Händelsehubb, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-138">tooenable streaming of Diagnostic Logs tooan Event Hub, use this command:</span></span>

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   <span data-ttu-id="3ced3-139">hello regel-ID för Service Bus är en sträng med formatet:</span><span class="sxs-lookup"><span data-stu-id="3ced3-139">hello Service Bus Rule ID is a string with this format:</span></span>

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- <span data-ttu-id="3ced3-140">tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-140">tooenable sending of Diagnostic Logs tooa Log Analytics workspace, use this command:</span></span>

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
   ```

- <span data-ttu-id="3ced3-141">Du kan hämta hello resurs-id för logganalys-arbetsytan med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-141">You can obtain hello resource id of your Log Analytics workspace using hello following command:</span></span>

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

<span data-ttu-id="3ced3-142">Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="3ced3-142">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="cli"></a><span data-ttu-id="3ced3-143">CLI</span><span class="sxs-lookup"><span data-stu-id="3ced3-143">CLI</span></span>

<span data-ttu-id="3ced3-144">tooenable mått och diagnostik loggar med hello Azure CLI, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3ced3-144">tooenable metrics and diagnostics logging using hello Azure CLI, use hello following commands:</span></span>

- <span data-ttu-id="3ced3-145">tooenable lagring av diagnostiska loggar i ett Lagringskonto, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-145">tooenable storage of Diagnostic Logs in a Storage Account, use this command:</span></span>

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   <span data-ttu-id="3ced3-146">hello Storage-konto-ID är hello resurs-id för hello storage-konto toowhich du vill toosend hello loggar.</span><span class="sxs-lookup"><span data-stu-id="3ced3-146">hello Storage Account ID is hello resource id for hello storage account toowhich you want toosend hello logs.</span></span>

- <span data-ttu-id="3ced3-147">tooenable strömning av diagnostikloggar tooan Händelsehubb, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-147">tooenable streaming of Diagnostic Logs tooan Event Hub, use this command:</span></span>

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   <span data-ttu-id="3ced3-148">hello regel-ID för Service Bus är en sträng med formatet:</span><span class="sxs-lookup"><span data-stu-id="3ced3-148">hello Service Bus Rule ID is a string with this format:</span></span>

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- <span data-ttu-id="3ced3-149">tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ced3-149">tooenable sending of Diagnostic Logs tooa Log Analytics workspace, use this command:</span></span>

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
   ```

<span data-ttu-id="3ced3-150">Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="3ced3-150">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="rest-api"></a><span data-ttu-id="3ced3-151">REST API</span><span class="sxs-lookup"><span data-stu-id="3ced3-151">REST API</span></span>

<span data-ttu-id="3ced3-152">Läs om hur för[ändra diagnostikinställningar med hello Azure övervakaren REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ced3-152">Read about how too[change Diagnostic settings using hello Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> 

### <a name="resource-manager-template"></a><span data-ttu-id="3ced3-153">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3ced3-153">Resource Manager template</span></span>

<span data-ttu-id="3ced3-154">Läs om hur för[aktivera diagnostikinställningar när resursen skapas med hjälp av Resource Manager-mall](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md).</span><span class="sxs-lookup"><span data-stu-id="3ced3-154">Read about how too[enable Diagnostic settings at resource creation using Resource Manager template](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md).</span></span> 

## <a name="stream-into-log-analytics"></a><span data-ttu-id="3ced3-155">Dataströmmen till logganalys</span><span class="sxs-lookup"><span data-stu-id="3ced3-155">Stream into Log Analytics</span></span> 
<span data-ttu-id="3ced3-156">Azure SQL Database-mätvärden och diagnostiska loggar strömmas till logganalys alternativet hello inbyggda ”skicka tooLog Analytics” hello-portalen eller genom att aktivera logganalys i diagnostikinställningen via Azure PowerShell-cmdlets, Azure CLI eller Azure-Monitor REST API.</span><span class="sxs-lookup"><span data-stu-id="3ced3-156">Azure SQL Database metrics and diagnostic logs can be streamed into Log Analytics using hello built-in “Send tooLog Analytics” option in hello portal, or by enabling Log Analytics in a diagnostic setting via Azure PowerShell cmdlets, Azure CLI, or Azure Monitor REST API.</span></span>

### <a name="installation-overview"></a><span data-ttu-id="3ced3-157">Installationsöversikt</span><span class="sxs-lookup"><span data-stu-id="3ced3-157">Installation overview</span></span>

<span data-ttu-id="3ced3-158">Övervaka Azure SQL Database flottan är enkelt med logganalys.</span><span class="sxs-lookup"><span data-stu-id="3ced3-158">Monitoring Azure SQL Database fleet is simple with Log Analytics.</span></span> <span data-ttu-id="3ced3-159">Tre steg krävs:</span><span class="sxs-lookup"><span data-stu-id="3ced3-159">Three steps are required:</span></span>

1.  <span data-ttu-id="3ced3-160">Skapa resurs för logganalys</span><span class="sxs-lookup"><span data-stu-id="3ced3-160">Create Log Analytics resource</span></span>
2.  <span data-ttu-id="3ced3-161">Konfigurera databaserna toorecord mått och diagnostikloggar i hello skapade logganalys</span><span class="sxs-lookup"><span data-stu-id="3ced3-161">Configure databases toorecord metrics and diagnostic logs into hello created Log Analytics</span></span>
3.  <span data-ttu-id="3ced3-162">Installera **Azure SQL Analytics** lösningar från galleriet i logganalys</span><span class="sxs-lookup"><span data-stu-id="3ced3-162">Install **Azure SQL Analytics** solution from gallery in Log Analytics</span></span>

### <a name="create-log-analytics-resource"></a><span data-ttu-id="3ced3-163">Skapa resurs för logganalys</span><span class="sxs-lookup"><span data-stu-id="3ced3-163">Create Log Analytics resource</span></span>

1. <span data-ttu-id="3ced3-164">Klicka på **ny** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="3ced3-164">Click **New** in hello left-hand menu.</span></span>
2. <span data-ttu-id="3ced3-165">Klicka på **övervakning och hantering**</span><span class="sxs-lookup"><span data-stu-id="3ced3-165">Click **Monitoring + Management**</span></span>
3. <span data-ttu-id="3ced3-166">Klicka på **logga Analytics**</span><span class="sxs-lookup"><span data-stu-id="3ced3-166">Click **Log Analytics**</span></span>
4. <span data-ttu-id="3ced3-167">Fyll i hello logganalys form med hello ytterligare information som krävs: arbetsytans namn, prenumeration, resursgrupp, plats och prisnivån.</span><span class="sxs-lookup"><span data-stu-id="3ced3-167">Fill in hello Log Analytics form with hello additional information required: workspace name, subscription, resource group, location, and pricing tier.</span></span>

   ![logganalys](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-toorecord-metrics-and-diagnostic-logs"></a><span data-ttu-id="3ced3-169">Konfigurera databaserna toorecord mått och diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="3ced3-169">Configure databases toorecord metrics and diagnostic logs</span></span>

<span data-ttu-id="3ced3-170">hello enklaste sättet tooconfigure där databaserna registrera sina mått är via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3ced3-170">hello easiest way tooconfigure where databases record their metrics is through hello Azure portal.</span></span> <span data-ttu-id="3ced3-171">I hello Azure-portalen, navigera tooyour Azure SQL Database-resursen och klickar på **diagnostikinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="3ced3-171">In hello Azure portal, navigate tooyour Azure SQL Database resource and click **Diagnostics settings**.</span></span> 

### <a name="install-hello-azure-sql-analytics-solution-from-gallery"></a><span data-ttu-id="3ced3-172">Installera hello Azure SQL Analytics lösning från galleriet</span><span class="sxs-lookup"><span data-stu-id="3ced3-172">Install hello Azure SQL Analytics solution from gallery</span></span>  

1. <span data-ttu-id="3ced3-173">Installera Azure SQL Analytics lösning när hello logganalys resursen skapas och dina data flödar till den.</span><span class="sxs-lookup"><span data-stu-id="3ced3-173">Once hello Log Analytics resource is created and your data is flowing into it, install Azure SQL Analytics solution.</span></span> <span data-ttu-id="3ced3-174">Detta kan göras via hello **lösningar galleriet** som du hittar på hello OMS startsida och hello sida-menyn.</span><span class="sxs-lookup"><span data-stu-id="3ced3-174">This can be done through hello **Solutions Gallery** that you can find on hello OMS homepage and in hello side menu.</span></span> <span data-ttu-id="3ced3-175">Hello Gallery och hitta och klickar på **Azure SQL Analytics** lösningen och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3ced3-175">In hello gallery, find and click **Azure SQL Analytics** solution and click **Add**.</span></span>

   ![övervakningslösning](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. <span data-ttu-id="3ced3-177">På startsidan OMS kallas en ny panel **Azure SQL Analytics** visas.</span><span class="sxs-lookup"><span data-stu-id="3ced3-177">On your OMS homepage, a new tile called **Azure SQL Analytics** appears.</span></span> <span data-ttu-id="3ced3-178">Om du markerar den här panelen öppnas hello Azure SQL instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3ced3-178">Selecting this tile opens hello Azure SQL Analytics dashboard.</span></span>

### <a name="using-azure-sql-analytics-solution"></a><span data-ttu-id="3ced3-179">Med Azure SQL Analytics-lösning</span><span class="sxs-lookup"><span data-stu-id="3ced3-179">Using Azure SQL Analytics Solution</span></span>

<span data-ttu-id="3ced3-180">Azure SQL-Analytics är en hierarkisk instrumentpanel som gör att du toonavigate via hello hierarkin för Azure SQL Database-resurser.</span><span class="sxs-lookup"><span data-stu-id="3ced3-180">Azure SQL Analytics is a hierarchical dashboard that allows you toonavigate through hello hierarchy of Azure SQL Database resources.</span></span> <span data-ttu-id="3ced3-181">Den här funktionen möjliggör du toodo övergripande övervakning men också kan du tooscope din övervakning toojust hello rätt uppsättning resurser.</span><span class="sxs-lookup"><span data-stu-id="3ced3-181">This capability enables you toodo high-level monitoring but it also enables you tooscope your monitoring toojust hello right set of resources.</span></span>
<span data-ttu-id="3ced3-182">Instrumentpanelen innehåller hello listor över olika resurser under hello markerad resurs.</span><span class="sxs-lookup"><span data-stu-id="3ced3-182">Dashboard contains hello lists of different resources under hello selected resource.</span></span> <span data-ttu-id="3ced3-183">För en vald prenumeration kan du till exempel se hello servrar, elastiska pooler och databaser som tillhör toohello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="3ced3-183">For example, for a selected subscription you can see hello all servers, elastic pools and databases that belong toohello selected subscription.</span></span> <span data-ttu-id="3ced3-184">Du kan dessutom se hello resurs användningsstatistik för resursen för elastiska pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="3ced3-184">Additionally, for Elastic Pools and databases, you can see hello resource usage metrics of that resource.</span></span> <span data-ttu-id="3ced3-185">Detta inkluderar diagram för DTU, CPU, IO, LOG, sessioner, arbetare, anslutningar och lagringsutrymme i GB.</span><span class="sxs-lookup"><span data-stu-id="3ced3-185">This includes charts for DTU, CPU, IO, LOG, sessions, workers, connections, and storage in GB.</span></span>

## <a name="stream-into-azure-event-hub"></a><span data-ttu-id="3ced3-186">Dataströmmen till Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="3ced3-186">Stream into Azure Event Hub</span></span>

<span data-ttu-id="3ced3-187">Azure SQL Database-mätvärden och diagnostiska loggar strömmas till Event Hub alternativet hello inbyggda ”dataströmmen tooan event hub” hello-portalen eller genom att aktivera Service Bus regel-Id i diagnostikinställningen via Azure PowerShell-Cmdlets, Azure CLI eller Azure-Monitor REST API.</span><span class="sxs-lookup"><span data-stu-id="3ced3-187">Azure SQL Database metrics and diagnostic logs can be streamed into Event Hub using hello built-in “Stream tooan event hub” option in hello portal, or by enabling Service Bus Rule Id in a diagnostic setting via Azure PowerShell Cmdlets, Azure CLI, or Azure Monitor REST API.</span></span> 

### <a name="what-toodo-with-metrics-and-diagnostic-logs-in-event-hub"></a><span data-ttu-id="3ced3-188">Vilka toodo med mått och diagnostikloggar i Event Hub?</span><span class="sxs-lookup"><span data-stu-id="3ced3-188">What toodo with metrics and diagnostic logs in Event Hub?</span></span>
<span data-ttu-id="3ced3-189">När hello valda data strömmas till Event Hub, är en steg-närmare tooenabling avancerade scenarier för övervakning.</span><span class="sxs-lookup"><span data-stu-id="3ced3-189">Once hello selected data is streamed into Event Hub, you are one step closer tooenabling advanced monitoring scenarios.</span></span> <span data-ttu-id="3ced3-190">Händelsehubbar fungerar som hello ”ytterdörren” för en händelsepipeline, och när data har samlats in i en Händelsehubb, det kan omvandlas och lagras med hjälp av en leverantör av realtidsanalys eller adaptrar för batchbearbetning/lagring.</span><span class="sxs-lookup"><span data-stu-id="3ced3-190">Event Hubs acts as hello "front door" for an event pipeline, and once data is collected into an Event Hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="3ced3-191">Händelsehubbar frikopplar hello framställning av en dataström med händelser från hello användningen av dessa händelser så att händelsekonsumenterna kan komma åt hello händelser på sitt eget schema.</span><span class="sxs-lookup"><span data-stu-id="3ced3-191">Event Hubs decouples hello production of a stream of events from hello consumption of those events, so that event consumers can access hello events on their own schedule.</span></span> <span data-ttu-id="3ced3-192">För mer information om Event Hub, se:</span><span class="sxs-lookup"><span data-stu-id="3ced3-192">For more information on Event Hub, see:</span></span>

- <span data-ttu-id="3ced3-193">[Vad är Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span><span class="sxs-lookup"><span data-stu-id="3ced3-193">[What are Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span></span>
- [<span data-ttu-id="3ced3-194">Kom igång med Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3ced3-194">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


<span data-ttu-id="3ced3-195">Här följer några olika sätt som du kan använda hello strömning kapaciteten:</span><span class="sxs-lookup"><span data-stu-id="3ced3-195">Here are just a few ways you might use hello streaming capability:</span></span>

-   <span data-ttu-id="3ced3-196">Visa tjänstens hälsa av strömning ”varm path” data tooPowerBI - med Händelsehubbar, Stream Analytics och PowerBI, kan du enkelt omvandla mätvärden och diagnostikfunktionerna data i nära realtidsinsikter på Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3ced3-196">View service health by streaming “hot path” data tooPowerBI - Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your metrics and diagnostics data into near real-time insights on your Azure services.</span></span> <span data-ttu-id="3ced3-197">En översikt över hur tooset upp en Händelsehubbar bearbeta data med Stream Analytics och använda PowerBI som utdata, se [Stream Analytics och Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="3ced3-197">For an overview of how tooset up an Event Hubs, process data with Stream Analytics, and use PowerBI as an output, see [Stream Analytics and Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span>
-   <span data-ttu-id="3ced3-198">Dataströmmen loggar toothird parts loggning och telemetri dataströmmar – med Händelsehubbar direktuppspelning av du kan hämta din mått och diagnostikloggar i toodifferent övervaknings- och log analytics tredjepartslösningar.</span><span class="sxs-lookup"><span data-stu-id="3ced3-198">Stream logs toothird-party logging and telemetry streams – Using Event Hubs streaming you can get your metrics and diagnostic logs in toodifferent third-party monitoring and log analytics solutions.</span></span> 
-   <span data-ttu-id="3ced3-199">Skapa en anpassad telemetri och loggning plattform – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar hello publicera och prenumerera uppbyggnad Händelsehubbar kan du tooflexibly mata in diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="3ced3-199">Build a custom telemetry and logging platform – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest diagnostic logs.</span></span> <span data-ttu-id="3ced3-200">Se [Dan Rosanova guide toousing Händelsehubbar i en global skala telemetri platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="3ced3-200">See [Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="stream-into-azure-storage"></a><span data-ttu-id="3ced3-201">Dataströmmen till Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3ced3-201">Stream into Azure Storage</span></span>

<span data-ttu-id="3ced3-202">Azure SQL Database-mätvärden och diagnostikloggar kan lagras i Azure Storage alternativet hello inbyggda ”Arkivera tooa storage-konto” i hello Azure-portalen eller genom att aktivera Azure Storage i diagnostikinställningen via Azure PowerShell-Cmdlets, Azure CLI eller Azure Övervakaren REST API.</span><span class="sxs-lookup"><span data-stu-id="3ced3-202">Azure SQL Database metrics and diagnostic logs can be stored into Azure Storage using hello built-in "Archive tooa storage account” option in hello Azure portal, or by enabling Azure Storage in a diagnostic setting via Azure PowerShell Cmdlets, Azure CLI, or Azure Monitor REST API.</span></span>

### <a name="schema-of-metrics-and-diagnostic-logs-in-hello-storage-account"></a><span data-ttu-id="3ced3-203">Schemat för mått och diagnostikloggar i hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="3ced3-203">Schema of metrics and diagnostic logs in hello storage account</span></span>

<span data-ttu-id="3ced3-204">När du har konfigurerat mått och diagnostikloggar samling, skapas en lagringsbehållare i hello storage-konto som du valde när hello första datarader är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="3ced3-204">Once you have set up metrics and diagnostic logs collection, a storage container is created in hello storage account you selected when hello first rows of data are available.</span></span> <span data-ttu-id="3ced3-205">hello strukturen för de här blobbar är:</span><span class="sxs-lookup"><span data-stu-id="3ced3-205">hello structure of these blobs is:</span></span>

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
<span data-ttu-id="3ced3-206">Eller att:</span><span class="sxs-lookup"><span data-stu-id="3ced3-206">Or, more simply:</span></span>

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

<span data-ttu-id="3ced3-207">Till exempel kanske en blob-namnet för 1 minut mått:</span><span class="sxs-lookup"><span data-stu-id="3ced3-207">For example, a blob name for 1-minute metrics might be:</span></span>

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

<span data-ttu-id="3ced3-208">Om du vill toorecord hello data från hello elastisk Pool, är blob-namnet något annat:</span><span class="sxs-lookup"><span data-stu-id="3ced3-208">In case you want toorecord hello data from hello Elastic Pool, blob name is a bit different:</span></span>

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-azure-storage"></a><span data-ttu-id="3ced3-209">Hämta mätvärden och loggar från Azure storage</span><span class="sxs-lookup"><span data-stu-id="3ced3-209">Download metrics and logs from Azure storage</span></span>

<span data-ttu-id="3ced3-210">Se [hämta mätvärden och diagnostiska loggar från Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span><span class="sxs-lookup"><span data-stu-id="3ced3-210">See [Download metrics and diagnostic logs from Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span></span>

## <a name="1-minute-metrics"></a><span data-ttu-id="3ced3-211">1 minut mått</span><span class="sxs-lookup"><span data-stu-id="3ced3-211">1-minute metrics</span></span>

| |  |
|---|---|
|<span data-ttu-id="3ced3-212">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="3ced3-212">**Resource**</span></span>|<span data-ttu-id="3ced3-213">**Mått**</span><span class="sxs-lookup"><span data-stu-id="3ced3-213">**Metrics**</span></span>|
|<span data-ttu-id="3ced3-214">Databas</span><span class="sxs-lookup"><span data-stu-id="3ced3-214">Database</span></span>|<span data-ttu-id="3ced3-215">DTU-procent DTU används, DTU gränsen, CPU-procent, fysiska data skrivskyddade procent loggen skriva procent, lyckade/misslyckade/Blocked av brandväggens anslutningar, sessioner procent, arbetare procent, lagring, lagringsprocent, XTP lagringsprocent deadlocks</span><span class="sxs-lookup"><span data-stu-id="3ced3-215">DTU percentage, DTU used, DTU limit, CPU percentage, Physical data read percentage, Log write percentage, Successful/Failed/Blocked by firewall connections, sessions percentage, workers percentage, storage, storage percentage, XTP storage percentage, deadlocks</span></span> |
|<span data-ttu-id="3ced3-216">Elastisk pool</span><span class="sxs-lookup"><span data-stu-id="3ced3-216">Elastic pool</span></span>|<span data-ttu-id="3ced3-217">eDTU procentandel eDTU används, eDTU gränsen, CPU-procent, fysiska data skrivskyddade procent, skriva loggen procent, sessioner procent, arbetare procent, lagring, lagringsprocent, lagringsgräns, XTP-lagringsprocent</span><span class="sxs-lookup"><span data-stu-id="3ced3-217">eDTU percentage, eDTU used, eDTU limit, CPU percentage, Physical data read percentage, Log write percentage, sessions percentage, workers percentage, storage, storage percentage, storage limit, XTP storage percentage</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="3ced3-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ced3-218">Next steps</span></span>

- <span data-ttu-id="3ced3-219">Läsa båda hello [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) och [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artiklar toogain förstå inte bara hur tooenable loggning, men hello mått och logga kategorier stöds av hello olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3ced3-219">Read both hello [Overview of metrics in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) and [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) articles toogain an understanding of not only how tooenable logging, but hello metrics and log categories supported by hello various Azure services.</span></span>
- <span data-ttu-id="3ced3-220">Läs dessa artiklar toolearn om händelsehubbar:</span><span class="sxs-lookup"><span data-stu-id="3ced3-220">Read these articles toolearn about event hubs:</span></span>
   - <span data-ttu-id="3ced3-221">[Vad är Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span><span class="sxs-lookup"><span data-stu-id="3ced3-221">[What are Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span></span>
   - [<span data-ttu-id="3ced3-222">Kom igång med Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3ced3-222">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- <span data-ttu-id="3ced3-223">Se [hämta mätvärden och diagnostiska loggar från Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span><span class="sxs-lookup"><span data-stu-id="3ced3-223">See [Download metrics and diagnostic logs from Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span></span>
