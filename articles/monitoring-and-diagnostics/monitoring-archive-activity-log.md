---
title: aaaArchive hello Azure-aktivitetsloggen | Microsoft Docs
description: "Lär dig hur tooarchive din Azure aktivitet loggar för långsiktig kvarhållning i ett lagringskonto."
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
ms.openlocfilehash: 58c6d3a3a31398287f66f76999d48f2942ab5109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="archive-hello-azure-activity-log"></a><span data-ttu-id="242a4-103">Arkivera hello Azure-aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="242a4-103">Archive hello Azure Activity Log</span></span>
<span data-ttu-id="242a4-104">I den här artikeln visar vi hur du kan använda hello Azure-portalen, PowerShell-Cmdlets och plattformsoberoende CLI tooarchive din [ **Azure-aktivitetsloggen** ](monitoring-overview-activity-logs.md) i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="242a4-104">In this article, we show how you can use hello Azure portal, PowerShell Cmdlets, or Cross-Platform CLI tooarchive your [**Azure Activity Log**](monitoring-overview-activity-logs.md) in a storage account.</span></span> <span data-ttu-id="242a4-105">Det här alternativet är användbart om du vill att tooretain din aktivitetsloggen som är längre än 90 dagar (med fullständig kontroll över hello bevarandeprincip) för granska statiska analys eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="242a4-105">This option is useful if you would like tooretain your Activity Log longer than 90 days (with full control over hello retention policy) for audit, static analysis, or backup.</span></span> <span data-ttu-id="242a4-106">Om du bara behöver tooretain behöver inte händelserna i 90 dagar eller mindre du tooset in arkivering tooa lagringskontot eftersom aktivitetsloggen händelser är kvar i hello Azure-plattformen i 90 dagar utan att aktivera arkivering.</span><span class="sxs-lookup"><span data-stu-id="242a4-106">If you only need tooretain your events for 90 days or less you do not need tooset up archival tooa storage account, since Activity Log events are retained in hello Azure platform for 90 days without enabling archival.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="242a4-107">Krav</span><span class="sxs-lookup"><span data-stu-id="242a4-107">Prerequisites</span></span>
<span data-ttu-id="242a4-108">Innan du börjar behöver du för[skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich som du kan arkivera dina aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="242a4-108">Before you begin, you need too[create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich you can archive your Activity Log.</span></span> <span data-ttu-id="242a4-109">Vi rekommenderar starkt att du inte använder ett befintligt lagringskonto som har andra, icke-övervakning data som lagras i den så att du bättre kan styra åtkomst till toomonitoring data.</span><span class="sxs-lookup"><span data-stu-id="242a4-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access toomonitoring data.</span></span> <span data-ttu-id="242a4-110">Men om du även arkiverar diagnostiska loggar och mått tooa storage-konto, kan det vara meningsfullt toouse detta lagringskonto för dina aktiviteter logga samt tookeep alla övervakningsdata på en central plats.</span><span class="sxs-lookup"><span data-stu-id="242a4-110">However, if you are also archiving Diagnostic Logs and metrics tooa storage account, it may make sense toouse that storage account for your Activity Log as well tookeep all monitoring data in a central location.</span></span> <span data-ttu-id="242a4-111">hello storage-konto du använder måste vara ett allmänt lagringskonto inte ett blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="242a4-111">hello storage account you use must be a general purpose storage account, not a blob storage account.</span></span> <span data-ttu-id="242a4-112">hello storage-konto har inte toobe i hello samma prenumeration som hello prenumeration avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="242a4-112">hello storage account does not have toobe in hello same subscription as hello subscription emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="log-profile"></a><span data-ttu-id="242a4-113">Log-profil</span><span class="sxs-lookup"><span data-stu-id="242a4-113">Log Profile</span></span>
<span data-ttu-id="242a4-114">tooarchive hello aktivitetsloggen med hjälp av hello metoderna nedan, ange hello **loggen profil** för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="242a4-114">tooarchive hello Activity Log using any of hello methods below, you set hello **Log Profile** for a subscription.</span></span> <span data-ttu-id="242a4-115">hello loggen profil definierar hello typ av händelser som lagras eller strömmas och hello utdata – lagring konto och/eller event hub.</span><span class="sxs-lookup"><span data-stu-id="242a4-115">hello Log Profile defines hello type of events that are stored or streamed and hello outputs—storage account and/or event hub.</span></span> <span data-ttu-id="242a4-116">Den definierar även hello bevarandeprincip (antal dagar tooretain) för händelser som lagras i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="242a4-116">It also defines hello retention policy (number of days tooretain) for events stored in a storage account.</span></span> <span data-ttu-id="242a4-117">Om hello bevarandeprincip anges toozero lagras händelser på obestämd tid.</span><span class="sxs-lookup"><span data-stu-id="242a4-117">If hello retention policy is set toozero, events are stored indefinitely.</span></span> <span data-ttu-id="242a4-118">Detta kan annars ställas in tooany värde mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="242a4-118">Otherwise, this can be set tooany value between 1 and 2147483647.</span></span> <span data-ttu-id="242a4-119">Bevarandeprinciper är tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip kommer att tas bort.</span><span class="sxs-lookup"><span data-stu-id="242a4-119">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy will be deleted.</span></span> <span data-ttu-id="242a4-120">Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort.</span><span class="sxs-lookup"><span data-stu-id="242a4-120">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted.</span></span> <span data-ttu-id="242a4-121">[Du kan läsa mer om loggen profiler här](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span><span class="sxs-lookup"><span data-stu-id="242a4-121">[You can read more about log profiles here](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span></span> 

## <a name="archive-hello-activity-log-using-hello-portal"></a><span data-ttu-id="242a4-122">Arkivera hello aktivitetsloggen med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="242a4-122">Archive hello Activity Log using hello portal</span></span>
1. <span data-ttu-id="242a4-123">I hello-portalen klickar du på hello **aktivitetsloggen** länk på hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="242a4-123">In hello portal, click hello **Activity Log** link on hello left-side navigation.</span></span> <span data-ttu-id="242a4-124">Om du inte ser en länk för hello aktivitetsloggen, klickar du på hello **fler tjänster** länka först.</span><span class="sxs-lookup"><span data-stu-id="242a4-124">If you don’t see a link for hello Activity Log, click hello **More Services** link first.</span></span>
   
    ![Navigera tooActivity loggen bladet](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. <span data-ttu-id="242a4-126">Hello överkant hello-bladet, klickar du på **exportera**.</span><span class="sxs-lookup"><span data-stu-id="242a4-126">At hello top of hello blade, click **Export**.</span></span>
   
    ![Klicka på hello Export](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. <span data-ttu-id="242a4-128">Hello-bladet som visas kryssrutan hello för **exportera lagringskontot tooa** och välj ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="242a4-128">In hello blade that appears, check hello box for **Export tooa storage account** and select a storage account.</span></span>
   
    ![Ange ett lagringskonto](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. <span data-ttu-id="242a4-130">Använder hello skjutreglaget eller textruta, definiera ett antal dagar som aktiviteten logghändelser ska behållas i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="242a4-130">Using hello slider or text box, define a number of days for which Activity Log events should be kept in your storage account.</span></span> <span data-ttu-id="242a4-131">Om du föredrar toohave data kvar i hello storage-konto på obestämd tid genom att ange det här antalet toozero.</span><span class="sxs-lookup"><span data-stu-id="242a4-131">If you prefer toohave your data persisted in hello storage account indefinitely, set this number toozero.</span></span>
5. <span data-ttu-id="242a4-132">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="242a4-132">Click **Save**.</span></span>

## <a name="archive-hello-activity-log-via-powershell"></a><span data-ttu-id="242a4-133">Arkivera hello aktivitetsloggen via PowerShell</span><span class="sxs-lookup"><span data-stu-id="242a4-133">Archive hello Activity Log via PowerShell</span></span>
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| <span data-ttu-id="242a4-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="242a4-134">Property</span></span> | <span data-ttu-id="242a4-135">Krävs</span><span class="sxs-lookup"><span data-stu-id="242a4-135">Required</span></span> | <span data-ttu-id="242a4-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="242a4-136">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="242a4-137">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="242a4-137">StorageAccountId</span></span> |<span data-ttu-id="242a4-138">Nej</span><span class="sxs-lookup"><span data-stu-id="242a4-138">No</span></span> |<span data-ttu-id="242a4-139">Resurs-ID för hello Lagringskonto toowhich aktivitetsloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="242a4-139">Resource ID of hello Storage Account toowhich Activity Logs should be saved.</span></span> |
| <span data-ttu-id="242a4-140">Platser</span><span class="sxs-lookup"><span data-stu-id="242a4-140">Locations</span></span> |<span data-ttu-id="242a4-141">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-141">Yes</span></span> |<span data-ttu-id="242a4-142">Kommaavgränsad lista över regioner som du vill att toocollect aktivitetsloggen händelser.</span><span class="sxs-lookup"><span data-stu-id="242a4-142">Comma-separated list of regions for which you would like toocollect Activity Log events.</span></span> <span data-ttu-id="242a4-143">Du kan visa en lista över alla regioner [genom att besöka sidan](https://azure.microsoft.com/en-us/regions) eller genom att använda [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="242a4-143">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="242a4-144">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="242a4-144">RetentionInDays</span></span> |<span data-ttu-id="242a4-145">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-145">Yes</span></span> |<span data-ttu-id="242a4-146">Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="242a4-146">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="242a4-147">Värdet noll lagrar hello loggar på obestämd tid (alltid).</span><span class="sxs-lookup"><span data-stu-id="242a4-147">A value of zero stores hello logs indefinitely (forever).</span></span> |
| <span data-ttu-id="242a4-148">Kategorier</span><span class="sxs-lookup"><span data-stu-id="242a4-148">Categories</span></span> |<span data-ttu-id="242a4-149">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-149">Yes</span></span> |<span data-ttu-id="242a4-150">Kommaavgränsad lista över kategorier som ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="242a4-150">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="242a4-151">Möjliga värden är skriva och ta bort åtgärd.</span><span class="sxs-lookup"><span data-stu-id="242a4-151">Possible values are Write, Delete, and Action.</span></span> |

## <a name="archive-hello-activity-log-via-cli"></a><span data-ttu-id="242a4-152">Arkivera hello aktivitetsloggen via CLI</span><span class="sxs-lookup"><span data-stu-id="242a4-152">Archive hello Activity Log via CLI</span></span>
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| <span data-ttu-id="242a4-153">Egenskap</span><span class="sxs-lookup"><span data-stu-id="242a4-153">Property</span></span> | <span data-ttu-id="242a4-154">Krävs</span><span class="sxs-lookup"><span data-stu-id="242a4-154">Required</span></span> | <span data-ttu-id="242a4-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="242a4-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="242a4-156">namn</span><span class="sxs-lookup"><span data-stu-id="242a4-156">name</span></span> |<span data-ttu-id="242a4-157">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-157">Yes</span></span> |<span data-ttu-id="242a4-158">Namnet på loggen profilen.</span><span class="sxs-lookup"><span data-stu-id="242a4-158">Name of your log profile.</span></span> |
| <span data-ttu-id="242a4-159">storageId</span><span class="sxs-lookup"><span data-stu-id="242a4-159">storageId</span></span> |<span data-ttu-id="242a4-160">Nej</span><span class="sxs-lookup"><span data-stu-id="242a4-160">No</span></span> |<span data-ttu-id="242a4-161">Resurs-ID för hello Lagringskonto toowhich aktivitetsloggar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="242a4-161">Resource ID of hello Storage Account toowhich Activity Logs should be saved.</span></span> |
| <span data-ttu-id="242a4-162">Platser</span><span class="sxs-lookup"><span data-stu-id="242a4-162">locations</span></span> |<span data-ttu-id="242a4-163">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-163">Yes</span></span> |<span data-ttu-id="242a4-164">Kommaavgränsad lista över regioner som du vill att toocollect aktivitetsloggen händelser.</span><span class="sxs-lookup"><span data-stu-id="242a4-164">Comma-separated list of regions for which you would like toocollect Activity Log events.</span></span> <span data-ttu-id="242a4-165">Du kan visa en lista över alla regioner [genom att besöka sidan](https://azure.microsoft.com/en-us/regions) eller genom att använda [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="242a4-165">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="242a4-166">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="242a4-166">retentionInDays</span></span> |<span data-ttu-id="242a4-167">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-167">Yes</span></span> |<span data-ttu-id="242a4-168">Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647.</span><span class="sxs-lookup"><span data-stu-id="242a4-168">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="242a4-169">Värdet noll lagrar hello loggar på obestämd tid (alltid).</span><span class="sxs-lookup"><span data-stu-id="242a4-169">A value of zero will store hello logs indefinitely (forever).</span></span> |
| <span data-ttu-id="242a4-170">Kategorier</span><span class="sxs-lookup"><span data-stu-id="242a4-170">categories</span></span> |<span data-ttu-id="242a4-171">Ja</span><span class="sxs-lookup"><span data-stu-id="242a4-171">Yes</span></span> |<span data-ttu-id="242a4-172">Kommaavgränsad lista över kategorier som ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="242a4-172">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="242a4-173">Möjliga värden är skriva och ta bort åtgärd.</span><span class="sxs-lookup"><span data-stu-id="242a4-173">Possible values are Write, Delete, and Action.</span></span> |

## <a name="storage-schema-of-hello-activity-log"></a><span data-ttu-id="242a4-174">Schemat för lagring av hello aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="242a4-174">Storage schema of hello Activity Log</span></span>
<span data-ttu-id="242a4-175">När du har konfigurerat arkivering, skapas en lagringsbehållare i hello storage-konto så snart aktivitetsloggen händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="242a4-175">Once you have set up archival, a storage container will be created in hello storage account as soon as an Activity Log event occurs.</span></span> <span data-ttu-id="242a4-176">Hej blobbar i behållaren hello Följ hello samma format över hello aktivitetsloggen och diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="242a4-176">hello blobs within hello container follow hello same format across hello Activity Log and Diagnostic Logs.</span></span> <span data-ttu-id="242a4-177">hello strukturen för de här blobbar är:</span><span class="sxs-lookup"><span data-stu-id="242a4-177">hello structure of these blobs is:</span></span>

> <span data-ttu-id="242a4-178">insikter-operativa-loggar/name = standard/resourceId = / PRENUMERATIONER / {prenumerations-ID} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="242a4-178">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="242a4-179">Till exempel kan en blobbnamnet vara:</span><span class="sxs-lookup"><span data-stu-id="242a4-179">For example, a blob name might be:</span></span>

> <span data-ttu-id="242a4-180">Insights-Operational-Logs/Name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="242a4-180">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="242a4-181">Varje PT1H.json blobb innehåller en JSON-blob av händelser som inträffade inom hello timme som anges i hello blob-URL (t.ex. h = 12).</span><span class="sxs-lookup"><span data-stu-id="242a4-181">Each PT1H.json blob contains a JSON blob of events that occurred within hello hour specified in hello blob URL (e.g. h=12).</span></span> <span data-ttu-id="242a4-182">Under hello finns timme är händelser tillagda toohello PT1H.json filen när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="242a4-182">During hello present hour, events are appended toohello PT1H.json file as they occur.</span></span> <span data-ttu-id="242a4-183">Hej minuten (m = 00) är alltid 00, eftersom aktiviteten logghändelser delas upp i enskilda blobbar per timme.</span><span class="sxs-lookup"><span data-stu-id="242a4-183">hello minute value (m=00) is always 00, since Activity Log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="242a4-184">I hello PT1H.json filen lagras varje händelse i hello ”innehåller” matris, efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="242a4-184">Within hello PT1H.json file, each event is stored in hello “records” array, following this format:</span></span>

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


| <span data-ttu-id="242a4-185">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="242a4-185">Element name</span></span> | <span data-ttu-id="242a4-186">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="242a4-186">Description</span></span> |
| --- | --- |
| <span data-ttu-id="242a4-187">time</span><span class="sxs-lookup"><span data-stu-id="242a4-187">time</span></span> |<span data-ttu-id="242a4-188">Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="242a4-188">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="242a4-189">resourceId</span><span class="sxs-lookup"><span data-stu-id="242a4-189">resourceId</span></span> |<span data-ttu-id="242a4-190">Resurs-ID för hello påverkas resurs.</span><span class="sxs-lookup"><span data-stu-id="242a4-190">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="242a4-191">operationName</span><span class="sxs-lookup"><span data-stu-id="242a4-191">operationName</span></span> |<span data-ttu-id="242a4-192">Namnet på hello igen.</span><span class="sxs-lookup"><span data-stu-id="242a4-192">Name of hello operation.</span></span> |
| <span data-ttu-id="242a4-193">category</span><span class="sxs-lookup"><span data-stu-id="242a4-193">category</span></span> |<span data-ttu-id="242a4-194">Kategori av hello åtgärder, t.ex.</span><span class="sxs-lookup"><span data-stu-id="242a4-194">Category of hello action, eg.</span></span> <span data-ttu-id="242a4-195">Skrivning, Läs, åtgärden.</span><span class="sxs-lookup"><span data-stu-id="242a4-195">Write, Read, Action.</span></span> |
| <span data-ttu-id="242a4-196">resultType</span><span class="sxs-lookup"><span data-stu-id="242a4-196">resultType</span></span> |<span data-ttu-id="242a4-197">Hej typ av hello resultat, t.ex.</span><span class="sxs-lookup"><span data-stu-id="242a4-197">hello type of hello result, eg.</span></span> <span data-ttu-id="242a4-198">Lyckades, fel, Start</span><span class="sxs-lookup"><span data-stu-id="242a4-198">Success, Failure, Start</span></span> |
| <span data-ttu-id="242a4-199">resultSignature</span><span class="sxs-lookup"><span data-stu-id="242a4-199">resultSignature</span></span> |<span data-ttu-id="242a4-200">Beror på hello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="242a4-200">Depends on hello resource type.</span></span> |
| <span data-ttu-id="242a4-201">durationMs</span><span class="sxs-lookup"><span data-stu-id="242a4-201">durationMs</span></span> |<span data-ttu-id="242a4-202">Varaktighet för hello-åtgärden i millisekunder</span><span class="sxs-lookup"><span data-stu-id="242a4-202">Duration of hello operation in milliseconds</span></span> |
| <span data-ttu-id="242a4-203">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="242a4-203">callerIpAddress</span></span> |<span data-ttu-id="242a4-204">IP-adressen för hello-användare som har utfört hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="242a4-204">IP address of hello user who has performed hello operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="242a4-205">correlationId</span><span class="sxs-lookup"><span data-stu-id="242a4-205">correlationId</span></span> |<span data-ttu-id="242a4-206">Vanligtvis ett GUID i hello-strängformat.</span><span class="sxs-lookup"><span data-stu-id="242a4-206">Usually a GUID in hello string format.</span></span> <span data-ttu-id="242a4-207">Händelser som delar en correlationId tillhör toohello samma uber åtgärd.</span><span class="sxs-lookup"><span data-stu-id="242a4-207">Events that share a correlationId belong toohello same uber action.</span></span> |
| <span data-ttu-id="242a4-208">identity</span><span class="sxs-lookup"><span data-stu-id="242a4-208">identity</span></span> |<span data-ttu-id="242a4-209">JSON-blob som beskriver hello auktoriserings- och anspråk.</span><span class="sxs-lookup"><span data-stu-id="242a4-209">JSON blob describing hello authorization and claims.</span></span> |
| <span data-ttu-id="242a4-210">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="242a4-210">authorization</span></span> |<span data-ttu-id="242a4-211">BLOB RBAC egenskaper för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="242a4-211">Blob of RBAC properties of hello event.</span></span> <span data-ttu-id="242a4-212">Normalt innehåller hello ””, ”roll” och ”omfattning” egenskaper.</span><span class="sxs-lookup"><span data-stu-id="242a4-212">Usually includes hello “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="242a4-213">nivå</span><span class="sxs-lookup"><span data-stu-id="242a4-213">level</span></span> |<span data-ttu-id="242a4-214">Nivå av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="242a4-214">Level of hello event.</span></span> <span data-ttu-id="242a4-215">En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig”</span><span class="sxs-lookup"><span data-stu-id="242a4-215">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="242a4-216">location</span><span class="sxs-lookup"><span data-stu-id="242a4-216">location</span></span> |<span data-ttu-id="242a4-217">Region i vilken hello plats uppstod (eller global).</span><span class="sxs-lookup"><span data-stu-id="242a4-217">Region in which hello location occurred (or global).</span></span> |
| <span data-ttu-id="242a4-218">properties</span><span class="sxs-lookup"><span data-stu-id="242a4-218">properties</span></span> |<span data-ttu-id="242a4-219">En uppsättning `<Key, Value>` par (d.v.s. ordlista) som beskriver hello information om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="242a4-219">Set of `<Key, Value>` pairs (i.e. Dictionary) describing hello details of hello event.</span></span> |

> [!NOTE]
> <span data-ttu-id="242a4-220">hello egenskaper och användning av dessa egenskaper kan variera beroende på hello resurs.</span><span class="sxs-lookup"><span data-stu-id="242a4-220">hello properties and usage of those properties can vary depending on hello resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="242a4-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="242a4-221">Next steps</span></span>
* [<span data-ttu-id="242a4-222">Ladda ned blobbar för analys</span><span class="sxs-lookup"><span data-stu-id="242a4-222">Download blobs for analysis</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [<span data-ttu-id="242a4-223">Strömma hello aktivitetsloggen tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="242a4-223">Stream hello Activity Log tooEvent Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
* [<span data-ttu-id="242a4-224">Läs mer om hello aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="242a4-224">Read more about hello Activity Log</span></span>](monitoring-overview-activity-logs.md)

