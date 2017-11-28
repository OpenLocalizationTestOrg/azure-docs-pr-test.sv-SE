---
title: aaaOverview av Azure diagnostikloggar | Microsoft Docs
description: "Lär dig vad Azure diagnostikloggar är och hur du kan använda dem toounderstand händelser som inträffar inom en Azure-resurs."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem; magoedte
ms.openlocfilehash: e38991c540626b4bb5b5b9a995276881ee58f368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a><span data-ttu-id="9cf5d-103">Samla in och använda loggdata från resurserna i Azure</span><span class="sxs-lookup"><span data-stu-id="9cf5d-103">Collect and consume log data from your Azure resources</span></span>

## <a name="what-are-azure-resource-diagnostic-logs"></a><span data-ttu-id="9cf5d-104">Vad är Azure-resurs diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="9cf5d-104">What are Azure resource diagnostic logs</span></span>
<span data-ttu-id="9cf5d-105">**Azure resursnivå diagnostikloggar** är loggar orsakat av en resurs som innehåller omfattande, ofta data om hello driften av den här resursen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-105">**Azure resource-level diagnostic logs** are logs emitted by a resource that provide rich, frequent data about hello operation of that resource.</span></span> <span data-ttu-id="9cf5d-106">hello innehållet i de här loggarna varierar beroende på resurstypen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-106">hello content of these logs varies by resource type.</span></span> <span data-ttu-id="9cf5d-107">Till exempel finns Nätverkssäkerhetsgruppen regeln räknare och Key Vault granskningar två typer av loggar för resursen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-107">For example, Network Security Group rule counters and Key Vault audits are two categories of resource logs.</span></span>

<span data-ttu-id="9cf5d-108">Resursnivå diagnostikloggar skiljer sig från hello [aktivitetsloggen](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="9cf5d-108">Resource-level diagnostic logs differ from hello [Activity Log](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="9cf5d-109">hello aktivitetsloggen ger inblick i hello-åtgärder som utfördes på resurser i prenumerationen med Resource Manager, till exempel skapa en virtuell dator eller ta bort en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-109">hello Activity Log provides insight into hello operations that were performed on resources in your subscription using Resource Manager, for example, creating a virtual machine or deleting a logic app.</span></span> <span data-ttu-id="9cf5d-110">hello aktivitetsloggen är en prenumeration på objektnivå logg.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-110">hello Activity Log is a subscription-level log.</span></span> <span data-ttu-id="9cf5d-111">Resursnivå diagnostikloggar ger kunskaper om åtgärder som utförts i den här resursen, till exempel en hemlighet komma från ett Nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-111">Resource-level diagnostic logs provide insight into operations that were performed within that resource itself, for example, getting a secret from a Key Vault.</span></span>

<span data-ttu-id="9cf5d-112">Resursnivå diagnostikloggar också skilja sig från diagnostikloggar för gäst-OS-nivå.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-112">Resource-level diagnostic logs also differ from guest OS-level diagnostic logs.</span></span> <span data-ttu-id="9cf5d-113">Gästoperativsystem diagnostikloggar är de som samlats in av en agent som körs i en virtuell dator eller andra stöd för resurstypen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-113">Guest OS diagnostic logs are those collected by an agent running inside of a virtual machine or other supported resource type.</span></span> <span data-ttu-id="9cf5d-114">Resursnivå diagnostikloggar kräver ingen agent och avbilda resurs-specifik information från hello Azure-plattformen, medan gäst-OS-nivå diagnostikloggar samla in data från hello operativsystem och program som körs på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-114">Resource-level diagnostic logs require no agent and capture resource-specific data from hello Azure platform itself, while guest OS-level diagnostic logs capture data from hello operating system and applications running on a virtual machine.</span></span>

<span data-ttu-id="9cf5d-115">Inte alla resurser som stöder hello ny typ av resurs diagnostikloggar som beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-115">Not all resources support hello new type of resource diagnostic logs described here.</span></span> <span data-ttu-id="9cf5d-116">Den här artikeln innehåller en lista över avsnitt som vilka resurstyper stöder hello nya resursnivå diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-116">This article contains a section listing which resource types support hello new resource-level diagnostic logs.</span></span>

![<span data-ttu-id="9cf5d-117">Resursen diagnostik loggar jämfört med andra typer av loggar</span><span class="sxs-lookup"><span data-stu-id="9cf5d-117">Resource diagnostics logs vs other types of logs</span></span> ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a><span data-ttu-id="9cf5d-118">Vad du kan göra med resursnivå diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="9cf5d-118">What you can do with resource-level diagnostic logs</span></span>
<span data-ttu-id="9cf5d-119">Här följer några av hello saker du kan göra med resurs diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-119">Here are some of hello things you can do with resource diagnostic logs:</span></span>

![Logiska placeringen av resursen diagnostikloggar](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* <span data-ttu-id="9cf5d-121">Spara dem tooa [ **Lagringskonto** ](monitoring-archive-diagnostic-logs.md) för granskning eller Manuell kontroll.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-121">Save them tooa [**Storage Account**](monitoring-archive-diagnostic-logs.md) for auditing or manual inspection.</span></span> <span data-ttu-id="9cf5d-122">Du kan ange hello kvarhållning tid (i dagar) med **diagnostiska resursinställningar**.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-122">You can specify hello retention time (in days) using **resource diagnostic settings**.</span></span>
* <span data-ttu-id="9cf5d-123">[Strömma dem för**Händelsehubbar** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) för införandet av en tjänst från tredje part eller anpassade analytics lösning, till exempel PowerBI.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-123">[Stream them too**Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) for ingestion by a third-party service or custom analytics solution such as PowerBI.</span></span>
* <span data-ttu-id="9cf5d-124">Analysera dem med [OMS logganalys](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="9cf5d-124">Analyze them with [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)</span></span>

<span data-ttu-id="9cf5d-125">Du kan använda ett lagringskonto eller Händelsehubbar namnområde som inte är i hello samma prenumeration som hello en avger loggar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-125">You can use a storage account or Event Hubs namespace that is not in hello same subscription as hello one emitting logs.</span></span> <span data-ttu-id="9cf5d-126">hello-användare som konfigurerar hello inställning måste ha hello lämpliga RBAC åtkomst tooboth prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-126">hello user who configures hello setting must have hello appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="resource-diagnostic-settings"></a><span data-ttu-id="9cf5d-127">Diagnostikinställningar för resurs</span><span class="sxs-lookup"><span data-stu-id="9cf5d-127">Resource diagnostic settings</span></span>
<span data-ttu-id="9cf5d-128">Resursen diagnostikloggar för icke-beräkning resurser konfigureras med hjälp av diagnostikinställningar för resursen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-128">Resource diagnostic logs for non-Compute resources are configured using resource diagnostic settings.</span></span> <span data-ttu-id="9cf5d-129">**Diagnostiska resursinställningar** för en resurskontroll:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-129">**Resource diagnostic settings** for a resource control:</span></span>

* <span data-ttu-id="9cf5d-130">Där resursen diagnostikloggar och mått som skickas (Storage-konto, Event Hubs och/eller OMS logganalys).</span><span class="sxs-lookup"><span data-stu-id="9cf5d-130">Where resource diagnostic logs and metrics are sent (Storage Account, Event Hubs, and/or OMS Log Analytics).</span></span>
* <span data-ttu-id="9cf5d-131">Vilka kategorier loggen skickas och om mått data skickas också.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-131">Which log categories are sent and whether metric data is also sent.</span></span>
* <span data-ttu-id="9cf5d-132">Hur länge varje logg kategori ska behållas i ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9cf5d-132">How long each log category should be retained in a storage account</span></span>
    - <span data-ttu-id="9cf5d-133">En kvarhållning av noll dagar innebär loggar behålls alltid.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-133">A retention of zero days means logs are kept forever.</span></span> <span data-ttu-id="9cf5d-134">Annars kan hello värdet vara valfritt antal dagar mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-134">Otherwise, hello value can be any number of days between 1 and 2147483647.</span></span>
    - <span data-ttu-id="9cf5d-135">Om bevarandeprinciper har angetts men lagring loggar i ett Storage-konto har inaktiverats (till exempel om endast Händelsehubbar eller OMS-alternativen är markerade), har hello bevarandeprinciper ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-135">If retention policies are set but storing logs in a Storage Account is disabled (for example, if only Event Hubs or OMS options are selected), hello retention policies have no effect.</span></span>
    - <span data-ttu-id="9cf5d-136">Bevarandeprinciper tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip tas bort.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-136">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy are deleted.</span></span> <span data-ttu-id="9cf5d-137">Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-137">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted.</span></span>

<span data-ttu-id="9cf5d-138">Dessa inställningar konfigureras enkelt via hello diagnostikinställningarna för en resurs i hello Azure-portalen, via Azure PowerShell och CLI-kommandon eller via hello [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="9cf5d-138">These settings are easily configured via hello diagnostic settings for a resource in hello Azure portal, via Azure PowerShell and CLI commands, or via hello [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="9cf5d-139">Diagnostikloggar och mått för från hello gäst OS lager av beräkning resurser (till exempel virtuella datorer eller Service Fabric) Använd [en separat mekanism för konfiguration och val av utdata](../azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="9cf5d-139">Diagnostic logs and metrics for from hello guest OS layer of Compute resources (for example, VMs or Service Fabric) use [a separate mechanism for configuration and selection of outputs](../azure-diagnostics.md).</span></span>
>
>

## <a name="how-tooenable-collection-of-resource-diagnostic-logs"></a><span data-ttu-id="9cf5d-140">Hur tooenable samling resurs diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="9cf5d-140">How tooenable collection of resource diagnostic logs</span></span>
<span data-ttu-id="9cf5d-141">Samling av resursen diagnostikloggar kan aktiveras [som en del av att skapa en resurs i en Resource Manager-mall](./monitoring-enable-diagnostic-logs-using-template.md) eller när en resurs har skapats från den här resursen sida i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-141">Collection of resource diagnostic logs can be enabled [as part of creating a resource in a Resource Manager template](./monitoring-enable-diagnostic-logs-using-template.md) or after a resource is created from that resource's page in hello portal.</span></span> <span data-ttu-id="9cf5d-142">Du kan också aktivera samlingen när som helst via Azure PowerShell eller CLI-kommandon, eller använder hello Azure övervakaren REST API.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-142">You can also enable collection at any point using Azure PowerShell or CLI commands, or using hello Azure Monitor REST API.</span></span>

> [!TIP]
> <span data-ttu-id="9cf5d-143">Dessa instruktioner gäller kanske inte direkt tooevery resurs.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-143">These instructions may not apply directly tooevery resource.</span></span> <span data-ttu-id="9cf5d-144">Finns hello schemat länkarna längst ned hello i den här sidan toounderstand särskilda åtgärder som gäller toocertain resurstyper.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-144">See hello schema links at hello bottom of this page toounderstand special steps that may apply toocertain resource types.</span></span>
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-hello-portal"></a><span data-ttu-id="9cf5d-145">Aktivera insamling av resursen diagnostikloggar hello-portalen</span><span class="sxs-lookup"><span data-stu-id="9cf5d-145">Enable collection of resource diagnostic logs in hello portal</span></span>
<span data-ttu-id="9cf5d-146">Du kan aktivera insamling av resursen diagnostikloggar i hello Azure portal när en resurs har skapats genom att gå tooa specifik resurs eller genom att gå tooAzure övervakaren.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-146">You can enable collection of resource diagnostic logs in hello Azure portal after a resource has been created either by going tooa specific resource or by navigating tooAzure Monitor.</span></span> <span data-ttu-id="9cf5d-147">tooenable via Azure-Monitor:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-147">tooenable this via Azure Monitor:</span></span>

1. <span data-ttu-id="9cf5d-148">I hello [Azure-portalen](http://portal.azure.com), navigera tooAzure övervakaren och klicka på **diagnostikinställningar**</span><span class="sxs-lookup"><span data-stu-id="9cf5d-148">In hello [Azure portal](http://portal.azure.com), navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="9cf5d-150">Du kan också hello-filterlista av resursgruppen eller resursen, och klicka sedan på hello resursen som du vill att tooset en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-150">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="9cf5d-151">Om det finns inga inställningar på hello resursen som du har valt, kan du ange toocreate en inställning.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-151">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="9cf5d-152">Klicka på ”Aktivera diagnostik”.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-152">Click "Turn on diagnostics."</span></span>

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="9cf5d-154">Om det finns befintliga inställningarna på hello resurs, visas en lista över inställningar som redan har konfigurerats på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-154">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="9cf5d-155">Klicka på ”Lägg till diagnostikinställningen”.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-155">Click "Add diagnostic setting."</span></span>

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="9cf5d-157">Ge din ange ett namn, hello kryssrutorna för varje mål toowhich du gillar toosend data och konfigurera vilken resurs som används för varje mål.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-157">Give your setting a name, check hello boxes for each destination toowhich you would like toosend data, and configure which resource is used for each destination.</span></span> <span data-ttu-id="9cf5d-158">Du kan också ange ett antal dagar tooretain loggarna för med hello **bevarande (dagar)** skjutreglage (endast tillämpliga toohello konto lagringsplats).</span><span class="sxs-lookup"><span data-stu-id="9cf5d-158">Optionally, set a number of days tooretain these logs by using hello **Retention (days)** sliders (only applicable toohello storage account destination).</span></span> <span data-ttu-id="9cf5d-159">En kvarhållning av noll dagar lagrar hello loggar under obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-159">A retention of zero days stores hello logs indefinitely.</span></span>
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="9cf5d-161">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-161">Click **Save**.</span></span>

<span data-ttu-id="9cf5d-162">Efter en liten stund angetts hello nya visas inställningen i din lista över inställningar för den här resursen och diagnostikloggar skickas toohello mål som nya händelsedata genereras.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-162">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are sent toohello specified destinations as soon as new event data is generated.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a><span data-ttu-id="9cf5d-163">Aktivera insamling av resursen diagnostikloggar via PowerShell</span><span class="sxs-lookup"><span data-stu-id="9cf5d-163">Enable collection of resource diagnostic logs via PowerShell</span></span>
<span data-ttu-id="9cf5d-164">tooenable samling resurs diagnostikloggar via Azure PowerShell, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-164">tooenable collection of resource diagnostic logs via Azure PowerShell, use hello following commands:</span></span>

<span data-ttu-id="9cf5d-165">tooenable lagring av diagnostiska loggar i ett lagringskonto, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-165">tooenable storage of diagnostic logs in a storage account, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

<span data-ttu-id="9cf5d-166">hello lagring konto-ID är hello resurs-ID för hello storage-konto toowhich du vill toosend hello loggar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-166">hello storage account ID is hello resource ID for hello storage account toowhich you want toosend hello logs.</span></span>

<span data-ttu-id="9cf5d-167">tooenable strömning av diagnostikloggar tooan händelsehubb, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-167">tooenable streaming of diagnostic logs tooan event hub, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

<span data-ttu-id="9cf5d-168">regel-ID för hello service bus är en sträng med formatet: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-168">hello service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="9cf5d-169">tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-169">tooenable sending of diagnostic logs tooa Log Analytics workspace, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
```

<span data-ttu-id="9cf5d-170">Du kan hämta hello resurs-ID för logganalys-arbetsytan med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-170">You can obtain hello resource ID of your Log Analytics workspace using hello following command:</span></span>

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

<span data-ttu-id="9cf5d-171">Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-171">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a><span data-ttu-id="9cf5d-172">Aktivera insamling av resursen diagnostikloggar via CLI</span><span class="sxs-lookup"><span data-stu-id="9cf5d-172">Enable collection of resource diagnostic logs via CLI</span></span>
<span data-ttu-id="9cf5d-173">tooenable samling resurs diagnostikloggar via hello Azure CLI, använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-173">tooenable collection of resource diagnostic logs via hello Azure CLI, use hello following commands:</span></span>

<span data-ttu-id="9cf5d-174">tooenable lagring av diagnostiska loggar i ett Lagringskonto, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-174">tooenable storage of diagnostic logs in a Storage Account, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

<span data-ttu-id="9cf5d-175">hello lagring konto-ID är hello resurs-ID för hello storage-konto toowhich du vill toosend hello loggar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-175">hello storage account ID is hello resource ID for hello storage account toowhich you want toosend hello logs.</span></span>

<span data-ttu-id="9cf5d-176">tooenable strömning av diagnostikloggar tooan händelsehubb, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-176">tooenable streaming of diagnostic logs tooan event hub, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

<span data-ttu-id="9cf5d-177">regel-ID för hello service bus är en sträng med formatet: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-177">hello service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="9cf5d-178">tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9cf5d-178">tooenable sending of diagnostic logs tooa Log Analytics workspace, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
```

<span data-ttu-id="9cf5d-179">Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-179">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a><span data-ttu-id="9cf5d-180">Aktivera insamling av resursen diagnostikloggar via REST API</span><span class="sxs-lookup"><span data-stu-id="9cf5d-180">Enable collection of resource diagnostic logs via REST API</span></span>
<span data-ttu-id="9cf5d-181">toochange diagnostikinställningar med hello Azure övervakaren REST API finns [dokumentet](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="9cf5d-181">toochange diagnostic settings using hello Azure Monitor REST API, see [this document](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span>

## <a name="manage-resource-diagnostic-settings-in-hello-portal"></a><span data-ttu-id="9cf5d-182">Hantera diagnostiska resursinställningar hello-portalen</span><span class="sxs-lookup"><span data-stu-id="9cf5d-182">Manage resource diagnostic settings in hello portal</span></span>
<span data-ttu-id="9cf5d-183">Se till att alla dina resurser har ställts in för diagnostikinställningar.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-183">Ensure that all of your resources are set up with diagnostic settings.</span></span> <span data-ttu-id="9cf5d-184">Navigera för**övervakaren** i hello portalen och öppna **diagnostikinställningar**.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-184">Navigate too**Monitor** in hello portal and open **Diagnostic settings**.</span></span>

![Den diagnostiska loggar bladet i hello portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

<span data-ttu-id="9cf5d-186">Du kan ha tooclick ”fler tjänster” toofind hello övervakaren avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-186">You may have tooclick "More services" toofind hello Monitor section.</span></span>

<span data-ttu-id="9cf5d-187">Här kan du visa och filtrera alla resurser som stöder diagnostikinställningar toosee om de har aktiverat diagnostik.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-187">Here you can view and filter all resources that support diagnostic settings toosee if they have diagnostics enabled.</span></span> <span data-ttu-id="9cf5d-188">Du kan också detaljnivån toosee om flera inställningar konfigureras för en resurs och kontrollera vilka storage-konto, Händelsehubbar namnområde och/eller logganalys-arbetsytan som data flödar till.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-188">You can also drill down toosee if multiple settings are set on a resource and check which storage account, Event Hubs namespace, and/or Log Analytics workspace that data are flowing to.</span></span>

![Den diagnostiska loggar resulterar i portalen](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

<span data-ttu-id="9cf5d-190">Lägga till en diagnostikinställningen öppnas hello diagnostikinställningar för vyn, där du kan aktivera, inaktivera eller ändra dina diagnostikinställningar för hello markerat resurs.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-190">Adding a diagnostic setting brings up hello Diagnostic Settings view, where you can enable, disable, or modify your diagnostic settings for hello selected resource.</span></span>

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a><span data-ttu-id="9cf5d-191">Tjänster som stöds, kategorier och scheman för resource diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="9cf5d-191">Supported services, categories, and schemas for resource diagnostic logs</span></span>
<span data-ttu-id="9cf5d-192">[Finns den här artikeln](monitoring-diagnostic-logs-schema.md) för en fullständig lista över tjänster som stöds och hello loggen kategorier och scheman som används av tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="9cf5d-192">[See this article](monitoring-diagnostic-logs-schema.md) for a complete list of supported services and hello log categories and schemas used by those services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cf5d-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9cf5d-193">Next steps</span></span>

* [<span data-ttu-id="9cf5d-194">Strömma resurs diagnostikloggar för**Händelsehubbar**</span><span class="sxs-lookup"><span data-stu-id="9cf5d-194">Stream resource diagnostic logs too**Event Hubs**</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="9cf5d-195">Ändra resurs diagnostikinställningar med hello Azure övervakaren REST API</span><span class="sxs-lookup"><span data-stu-id="9cf5d-195">Change resource diagnostic settings using hello Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [<span data-ttu-id="9cf5d-196">Analysera loggar från Azure storage med logganalys</span><span class="sxs-lookup"><span data-stu-id="9cf5d-196">Analyze logs from Azure storage with Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
