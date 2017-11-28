---
title: "aaaAzure Key Vault-lösning i Log Analytics | Microsoft Docs"
description: "Du kan använda hello Azure Key Vault-lösning i logganalys tooreview Azure Key Vault loggar."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 1c6eae26ded7ad55b0159a3be09cdc9901596298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a><span data-ttu-id="18870-103">Azure Key Vault Analytics lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="18870-103">Azure Key Vault Analytics solution in Log Analytics</span></span>

![Key Vault symbol](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

<span data-ttu-id="18870-105">Du kan använda hello Azure Key Vault-lösning i logganalys tooreview Azure Key Vault AuditEvent loggar.</span><span class="sxs-lookup"><span data-stu-id="18870-105">You can use hello Azure Key Vault solution in Log Analytics tooreview Azure Key Vault AuditEvent logs.</span></span>

<span data-ttu-id="18870-106">toouse hello lösningen måste tooenable loggning av Azure Key Vault diagnostik- och direkt hello diagnostik tooa logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="18870-106">toouse hello solution, you need tooenable logging of Azure Key Vault diagnostics and direct hello diagnostics tooa Log Analytics workspace.</span></span> <span data-ttu-id="18870-107">Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="18870-107">It is not necessary toowrite hello logs tooAzure Blob storage.</span></span>

> [!NOTE]
> <span data-ttu-id="18870-108">Hello stöds i januari 2017 sätt att skicka loggar från Nyckelvalvet tooLog Analytics har ändrats.</span><span class="sxs-lookup"><span data-stu-id="18870-108">In January 2017, hello supported way of sending logs from Key Vault tooLog Analytics changed.</span></span> <span data-ttu-id="18870-109">Visar om hello Key Vault-lösningen som du använder *(föråldrad)* i hello titeln, referera för[migrera från hello gamla Key Vault lösning](#migrating-from-the-old-key-vault-solution) för steg behöver du toofollow.</span><span class="sxs-lookup"><span data-stu-id="18870-109">If hello Key Vault solution you are using shows *(deprecated)* in hello title, refer too[migrating from hello old Key Vault solution](#migrating-from-the-old-key-vault-solution) for steps you need toofollow.</span></span>
>
>

## <a name="install-and-configure-hello-solution"></a><span data-ttu-id="18870-110">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="18870-110">Install and configure hello solution</span></span>
<span data-ttu-id="18870-111">Använd följande instruktioner tooinstall hello och konfigurera hello Azure Key Vault-lösningen:</span><span class="sxs-lookup"><span data-stu-id="18870-111">Use hello following instructions tooinstall and configure hello Azure Key Vault solution:</span></span>

1. <span data-ttu-id="18870-112">Aktivera hello Azure Key Vault-lösning från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="18870-112">Enable hello Azure Key Vault solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="18870-113">Aktivera diagnostik loggning för hello Key Vault resurser toomonitor, med antingen hello [portal](#enable-key-vault-diagnostics-in-the-portal) eller [PowerShell](#enable-key-vault-diagnostics-using-powershell)</span><span class="sxs-lookup"><span data-stu-id="18870-113">Enable diagnostics logging for hello Key Vault resources toomonitor, using either hello [portal](#enable-key-vault-diagnostics-in-the-portal) or [PowerShell](#enable-key-vault-diagnostics-using-powershell)</span></span>

### <a name="enable-key-vault-diagnostics-in-hello-portal"></a><span data-ttu-id="18870-114">Aktivera Key Vault-diagnostik i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="18870-114">Enable Key Vault diagnostics in hello portal</span></span>

1. <span data-ttu-id="18870-115">Navigera i hello Azure-portalen, toohello Key Vault resurs toomonitor</span><span class="sxs-lookup"><span data-stu-id="18870-115">In hello Azure portal, navigate toohello Key Vault resource toomonitor</span></span>
2. <span data-ttu-id="18870-116">Välj *diagnostik loggar* tooopen hello följande sida</span><span class="sxs-lookup"><span data-stu-id="18870-116">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![Bild av Azure Key Vault sida vid sida](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. <span data-ttu-id="18870-118">Klicka på *aktivera diagnostiken* tooopen hello följande sida</span><span class="sxs-lookup"><span data-stu-id="18870-118">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![Bild av Azure Key Vault sida vid sida](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. <span data-ttu-id="18870-120">tooturn på diagnostik, klickar du på *på* under *Status*</span><span class="sxs-lookup"><span data-stu-id="18870-120">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="18870-121">Klicka på hello kryssrutan för *skicka tooLog Analytics*</span><span class="sxs-lookup"><span data-stu-id="18870-121">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="18870-122">Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="18870-122">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="18870-123">tooenable *AuditEvent* loggar, klickar du på kryssrutan hello under logg</span><span class="sxs-lookup"><span data-stu-id="18870-123">tooenable *AuditEvent* logs, click hello checkbox under Log</span></span>
8. <span data-ttu-id="18870-124">Klicka på *spara* tooenable hello loggning av diagnostik tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="18870-124">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

### <a name="enable-key-vault-diagnostics-using-powershell"></a><span data-ttu-id="18870-125">Aktivera diagnostik för Key Vault med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="18870-125">Enable Key Vault diagnostics using PowerShell</span></span>
<span data-ttu-id="18870-126">hello följande PowerShell-skript innehåller ett exempel på hur toouse `Set-AzureRmDiagnosticSetting` tooenable loggning för Key Vault:</span><span class="sxs-lookup"><span data-stu-id="18870-126">hello following PowerShell script provides an example of how toouse `Set-AzureRmDiagnosticSetting` tooenable diagnostic logging for Key Vault:</span></span>
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a><span data-ttu-id="18870-127">Granska Azure Key Vault information för samlingen</span><span class="sxs-lookup"><span data-stu-id="18870-127">Review Azure Key Vault data collection details</span></span>
<span data-ttu-id="18870-128">Azure Key Vault-lösningen samlar in diagnostik loggarna direkt från hello Key Vault.</span><span class="sxs-lookup"><span data-stu-id="18870-128">Azure Key Vault solution collects diagnostics logs directly from hello Key Vault.</span></span>
<span data-ttu-id="18870-129">Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage och ingen agent krävs för datainsamling.</span><span class="sxs-lookup"><span data-stu-id="18870-129">It is not necessary toowrite hello logs tooAzure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="18870-130">hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="18870-130">hello following table shows data collection methods and other details about how data is collected for Azure Key Vault.</span></span>

| <span data-ttu-id="18870-131">Plattform</span><span class="sxs-lookup"><span data-stu-id="18870-131">Platform</span></span> | <span data-ttu-id="18870-132">Styr agent</span><span class="sxs-lookup"><span data-stu-id="18870-132">Direct agent</span></span> | <span data-ttu-id="18870-133">System Center Operations Manager-agenten</span><span class="sxs-lookup"><span data-stu-id="18870-133">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="18870-134">Azure</span><span class="sxs-lookup"><span data-stu-id="18870-134">Azure</span></span> | <span data-ttu-id="18870-135">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="18870-135">Operations Manager required?</span></span> | <span data-ttu-id="18870-136">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="18870-136">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="18870-137">Insamlingsfrekvens</span><span class="sxs-lookup"><span data-stu-id="18870-137">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="18870-138">Azure</span><span class="sxs-lookup"><span data-stu-id="18870-138">Azure</span></span> |  |  |<span data-ttu-id="18870-139">&#8226;</span><span class="sxs-lookup"><span data-stu-id="18870-139">&#8226;</span></span> |  |  | <span data-ttu-id="18870-140">anländer</span><span class="sxs-lookup"><span data-stu-id="18870-140">on arrival</span></span> |

## <a name="use-azure-key-vault"></a><span data-ttu-id="18870-141">Använda Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="18870-141">Use Azure Key Vault</span></span>
<span data-ttu-id="18870-142">När du [installera hello lösning](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), visa hello Key Vault data genom att klicka på hello **Azure Key Vault** panelen från hello **översikt** sidan i logganalys.</span><span class="sxs-lookup"><span data-stu-id="18870-142">After you [install hello solution](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), view hello Key Vault data by clicking hello **Azure Key Vault** tile from hello **Overview** page of Log Analytics.</span></span>

![Bild av Azure Key Vault sida vid sida](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

<span data-ttu-id="18870-144">När du klickar på hello **översikt** sida vid sida, kan du visa sammanfattningar av loggar och sedan gå i toodetails för hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="18870-144">After you click hello **Overview** tile, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="18870-145">Volymen för alla åtgärder i nyckelvalvet över tid</span><span class="sxs-lookup"><span data-stu-id="18870-145">Volume of all key vault operations over time</span></span>
* <span data-ttu-id="18870-146">Det gick inte åtgärden volymer över tid</span><span class="sxs-lookup"><span data-stu-id="18870-146">Failed operation volumes over time</span></span>
* <span data-ttu-id="18870-147">Genomsnittlig svarstid för operativa för åtgärden</span><span class="sxs-lookup"><span data-stu-id="18870-147">Average operational latency by operation</span></span>
* <span data-ttu-id="18870-148">Tjänstkvalitet för åtgärder med hello antal åtgärder som tar mer än 1 000 ms och en lista över åtgärder som tar mer än 1 000 ms</span><span class="sxs-lookup"><span data-stu-id="18870-148">Quality of service for operations with hello number of operations that take more than 1000 ms and a list of operations that take more than 1000 ms</span></span>

![Bild av Azure Key Vault-instrumentpanelen](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Bild av Azure Key Vault-instrumentpanelen](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="tooview-details-for-any-operation"></a><span data-ttu-id="18870-151">tooview information för någon åtgärd</span><span class="sxs-lookup"><span data-stu-id="18870-151">tooview details for any operation</span></span>
1. <span data-ttu-id="18870-152">På hello **översikt** klickar du på hello **Azure Key Vault** panelen.</span><span class="sxs-lookup"><span data-stu-id="18870-152">On hello **Overview** page, click hello **Azure Key Vault** tile.</span></span>
2. <span data-ttu-id="18870-153">På hello **Azure Key Vault** instrumentpanel, granska hello sammanfattningsinformation i ett hello blad och klicka sedan på en tooview detaljerad information om den hello söksidan för loggen.</span><span class="sxs-lookup"><span data-stu-id="18870-153">On hello **Azure Key Vault** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information about it in hello log search page.</span></span>

    <span data-ttu-id="18870-154">Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor.</span><span class="sxs-lookup"><span data-stu-id="18870-154">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="18870-155">Du kan också filtrera efter facets toonarrow hello resultat.</span><span class="sxs-lookup"><span data-stu-id="18870-155">You can also filter by facets toonarrow hello results.</span></span>

## <a name="log-analytics-records"></a><span data-ttu-id="18870-156">Log Analytics-poster</span><span class="sxs-lookup"><span data-stu-id="18870-156">Log Analytics records</span></span>
<span data-ttu-id="18870-157">hello Azure Key Vault lösning analyserar poster som har en typ av **KeyVaults** som samlas in från [AuditEvent loggar](../key-vault/key-vault-logging.md) i Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="18870-157">hello Azure Key Vault solution analyzes records that have a type of **KeyVaults** that are collected from [AuditEvent logs](../key-vault/key-vault-logging.md) in Azure Diagnostics.</span></span>  <span data-ttu-id="18870-158">Egenskaper för dessa poster finns i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="18870-158">Properties for these records are in hello following table:</span></span>  

| <span data-ttu-id="18870-159">Egenskap</span><span class="sxs-lookup"><span data-stu-id="18870-159">Property</span></span> | <span data-ttu-id="18870-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="18870-160">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="18870-161">Typ</span><span class="sxs-lookup"><span data-stu-id="18870-161">Type</span></span> |<span data-ttu-id="18870-162">*AzureDiagnostics*</span><span class="sxs-lookup"><span data-stu-id="18870-162">*AzureDiagnostics*</span></span> |
| <span data-ttu-id="18870-163">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="18870-163">SourceSystem</span></span> |<span data-ttu-id="18870-164">*Azure*</span><span class="sxs-lookup"><span data-stu-id="18870-164">*Azure*</span></span> |
| <span data-ttu-id="18870-165">CallerIpAddress</span><span class="sxs-lookup"><span data-stu-id="18870-165">CallerIpAddress</span></span> |<span data-ttu-id="18870-166">IP-adressen för hello-klienten som gjorde begäran hello</span><span class="sxs-lookup"><span data-stu-id="18870-166">IP address of hello client who made hello request</span></span> |
| <span data-ttu-id="18870-167">Kategori</span><span class="sxs-lookup"><span data-stu-id="18870-167">Category</span></span> | <span data-ttu-id="18870-168">*AuditEvent*</span><span class="sxs-lookup"><span data-stu-id="18870-168">*AuditEvent*</span></span> |
| <span data-ttu-id="18870-169">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="18870-169">CorrelationId</span></span> |<span data-ttu-id="18870-170">Ett valfritt GUID som hello klienten kan skicka toocorrelate klientsidan loggar med loggar av tjänsten på klientsidan (Key Vault).</span><span class="sxs-lookup"><span data-stu-id="18870-170">An optional GUID that hello client can pass toocorrelate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="18870-171">DurationMs</span><span class="sxs-lookup"><span data-stu-id="18870-171">DurationMs</span></span> |<span data-ttu-id="18870-172">Tiden det tog tooservice hello REST-API-begäran, i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="18870-172">Time it took tooservice hello REST API request, in milliseconds.</span></span> <span data-ttu-id="18870-173">Nu omfattar inte Nätverksfördröjningen, så hello tid som du mäter på klientsidan för hello inte kanske stämmer med den här gången.</span><span class="sxs-lookup"><span data-stu-id="18870-173">This time does not include network latency, so hello time that you measure on hello client side might not match this time.</span></span> |
| <span data-ttu-id="18870-174">httpStatusCode_d</span><span class="sxs-lookup"><span data-stu-id="18870-174">httpStatusCode_d</span></span> |<span data-ttu-id="18870-175">HTTP-statuskoden som returnerades av hello begäran (till exempel *200*)</span><span class="sxs-lookup"><span data-stu-id="18870-175">HTTP status code returned by hello request (for example, *200*)</span></span> |
| <span data-ttu-id="18870-176">id_s</span><span class="sxs-lookup"><span data-stu-id="18870-176">id_s</span></span> |<span data-ttu-id="18870-177">Unikt ID för hello begäran</span><span class="sxs-lookup"><span data-stu-id="18870-177">Unique ID of hello request</span></span> |
| <span data-ttu-id="18870-178">identity_claim_appid_g</span><span class="sxs-lookup"><span data-stu-id="18870-178">identity_claim_appid_g</span></span> | <span data-ttu-id="18870-179">GUID för hello program-id</span><span class="sxs-lookup"><span data-stu-id="18870-179">GUID for hello application id</span></span> |
| <span data-ttu-id="18870-180">OperationName</span><span class="sxs-lookup"><span data-stu-id="18870-180">OperationName</span></span> |<span data-ttu-id="18870-181">Namnet på hello åtgärd utförs, enligt beskrivningen i [Azure Key Vault-loggning](../key-vault/key-vault-logging.md)</span><span class="sxs-lookup"><span data-stu-id="18870-181">Name of hello operation, as documented in [Azure Key Vault Logging](../key-vault/key-vault-logging.md)</span></span> |
| <span data-ttu-id="18870-182">OperationVersion</span><span class="sxs-lookup"><span data-stu-id="18870-182">OperationVersion</span></span> |<span data-ttu-id="18870-183">REST API-version som begärs av hello klient (till exempel *2015-06-01*)</span><span class="sxs-lookup"><span data-stu-id="18870-183">REST API version requested by hello client (for example *2015-06-01*)</span></span> |
| <span data-ttu-id="18870-184">requestUri_s</span><span class="sxs-lookup"><span data-stu-id="18870-184">requestUri_s</span></span> |<span data-ttu-id="18870-185">URI för begäran om hello</span><span class="sxs-lookup"><span data-stu-id="18870-185">Uri of hello request</span></span> |
| <span data-ttu-id="18870-186">Resurs</span><span class="sxs-lookup"><span data-stu-id="18870-186">Resource</span></span> |<span data-ttu-id="18870-187">Namnet på hello nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="18870-187">Name of hello key vault</span></span> |
| <span data-ttu-id="18870-188">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="18870-188">ResourceGroup</span></span> |<span data-ttu-id="18870-189">Resursgruppen för hello nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="18870-189">Resource group of hello key vault</span></span> |
| <span data-ttu-id="18870-190">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="18870-190">ResourceId</span></span> |<span data-ttu-id="18870-191">Azure Resource Manager Resource-ID.</span><span class="sxs-lookup"><span data-stu-id="18870-191">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="18870-192">För Key Vault loggar är hello Key Vault resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="18870-192">For Key Vault logs, this is hello Key Vault resource ID.</span></span> |
| <span data-ttu-id="18870-193">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="18870-193">ResourceProvider</span></span> |<span data-ttu-id="18870-194">*MICROSOFT. KEYVAULT*</span><span class="sxs-lookup"><span data-stu-id="18870-194">*MICROSOFT.KEYVAULT*</span></span> |
| <span data-ttu-id="18870-195">ResourceType</span><span class="sxs-lookup"><span data-stu-id="18870-195">ResourceType</span></span> | <span data-ttu-id="18870-196">*VALV*</span><span class="sxs-lookup"><span data-stu-id="18870-196">*VAULTS*</span></span> |
| <span data-ttu-id="18870-197">ResultSignature</span><span class="sxs-lookup"><span data-stu-id="18870-197">ResultSignature</span></span> |<span data-ttu-id="18870-198">HTTP-status (till exempel *OK*)</span><span class="sxs-lookup"><span data-stu-id="18870-198">HTTP status (for example, *OK*)</span></span> |
| <span data-ttu-id="18870-199">ResultType</span><span class="sxs-lookup"><span data-stu-id="18870-199">ResultType</span></span> |<span data-ttu-id="18870-200">Resultatet av REST API-begäran (till exempel *lyckade*)</span><span class="sxs-lookup"><span data-stu-id="18870-200">Result of REST API request (for example, *Success*)</span></span> |
| <span data-ttu-id="18870-201">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="18870-201">SubscriptionId</span></span> |<span data-ttu-id="18870-202">Azure prenumerations-ID för hello prenumeration som innehåller hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="18870-202">Azure subscription ID of hello subscription containing hello Key Vault</span></span> |

## <a name="migrating-from-hello-old-key-vault-solution"></a><span data-ttu-id="18870-203">Migrera från hello gamla Key Vault-lösning</span><span class="sxs-lookup"><span data-stu-id="18870-203">Migrating from hello old Key Vault solution</span></span>
<span data-ttu-id="18870-204">Hello stöds i januari 2017 sätt att skicka loggar från Nyckelvalvet tooLog Analytics har ändrats.</span><span class="sxs-lookup"><span data-stu-id="18870-204">In January 2017, hello supported way of sending logs from Key Vault tooLog Analytics changed.</span></span> <span data-ttu-id="18870-205">Ändringarna ger hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="18870-205">These changes provide hello following advantages:</span></span>
+ <span data-ttu-id="18870-206">Loggarna skrivs direkt tooLog Analytics utan hello måste toouse ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="18870-206">Logs are written directly tooLog Analytics without hello need toouse a storage account</span></span>
+ <span data-ttu-id="18870-207">Mindre fördröjning från hello tid när loggar är genereras toothem som är tillgängliga i logganalys</span><span class="sxs-lookup"><span data-stu-id="18870-207">Less latency from hello time when logs are generated toothem being available in Log Analytics</span></span>
+ <span data-ttu-id="18870-208">Färre konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="18870-208">Fewer configuration steps</span></span>
+ <span data-ttu-id="18870-209">Ett vanligt format för alla typer av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="18870-209">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="18870-210">toouse hello uppdateras lösningen:</span><span class="sxs-lookup"><span data-stu-id="18870-210">toouse hello updated solution:</span></span>

1. [<span data-ttu-id="18870-211">Konfigurera diagnostik toobe tooLog Analytics skickas direkt från Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="18870-211">Configure diagnostics toobe sent directly tooLog Analytics from Key Vault</span></span>](#enable-key-vault-diagnostics-in-the-portal)  
2. <span data-ttu-id="18870-212">Aktivera hello Azure Key Vault-lösning med hjälp av hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleri](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="18870-212">Enable hello Azure Key Vault solution by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="18870-213">Uppdatera alla sparade frågor, instrumentpaneler eller aviseringar toouse hello ny datatyp</span><span class="sxs-lookup"><span data-stu-id="18870-213">Update any saved queries, dashboards, or alerts toouse hello new data type</span></span>
  + <span data-ttu-id="18870-214">Typen är ändra från: KeyVaults tooAzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="18870-214">Type is change from: KeyVaults tooAzureDiagnostics.</span></span> <span data-ttu-id="18870-215">Du kan använda hello ResourceType toofilter tooKey valvet loggar.</span><span class="sxs-lookup"><span data-stu-id="18870-215">You can use hello ResourceType toofilter tooKey Vault Logs.</span></span>
  - <span data-ttu-id="18870-216">I stället för: `Type=KeyVaults`, använda`Type=AzureDiagnostics ResourceType=VAULTS`</span><span class="sxs-lookup"><span data-stu-id="18870-216">Instead of: `Type=KeyVaults`, use `Type=AzureDiagnostics ResourceType=VAULTS`</span></span>
  + <span data-ttu-id="18870-217">Fält: (fältnamn är skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="18870-217">Fields: (Field names are case-sensitive)</span></span>
  - <span data-ttu-id="18870-218">För alla fält som har suffixet \_s, \_d, eller \_g i hello namn, ändra hello första toolower skiftlägeskänslighet</span><span class="sxs-lookup"><span data-stu-id="18870-218">For any field that has a suffix of \_s, \_d, or \_g in hello name, change hello first character toolower case</span></span>
  - <span data-ttu-id="18870-219">För alla fält som har suffixet \_o i namn hello data delas upp i enskilda fält baserat på hello kapslade fältnamn.</span><span class="sxs-lookup"><span data-stu-id="18870-219">For any field that has a suffix of \_o in name, hello data is split into individual fields based on hello nested field names.</span></span> <span data-ttu-id="18870-220">Till exempel lagras hello UPN för hello anroparen i ett fält`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span><span class="sxs-lookup"><span data-stu-id="18870-220">For example, hello UPN of hello caller is stored in a field `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span></span>
   - <span data-ttu-id="18870-221">Fältet CallerIpAddress ändras tooCallerIPAddress</span><span class="sxs-lookup"><span data-stu-id="18870-221">Field CallerIpAddress changed tooCallerIPAddress</span></span>
   - <span data-ttu-id="18870-222">Fältet RemoteIPCountry finns inte längre</span><span class="sxs-lookup"><span data-stu-id="18870-222">Field RemoteIPCountry is no longer present</span></span>
4. <span data-ttu-id="18870-223">Ta bort hello *Key Vault Analytics (inaktuell)* lösning.</span><span class="sxs-lookup"><span data-stu-id="18870-223">Remove hello *Key Vault Analytics (Deprecated)* solution.</span></span> <span data-ttu-id="18870-224">Om du använder PowerShell använder du`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="18870-224">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span></span>

<span data-ttu-id="18870-225">Data som samlas in innan hello ändringen inte visas i hello ny lösning.</span><span class="sxs-lookup"><span data-stu-id="18870-225">Data collected before hello change is not visible in hello new solution.</span></span> <span data-ttu-id="18870-226">Du kan fortsätta tooquery för den här data med hjälp av hello gamla typ och fältnamn.</span><span class="sxs-lookup"><span data-stu-id="18870-226">You can continue tooquery for this data using hello old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="18870-227">Felsökning</span><span class="sxs-lookup"><span data-stu-id="18870-227">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="18870-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18870-228">Next steps</span></span>
* <span data-ttu-id="18870-229">Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerade Azure Key Vault-data.</span><span class="sxs-lookup"><span data-stu-id="18870-229">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Azure Key Vault data.</span></span>
