---
title: Importera data till analyser i Azure Application Insights | Microsoft Docs
description: "Importera statiska data att ansluta med app telemetri eller importera en separat dataström frågan med Analytics."
services: application-insights
keywords: "Öppna schema, import av data"
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bwren
ms.openlocfilehash: aa855a9050ec4e5e7c5db88b7209b8bb48bdba51
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="import-data-into-analytics"></a><span data-ttu-id="50af2-104">Importera data till Analytics</span><span class="sxs-lookup"><span data-stu-id="50af2-104">Import data into Analytics</span></span>

<span data-ttu-id="50af2-105">Importera inga tabelldata i [Analytics](app-insights-analytics.md), antingen för att ansluta till den med [Programinsikter](app-insights-overview.md) telemetri från din app, eller så att du kan analysera dem som en separat dataström.</span><span class="sxs-lookup"><span data-stu-id="50af2-105">Import any tabular data into [Analytics](app-insights-analytics.md), either to join it with [Application Insights](app-insights-overview.md) telemetry from your app, or so that you can analyze it as a separate stream.</span></span> <span data-ttu-id="50af2-106">Analytics är ett kraftfullt frågespråk som är väl lämpade för analys av stora volymer tidsstämplad dataströmmar telemetri.</span><span class="sxs-lookup"><span data-stu-id="50af2-106">Analytics is a powerful query language well-suited to analyzing high-volume timestamped streams of telemetry.</span></span>

<span data-ttu-id="50af2-107">Du kan importera data till Analytics med hjälp av ditt eget schema.</span><span class="sxs-lookup"><span data-stu-id="50af2-107">You can import data into Analytics using your own schema.</span></span> <span data-ttu-id="50af2-108">Det behöver inte använda standard Application Insights-scheman som begäran eller trace.</span><span class="sxs-lookup"><span data-stu-id="50af2-108">It doesn't have to use the standard Application Insights schemas such as request or trace.</span></span>

<span data-ttu-id="50af2-109">Du kan importera JSON eller DSV (avgränsare med kommaavgränsade värden - kommatecken eller semikolon fliken) filer.</span><span class="sxs-lookup"><span data-stu-id="50af2-109">You can import JSON or DSV (delimiter-separated values - comma, semicolon or tab) files.</span></span>

<span data-ttu-id="50af2-110">Det finns tre situationer som du importerar till Analytics kan vara användbar:</span><span class="sxs-lookup"><span data-stu-id="50af2-110">There are three situations where importing to Analytics is useful:</span></span>

* <span data-ttu-id="50af2-111">**Anslut med app telemetri.**</span><span class="sxs-lookup"><span data-stu-id="50af2-111">**Join with app telemetry.**</span></span> <span data-ttu-id="50af2-112">Exempelvis kan du importera en tabell som mappar URL: er från webbplatsen till mer lättläst rubriker.</span><span class="sxs-lookup"><span data-stu-id="50af2-112">For example, you could import a table that maps URLs from your website to more readable page titles.</span></span> <span data-ttu-id="50af2-113">Du kan skapa en instrumentpanel diagramrapport som visar de tio populäraste sidorna på webbplatsen i analyser.</span><span class="sxs-lookup"><span data-stu-id="50af2-113">In Analytics, you can create a dashboard chart report that shows the ten most popular pages in your website.</span></span> <span data-ttu-id="50af2-114">Det kan nu visa sidnamn i stället för URL: er.</span><span class="sxs-lookup"><span data-stu-id="50af2-114">Now it can show the page titles instead of the URLs.</span></span>
* <span data-ttu-id="50af2-115">**Korrelera programmets telemetri** med andra källor, till exempel nätverkstrafik, server-data eller CDN loggfiler.</span><span class="sxs-lookup"><span data-stu-id="50af2-115">**Correlate your application telemetry** with other sources such as network traffic, server data, or CDN log files.</span></span>
* <span data-ttu-id="50af2-116">**Gäller Analytics en separat dataström.**</span><span class="sxs-lookup"><span data-stu-id="50af2-116">**Apply Analytics to a separate data stream.**</span></span> <span data-ttu-id="50af2-117">Application Insights Analytics är ett kraftfullt verktyg som fungerar bra med sparse, tidsstämplad dataströmmar - mycket bättre än SQL i många fall.</span><span class="sxs-lookup"><span data-stu-id="50af2-117">Application Insights Analytics is a powerful tool, that works well with sparse, timestamped streams - much better than SQL in many cases.</span></span> <span data-ttu-id="50af2-118">Om du har en sådan dataström från någon annan källa kan analysera du dem med Analytics.</span><span class="sxs-lookup"><span data-stu-id="50af2-118">If you have such a stream from some other source, you can analyze it with Analytics.</span></span>

<span data-ttu-id="50af2-119">Det är enkelt att skicka data till datakällan.</span><span class="sxs-lookup"><span data-stu-id="50af2-119">Sending data to your data source is easy.</span></span> 

1. <span data-ttu-id="50af2-120">(En gång) Definiera schemat för dina data i en-datakälla.</span><span class="sxs-lookup"><span data-stu-id="50af2-120">(One time) Define the schema of your data in a 'data source'.</span></span>
2. <span data-ttu-id="50af2-121">(Med jämna mellanrum) Ladda upp data till Azure-lagring och anropa REST-API för att meddela oss att nya data väntar införandet.</span><span class="sxs-lookup"><span data-stu-id="50af2-121">(Periodically) Upload your data to Azure storage, and call the REST API to notify us that new data is waiting for ingestion.</span></span> <span data-ttu-id="50af2-122">Data är tillgängliga för frågan i Analytics inom några minuter.</span><span class="sxs-lookup"><span data-stu-id="50af2-122">Within a few minutes, the data is available for query in Analytics.</span></span>

<span data-ttu-id="50af2-123">Frekvensen av överföringen har definierats av dig och hur snabbt vill du att dina data ska vara tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="50af2-123">The frequency of the upload is defined by you and how fast would you like your data to be available for queries.</span></span> <span data-ttu-id="50af2-124">Det är mer effektivt att överföra data i större segment, men inte är större än 1GB.</span><span class="sxs-lookup"><span data-stu-id="50af2-124">It is more efficient to upload data in larger chunks, but not larger than 1GB.</span></span>

> [!NOTE]
> <span data-ttu-id="50af2-125">*Har många datakällor för att analysera?*</span><span class="sxs-lookup"><span data-stu-id="50af2-125">*Got lots of data sources to analyze?*</span></span> [<span data-ttu-id="50af2-126">*Överväg att använda* logstash *att leverera data till Application Insights.*</span><span class="sxs-lookup"><span data-stu-id="50af2-126">*Consider using* logstash *to ship your data into Application Insights.*</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a><span data-ttu-id="50af2-127">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="50af2-127">Before you start</span></span>

<span data-ttu-id="50af2-128">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="50af2-128">You need:</span></span>

1. <span data-ttu-id="50af2-129">En Application Insights-resurs i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="50af2-129">An Application Insights resource in Microsoft Azure.</span></span>

 * <span data-ttu-id="50af2-130">Om du vill analysera dina data separat från andra telemetri [skapar en ny Application Insights-resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="50af2-130">If you want to analyze your data separately from any other telemetry, [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span>
 * <span data-ttu-id="50af2-131">Om du ansluter eller jämförelse av dina data med telemetri från en app som redan har konfigurerats med Application Insights, kan du använda resursen för appen.</span><span class="sxs-lookup"><span data-stu-id="50af2-131">If you're joining or comparing your data with telemetry from an app that is already set up with Application Insights, then you can use the resource for that app.</span></span>
 * <span data-ttu-id="50af2-132">Medarbetare eller ägare åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="50af2-132">Contributor or owner access to that resource.</span></span>
 
2. <span data-ttu-id="50af2-133">Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="50af2-133">Azure storage.</span></span> <span data-ttu-id="50af2-134">Du överför till Azure-lagring och Analytics hämtar dina data därifrån.</span><span class="sxs-lookup"><span data-stu-id="50af2-134">You upload to Azure storage, and Analytics gets your data from there.</span></span> 

 * <span data-ttu-id="50af2-135">Vi rekommenderar att du skapar ett dedikerat storage-konto för din BLOB.</span><span class="sxs-lookup"><span data-stu-id="50af2-135">We recommend you create a dedicated storage account for your blobs.</span></span> <span data-ttu-id="50af2-136">Det tar längre tid för våra processer att läsa din blobbar om dina blobbar som delas med andra processer.</span><span class="sxs-lookup"><span data-stu-id="50af2-136">If your blobs are shared with other processes, it takes longer for our processes to read your blobs.</span></span>


## <a name="define-your-schema"></a><span data-ttu-id="50af2-137">Definiera schemat</span><span class="sxs-lookup"><span data-stu-id="50af2-137">Define your schema</span></span>

<span data-ttu-id="50af2-138">Innan du kan importera data, måste du definiera en *datakällan* som anger schemat för dina data.</span><span class="sxs-lookup"><span data-stu-id="50af2-138">Before you can import data, you must define a *data source,* which specifies the schema of your data.</span></span>
<span data-ttu-id="50af2-139">Du kan ha upp till 50 datakällor i en enda Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="50af2-139">You can have up to 50 data sources in a single Application Insights resource</span></span>

1. <span data-ttu-id="50af2-140">Starta guiden datakälla.</span><span class="sxs-lookup"><span data-stu-id="50af2-140">Start the data source wizard.</span></span> <span data-ttu-id="50af2-141">Använd knappen ”Lägg till ny datakälla”.</span><span class="sxs-lookup"><span data-stu-id="50af2-141">Use "Add new data source" button.</span></span> <span data-ttu-id="50af2-142">Du kan också - klicka på inställningsknappen i övre högra hörnet och välj ”datakällor” i listrutan.</span><span class="sxs-lookup"><span data-stu-id="50af2-142">Alternatively - click on settings button in right upper corner and choose "Data Sources" in dropdown menu.</span></span>

    ![Lägg till ny datakälla](./media/app-insights-analytics-import/add-new-data-source.png)

    <span data-ttu-id="50af2-144">Ange ett namn för den nya datakällan.</span><span class="sxs-lookup"><span data-stu-id="50af2-144">Provide a name for your new data source.</span></span>

2. <span data-ttu-id="50af2-145">Definiera formatet för de filer som du kommer att överföra.</span><span class="sxs-lookup"><span data-stu-id="50af2-145">Define format of the files that you will upload.</span></span>

    <span data-ttu-id="50af2-146">Du kan definiera formatet manuellt eller ladda upp en exempelfil.</span><span class="sxs-lookup"><span data-stu-id="50af2-146">You can either define the format manually, or upload a sample file.</span></span>

    <span data-ttu-id="50af2-147">Om data finns i CSV-format, kan den första raden i exempel vara kolumnrubrikerna.</span><span class="sxs-lookup"><span data-stu-id="50af2-147">If the data is in CSV format, the first row of the sample can be column headers.</span></span> <span data-ttu-id="50af2-148">Du kan ändra fältnamn i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="50af2-148">You can change the field names in the next step.</span></span>

    <span data-ttu-id="50af2-149">Exemplet ska innehålla minst 10 rader eller poster av data.</span><span class="sxs-lookup"><span data-stu-id="50af2-149">The sample should include at least 10 rows or records of data.</span></span>

    <span data-ttu-id="50af2-150">Kolumnen eller fältnamn bör ha alfanumeriskt namn (utan blanksteg eller skiljetecken).</span><span class="sxs-lookup"><span data-stu-id="50af2-150">Column or field names should have alphanumeric names (without spaces or punctuation).</span></span>

    ![Överför en exempelfil](./media/app-insights-analytics-import/sample-data-file.png)


3. <span data-ttu-id="50af2-152">Granska det schema som du har gjort i guiden.</span><span class="sxs-lookup"><span data-stu-id="50af2-152">Review the schema that the wizard has got.</span></span> <span data-ttu-id="50af2-153">Om den härleda typer från ett exempel kan du behöva justera antydda typer av kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="50af2-153">If it inferred the types from a sample, you might need to adjust the inferred types of the columns.</span></span>

    ![Granska antydda schemat](./media/app-insights-analytics-import/data-source-review-schema.png)

 * <span data-ttu-id="50af2-155">(Valfritt.) Överför schemadefinition.</span><span class="sxs-lookup"><span data-stu-id="50af2-155">(Optional.) Upload a schema definition.</span></span> <span data-ttu-id="50af2-156">Se nedanstående format.</span><span class="sxs-lookup"><span data-stu-id="50af2-156">See the format below.</span></span>

 * <span data-ttu-id="50af2-157">Välj en tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="50af2-157">Select a Timestamp.</span></span> <span data-ttu-id="50af2-158">Alla data i Analytics måste ha något tidsstämpelsfält.</span><span class="sxs-lookup"><span data-stu-id="50af2-158">All data in Analytics must have a timestamp field.</span></span> <span data-ttu-id="50af2-159">Det måste vara av typen `datetime`, men det behöver inte ha namnet ”tidsstämpel'.</span><span class="sxs-lookup"><span data-stu-id="50af2-159">It must have type `datetime`, but it doesn't have to be named 'timestamp'.</span></span> <span data-ttu-id="50af2-160">Om dina data har en kolumn som innehåller datum och tid med ISO-format, väljer du det som kolumn för tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="50af2-160">If your data has a column containing a date and time in ISO format, choose this as the timestamp column.</span></span> <span data-ttu-id="50af2-161">Annars väljer du ”som data anlänt” och importen lägger till något tidsstämpelsfält.</span><span class="sxs-lookup"><span data-stu-id="50af2-161">Otherwise, choose "as data arrived", and the import process will add a timestamp field.</span></span>

5. <span data-ttu-id="50af2-162">Skapa datakällan.</span><span class="sxs-lookup"><span data-stu-id="50af2-162">Create the data source.</span></span>

### <a name="schema-definition-file-format"></a><span data-ttu-id="50af2-163">Schemaformat för paketdefinitionsfiler</span><span class="sxs-lookup"><span data-stu-id="50af2-163">Schema definition file format</span></span>

<span data-ttu-id="50af2-164">Du kan läsa in schemadefinitionen från en fil i stället för att redigera schemat i Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="50af2-164">Instead of editing the schema in UI, you can load the schema definition from a file.</span></span> <span data-ttu-id="50af2-165">Formatet schema definition är följande:</span><span class="sxs-lookup"><span data-stu-id="50af2-165">The schema definition format is as follows:</span></span> 

<span data-ttu-id="50af2-166">Avgränsad format</span><span class="sxs-lookup"><span data-stu-id="50af2-166">Delimited format</span></span> 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

<span data-ttu-id="50af2-167">JSON-format</span><span class="sxs-lookup"><span data-stu-id="50af2-167">JSON format</span></span> 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
<span data-ttu-id="50af2-168">Varje kolumn identifieras av plats, namn och typen.</span><span class="sxs-lookup"><span data-stu-id="50af2-168">Each column is identified by the location, name and type.</span></span> 

* <span data-ttu-id="50af2-169">Plats – är för avgränsad filformatet den positionen för det mappade värdet.</span><span class="sxs-lookup"><span data-stu-id="50af2-169">Location – For delimited file format it is the position of the mapped value.</span></span> <span data-ttu-id="50af2-170">JSON-format är jpath av mappade nyckeln.</span><span class="sxs-lookup"><span data-stu-id="50af2-170">For JSON format, it is the jpath of the mapped key.</span></span>
* <span data-ttu-id="50af2-171">Namn – visningsnamnet för kolumnen.</span><span class="sxs-lookup"><span data-stu-id="50af2-171">Name – the displayed name of the column.</span></span>
* <span data-ttu-id="50af2-172">Typ – datatypen för kolumnen.</span><span class="sxs-lookup"><span data-stu-id="50af2-172">Type – the data type of that column.</span></span>
 
<span data-ttu-id="50af2-173">Om en exempeldata användes och filformatet avgränsas schemadefinitionen Mappa alla kolumner och lägga till nya kolumner i slutet.</span><span class="sxs-lookup"><span data-stu-id="50af2-173">In case a sample data was used and file format is delimited, the schema definition must map all columns and add new columns at the end.</span></span> 

<span data-ttu-id="50af2-174">JSON tillåter delvis mappning av data, därför schemadefinitionen för JSON-formatet saknar att mappa alla nycklar som finns i en exempeldata.</span><span class="sxs-lookup"><span data-stu-id="50af2-174">JSON allows partial mapping of the data, therefore the schema definition of JSON format doesn’t have to map every key which is found in a sample data.</span></span> <span data-ttu-id="50af2-175">Det kan också mappa kolumner som inte är en del av exempeldata.</span><span class="sxs-lookup"><span data-stu-id="50af2-175">It can also map columns which are not part of the sample data.</span></span> 

## <a name="import-data"></a><span data-ttu-id="50af2-176">Importera data</span><span class="sxs-lookup"><span data-stu-id="50af2-176">Import data</span></span>

<span data-ttu-id="50af2-177">För att importera data och överföra den till Azure storage, skapa en snabbtangent för den och göra ett REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="50af2-177">To import data, you upload it to Azure storage, create an access key for it, and then make a REST API call.</span></span>

![Lägg till ny datakälla](./media/app-insights-analytics-import/analytics-upload-process.png)

<span data-ttu-id="50af2-179">Du kan utföra följande process manuellt eller konfigurera ett automatiserat system för att göra det med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="50af2-179">You can perform the following process manually, or set up an automated system to do it at regular intervals.</span></span> <span data-ttu-id="50af2-180">Du måste följa de här stegen för varje datablock som du vill importera.</span><span class="sxs-lookup"><span data-stu-id="50af2-180">You need to follow these steps for each block of data you want to import.</span></span>

1. <span data-ttu-id="50af2-181">Överföra data till [Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="50af2-181">Upload the data to [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> 

 * <span data-ttu-id="50af2-182">Blobbar kan vara en storlek på upp till 1GB okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="50af2-182">Blobs can be any size up to 1GB uncompressed.</span></span> <span data-ttu-id="50af2-183">Stora blobbar MB hundratals lämpar sig ur prestandaperspektiv.</span><span class="sxs-lookup"><span data-stu-id="50af2-183">Large blobs of hundreds of MB are ideal from a performance perspective.</span></span>
 * <span data-ttu-id="50af2-184">Du kan komprimera den med Gzip att förbättra tid och svarstid för att data ska vara tillgängliga för frågan.</span><span class="sxs-lookup"><span data-stu-id="50af2-184">You can compress it with Gzip to improve upload time and latency for the data to be available for query.</span></span> <span data-ttu-id="50af2-185">Använd den `.gz` filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="50af2-185">Use the `.gz` filename extension.</span></span>
 * <span data-ttu-id="50af2-186">Det är bäst att använda ett separat lagringskonto för detta ändamål för att undvika anrop från olika tjänster sänka prestanda.</span><span class="sxs-lookup"><span data-stu-id="50af2-186">It's best to use a separate storage account for this purpose, to avoid calls from different services slowing performance.</span></span>
 * <span data-ttu-id="50af2-187">När du skickar data i hög frekvens rekommenderas några sekunders att använda mer än ett lagringskonto av prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="50af2-187">When sending data in high frequency, every few seconds, it is recommended to use more than one storage account, for performance reasons.</span></span>

 
2. <span data-ttu-id="50af2-188">[Skapa en nyckel för signatur för delad åtkomst för blobben](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="50af2-188">[Create a Shared Access Signature key for the blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span> <span data-ttu-id="50af2-189">Nyckeln bör ha en utgångsperiod på en dag och ge läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="50af2-189">The key should have an expiration period of one day and provide read access.</span></span>
3. <span data-ttu-id="50af2-190">Skapa ett REST-anrop för att meddela Application Insights som väntar på data.</span><span class="sxs-lookup"><span data-stu-id="50af2-190">Make a REST call to notify Application Insights that data is waiting.</span></span>

 * <span data-ttu-id="50af2-191">Slutpunkt:`https://dc.services.visualstudio.com/v2/track`</span><span class="sxs-lookup"><span data-stu-id="50af2-191">Endpoint: `https://dc.services.visualstudio.com/v2/track`</span></span>
 * <span data-ttu-id="50af2-192">HTTP-metod: POST</span><span class="sxs-lookup"><span data-stu-id="50af2-192">HTTP method: POST</span></span>
 * <span data-ttu-id="50af2-193">Nyttolasten:</span><span class="sxs-lookup"><span data-stu-id="50af2-193">Payload:</span></span>

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

<span data-ttu-id="50af2-194">Platshållarna är:</span><span class="sxs-lookup"><span data-stu-id="50af2-194">The placeholders are:</span></span>

* <span data-ttu-id="50af2-195">`Blob URI with Shared Access Key`: Du får detta från proceduren för att skapa en nyckel.</span><span class="sxs-lookup"><span data-stu-id="50af2-195">`Blob URI with Shared Access Key`: You get this from the procedure for creating a key.</span></span> <span data-ttu-id="50af2-196">Det är specifika för blob.</span><span class="sxs-lookup"><span data-stu-id="50af2-196">It is specific to the blob.</span></span>
* <span data-ttu-id="50af2-197">`Schema ID`: Schemat ID genereras för din definierat schema.</span><span class="sxs-lookup"><span data-stu-id="50af2-197">`Schema ID`: The schema ID generated for your defined schema.</span></span> <span data-ttu-id="50af2-198">Informationen i det här blob bör passa schemat.</span><span class="sxs-lookup"><span data-stu-id="50af2-198">The data in this blob should conform to the schema.</span></span>
* <span data-ttu-id="50af2-199">`DateTime`: Den tid då begäran har skickats, UTC.</span><span class="sxs-lookup"><span data-stu-id="50af2-199">`DateTime`: The time at which the request is submitted, UTC.</span></span> <span data-ttu-id="50af2-200">Vi godkänna dessa format: ISO8601 (som ”2016-01-01 13:45:01”); Rfc822 (”ons, 14 Dec 16 14:57:01 + 0000”); RFC850 (”onsdag, 14-Dec-16 14:57:00 UTC”); RFC1123 (”ons, 14 Dec 2016 14:57:00 + 0000”).</span><span class="sxs-lookup"><span data-stu-id="50af2-200">We accept these formats: ISO8601 (like "2016-01-01 13:45:01"); RFC822 ("Wed, 14 Dec 16 14:57:01 +0000"); RFC850 ("Wednesday, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Wed, 14 Dec 2016 14:57:00 +0000").</span></span>
* <span data-ttu-id="50af2-201">`Instrumentation key`i Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="50af2-201">`Instrumentation key` of your Application Insights resource.</span></span>

<span data-ttu-id="50af2-202">Data är tillgängliga i Analytics efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="50af2-202">The data is available in Analytics after a few minutes.</span></span>

## <a name="error-responses"></a><span data-ttu-id="50af2-203">Felsvar</span><span class="sxs-lookup"><span data-stu-id="50af2-203">Error responses</span></span>

* <span data-ttu-id="50af2-204">**400 Felaktig begäran**: Anger att nyttolasten i begäran är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="50af2-204">**400 bad request**: indicates that the request payload is invalid.</span></span> <span data-ttu-id="50af2-205">Kontrollera:</span><span class="sxs-lookup"><span data-stu-id="50af2-205">Check:</span></span>
 * <span data-ttu-id="50af2-206">Rätt instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="50af2-206">Correct instrumentation key.</span></span>
 * <span data-ttu-id="50af2-207">Giltig tid-värde.</span><span class="sxs-lookup"><span data-stu-id="50af2-207">Valid time value.</span></span> <span data-ttu-id="50af2-208">Det bör vara nu i UTC-tid.</span><span class="sxs-lookup"><span data-stu-id="50af2-208">It should be the time now in UTC.</span></span>
 * <span data-ttu-id="50af2-209">JSON av händelsen överensstämmer med schemat.</span><span class="sxs-lookup"><span data-stu-id="50af2-209">JSON of the event conforms to the schema.</span></span>
* <span data-ttu-id="50af2-210">**403 Nekad**: blob som du har skickat är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="50af2-210">**403 Forbidden**: The blob you've sent is not accessible.</span></span> <span data-ttu-id="50af2-211">Se till att den delade åtkomstnyckeln är giltigt och inte har gått ut.</span><span class="sxs-lookup"><span data-stu-id="50af2-211">Make sure that the shared access key is valid and has not expired.</span></span>
* <span data-ttu-id="50af2-212">**Det gick inte att hitta 404**:</span><span class="sxs-lookup"><span data-stu-id="50af2-212">**404 Not Found**:</span></span>
 * <span data-ttu-id="50af2-213">Blobben finns inte.</span><span class="sxs-lookup"><span data-stu-id="50af2-213">The blob doesn't exist.</span></span>
 * <span data-ttu-id="50af2-214">Målentiteten är felaktigt.</span><span class="sxs-lookup"><span data-stu-id="50af2-214">The sourceId is wrong.</span></span>

<span data-ttu-id="50af2-215">Mer detaljerad information finns i svarsmeddelandet för fel.</span><span class="sxs-lookup"><span data-stu-id="50af2-215">More detailed information is available in the response error message.</span></span>


## <a name="sample-code"></a><span data-ttu-id="50af2-216">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="50af2-216">Sample code</span></span>

<span data-ttu-id="50af2-217">Den här koden används den [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="50af2-217">This code uses the [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet package.</span></span>

### <a name="classes"></a><span data-ttu-id="50af2-218">Klasser</span><span class="sxs-lookup"><span data-stu-id="50af2-218">Classes</span></span>

```C#
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a><span data-ttu-id="50af2-219">För in data</span><span class="sxs-lookup"><span data-stu-id="50af2-219">Ingest data</span></span>

<span data-ttu-id="50af2-220">Använd den här koden för varje blob.</span><span class="sxs-lookup"><span data-stu-id="50af2-220">Use this code for each blob.</span></span> 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a><span data-ttu-id="50af2-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50af2-221">Next steps</span></span>

* [<span data-ttu-id="50af2-222">Genomgång av Log Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="50af2-222">Tour of the Log Analytics query language</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="50af2-223">Använd *Logstash* att skicka data till Application Insights</span><span class="sxs-lookup"><span data-stu-id="50af2-223">Use *Logstash* to send data to Application Insights</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
