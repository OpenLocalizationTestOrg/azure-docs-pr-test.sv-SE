---
title: "aaaViewing diagnostikloggarna för Azure Data Lake Store | Microsoft Docs"
description: "Förstå hur toosetup och åtkomst till diagnostikloggarna för Azure Data Lake Store "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 11fbf7f517f97abdcaf809c1ebeeb51424ab2c1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a><span data-ttu-id="d5d10-103">Åtkomst till diagnostikloggarna för Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d5d10-103">Accessing diagnostic logs for Azure Data Lake Store</span></span>
<span data-ttu-id="d5d10-104">Läs mer om hur tooenable diagnostikloggning för ditt Data Lake Store-konto och hur tooview hello loggar samlas in för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="d5d10-104">Learn about how tooenable diagnostic logging for your Data Lake Store account and how tooview hello logs collected for your account.</span></span>

<span data-ttu-id="d5d10-105">Organisationer kan aktivera diagnostikloggning för sina Azure Data Lake Store konto toocollect data granskningsspår från filåtkomstförsök som ger information, till exempel listan över användare som ansluter till hello data, hur ofta hello data används, hur mycket data lagras i hello konto, osv.</span><span class="sxs-lookup"><span data-stu-id="d5d10-105">Organizations can enable diagnostic logging for their Azure Data Lake Store account toocollect data access audit trails that provides information such as list of users accessing hello data, how frequently hello data is accessed, how much data is stored in hello account, etc.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5d10-106">Krav</span><span class="sxs-lookup"><span data-stu-id="d5d10-106">Prerequisites</span></span>
* <span data-ttu-id="d5d10-107">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-107">**An Azure subscription**.</span></span> <span data-ttu-id="d5d10-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5d10-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d5d10-109">**Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="d5d10-110">Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d5d10-110">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span>

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a><span data-ttu-id="d5d10-111">Aktivera diagnostikloggning för ditt Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="d5d10-111">Enable diagnostic logging for your Data Lake Store account</span></span>
1. <span data-ttu-id="d5d10-112">Logga in på nytt toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d5d10-112">Sign on toohello new [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d5d10-113">Öppna Data Lake Store-konto och ditt Data Lake Store-kontoblad klickar du på **inställningar**, och klicka sedan på **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-113">Open your Data Lake Store account, and from your Data Lake Store account blade, click **Settings**, and then click **Diagnostic logs**.</span></span>
3. <span data-ttu-id="d5d10-114">I hello **diagnostik loggar** bladet, klickar du på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-114">In hello **Diagnostics logs** blade, click **Turn on diagnostics**.</span></span>

    <span data-ttu-id="d5d10-115">![Aktivera diagnostikloggning](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "aktivera diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="d5d10-115">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Enable diagnostic logs")</span></span>

3. <span data-ttu-id="d5d10-116">I hello **diagnostiska** bladet gör följande ändringar tooconfigure diagnostikloggning hello.</span><span class="sxs-lookup"><span data-stu-id="d5d10-116">In hello **Diagnostic** blade, make hello following changes tooconfigure diagnostic logging.</span></span>
   
    <span data-ttu-id="d5d10-117">![Aktivera diagnostikloggning](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "aktivera diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="d5d10-117">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>
   
   * <span data-ttu-id="d5d10-118">Ange **Status** för**på** tooenable diagnostikloggning.</span><span class="sxs-lookup"><span data-stu-id="d5d10-118">Set **Status** too**On** tooenable diagnostic logging.</span></span>
   * <span data-ttu-id="d5d10-119">Du kan välja toostore/bearbeta hello data på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="d5d10-119">You can choose toostore/process hello data in different ways.</span></span>
     
        * <span data-ttu-id="d5d10-120">Välj alternativet för hello för**Arkivera tooa lagringskonto** toostore loggar tooan Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d5d10-120">Select hello option too**Archive tooa storage account** toostore logs tooan Azure Storage account.</span></span> <span data-ttu-id="d5d10-121">Du kan använda det här alternativet om du vill tooarchive hello data som ska bearbetas i batch vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="d5d10-121">You use this option if you want tooarchive hello data that will be batch-processed at a later date.</span></span> <span data-ttu-id="d5d10-122">Om du väljer det här alternativet måste du ange ett Azure Storage-konto toosave hello loggarna.</span><span class="sxs-lookup"><span data-stu-id="d5d10-122">If you select this option you must provide an Azure Storage account toosave hello logs to.</span></span>
        
        * <span data-ttu-id="d5d10-123">Välj alternativet för hello för**dataströmmen tooan händelsehubb** toostream logga data tooan Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="d5d10-123">Select hello option too**Stream tooan event hub** toostream log data tooan Azure Event Hub.</span></span> <span data-ttu-id="d5d10-124">Du kommer troligen använda det här alternativet om du har en nedströms bearbetning pipeline tooanalyze inkommande loggar i realtid.</span><span class="sxs-lookup"><span data-stu-id="d5d10-124">Most likely you will use this option if you have a downstream processing pipeline tooanalyze incoming logs at real time.</span></span> <span data-ttu-id="d5d10-125">Om du väljer det här alternativet måste du ange hello information för hello Azure Event Hub du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="d5d10-125">If you select this option, you must provide hello details for hello Azure Event Hub you want toouse.</span></span>

        * <span data-ttu-id="d5d10-126">Välj alternativet för hello för**skicka tooLog Analytics** toouse hello Azure logganalys tooanalyze hello genereras tjänstlogginformation.</span><span class="sxs-lookup"><span data-stu-id="d5d10-126">Select hello option too**Send tooLog Analytics** toouse hello Azure Log Analytics service tooanalyze hello generated log data.</span></span> <span data-ttu-id="d5d10-127">Om du väljer det här alternativet måste du ange hello information för hello Operations Management Suite-arbetsyta som du skulle använda hello utföra logganalys.</span><span class="sxs-lookup"><span data-stu-id="d5d10-127">If you select this option, you must provide hello details for hello Operations Management Suite workspace that you would use hello perform log analysis.</span></span>
     
   * <span data-ttu-id="d5d10-128">Ange om du vill tooget granskningsloggar eller begäran loggar eller båda.</span><span class="sxs-lookup"><span data-stu-id="d5d10-128">Specify whether you want tooget audit logs or request logs or both.</span></span>
   * <span data-ttu-id="d5d10-129">Ange hello antal dagar som hello data måste behållas.</span><span class="sxs-lookup"><span data-stu-id="d5d10-129">Specify hello number of days for which hello data must be retained.</span></span> <span data-ttu-id="d5d10-130">Kvarhållning gäller endast om du använder Azure storage-konto tooarchive loggdata.</span><span class="sxs-lookup"><span data-stu-id="d5d10-130">Retention is only applicable if you are using Azure storage account tooarchive log data.</span></span>
   * <span data-ttu-id="d5d10-131">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-131">Click **Save**.</span></span>

<span data-ttu-id="d5d10-132">När du har aktiverat diagnostikinställningar, kan du titta på hello loggar in hello **diagnostikloggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="d5d10-132">Once you have enabled diagnostic settings, you can watch hello logs in hello **Diagnostic Logs** tab.</span></span>

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a><span data-ttu-id="d5d10-133">Visa diagnostikloggar för ditt Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="d5d10-133">View diagnostic logs for your Data Lake Store account</span></span>
<span data-ttu-id="d5d10-134">Det finns två sätt tooview hello loggdata för ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="d5d10-134">There are two ways tooview hello log data for your Data Lake Store account.</span></span>

* <span data-ttu-id="d5d10-135">Inställningar för Visa från hello Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="d5d10-135">From hello Data Lake Store account settings view</span></span>
* <span data-ttu-id="d5d10-136">Från hello Azure Storage-konto som innehåller hello-data</span><span class="sxs-lookup"><span data-stu-id="d5d10-136">From hello Azure Storage account where hello data is stored</span></span>

### <a name="using-hello-data-lake-store-settings-view"></a><span data-ttu-id="d5d10-137">Med hjälp av hello Visa inställningar för Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d5d10-137">Using hello Data Lake Store Settings view</span></span>
1. <span data-ttu-id="d5d10-138">Från ditt Data Lake Store-konto **inställningar** bladet, klickar du på **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-138">From your Data Lake Store account **Settings** blade, click **Diagnostic Logs**.</span></span>
   
    <span data-ttu-id="d5d10-139">![Visa diagnostikloggning](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="d5d10-139">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span> 
2. <span data-ttu-id="d5d10-140">I hello **diagnostikloggar** bladet bör du se hello loggar kategoriserade efter **granskningsloggar** och **begära loggar**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-140">In hello **Diagnostic Logs** blade, you should see hello logs categorized by **Audit Logs** and **Request Logs**.</span></span>
   
   * <span data-ttu-id="d5d10-141">Begäran loggar avbilda alla API-begäranden som görs på hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="d5d10-141">Request logs capture every API request made on hello Data Lake Store account.</span></span>
   * <span data-ttu-id="d5d10-142">Granskningsloggar är liknande toorequest loggar men ger en mycket mer detaljerad uppdelning av hello åtgärder som utförs på hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="d5d10-142">Audit Logs are similar toorequest Logs but provide a much more detailed breakdown of hello operations being performed on hello Data Lake Store account.</span></span> <span data-ttu-id="d5d10-143">En enskild överföring API-anrop i begäran loggar kan exempelvis resultera i flera ”tilläggsåtgärder” i hello granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="d5d10-143">For example, a single upload API call in request logs might result in multiple "Append" operations in hello audit logs.</span></span>
3. <span data-ttu-id="d5d10-144">Klicka på hello **hämta** länken mot varje logga post toodownload hello loggar.</span><span class="sxs-lookup"><span data-stu-id="d5d10-144">Click hello **Download** link against each log entry toodownload hello logs.</span></span>

### <a name="from-hello-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="d5d10-145">Från hello Azure Storage-konto innehåller som loggdata</span><span class="sxs-lookup"><span data-stu-id="d5d10-145">From hello Azure Storage account that contains log data</span></span>
1. <span data-ttu-id="d5d10-146">Öppna hello Azure Storage-konto bladet som är associerade med Data Lake Store loggning och klicka sedan på Blobbar.</span><span class="sxs-lookup"><span data-stu-id="d5d10-146">Open hello Azure Storage account blade associated with Data Lake Store for logging, and then click Blobs.</span></span> <span data-ttu-id="d5d10-147">Hej **Blob-tjänst** bladet visar två behållare.</span><span class="sxs-lookup"><span data-stu-id="d5d10-147">hello **Blob service** blade lists two containers.</span></span>
   
    <span data-ttu-id="d5d10-148">![Visa diagnostikloggning](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="d5d10-148">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>
   
   * <span data-ttu-id="d5d10-149">hello behållaren **insikter loggar granskning** innehåller hello granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="d5d10-149">hello container **insights-logs-audit** contains hello audit logs.</span></span>
   * <span data-ttu-id="d5d10-150">hello behållaren **insikter loggar begäran** innehåller hello begäran loggar.</span><span class="sxs-lookup"><span data-stu-id="d5d10-150">hello container **insights-logs-requests** contains hello request logs.</span></span>
2. <span data-ttu-id="d5d10-151">I dessa behållare lagras hello loggar under hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="d5d10-151">Within these containers, hello logs are stored under hello following structure.</span></span>
   
    <span data-ttu-id="d5d10-152">![Visa diagnostikloggning](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="d5d10-152">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "View diagnostic logs")</span></span>
   
    <span data-ttu-id="d5d10-153">Exempelvis kunde hello fullständig sökväg tooan granskningsloggen`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="d5d10-153">As an example, hello complete path tooan audit log could be `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span></span>
   
    <span data-ttu-id="d5d10-154">Similary, hello fullständig sökväg tooa loggen kan vara`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="d5d10-154">Similary, hello complete path tooa request log could be `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span></span>

## <a name="understand-hello-structure-of-hello-log-data"></a><span data-ttu-id="d5d10-155">Förstå hello strukturen för hello loggdata</span><span class="sxs-lookup"><span data-stu-id="d5d10-155">Understand hello structure of hello log data</span></span>
<span data-ttu-id="d5d10-156">hello finns gransknings-och begäran i en JSON-format.</span><span class="sxs-lookup"><span data-stu-id="d5d10-156">hello audit and request logs are in a JSON format.</span></span> <span data-ttu-id="d5d10-157">I det här avsnittet ska vi titta på hello struktur JSON för begäran och granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="d5d10-157">In this section, we look at hello structure of JSON for request and audit logs.</span></span>

### <a name="request-logs"></a><span data-ttu-id="d5d10-158">Begäran loggar</span><span class="sxs-lookup"><span data-stu-id="d5d10-158">Request logs</span></span>
<span data-ttu-id="d5d10-159">Här är ett exempel i hello JSON-formaterad loggen.</span><span class="sxs-lookup"><span data-stu-id="d5d10-159">Here's a sample entry in hello JSON-formatted request log.</span></span> <span data-ttu-id="d5d10-160">Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="d5d10-160">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a><span data-ttu-id="d5d10-161">Schemat för begäran-logg</span><span class="sxs-lookup"><span data-stu-id="d5d10-161">Request log schema</span></span>
| <span data-ttu-id="d5d10-162">Namn</span><span class="sxs-lookup"><span data-stu-id="d5d10-162">Name</span></span> | <span data-ttu-id="d5d10-163">Typ</span><span class="sxs-lookup"><span data-stu-id="d5d10-163">Type</span></span> | <span data-ttu-id="d5d10-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5d10-164">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d5d10-165">time</span><span class="sxs-lookup"><span data-stu-id="d5d10-165">time</span></span> |<span data-ttu-id="d5d10-166">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-166">String</span></span> |<span data-ttu-id="d5d10-167">hello tidsstämpeln (i UTC) för hello logg</span><span class="sxs-lookup"><span data-stu-id="d5d10-167">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="d5d10-168">resourceId</span><span class="sxs-lookup"><span data-stu-id="d5d10-168">resourceId</span></span> |<span data-ttu-id="d5d10-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-169">String</span></span> |<span data-ttu-id="d5d10-170">hello-ID för hello-resurs som åtgärden tog placera på</span><span class="sxs-lookup"><span data-stu-id="d5d10-170">hello ID of hello resource that operation took place on</span></span> |
| <span data-ttu-id="d5d10-171">category</span><span class="sxs-lookup"><span data-stu-id="d5d10-171">category</span></span> |<span data-ttu-id="d5d10-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-172">String</span></span> |<span data-ttu-id="d5d10-173">hello loggen kategori.</span><span class="sxs-lookup"><span data-stu-id="d5d10-173">hello log category.</span></span> <span data-ttu-id="d5d10-174">Till exempel **begäranden**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-174">For example, **Requests**.</span></span> |
| <span data-ttu-id="d5d10-175">operationName</span><span class="sxs-lookup"><span data-stu-id="d5d10-175">operationName</span></span> |<span data-ttu-id="d5d10-176">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-176">String</span></span> |<span data-ttu-id="d5d10-177">Namnet på hello-åtgärd som loggas.</span><span class="sxs-lookup"><span data-stu-id="d5d10-177">Name of hello operation that is logged.</span></span> <span data-ttu-id="d5d10-178">Till exempel getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="d5d10-178">For example, getfilestatus.</span></span> |
| <span data-ttu-id="d5d10-179">resultType</span><span class="sxs-lookup"><span data-stu-id="d5d10-179">resultType</span></span> |<span data-ttu-id="d5d10-180">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-180">String</span></span> |<span data-ttu-id="d5d10-181">hello status för hello åtgärden, till exempel 200.</span><span class="sxs-lookup"><span data-stu-id="d5d10-181">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="d5d10-182">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="d5d10-182">callerIpAddress</span></span> |<span data-ttu-id="d5d10-183">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-183">String</span></span> |<span data-ttu-id="d5d10-184">hello hello klientens hello begäran IP-adress</span><span class="sxs-lookup"><span data-stu-id="d5d10-184">hello IP address of hello client making hello request</span></span> |
| <span data-ttu-id="d5d10-185">correlationId</span><span class="sxs-lookup"><span data-stu-id="d5d10-185">correlationId</span></span> |<span data-ttu-id="d5d10-186">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-186">String</span></span> |<span data-ttu-id="d5d10-187">hello-id för hello-logg som kan användas toogroup tillsammans en uppsättning relaterade loggposter</span><span class="sxs-lookup"><span data-stu-id="d5d10-187">hello id of hello log that can used toogroup together a set of related log entries</span></span> |
| <span data-ttu-id="d5d10-188">identity</span><span class="sxs-lookup"><span data-stu-id="d5d10-188">identity</span></span> |<span data-ttu-id="d5d10-189">Objekt</span><span class="sxs-lookup"><span data-stu-id="d5d10-189">Object</span></span> |<span data-ttu-id="d5d10-190">hello-identitet som genererade hello logg</span><span class="sxs-lookup"><span data-stu-id="d5d10-190">hello identity that generated hello log</span></span> |
| <span data-ttu-id="d5d10-191">properties</span><span class="sxs-lookup"><span data-stu-id="d5d10-191">properties</span></span> |<span data-ttu-id="d5d10-192">JSON</span><span class="sxs-lookup"><span data-stu-id="d5d10-192">JSON</span></span> |<span data-ttu-id="d5d10-193">Mer information finns nedan</span><span class="sxs-lookup"><span data-stu-id="d5d10-193">See below for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="d5d10-194">Begäran loggen egenskaper schema</span><span class="sxs-lookup"><span data-stu-id="d5d10-194">Request log properties schema</span></span>
| <span data-ttu-id="d5d10-195">Namn</span><span class="sxs-lookup"><span data-stu-id="d5d10-195">Name</span></span> | <span data-ttu-id="d5d10-196">Typ</span><span class="sxs-lookup"><span data-stu-id="d5d10-196">Type</span></span> | <span data-ttu-id="d5d10-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5d10-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d5d10-198">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="d5d10-198">HttpMethod</span></span> |<span data-ttu-id="d5d10-199">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-199">String</span></span> |<span data-ttu-id="d5d10-200">hello HTTP-metod som används för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d5d10-200">hello HTTP Method used for hello operation.</span></span> <span data-ttu-id="d5d10-201">Till exempel få.</span><span class="sxs-lookup"><span data-stu-id="d5d10-201">For example, GET.</span></span> |
| <span data-ttu-id="d5d10-202">Sökväg</span><span class="sxs-lookup"><span data-stu-id="d5d10-202">Path</span></span> |<span data-ttu-id="d5d10-203">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-203">String</span></span> |<span data-ttu-id="d5d10-204">hello sökväg hello åtgärden utfördes på</span><span class="sxs-lookup"><span data-stu-id="d5d10-204">hello path hello operation was performed on</span></span> |
| <span data-ttu-id="d5d10-205">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="d5d10-205">RequestContentLength</span></span> |<span data-ttu-id="d5d10-206">int</span><span class="sxs-lookup"><span data-stu-id="d5d10-206">int</span></span> |<span data-ttu-id="d5d10-207">hello innehållslängden för hello HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="d5d10-207">hello content length of hello HTTP request</span></span> |
| <span data-ttu-id="d5d10-208">ClientRequestId</span><span class="sxs-lookup"><span data-stu-id="d5d10-208">ClientRequestId</span></span> |<span data-ttu-id="d5d10-209">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-209">String</span></span> |<span data-ttu-id="d5d10-210">hello-Id som unikt identifierar den här begäran</span><span class="sxs-lookup"><span data-stu-id="d5d10-210">hello Id that uniquely identifies this request</span></span> |
| <span data-ttu-id="d5d10-211">StartTime</span><span class="sxs-lookup"><span data-stu-id="d5d10-211">StartTime</span></span> |<span data-ttu-id="d5d10-212">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-212">String</span></span> |<span data-ttu-id="d5d10-213">hello tid på vilka hello servern tar emot hello begäran</span><span class="sxs-lookup"><span data-stu-id="d5d10-213">hello time at which hello server received hello request</span></span> |
| <span data-ttu-id="d5d10-214">Sluttid</span><span class="sxs-lookup"><span data-stu-id="d5d10-214">EndTime</span></span> |<span data-ttu-id="d5d10-215">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-215">String</span></span> |<span data-ttu-id="d5d10-216">hello tid vid vilken hello servern skickade ett svar</span><span class="sxs-lookup"><span data-stu-id="d5d10-216">hello time at which hello server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="d5d10-217">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="d5d10-217">Audit logs</span></span>
<span data-ttu-id="d5d10-218">Här är ett exempel i hello JSON-formaterad granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="d5d10-218">Here's a sample entry in hello JSON-formatted audit log.</span></span> <span data-ttu-id="d5d10-219">Varje blobb har en rotobjektet kallas **poster** som innehåller en uppsättning objekt</span><span class="sxs-lookup"><span data-stu-id="d5d10-219">Each blob has one root object called **records** that contains an array of log objects</span></span>

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a><span data-ttu-id="d5d10-220">Granska loggen schema</span><span class="sxs-lookup"><span data-stu-id="d5d10-220">Audit log schema</span></span>
| <span data-ttu-id="d5d10-221">Namn</span><span class="sxs-lookup"><span data-stu-id="d5d10-221">Name</span></span> | <span data-ttu-id="d5d10-222">Typ</span><span class="sxs-lookup"><span data-stu-id="d5d10-222">Type</span></span> | <span data-ttu-id="d5d10-223">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5d10-223">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d5d10-224">time</span><span class="sxs-lookup"><span data-stu-id="d5d10-224">time</span></span> |<span data-ttu-id="d5d10-225">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-225">String</span></span> |<span data-ttu-id="d5d10-226">hello tidsstämpeln (i UTC) för hello logg</span><span class="sxs-lookup"><span data-stu-id="d5d10-226">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="d5d10-227">resourceId</span><span class="sxs-lookup"><span data-stu-id="d5d10-227">resourceId</span></span> |<span data-ttu-id="d5d10-228">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-228">String</span></span> |<span data-ttu-id="d5d10-229">hello-ID för hello-resurs som åtgärden tog placera på</span><span class="sxs-lookup"><span data-stu-id="d5d10-229">hello ID of hello resource that operation took place on</span></span> |
| <span data-ttu-id="d5d10-230">category</span><span class="sxs-lookup"><span data-stu-id="d5d10-230">category</span></span> |<span data-ttu-id="d5d10-231">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-231">String</span></span> |<span data-ttu-id="d5d10-232">hello loggen kategori.</span><span class="sxs-lookup"><span data-stu-id="d5d10-232">hello log category.</span></span> <span data-ttu-id="d5d10-233">Till exempel **Audit**.</span><span class="sxs-lookup"><span data-stu-id="d5d10-233">For example, **Audit**.</span></span> |
| <span data-ttu-id="d5d10-234">operationName</span><span class="sxs-lookup"><span data-stu-id="d5d10-234">operationName</span></span> |<span data-ttu-id="d5d10-235">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-235">String</span></span> |<span data-ttu-id="d5d10-236">Namnet på hello-åtgärd som loggas.</span><span class="sxs-lookup"><span data-stu-id="d5d10-236">Name of hello operation that is logged.</span></span> <span data-ttu-id="d5d10-237">Till exempel getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="d5d10-237">For example, getfilestatus.</span></span> |
| <span data-ttu-id="d5d10-238">resultType</span><span class="sxs-lookup"><span data-stu-id="d5d10-238">resultType</span></span> |<span data-ttu-id="d5d10-239">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-239">String</span></span> |<span data-ttu-id="d5d10-240">hello status för hello åtgärden, till exempel 200.</span><span class="sxs-lookup"><span data-stu-id="d5d10-240">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="d5d10-241">correlationId</span><span class="sxs-lookup"><span data-stu-id="d5d10-241">correlationId</span></span> |<span data-ttu-id="d5d10-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-242">String</span></span> |<span data-ttu-id="d5d10-243">hello-id för hello-logg som kan användas toogroup tillsammans en uppsättning relaterade loggposter</span><span class="sxs-lookup"><span data-stu-id="d5d10-243">hello id of hello log that can used toogroup together a set of related log entries</span></span> |
| <span data-ttu-id="d5d10-244">identity</span><span class="sxs-lookup"><span data-stu-id="d5d10-244">identity</span></span> |<span data-ttu-id="d5d10-245">Objekt</span><span class="sxs-lookup"><span data-stu-id="d5d10-245">Object</span></span> |<span data-ttu-id="d5d10-246">hello-identitet som genererade hello logg</span><span class="sxs-lookup"><span data-stu-id="d5d10-246">hello identity that generated hello log</span></span> |
| <span data-ttu-id="d5d10-247">properties</span><span class="sxs-lookup"><span data-stu-id="d5d10-247">properties</span></span> |<span data-ttu-id="d5d10-248">JSON</span><span class="sxs-lookup"><span data-stu-id="d5d10-248">JSON</span></span> |<span data-ttu-id="d5d10-249">Mer information finns nedan</span><span class="sxs-lookup"><span data-stu-id="d5d10-249">See below for details</span></span> |

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="d5d10-250">Granska loggen egenskaper schema</span><span class="sxs-lookup"><span data-stu-id="d5d10-250">Audit log properties schema</span></span>
| <span data-ttu-id="d5d10-251">Namn</span><span class="sxs-lookup"><span data-stu-id="d5d10-251">Name</span></span> | <span data-ttu-id="d5d10-252">Typ</span><span class="sxs-lookup"><span data-stu-id="d5d10-252">Type</span></span> | <span data-ttu-id="d5d10-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5d10-253">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d5d10-254">StreamName</span><span class="sxs-lookup"><span data-stu-id="d5d10-254">StreamName</span></span> |<span data-ttu-id="d5d10-255">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5d10-255">String</span></span> |<span data-ttu-id="d5d10-256">hello sökväg hello åtgärden utfördes på</span><span class="sxs-lookup"><span data-stu-id="d5d10-256">hello path hello operation was performed on</span></span> |

## <a name="samples-tooprocess-hello-log-data"></a><span data-ttu-id="d5d10-257">Exempel tooprocess hello loggdata</span><span class="sxs-lookup"><span data-stu-id="d5d10-257">Samples tooprocess hello log data</span></span>
<span data-ttu-id="d5d10-258">Azure Data Lake Store ger ett exempel på hur tooprocess och analysera hello loggdata.</span><span class="sxs-lookup"><span data-stu-id="d5d10-258">Azure Data Lake Store provides a sample on how tooprocess and analyze hello log data.</span></span> <span data-ttu-id="d5d10-259">Du kan hitta hello exempelprogram på [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="d5d10-259">You can find hello sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span> 

## <a name="see-also"></a><span data-ttu-id="d5d10-260">Se även</span><span class="sxs-lookup"><span data-stu-id="d5d10-260">See also</span></span>
* [<span data-ttu-id="d5d10-261">Översikt över Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d5d10-261">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="d5d10-262">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d5d10-262">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)

