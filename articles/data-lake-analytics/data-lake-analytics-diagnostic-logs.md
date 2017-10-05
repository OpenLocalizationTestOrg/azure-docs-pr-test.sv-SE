---
title: "Visa diagnostikloggar för Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig att konfigurera och komma åt diagnostikloggarna för Azure Data Lake analytics "
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
ms.openlocfilehash: 6c74db1659742aa41306388273bec46800ba7609
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a><span data-ttu-id="cd515-103">Åtkomst till diagnostikloggarna för Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd515-103">Accessing diagnostic logs for Azure Data Lake Analytics</span></span>

<span data-ttu-id="cd515-104">Diagnostikloggning kan du samla in data granskningsspår från filåtkomstförsök.</span><span class="sxs-lookup"><span data-stu-id="cd515-104">Diagnostic logging allows you to collect data access audit trails.</span></span> <span data-ttu-id="cd515-105">Dessa loggar finns information som:</span><span class="sxs-lookup"><span data-stu-id="cd515-105">These logs provide information such as:</span></span>

* <span data-ttu-id="cd515-106">En lista över användare som har öppnat data.</span><span class="sxs-lookup"><span data-stu-id="cd515-106">A list of users that accessed the data.</span></span>
* <span data-ttu-id="cd515-107">Hur ofta data används.</span><span class="sxs-lookup"><span data-stu-id="cd515-107">How frequently the data is accessed.</span></span>
* <span data-ttu-id="cd515-108">Hur mycket data som lagras på kontot.</span><span class="sxs-lookup"><span data-stu-id="cd515-108">How much data is stored in the account.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="cd515-109">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="cd515-109">Enable logging</span></span>

1. <span data-ttu-id="cd515-110">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cd515-110">Sign on to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="cd515-111">Öppna Data Lake Analytics-kontot och markera **diagnostikloggar** från den __övervakaren__ avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cd515-111">Open your Data Lake Analytics account and select **Diagnostic logs** from the __Monitor__ section.</span></span> <span data-ttu-id="cd515-112">Välj därefter __aktivera diagnostiken__.</span><span class="sxs-lookup"><span data-stu-id="cd515-112">Next, select __Turn on diagnostics__.</span></span>

    ![Aktivera diagnostik för att samla in granskning och begära loggar](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. <span data-ttu-id="cd515-114">Från __diagnostikinställningarna__, ange status till __på__ och välj alternativ för loggning.</span><span class="sxs-lookup"><span data-stu-id="cd515-114">From __Diagnostics settings__, set the status to __On__ and select logging options.</span></span>

    <span data-ttu-id="cd515-115">![Aktivera diagnostik för att samla in granskning och begära loggar](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "aktivera diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="cd515-115">![Turn on diagnostics to collect audit and request logs](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>

   * <span data-ttu-id="cd515-116">Ange **Status** till **på** diagnostikloggning ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="cd515-116">Set **Status** to **On** to enable diagnostic logging.</span></span>

   * <span data-ttu-id="cd515-117">Du kan välja att lagra/bearbeta data i tre olika sätt.</span><span class="sxs-lookup"><span data-stu-id="cd515-117">You can choose to store/process the data in three different ways.</span></span>

     * <span data-ttu-id="cd515-118">Välj __arkivet till ett lagringskonto__ att lagra loggfiler i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cd515-118">Select __Archive to a storage account__ to store logs in an Azure storage account.</span></span> <span data-ttu-id="cd515-119">Använd det här alternativet om du vill arkivera informationen.</span><span class="sxs-lookup"><span data-stu-id="cd515-119">Use this option if you want to archive the data.</span></span> <span data-ttu-id="cd515-120">Om du väljer det här alternativet måste du ange ett Azure storage-konto om du vill spara loggfilerna för att.</span><span class="sxs-lookup"><span data-stu-id="cd515-120">If you select this option, you must provide an Azure storage account to save the logs to.</span></span>

     * <span data-ttu-id="cd515-121">Välj **dataströmmen till en Händelsehubb** dataströmmen logga data till en Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="cd515-121">Select **Stream to an Event Hub** to stream log data to an Azure Event Hub.</span></span> <span data-ttu-id="cd515-122">Använd det här alternativet om du har en pipeline för nedströms bearbetning som analysera inkommande loggarna i realtid.</span><span class="sxs-lookup"><span data-stu-id="cd515-122">Use this option if you have a downstream processing pipeline that is analyzing incoming logs in real time.</span></span> <span data-ttu-id="cd515-123">Om du väljer det här alternativet måste du ange detaljer för Azure-Händelsehubb som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="cd515-123">If you select this option, you must provide the details for the Azure Event Hub you want to use.</span></span>

     * <span data-ttu-id="cd515-124">Välj __skicka till logganalys__ att skicka data till Log Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cd515-124">Select __Send to Log Analytics__ to send the data to the Log Analytics service.</span></span> <span data-ttu-id="cd515-125">Använd det här alternativet om du vill använda Log Analytics för att samla in och analysera loggfiler.</span><span class="sxs-lookup"><span data-stu-id="cd515-125">Use this option if you want to use Log Analytics to gather and analyze logs.</span></span>
   * <span data-ttu-id="cd515-126">Ange om du vill hämta granskningsloggar eller begäran loggar eller båda.</span><span class="sxs-lookup"><span data-stu-id="cd515-126">Specify whether you want to get audit logs or request logs or both.</span></span>  <span data-ttu-id="cd515-127">En Begärandelogg samlar in varje API-begäran.</span><span class="sxs-lookup"><span data-stu-id="cd515-127">A request log captures every API request.</span></span> <span data-ttu-id="cd515-128">En granskningslogg registrerar alla åtgärder som utlöses av denna API-begäran.</span><span class="sxs-lookup"><span data-stu-id="cd515-128">An audit log records all operations that are triggered by that API request.</span></span>

   * <span data-ttu-id="cd515-129">För __arkivet till ett lagringskonto__, ange antalet dagar att behålla data.</span><span class="sxs-lookup"><span data-stu-id="cd515-129">For __Archive to a storage account__, specify the number of days to retain the data.</span></span>

   * <span data-ttu-id="cd515-130">Klicka på __Spara__.</span><span class="sxs-lookup"><span data-stu-id="cd515-130">Click __Save__.</span></span>

        > [!NOTE]
        > <span data-ttu-id="cd515-131">Du måste välja antingen __arkivet till ett lagringskonto__, __dataströmmen till en Händelsehubb__ eller __skicka till logganalys__ innan du klickar på den __spara__ knappen.</span><span class="sxs-lookup"><span data-stu-id="cd515-131">You must select either __Archive to a storage account__, __Stream to an Event Hub__ or __Send to Log Analytics__ before clicking the __Save__ button.</span></span>

<span data-ttu-id="cd515-132">När du har aktiverat diagnostikinställningar, du kan återgå till den __diagnostik loggar__ bladet för att visa loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="cd515-132">Once you have enabled diagnostic settings, you can return to the __Diagnostics logs__ blade to view the logs.</span></span>

## <a name="view-logs"></a><span data-ttu-id="cd515-133">Visa loggfiler</span><span class="sxs-lookup"><span data-stu-id="cd515-133">View logs</span></span>

### <a name="use-the-data-lake-analytics-view"></a><span data-ttu-id="cd515-134">Använd vyn Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd515-134">Use the Data Lake Analytics view</span></span>

1. <span data-ttu-id="cd515-135">Från Data Lake Analytics-kontot bladet under **övervakning**väljer **diagnostikloggar** och välj sedan en post som visar loggar för.</span><span class="sxs-lookup"><span data-stu-id="cd515-135">From your Data Lake Analytics account blade, under **Monitoring**, select **Diagnostic Logs** and then select an entry to display logs for.</span></span>

    <span data-ttu-id="cd515-136">![Visa diagnostikloggning](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="cd515-136">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span>

2. <span data-ttu-id="cd515-137">Loggarna kategoriseras efter **granskningsloggar** och **begära loggar**.</span><span class="sxs-lookup"><span data-stu-id="cd515-137">The logs are categorized by **Audit Logs** and **Request Logs**.</span></span>

    ![loggposter](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * <span data-ttu-id="cd515-139">Begäran loggar avbilda alla API-begäranden som görs på Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="cd515-139">Request logs capture every API request made on the Data Lake Analytics account.</span></span>
   * <span data-ttu-id="cd515-140">Granskningsloggar liknar begära loggar, men ger en mycket mer detaljerad analys av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cd515-140">Audit Logs are similar to request Logs but provide a much more detailed breakdown of the operations.</span></span> <span data-ttu-id="cd515-141">En enskild överföring API-anrop i loggen för begäran kan medföra flera ”tilläggsåtgärder” i dess granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="cd515-141">For example, a single upload API call in a request log can result in multiple "Append" operations in its audit log.</span></span>

3. <span data-ttu-id="cd515-142">Klicka på den **hämta** länk för en loggpost att hämta loggen.</span><span class="sxs-lookup"><span data-stu-id="cd515-142">Click the **Download** link for a log entry to download that log.</span></span>

### <a name="use-the-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="cd515-143">Använd Azure Storage-konto som innehåller loggdata</span><span class="sxs-lookup"><span data-stu-id="cd515-143">Use the Azure Storage account that contains log data</span></span>

1. <span data-ttu-id="cd515-144">Öppna bladet Azure Storage-konto som är associerade med Data Lake Analytics loggning och klicka sedan på __Blobbar__.</span><span class="sxs-lookup"><span data-stu-id="cd515-144">Open the Azure Storage account blade associated with Data Lake Analytics for logging, and then click __Blobs__.</span></span> <span data-ttu-id="cd515-145">Den **Blob-tjänst** bladet visar två behållare.</span><span class="sxs-lookup"><span data-stu-id="cd515-145">The **Blob service** blade lists two containers.</span></span>

    <span data-ttu-id="cd515-146">![Visa diagnostikloggning](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Visa diagnostikloggar")</span><span class="sxs-lookup"><span data-stu-id="cd515-146">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>

   * <span data-ttu-id="cd515-147">Behållaren **insikter loggar granskning** innehåller granskningsloggarna.</span><span class="sxs-lookup"><span data-stu-id="cd515-147">The container **insights-logs-audit** contains the audit logs.</span></span>
   * <span data-ttu-id="cd515-148">Behållaren **insikter loggar begäran** innehåller loggarna begäran.</span><span class="sxs-lookup"><span data-stu-id="cd515-148">The container **insights-logs-requests** contains the request logs.</span></span>
2. <span data-ttu-id="cd515-149">Loggfilerna lagras i dessa behållare under följande struktur:</span><span class="sxs-lookup"><span data-stu-id="cd515-149">Within these containers, the logs are stored under the following structure:</span></span>

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
   > <span data-ttu-id="cd515-150">Den `##` posterna i sökvägen innehåller år, månad, dag och timme där loggen har skapats.</span><span class="sxs-lookup"><span data-stu-id="cd515-150">The `##` entries in the path contain the year, month, day, and hour in which the log was created.</span></span> <span data-ttu-id="cd515-151">Data Lake Analytics skapar en fil varje timme så `m=` alltid innehåller värdet `00`.</span><span class="sxs-lookup"><span data-stu-id="cd515-151">Data Lake Analytics creates one file every hour, so `m=` always contains a value of `00`.</span></span>

    <span data-ttu-id="cd515-152">Den fullständiga sökvägen till en granskningslogg kan vara ett exempel:</span><span class="sxs-lookup"><span data-stu-id="cd515-152">As an example, the complete path to an audit log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    <span data-ttu-id="cd515-153">På liknande sätt kan det vara den fullständiga sökvägen till en Begärandelogg:</span><span class="sxs-lookup"><span data-stu-id="cd515-153">Similarly, the complete path to a request log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a><span data-ttu-id="cd515-154">Loggen struktur</span><span class="sxs-lookup"><span data-stu-id="cd515-154">Log structure</span></span>

<span data-ttu-id="cd515-155">Granskning och begäran loggarna finns i ett strukturerat JSON-format.</span><span class="sxs-lookup"><span data-stu-id="cd515-155">The audit and request logs are in a structured JSON format.</span></span>

### <a name="request-logs"></a><span data-ttu-id="cd515-156">Begäran loggar</span><span class="sxs-lookup"><span data-stu-id="cd515-156">Request logs</span></span>

<span data-ttu-id="cd515-157">Här är ett exempel i JSON-formaterad begäran-loggen.</span><span class="sxs-lookup"><span data-stu-id="cd515-157">Here's a sample entry in the JSON-formatted request log.</span></span> <span data-ttu-id="cd515-158">Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="cd515-158">Each blob has one root object called **records** that contains an array of log objects.</span></span>

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

#### <a name="request-log-schema"></a><span data-ttu-id="cd515-159">Schemat för begäran-logg</span><span class="sxs-lookup"><span data-stu-id="cd515-159">Request log schema</span></span>

| <span data-ttu-id="cd515-160">Namn</span><span class="sxs-lookup"><span data-stu-id="cd515-160">Name</span></span> | <span data-ttu-id="cd515-161">Typ</span><span class="sxs-lookup"><span data-stu-id="cd515-161">Type</span></span> | <span data-ttu-id="cd515-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd515-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd515-163">time</span><span class="sxs-lookup"><span data-stu-id="cd515-163">time</span></span> |<span data-ttu-id="cd515-164">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-164">String</span></span> |<span data-ttu-id="cd515-165">Loggen tidsstämpel (i UTC)</span><span class="sxs-lookup"><span data-stu-id="cd515-165">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="cd515-166">resourceId</span><span class="sxs-lookup"><span data-stu-id="cd515-166">resourceId</span></span> |<span data-ttu-id="cd515-167">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-167">String</span></span> |<span data-ttu-id="cd515-168">Identifierare för den resurs som åtgärden tog placera på</span><span class="sxs-lookup"><span data-stu-id="cd515-168">The identifier of the resource that operation took place on</span></span> |
| <span data-ttu-id="cd515-169">category</span><span class="sxs-lookup"><span data-stu-id="cd515-169">category</span></span> |<span data-ttu-id="cd515-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-170">String</span></span> |<span data-ttu-id="cd515-171">Log-kategori.</span><span class="sxs-lookup"><span data-stu-id="cd515-171">The log category.</span></span> <span data-ttu-id="cd515-172">Till exempel **begäranden**.</span><span class="sxs-lookup"><span data-stu-id="cd515-172">For example, **Requests**.</span></span> |
| <span data-ttu-id="cd515-173">operationName</span><span class="sxs-lookup"><span data-stu-id="cd515-173">operationName</span></span> |<span data-ttu-id="cd515-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-174">String</span></span> |<span data-ttu-id="cd515-175">Namnet på åtgärden som är inloggad.</span><span class="sxs-lookup"><span data-stu-id="cd515-175">Name of the operation that is logged.</span></span> <span data-ttu-id="cd515-176">Till exempel GetAggregatedJobHistory.</span><span class="sxs-lookup"><span data-stu-id="cd515-176">For example, GetAggregatedJobHistory.</span></span> |
| <span data-ttu-id="cd515-177">resultType</span><span class="sxs-lookup"><span data-stu-id="cd515-177">resultType</span></span> |<span data-ttu-id="cd515-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-178">String</span></span> |<span data-ttu-id="cd515-179">Status för åtgärden, till exempel 200.</span><span class="sxs-lookup"><span data-stu-id="cd515-179">The status of the operation, For example, 200.</span></span> |
| <span data-ttu-id="cd515-180">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="cd515-180">callerIpAddress</span></span> |<span data-ttu-id="cd515-181">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-181">String</span></span> |<span data-ttu-id="cd515-182">IP-adressen för den klient som begäran</span><span class="sxs-lookup"><span data-stu-id="cd515-182">The IP address of the client making the request</span></span> |
| <span data-ttu-id="cd515-183">correlationId</span><span class="sxs-lookup"><span data-stu-id="cd515-183">correlationId</span></span> |<span data-ttu-id="cd515-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-184">String</span></span> |<span data-ttu-id="cd515-185">Identifierare för loggen.</span><span class="sxs-lookup"><span data-stu-id="cd515-185">The identifier of the log.</span></span> <span data-ttu-id="cd515-186">Det här värdet kan användas för att gruppera en uppsättning relaterade loggposter.</span><span class="sxs-lookup"><span data-stu-id="cd515-186">This value can be used to group a set of related log entries.</span></span> |
| <span data-ttu-id="cd515-187">identity</span><span class="sxs-lookup"><span data-stu-id="cd515-187">identity</span></span> |<span data-ttu-id="cd515-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="cd515-188">Object</span></span> |<span data-ttu-id="cd515-189">Den identitet som skapar loggen</span><span class="sxs-lookup"><span data-stu-id="cd515-189">The identity that generated the log</span></span> |
| <span data-ttu-id="cd515-190">properties</span><span class="sxs-lookup"><span data-stu-id="cd515-190">properties</span></span> |<span data-ttu-id="cd515-191">JSON</span><span class="sxs-lookup"><span data-stu-id="cd515-191">JSON</span></span> |<span data-ttu-id="cd515-192">Finns i nästa avsnitt (begäran loggen egenskaper schema) mer information</span><span class="sxs-lookup"><span data-stu-id="cd515-192">See the next section (Request log properties schema) for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="cd515-193">Begäran loggen egenskaper schema</span><span class="sxs-lookup"><span data-stu-id="cd515-193">Request log properties schema</span></span>

| <span data-ttu-id="cd515-194">Namn</span><span class="sxs-lookup"><span data-stu-id="cd515-194">Name</span></span> | <span data-ttu-id="cd515-195">Typ</span><span class="sxs-lookup"><span data-stu-id="cd515-195">Type</span></span> | <span data-ttu-id="cd515-196">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd515-196">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd515-197">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="cd515-197">HttpMethod</span></span> |<span data-ttu-id="cd515-198">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-198">String</span></span> |<span data-ttu-id="cd515-199">HTTP-metoden används för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="cd515-199">The HTTP Method used for the operation.</span></span> <span data-ttu-id="cd515-200">Till exempel få.</span><span class="sxs-lookup"><span data-stu-id="cd515-200">For example, GET.</span></span> |
| <span data-ttu-id="cd515-201">Sökväg</span><span class="sxs-lookup"><span data-stu-id="cd515-201">Path</span></span> |<span data-ttu-id="cd515-202">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-202">String</span></span> |<span data-ttu-id="cd515-203">Sökvägen åtgärden utfördes på</span><span class="sxs-lookup"><span data-stu-id="cd515-203">The path the operation was performed on</span></span> |
| <span data-ttu-id="cd515-204">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="cd515-204">RequestContentLength</span></span> |<span data-ttu-id="cd515-205">int</span><span class="sxs-lookup"><span data-stu-id="cd515-205">int</span></span> |<span data-ttu-id="cd515-206">Den maximala längden för HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="cd515-206">The content length of the HTTP request</span></span> |
| <span data-ttu-id="cd515-207">ClientRequestId</span><span class="sxs-lookup"><span data-stu-id="cd515-207">ClientRequestId</span></span> |<span data-ttu-id="cd515-208">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-208">String</span></span> |<span data-ttu-id="cd515-209">Det ID som unikt identifierar den här begäran</span><span class="sxs-lookup"><span data-stu-id="cd515-209">The identifier that uniquely identifies this request</span></span> |
| <span data-ttu-id="cd515-210">StartTime</span><span class="sxs-lookup"><span data-stu-id="cd515-210">StartTime</span></span> |<span data-ttu-id="cd515-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-211">String</span></span> |<span data-ttu-id="cd515-212">Den tidpunkt då servern tog emot begäran</span><span class="sxs-lookup"><span data-stu-id="cd515-212">The time at which the server received the request</span></span> |
| <span data-ttu-id="cd515-213">Sluttid</span><span class="sxs-lookup"><span data-stu-id="cd515-213">EndTime</span></span> |<span data-ttu-id="cd515-214">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-214">String</span></span> |<span data-ttu-id="cd515-215">Den tid då servern skickade ett svar</span><span class="sxs-lookup"><span data-stu-id="cd515-215">The time at which the server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="cd515-216">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="cd515-216">Audit logs</span></span>

<span data-ttu-id="cd515-217">Här är ett exempel i JSON-formaterad granskningsloggen.</span><span class="sxs-lookup"><span data-stu-id="cd515-217">Here's a sample entry in the JSON-formatted audit log.</span></span> <span data-ttu-id="cd515-218">Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="cd515-218">Each blob has one root object called **records** that contains an array of log objects.</span></span>

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

#### <a name="audit-log-schema"></a><span data-ttu-id="cd515-219">Granska loggen schema</span><span class="sxs-lookup"><span data-stu-id="cd515-219">Audit log schema</span></span>

| <span data-ttu-id="cd515-220">Namn</span><span class="sxs-lookup"><span data-stu-id="cd515-220">Name</span></span> | <span data-ttu-id="cd515-221">Typ</span><span class="sxs-lookup"><span data-stu-id="cd515-221">Type</span></span> | <span data-ttu-id="cd515-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd515-222">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd515-223">time</span><span class="sxs-lookup"><span data-stu-id="cd515-223">time</span></span> |<span data-ttu-id="cd515-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-224">String</span></span> |<span data-ttu-id="cd515-225">Loggen tidsstämpel (i UTC)</span><span class="sxs-lookup"><span data-stu-id="cd515-225">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="cd515-226">resourceId</span><span class="sxs-lookup"><span data-stu-id="cd515-226">resourceId</span></span> |<span data-ttu-id="cd515-227">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-227">String</span></span> |<span data-ttu-id="cd515-228">Identifierare för den resurs som åtgärden tog placera på</span><span class="sxs-lookup"><span data-stu-id="cd515-228">The identifier of the resource that operation took place on</span></span> |
| <span data-ttu-id="cd515-229">category</span><span class="sxs-lookup"><span data-stu-id="cd515-229">category</span></span> |<span data-ttu-id="cd515-230">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-230">String</span></span> |<span data-ttu-id="cd515-231">Log-kategori.</span><span class="sxs-lookup"><span data-stu-id="cd515-231">The log category.</span></span> <span data-ttu-id="cd515-232">Till exempel **Audit**.</span><span class="sxs-lookup"><span data-stu-id="cd515-232">For example, **Audit**.</span></span> |
| <span data-ttu-id="cd515-233">operationName</span><span class="sxs-lookup"><span data-stu-id="cd515-233">operationName</span></span> |<span data-ttu-id="cd515-234">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-234">String</span></span> |<span data-ttu-id="cd515-235">Namnet på åtgärden som är inloggad.</span><span class="sxs-lookup"><span data-stu-id="cd515-235">Name of the operation that is logged.</span></span> <span data-ttu-id="cd515-236">Till exempel JobSubmitted.</span><span class="sxs-lookup"><span data-stu-id="cd515-236">For example, JobSubmitted.</span></span> |
| <span data-ttu-id="cd515-237">resultType</span><span class="sxs-lookup"><span data-stu-id="cd515-237">resultType</span></span> |<span data-ttu-id="cd515-238">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-238">String</span></span> |<span data-ttu-id="cd515-239">En substatus för jobbstatus (operationName).</span><span class="sxs-lookup"><span data-stu-id="cd515-239">A substatus for the job status (operationName).</span></span> |
| <span data-ttu-id="cd515-240">resultSignature</span><span class="sxs-lookup"><span data-stu-id="cd515-240">resultSignature</span></span> |<span data-ttu-id="cd515-241">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-241">String</span></span> |<span data-ttu-id="cd515-242">Ytterligare information om jobbets status (operationName).</span><span class="sxs-lookup"><span data-stu-id="cd515-242">Additional details on the job status (operationName).</span></span> |
| <span data-ttu-id="cd515-243">identity</span><span class="sxs-lookup"><span data-stu-id="cd515-243">identity</span></span> |<span data-ttu-id="cd515-244">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-244">String</span></span> |<span data-ttu-id="cd515-245">Användaren som begärde igen.</span><span class="sxs-lookup"><span data-stu-id="cd515-245">The user that requested the operation.</span></span> <span data-ttu-id="cd515-246">Till exempel susan@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="cd515-246">For example, susan@contoso.com.</span></span> |
| <span data-ttu-id="cd515-247">properties</span><span class="sxs-lookup"><span data-stu-id="cd515-247">properties</span></span> |<span data-ttu-id="cd515-248">JSON</span><span class="sxs-lookup"><span data-stu-id="cd515-248">JSON</span></span> |<span data-ttu-id="cd515-249">Finns i nästa avsnitt (Granska loggen egenskaper schema) mer information</span><span class="sxs-lookup"><span data-stu-id="cd515-249">See the next section (Audit log properties schema) for details</span></span> |

> [!NOTE]
> <span data-ttu-id="cd515-250">**resultType** och **resultSignature** innehåller information om resultatet av en åtgärd och bara innehålla ett värde om en åtgärd har slutförts.</span><span class="sxs-lookup"><span data-stu-id="cd515-250">**resultType** and **resultSignature** provide information on the result of an operation, and only contain a value if an operation has completed.</span></span> <span data-ttu-id="cd515-251">Till exempel som endast innehåller ett värde när **operationName** innehåller värdet **JobStarted** eller **JobEnded**.</span><span class="sxs-lookup"><span data-stu-id="cd515-251">For example, they only contain a value when **operationName** contains a value of **JobStarted** or **JobEnded**.</span></span>
>
>

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="cd515-252">Granska loggen egenskaper schema</span><span class="sxs-lookup"><span data-stu-id="cd515-252">Audit log properties schema</span></span>

| <span data-ttu-id="cd515-253">Namn</span><span class="sxs-lookup"><span data-stu-id="cd515-253">Name</span></span> | <span data-ttu-id="cd515-254">Typ</span><span class="sxs-lookup"><span data-stu-id="cd515-254">Type</span></span> | <span data-ttu-id="cd515-255">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd515-255">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cd515-256">JobId</span><span class="sxs-lookup"><span data-stu-id="cd515-256">JobId</span></span> |<span data-ttu-id="cd515-257">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-257">String</span></span> |<span data-ttu-id="cd515-258">Det ID som tilldelats jobbet</span><span class="sxs-lookup"><span data-stu-id="cd515-258">The ID assigned to the job</span></span> |
| <span data-ttu-id="cd515-259">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="cd515-259">JobName</span></span> |<span data-ttu-id="cd515-260">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-260">String</span></span> |<span data-ttu-id="cd515-261">Namnet som angavs för jobbet</span><span class="sxs-lookup"><span data-stu-id="cd515-261">The name that was provided for the job</span></span> |
| <span data-ttu-id="cd515-262">JobRunTime</span><span class="sxs-lookup"><span data-stu-id="cd515-262">JobRunTime</span></span> |<span data-ttu-id="cd515-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-263">String</span></span> |<span data-ttu-id="cd515-264">Körningsmiljön som används för att bearbeta i jobbet</span><span class="sxs-lookup"><span data-stu-id="cd515-264">The runtime used to process the job</span></span> |
| <span data-ttu-id="cd515-265">SubmitTime</span><span class="sxs-lookup"><span data-stu-id="cd515-265">SubmitTime</span></span> |<span data-ttu-id="cd515-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-266">String</span></span> |<span data-ttu-id="cd515-267">Den tid (i UTC) då jobbet skickades</span><span class="sxs-lookup"><span data-stu-id="cd515-267">The time (in UTC) that the job was submitted</span></span> |
| <span data-ttu-id="cd515-268">StartTime</span><span class="sxs-lookup"><span data-stu-id="cd515-268">StartTime</span></span> |<span data-ttu-id="cd515-269">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-269">String</span></span> |<span data-ttu-id="cd515-270">Den tid som jobbet började köras efter att (i UTC)</span><span class="sxs-lookup"><span data-stu-id="cd515-270">The time the job started running after submission (in UTC)</span></span> |
| <span data-ttu-id="cd515-271">Sluttid</span><span class="sxs-lookup"><span data-stu-id="cd515-271">EndTime</span></span> |<span data-ttu-id="cd515-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-272">String</span></span> |<span data-ttu-id="cd515-273">Den tid som jobbet avslutades</span><span class="sxs-lookup"><span data-stu-id="cd515-273">The time the job ended</span></span> |
| <span data-ttu-id="cd515-274">Parallellitet</span><span class="sxs-lookup"><span data-stu-id="cd515-274">Parallelism</span></span> |<span data-ttu-id="cd515-275">Sträng</span><span class="sxs-lookup"><span data-stu-id="cd515-275">String</span></span> |<span data-ttu-id="cd515-276">Antalet Data Lake Analytics-enheter som krävs för det här jobbet under överföring</span><span class="sxs-lookup"><span data-stu-id="cd515-276">The number of Data Lake Analytics units requested for this job during submission</span></span> |

> [!NOTE]
> <span data-ttu-id="cd515-277">**SubmitTime**, **StartTime**, **EndTime**, och **parallellitet** innehåller information om en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="cd515-277">**SubmitTime**, **StartTime**, **EndTime**, and **Parallelism** provide information on an operation.</span></span> <span data-ttu-id="cd515-278">Dessa poster endast innehåller ett värde om som åtgärden har startats eller slutförts.</span><span class="sxs-lookup"><span data-stu-id="cd515-278">These entries only contain a value if that operation has started or completed.</span></span> <span data-ttu-id="cd515-279">Till exempel **SubmitTime** endast innehåller ett värde efter **operationName** har värdet **JobSubmitted**.</span><span class="sxs-lookup"><span data-stu-id="cd515-279">For example, **SubmitTime** only contains a value after **operationName** has the value **JobSubmitted**.</span></span>

## <a name="process-the-log-data"></a><span data-ttu-id="cd515-280">Bearbeta loggdata</span><span class="sxs-lookup"><span data-stu-id="cd515-280">Process the log data</span></span>

<span data-ttu-id="cd515-281">Azure Data Lake Analytics innehåller ett exempel att bearbeta och analysera loggdata.</span><span class="sxs-lookup"><span data-stu-id="cd515-281">Azure Data Lake Analytics provides a sample on how to process and analyze the log data.</span></span> <span data-ttu-id="cd515-282">Du kan hitta exempel på [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="cd515-282">You can find the sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd515-283">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd515-283">Next steps</span></span>
* [<span data-ttu-id="cd515-284">Översikt över Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cd515-284">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
