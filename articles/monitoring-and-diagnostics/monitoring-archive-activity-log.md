---
title: Arkivera Azure aktivitetsloggen | Microsoft Docs
description: "Lär dig mer om att arkivera dina Azure-aktivitetsloggen för långsiktig kvarhållning i ett lagringskonto."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 0e3a5b84f57eac96249430fa1c2c4cc076c2926a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="archive-the-azure-activity-log"></a><span data-ttu-id="d4d27-103">Arkivera Azure-aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="d4d27-103">Archive the Azure Activity Log</span></span>
<span data-ttu-id="d4d27-104">I den här artikeln visar vi hur du kan använda Azure-portalen, PowerShell-Cmdlets och plattformsoberoende CLI för att arkivera dina [ **Azure-aktivitetsloggen** ](monitoring-overview-activity-logs.md) i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d4d27-104">In this article, we show how you can use the Azure portal, PowerShell Cmdlets, or Cross-Platform CLI to archive your [**Azure Activity Log**](monitoring-overview-activity-logs.md) in a storage account.</span></span> <span data-ttu-id="d4d27-105">Det här alternativet är användbart om du vill behålla din aktivitetsloggen som är längre än 90 dagar (med fullständig kontroll över bevarandeprincipen) för granskning, statiska analys eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="d4d27-105">This option is useful if you would like to retain your Activity Log longer than 90 days (with full control over the retention policy) for audit, static analysis, or backup.</span></span> <span data-ttu-id="d4d27-106">Om du bara behöver lagra dina händelser i 90 dagar eller mindre och du behöver inte konfigurera arkivering till ett lagringskonto eftersom aktivitetsloggen händelser lagras i Azure-plattformen i 90 dagar utan att aktivera arkivering.</span><span class="sxs-lookup"><span data-stu-id="d4d27-106">If you only need to retain your events for 90 days or less you do not need to set up archival to a storage account, since Activity Log events are retained in the Azure platform for 90 days without enabling archival.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4d27-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d4d27-107">Prerequisites</span></span>
<span data-ttu-id="d4d27-108">Innan du börjar måste du [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) som du kan arkivera dina aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-108">Before you begin, you need to [create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) to which you can archive your Activity Log.</span></span> <span data-ttu-id="d4d27-109">Vi rekommenderar starkt att du inte använder ett befintligt lagringskonto som har andra, icke-övervakning data som lagras i den så att du bättre kan styra åtkomsten till övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="d4d27-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access to monitoring data.</span></span> <span data-ttu-id="d4d27-110">Men om du även arkiverar diagnostikloggar och mått till ett lagringskonto, kan det vara bra att använda detta lagringskonto för aktivitetsloggen samt för att hålla alla övervakningsdata på en central plats.</span><span class="sxs-lookup"><span data-stu-id="d4d27-110">However, if you are also archiving Diagnostic Logs and metrics to a storage account, it may make sense to use that storage account for your Activity Log as well to keep all monitoring data in a central location.</span></span> <span data-ttu-id="d4d27-111">Storage-konto som du använder måste vara ett allmänt lagringskonto inte ett blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d4d27-111">The storage account you use must be a general purpose storage account, not a blob storage account.</span></span> <span data-ttu-id="d4d27-112">Storage-konto behöver inte finnas i samma prenumeration som prenumerationen avger loggar så länge som den användare som konfigurerar inställningen har lämplig RBAC åtkomst till båda prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="d4d27-112">The storage account does not have to be in the same subscription as the subscription emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="log-profile"></a><span data-ttu-id="d4d27-113">Log-profil</span><span class="sxs-lookup"><span data-stu-id="d4d27-113">Log Profile</span></span>
<span data-ttu-id="d4d27-114">Om du vill arkivera aktivitetsloggen med någon av metoderna nedan kan du ange den **loggen profil** för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d4d27-114">To archive the Activity Log using any of the methods below, you set the **Log Profile** for a subscription.</span></span> <span data-ttu-id="d4d27-115">Log-profil definierar typ av händelser som lagras eller strömmas och utdata – lagring konto och/eller event hub.</span><span class="sxs-lookup"><span data-stu-id="d4d27-115">The Log Profile defines the type of events that are stored or streamed and the outputs—storage account and/or event hub.</span></span> <span data-ttu-id="d4d27-116">Den definierar även bevarandeprincip (antal dagar att behålla) för händelser som lagras i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d4d27-116">It also defines the retention policy (number of days to retain) for events stored in a storage account.</span></span> <span data-ttu-id="d4d27-117">Om bevarandeprincipen anges till noll, som händelser lagras på obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="d4d27-117">If the retention policy is set to zero, events are stored indefinitely.</span></span> <span data-ttu-id="d4d27-118">I annat fall kan detta anges till ett värde mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="d4d27-118">Otherwise, this can be set to any value between 1 and 2147483647.</span></span> <span data-ttu-id="d4d27-119">Bevarandeprinciper är tillämpade per dag, så i slutet av dagen (UTC) loggar från den dagen är nu utöver kvarhållning princip kommer att tas bort.</span><span class="sxs-lookup"><span data-stu-id="d4d27-119">Retention policies are applied per-day, so at the end of a day (UTC), logs from the day that is now beyond the retention policy will be deleted.</span></span> <span data-ttu-id="d4d27-120">Till exempel om du har en bevarandeprincip på en dag skulle i början av dagen idag loggar från dag före igår tas bort.</span><span class="sxs-lookup"><span data-stu-id="d4d27-120">For example, if you had a retention policy of one day, at the beginning of the day today the logs from the day before yesterday would be deleted.</span></span> <span data-ttu-id="d4d27-121">[Du kan läsa mer om loggen profiler här](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span><span class="sxs-lookup"><span data-stu-id="d4d27-121">[You can read more about log profiles here](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span></span> 

## <a name="archive-the-activity-log-using-the-portal"></a><span data-ttu-id="d4d27-122">Arkivera aktivitetsloggen med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="d4d27-122">Archive the Activity Log using the portal</span></span>
1. <span data-ttu-id="d4d27-123">I portalen klickar du på den **aktivitetsloggen** länk i navigeringen till vänster.</span><span class="sxs-lookup"><span data-stu-id="d4d27-123">In the portal, click the **Activity Log** link on the left-side navigation.</span></span> <span data-ttu-id="d4d27-124">Om du inte ser en länk för aktivitetsloggen, klickar du på den **fler tjänster** länka först.</span><span class="sxs-lookup"><span data-stu-id="d4d27-124">If you don’t see a link for the Activity Log, click the **More Services** link first.</span></span>
   
    ![Navigera till bladet för aktivitetsloggen](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. <span data-ttu-id="d4d27-126">Överst på bladet klickar du på **exportera**.</span><span class="sxs-lookup"><span data-stu-id="d4d27-126">At the top of the blade, click **Export**.</span></span>
   
    ![Klicka på knappen Export](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. <span data-ttu-id="d4d27-128">I bladet som visas, markera kryssrutan för **exportera till ett lagringskonto** och välj ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d4d27-128">In the blade that appears, check the box for **Export to a storage account** and select a storage account.</span></span>
   
    ![Ange ett lagringskonto](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. <span data-ttu-id="d4d27-130">Med hjälp av skjutreglaget eller textruta, definiera ett antal dagar som aktiviteten logghändelser ska behållas i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d4d27-130">Using the slider or text box, define a number of days for which Activity Log events should be kept in your storage account.</span></span> <span data-ttu-id="d4d27-131">Om du vill ha dina data kvar i lagringskontot på obestämd tid genom att ange siffran noll.</span><span class="sxs-lookup"><span data-stu-id="d4d27-131">If you prefer to have your data persisted in the storage account indefinitely, set this number to zero.</span></span>
5. <span data-ttu-id="d4d27-132">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d4d27-132">Click **Save**.</span></span>

## <a name="archive-the-activity-log-via-powershell"></a><span data-ttu-id="d4d27-133">Arkivera aktivitetsloggen via PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4d27-133">Archive the Activity Log via PowerShell</span></span>
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| <span data-ttu-id="d4d27-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d4d27-134">Property</span></span> | <span data-ttu-id="d4d27-135">Krävs</span><span class="sxs-lookup"><span data-stu-id="d4d27-135">Required</span></span> | <span data-ttu-id="d4d27-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d4d27-136">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d4d27-137">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="d4d27-137">StorageAccountId</span></span> |<span data-ttu-id="d4d27-138">Nej</span><span class="sxs-lookup"><span data-stu-id="d4d27-138">No</span></span> |<span data-ttu-id="d4d27-139">Resurs-ID för Storage-konto som aktivitetsloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="d4d27-139">Resource ID of the Storage Account to which Activity Logs should be saved.</span></span> |
| <span data-ttu-id="d4d27-140">Platser</span><span class="sxs-lookup"><span data-stu-id="d4d27-140">Locations</span></span> |<span data-ttu-id="d4d27-141">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-141">Yes</span></span> |<span data-ttu-id="d4d27-142">Kommaavgränsad lista över regioner som du vill samla in händelser för aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-142">Comma-separated list of regions for which you would like to collect Activity Log events.</span></span> <span data-ttu-id="d4d27-143">Du kan visa en lista över alla regioner [genom att besöka sidan](https://azure.microsoft.com/en-us/regions) eller genom att använda [Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="d4d27-143">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [the Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="d4d27-144">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="d4d27-144">RetentionInDays</span></span> |<span data-ttu-id="d4d27-145">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-145">Yes</span></span> |<span data-ttu-id="d4d27-146">Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="d4d27-146">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="d4d27-147">Värdet noll lagrar loggarna på obestämd tid (alltid).</span><span class="sxs-lookup"><span data-stu-id="d4d27-147">A value of zero stores the logs indefinitely (forever).</span></span> |
| <span data-ttu-id="d4d27-148">Kategorier</span><span class="sxs-lookup"><span data-stu-id="d4d27-148">Categories</span></span> |<span data-ttu-id="d4d27-149">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-149">Yes</span></span> |<span data-ttu-id="d4d27-150">Kommaavgränsad lista över kategorier som ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="d4d27-150">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="d4d27-151">Möjliga värden är skriva och ta bort åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d4d27-151">Possible values are Write, Delete, and Action.</span></span> |

## <a name="archive-the-activity-log-via-cli"></a><span data-ttu-id="d4d27-152">Arkivera aktivitetsloggen via CLI</span><span class="sxs-lookup"><span data-stu-id="d4d27-152">Archive the Activity Log via CLI</span></span>
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| <span data-ttu-id="d4d27-153">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d4d27-153">Property</span></span> | <span data-ttu-id="d4d27-154">Krävs</span><span class="sxs-lookup"><span data-stu-id="d4d27-154">Required</span></span> | <span data-ttu-id="d4d27-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d4d27-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d4d27-156">namn</span><span class="sxs-lookup"><span data-stu-id="d4d27-156">name</span></span> |<span data-ttu-id="d4d27-157">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-157">Yes</span></span> |<span data-ttu-id="d4d27-158">Namnet på loggen profilen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-158">Name of your log profile.</span></span> |
| <span data-ttu-id="d4d27-159">storageId</span><span class="sxs-lookup"><span data-stu-id="d4d27-159">storageId</span></span> |<span data-ttu-id="d4d27-160">Nej</span><span class="sxs-lookup"><span data-stu-id="d4d27-160">No</span></span> |<span data-ttu-id="d4d27-161">Resurs-ID för Storage-konto som aktivitetsloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="d4d27-161">Resource ID of the Storage Account to which Activity Logs should be saved.</span></span> |
| <span data-ttu-id="d4d27-162">Platser</span><span class="sxs-lookup"><span data-stu-id="d4d27-162">locations</span></span> |<span data-ttu-id="d4d27-163">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-163">Yes</span></span> |<span data-ttu-id="d4d27-164">Kommaavgränsad lista över regioner som du vill samla in händelser för aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-164">Comma-separated list of regions for which you would like to collect Activity Log events.</span></span> <span data-ttu-id="d4d27-165">Du kan visa en lista över alla regioner [genom att besöka sidan](https://azure.microsoft.com/en-us/regions) eller genom att använda [Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="d4d27-165">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [the Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="d4d27-166">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="d4d27-166">retentionInDays</span></span> |<span data-ttu-id="d4d27-167">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-167">Yes</span></span> |<span data-ttu-id="d4d27-168">Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="d4d27-168">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="d4d27-169">Värdet noll lagrar loggarna på obestämd tid (alltid).</span><span class="sxs-lookup"><span data-stu-id="d4d27-169">A value of zero will store the logs indefinitely (forever).</span></span> |
| <span data-ttu-id="d4d27-170">Kategorier</span><span class="sxs-lookup"><span data-stu-id="d4d27-170">categories</span></span> |<span data-ttu-id="d4d27-171">Ja</span><span class="sxs-lookup"><span data-stu-id="d4d27-171">Yes</span></span> |<span data-ttu-id="d4d27-172">Kommaavgränsad lista över kategorier som ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="d4d27-172">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="d4d27-173">Möjliga värden är skriva och ta bort åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d4d27-173">Possible values are Write, Delete, and Action.</span></span> |

## <a name="storage-schema-of-the-activity-log"></a><span data-ttu-id="d4d27-174">Storage-schemat för aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="d4d27-174">Storage schema of the Activity Log</span></span>
<span data-ttu-id="d4d27-175">När du har konfigurerat arkivering, skapas en lagringsbehållare i storage-konto så snart aktivitetsloggen händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="d4d27-175">Once you have set up archival, a storage container will be created in the storage account as soon as an Activity Log event occurs.</span></span> <span data-ttu-id="d4d27-176">Blobbar i behållaren följer samma format i aktivitetsloggen och diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="d4d27-176">The blobs within the container follow the same format across the Activity Log and Diagnostic Logs.</span></span> <span data-ttu-id="d4d27-177">Strukturen för de här blobbar är:</span><span class="sxs-lookup"><span data-stu-id="d4d27-177">The structure of these blobs is:</span></span>

> <span data-ttu-id="d4d27-178">insikter-operativa-loggar/name = standard/resourceId = / PRENUMERATIONER / {prenumerations-ID} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="d4d27-178">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="d4d27-179">Till exempel kan en blobbnamnet vara:</span><span class="sxs-lookup"><span data-stu-id="d4d27-179">For example, a blob name might be:</span></span>

> <span data-ttu-id="d4d27-180">Insights-Operational-Logs/Name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="d4d27-180">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="d4d27-181">Varje PT1H.json blobb innehåller en JSON-blob av händelser som inträffade inom en timme som anges i blob-URL (t.ex. h = 12).</span><span class="sxs-lookup"><span data-stu-id="d4d27-181">Each PT1H.json blob contains a JSON blob of events that occurred within the hour specified in the blob URL (e.g. h=12).</span></span> <span data-ttu-id="d4d27-182">Under den aktuella timman läggs händelser till filen PT1H.json när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="d4d27-182">During the present hour, events are appended to the PT1H.json file as they occur.</span></span> <span data-ttu-id="d4d27-183">Minuten (m = 00) är alltid 00, eftersom aktiviteten logghändelser delas upp i enskilda blobbar per timme.</span><span class="sxs-lookup"><span data-stu-id="d4d27-183">The minute value (m=00) is always 00, since Activity Log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="d4d27-184">Varje händelse lagras i filen PT1H.json i matrisen ”innehåller” följa det här formatet:</span><span class="sxs-lookup"><span data-stu-id="d4d27-184">Within the PT1H.json file, each event is stored in the “records” array, following this format:</span></span>

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| <span data-ttu-id="d4d27-185">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="d4d27-185">Element name</span></span> | <span data-ttu-id="d4d27-186">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d4d27-186">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d4d27-187">time</span><span class="sxs-lookup"><span data-stu-id="d4d27-187">time</span></span> |<span data-ttu-id="d4d27-188">Tidsstämpel när händelsen skapades av tjänsten Azure motsvarande händelsen begäran bearbetades.</span><span class="sxs-lookup"><span data-stu-id="d4d27-188">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="d4d27-189">resourceId</span><span class="sxs-lookup"><span data-stu-id="d4d27-189">resourceId</span></span> |<span data-ttu-id="d4d27-190">Resurs-ID för resursen påverkas.</span><span class="sxs-lookup"><span data-stu-id="d4d27-190">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="d4d27-191">operationName</span><span class="sxs-lookup"><span data-stu-id="d4d27-191">operationName</span></span> |<span data-ttu-id="d4d27-192">Namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d4d27-192">Name of the operation.</span></span> |
| <span data-ttu-id="d4d27-193">category</span><span class="sxs-lookup"><span data-stu-id="d4d27-193">category</span></span> |<span data-ttu-id="d4d27-194">Kategori för åtgärden, t.ex.</span><span class="sxs-lookup"><span data-stu-id="d4d27-194">Category of the action, eg.</span></span> <span data-ttu-id="d4d27-195">Skrivning, Läs, åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d4d27-195">Write, Read, Action.</span></span> |
| <span data-ttu-id="d4d27-196">resultType</span><span class="sxs-lookup"><span data-stu-id="d4d27-196">resultType</span></span> |<span data-ttu-id="d4d27-197">Typ av resultat, t.ex.</span><span class="sxs-lookup"><span data-stu-id="d4d27-197">The type of the result, eg.</span></span> <span data-ttu-id="d4d27-198">Lyckades, fel, Start</span><span class="sxs-lookup"><span data-stu-id="d4d27-198">Success, Failure, Start</span></span> |
| <span data-ttu-id="d4d27-199">resultSignature</span><span class="sxs-lookup"><span data-stu-id="d4d27-199">resultSignature</span></span> |<span data-ttu-id="d4d27-200">Beror på resurstypen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-200">Depends on the resource type.</span></span> |
| <span data-ttu-id="d4d27-201">durationMs</span><span class="sxs-lookup"><span data-stu-id="d4d27-201">durationMs</span></span> |<span data-ttu-id="d4d27-202">Varaktighet för åtgärden i millisekunder</span><span class="sxs-lookup"><span data-stu-id="d4d27-202">Duration of the operation in milliseconds</span></span> |
| <span data-ttu-id="d4d27-203">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="d4d27-203">callerIpAddress</span></span> |<span data-ttu-id="d4d27-204">IP-adressen för den användare som utförde åtgärden, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="d4d27-204">IP address of the user who has performed the operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="d4d27-205">correlationId</span><span class="sxs-lookup"><span data-stu-id="d4d27-205">correlationId</span></span> |<span data-ttu-id="d4d27-206">Vanligtvis ett GUID i strängformatet.</span><span class="sxs-lookup"><span data-stu-id="d4d27-206">Usually a GUID in the string format.</span></span> <span data-ttu-id="d4d27-207">Händelser som delar en correlationId tillhöra samma uber åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d4d27-207">Events that share a correlationId belong to the same uber action.</span></span> |
| <span data-ttu-id="d4d27-208">identity</span><span class="sxs-lookup"><span data-stu-id="d4d27-208">identity</span></span> |<span data-ttu-id="d4d27-209">JSON-blob som beskriver auktoriserings- och anspråk.</span><span class="sxs-lookup"><span data-stu-id="d4d27-209">JSON blob describing the authorization and claims.</span></span> |
| <span data-ttu-id="d4d27-210">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="d4d27-210">authorization</span></span> |<span data-ttu-id="d4d27-211">BLOB RBAC egenskaper för händelsen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-211">Blob of RBAC properties of the event.</span></span> <span data-ttu-id="d4d27-212">Inkluderar vanligtvis egenskaperna ””, ”roll” och ”omfattning”.</span><span class="sxs-lookup"><span data-stu-id="d4d27-212">Usually includes the “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="d4d27-213">nivå</span><span class="sxs-lookup"><span data-stu-id="d4d27-213">level</span></span> |<span data-ttu-id="d4d27-214">Nivå av händelsen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-214">Level of the event.</span></span> <span data-ttu-id="d4d27-215">Ett av följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="d4d27-215">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="d4d27-216">location</span><span class="sxs-lookup"><span data-stu-id="d4d27-216">location</span></span> |<span data-ttu-id="d4d27-217">Region i platsen uppstod (eller global).</span><span class="sxs-lookup"><span data-stu-id="d4d27-217">Region in which the location occurred (or global).</span></span> |
| <span data-ttu-id="d4d27-218">properties</span><span class="sxs-lookup"><span data-stu-id="d4d27-218">properties</span></span> |<span data-ttu-id="d4d27-219">En uppsättning `<Key, Value>` par (d.v.s. ordlista) som beskriver information om händelsen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-219">Set of `<Key, Value>` pairs (i.e. Dictionary) describing the details of the event.</span></span> |

> [!NOTE]
> <span data-ttu-id="d4d27-220">Egenskaper och användning av dessa egenskaper kan variera beroende på resursen.</span><span class="sxs-lookup"><span data-stu-id="d4d27-220">The properties and usage of those properties can vary depending on the resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d4d27-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4d27-221">Next steps</span></span>
* [<span data-ttu-id="d4d27-222">Ladda ned blobbar för analys</span><span class="sxs-lookup"><span data-stu-id="d4d27-222">Download blobs for analysis</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [<span data-ttu-id="d4d27-223">Dataströmmen aktivitetsloggen i Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="d4d27-223">Stream the Activity Log to Event Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
* [<span data-ttu-id="d4d27-224">Läs mer om aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="d4d27-224">Read more about the Activity Log</span></span>](monitoring-overview-activity-logs.md)

