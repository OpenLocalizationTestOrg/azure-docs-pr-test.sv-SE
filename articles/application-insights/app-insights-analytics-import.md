---
title: aaaImport dina data tooAnalytics i Azure Application Insights | Microsoft Docs
description: "Importera statiska data toojoin med app telemetri eller importera en separat data dataströmmen tooquery med Analytics."
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
ms.openlocfilehash: 7a3a3c9155adc1885dd366ddb13dda80bb894adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-analytics"></a><span data-ttu-id="56b47-104">Importera data till Analytics</span><span class="sxs-lookup"><span data-stu-id="56b47-104">Import data into Analytics</span></span>

<span data-ttu-id="56b47-105">Importera inga tabelldata i [Analytics](app-insights-analytics.md), antingen toojoin med [Programinsikter](app-insights-overview.md) telemetri från din app, eller så att du kan analysera dem som en separat dataström.</span><span class="sxs-lookup"><span data-stu-id="56b47-105">Import any tabular data into [Analytics](app-insights-analytics.md), either toojoin it with [Application Insights](app-insights-overview.md) telemetry from your app, or so that you can analyze it as a separate stream.</span></span> <span data-ttu-id="56b47-106">Analytics är en kraftfull query language väl lämpade tooanalyzing omfattande tidsstämplad dataströmmar telemetri.</span><span class="sxs-lookup"><span data-stu-id="56b47-106">Analytics is a powerful query language well-suited tooanalyzing high-volume timestamped streams of telemetry.</span></span>

<span data-ttu-id="56b47-107">Du kan importera data till Analytics med hjälp av ditt eget schema.</span><span class="sxs-lookup"><span data-stu-id="56b47-107">You can import data into Analytics using your own schema.</span></span> <span data-ttu-id="56b47-108">Den har inte toouse hello standard Application Insights scheman som begäran eller trace.</span><span class="sxs-lookup"><span data-stu-id="56b47-108">It doesn't have toouse hello standard Application Insights schemas such as request or trace.</span></span>

<span data-ttu-id="56b47-109">Du kan importera JSON eller DSV (avgränsare med kommaavgränsade värden - kommatecken eller semikolon fliken) filer.</span><span class="sxs-lookup"><span data-stu-id="56b47-109">You can import JSON or DSV (delimiter-separated values - comma, semicolon or tab) files.</span></span>

<span data-ttu-id="56b47-110">Det finns tre situationer där importerar tooAnalytics är användbar:</span><span class="sxs-lookup"><span data-stu-id="56b47-110">There are three situations where importing tooAnalytics is useful:</span></span>

* <span data-ttu-id="56b47-111">**Anslut med app telemetri.**</span><span class="sxs-lookup"><span data-stu-id="56b47-111">**Join with app telemetry.**</span></span> <span data-ttu-id="56b47-112">Exempelvis kan du importera en tabell som mappar URL: er från din webbplats toomore läsbar rubriker.</span><span class="sxs-lookup"><span data-stu-id="56b47-112">For example, you could import a table that maps URLs from your website toomore readable page titles.</span></span> <span data-ttu-id="56b47-113">Du kan skapa en instrumentpanel diagramrapport som visar hello tio populäraste sidorna på webbplatsen i analyser.</span><span class="sxs-lookup"><span data-stu-id="56b47-113">In Analytics, you can create a dashboard chart report that shows hello ten most popular pages in your website.</span></span> <span data-ttu-id="56b47-114">Det kan nu visa hello sidnamn i stället för hello URL: er.</span><span class="sxs-lookup"><span data-stu-id="56b47-114">Now it can show hello page titles instead of hello URLs.</span></span>
* <span data-ttu-id="56b47-115">**Korrelera programmets telemetri** med andra källor, till exempel nätverkstrafik, server-data eller CDN loggfiler.</span><span class="sxs-lookup"><span data-stu-id="56b47-115">**Correlate your application telemetry** with other sources such as network traffic, server data, or CDN log files.</span></span>
* <span data-ttu-id="56b47-116">**Tillämpa Analytics tooa separat dataström.**</span><span class="sxs-lookup"><span data-stu-id="56b47-116">**Apply Analytics tooa separate data stream.**</span></span> <span data-ttu-id="56b47-117">Application Insights Analytics är ett kraftfullt verktyg som fungerar bra med sparse, tidsstämplad dataströmmar - mycket bättre än SQL i många fall.</span><span class="sxs-lookup"><span data-stu-id="56b47-117">Application Insights Analytics is a powerful tool, that works well with sparse, timestamped streams - much better than SQL in many cases.</span></span> <span data-ttu-id="56b47-118">Om du har en sådan dataström från någon annan källa kan analysera du dem med Analytics.</span><span class="sxs-lookup"><span data-stu-id="56b47-118">If you have such a stream from some other source, you can analyze it with Analytics.</span></span>

<span data-ttu-id="56b47-119">Skicka tooyour datakälla är enkelt.</span><span class="sxs-lookup"><span data-stu-id="56b47-119">Sending data tooyour data source is easy.</span></span> 

1. <span data-ttu-id="56b47-120">(En gång) Definiera hello schemat för dina data i en-datakälla.</span><span class="sxs-lookup"><span data-stu-id="56b47-120">(One time) Define hello schema of your data in a 'data source'.</span></span>
2. <span data-ttu-id="56b47-121">(Med jämna mellanrum) Överför tooAzure datalagringen och anropa hello REST API toonotify oss att nya data väntar införandet.</span><span class="sxs-lookup"><span data-stu-id="56b47-121">(Periodically) Upload your data tooAzure storage, and call hello REST API toonotify us that new data is waiting for ingestion.</span></span> <span data-ttu-id="56b47-122">Hello data är tillgängliga för frågor i Analytics inom några minuter.</span><span class="sxs-lookup"><span data-stu-id="56b47-122">Within a few minutes, hello data is available for query in Analytics.</span></span>

<span data-ttu-id="56b47-123">hello frekvensen av hello överför definieras av dig och hur snabbt vill du att dina data toobe som är tillgängligt för frågor.</span><span class="sxs-lookup"><span data-stu-id="56b47-123">hello frequency of hello upload is defined by you and how fast would you like your data toobe available for queries.</span></span> <span data-ttu-id="56b47-124">Det är effektivare tooupload data i större segment, men inte är större än 1GB.</span><span class="sxs-lookup"><span data-stu-id="56b47-124">It is more efficient tooupload data in larger chunks, but not larger than 1GB.</span></span>

> [!NOTE]
> <span data-ttu-id="56b47-125">*Har mycket data sources tooanalyze?*</span><span class="sxs-lookup"><span data-stu-id="56b47-125">*Got lots of data sources tooanalyze?*</span></span> [<span data-ttu-id="56b47-126">*Överväg att använda* logstash *tooship data till Application Insights.*</span><span class="sxs-lookup"><span data-stu-id="56b47-126">*Consider using* logstash *tooship your data into Application Insights.*</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a><span data-ttu-id="56b47-127">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="56b47-127">Before you start</span></span>

<span data-ttu-id="56b47-128">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="56b47-128">You need:</span></span>

1. <span data-ttu-id="56b47-129">En Application Insights-resurs i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="56b47-129">An Application Insights resource in Microsoft Azure.</span></span>

 * <span data-ttu-id="56b47-130">Om du vill tooanalyze dina data separat från andra telemetri [skapar en ny Application Insights-resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="56b47-130">If you want tooanalyze your data separately from any other telemetry, [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span>
 * <span data-ttu-id="56b47-131">Om du ansluter eller jämförelse av dina data med telemetri från en app som redan har konfigurerats med Application Insights, kan du använda hello resurs för appen.</span><span class="sxs-lookup"><span data-stu-id="56b47-131">If you're joining or comparing your data with telemetry from an app that is already set up with Application Insights, then you can use hello resource for that app.</span></span>
 * <span data-ttu-id="56b47-132">Medarbetare eller ägare åtkomst toothat resurs.</span><span class="sxs-lookup"><span data-stu-id="56b47-132">Contributor or owner access toothat resource.</span></span>
 
2. <span data-ttu-id="56b47-133">Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="56b47-133">Azure storage.</span></span> <span data-ttu-id="56b47-134">Du överför tooAzure lagring och Analytics hämtar dina data därifrån.</span><span class="sxs-lookup"><span data-stu-id="56b47-134">You upload tooAzure storage, and Analytics gets your data from there.</span></span> 

 * <span data-ttu-id="56b47-135">Vi rekommenderar att du skapar ett dedikerat storage-konto för din BLOB.</span><span class="sxs-lookup"><span data-stu-id="56b47-135">We recommend you create a dedicated storage account for your blobs.</span></span> <span data-ttu-id="56b47-136">Om dina blobbar som delas med andra processer, tar det längre tid för våra processer tooread dina blobbar.</span><span class="sxs-lookup"><span data-stu-id="56b47-136">If your blobs are shared with other processes, it takes longer for our processes tooread your blobs.</span></span>


## <a name="define-your-schema"></a><span data-ttu-id="56b47-137">Definiera schemat</span><span class="sxs-lookup"><span data-stu-id="56b47-137">Define your schema</span></span>

<span data-ttu-id="56b47-138">Innan du kan importera data, måste du definiera en *datakällan* som anger hello schemat för dina data.</span><span class="sxs-lookup"><span data-stu-id="56b47-138">Before you can import data, you must define a *data source,* which specifies hello schema of your data.</span></span>
<span data-ttu-id="56b47-139">Du kan ha too50 datakällor i en enda Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="56b47-139">You can have up too50 data sources in a single Application Insights resource</span></span>

1. <span data-ttu-id="56b47-140">Starta guiden hello datakälla.</span><span class="sxs-lookup"><span data-stu-id="56b47-140">Start hello data source wizard.</span></span> <span data-ttu-id="56b47-141">Använd knappen ”Lägg till ny datakälla”.</span><span class="sxs-lookup"><span data-stu-id="56b47-141">Use "Add new data source" button.</span></span> <span data-ttu-id="56b47-142">Du kan också - klicka på inställningsknappen i övre högra hörnet och välj ”datakällor” i listrutan.</span><span class="sxs-lookup"><span data-stu-id="56b47-142">Alternatively - click on settings button in right upper corner and choose "Data Sources" in dropdown menu.</span></span>

    ![Lägg till ny datakälla](./media/app-insights-analytics-import/add-new-data-source.png)

    <span data-ttu-id="56b47-144">Ange ett namn för den nya datakällan.</span><span class="sxs-lookup"><span data-stu-id="56b47-144">Provide a name for your new data source.</span></span>

2. <span data-ttu-id="56b47-145">Definiera format hello-filer som du överför.</span><span class="sxs-lookup"><span data-stu-id="56b47-145">Define format of hello files that you will upload.</span></span>

    <span data-ttu-id="56b47-146">Du kan definiera hello format manuellt eller ladda upp en exempelfil.</span><span class="sxs-lookup"><span data-stu-id="56b47-146">You can either define hello format manually, or upload a sample file.</span></span>

    <span data-ttu-id="56b47-147">Om hello data i CSV-format, vara hello första raden i exempel hello kolumnrubrikerna.</span><span class="sxs-lookup"><span data-stu-id="56b47-147">If hello data is in CSV format, hello first row of hello sample can be column headers.</span></span> <span data-ttu-id="56b47-148">Du kan ändra hello fältnamnen i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="56b47-148">You can change hello field names in hello next step.</span></span>

    <span data-ttu-id="56b47-149">hello exempel ska innehålla minst 10 rader eller poster av data.</span><span class="sxs-lookup"><span data-stu-id="56b47-149">hello sample should include at least 10 rows or records of data.</span></span>

    <span data-ttu-id="56b47-150">Kolumnen eller fältnamn bör ha alfanumeriskt namn (utan blanksteg eller skiljetecken).</span><span class="sxs-lookup"><span data-stu-id="56b47-150">Column or field names should have alphanumeric names (without spaces or punctuation).</span></span>

    ![Överför en exempelfil](./media/app-insights-analytics-import/sample-data-file.png)


3. <span data-ttu-id="56b47-152">Granska hello schema som hello guiden har stött på.</span><span class="sxs-lookup"><span data-stu-id="56b47-152">Review hello schema that hello wizard has got.</span></span> <span data-ttu-id="56b47-153">Om den härleda hello typer från ett exempel behöva tooadjust hello härleda typer av hello kolumner.</span><span class="sxs-lookup"><span data-stu-id="56b47-153">If it inferred hello types from a sample, you might need tooadjust hello inferred types of hello columns.</span></span>

    ![Granska hello härleda schema](./media/app-insights-analytics-import/data-source-review-schema.png)

 * <span data-ttu-id="56b47-155">(Valfritt.) Överför schemadefinition.</span><span class="sxs-lookup"><span data-stu-id="56b47-155">(Optional.) Upload a schema definition.</span></span> <span data-ttu-id="56b47-156">Se hello-formatet.</span><span class="sxs-lookup"><span data-stu-id="56b47-156">See hello format below.</span></span>

 * <span data-ttu-id="56b47-157">Välj en tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="56b47-157">Select a Timestamp.</span></span> <span data-ttu-id="56b47-158">Alla data i Analytics måste ha något tidsstämpelsfält.</span><span class="sxs-lookup"><span data-stu-id="56b47-158">All data in Analytics must have a timestamp field.</span></span> <span data-ttu-id="56b47-159">Det måste vara av typen `datetime`, men som inte har toobe med namnet 'tidsstämpel'.</span><span class="sxs-lookup"><span data-stu-id="56b47-159">It must have type `datetime`, but it doesn't have toobe named 'timestamp'.</span></span> <span data-ttu-id="56b47-160">Om dina data har en kolumn som innehåller datum och tid med ISO-format, väljer du det som hello kolumn för tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="56b47-160">If your data has a column containing a date and time in ISO format, choose this as hello timestamp column.</span></span> <span data-ttu-id="56b47-161">Annars väljer du ”som data anlänt” och hello importen lägger till något tidsstämpelsfält.</span><span class="sxs-lookup"><span data-stu-id="56b47-161">Otherwise, choose "as data arrived", and hello import process will add a timestamp field.</span></span>

5. <span data-ttu-id="56b47-162">Skapa hello datakälla.</span><span class="sxs-lookup"><span data-stu-id="56b47-162">Create hello data source.</span></span>

### <a name="schema-definition-file-format"></a><span data-ttu-id="56b47-163">Schemaformat för paketdefinitionsfiler</span><span class="sxs-lookup"><span data-stu-id="56b47-163">Schema definition file format</span></span>

<span data-ttu-id="56b47-164">Du kan läsa in hello schemadefinition från en fil i stället för att redigera hello schemat i Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="56b47-164">Instead of editing hello schema in UI, you can load hello schema definition from a file.</span></span> <span data-ttu-id="56b47-165">hello schema definition format är följande:</span><span class="sxs-lookup"><span data-stu-id="56b47-165">hello schema definition format is as follows:</span></span> 

<span data-ttu-id="56b47-166">Avgränsad format</span><span class="sxs-lookup"><span data-stu-id="56b47-166">Delimited format</span></span> 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

<span data-ttu-id="56b47-167">JSON-format</span><span class="sxs-lookup"><span data-stu-id="56b47-167">JSON format</span></span> 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
<span data-ttu-id="56b47-168">Varje kolumn identifieras av hello plats, namn och typen.</span><span class="sxs-lookup"><span data-stu-id="56b47-168">Each column is identified by hello location, name and type.</span></span> 

* <span data-ttu-id="56b47-169">Plats – är för avgränsad filformatet den hello positionen för hello mappade värdet.</span><span class="sxs-lookup"><span data-stu-id="56b47-169">Location – For delimited file format it is hello position of hello mapped value.</span></span> <span data-ttu-id="56b47-170">JSON-format är det hello jpath hello mappas nyckel.</span><span class="sxs-lookup"><span data-stu-id="56b47-170">For JSON format, it is hello jpath of hello mapped key.</span></span>
* <span data-ttu-id="56b47-171">Namn – hello visas namnet på hello-kolumn.</span><span class="sxs-lookup"><span data-stu-id="56b47-171">Name – hello displayed name of hello column.</span></span>
* <span data-ttu-id="56b47-172">Typ – hello datatypen för kolumnen.</span><span class="sxs-lookup"><span data-stu-id="56b47-172">Type – hello data type of that column.</span></span>
 
<span data-ttu-id="56b47-173">Om en exempeldata användes och filformatet avgränsas hello schemadefinition Mappa alla kolumner och lägga till nya kolumner hello slutet.</span><span class="sxs-lookup"><span data-stu-id="56b47-173">In case a sample data was used and file format is delimited, hello schema definition must map all columns and add new columns at hello end.</span></span> 

<span data-ttu-id="56b47-174">JSON tillåter delvis mappning av hello data, därför hello schemadefinitionen för JSON-formatet saknar toomap alla nycklar som finns i en exempeldata.</span><span class="sxs-lookup"><span data-stu-id="56b47-174">JSON allows partial mapping of hello data, therefore hello schema definition of JSON format doesn’t have toomap every key which is found in a sample data.</span></span> <span data-ttu-id="56b47-175">Det kan också mappa kolumner som inte är en del av hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="56b47-175">It can also map columns which are not part of hello sample data.</span></span> 

## <a name="import-data"></a><span data-ttu-id="56b47-176">Importera data</span><span class="sxs-lookup"><span data-stu-id="56b47-176">Import data</span></span>

<span data-ttu-id="56b47-177">tooimport data, ladda upp den tooAzure lagring, skapa en snabbtangent för den och göra ett REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="56b47-177">tooimport data, you upload it tooAzure storage, create an access key for it, and then make a REST API call.</span></span>

![Lägg till ny datakälla](./media/app-insights-analytics-import/analytics-upload-process.png)

<span data-ttu-id="56b47-179">Du kan utföra hello följande process manuellt eller ställa in en automatisk toodo med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="56b47-179">You can perform hello following process manually, or set up an automated system toodo it at regular intervals.</span></span> <span data-ttu-id="56b47-180">Du måste toofollow dessa steg för varje datablock som du vill använda tooimport.</span><span class="sxs-lookup"><span data-stu-id="56b47-180">You need toofollow these steps for each block of data you want tooimport.</span></span>

1. <span data-ttu-id="56b47-181">Ladda upp hello data för[Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="56b47-181">Upload hello data too[Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> 

 * <span data-ttu-id="56b47-182">Blobbar kan vara valfri storlek in too1GB okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="56b47-182">Blobs can be any size up too1GB uncompressed.</span></span> <span data-ttu-id="56b47-183">Stora blobbar MB hundratals lämpar sig ur prestandaperspektiv.</span><span class="sxs-lookup"><span data-stu-id="56b47-183">Large blobs of hundreds of MB are ideal from a performance perspective.</span></span>
 * <span data-ttu-id="56b47-184">Du kan komprimera den med Gzip tooimprove tid och svarstid för hello data toobe tillgängliga för frågor.</span><span class="sxs-lookup"><span data-stu-id="56b47-184">You can compress it with Gzip tooimprove upload time and latency for hello data toobe available for query.</span></span> <span data-ttu-id="56b47-185">Använd hello `.gz` filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="56b47-185">Use hello `.gz` filename extension.</span></span>
 * <span data-ttu-id="56b47-186">Det är bästa toouse ett separat lagringskonto för detta ändamål tooavoid anrop från olika tjänster sänka prestanda.</span><span class="sxs-lookup"><span data-stu-id="56b47-186">It's best toouse a separate storage account for this purpose, tooavoid calls from different services slowing performance.</span></span>
 * <span data-ttu-id="56b47-187">När du skickar data i hög frekvens rekommenderas några sekunders toouse mer än ett lagringskonto av prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="56b47-187">When sending data in high frequency, every few seconds, it is recommended toouse more than one storage account, for performance reasons.</span></span>

 
2. <span data-ttu-id="56b47-188">[Skapa en nyckel för signatur för delad åtkomst för hello blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span><span class="sxs-lookup"><span data-stu-id="56b47-188">[Create a Shared Access Signature key for hello blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md).</span></span> <span data-ttu-id="56b47-189">hello nyckel bör ha en utgångsperiod på en dag och ger läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="56b47-189">hello key should have an expiration period of one day and provide read access.</span></span>
3. <span data-ttu-id="56b47-190">Skapa ett REST-anrop toonotify Application Insights som väntar på data.</span><span class="sxs-lookup"><span data-stu-id="56b47-190">Make a REST call toonotify Application Insights that data is waiting.</span></span>

 * <span data-ttu-id="56b47-191">Slutpunkt:`https://dc.services.visualstudio.com/v2/track`</span><span class="sxs-lookup"><span data-stu-id="56b47-191">Endpoint: `https://dc.services.visualstudio.com/v2/track`</span></span>
 * <span data-ttu-id="56b47-192">HTTP-metod: POST</span><span class="sxs-lookup"><span data-stu-id="56b47-192">HTTP method: POST</span></span>
 * <span data-ttu-id="56b47-193">Nyttolasten:</span><span class="sxs-lookup"><span data-stu-id="56b47-193">Payload:</span></span>

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

<span data-ttu-id="56b47-194">hello platshållare är:</span><span class="sxs-lookup"><span data-stu-id="56b47-194">hello placeholders are:</span></span>

* <span data-ttu-id="56b47-195">`Blob URI with Shared Access Key`: Du behöver från hello procedur för att skapa en nyckel.</span><span class="sxs-lookup"><span data-stu-id="56b47-195">`Blob URI with Shared Access Key`: You get this from hello procedure for creating a key.</span></span> <span data-ttu-id="56b47-196">Är det specifika toohello blob.</span><span class="sxs-lookup"><span data-stu-id="56b47-196">It is specific toohello blob.</span></span>
* <span data-ttu-id="56b47-197">`Schema ID`: hello schema-ID har genererats för din definierat schema.</span><span class="sxs-lookup"><span data-stu-id="56b47-197">`Schema ID`: hello schema ID generated for your defined schema.</span></span> <span data-ttu-id="56b47-198">hello data i den här blob ska följa toohello schema.</span><span class="sxs-lookup"><span data-stu-id="56b47-198">hello data in this blob should conform toohello schema.</span></span>
* <span data-ttu-id="56b47-199">`DateTime`: hello UTC-tid på vilka hello begäran har skickats.</span><span class="sxs-lookup"><span data-stu-id="56b47-199">`DateTime`: hello time at which hello request is submitted, UTC.</span></span> <span data-ttu-id="56b47-200">Vi godkänna dessa format: ISO8601 (som ”2016-01-01 13:45:01”); Rfc822 (”ons, 14 Dec 16 14:57:01 + 0000”); RFC850 (”onsdag, 14-Dec-16 14:57:00 UTC”); RFC1123 (”ons, 14 Dec 2016 14:57:00 + 0000”).</span><span class="sxs-lookup"><span data-stu-id="56b47-200">We accept these formats: ISO8601 (like "2016-01-01 13:45:01"); RFC822 ("Wed, 14 Dec 16 14:57:01 +0000"); RFC850 ("Wednesday, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Wed, 14 Dec 2016 14:57:00 +0000").</span></span>
* <span data-ttu-id="56b47-201">`Instrumentation key`i Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="56b47-201">`Instrumentation key` of your Application Insights resource.</span></span>

<span data-ttu-id="56b47-202">hello data är tillgängliga i Analytics efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="56b47-202">hello data is available in Analytics after a few minutes.</span></span>

## <a name="error-responses"></a><span data-ttu-id="56b47-203">Felsvar</span><span class="sxs-lookup"><span data-stu-id="56b47-203">Error responses</span></span>

* <span data-ttu-id="56b47-204">**400 Felaktig begäran**: Anger att hello-nyttolasten i begäran är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="56b47-204">**400 bad request**: indicates that hello request payload is invalid.</span></span> <span data-ttu-id="56b47-205">Kontrollera:</span><span class="sxs-lookup"><span data-stu-id="56b47-205">Check:</span></span>
 * <span data-ttu-id="56b47-206">Rätt instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="56b47-206">Correct instrumentation key.</span></span>
 * <span data-ttu-id="56b47-207">Giltig tid-värde.</span><span class="sxs-lookup"><span data-stu-id="56b47-207">Valid time value.</span></span> <span data-ttu-id="56b47-208">Det bör vara hello nu i UTC-tid.</span><span class="sxs-lookup"><span data-stu-id="56b47-208">It should be hello time now in UTC.</span></span>
 * <span data-ttu-id="56b47-209">JSON hello händelse överensstämmer toohello schema.</span><span class="sxs-lookup"><span data-stu-id="56b47-209">JSON of hello event conforms toohello schema.</span></span>
* <span data-ttu-id="56b47-210">**403 Nekad**: hello blob som du har skickat är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="56b47-210">**403 Forbidden**: hello blob you've sent is not accessible.</span></span> <span data-ttu-id="56b47-211">Kontrollera att nyckeln hello delad åtkomst är giltigt och inte har gått ut.</span><span class="sxs-lookup"><span data-stu-id="56b47-211">Make sure that hello shared access key is valid and has not expired.</span></span>
* <span data-ttu-id="56b47-212">**Det gick inte att hitta 404**:</span><span class="sxs-lookup"><span data-stu-id="56b47-212">**404 Not Found**:</span></span>
 * <span data-ttu-id="56b47-213">hello-blob finns inte.</span><span class="sxs-lookup"><span data-stu-id="56b47-213">hello blob doesn't exist.</span></span>
 * <span data-ttu-id="56b47-214">hello sourceId är felaktigt.</span><span class="sxs-lookup"><span data-stu-id="56b47-214">hello sourceId is wrong.</span></span>

<span data-ttu-id="56b47-215">Mer detaljerad information finns i hello svar felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="56b47-215">More detailed information is available in hello response error message.</span></span>


## <a name="sample-code"></a><span data-ttu-id="56b47-216">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="56b47-216">Sample code</span></span>

<span data-ttu-id="56b47-217">Den här koden använder hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="56b47-217">This code uses hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet package.</span></span>

### <a name="classes"></a><span data-ttu-id="56b47-218">Klasser</span><span class="sxs-lookup"><span data-stu-id="56b47-218">Classes</span></span>

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

### <a name="ingest-data"></a><span data-ttu-id="56b47-219">För in data</span><span class="sxs-lookup"><span data-stu-id="56b47-219">Ingest data</span></span>

<span data-ttu-id="56b47-220">Använd den här koden för varje blob.</span><span class="sxs-lookup"><span data-stu-id="56b47-220">Use this code for each blob.</span></span> 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a><span data-ttu-id="56b47-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56b47-221">Next steps</span></span>

* [<span data-ttu-id="56b47-222">Genomgång av hello Log Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="56b47-222">Tour of hello Log Analytics query language</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="56b47-223">Använd *Logstash* toosend data tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="56b47-223">Use *Logstash* toosend data tooApplication Insights</span></span>](https://github.com/Microsoft/logstash-output-application-insights)
