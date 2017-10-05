---
title: Arkivera Azure diagnostikloggar | Microsoft Docs
description: "Lär dig mer om att arkivera dina Azure diagnostikloggar för långsiktig kvarhållning i ett lagringskonto."
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
ms.openlocfilehash: dbc5f89001dcb6cd1ab061cb0a9632e4e5d2c1c7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="archive-azure-diagnostic-logs"></a><span data-ttu-id="df571-103">Arkivera Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="df571-103">Archive Azure Diagnostic Logs</span></span>
<span data-ttu-id="df571-104">I den här artikeln visar vi hur du kan använda Azure-portalen, PowerShell-Cmdlets, CLI eller REST API för att arkivera dina [Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="df571-104">In this article, we show how you can use the Azure portal, PowerShell Cmdlets, CLI, or REST API to archive your [Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md) in a storage account.</span></span> <span data-ttu-id="df571-105">Det här alternativet är användbart om du vill behålla dina diagnostikloggar med ett valfritt bevarandeprincipen för granskning, statiska analys eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="df571-105">This option is useful if you would like to retain your diagnostic logs with an optional retention policy for audit, static analysis, or backup.</span></span> <span data-ttu-id="df571-106">Storage-konto behöver inte finnas i samma prenumeration som resursen avger loggar så länge som den användare som konfigurerar inställningen har lämplig RBAC åtkomst till båda prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="df571-106">The storage account does not have to be in the same subscription as the resource emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df571-107">Krav</span><span class="sxs-lookup"><span data-stu-id="df571-107">Prerequisites</span></span>
<span data-ttu-id="df571-108">Innan du börjar måste du [skapa ett lagringskonto](../storage/storage-create-storage-account.md) som du kan arkivera dina diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="df571-108">Before you begin, you need to [create a storage account](../storage/storage-create-storage-account.md) to which you can archive your diagnostic logs.</span></span> <span data-ttu-id="df571-109">Vi rekommenderar starkt att du inte använder ett befintligt lagringskonto som har andra, icke-övervakning data som lagras i den så att du bättre kan styra åtkomsten till övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="df571-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access to monitoring data.</span></span> <span data-ttu-id="df571-110">Men om du arkiverar även din aktivitetsloggen och diagnostik mått till ett lagringskonto, kan det vara bra att använda detta lagringskonto för dina diagnostikloggar för att hålla alla övervakningsdata på en central plats.</span><span class="sxs-lookup"><span data-stu-id="df571-110">However, if you are also archiving your Activity Log and diagnostic metrics to a storage account, it may make sense to use that storage account for your diagnostic logs as well to keep all monitoring data in a central location.</span></span> <span data-ttu-id="df571-111">Storage-konto som du använder måste vara ett allmänt lagringskonto inte ett blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="df571-111">The storage account you use must be a general purpose storage account, not a blob storage account.</span></span>

## <a name="diagnostic-settings"></a><span data-ttu-id="df571-112">Diagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="df571-112">Diagnostic settings</span></span>
<span data-ttu-id="df571-113">Om du vill arkivera dina diagnostikloggar med någon av metoderna nedan kan du ange en **diagnostikinställningen** för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="df571-113">To archive your diagnostic logs using any of the methods below, you set a **diagnostic setting** for a particular resource.</span></span> <span data-ttu-id="df571-114">Diagnostikinställningen för en resurs definierar kategorier av loggar och mått data som skickas till ett mål (storage-konto, Händelsehubbar namnområde eller logganalys).</span><span class="sxs-lookup"><span data-stu-id="df571-114">A diagnostic setting for a resource defines the categories of logs and metric data sent to a destination (storage account, Event Hubs namespace, or Log Analytics).</span></span> <span data-ttu-id="df571-115">Den definierar även bevarandeprincip (antal dagar att behålla) för händelser för varje logg kategori och mått data som lagras i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="df571-115">It also defines the retention policy (number of days to retain) for events of each log category and metric data stored in a storage account.</span></span> <span data-ttu-id="df571-116">Om en bevarandeprincip har angetts till noll lagras händelser för den logg kategorin på obestämd tid (dvs, oändligt).</span><span class="sxs-lookup"><span data-stu-id="df571-116">If a retention policy is set to zero, events for that log category are stored indefinitely (that is to say, forever).</span></span> <span data-ttu-id="df571-117">En bevarandeprincip kan annars vara valfritt antal dagar mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="df571-117">A retention policy can otherwise be any number of days between 1 and 2147483647.</span></span> <span data-ttu-id="df571-118">[Du kan läsa mer om diagnostikinställningar här](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="df571-118">[You can read more about diagnostic settings here](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span> <span data-ttu-id="df571-119">Bevarandeprinciper är tillämpade per dag, så i slutet av dagen (UTC) loggar från den dagen är nu utöver kvarhållning princip kommer att tas bort.</span><span class="sxs-lookup"><span data-stu-id="df571-119">Retention policies are applied per-day, so at the end of a day (UTC), logs from the day that is now beyond the retention policy will be deleted.</span></span> <span data-ttu-id="df571-120">Till exempel om du har en bevarandeprincip på en dag skulle i början av dagen idag loggar från dag före igår tas bort</span><span class="sxs-lookup"><span data-stu-id="df571-120">For example, if you had a retention policy of one day, at the beginning of the day today the logs from the day before yesterday would be deleted</span></span>

## <a name="archive-diagnostic-logs-using-the-portal"></a><span data-ttu-id="df571-121">Arkivera diagnostikloggar med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="df571-121">Archive diagnostic logs using the portal</span></span>
1. <span data-ttu-id="df571-122">Gå till Azure-Monitor i portalen och klicka på **diagnostikinställningar**</span><span class="sxs-lookup"><span data-stu-id="df571-122">In the portal, navigate to Azure Monitor and click on **Diagnostic Settings**</span></span>

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="df571-124">Om du vill filtrera listan efter resursgrupp eller resurstyp, och klicka sedan på resursen som du vill ange en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="df571-124">Optionally filter the list by resource group or resource type, then click on the resource for which you would like to set a diagnostic setting.</span></span>

3. <span data-ttu-id="df571-125">Om det finns inga inställningar på resursen har du valt, uppmanas du för att skapa en inställning.</span><span class="sxs-lookup"><span data-stu-id="df571-125">If no settings exist on the resource you have selected, you are prompted to create a setting.</span></span> <span data-ttu-id="df571-126">Klicka på ”Aktivera diagnostik”.</span><span class="sxs-lookup"><span data-stu-id="df571-126">Click "Turn on diagnostics."</span></span>

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="df571-128">Om det finns befintliga inställningarna på resursen, visas en lista över inställningar som redan har konfigurerats på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="df571-128">If there are existing settings on the resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="df571-129">Klicka på ”Lägg till diagnostikinställningen”.</span><span class="sxs-lookup"><span data-stu-id="df571-129">Click "Add diagnostic setting."</span></span>

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="df571-131">Ge din ange ett namn och markera kryssrutan för **exportera till Lagringskontot**, Välj ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="df571-131">Give your setting a name and check the box for **Export to Storage Account**, then select a storage account.</span></span> <span data-ttu-id="df571-132">Du kan också ange ett antal dagar att behålla dessa loggar med hjälp av den **bevarande (dagar)** skjutreglagen.</span><span class="sxs-lookup"><span data-stu-id="df571-132">Optionally, set a number of days to retain these logs by using the **Retention (days)** sliders.</span></span> <span data-ttu-id="df571-133">En kvarhållning av noll dagar lagrar loggarna på obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="df571-133">A retention of zero days stores the logs indefinitely.</span></span>
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="df571-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="df571-135">Click **Save**.</span></span>

<span data-ttu-id="df571-136">Den nya inställningen visas i din lista över inställningar för den här resursen efter en liten stund och diagnostikloggar arkiveras till att lagringskontot när nya händelsedata genereras.</span><span class="sxs-lookup"><span data-stu-id="df571-136">After a few moments, the new setting appears in your list of settings for this resource, and diagnostic logs are archived to that storage account as soon as new event data is generated.</span></span>

## <a name="archive-diagnostic-logs-via-azure-powershell"></a><span data-ttu-id="df571-137">Arkivera diagnostikloggar via Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="df571-137">Archive diagnostic logs via Azure PowerShell</span></span>
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| <span data-ttu-id="df571-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="df571-138">Property</span></span> | <span data-ttu-id="df571-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="df571-139">Required</span></span> | <span data-ttu-id="df571-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df571-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df571-141">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="df571-141">ResourceId</span></span> |<span data-ttu-id="df571-142">Ja</span><span class="sxs-lookup"><span data-stu-id="df571-142">Yes</span></span> |<span data-ttu-id="df571-143">Resurs-ID för den resurs som du vill ange en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="df571-143">Resource ID of the resource on which you want to set a diagnostic setting.</span></span> |
| <span data-ttu-id="df571-144">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="df571-144">StorageAccountId</span></span> |<span data-ttu-id="df571-145">Nej</span><span class="sxs-lookup"><span data-stu-id="df571-145">No</span></span> |<span data-ttu-id="df571-146">Resurs-ID för det Lagringskonto där diagnostikloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="df571-146">Resource ID of the Storage Account to which Diagnostic Logs should be saved.</span></span> |
| <span data-ttu-id="df571-147">Kategorier</span><span class="sxs-lookup"><span data-stu-id="df571-147">Categories</span></span> |<span data-ttu-id="df571-148">Nej</span><span class="sxs-lookup"><span data-stu-id="df571-148">No</span></span> |<span data-ttu-id="df571-149">Kommaavgränsad lista över loggen kategorier för att aktivera.</span><span class="sxs-lookup"><span data-stu-id="df571-149">Comma-separated list of log categories to enable.</span></span> |
| <span data-ttu-id="df571-150">Enabled</span><span class="sxs-lookup"><span data-stu-id="df571-150">Enabled</span></span> |<span data-ttu-id="df571-151">Ja</span><span class="sxs-lookup"><span data-stu-id="df571-151">Yes</span></span> |<span data-ttu-id="df571-152">Booleskt värde som anger om diagnostik är aktiverade eller inaktiverade på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="df571-152">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |
| <span data-ttu-id="df571-153">RetentionEnabled</span><span class="sxs-lookup"><span data-stu-id="df571-153">RetentionEnabled</span></span> |<span data-ttu-id="df571-154">Nej</span><span class="sxs-lookup"><span data-stu-id="df571-154">No</span></span> |<span data-ttu-id="df571-155">Booleskt värde som anger om en bevarandeprincip är aktiverade på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="df571-155">Boolean indicating if a retention policy are enabled on this resource.</span></span> |
| <span data-ttu-id="df571-156">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="df571-156">RetentionInDays</span></span> |<span data-ttu-id="df571-157">Nej</span><span class="sxs-lookup"><span data-stu-id="df571-157">No</span></span> |<span data-ttu-id="df571-158">Antal dagar som händelser ska behållas mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="df571-158">Number of days for which events should be retained between 1 and 2147483647.</span></span> <span data-ttu-id="df571-159">Värdet noll lagrar loggarna på obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="df571-159">A value of zero stores the logs indefinitely.</span></span> |

## <a name="archive-diagnostic-logs-via-the-cross-platform-cli"></a><span data-ttu-id="df571-160">Arkivera diagnostikloggar via plattformsoberoende CLI</span><span class="sxs-lookup"><span data-stu-id="df571-160">Archive diagnostic logs via the Cross-Platform CLI</span></span>
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| <span data-ttu-id="df571-161">Egenskap</span><span class="sxs-lookup"><span data-stu-id="df571-161">Property</span></span> | <span data-ttu-id="df571-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="df571-162">Required</span></span> | <span data-ttu-id="df571-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df571-163">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df571-164">resourceId</span><span class="sxs-lookup"><span data-stu-id="df571-164">resourceId</span></span> |<span data-ttu-id="df571-165">Ja</span><span class="sxs-lookup"><span data-stu-id="df571-165">Yes</span></span> |<span data-ttu-id="df571-166">Resurs-ID för den resurs som du vill ange en diagnostikinställningen.</span><span class="sxs-lookup"><span data-stu-id="df571-166">Resource ID of the resource on which you want to set a diagnostic setting.</span></span> |
| <span data-ttu-id="df571-167">storageId</span><span class="sxs-lookup"><span data-stu-id="df571-167">storageId</span></span> |<span data-ttu-id="df571-168">Nej</span><span class="sxs-lookup"><span data-stu-id="df571-168">No</span></span> |<span data-ttu-id="df571-169">Resurs-ID för det Lagringskonto där diagnostikloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="df571-169">Resource ID of the Storage Account to which diagnostic logs should be saved.</span></span> |
| <span data-ttu-id="df571-170">Kategorier</span><span class="sxs-lookup"><span data-stu-id="df571-170">categories</span></span> |<span data-ttu-id="df571-171">Nej</span><span class="sxs-lookup"><span data-stu-id="df571-171">No</span></span> |<span data-ttu-id="df571-172">Kommaavgränsad lista över loggen kategorier för att aktivera.</span><span class="sxs-lookup"><span data-stu-id="df571-172">Comma-separated list of log categories to enable.</span></span> |
| <span data-ttu-id="df571-173">aktiverad</span><span class="sxs-lookup"><span data-stu-id="df571-173">enabled</span></span> |<span data-ttu-id="df571-174">Ja</span><span class="sxs-lookup"><span data-stu-id="df571-174">Yes</span></span> |<span data-ttu-id="df571-175">Booleskt värde som anger om diagnostik är aktiverade eller inaktiverade på den här resursen.</span><span class="sxs-lookup"><span data-stu-id="df571-175">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a><span data-ttu-id="df571-176">Arkivera diagnostikloggar via REST API</span><span class="sxs-lookup"><span data-stu-id="df571-176">Archive diagnostic logs via the REST API</span></span>
<span data-ttu-id="df571-177">[Det här dokumentet finns](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) information om hur du ställer in en diagnostikinställningen med hjälp av REST API för Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="df571-177">[See this document](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) for information on how you can set up a diagnostic setting using the Azure Monitor REST API.</span></span>

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a><span data-ttu-id="df571-178">Schemat för diagnostikloggar i storage-konto</span><span class="sxs-lookup"><span data-stu-id="df571-178">Schema of diagnostic logs in the storage account</span></span>
<span data-ttu-id="df571-179">När du har konfigurerat arkivering, skapas en lagringsbehållare i lagringskontot när en händelse inträffar i en av logg-kategorier som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="df571-179">Once you have set up archival, a storage container is created in the storage account as soon as an event occurs in one of the log categories you have enabled.</span></span> <span data-ttu-id="df571-180">Blobbar i behållaren följer samma format över diagnostikloggar och aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="df571-180">The blobs within the container follow the same format across Diagnostic Logs and the Activity Log.</span></span> <span data-ttu-id="df571-181">Strukturen för de här blobbar är:</span><span class="sxs-lookup"><span data-stu-id="df571-181">The structure of these blobs is:</span></span>

> <span data-ttu-id="df571-182">insikter - loggar-{kategori loggnamn} / resourceId = / PRENUMERATIONER / {prenumerations-ID} /RESOURCEGROUPS/ {resursgruppens namn} /PROVIDERS/ {resurs providernamn} / {resurstyp} / {resursnamn} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="df571-182">insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="df571-183">Eller fler bara</span><span class="sxs-lookup"><span data-stu-id="df571-183">Or, more simply,</span></span>

> <span data-ttu-id="df571-184">insikter - loggar-{kategori loggnamn} / resourceId = / {resursens Id} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="df571-184">insights-logs-{log category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="df571-185">Till exempel kan en blobbnamnet vara:</span><span class="sxs-lookup"><span data-stu-id="df571-185">For example, a blob name might be:</span></span>

> <span data-ttu-id="df571-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="df571-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="df571-187">Varje PT1H.json blobb innehåller en JSON-blob av händelser som inträffade inom en timme som anges i blob-URL (till exempel h = 12).</span><span class="sxs-lookup"><span data-stu-id="df571-187">Each PT1H.json blob contains a JSON blob of events that occurred within the hour specified in the blob URL (for example, h=12).</span></span> <span data-ttu-id="df571-188">Under den aktuella timman läggs händelser till filen PT1H.json när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="df571-188">During the present hour, events are appended to the PT1H.json file as they occur.</span></span> <span data-ttu-id="df571-189">Minuten (m = 00) är alltid 00, eftersom diagnostiska logghändelser delas upp i enskilda blobbar per timme.</span><span class="sxs-lookup"><span data-stu-id="df571-189">The minute value (m=00) is always 00, since diagnostic log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="df571-190">Varje händelse lagras i filen PT1H.json i matrisen ”innehåller” följa det här formatet:</span><span class="sxs-lookup"><span data-stu-id="df571-190">Within the PT1H.json file, each event is stored in the “records” array, following this format:</span></span>

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

| <span data-ttu-id="df571-191">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="df571-191">Element name</span></span> | <span data-ttu-id="df571-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df571-192">Description</span></span> |
| --- | --- |
| <span data-ttu-id="df571-193">time</span><span class="sxs-lookup"><span data-stu-id="df571-193">time</span></span> |<span data-ttu-id="df571-194">Tidsstämpel när händelsen skapades av tjänsten Azure motsvarande händelsen begäran bearbetades.</span><span class="sxs-lookup"><span data-stu-id="df571-194">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="df571-195">resourceId</span><span class="sxs-lookup"><span data-stu-id="df571-195">resourceId</span></span> |<span data-ttu-id="df571-196">Resurs-ID för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="df571-196">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="df571-197">operationName</span><span class="sxs-lookup"><span data-stu-id="df571-197">operationName</span></span> |<span data-ttu-id="df571-198">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="df571-198">Name of the operation.</span></span> |
| <span data-ttu-id="df571-199">category</span><span class="sxs-lookup"><span data-stu-id="df571-199">category</span></span> |<span data-ttu-id="df571-200">Loggen kategori för händelsen.</span><span class="sxs-lookup"><span data-stu-id="df571-200">Log category of the event.</span></span> |
| <span data-ttu-id="df571-201">properties</span><span class="sxs-lookup"><span data-stu-id="df571-201">properties</span></span> |<span data-ttu-id="df571-202">En uppsättning `<Key, Value>` par (d.v.s. ordlista) som beskriver information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="df571-202">Set of `<Key, Value>` pairs (i.e. Dictionary) describing the details of the event.</span></span> |

> [!NOTE]
> <span data-ttu-id="df571-203">Egenskaper och användning av dessa egenskaper kan variera beroende på resursen.</span><span class="sxs-lookup"><span data-stu-id="df571-203">The properties and usage of those properties can vary depending on the resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="df571-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df571-204">Next steps</span></span>
* [<span data-ttu-id="df571-205">Ladda ned blobbar för analys</span><span class="sxs-lookup"><span data-stu-id="df571-205">Download blobs for analysis</span></span>](../storage/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="df571-206">Dataströmmen diagnostiska loggar till ett namnområde för Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="df571-206">Stream diagnostic logs to an Event Hubs namespace</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="df571-207">Läs mer om diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="df571-207">Read more about diagnostic logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
