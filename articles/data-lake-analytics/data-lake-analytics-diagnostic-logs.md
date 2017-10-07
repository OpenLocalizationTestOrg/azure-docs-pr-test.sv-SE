---
title: "aaaViewing diagnostikloggarna för Azure Data Lake Analytics | Microsoft Docs"
description: "Förstå hur toosetup och åtkomst diagnostiska loggar för Azure Data Lake analytics "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4cd1eb6f585c1ef96c358340232ef85721a972b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a><span data-ttu-id="52617-103">Åtkomst till diagnostikloggarna för Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="52617-103">Accessing diagnostic logs for Azure Data Lake Analytics</span></span>

<span data-ttu-id="52617-104">Diagnostikloggning kan toocollect data granskningsspår från filåtkomstförsök.</span><span class="sxs-lookup"><span data-stu-id="52617-104">Diagnostic logging allows you toocollect data access audit trails.</span></span> <span data-ttu-id="52617-105">Dessa loggar finns information som:</span><span class="sxs-lookup"><span data-stu-id="52617-105">These logs provide information such as:</span></span>

* <span data-ttu-id="52617-106">En lista över användare som har öppnat hello data.</span><span class="sxs-lookup"><span data-stu-id="52617-106">A list of users that accessed hello data.</span></span>
* <span data-ttu-id="52617-107">Hur ofta hello data används.</span><span class="sxs-lookup"><span data-stu-id="52617-107">How frequently hello data is accessed.</span></span>
* <span data-ttu-id="52617-108">Hur mycket data som lagras i hello-konto.</span><span class="sxs-lookup"><span data-stu-id="52617-108">How much data is stored in hello account.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="52617-109">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="52617-109">Enable logging</span></span>

1. <span data-ttu-id="52617-110">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="52617-110">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="52617-111">Öppna Data Lake Analytics-kontot och markera **diagnostikloggar** från hello __övervakaren__ avsnitt.</span><span class="sxs-lookup"><span data-stu-id="52617-111">Open your Data Lake Analytics account and select **Diagnostic logs** from hello __Monitor__ section.</span></span> <span data-ttu-id="52617-112">Välj därefter __aktivera diagnostiken__.</span><span class="sxs-lookup"><span data-stu-id="52617-112">Next, select __Turn on diagnostics__.</span></span>

    ![Aktivera diagnostik toocollect granskning och begära loggar](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. <span data-ttu-id="52617-114">Från __diagnostikinställningarna__, ange hello status too__On__ och välja alternativ för loggning.</span><span class="sxs-lookup"><span data-stu-id="52617-114">From __Diagnostics settings__, set hello status too__On__ and select logging options.</span></span>

    <span data-ttu-id="52617-115">![Aktivera diagnostik toocollect granskning och begära loggar](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "aktivera diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="52617-115">![Turn on diagnostics toocollect audit and request logs](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>

   * <span data-ttu-id="52617-116">Ange **Status** för**på** tooenable diagnostikloggning.</span><span class="sxs-lookup"><span data-stu-id="52617-116">Set **Status** too**On** tooenable diagnostic logging.</span></span>

   * <span data-ttu-id="52617-117">Du kan välja toostore/bearbeta hello data på tre olika sätt.</span><span class="sxs-lookup"><span data-stu-id="52617-117">You can choose toostore/process hello data in three different ways.</span></span>

     * <span data-ttu-id="52617-118">Välj __Arkivera tooa lagringskonto__ toostore loggar i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="52617-118">Select __Archive tooa storage account__ toostore logs in an Azure storage account.</span></span> <span data-ttu-id="52617-119">Använd det här alternativet om du vill tooarchive hello data.</span><span class="sxs-lookup"><span data-stu-id="52617-119">Use this option if you want tooarchive hello data.</span></span> <span data-ttu-id="52617-120">Om du väljer det här alternativet måste du ange ett Azure storage-konto toosave hello loggarna.</span><span class="sxs-lookup"><span data-stu-id="52617-120">If you select this option, you must provide an Azure storage account toosave hello logs to.</span></span>

     * <span data-ttu-id="52617-121">Välj **strömma tooan Händelsehubb** toostream logga data tooan Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="52617-121">Select **Stream tooan Event Hub** toostream log data tooan Azure Event Hub.</span></span> <span data-ttu-id="52617-122">Använd det här alternativet om du har en pipeline för nedströms bearbetning som analysera inkommande loggarna i realtid.</span><span class="sxs-lookup"><span data-stu-id="52617-122">Use this option if you have a downstream processing pipeline that is analyzing incoming logs in real time.</span></span> <span data-ttu-id="52617-123">Om du väljer det här alternativet måste du ange hello information för hello Azure Event Hub du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="52617-123">If you select this option, you must provide hello details for hello Azure Event Hub you want toouse.</span></span>

     * <span data-ttu-id="52617-124">Välj __skicka tooLog Analytics__ toosend hello data toohello logganalys-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="52617-124">Select __Send tooLog Analytics__ toosend hello data toohello Log Analytics service.</span></span> <span data-ttu-id="52617-125">Använd det här alternativet om du vill toouse logganalys toogather och analysera loggfiler.</span><span class="sxs-lookup"><span data-stu-id="52617-125">Use this option if you want toouse Log Analytics toogather and analyze logs.</span></span>
   * <span data-ttu-id="52617-126">Ange om du vill tooget granskningsloggar eller begäran loggar eller båda.</span><span class="sxs-lookup"><span data-stu-id="52617-126">Specify whether you want tooget audit logs or request logs or both.</span></span>  <span data-ttu-id="52617-127">En Begärandelogg samlar in varje API-begäran.</span><span class="sxs-lookup"><span data-stu-id="52617-127">A request log captures every API request.</span></span> <span data-ttu-id="52617-128">En granskningslogg registrerar alla åtgärder som utlöses av denna API-begäran.</span><span class="sxs-lookup"><span data-stu-id="52617-128">An audit log records all operations that are triggered by that API request.</span></span>

   * <span data-ttu-id="52617-129">För __Arkivera tooa lagringskonto__, ange hello antal dagar tooretain hello data.</span><span class="sxs-lookup"><span data-stu-id="52617-129">For __Archive tooa storage account__, specify hello number of days tooretain hello data.</span></span>

   * <span data-ttu-id="52617-130">Klicka på __Spara__.</span><span class="sxs-lookup"><span data-stu-id="52617-130">Click __Save__.</span></span>

        > [!NOTE]
        > <span data-ttu-id="52617-131">Du måste välja antingen __Arkivera tooa lagringskonto__, __strömma tooan Händelsehubb__ eller __skicka tooLog Analytics__ innan du klickar på hello __spara__knappen.</span><span class="sxs-lookup"><span data-stu-id="52617-131">You must select either __Archive tooa storage account__, __Stream tooan Event Hub__ or __Send tooLog Analytics__ before clicking hello __Save__ button.</span></span>

<span data-ttu-id="52617-132">När du har aktiverat diagnostikinställningar kan du gå tillbaka toohello __diagnostik loggar__ bladet tooview hello loggar.</span><span class="sxs-lookup"><span data-stu-id="52617-132">Once you have enabled diagnostic settings, you can return toohello __Diagnostics logs__ blade tooview hello logs.</span></span>

## <a name="view-logs"></a><span data-ttu-id="52617-133">Visa loggfiler</span><span class="sxs-lookup"><span data-stu-id="52617-133">View logs</span></span>

### <a name="use-hello-data-lake-analytics-view"></a><span data-ttu-id="52617-134">Använd hello Data Lake Analytics visa</span><span class="sxs-lookup"><span data-stu-id="52617-134">Use hello Data Lake Analytics view</span></span>

1. <span data-ttu-id="52617-135">Från Data Lake Analytics-kontot bladet under **övervakning**väljer **diagnostikloggar** och välj sedan en post toodisplay loggar för.</span><span class="sxs-lookup"><span data-stu-id="52617-135">From your Data Lake Analytics account blade, under **Monitoring**, select **Diagnostic Logs** and then select an entry toodisplay logs for.</span></span>

    <span data-ttu-id="52617-136">![Visa diagnostikloggning](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="52617-136">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span>

2. <span data-ttu-id="52617-137">hello loggar kategoriseras efter **granskningsloggar** och **begära loggar**.</span><span class="sxs-lookup"><span data-stu-id="52617-137">hello logs are categorized by **Audit Logs** and **Request Logs**.</span></span>

    ![loggposter](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * <span data-ttu-id="52617-139">Begäran loggar avbilda alla API-begäranden som görs på hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="52617-139">Request logs capture every API request made on hello Data Lake Analytics account.</span></span>
   * <span data-ttu-id="52617-140">Granskningsloggar är liknande toorequest loggar men ger en mycket mer detaljerad uppdelning av hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="52617-140">Audit Logs are similar toorequest Logs but provide a much more detailed breakdown of hello operations.</span></span> <span data-ttu-id="52617-141">En enskild överföring API-anrop i loggen för begäran kan medföra flera ”tilläggsåtgärder” i dess granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="52617-141">For example, a single upload API call in a request log can result in multiple "Append" operations in its audit log.</span></span>

3. <span data-ttu-id="52617-142">Klicka på hello **hämta** länk för en logg post toodownload loggar.</span><span class="sxs-lookup"><span data-stu-id="52617-142">Click hello **Download** link for a log entry toodownload that log.</span></span>

### <a name="use-hello-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="52617-143">Använd hello Azure Storage-konto som innehåller loggdata</span><span class="sxs-lookup"><span data-stu-id="52617-143">Use hello Azure Storage account that contains log data</span></span>

1. <span data-ttu-id="52617-144">Öppna hello Azure Storage-konto bladet som är associerade med Data Lake Analytics loggning och klicka sedan på __Blobbar__.</span><span class="sxs-lookup"><span data-stu-id="52617-144">Open hello Azure Storage account blade associated with Data Lake Analytics for logging, and then click __Blobs__.</span></span> <span data-ttu-id="52617-145">Hej **Blob-tjänst** bladet visar två behållare.</span><span class="sxs-lookup"><span data-stu-id="52617-145">hello **Blob service** blade lists two containers.</span></span>

    <span data-ttu-id="52617-146">![Visa diagnostikloggning](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="52617-146">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>

   * <span data-ttu-id="52617-147">hello behållaren **insikter loggar granskning** innehåller hello granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="52617-147">hello container **insights-logs-audit** contains hello audit logs.</span></span>
   * <span data-ttu-id="52617-148">hello behållaren **insikter loggar begäran** innehåller hello begäran loggar.</span><span class="sxs-lookup"><span data-stu-id="52617-148">hello container **insights-logs-requests** contains hello request logs.</span></span>
2. <span data-ttu-id="52617-149">I dessa behållare lagras hello loggar under hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="52617-149">Within these containers, hello logs are stored under hello following structure:</span></span>

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > <span data-ttu-id="52617-150">Hej `##` posterna i hello sökvägen innehåller hello år, månad, dag och timme i vilka hello loggen skapades.</span><span class="sxs-lookup"><span data-stu-id="52617-150">hello `##` entries in hello path contain hello year, month, day, and hour in which hello log was created.</span></span> <span data-ttu-id="52617-151">Data Lake Analytics skapar en fil varje timme så `m=` alltid innehåller värdet `00`.</span><span class="sxs-lookup"><span data-stu-id="52617-151">Data Lake Analytics creates one file every hour, so `m=` always contains a value of `00`.</span></span>

    <span data-ttu-id="52617-152">Exempelvis kan hello fullständig sökväg tooan granskningsloggen vara:</span><span class="sxs-lookup"><span data-stu-id="52617-152">As an example, hello complete path tooan audit log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    <span data-ttu-id="52617-153">På liknande sätt kan hello fullständig sökväg tooa loggen vara:</span><span class="sxs-lookup"><span data-stu-id="52617-153">Similarly, hello complete path tooa request log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a><span data-ttu-id="52617-154">Loggen struktur</span><span class="sxs-lookup"><span data-stu-id="52617-154">Log structure</span></span>

<span data-ttu-id="52617-155">hello är gransknings-och begäran i ett strukturerat JSON-format.</span><span class="sxs-lookup"><span data-stu-id="52617-155">hello audit and request logs are in a structured JSON format.</span></span>

### <a name="request-logs"></a><span data-ttu-id="52617-156">Begäran loggar</span><span class="sxs-lookup"><span data-stu-id="52617-156">Request logs</span></span>

<span data-ttu-id="52617-157">Här är ett exempel i hello JSON-formaterad loggen.</span><span class="sxs-lookup"><span data-stu-id="52617-157">Here's a sample entry in hello JSON-formatted request log.</span></span> <span data-ttu-id="52617-158">Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="52617-158">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a><span data-ttu-id="52617-159">Schemat för begäran-logg</span><span class="sxs-lookup"><span data-stu-id="52617-159">Request log schema</span></span>

| <span data-ttu-id="52617-160">Namn</span><span class="sxs-lookup"><span data-stu-id="52617-160">Name</span></span> | <span data-ttu-id="52617-161">Typ</span><span class="sxs-lookup"><span data-stu-id="52617-161">Type</span></span> | <span data-ttu-id="52617-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="52617-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52617-163">time</span><span class="sxs-lookup"><span data-stu-id="52617-163">time</span></span> |<span data-ttu-id="52617-164">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-164">String</span></span> |<span data-ttu-id="52617-165">hello tidsstämpeln (i UTC) för hello logg</span><span class="sxs-lookup"><span data-stu-id="52617-165">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="52617-166">resourceId</span><span class="sxs-lookup"><span data-stu-id="52617-166">resourceId</span></span> |<span data-ttu-id="52617-167">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-167">String</span></span> |<span data-ttu-id="52617-168">hello identifierare för hello-resurs som åtgärden tog placera på</span><span class="sxs-lookup"><span data-stu-id="52617-168">hello identifier of hello resource that operation took place on</span></span> |
| <span data-ttu-id="52617-169">category</span><span class="sxs-lookup"><span data-stu-id="52617-169">category</span></span> |<span data-ttu-id="52617-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-170">String</span></span> |<span data-ttu-id="52617-171">hello loggen kategori.</span><span class="sxs-lookup"><span data-stu-id="52617-171">hello log category.</span></span> <span data-ttu-id="52617-172">Till exempel **begäranden**.</span><span class="sxs-lookup"><span data-stu-id="52617-172">For example, **Requests**.</span></span> |
| <span data-ttu-id="52617-173">operationName</span><span class="sxs-lookup"><span data-stu-id="52617-173">operationName</span></span> |<span data-ttu-id="52617-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-174">String</span></span> |<span data-ttu-id="52617-175">Namnet på hello-åtgärd som loggas.</span><span class="sxs-lookup"><span data-stu-id="52617-175">Name of hello operation that is logged.</span></span> <span data-ttu-id="52617-176">Till exempel GetAggregatedJobHistory.</span><span class="sxs-lookup"><span data-stu-id="52617-176">For example, GetAggregatedJobHistory.</span></span> |
| <span data-ttu-id="52617-177">resultType</span><span class="sxs-lookup"><span data-stu-id="52617-177">resultType</span></span> |<span data-ttu-id="52617-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-178">String</span></span> |<span data-ttu-id="52617-179">hello status för hello åtgärden, till exempel 200.</span><span class="sxs-lookup"><span data-stu-id="52617-179">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="52617-180">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="52617-180">callerIpAddress</span></span> |<span data-ttu-id="52617-181">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-181">String</span></span> |<span data-ttu-id="52617-182">hello hello klientens hello begäran IP-adress</span><span class="sxs-lookup"><span data-stu-id="52617-182">hello IP address of hello client making hello request</span></span> |
| <span data-ttu-id="52617-183">correlationId</span><span class="sxs-lookup"><span data-stu-id="52617-183">correlationId</span></span> |<span data-ttu-id="52617-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-184">String</span></span> |<span data-ttu-id="52617-185">hello identifierare för hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="52617-185">hello identifier of hello log.</span></span> <span data-ttu-id="52617-186">Det här värdet kan vara används toogroup en uppsättning relaterade loggposter.</span><span class="sxs-lookup"><span data-stu-id="52617-186">This value can be used toogroup a set of related log entries.</span></span> |
| <span data-ttu-id="52617-187">identity</span><span class="sxs-lookup"><span data-stu-id="52617-187">identity</span></span> |<span data-ttu-id="52617-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="52617-188">Object</span></span> |<span data-ttu-id="52617-189">hello-identitet som genererade hello logg</span><span class="sxs-lookup"><span data-stu-id="52617-189">hello identity that generated hello log</span></span> |
| <span data-ttu-id="52617-190">properties</span><span class="sxs-lookup"><span data-stu-id="52617-190">properties</span></span> |<span data-ttu-id="52617-191">JSON</span><span class="sxs-lookup"><span data-stu-id="52617-191">JSON</span></span> |<span data-ttu-id="52617-192">Se nedan hello (begäran loggen egenskaper schema) mer information</span><span class="sxs-lookup"><span data-stu-id="52617-192">See hello next section (Request log properties schema) for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="52617-193">Begäran loggen egenskaper schema</span><span class="sxs-lookup"><span data-stu-id="52617-193">Request log properties schema</span></span>

| <span data-ttu-id="52617-194">Namn</span><span class="sxs-lookup"><span data-stu-id="52617-194">Name</span></span> | <span data-ttu-id="52617-195">Typ</span><span class="sxs-lookup"><span data-stu-id="52617-195">Type</span></span> | <span data-ttu-id="52617-196">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="52617-196">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52617-197">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="52617-197">HttpMethod</span></span> |<span data-ttu-id="52617-198">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-198">String</span></span> |<span data-ttu-id="52617-199">hello HTTP-metod som används för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="52617-199">hello HTTP Method used for hello operation.</span></span> <span data-ttu-id="52617-200">Till exempel få.</span><span class="sxs-lookup"><span data-stu-id="52617-200">For example, GET.</span></span> |
| <span data-ttu-id="52617-201">Sökväg</span><span class="sxs-lookup"><span data-stu-id="52617-201">Path</span></span> |<span data-ttu-id="52617-202">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-202">String</span></span> |<span data-ttu-id="52617-203">hello sökväg hello åtgärden utfördes på</span><span class="sxs-lookup"><span data-stu-id="52617-203">hello path hello operation was performed on</span></span> |
| <span data-ttu-id="52617-204">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="52617-204">RequestContentLength</span></span> |<span data-ttu-id="52617-205">int</span><span class="sxs-lookup"><span data-stu-id="52617-205">int</span></span> |<span data-ttu-id="52617-206">hello innehållslängden för hello HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="52617-206">hello content length of hello HTTP request</span></span> |
| <span data-ttu-id="52617-207">ClientRequestId</span><span class="sxs-lookup"><span data-stu-id="52617-207">ClientRequestId</span></span> |<span data-ttu-id="52617-208">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-208">String</span></span> |<span data-ttu-id="52617-209">hello-identifierare som unikt identifierar den här begäran</span><span class="sxs-lookup"><span data-stu-id="52617-209">hello identifier that uniquely identifies this request</span></span> |
| <span data-ttu-id="52617-210">StartTime</span><span class="sxs-lookup"><span data-stu-id="52617-210">StartTime</span></span> |<span data-ttu-id="52617-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-211">String</span></span> |<span data-ttu-id="52617-212">hello tid på vilka hello servern tar emot hello begäran</span><span class="sxs-lookup"><span data-stu-id="52617-212">hello time at which hello server received hello request</span></span> |
| <span data-ttu-id="52617-213">Sluttid</span><span class="sxs-lookup"><span data-stu-id="52617-213">EndTime</span></span> |<span data-ttu-id="52617-214">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-214">String</span></span> |<span data-ttu-id="52617-215">hello tid vid vilken hello servern skickade ett svar</span><span class="sxs-lookup"><span data-stu-id="52617-215">hello time at which hello server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="52617-216">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="52617-216">Audit logs</span></span>

<span data-ttu-id="52617-217">Här är ett exempel i hello JSON-formaterad granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="52617-217">Here's a sample entry in hello JSON-formatted audit log.</span></span> <span data-ttu-id="52617-218">Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="52617-218">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a><span data-ttu-id="52617-219">Granska loggen schema</span><span class="sxs-lookup"><span data-stu-id="52617-219">Audit log schema</span></span>

| <span data-ttu-id="52617-220">Namn</span><span class="sxs-lookup"><span data-stu-id="52617-220">Name</span></span> | <span data-ttu-id="52617-221">Typ</span><span class="sxs-lookup"><span data-stu-id="52617-221">Type</span></span> | <span data-ttu-id="52617-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="52617-222">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52617-223">time</span><span class="sxs-lookup"><span data-stu-id="52617-223">time</span></span> |<span data-ttu-id="52617-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-224">String</span></span> |<span data-ttu-id="52617-225">hello tidsstämpeln (i UTC) för hello logg</span><span class="sxs-lookup"><span data-stu-id="52617-225">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="52617-226">resourceId</span><span class="sxs-lookup"><span data-stu-id="52617-226">resourceId</span></span> |<span data-ttu-id="52617-227">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-227">String</span></span> |<span data-ttu-id="52617-228">hello identifierare för hello-resurs som åtgärden tog placera på</span><span class="sxs-lookup"><span data-stu-id="52617-228">hello identifier of hello resource that operation took place on</span></span> |
| <span data-ttu-id="52617-229">category</span><span class="sxs-lookup"><span data-stu-id="52617-229">category</span></span> |<span data-ttu-id="52617-230">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-230">String</span></span> |<span data-ttu-id="52617-231">hello loggen kategori.</span><span class="sxs-lookup"><span data-stu-id="52617-231">hello log category.</span></span> <span data-ttu-id="52617-232">Till exempel **Audit**.</span><span class="sxs-lookup"><span data-stu-id="52617-232">For example, **Audit**.</span></span> |
| <span data-ttu-id="52617-233">operationName</span><span class="sxs-lookup"><span data-stu-id="52617-233">operationName</span></span> |<span data-ttu-id="52617-234">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-234">String</span></span> |<span data-ttu-id="52617-235">Namnet på hello-åtgärd som loggas.</span><span class="sxs-lookup"><span data-stu-id="52617-235">Name of hello operation that is logged.</span></span> <span data-ttu-id="52617-236">Till exempel JobSubmitted.</span><span class="sxs-lookup"><span data-stu-id="52617-236">For example, JobSubmitted.</span></span> |
| <span data-ttu-id="52617-237">resultType</span><span class="sxs-lookup"><span data-stu-id="52617-237">resultType</span></span> |<span data-ttu-id="52617-238">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-238">String</span></span> |<span data-ttu-id="52617-239">En substatus för jobbstatus för hello (operationName).</span><span class="sxs-lookup"><span data-stu-id="52617-239">A substatus for hello job status (operationName).</span></span> |
| <span data-ttu-id="52617-240">resultSignature</span><span class="sxs-lookup"><span data-stu-id="52617-240">resultSignature</span></span> |<span data-ttu-id="52617-241">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-241">String</span></span> |<span data-ttu-id="52617-242">Ytterligare information om hello jobbets status (operationName).</span><span class="sxs-lookup"><span data-stu-id="52617-242">Additional details on hello job status (operationName).</span></span> |
| <span data-ttu-id="52617-243">identity</span><span class="sxs-lookup"><span data-stu-id="52617-243">identity</span></span> |<span data-ttu-id="52617-244">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-244">String</span></span> |<span data-ttu-id="52617-245">hello-användaren som begärde hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="52617-245">hello user that requested hello operation.</span></span> <span data-ttu-id="52617-246">Till exempel susan@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="52617-246">For example, susan@contoso.com.</span></span> |
| <span data-ttu-id="52617-247">properties</span><span class="sxs-lookup"><span data-stu-id="52617-247">properties</span></span> |<span data-ttu-id="52617-248">JSON</span><span class="sxs-lookup"><span data-stu-id="52617-248">JSON</span></span> |<span data-ttu-id="52617-249">Se nedan hello (Granska loggen egenskaper schema) mer information</span><span class="sxs-lookup"><span data-stu-id="52617-249">See hello next section (Audit log properties schema) for details</span></span> |

> [!NOTE]
> <span data-ttu-id="52617-250">**resultType** och **resultSignature** innehåller information om hello resultatet av en åtgärd och bara innehålla ett värde om en åtgärd har slutförts.</span><span class="sxs-lookup"><span data-stu-id="52617-250">**resultType** and **resultSignature** provide information on hello result of an operation, and only contain a value if an operation has completed.</span></span> <span data-ttu-id="52617-251">Till exempel som endast innehåller ett värde när **operationName** innehåller värdet **JobStarted** eller **JobEnded**.</span><span class="sxs-lookup"><span data-stu-id="52617-251">For example, they only contain a value when **operationName** contains a value of **JobStarted** or **JobEnded**.</span></span>
>
>

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="52617-252">Granska loggen egenskaper schema</span><span class="sxs-lookup"><span data-stu-id="52617-252">Audit log properties schema</span></span>

| <span data-ttu-id="52617-253">Namn</span><span class="sxs-lookup"><span data-stu-id="52617-253">Name</span></span> | <span data-ttu-id="52617-254">Typ</span><span class="sxs-lookup"><span data-stu-id="52617-254">Type</span></span> | <span data-ttu-id="52617-255">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="52617-255">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52617-256">JobId</span><span class="sxs-lookup"><span data-stu-id="52617-256">JobId</span></span> |<span data-ttu-id="52617-257">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-257">String</span></span> |<span data-ttu-id="52617-258">hello-ID tilldelade toohello jobb</span><span class="sxs-lookup"><span data-stu-id="52617-258">hello ID assigned toohello job</span></span> |
| <span data-ttu-id="52617-259">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="52617-259">JobName</span></span> |<span data-ttu-id="52617-260">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-260">String</span></span> |<span data-ttu-id="52617-261">hello-namnet som angavs för hello jobb</span><span class="sxs-lookup"><span data-stu-id="52617-261">hello name that was provided for hello job</span></span> |
| <span data-ttu-id="52617-262">JobRunTime</span><span class="sxs-lookup"><span data-stu-id="52617-262">JobRunTime</span></span> |<span data-ttu-id="52617-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-263">String</span></span> |<span data-ttu-id="52617-264">hello runtime används tooprocess hello jobb</span><span class="sxs-lookup"><span data-stu-id="52617-264">hello runtime used tooprocess hello job</span></span> |
| <span data-ttu-id="52617-265">SubmitTime</span><span class="sxs-lookup"><span data-stu-id="52617-265">SubmitTime</span></span> |<span data-ttu-id="52617-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-266">String</span></span> |<span data-ttu-id="52617-267">hello tid (i UTC) hello jobbet har skickats</span><span class="sxs-lookup"><span data-stu-id="52617-267">hello time (in UTC) that hello job was submitted</span></span> |
| <span data-ttu-id="52617-268">StartTime</span><span class="sxs-lookup"><span data-stu-id="52617-268">StartTime</span></span> |<span data-ttu-id="52617-269">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-269">String</span></span> |<span data-ttu-id="52617-270">hello hello jobbet började köras efter att (i UTC)</span><span class="sxs-lookup"><span data-stu-id="52617-270">hello time hello job started running after submission (in UTC)</span></span> |
| <span data-ttu-id="52617-271">Sluttid</span><span class="sxs-lookup"><span data-stu-id="52617-271">EndTime</span></span> |<span data-ttu-id="52617-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-272">String</span></span> |<span data-ttu-id="52617-273">hello hello jobbet avslutades</span><span class="sxs-lookup"><span data-stu-id="52617-273">hello time hello job ended</span></span> |
| <span data-ttu-id="52617-274">Parallellitet</span><span class="sxs-lookup"><span data-stu-id="52617-274">Parallelism</span></span> |<span data-ttu-id="52617-275">Sträng</span><span class="sxs-lookup"><span data-stu-id="52617-275">String</span></span> |<span data-ttu-id="52617-276">hello antalet Data Lake Analytics-enheter som krävs för det här jobbet under överföring</span><span class="sxs-lookup"><span data-stu-id="52617-276">hello number of Data Lake Analytics units requested for this job during submission</span></span> |

> [!NOTE]
> <span data-ttu-id="52617-277">**SubmitTime**, **StartTime**, **EndTime**, och **parallellitet** innehåller information om en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="52617-277">**SubmitTime**, **StartTime**, **EndTime**, and **Parallelism** provide information on an operation.</span></span> <span data-ttu-id="52617-278">Dessa poster endast innehåller ett värde om som åtgärden har startats eller slutförts.</span><span class="sxs-lookup"><span data-stu-id="52617-278">These entries only contain a value if that operation has started or completed.</span></span> <span data-ttu-id="52617-279">Till exempel **SubmitTime** endast innehåller ett värde efter **operationName** har hello värde **JobSubmitted**.</span><span class="sxs-lookup"><span data-stu-id="52617-279">For example, **SubmitTime** only contains a value after **operationName** has hello value **JobSubmitted**.</span></span>

## <a name="process-hello-log-data"></a><span data-ttu-id="52617-280">Bearbeta hello loggdata</span><span class="sxs-lookup"><span data-stu-id="52617-280">Process hello log data</span></span>

<span data-ttu-id="52617-281">Azure Data Lake Analytics innehåller ett exempel på hur tooprocess och analysera hello loggdata.</span><span class="sxs-lookup"><span data-stu-id="52617-281">Azure Data Lake Analytics provides a sample on how tooprocess and analyze hello log data.</span></span> <span data-ttu-id="52617-282">Du kan hitta hello exempelprogram på [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="52617-282">You can find hello sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span>

## <a name="next-steps"></a><span data-ttu-id="52617-283">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52617-283">Next steps</span></span>
* [<span data-ttu-id="52617-284">Översikt över Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="52617-284">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
