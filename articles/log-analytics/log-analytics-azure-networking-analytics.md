---
title: "Azure nätverk Analytics lösning i Log Analytics | Microsoft Docs"
description: "Du kan använda Azure-nätverk Analytics-lösning i logganalys för att granska grupp säkerhetsloggar av Azure-nätverk och Azure Programgateway loggar."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 06b67322b3812a668a515ecc357171ede1d85441
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a><span data-ttu-id="bf14c-103">Azure nätverk övervakning lösningar i logganalys</span><span class="sxs-lookup"><span data-stu-id="bf14c-103">Azure networking monitoring solutions in Log Analytics</span></span>

<span data-ttu-id="bf14c-104">Logganalys erbjuder följande lösningar för att övervaka dina nätverk:</span><span class="sxs-lookup"><span data-stu-id="bf14c-104">Log Analytics offers the following solutions for monitoring your networks:</span></span>
* <span data-ttu-id="bf14c-105">Network Performance Monitor (NPM) till</span><span class="sxs-lookup"><span data-stu-id="bf14c-105">Network Performance Monitor (NPM) to</span></span>
 * <span data-ttu-id="bf14c-106">Övervaka hälsotillståndet hos ditt nätverk</span><span class="sxs-lookup"><span data-stu-id="bf14c-106">Monitor the health of your network</span></span>
* <span data-ttu-id="bf14c-107">Azure Application Gateway analyser för att granska</span><span class="sxs-lookup"><span data-stu-id="bf14c-107">Azure Application Gateway analytics to review</span></span>
 * <span data-ttu-id="bf14c-108">Azure Application Gateway-loggar</span><span class="sxs-lookup"><span data-stu-id="bf14c-108">Azure Application Gateway logs</span></span>
 * <span data-ttu-id="bf14c-109">Azure Application Gateway-mått</span><span class="sxs-lookup"><span data-stu-id="bf14c-109">Azure Application Gateway metrics</span></span>
* <span data-ttu-id="bf14c-110">Azure Network Security Group analyser för att granska</span><span class="sxs-lookup"><span data-stu-id="bf14c-110">Azure Network Security Group analytics to review</span></span>
 * <span data-ttu-id="bf14c-111">Nätverkssäkerhetsgruppen för Azure-loggar</span><span class="sxs-lookup"><span data-stu-id="bf14c-111">Azure Network Security Group logs</span></span>

## <a name="network-performance-monitor-npm"></a><span data-ttu-id="bf14c-112">Network Performance Monitor (NPM)</span><span class="sxs-lookup"><span data-stu-id="bf14c-112">Network Performance Monitor (NPM)</span></span>

<span data-ttu-id="bf14c-113">Den [Network Performance Monitor](log-analytics-network-performance-monitor.md) hanteringslösning är ett nätverk övervakningslösning som övervakar hälsa, tillgänglighet och tillgänglighet av nätverk.</span><span class="sxs-lookup"><span data-stu-id="bf14c-113">The [Network Performance Monitor](log-analytics-network-performance-monitor.md) management solution is a network monitoring solution, that monitors the health, availability and reachability of networks.</span></span>  <span data-ttu-id="bf14c-114">Används för att övervaka anslutningen mellan:</span><span class="sxs-lookup"><span data-stu-id="bf14c-114">It is used to monitor connectivity between:</span></span>

* <span data-ttu-id="bf14c-115">offentliga molnet och lokalt</span><span class="sxs-lookup"><span data-stu-id="bf14c-115">Public cloud and on-premises</span></span>
* <span data-ttu-id="bf14c-116">datacenter och användarplatser (avdelningskontor)</span><span class="sxs-lookup"><span data-stu-id="bf14c-116">Data centers and user locations (branch offices)</span></span>
* <span data-ttu-id="bf14c-117">undernät som är värd för olika nivåer av ett program med flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="bf14c-117">Subnets hosting various tiers of a multi-tiered application.</span></span>

<span data-ttu-id="bf14c-118">Mer information finns i [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="bf14c-118">For more information, see [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span></span>

## <a name="azure-application-gateway-and-network-security-group-analytics"></a><span data-ttu-id="bf14c-119">Azure Application Gateway och Nätverkssäkerhetsgruppen analytics</span><span class="sxs-lookup"><span data-stu-id="bf14c-119">Azure Application Gateway and Network Security Group analytics</span></span>
<span data-ttu-id="bf14c-120">Använda lösningarna:</span><span class="sxs-lookup"><span data-stu-id="bf14c-120">To use the solutions:</span></span>
1. <span data-ttu-id="bf14c-121">Lägg till hanteringslösningen Log Analytics och</span><span class="sxs-lookup"><span data-stu-id="bf14c-121">Add the management solution to Log Analytics, and</span></span>
2. <span data-ttu-id="bf14c-122">Aktivera diagnostik för att dirigera diagnostik till logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="bf14c-122">Enable diagnostics to direct the diagnostics to a Log Analytics workspace.</span></span> <span data-ttu-id="bf14c-123">Du behöver inte skriva loggarna till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bf14c-123">It is not necessary to write the logs to Azure Blob storage.</span></span>

<span data-ttu-id="bf14c-124">Du kan aktivera diagnostik- och motsvarande lösningen för en eller båda av Programgateway och nätverk säkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="bf14c-124">You can enable diagnostics and the corresponding solution for either one or both of Application Gateway and Networking Security Groups.</span></span>

<span data-ttu-id="bf14c-125">Om du inte aktivera diagnostikloggning för en viss resurstyp, men installerar lösningen, instrumentpanelen blad för den här resursen är tomma och ett felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="bf14c-125">If you do not enable diagnostic logging for a particular resource type, but install the solution, the dashboard blades for that resource are blank and display an error message.</span></span>

> [!NOTE]
> <span data-ttu-id="bf14c-126">Sättet att skicka loggar från Programgatewayer och Nätverkssäkerhetsgrupper till logganalys ändras i januari 2017.</span><span class="sxs-lookup"><span data-stu-id="bf14c-126">In January 2017, the supported way of sending logs from Application Gateways and Network Security Groups to Log Analytics changed.</span></span> <span data-ttu-id="bf14c-127">Om du ser den **Azure-nätverk Analytics (föråldrad)** lösning, referera till [migrera från den gamla nätverk Analytics lösningen](#migrating-from-the-old-networking-analytics-solution) anvisningar för hur du ska följa.</span><span class="sxs-lookup"><span data-stu-id="bf14c-127">If you see the **Azure Networking Analytics (deprecated)** solution, refer to [migrating from the old Networking Analytics solution](#migrating-from-the-old-networking-analytics-solution) for steps you need to follow.</span></span>
>
>

## <a name="review-azure-networking-data-collection-details"></a><span data-ttu-id="bf14c-128">Granska Azure nätverk information för samlingen</span><span class="sxs-lookup"><span data-stu-id="bf14c-128">Review Azure networking data collection details</span></span>
<span data-ttu-id="bf14c-129">Samla in diagnostik loggarna direkt från Azure Programgatewayer och Nätverkssäkerhetsgrupper Azure Programgateway analyser och Nätverkssäkerhetsgruppen management Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="bf14c-129">The Azure Application Gateway analytics and the Network Security Group analytics management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups.</span></span> <span data-ttu-id="bf14c-130">Det är inte nödvändigt att skriva loggarna till Azure Blob storage och ingen agent krävs för datainsamling.</span><span class="sxs-lookup"><span data-stu-id="bf14c-130">It is not necessary to write the logs to Azure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="bf14c-131">I följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Azure Programgateway analyser och Nätverkssäkerhetsgruppen analytics.</span><span class="sxs-lookup"><span data-stu-id="bf14c-131">The following table shows data collection methods and other details about how data is collected for Azure Application Gateway analytics and the Network Security Group analytics.</span></span>

| <span data-ttu-id="bf14c-132">Plattform</span><span class="sxs-lookup"><span data-stu-id="bf14c-132">Platform</span></span> | <span data-ttu-id="bf14c-133">Styr agent</span><span class="sxs-lookup"><span data-stu-id="bf14c-133">Direct agent</span></span> | <span data-ttu-id="bf14c-134">System Center Operations Manager-agenten</span><span class="sxs-lookup"><span data-stu-id="bf14c-134">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="bf14c-135">Azure</span><span class="sxs-lookup"><span data-stu-id="bf14c-135">Azure</span></span> | <span data-ttu-id="bf14c-136">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="bf14c-136">Operations Manager required?</span></span> | <span data-ttu-id="bf14c-137">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="bf14c-137">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="bf14c-138">Insamlingsfrekvens</span><span class="sxs-lookup"><span data-stu-id="bf14c-138">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bf14c-139">Azure</span><span class="sxs-lookup"><span data-stu-id="bf14c-139">Azure</span></span> |  |  |<span data-ttu-id="bf14c-140">&#8226;</span><span class="sxs-lookup"><span data-stu-id="bf14c-140">&#8226;</span></span> |  |  |<span data-ttu-id="bf14c-141">När du har loggat</span><span class="sxs-lookup"><span data-stu-id="bf14c-141">when logged</span></span> |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a><span data-ttu-id="bf14c-142">Azure Application Gateway analytics lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="bf14c-142">Azure Application Gateway analytics solution in Log Analytics</span></span>

![Azure Application Gateway Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="bf14c-144">Följande loggar stöds för Programgatewayer:</span><span class="sxs-lookup"><span data-stu-id="bf14c-144">The following logs are supported for Application Gateways:</span></span>

* <span data-ttu-id="bf14c-145">ApplicationGatewayAccessLog</span><span class="sxs-lookup"><span data-stu-id="bf14c-145">ApplicationGatewayAccessLog</span></span>
* <span data-ttu-id="bf14c-146">ApplicationGatewayPerformanceLog</span><span class="sxs-lookup"><span data-stu-id="bf14c-146">ApplicationGatewayPerformanceLog</span></span>
* <span data-ttu-id="bf14c-147">ApplicationGatewayFirewallLog</span><span class="sxs-lookup"><span data-stu-id="bf14c-147">ApplicationGatewayFirewallLog</span></span>

<span data-ttu-id="bf14c-148">Följande mått stöds för Programgatewayer:</span><span class="sxs-lookup"><span data-stu-id="bf14c-148">The following metrics are supported for Application Gateways:</span></span>

* <span data-ttu-id="bf14c-149">5 minut genomflöde</span><span class="sxs-lookup"><span data-stu-id="bf14c-149">5 minute throughput</span></span>

### <a name="install-and-configure-the-solution"></a><span data-ttu-id="bf14c-150">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="bf14c-150">Install and configure the solution</span></span>
<span data-ttu-id="bf14c-151">Använd följande instruktioner för att installera och konfigurera Azure Programgateway analytics lösningen:</span><span class="sxs-lookup"><span data-stu-id="bf14c-151">Use the following instructions to install and configure the Azure Application Gateway analytics solution:</span></span>

1. <span data-ttu-id="bf14c-152">Aktivera Azure Programgateway analytics-lösning från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) eller genom att använda processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="bf14c-152">Enable the Azure Application Gateway analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="bf14c-153">Aktivera diagnostik loggning för den [Programgatewayer](../application-gateway/application-gateway-diagnostics.md) du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="bf14c-153">Enable diagnostics logging for the [Application Gateways](../application-gateway/application-gateway-diagnostics.md) you want to monitor.</span></span>

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a><span data-ttu-id="bf14c-154">Aktivera Azure Programgateway diagnostik i portalen</span><span class="sxs-lookup"><span data-stu-id="bf14c-154">Enable Azure Application Gateway diagnostics in the portal</span></span>

1. <span data-ttu-id="bf14c-155">Navigera till resursen Programgateway att övervaka i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bf14c-155">In the Azure portal, navigate to the Application Gateway resource to monitor</span></span>
2. <span data-ttu-id="bf14c-156">Välj *diagnostik loggar* att öppna följande sida</span><span class="sxs-lookup"><span data-stu-id="bf14c-156">Select *Diagnostics logs* to open the following page</span></span>

   ![Bild av Azure Programgateway resurs](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. <span data-ttu-id="bf14c-158">Klicka på *aktivera diagnostiken* att öppna följande sida</span><span class="sxs-lookup"><span data-stu-id="bf14c-158">Click *Turn on diagnostics* to open the following page</span></span>

   ![Bild av Azure Programgateway resurs](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. <span data-ttu-id="bf14c-160">Aktivera diagnostik, klicka på *på* under *Status*</span><span class="sxs-lookup"><span data-stu-id="bf14c-160">To turn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="bf14c-161">Klicka på kryssrutan för *skicka till logganalys*</span><span class="sxs-lookup"><span data-stu-id="bf14c-161">Click the checkbox for *Send to Log Analytics*</span></span>
6. <span data-ttu-id="bf14c-162">Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="bf14c-162">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="bf14c-163">Klickar du på kryssrutan under **loggen** för varje logg att samla in</span><span class="sxs-lookup"><span data-stu-id="bf14c-163">Click the checkbox under **Log** for each of the log types to collect</span></span>
8. <span data-ttu-id="bf14c-164">Klicka på *spara* att aktivera loggning av diagnostik till logganalys</span><span class="sxs-lookup"><span data-stu-id="bf14c-164">Click *Save* to enable the logging of diagnostics to Log Analytics</span></span>

#### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="bf14c-165">Aktivera Azure Nätverksdiagnostik med PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf14c-165">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="bf14c-166">Följande PowerShell-skript innehåller ett exempel på hur du aktiverar loggning för programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="bf14c-166">The following PowerShell script provides an example of how to enable diagnostic logging for application gateways.</span></span>

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a><span data-ttu-id="bf14c-167">Använd Azure Programgateway analytics</span><span class="sxs-lookup"><span data-stu-id="bf14c-167">Use Azure Application Gateway analytics</span></span>
![Bild av Azure Programgateway analytics panelen](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

<span data-ttu-id="bf14c-169">När du klickar på den **Azure Programgateway analytics** panelen på Översikt, kan du visa sammanfattningar av loggar och visa sedan detaljnivåerna till information i följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="bf14c-169">After you click the **Azure Application Gateway analytics** tile on the Overview, you can view summaries of your logs and then drill in to details for the following categories:</span></span>

* <span data-ttu-id="bf14c-170">Programåtkomst för Gateway-loggar</span><span class="sxs-lookup"><span data-stu-id="bf14c-170">Application Gateway Access logs</span></span>
  * <span data-ttu-id="bf14c-171">Klient- och fel för åtkomstloggar för Programgateway</span><span class="sxs-lookup"><span data-stu-id="bf14c-171">Client and server errors for Application Gateway access logs</span></span>
  * <span data-ttu-id="bf14c-172">Förfrågningar per timme för varje Programgateway</span><span class="sxs-lookup"><span data-stu-id="bf14c-172">Requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="bf14c-173">Misslyckade förfrågningar per timme för varje Programgateway</span><span class="sxs-lookup"><span data-stu-id="bf14c-173">Failed requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="bf14c-174">Fel per användaragent för Programgatewayer</span><span class="sxs-lookup"><span data-stu-id="bf14c-174">Errors by user agent for Application Gateways</span></span>
* <span data-ttu-id="bf14c-175">Programprestanda för Gateway</span><span class="sxs-lookup"><span data-stu-id="bf14c-175">Application Gateway performance</span></span>
  * <span data-ttu-id="bf14c-176">Värden hälsa för Programgateway</span><span class="sxs-lookup"><span data-stu-id="bf14c-176">Host health for Application Gateway</span></span>
  * <span data-ttu-id="bf14c-177">Maximal och 95 percentil för Programgateway misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="bf14c-177">Maximum and 95th percentile for Application Gateway failed requests</span></span>

![Bild av instrumentpanelen för Azure Programgateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Bild av instrumentpanelen för Azure Programgateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

<span data-ttu-id="bf14c-180">På den **Azure Programgateway analytics** instrumentpanel, Granska sammanfattningen i ett av bladen och klicka sedan på en om du vill visa detaljerad information på sidan logga.</span><span class="sxs-lookup"><span data-stu-id="bf14c-180">On the **Azure Application Gateway analytics** dashboard, review the summary information in one of the blades, and then click one to view detailed information on the log search page.</span></span>

<span data-ttu-id="bf14c-181">Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av sidorna loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="bf14c-181">On any of the log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="bf14c-182">Du kan också filtrera efter aspekter att begränsa resultaten.</span><span class="sxs-lookup"><span data-stu-id="bf14c-182">You can also filter by facets to narrow the results.</span></span>


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a><span data-ttu-id="bf14c-183">Azure Network Security Group analytics lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="bf14c-183">Azure Network Security Group analytics solution in Log Analytics</span></span>

![Azure Network Security Group Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="bf14c-185">Följande loggar stöds för nätverkssäkerhetsgrupper:</span><span class="sxs-lookup"><span data-stu-id="bf14c-185">The following logs are supported for network security groups:</span></span>

* <span data-ttu-id="bf14c-186">NetworkSecurityGroupEvent</span><span class="sxs-lookup"><span data-stu-id="bf14c-186">NetworkSecurityGroupEvent</span></span>
* <span data-ttu-id="bf14c-187">NetworkSecurityGroupRuleCounter</span><span class="sxs-lookup"><span data-stu-id="bf14c-187">NetworkSecurityGroupRuleCounter</span></span>

### <a name="install-and-configure-the-solution"></a><span data-ttu-id="bf14c-188">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="bf14c-188">Install and configure the solution</span></span>
<span data-ttu-id="bf14c-189">Använd följande instruktioner för att installera och konfigurera Azure-nätverk Analytics-lösningen:</span><span class="sxs-lookup"><span data-stu-id="bf14c-189">Use the following instructions to install and configure the Azure Networking Analytics solution:</span></span>

1. <span data-ttu-id="bf14c-190">Aktivera Azure Network Security Group analytics-lösning från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) eller genom att använda processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="bf14c-190">Enable the Azure Network Security Group analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="bf14c-191">Aktivera diagnostik loggning för den [Nätverkssäkerhetsgruppen](../virtual-network/virtual-network-nsg-manage-log.md) resurser som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="bf14c-191">Enable diagnostics logging for the [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md) resources you want to monitor.</span></span>

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a><span data-ttu-id="bf14c-192">Aktivera diagnostik av gruppen för Azure-nätverk i portalen</span><span class="sxs-lookup"><span data-stu-id="bf14c-192">Enable Azure network security group diagnostics in the portal</span></span>

1. <span data-ttu-id="bf14c-193">Navigera till Nätverkssäkerhetsgruppen resursen ska övervaka i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bf14c-193">In the Azure portal, navigate to the Network Security Group resource to monitor</span></span>
2. <span data-ttu-id="bf14c-194">Välj *diagnostik loggar* att öppna följande sida</span><span class="sxs-lookup"><span data-stu-id="bf14c-194">Select *Diagnostics logs* to open the following page</span></span>

   ![Bild av Azure Network Security Group resurs](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. <span data-ttu-id="bf14c-196">Klicka på *aktivera diagnostiken* att öppna följande sida</span><span class="sxs-lookup"><span data-stu-id="bf14c-196">Click *Turn on diagnostics* to open the following page</span></span>

   ![Bild av Azure Network Security Group resurs](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. <span data-ttu-id="bf14c-198">Aktivera diagnostik, klicka på *på* under *Status*</span><span class="sxs-lookup"><span data-stu-id="bf14c-198">To turn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="bf14c-199">Klicka på kryssrutan för *skicka till logganalys*</span><span class="sxs-lookup"><span data-stu-id="bf14c-199">Click the checkbox for *Send to Log Analytics*</span></span>
6. <span data-ttu-id="bf14c-200">Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="bf14c-200">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="bf14c-201">Klickar du på kryssrutan under **loggen** för varje logg att samla in</span><span class="sxs-lookup"><span data-stu-id="bf14c-201">Click the checkbox under **Log** for each of the log types to collect</span></span>
8. <span data-ttu-id="bf14c-202">Klicka på *spara* att aktivera loggning av diagnostik till logganalys</span><span class="sxs-lookup"><span data-stu-id="bf14c-202">Click *Save* to enable the logging of diagnostics to Log Analytics</span></span>

### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="bf14c-203">Aktivera Azure Nätverksdiagnostik med PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf14c-203">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="bf14c-204">Följande PowerShell-skript innehåller ett exempel på hur du aktiverar loggning för nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="bf14c-204">The following PowerShell script provides an example of how to enable diagnostic logging for network security groups</span></span>
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a><span data-ttu-id="bf14c-205">Använd Azure Network Security Group analytics</span><span class="sxs-lookup"><span data-stu-id="bf14c-205">Use Azure Network Security Group analytics</span></span>
<span data-ttu-id="bf14c-206">När du klickar på den **Azure Network Security Group analytics** panelen på Översikt, kan du visa sammanfattningar av loggar och visa sedan detaljnivåerna till information i följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="bf14c-206">After you click the **Azure Network Security Group analytics** tile on the Overview, you can view summaries of your logs and then drill in to details for the following categories:</span></span>

* <span data-ttu-id="bf14c-207">Nätverkssäkerhetsgruppen blockeras flöden</span><span class="sxs-lookup"><span data-stu-id="bf14c-207">Network security group blocked flows</span></span>
  * <span data-ttu-id="bf14c-208">Regler för nätverkssäkerhetsgrupper med blockerade flöden</span><span class="sxs-lookup"><span data-stu-id="bf14c-208">Network security group rules with blocked flows</span></span>
  * <span data-ttu-id="bf14c-209">MAC-adresser med blockerade flöden</span><span class="sxs-lookup"><span data-stu-id="bf14c-209">MAC addresses with blocked flows</span></span>
* <span data-ttu-id="bf14c-210">Nätverkssäkerhetsgruppen tillåtna flöden</span><span class="sxs-lookup"><span data-stu-id="bf14c-210">Network security group allowed flows</span></span>
  * <span data-ttu-id="bf14c-211">Regler för nätverkssäkerhetsgrupper med tillåtna flöden</span><span class="sxs-lookup"><span data-stu-id="bf14c-211">Network security group rules with allowed flows</span></span>
  * <span data-ttu-id="bf14c-212">MAC-adresser med tillåtna flöden</span><span class="sxs-lookup"><span data-stu-id="bf14c-212">MAC addresses with allowed flows</span></span>

![Bild av instrumentpanelen för Azure Nätverkssäkerhetsgrupp](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Bild av instrumentpanelen för Azure Nätverkssäkerhetsgrupp](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

<span data-ttu-id="bf14c-215">På den **Azure Network Security Group analytics** instrumentpanel, Granska sammanfattningen i ett av bladen och klicka sedan på en om du vill visa detaljerad information på sidan logga.</span><span class="sxs-lookup"><span data-stu-id="bf14c-215">On the **Azure Network Security Group analytics** dashboard, review the summary information in one of the blades, and then click one to view detailed information on the log search page.</span></span>

<span data-ttu-id="bf14c-216">Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av sidorna loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="bf14c-216">On any of the log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="bf14c-217">Du kan också filtrera efter aspekter att begränsa resultaten.</span><span class="sxs-lookup"><span data-stu-id="bf14c-217">You can also filter by facets to narrow the results.</span></span>

## <a name="migrating-from-the-old-networking-analytics-solution"></a><span data-ttu-id="bf14c-218">Migrera från den gamla nätverk Analytics-lösningen</span><span class="sxs-lookup"><span data-stu-id="bf14c-218">Migrating from the old Networking Analytics solution</span></span>
<span data-ttu-id="bf14c-219">Sättet att skicka loggar från Azure Programgatewayer och säkerhetsgrupper för Azure-nätverket till logganalys ändras i januari 2017.</span><span class="sxs-lookup"><span data-stu-id="bf14c-219">In January 2017, the supported way of sending logs from Azure Application Gateways and Azure Network Security Groups to Log Analytics changed.</span></span> <span data-ttu-id="bf14c-220">Ändringarna ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bf14c-220">These changes provide the following advantages:</span></span>
+ <span data-ttu-id="bf14c-221">Loggarna skrivs direkt till Log Analytics utan att behöva använda ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="bf14c-221">Logs are written directly to Log Analytics without the need to use a storage account</span></span>
+ <span data-ttu-id="bf14c-222">Mindre fördröjning från när loggar genereras till dem som finns i logganalys</span><span class="sxs-lookup"><span data-stu-id="bf14c-222">Less latency from the time when logs are generated to them being available in Log Analytics</span></span>
+ <span data-ttu-id="bf14c-223">Färre konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="bf14c-223">Fewer configuration steps</span></span>
+ <span data-ttu-id="bf14c-224">Ett vanligt format för alla typer av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="bf14c-224">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="bf14c-225">Använda de uppdaterade lösningarna:</span><span class="sxs-lookup"><span data-stu-id="bf14c-225">To use the updated solutions:</span></span>

1. [<span data-ttu-id="bf14c-226">Konfigurera diagnostik skickas direkt till Log Analytics från Azure Programgatewayer</span><span class="sxs-lookup"><span data-stu-id="bf14c-226">Configure diagnostics to be sent directly to Log Analytics from Azure Application Gateways</span></span>](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [<span data-ttu-id="bf14c-227">Konfigurera diagnostik skickas direkt till Log Analytics från Azure Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="bf14c-227">Configure diagnostics to be sent directly to Log Analytics from Azure Network Security Groups</span></span>](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. <span data-ttu-id="bf14c-228">Aktivera den *Azure Application Gateway Analytics* och *Azure Network Security Group Analytics* lösning med hjälp av den process som beskrivs i [lägga till logganalys lösningar från den Lösningar galleri](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="bf14c-228">Enable the *Azure Application Gateway Analytics* and the *Azure Network Security Group Analytics* solution by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="bf14c-229">Uppdatera alla sparade frågor, instrumentpaneler eller aviseringar för att använda den nya datatypen</span><span class="sxs-lookup"><span data-stu-id="bf14c-229">Update any saved queries, dashboards, or alerts to use the new data type</span></span>
  + <span data-ttu-id="bf14c-230">Typen är att AzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="bf14c-230">Type is to AzureDiagnostics.</span></span> <span data-ttu-id="bf14c-231">Du kan använda resurstypens för att filtrera till Azure-nätverk loggarna.</span><span class="sxs-lookup"><span data-stu-id="bf14c-231">You can use the ResourceType to filter to Azure networking logs.</span></span>

    | <span data-ttu-id="bf14c-232">Istället för:</span><span class="sxs-lookup"><span data-stu-id="bf14c-232">Instead of:</span></span> | <span data-ttu-id="bf14c-233">Användning:</span><span class="sxs-lookup"><span data-stu-id="bf14c-233">Use:</span></span> |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + <span data-ttu-id="bf14c-234">För alla fält som har suffixet \_s, \_d, eller \_g i namnet, ändra det första tecknet till gemener</span><span class="sxs-lookup"><span data-stu-id="bf14c-234">For any field that has a suffix of \_s, \_d, or \_g in the name, change the first character to lower case</span></span>
   + <span data-ttu-id="bf14c-235">För alla fält som har suffixet \_o i namn data delas upp i enskilda fält baserat på de kapslade fältnamn.</span><span class="sxs-lookup"><span data-stu-id="bf14c-235">For any field that has a suffix of \_o in name, the data is split into individual fields based on the nested field names.</span></span>
4. <span data-ttu-id="bf14c-236">Ta bort den *Azure nätverk Analytics (inaktuell)* lösning.</span><span class="sxs-lookup"><span data-stu-id="bf14c-236">Remove the *Azure Networking Analytics (Deprecated)* solution.</span></span>
  + <span data-ttu-id="bf14c-237">Om du använder PowerShell använder du`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="bf14c-237">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span></span>

<span data-ttu-id="bf14c-238">Data som samlas in innan ändringen inte visas i den nya lösningen.</span><span class="sxs-lookup"><span data-stu-id="bf14c-238">Data collected before the change is not visible in the new solution.</span></span> <span data-ttu-id="bf14c-239">Du kan fortsätta att fråga efter data med hjälp av den gamla typen och fältnamn.</span><span class="sxs-lookup"><span data-stu-id="bf14c-239">You can continue to query for this data using the old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bf14c-240">Felsökning</span><span class="sxs-lookup"><span data-stu-id="bf14c-240">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="bf14c-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf14c-241">Next steps</span></span>
* <span data-ttu-id="bf14c-242">Använd [logga sökningar i logganalys](log-analytics-log-searches.md) att visa detaljerad Azure-diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="bf14c-242">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed Azure diagnostics data.</span></span>
