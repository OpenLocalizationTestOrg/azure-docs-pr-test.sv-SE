---
title: aaaArchive Azure diagnostikloggar | Microsoft Docs
description: "Lär dig hur tooarchive din Azure diagnostikloggar för långsiktig kvarhållning i ett lagringskonto."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: bc9edbd3a649023a728b7fe77130dba2b6e6370d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="archive-azure-diagnostic-logs"></a><span data-ttu-id="3f2aa-103">Arkivera Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="3f2aa-103">Archive Azure Diagnostic Logs</span></span>
<span data-ttu-id="3f2aa-104">I den här artikeln visar vi hur du kan använda hello Azure-portalen PowerShell-Cmdlets, CLI eller REST API tooarchive din [Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-104">In this article, we show how you can use hello Azure portal, PowerShell Cmdlets, CLI, or REST API tooarchive your [Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md) in a storage account.</span></span> <span data-ttu-id="3f2aa-105">Det här alternativet är användbart om du vill att tooretain diagnostikloggar med ett valfritt bevarandeprincipen för granskning, statiska analys eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-105">This option is useful if you would like tooretain your diagnostic logs with an optional retention policy for audit, static analysis, or backup.</span></span> <span data-ttu-id="3f2aa-106">hello storage-konto har inte toobe i hello samma prenumeration som hello resurs avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-106">hello storage account does not have toobe in hello same subscription as hello resource emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f2aa-107">Krav</span><span class="sxs-lookup"><span data-stu-id="3f2aa-107">Prerequisites</span></span>
<span data-ttu-id="3f2aa-108">Innan du börjar behöver du för[skapa ett lagringskonto](../storage/storage-create-storage-account.md) toowhich som du kan arkivera dina diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-108">Before you begin, you need too[create a storage account](../storage/storage-create-storage-account.md) toowhich you can archive your diagnostic logs.</span></span> <span data-ttu-id="3f2aa-109">Vi rekommenderar starkt att du inte använder ett befintligt lagringskonto som har andra, icke-övervakning data som lagras i den så att du bättre kan styra åtkomst till toomonitoring data.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access toomonitoring data.</span></span> <span data-ttu-id="3f2aa-110">Men om du arkiverar även dina aktivitetsloggen och diagnostik mått tooa storage-konto, kan det vara klokt toouse detta lagringskonto för dina diagnostikloggar samt tookeep alla övervakningsdata på en central plats.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-110">However, if you are also archiving your Activity Log and diagnostic metrics tooa storage account, it may make sense toouse that storage account for your diagnostic logs as well tookeep all monitoring data in a central location.</span></span> <span data-ttu-id="3f2aa-111">hello storage-konto du använder måste vara ett allmänt lagringskonto inte ett blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-111">hello storage account you use must be a general purpose storage account, not a blob storage account.</span></span>

## <a name="diagnostic-settings"></a><span data-ttu-id="3f2aa-112">Diagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="3f2aa-112">Diagnostic settings</span></span>
<span data-ttu-id="3f2aa-113">tooarchive dina diagnostikloggar med hjälp av hello metoderna nedan, som du ställer in en **diagnostikinställningen** för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-113">tooarchive your diagnostic logs using any of hello methods below, you set a **diagnostic setting** for a particular resource.</span></span> <span data-ttu-id="3f2aa-114">Diagnostikinställningen för en resurs definierar hello kategorier av loggar och mätvärden skickas tooa destination (storage-konto, Händelsehubbar namnområde eller logganalys).</span><span class="sxs-lookup"><span data-stu-id="3f2aa-114">A diagnostic setting for a resource defines hello categories of logs and metric data sent tooa destination (storage account, Event Hubs namespace, or Log Analytics).</span></span> <span data-ttu-id="3f2aa-115">Den definierar även hello bevarandeprincip (antal dagar tooretain) för händelser för varje logg kategori och mått data som lagras i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-115">It also defines hello retention policy (number of days tooretain) for events of each log category and metric data stored in a storage account.</span></span> <span data-ttu-id="3f2aa-116">Om en bevarandeprincip anges toozero lagras händelser för log kategorin på obestämd tid (d.v.s. toosay, oändligt).</span><span class="sxs-lookup"><span data-stu-id="3f2aa-116">If a retention policy is set toozero, events for that log category are stored indefinitely (that is toosay, forever).</span></span> <span data-ttu-id="3f2aa-117">En bevarandeprincip kan annars vara valfritt antal dagar mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-117">A retention policy can otherwise be any number of days between 1 and 2147483647.</span></span> <span data-ttu-id="3f2aa-118">[Du kan läsa mer om diagnostikinställningar här](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="3f2aa-118">[You can read more about diagnostic settings here](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span> <span data-ttu-id="3f2aa-119">Bevarandeprinciper är tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip kommer att tas bort.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-119">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy will be deleted.</span></span> <span data-ttu-id="3f2aa-120">Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort</span><span class="sxs-lookup"><span data-stu-id="3f2aa-120">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted</span></span>

## <a name="archive-diagnostic-logs-using-hello-portal"></a><span data-ttu-id="3f2aa-121">Arkivera diagnostikloggar med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="3f2aa-121">Archive diagnostic logs using hello portal</span></span>
1. <span data-ttu-id="3f2aa-122">Navigera tooAzure Övervakare i hello-portalen och klicka på **diagnostikinställningar**</span><span class="sxs-lookup"><span data-stu-id="3f2aa-122">In hello portal, navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="3f2aa-124">Du kan också hello-filterlista av resursgruppen eller resursen, och klicka sedan på hello resursen som du vill att tooset en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-124">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="3f2aa-125">Om det finns inga inställningar på hello resursen som du har valt, kan du ange toocreate en inställning.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-125">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="3f2aa-126">Klicka på ”Aktivera diagnostik”.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-126">Click "Turn on diagnostics."</span></span>

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="3f2aa-128">Om det finns befintliga inställningarna på hello resurs, visas en lista över inställningar som redan har konfigurerats på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-128">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="3f2aa-129">Klicka på ”Lägg till diagnostikinställningen”.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-129">Click "Add diagnostic setting."</span></span>

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="3f2aa-131">Ge ange ett namn och kryssrutan för hello **exportera tooStorage konto**, Välj ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-131">Give your setting a name and check hello box for **Export tooStorage Account**, then select a storage account.</span></span> <span data-ttu-id="3f2aa-132">Du kan också ange ett antal dagar tooretain loggarna för med hello **bevarande (dagar)** skjutreglagen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-132">Optionally, set a number of days tooretain these logs by using hello **Retention (days)** sliders.</span></span> <span data-ttu-id="3f2aa-133">En kvarhållning av noll dagar lagrar hello loggar under obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-133">A retention of zero days stores hello logs indefinitely.</span></span>
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="3f2aa-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-135">Click **Save**.</span></span>

<span data-ttu-id="3f2aa-136">Efter en liten stund hello nya inställningen som visas i din lista över inställningar för den här resursen och diagnostikloggar är arkiverade toothat lagring kontot så snart nya händelsedata genereras.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-136">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are archived toothat storage account as soon as new event data is generated.</span></span>

## <a name="archive-diagnostic-logs-via-azure-powershell"></a><span data-ttu-id="3f2aa-137">Arkivera diagnostikloggar via Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f2aa-137">Archive diagnostic logs via Azure PowerShell</span></span>
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| <span data-ttu-id="3f2aa-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3f2aa-138">Property</span></span> | <span data-ttu-id="3f2aa-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="3f2aa-139">Required</span></span> | <span data-ttu-id="3f2aa-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3f2aa-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f2aa-141">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="3f2aa-141">ResourceId</span></span> |<span data-ttu-id="3f2aa-142">Ja</span><span class="sxs-lookup"><span data-stu-id="3f2aa-142">Yes</span></span> |<span data-ttu-id="3f2aa-143">Resurs-ID för hello resursen som du vill tooset en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-143">Resource ID of hello resource on which you want tooset a diagnostic setting.</span></span> |
| <span data-ttu-id="3f2aa-144">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="3f2aa-144">StorageAccountId</span></span> |<span data-ttu-id="3f2aa-145">Nej</span><span class="sxs-lookup"><span data-stu-id="3f2aa-145">No</span></span> |<span data-ttu-id="3f2aa-146">Resurs-ID för hello Lagringskonto toowhich diagnostikloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-146">Resource ID of hello Storage Account toowhich Diagnostic Logs should be saved.</span></span> |
| <span data-ttu-id="3f2aa-147">Kategorier</span><span class="sxs-lookup"><span data-stu-id="3f2aa-147">Categories</span></span> |<span data-ttu-id="3f2aa-148">Nej</span><span class="sxs-lookup"><span data-stu-id="3f2aa-148">No</span></span> |<span data-ttu-id="3f2aa-149">Kommaavgränsad lista över loggen kategorier tooenable.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-149">Comma-separated list of log categories tooenable.</span></span> |
| <span data-ttu-id="3f2aa-150">Enabled</span><span class="sxs-lookup"><span data-stu-id="3f2aa-150">Enabled</span></span> |<span data-ttu-id="3f2aa-151">Ja</span><span class="sxs-lookup"><span data-stu-id="3f2aa-151">Yes</span></span> |<span data-ttu-id="3f2aa-152">Booleskt värde som anger om diagnostik är aktiverade eller inaktiverade på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-152">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |
| <span data-ttu-id="3f2aa-153">RetentionEnabled</span><span class="sxs-lookup"><span data-stu-id="3f2aa-153">RetentionEnabled</span></span> |<span data-ttu-id="3f2aa-154">Nej</span><span class="sxs-lookup"><span data-stu-id="3f2aa-154">No</span></span> |<span data-ttu-id="3f2aa-155">Booleskt värde som anger om en bevarandeprincip är aktiverade på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-155">Boolean indicating if a retention policy are enabled on this resource.</span></span> |
| <span data-ttu-id="3f2aa-156">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="3f2aa-156">RetentionInDays</span></span> |<span data-ttu-id="3f2aa-157">Nej</span><span class="sxs-lookup"><span data-stu-id="3f2aa-157">No</span></span> |<span data-ttu-id="3f2aa-158">Antal dagar som händelser ska behållas mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-158">Number of days for which events should be retained between 1 and 2147483647.</span></span> <span data-ttu-id="3f2aa-159">Värdet noll lagrar hello loggar under obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-159">A value of zero stores hello logs indefinitely.</span></span> |

## <a name="archive-diagnostic-logs-via-hello-cross-platform-cli"></a><span data-ttu-id="3f2aa-160">Arkivera diagnostikloggar via hello plattformsoberoende CLI</span><span class="sxs-lookup"><span data-stu-id="3f2aa-160">Archive diagnostic logs via hello Cross-Platform CLI</span></span>
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| <span data-ttu-id="3f2aa-161">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3f2aa-161">Property</span></span> | <span data-ttu-id="3f2aa-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="3f2aa-162">Required</span></span> | <span data-ttu-id="3f2aa-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3f2aa-163">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f2aa-164">resourceId</span><span class="sxs-lookup"><span data-stu-id="3f2aa-164">resourceId</span></span> |<span data-ttu-id="3f2aa-165">Ja</span><span class="sxs-lookup"><span data-stu-id="3f2aa-165">Yes</span></span> |<span data-ttu-id="3f2aa-166">Resurs-ID för hello resursen som du vill tooset en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-166">Resource ID of hello resource on which you want tooset a diagnostic setting.</span></span> |
| <span data-ttu-id="3f2aa-167">storageId</span><span class="sxs-lookup"><span data-stu-id="3f2aa-167">storageId</span></span> |<span data-ttu-id="3f2aa-168">Nej</span><span class="sxs-lookup"><span data-stu-id="3f2aa-168">No</span></span> |<span data-ttu-id="3f2aa-169">Resurs-ID för Lagringskontot hello toowhich diagnostikloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-169">Resource ID of hello Storage Account toowhich diagnostic logs should be saved.</span></span> |
| <span data-ttu-id="3f2aa-170">Kategorier</span><span class="sxs-lookup"><span data-stu-id="3f2aa-170">categories</span></span> |<span data-ttu-id="3f2aa-171">Nej</span><span class="sxs-lookup"><span data-stu-id="3f2aa-171">No</span></span> |<span data-ttu-id="3f2aa-172">Kommaavgränsad lista över loggen kategorier tooenable.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-172">Comma-separated list of log categories tooenable.</span></span> |
| <span data-ttu-id="3f2aa-173">aktiverad</span><span class="sxs-lookup"><span data-stu-id="3f2aa-173">enabled</span></span> |<span data-ttu-id="3f2aa-174">Ja</span><span class="sxs-lookup"><span data-stu-id="3f2aa-174">Yes</span></span> |<span data-ttu-id="3f2aa-175">Booleskt värde som anger om diagnostik är aktiverade eller inaktiverade på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-175">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |

## <a name="archive-diagnostic-logs-via-hello-rest-api"></a><span data-ttu-id="3f2aa-176">Arkivera diagnostikloggar via hello REST API</span><span class="sxs-lookup"><span data-stu-id="3f2aa-176">Archive diagnostic logs via hello REST API</span></span>
<span data-ttu-id="3f2aa-177">[Det här dokumentet finns](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) information om hur du ställer in en diagnostikinställningen med hello Azure övervakaren REST API.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-177">[See this document](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) for information on how you can set up a diagnostic setting using hello Azure Monitor REST API.</span></span>

## <a name="schema-of-diagnostic-logs-in-hello-storage-account"></a><span data-ttu-id="3f2aa-178">Schemat för diagnostikloggar i hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="3f2aa-178">Schema of diagnostic logs in hello storage account</span></span>
<span data-ttu-id="3f2aa-179">När du har konfigurerat arkivering, skapas en lagringsbehållare i hello storage-konto när en händelse inträffar i en av hello loggen kategorier som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-179">Once you have set up archival, a storage container is created in hello storage account as soon as an event occurs in one of hello log categories you have enabled.</span></span> <span data-ttu-id="3f2aa-180">Hej blobbar i behållaren hello följa hello samma format över diagnostikloggar och hello aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-180">hello blobs within hello container follow hello same format across Diagnostic Logs and hello Activity Log.</span></span> <span data-ttu-id="3f2aa-181">hello strukturen för de här blobbar är:</span><span class="sxs-lookup"><span data-stu-id="3f2aa-181">hello structure of these blobs is:</span></span>

> <span data-ttu-id="3f2aa-182">insikter - loggar-{kategori loggnamn} / resourceId = / PRENUMERATIONER / {prenumerations-ID} /RESOURCEGROUPS/ {resursgruppens namn} /PROVIDERS/ {resurs providernamn} / {resurstyp} / {resursnamn} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="3f2aa-182">insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="3f2aa-183">Eller fler bara</span><span class="sxs-lookup"><span data-stu-id="3f2aa-183">Or, more simply,</span></span>

> <span data-ttu-id="3f2aa-184">insikter - loggar-{kategori loggnamn} / resourceId = / {resursens Id} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="3f2aa-184">insights-logs-{log category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="3f2aa-185">Till exempel kan en blobbnamnet vara:</span><span class="sxs-lookup"><span data-stu-id="3f2aa-185">For example, a blob name might be:</span></span>

> <span data-ttu-id="3f2aa-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="3f2aa-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="3f2aa-187">Varje PT1H.json blobb innehåller en JSON-blob av händelser som inträffade inom hello timme som anges i hello blob-URL (till exempel h = 12).</span><span class="sxs-lookup"><span data-stu-id="3f2aa-187">Each PT1H.json blob contains a JSON blob of events that occurred within hello hour specified in hello blob URL (for example, h=12).</span></span> <span data-ttu-id="3f2aa-188">Under hello finns timme är händelser tillagda toohello PT1H.json filen när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-188">During hello present hour, events are appended toohello PT1H.json file as they occur.</span></span> <span data-ttu-id="3f2aa-189">Hej minuten (m = 00) är alltid 00, eftersom diagnostiska logghändelser delas upp i enskilda blobbar per timme.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-189">hello minute value (m=00) is always 00, since diagnostic log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="3f2aa-190">I hello PT1H.json filen lagras varje händelse i hello ”innehåller” matris, efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="3f2aa-190">Within hello PT1H.json file, each event is stored in hello “records” array, following this format:</span></span>

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| <span data-ttu-id="3f2aa-191">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="3f2aa-191">Element name</span></span> | <span data-ttu-id="3f2aa-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3f2aa-192">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3f2aa-193">time</span><span class="sxs-lookup"><span data-stu-id="3f2aa-193">time</span></span> |<span data-ttu-id="3f2aa-194">Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-194">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="3f2aa-195">resourceId</span><span class="sxs-lookup"><span data-stu-id="3f2aa-195">resourceId</span></span> |<span data-ttu-id="3f2aa-196">Resurs-ID för hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-196">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="3f2aa-197">operationName</span><span class="sxs-lookup"><span data-stu-id="3f2aa-197">operationName</span></span> |<span data-ttu-id="3f2aa-198">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-198">Name of hello operation.</span></span> |
| <span data-ttu-id="3f2aa-199">category</span><span class="sxs-lookup"><span data-stu-id="3f2aa-199">category</span></span> |<span data-ttu-id="3f2aa-200">Loggen kategori hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-200">Log category of hello event.</span></span> |
| <span data-ttu-id="3f2aa-201">properties</span><span class="sxs-lookup"><span data-stu-id="3f2aa-201">properties</span></span> |<span data-ttu-id="3f2aa-202">En uppsättning `<Key, Value>` par (d.v.s. ordlista) som beskriver hello information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-202">Set of `<Key, Value>` pairs (i.e. Dictionary) describing hello details of hello event.</span></span> |

> [!NOTE]
> <span data-ttu-id="3f2aa-203">hello egenskaper och användning av dessa egenskaper kan variera beroende på hello resurs.</span><span class="sxs-lookup"><span data-stu-id="3f2aa-203">hello properties and usage of those properties can vary depending on hello resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="3f2aa-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f2aa-204">Next steps</span></span>
* [<span data-ttu-id="3f2aa-205">Ladda ned blobbar för analys</span><span class="sxs-lookup"><span data-stu-id="3f2aa-205">Download blobs for analysis</span></span>](../storage/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="3f2aa-206">Dataströmmen diagnostiska loggar tooan Händelsehubbar namnområde</span><span class="sxs-lookup"><span data-stu-id="3f2aa-206">Stream diagnostic logs tooan Event Hubs namespace</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="3f2aa-207">Läs mer om diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="3f2aa-207">Read more about diagnostic logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
