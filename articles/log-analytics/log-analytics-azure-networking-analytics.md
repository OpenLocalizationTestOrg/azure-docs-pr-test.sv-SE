---
title: "aaaAzure nätverk Analytics lösning i Log Analytics | Microsoft Docs"
description: "Du kan använda hello Azure nätverk Analytics lösning i gruppen för Log Analytics tooreview Azure-nätverk säkerhetsloggar och Azure Programgateway loggar."
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
ms.openlocfilehash: 3674189786bacccc82e6708e78f14c92178e6676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a><span data-ttu-id="6f5b7-103">Azure nätverk övervakning lösningar i logganalys</span><span class="sxs-lookup"><span data-stu-id="6f5b7-103">Azure networking monitoring solutions in Log Analytics</span></span>

<span data-ttu-id="6f5b7-104">Logganalys erbjuder följande lösningar för att övervaka dina nätverk hello:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-104">Log Analytics offers hello following solutions for monitoring your networks:</span></span>
* <span data-ttu-id="6f5b7-105">Network Performance Monitor (NPM) till</span><span class="sxs-lookup"><span data-stu-id="6f5b7-105">Network Performance Monitor (NPM) to</span></span>
 * <span data-ttu-id="6f5b7-106">Övervaka hello hälsotillstånd för nätverket</span><span class="sxs-lookup"><span data-stu-id="6f5b7-106">Monitor hello health of your network</span></span>
* <span data-ttu-id="6f5b7-107">Azure Application Gateway analytics tooreview</span><span class="sxs-lookup"><span data-stu-id="6f5b7-107">Azure Application Gateway analytics tooreview</span></span>
 * <span data-ttu-id="6f5b7-108">Azure Application Gateway-loggar</span><span class="sxs-lookup"><span data-stu-id="6f5b7-108">Azure Application Gateway logs</span></span>
 * <span data-ttu-id="6f5b7-109">Azure Application Gateway-mått</span><span class="sxs-lookup"><span data-stu-id="6f5b7-109">Azure Application Gateway metrics</span></span>
* <span data-ttu-id="6f5b7-110">Azure Network Security Group analytics tooreview</span><span class="sxs-lookup"><span data-stu-id="6f5b7-110">Azure Network Security Group analytics tooreview</span></span>
 * <span data-ttu-id="6f5b7-111">Nätverkssäkerhetsgruppen för Azure-loggar</span><span class="sxs-lookup"><span data-stu-id="6f5b7-111">Azure Network Security Group logs</span></span>

## <a name="network-performance-monitor-npm"></a><span data-ttu-id="6f5b7-112">Network Performance Monitor (NPM)</span><span class="sxs-lookup"><span data-stu-id="6f5b7-112">Network Performance Monitor (NPM)</span></span>

<span data-ttu-id="6f5b7-113">Hej [Network Performance Monitor](log-analytics-network-performance-monitor.md) hanteringslösning är ett nätverk övervakningslösning som övervakar hello hälsa, tillgänglighet och tillgänglighet av nätverk.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-113">hello [Network Performance Monitor](log-analytics-network-performance-monitor.md) management solution is a network monitoring solution, that monitors hello health, availability and reachability of networks.</span></span>  <span data-ttu-id="6f5b7-114">Det är används toomonitor anslutningen mellan:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-114">It is used toomonitor connectivity between:</span></span>

* <span data-ttu-id="6f5b7-115">offentliga molnet och lokalt</span><span class="sxs-lookup"><span data-stu-id="6f5b7-115">Public cloud and on-premises</span></span>
* <span data-ttu-id="6f5b7-116">datacenter och användarplatser (avdelningskontor)</span><span class="sxs-lookup"><span data-stu-id="6f5b7-116">Data centers and user locations (branch offices)</span></span>
* <span data-ttu-id="6f5b7-117">undernät som är värd för olika nivåer av ett program med flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-117">Subnets hosting various tiers of a multi-tiered application.</span></span>

<span data-ttu-id="6f5b7-118">Mer information finns i [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="6f5b7-118">For more information, see [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span></span>

## <a name="azure-application-gateway-and-network-security-group-analytics"></a><span data-ttu-id="6f5b7-119">Azure Application Gateway och Nätverkssäkerhetsgruppen analytics</span><span class="sxs-lookup"><span data-stu-id="6f5b7-119">Azure Application Gateway and Network Security Group analytics</span></span>
<span data-ttu-id="6f5b7-120">toouse hello lösningar:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-120">toouse hello solutions:</span></span>
1. <span data-ttu-id="6f5b7-121">Lägg till hello management lösning tooLog analyser och</span><span class="sxs-lookup"><span data-stu-id="6f5b7-121">Add hello management solution tooLog Analytics, and</span></span>
2. <span data-ttu-id="6f5b7-122">Aktivera diagnostik toodirect hello diagnostik tooa logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-122">Enable diagnostics toodirect hello diagnostics tooa Log Analytics workspace.</span></span> <span data-ttu-id="6f5b7-123">Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-123">It is not necessary toowrite hello logs tooAzure Blob storage.</span></span>

<span data-ttu-id="6f5b7-124">Du kan aktivera diagnostik- och hello motsvarande lösning för en eller båda av Programgateway och nätverk säkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-124">You can enable diagnostics and hello corresponding solution for either one or both of Application Gateway and Networking Security Groups.</span></span>

<span data-ttu-id="6f5b7-125">Om du inte aktivera diagnostikloggning för en viss resurstyp och installera hello lösning hello instrumentpanelen blad för den här resursen är tomma och ett felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-125">If you do not enable diagnostic logging for a particular resource type, but install hello solution, hello dashboard blades for that resource are blank and display an error message.</span></span>

> [!NOTE]
> <span data-ttu-id="6f5b7-126">Hello stöds i januari 2017 sätt att skicka loggar från Programgatewayer och Nätverkssäkerhetsgrupper tooLog Analytics har ändrats.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-126">In January 2017, hello supported way of sending logs from Application Gateways and Network Security Groups tooLog Analytics changed.</span></span> <span data-ttu-id="6f5b7-127">Om du ser hello **Azure-nätverk Analytics (föråldrad)** lösningen finns för[migrera från hello gamla nätverk Analytics lösning](#migrating-from-the-old-networking-analytics-solution) för steg behöver du toofollow.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-127">If you see hello **Azure Networking Analytics (deprecated)** solution, refer too[migrating from hello old Networking Analytics solution](#migrating-from-the-old-networking-analytics-solution) for steps you need toofollow.</span></span>
>
>

## <a name="review-azure-networking-data-collection-details"></a><span data-ttu-id="6f5b7-128">Granska Azure nätverk information för samlingen</span><span class="sxs-lookup"><span data-stu-id="6f5b7-128">Review Azure networking data collection details</span></span>
<span data-ttu-id="6f5b7-129">samla in diagnostik loggarna direkt från Azure Programgatewayer och Nätverkssäkerhetsgrupper hello Azure Programgateway analyser och hello Network Security Group management Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-129">hello Azure Application Gateway analytics and hello Network Security Group analytics management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups.</span></span> <span data-ttu-id="6f5b7-130">Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage och ingen agent krävs för datainsamling.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-130">It is not necessary toowrite hello logs tooAzure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="6f5b7-131">hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Azure Programgateway analyser och hello Nätverkssäkerhetsgruppen analytics.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-131">hello following table shows data collection methods and other details about how data is collected for Azure Application Gateway analytics and hello Network Security Group analytics.</span></span>

| <span data-ttu-id="6f5b7-132">Plattform</span><span class="sxs-lookup"><span data-stu-id="6f5b7-132">Platform</span></span> | <span data-ttu-id="6f5b7-133">Styr agent</span><span class="sxs-lookup"><span data-stu-id="6f5b7-133">Direct agent</span></span> | <span data-ttu-id="6f5b7-134">System Center Operations Manager-agenten</span><span class="sxs-lookup"><span data-stu-id="6f5b7-134">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="6f5b7-135">Azure</span><span class="sxs-lookup"><span data-stu-id="6f5b7-135">Azure</span></span> | <span data-ttu-id="6f5b7-136">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="6f5b7-136">Operations Manager required?</span></span> | <span data-ttu-id="6f5b7-137">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="6f5b7-137">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="6f5b7-138">Insamlingsfrekvens</span><span class="sxs-lookup"><span data-stu-id="6f5b7-138">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="6f5b7-139">Azure</span><span class="sxs-lookup"><span data-stu-id="6f5b7-139">Azure</span></span> |  |  |<span data-ttu-id="6f5b7-140">&#8226;</span><span class="sxs-lookup"><span data-stu-id="6f5b7-140">&#8226;</span></span> |  |  |<span data-ttu-id="6f5b7-141">När du har loggat</span><span class="sxs-lookup"><span data-stu-id="6f5b7-141">when logged</span></span> |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a><span data-ttu-id="6f5b7-142">Azure Application Gateway analytics lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="6f5b7-142">Azure Application Gateway analytics solution in Log Analytics</span></span>

![Azure Application Gateway Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="6f5b7-144">följande loggar hello stöds för Programgatewayer:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-144">hello following logs are supported for Application Gateways:</span></span>

* <span data-ttu-id="6f5b7-145">ApplicationGatewayAccessLog</span><span class="sxs-lookup"><span data-stu-id="6f5b7-145">ApplicationGatewayAccessLog</span></span>
* <span data-ttu-id="6f5b7-146">ApplicationGatewayPerformanceLog</span><span class="sxs-lookup"><span data-stu-id="6f5b7-146">ApplicationGatewayPerformanceLog</span></span>
* <span data-ttu-id="6f5b7-147">ApplicationGatewayFirewallLog</span><span class="sxs-lookup"><span data-stu-id="6f5b7-147">ApplicationGatewayFirewallLog</span></span>

<span data-ttu-id="6f5b7-148">hello följande mått stöds för Programgatewayer:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-148">hello following metrics are supported for Application Gateways:</span></span>

* <span data-ttu-id="6f5b7-149">5 minut genomflöde</span><span class="sxs-lookup"><span data-stu-id="6f5b7-149">5 minute throughput</span></span>

### <a name="install-and-configure-hello-solution"></a><span data-ttu-id="6f5b7-150">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="6f5b7-150">Install and configure hello solution</span></span>
<span data-ttu-id="6f5b7-151">Använd följande instruktioner tooinstall hello och konfigurera hello Azure Programgateway analytics lösningen:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-151">Use hello following instructions tooinstall and configure hello Azure Application Gateway analytics solution:</span></span>

1. <span data-ttu-id="6f5b7-152">Aktivera hello Azure Programgateway analytics lösningar från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6f5b7-152">Enable hello Azure Application Gateway analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="6f5b7-153">Aktivera diagnostikloggning för hello [Programgatewayer](../application-gateway/application-gateway-diagnostics.md) du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-153">Enable diagnostics logging for hello [Application Gateways](../application-gateway/application-gateway-diagnostics.md) you want toomonitor.</span></span>

#### <a name="enable-azure-application-gateway-diagnostics-in-hello-portal"></a><span data-ttu-id="6f5b7-154">Aktivera Azure Programgateway diagnostik i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="6f5b7-154">Enable Azure Application Gateway diagnostics in hello portal</span></span>

1. <span data-ttu-id="6f5b7-155">Navigera i hello Azure-portalen, toohello Programgateway resurs toomonitor</span><span class="sxs-lookup"><span data-stu-id="6f5b7-155">In hello Azure portal, navigate toohello Application Gateway resource toomonitor</span></span>
2. <span data-ttu-id="6f5b7-156">Välj *diagnostik loggar* tooopen hello följande sida</span><span class="sxs-lookup"><span data-stu-id="6f5b7-156">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![Bild av Azure Programgateway resurs](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. <span data-ttu-id="6f5b7-158">Klicka på *aktivera diagnostiken* tooopen hello följande sida</span><span class="sxs-lookup"><span data-stu-id="6f5b7-158">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![Bild av Azure Programgateway resurs](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. <span data-ttu-id="6f5b7-160">tooturn på diagnostik, klickar du på *på* under *Status*</span><span class="sxs-lookup"><span data-stu-id="6f5b7-160">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="6f5b7-161">Klicka på hello kryssrutan för *skicka tooLog Analytics*</span><span class="sxs-lookup"><span data-stu-id="6f5b7-161">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="6f5b7-162">Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="6f5b7-162">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="6f5b7-163">Klicka på kryssrutan hello under **loggen** för varje hello loggen typer toocollect</span><span class="sxs-lookup"><span data-stu-id="6f5b7-163">Click hello checkbox under **Log** for each of hello log types toocollect</span></span>
8. <span data-ttu-id="6f5b7-164">Klicka på *spara* tooenable hello loggning av diagnostik tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="6f5b7-164">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

#### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="6f5b7-165">Aktivera Azure Nätverksdiagnostik med PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f5b7-165">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="6f5b7-166">hello följande PowerShell-skript innehåller ett exempel på hur tooenable loggning för programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-166">hello following PowerShell script provides an example of how tooenable diagnostic logging for application gateways.</span></span>

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a><span data-ttu-id="6f5b7-167">Använd Azure Programgateway analytics</span><span class="sxs-lookup"><span data-stu-id="6f5b7-167">Use Azure Application Gateway analytics</span></span>
![Bild av Azure Programgateway analytics panelen](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

<span data-ttu-id="6f5b7-169">När du klickar på hello **Azure Programgateway analytics** panelen på hello översikt, kan du visa sammanfattningar av loggar och visa sedan detaljnivåerna i toodetails för hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-169">After you click hello **Azure Application Gateway analytics** tile on hello Overview, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="6f5b7-170">Programåtkomst för Gateway-loggar</span><span class="sxs-lookup"><span data-stu-id="6f5b7-170">Application Gateway Access logs</span></span>
  * <span data-ttu-id="6f5b7-171">Klient- och fel för åtkomstloggar för Programgateway</span><span class="sxs-lookup"><span data-stu-id="6f5b7-171">Client and server errors for Application Gateway access logs</span></span>
  * <span data-ttu-id="6f5b7-172">Förfrågningar per timme för varje Programgateway</span><span class="sxs-lookup"><span data-stu-id="6f5b7-172">Requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="6f5b7-173">Misslyckade förfrågningar per timme för varje Programgateway</span><span class="sxs-lookup"><span data-stu-id="6f5b7-173">Failed requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="6f5b7-174">Fel per användaragent för Programgatewayer</span><span class="sxs-lookup"><span data-stu-id="6f5b7-174">Errors by user agent for Application Gateways</span></span>
* <span data-ttu-id="6f5b7-175">Programprestanda för Gateway</span><span class="sxs-lookup"><span data-stu-id="6f5b7-175">Application Gateway performance</span></span>
  * <span data-ttu-id="6f5b7-176">Värden hälsa för Programgateway</span><span class="sxs-lookup"><span data-stu-id="6f5b7-176">Host health for Application Gateway</span></span>
  * <span data-ttu-id="6f5b7-177">Maximal och 95 percentil för Programgateway misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-177">Maximum and 95th percentile for Application Gateway failed requests</span></span>

![Bild av instrumentpanelen för Azure Programgateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Bild av instrumentpanelen för Azure Programgateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

<span data-ttu-id="6f5b7-180">På hello **Azure Programgateway analytics** instrumentpanel, granska hello sammanfattningsinformation i ett hello blad och klicka sedan på en tooview detaljerad information om hello söksidan för loggen.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-180">On hello **Azure Application Gateway analytics** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information on hello log search page.</span></span>

<span data-ttu-id="6f5b7-181">Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-181">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="6f5b7-182">Du kan också filtrera efter facets toonarrow hello resultat.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-182">You can also filter by facets toonarrow hello results.</span></span>


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a><span data-ttu-id="6f5b7-183">Azure Network Security Group analytics lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="6f5b7-183">Azure Network Security Group analytics solution in Log Analytics</span></span>

![Azure Network Security Group Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="6f5b7-185">följande loggar hello stöds för nätverkssäkerhetsgrupper:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-185">hello following logs are supported for network security groups:</span></span>

* <span data-ttu-id="6f5b7-186">NetworkSecurityGroupEvent</span><span class="sxs-lookup"><span data-stu-id="6f5b7-186">NetworkSecurityGroupEvent</span></span>
* <span data-ttu-id="6f5b7-187">NetworkSecurityGroupRuleCounter</span><span class="sxs-lookup"><span data-stu-id="6f5b7-187">NetworkSecurityGroupRuleCounter</span></span>

### <a name="install-and-configure-hello-solution"></a><span data-ttu-id="6f5b7-188">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="6f5b7-188">Install and configure hello solution</span></span>
<span data-ttu-id="6f5b7-189">Använd följande instruktioner tooinstall hello och konfigurera hello Azure nätverk Analytics lösning:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-189">Use hello following instructions tooinstall and configure hello Azure Networking Analytics solution:</span></span>

1. <span data-ttu-id="6f5b7-190">Aktivera hello Azure Network Security Group analytics lösningar från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6f5b7-190">Enable hello Azure Network Security Group analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="6f5b7-191">Aktivera diagnostikloggning för hello [Nätverkssäkerhetsgruppen](../virtual-network/virtual-network-nsg-manage-log.md) resurser du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-191">Enable diagnostics logging for hello [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md) resources you want toomonitor.</span></span>

### <a name="enable-azure-network-security-group-diagnostics-in-hello-portal"></a><span data-ttu-id="6f5b7-192">Aktivera diagnostik av gruppen för Azure-nätverk i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="6f5b7-192">Enable Azure network security group diagnostics in hello portal</span></span>

1. <span data-ttu-id="6f5b7-193">Navigera i hello Azure-portalen, toohello Nätverkssäkerhetsgruppen resurs toomonitor</span><span class="sxs-lookup"><span data-stu-id="6f5b7-193">In hello Azure portal, navigate toohello Network Security Group resource toomonitor</span></span>
2. <span data-ttu-id="6f5b7-194">Välj *diagnostik loggar* tooopen hello följande sida</span><span class="sxs-lookup"><span data-stu-id="6f5b7-194">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![Bild av Azure Network Security Group resurs](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. <span data-ttu-id="6f5b7-196">Klicka på *aktivera diagnostiken* tooopen hello följande sida</span><span class="sxs-lookup"><span data-stu-id="6f5b7-196">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![Bild av Azure Network Security Group resurs](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. <span data-ttu-id="6f5b7-198">tooturn på diagnostik, klickar du på *på* under *Status*</span><span class="sxs-lookup"><span data-stu-id="6f5b7-198">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="6f5b7-199">Klicka på hello kryssrutan för *skicka tooLog Analytics*</span><span class="sxs-lookup"><span data-stu-id="6f5b7-199">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="6f5b7-200">Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="6f5b7-200">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="6f5b7-201">Klicka på kryssrutan hello under **loggen** för varje hello loggen typer toocollect</span><span class="sxs-lookup"><span data-stu-id="6f5b7-201">Click hello checkbox under **Log** for each of hello log types toocollect</span></span>
8. <span data-ttu-id="6f5b7-202">Klicka på *spara* tooenable hello loggning av diagnostik tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="6f5b7-202">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="6f5b7-203">Aktivera Azure Nätverksdiagnostik med PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f5b7-203">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="6f5b7-204">hello följande PowerShell-skript innehåller ett exempel på hur tooenable loggning för nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="6f5b7-204">hello following PowerShell script provides an example of how tooenable diagnostic logging for network security groups</span></span>
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a><span data-ttu-id="6f5b7-205">Använd Azure Network Security Group analytics</span><span class="sxs-lookup"><span data-stu-id="6f5b7-205">Use Azure Network Security Group analytics</span></span>
<span data-ttu-id="6f5b7-206">När du klickar på hello **Azure Network Security Group analytics** panelen på hello översikt, kan du visa sammanfattningar av loggar och visa sedan detaljnivåerna i toodetails för hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-206">After you click hello **Azure Network Security Group analytics** tile on hello Overview, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="6f5b7-207">Nätverkssäkerhetsgruppen blockeras flöden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-207">Network security group blocked flows</span></span>
  * <span data-ttu-id="6f5b7-208">Regler för nätverkssäkerhetsgrupper med blockerade flöden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-208">Network security group rules with blocked flows</span></span>
  * <span data-ttu-id="6f5b7-209">MAC-adresser med blockerade flöden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-209">MAC addresses with blocked flows</span></span>
* <span data-ttu-id="6f5b7-210">Nätverkssäkerhetsgruppen tillåtna flöden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-210">Network security group allowed flows</span></span>
  * <span data-ttu-id="6f5b7-211">Regler för nätverkssäkerhetsgrupper med tillåtna flöden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-211">Network security group rules with allowed flows</span></span>
  * <span data-ttu-id="6f5b7-212">MAC-adresser med tillåtna flöden</span><span class="sxs-lookup"><span data-stu-id="6f5b7-212">MAC addresses with allowed flows</span></span>

![Bild av instrumentpanelen för Azure Nätverkssäkerhetsgrupp](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Bild av instrumentpanelen för Azure Nätverkssäkerhetsgrupp](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

<span data-ttu-id="6f5b7-215">På hello **Azure Network Security Group analytics** instrumentpanel, granska hello sammanfattningsinformation i ett hello blad och klicka sedan på en tooview detaljerad information om hello söksidan för loggen.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-215">On hello **Azure Network Security Group analytics** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information on hello log search page.</span></span>

<span data-ttu-id="6f5b7-216">Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-216">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="6f5b7-217">Du kan också filtrera efter facets toonarrow hello resultat.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-217">You can also filter by facets toonarrow hello results.</span></span>

## <a name="migrating-from-hello-old-networking-analytics-solution"></a><span data-ttu-id="6f5b7-218">Migrera från hello gamla nätverk Analytics lösning</span><span class="sxs-lookup"><span data-stu-id="6f5b7-218">Migrating from hello old Networking Analytics solution</span></span>
<span data-ttu-id="6f5b7-219">Hello stöds i januari 2017 sätt att skicka loggar från Azure Programgatewayer och säkerhetsgrupper för Azure-nätverket tooLog Analytics har ändrats.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-219">In January 2017, hello supported way of sending logs from Azure Application Gateways and Azure Network Security Groups tooLog Analytics changed.</span></span> <span data-ttu-id="6f5b7-220">Ändringarna ger hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-220">These changes provide hello following advantages:</span></span>
+ <span data-ttu-id="6f5b7-221">Loggarna skrivs direkt tooLog Analytics utan hello måste toouse ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="6f5b7-221">Logs are written directly tooLog Analytics without hello need toouse a storage account</span></span>
+ <span data-ttu-id="6f5b7-222">Mindre fördröjning från hello tid när loggar är genereras toothem som är tillgängliga i logganalys</span><span class="sxs-lookup"><span data-stu-id="6f5b7-222">Less latency from hello time when logs are generated toothem being available in Log Analytics</span></span>
+ <span data-ttu-id="6f5b7-223">Färre konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="6f5b7-223">Fewer configuration steps</span></span>
+ <span data-ttu-id="6f5b7-224">Ett vanligt format för alla typer av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="6f5b7-224">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="6f5b7-225">toouse hello uppdateras lösningar:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-225">toouse hello updated solutions:</span></span>

1. [<span data-ttu-id="6f5b7-226">Konfigurera diagnostik toobe tooLog Analytics skickas direkt från Azure Programgatewayer</span><span class="sxs-lookup"><span data-stu-id="6f5b7-226">Configure diagnostics toobe sent directly tooLog Analytics from Azure Application Gateways</span></span>](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [<span data-ttu-id="6f5b7-227">Konfigurera diagnostik toobe tooLog Analytics skickas direkt från Azure Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="6f5b7-227">Configure diagnostics toobe sent directly tooLog Analytics from Azure Network Security Groups</span></span>](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. <span data-ttu-id="6f5b7-228">Aktivera hello *Azure Application Gateway Analytics* och hello *Azure Network Security Group Analytics* lösning genom att använda hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleri](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="6f5b7-228">Enable hello *Azure Application Gateway Analytics* and hello *Azure Network Security Group Analytics* solution by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="6f5b7-229">Uppdatera alla sparade frågor, instrumentpaneler eller aviseringar toouse hello ny datatyp</span><span class="sxs-lookup"><span data-stu-id="6f5b7-229">Update any saved queries, dashboards, or alerts toouse hello new data type</span></span>
  + <span data-ttu-id="6f5b7-230">Typen är tooAzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-230">Type is tooAzureDiagnostics.</span></span> <span data-ttu-id="6f5b7-231">Du kan använda hello ResourceType toofilter tooAzure nätverk loggar.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-231">You can use hello ResourceType toofilter tooAzure networking logs.</span></span>

    | <span data-ttu-id="6f5b7-232">Istället för:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-232">Instead of:</span></span> | <span data-ttu-id="6f5b7-233">Användning:</span><span class="sxs-lookup"><span data-stu-id="6f5b7-233">Use:</span></span> |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + <span data-ttu-id="6f5b7-234">För alla fält som har suffixet \_s, \_d, eller \_g i hello namn, ändra hello första toolower skiftlägeskänslighet</span><span class="sxs-lookup"><span data-stu-id="6f5b7-234">For any field that has a suffix of \_s, \_d, or \_g in hello name, change hello first character toolower case</span></span>
   + <span data-ttu-id="6f5b7-235">För alla fält som har suffixet \_o i namn hello data delas upp i enskilda fält baserat på hello kapslade fältnamn.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-235">For any field that has a suffix of \_o in name, hello data is split into individual fields based on hello nested field names.</span></span>
4. <span data-ttu-id="6f5b7-236">Ta bort hello *Azure nätverk Analytics (inaktuell)* lösning.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-236">Remove hello *Azure Networking Analytics (Deprecated)* solution.</span></span>
  + <span data-ttu-id="6f5b7-237">Om du använder PowerShell använder du`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="6f5b7-237">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span></span>

<span data-ttu-id="6f5b7-238">Data som samlas in innan hello ändringen inte visas i hello ny lösning.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-238">Data collected before hello change is not visible in hello new solution.</span></span> <span data-ttu-id="6f5b7-239">Du kan fortsätta tooquery för den här data med hjälp av hello gamla typ och fältnamn.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-239">You can continue tooquery for this data using hello old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6f5b7-240">Felsökning</span><span class="sxs-lookup"><span data-stu-id="6f5b7-240">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="6f5b7-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f5b7-241">Next steps</span></span>
* <span data-ttu-id="6f5b7-242">Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerad Azure-diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="6f5b7-242">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Azure diagnostics data.</span></span>
