---
title: komprimering och aaaFile format i Azure Data Factory | Microsoft Docs
description: "Läs mer om hello-filformat som stöds av Azure Data Factory."
keywords: BLOB-data, azure blob-kopia
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 9d40517b059fc533776bcc088db8c531ee5b003d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a><span data-ttu-id="b9fa2-104">Fil- och komprimering format som stöds av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b9fa2-104">File and compression formats supported by Azure Data Factory</span></span>
<span data-ttu-id="b9fa2-105">*Det här avsnittet gäller toohello följande kopplingar: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [filsystemet](data-factory-onprem-file-system-connector.md), [ FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), och [SFTP](data-factory-sftp-connector.md).*</span><span class="sxs-lookup"><span data-stu-id="b9fa2-105">*This topic applies toohello following connectors: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [File System](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), and [SFTP](data-factory-sftp-connector.md).*</span></span>

<span data-ttu-id="b9fa2-106">Azure Data Factory stöder hello följande filtyper format:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-106">Azure Data Factory supports hello following file format types:</span></span>

* [<span data-ttu-id="b9fa2-107">Textformat</span><span class="sxs-lookup"><span data-stu-id="b9fa2-107">Text format</span></span>](#text-format)
* [<span data-ttu-id="b9fa2-108">JSON-format</span><span class="sxs-lookup"><span data-stu-id="b9fa2-108">JSON format</span></span>](#json-format)
* [<span data-ttu-id="b9fa2-109">Avro-formatet</span><span class="sxs-lookup"><span data-stu-id="b9fa2-109">Avro format</span></span>](#avro-format)
* [<span data-ttu-id="b9fa2-110">ORC-format</span><span class="sxs-lookup"><span data-stu-id="b9fa2-110">ORC format</span></span>](#orc-format)
* [<span data-ttu-id="b9fa2-111">Parkettgolv format</span><span class="sxs-lookup"><span data-stu-id="b9fa2-111">Parquet format</span></span>](#parquet-format)

## <a name="text-format"></a><span data-ttu-id="b9fa2-112">Textformat</span><span class="sxs-lookup"><span data-stu-id="b9fa2-112">Text format</span></span>
<span data-ttu-id="b9fa2-113">Om du vill tooread från en textfil eller skriva tooa textfil, ange hello `type` egenskap i hello `format` avsnitt i hello datamängden för**TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-113">If you want tooread from a text file or write tooa text file, set hello `type` property in hello `format` section of hello dataset too**TextFormat**.</span></span> <span data-ttu-id="b9fa2-114">Du kan också ange hello följande **valfria** egenskaper i hello `format` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-114">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="b9fa2-115">Se [TextFormat exempel](#textformat-example) avsnittet om hur tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-115">See [TextFormat example](#textformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="b9fa2-116">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b9fa2-116">Property</span></span> | <span data-ttu-id="b9fa2-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b9fa2-117">Description</span></span> | <span data-ttu-id="b9fa2-118">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="b9fa2-118">Allowed values</span></span> | <span data-ttu-id="b9fa2-119">Krävs</span><span class="sxs-lookup"><span data-stu-id="b9fa2-119">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b9fa2-120">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="b9fa2-120">columnDelimiter</span></span> |<span data-ttu-id="b9fa2-121">hello tecknet används tooseparate kolumner i en fil.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-121">hello character used tooseparate columns in a file.</span></span> <span data-ttu-id="b9fa2-122">Kan du toouse en sällsynt icke utskrivbara tecken som kanske inte är troligt finns i dina data.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-122">You can consider toouse a rare unprintable char that may not likely exists in your data.</span></span> <span data-ttu-id="b9fa2-123">Till exempel ange ”\u0001” som representerar Start av rubriken (SOH).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-123">For example, specify "\u0001", which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="b9fa2-124">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-124">Only one character is allowed.</span></span> <span data-ttu-id="b9fa2-125">Hej **standard** värdet är **kommatecken (',')**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-125">hello **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="b9fa2-126">toouse Unicode-tecken finns för[Unicode-tecken](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello motsvarande kod för den.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-126">toouse a Unicode character, refer too[Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello corresponding code for it.</span></span> |<span data-ttu-id="b9fa2-127">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-127">No</span></span> |
| <span data-ttu-id="b9fa2-128">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="b9fa2-128">rowDelimiter</span></span> |<span data-ttu-id="b9fa2-129">hello tecknet används tooseparate rader i en fil.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-129">hello character used tooseparate rows in a file.</span></span> |<span data-ttu-id="b9fa2-130">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-130">Only one character is allowed.</span></span> <span data-ttu-id="b9fa2-131">Hej **standard** värdet är något av följande värden på Läs hello: **[”\r\n”, ”\r”, ”\n”]** och **”\r\n”** vid skrivning.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-131">hello **default** value is any of hello following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="b9fa2-132">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-132">No</span></span> |
| <span data-ttu-id="b9fa2-133">escapeChar</span><span class="sxs-lookup"><span data-stu-id="b9fa2-133">escapeChar</span></span> |<span data-ttu-id="b9fa2-134">hello specialtecken används tooescape avgränsare för en kolumn i hello innehållet i indatafilen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-134">hello special character used tooescape a column delimiter in hello content of input file.</span></span> <br/><br/><span data-ttu-id="b9fa2-135">Du kan inte ange både escapeChar och quoteChar för en tabell.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-135">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="b9fa2-136">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-136">Only one character is allowed.</span></span> <span data-ttu-id="b9fa2-137">Inget standardvärde.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-137">No default value.</span></span> <br/><br/><span data-ttu-id="b9fa2-138">Exempel: Om du har kommatecken (', ') som hello kolumnen avgränsare, men du vill att toohave hello kommatecken tecknet i texten hello (exempel: ”Hello, world”), kan du definiera '$' som hello escape-tecken och använda strängen ”$Hello, world” i hello källan.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-138">Example: if you have comma (',') as hello column delimiter but you want toohave hello comma character in hello text (example: "Hello, world"), you can define ‘$’ as hello escape character and use string "Hello$, world" in hello source.</span></span> |<span data-ttu-id="b9fa2-139">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-139">No</span></span> |
| <span data-ttu-id="b9fa2-140">quoteChar</span><span class="sxs-lookup"><span data-stu-id="b9fa2-140">quoteChar</span></span> |<span data-ttu-id="b9fa2-141">hello tecknet används tooquote ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-141">hello character used tooquote a string value.</span></span> <span data-ttu-id="b9fa2-142">hello kolumn- och avgränsare i hello citattecken skulle behandlas som en del av hello strängvärde.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-142">hello column and row delimiters inside hello quote characters would be treated as part of hello string value.</span></span> <span data-ttu-id="b9fa2-143">Den här egenskapen är tillämpliga tooboth indata och utdata datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-143">This property is applicable tooboth input and output datasets.</span></span><br/><br/><span data-ttu-id="b9fa2-144">Du kan inte ange både escapeChar och quoteChar för en tabell.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-144">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="b9fa2-145">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-145">Only one character is allowed.</span></span> <span data-ttu-id="b9fa2-146">Inget standardvärde.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-146">No default value.</span></span> <br/><br/><span data-ttu-id="b9fa2-147">Till exempel om du har kommatecken (', ') hello kolumnen avgränsare, men du vill toohave kommatecken tecknet i texten hello (exempel: < Hello, world >), kan du definiera ”(dubbla citattecken) som hello citattecken och Använd hello sträng” Hello, world ”i hello källan.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-147">For example, if you have comma (',') as hello column delimiter but you want toohave comma character in hello text (example: <Hello, world>), you can define " (double quote) as hello quote character and use hello string "Hello, world" in hello source.</span></span> |<span data-ttu-id="b9fa2-148">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-148">No</span></span> |
| <span data-ttu-id="b9fa2-149">nullValue</span><span class="sxs-lookup"><span data-stu-id="b9fa2-149">nullValue</span></span> |<span data-ttu-id="b9fa2-150">Ett eller flera tecken används toorepresent ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-150">One or more characters used toorepresent a null value.</span></span> |<span data-ttu-id="b9fa2-151">Ett eller flera tecken.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-151">One or more characters.</span></span> <span data-ttu-id="b9fa2-152">Hej **standard** värden är **”\N” och ”NULL”** vid läsning och **”\N”** vid skrivning.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-152">hello **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="b9fa2-153">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-153">No</span></span> |
| <span data-ttu-id="b9fa2-154">encodingName</span><span class="sxs-lookup"><span data-stu-id="b9fa2-154">encodingName</span></span> |<span data-ttu-id="b9fa2-155">Ange hello kodningsnamn.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-155">Specify hello encoding name.</span></span> |<span data-ttu-id="b9fa2-156">Ett giltigt kodningsnamn.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-156">A valid encoding name.</span></span> <span data-ttu-id="b9fa2-157">Se [Egenskapen Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-157">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="b9fa2-158">Exempel: windows-1250 or shift_jis.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-158">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="b9fa2-159">Hej **standard** värdet är **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-159">hello **default** value is **UTF-8**.</span></span> |<span data-ttu-id="b9fa2-160">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-160">No</span></span> |
| <span data-ttu-id="b9fa2-161">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="b9fa2-161">firstRowAsHeader</span></span> |<span data-ttu-id="b9fa2-162">Anger om tooconsider hello första raden som rubrik.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-162">Specifies whether tooconsider hello first row as a header.</span></span> <span data-ttu-id="b9fa2-163">För en indatauppsättning läser Data Factory den första raden som en rubrik.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-163">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="b9fa2-164">För en utdatauppsättning skriver Data Factory den första raden som en rubrik.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-164">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="b9fa2-165">Exempelscenarier finns i avsnittet med [användningsscenarier för `firstRowAsHeader` och `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-165">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="b9fa2-166">True</span><span class="sxs-lookup"><span data-stu-id="b9fa2-166">True</span></span><br/><span data-ttu-id="b9fa2-167"><b>False (standard)</b></span><span class="sxs-lookup"><span data-stu-id="b9fa2-167"><b>False (default)</b></span></span> |<span data-ttu-id="b9fa2-168">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-168">No</span></span> |
| <span data-ttu-id="b9fa2-169">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="b9fa2-169">skipLineCount</span></span> |<span data-ttu-id="b9fa2-170">Anger hello antal rader tooskip när data lästes från indatafiler.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-170">Indicates hello number of rows tooskip when reading data from input files.</span></span> <span data-ttu-id="b9fa2-171">Om både skipLineCount och firstRowAsHeader anges, hello rader hoppas över först och sedan hello rubrikinformation läses från hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-171">If both skipLineCount and firstRowAsHeader are specified, hello lines are skipped first and then hello header information is read from hello input file.</span></span> <br/><br/><span data-ttu-id="b9fa2-172">Exempelscenarier finns i avsnittet med [användningsscenarier för `firstRowAsHeader` och `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-172">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="b9fa2-173">Integer</span><span class="sxs-lookup"><span data-stu-id="b9fa2-173">Integer</span></span> |<span data-ttu-id="b9fa2-174">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-174">No</span></span> |
| <span data-ttu-id="b9fa2-175">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="b9fa2-175">treatEmptyAsNull</span></span> |<span data-ttu-id="b9fa2-176">Anger om tootreat null eller en tom sträng som en null-värde när data lästes från en indatafil.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-176">Specifies whether tootreat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="b9fa2-177">**True (standard)**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-177">**True (default)**</span></span><br/><span data-ttu-id="b9fa2-178">False</span><span class="sxs-lookup"><span data-stu-id="b9fa2-178">False</span></span> |<span data-ttu-id="b9fa2-179">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-179">No</span></span> |

### <a name="textformat-example"></a><span data-ttu-id="b9fa2-180">TextFormat-exempel</span><span class="sxs-lookup"><span data-stu-id="b9fa2-180">TextFormat example</span></span>
<span data-ttu-id="b9fa2-181">I följande JSON-definitionen för en dataset hello, har vissa hello valfria egenskaper angetts.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-181">In hello following JSON definition for a dataset, some of hello optional properties are specified.</span></span>

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

<span data-ttu-id="b9fa2-182">toouse en `escapeChar` i stället för `quoteChar`, Ersätt hello raden med `quoteChar` med hello följande escapeChar:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-182">toouse an `escapeChar` instead of `quoteChar`, replace hello line with `quoteChar` with hello following escapeChar:</span></span>

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="b9fa2-183">Användningsscenarier för firstRowAsHeader och skipLineCount</span><span class="sxs-lookup"><span data-stu-id="b9fa2-183">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="b9fa2-184">Du kopierar från en icke-källa tooa textfil och vill tooadd en huvudrad som innehåller hello schemat metadata (till exempel: SQL-schemat).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-184">You are copying from a non-file source tooa text file and would like tooadd a header line containing hello schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="b9fa2-185">Ange `firstRowAsHeader` som true i hello utdatauppsättningen för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-185">Specify `firstRowAsHeader` as true in hello output dataset for this scenario.</span></span>
* <span data-ttu-id="b9fa2-186">Du kopierar från en textfil som innehåller ett huvud rad tooa-fil som tar emot och vill toodrop rad.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-186">You are copying from a text file containing a header line tooa non-file sink and would like toodrop that line.</span></span> <span data-ttu-id="b9fa2-187">Ange `firstRowAsHeader` som true i hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-187">Specify `firstRowAsHeader` as true in hello input dataset.</span></span>
* <span data-ttu-id="b9fa2-188">Du kopierar från en textfil och vill tooskip några rader hello början som innehåller ingen information för data eller huvudet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-188">You are copying from a text file and want tooskip a few lines at hello beginning that contain no data or header information.</span></span> <span data-ttu-id="b9fa2-189">Ange `skipLineCount` tooindicate hello antalet rader toobe hoppas över.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-189">Specify `skipLineCount` tooindicate hello number of lines toobe skipped.</span></span> <span data-ttu-id="b9fa2-190">Om hello resten av hello-filen innehåller en rubrikrad, du kan också ange `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-190">If hello rest of hello file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="b9fa2-191">Om båda `skipLineCount` och `firstRowAsHeader` anges hello rader hoppas över först och sedan hello rubrikinformation läses från hello indatafilen</span><span class="sxs-lookup"><span data-stu-id="b9fa2-191">If both `skipLineCount` and `firstRowAsHeader` are specified, hello lines are skipped first and then hello header information is read from hello input file</span></span>

## <a name="json-format"></a><span data-ttu-id="b9fa2-192">JSON-format</span><span class="sxs-lookup"><span data-stu-id="b9fa2-192">JSON format</span></span>
<span data-ttu-id="b9fa2-193">för**importera och exportera en JSON-fil som-är i/från Azure Cosmos DB**, hello Se [Import/export JSON-dokument](data-factory-azure-documentdb-connector.md#importexport-json-documents) i avsnittet [flytta data till/från Azure Cosmos DB](data-factory-azure-documentdb-connector.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-193">too**import/export a JSON file as-is into/from Azure Cosmos DB**, hello see [Import/export JSON documents](data-factory-azure-documentdb-connector.md#importexport-json-documents) section in [Move data to/from Azure Cosmos DB](data-factory-azure-documentdb-connector.md) article.</span></span>

<span data-ttu-id="b9fa2-194">Om du vill tooparse hello JSON-filer eller skriva hello data i JSON-format, ange hello `type` egenskap i hello `format` avsnittet för**JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-194">If you want tooparse hello JSON files or write hello data in JSON format, set hello `type` property in hello `format` section too**JsonFormat**.</span></span> <span data-ttu-id="b9fa2-195">Du kan också ange hello följande **valfria** egenskaper i hello `format` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-195">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="b9fa2-196">Se [JsonFormat exempel](#jsonformat-example) avsnittet om hur tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-196">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="b9fa2-197">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b9fa2-197">Property</span></span> | <span data-ttu-id="b9fa2-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b9fa2-198">Description</span></span> | <span data-ttu-id="b9fa2-199">Krävs</span><span class="sxs-lookup"><span data-stu-id="b9fa2-199">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b9fa2-200">filePattern</span><span class="sxs-lookup"><span data-stu-id="b9fa2-200">filePattern</span></span> |<span data-ttu-id="b9fa2-201">Ange hello mönstret för data som lagras i varje JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-201">Indicate hello pattern of data stored in each JSON file.</span></span> <span data-ttu-id="b9fa2-202">Tillåtna värden är: **setOfObjects** och **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-202">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="b9fa2-203">Hej **standard** värdet är **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-203">hello **default** value is **setOfObjects**.</span></span> <span data-ttu-id="b9fa2-204">Detaljerad information om dessa mönster finns i avsnittet om [JSON-filmönster](#json-file-patterns).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-204">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="b9fa2-205">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-205">No</span></span> |
| <span data-ttu-id="b9fa2-206">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="b9fa2-206">jsonNodeReference</span></span> | <span data-ttu-id="b9fa2-207">Om du vill tooiterate och extrahera data från hello-objekt i en matris fältet med hello samma mönster, ange hello JSON-sökvägen i matrisen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-207">If you want tooiterate and extract data from hello objects inside an array field with hello same pattern, specify hello JSON path of that array.</span></span> <span data-ttu-id="b9fa2-208">Den här egenskapen stöds endast när du kopierar data från JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-208">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="b9fa2-209">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-209">No</span></span> |
| <span data-ttu-id="b9fa2-210">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="b9fa2-210">jsonPathDefinition</span></span> | <span data-ttu-id="b9fa2-211">Ange hello JSON sökvägsuttryck för varje kolumnmappning med ett anpassat kolumnnamn (start med gemener).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-211">Specify hello JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="b9fa2-212">Den här egenskapen stöds endast när du kopierar data från JSON-filer, och du kan extrahera data från objekt eller matriser.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-212">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="b9fa2-213">Börja med $ root; för fält under rotobjektet för fält i hello matris som valts av `jsonNodeReference` start från hello matriselement-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-213">For fields under root object, start with root $; for fields inside hello array chosen by `jsonNodeReference` property, start from hello array element.</span></span> <span data-ttu-id="b9fa2-214">Se [JsonFormat exempel](#jsonformat-example) avsnittet om hur tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-214">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span> | <span data-ttu-id="b9fa2-215">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-215">No</span></span> |
| <span data-ttu-id="b9fa2-216">encodingName</span><span class="sxs-lookup"><span data-stu-id="b9fa2-216">encodingName</span></span> |<span data-ttu-id="b9fa2-217">Ange hello kodningsnamn.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-217">Specify hello encoding name.</span></span> <span data-ttu-id="b9fa2-218">Hello lista över giltiga kodning namn, se: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-218">For hello list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="b9fa2-219">Exempel: windows-1250 or shift_jis.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-219">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="b9fa2-220">Hej **standard** värde är: **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-220">hello **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="b9fa2-221">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-221">No</span></span> |
| <span data-ttu-id="b9fa2-222">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="b9fa2-222">nestingSeparator</span></span> |<span data-ttu-id="b9fa2-223">Tecken som används tooseparate kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-223">Character that is used tooseparate nesting levels.</span></span> <span data-ttu-id="b9fa2-224">hello standardvärdet är '.' (punkt).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-224">hello default value is '.' (dot).</span></span> |<span data-ttu-id="b9fa2-225">Nej</span><span class="sxs-lookup"><span data-stu-id="b9fa2-225">No</span></span> |

### <a name="json-file-patterns"></a><span data-ttu-id="b9fa2-226">JSON-filmönster</span><span class="sxs-lookup"><span data-stu-id="b9fa2-226">JSON file patterns</span></span>

<span data-ttu-id="b9fa2-227">Kopieringsaktiviteten kan parsa hello efter mönster av JSON-filer:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-227">Copy activity can parse hello following patterns of JSON files:</span></span>

- <span data-ttu-id="b9fa2-228">**Typ I: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-228">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="b9fa2-229">Varje fil som innehåller ett enskilt objekt eller flera radavgränsade/sammanfogade objekt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-229">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="b9fa2-230">När det här alternativet väljs i en utdatauppsättning genererar kopieringsaktiviteten en enskild JSON-fil med ett objekt per rad (radavgränsade).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-230">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="b9fa2-231">**Exempel på JSON med enskilda objekt**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-231">**single object JSON example**</span></span>

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * <span data-ttu-id="b9fa2-232">**Exempel med radavgränsad JSON**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-232">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="b9fa2-233">**Exempel med sammanfogad JSON**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-233">**concatenated JSON example**</span></span>

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- <span data-ttu-id="b9fa2-234">**Typ II: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-234">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="b9fa2-235">Varje fil innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-235">Each file contains an array of objects.</span></span>

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a><span data-ttu-id="b9fa2-236">JsonFormat-exempel</span><span class="sxs-lookup"><span data-stu-id="b9fa2-236">JsonFormat example</span></span>

<span data-ttu-id="b9fa2-237">**Fall 1: Kopiera data från JSON-filer**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-237">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="b9fa2-238">Se hello följande två samplingarna när du kopierar data från JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-238">See hello following two samples when copying data from JSON files.</span></span> <span data-ttu-id="b9fa2-239">hello generiska pekar toonote:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-239">hello generic points toonote:</span></span>

<span data-ttu-id="b9fa2-240">**Exempel 1: hämta data från objektet och matrisen**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-240">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="b9fa2-241">I det här exemplet du förväntar dig en JSON-rotobjektet mappar toosingle post i tabellform resultat.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-241">In this sample, you expect one root JSON object maps toosingle record in tabular result.</span></span> <span data-ttu-id="b9fa2-242">Om du har en JSON-fil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-242">If you have a JSON file with hello following content:</span></span>  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagmentProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
<span data-ttu-id="b9fa2-243">och du vill toocopy den till en Azure SQL-tabellen i hello följande format genom att extrahera data från både objekt och matrisen:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-243">and you want toocopy it into an Azure SQL table in hello following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="b9fa2-244">id</span><span class="sxs-lookup"><span data-stu-id="b9fa2-244">id</span></span> | <span data-ttu-id="b9fa2-245">deviceType</span><span class="sxs-lookup"><span data-stu-id="b9fa2-245">deviceType</span></span> | <span data-ttu-id="b9fa2-246">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="b9fa2-246">targetResourceType</span></span> | <span data-ttu-id="b9fa2-247">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="b9fa2-247">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="b9fa2-248">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="b9fa2-248">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="b9fa2-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="b9fa2-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="b9fa2-250">PC</span><span class="sxs-lookup"><span data-stu-id="b9fa2-250">PC</span></span> | <span data-ttu-id="b9fa2-251">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="b9fa2-251">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="b9fa2-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="b9fa2-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="b9fa2-253">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="b9fa2-253">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="b9fa2-254">hello inkommande datamängd med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-254">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="b9fa2-255">Mer specifikt:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-255">More specifically:</span></span>

- <span data-ttu-id="b9fa2-256">`structure`avsnittet definierar hello anpassat kolumnnamn och hello motsvarande datatyp vid konvertering av tootabular data.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-256">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="b9fa2-257">Det här avsnittet är **valfria** om du inte behöver toodo kolumnmappningen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-257">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="b9fa2-258">Se [mappa dataset kolumner toodestination dataset källkolumner](data-factory-map-columns.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-258">See [Map source dataset columns toodestination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="b9fa2-259">`jsonPathDefinition`Anger hello JSON-sökvägen för varje kolumn som anger om tooextract hello data från.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-259">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="b9fa2-260">toocopy data från en matris, som du kan använda **matris [x] .property** tooextract värdet för hello anges egenskapen från hello x objekt eller du kan använda  **matris [*] .property** toofind hello-värde från vilket objekt som innehåller sådan egenskap.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-260">toocopy data from array, you can use **array[x].property** tooextract value of hello given property from hello xth object, or you can use **array[*].property** toofind hello value from any object containing such property.</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagmentProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagmentProcessRunId": "$.context.custom.dimensions[1].ResourceManagmentProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

<span data-ttu-id="b9fa2-261">**Exempel 2: mellan tillämpa flera objekt med samma mönster från en matris hello**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-261">**Sample 2: cross apply multiple objects with hello same pattern from array**</span></span>

<span data-ttu-id="b9fa2-262">I det här exemplet du förväntar dig tootransform en JSON-objekt i roten till flera poster i tabellform resultat.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-262">In this sample, you expect tootransform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="b9fa2-263">Om du har en JSON-fil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-263">If you have a JSON file with hello following content:</span></span>  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
<span data-ttu-id="b9fa2-264">och du vill toocopy den till en Azure SQL-tabellen i hello följande format genom att förenkla hello data i hello matris och korskoppling med hello vanliga rot-information:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-264">and you want toocopy it into an Azure SQL table in hello following format, by flattening hello data inside hello array and cross join with hello common root info:</span></span>

| <span data-ttu-id="b9fa2-265">ordernumber</span><span class="sxs-lookup"><span data-stu-id="b9fa2-265">ordernumber</span></span> | <span data-ttu-id="b9fa2-266">orderdate</span><span class="sxs-lookup"><span data-stu-id="b9fa2-266">orderdate</span></span> | <span data-ttu-id="b9fa2-267">order_pd</span><span class="sxs-lookup"><span data-stu-id="b9fa2-267">order_pd</span></span> | <span data-ttu-id="b9fa2-268">order_price</span><span class="sxs-lookup"><span data-stu-id="b9fa2-268">order_price</span></span> | <span data-ttu-id="b9fa2-269">city</span><span class="sxs-lookup"><span data-stu-id="b9fa2-269">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="b9fa2-270">01</span><span class="sxs-lookup"><span data-stu-id="b9fa2-270">01</span></span> | <span data-ttu-id="b9fa2-271">20170122</span><span class="sxs-lookup"><span data-stu-id="b9fa2-271">20170122</span></span> | <span data-ttu-id="b9fa2-272">P1</span><span class="sxs-lookup"><span data-stu-id="b9fa2-272">P1</span></span> | <span data-ttu-id="b9fa2-273">23</span><span class="sxs-lookup"><span data-stu-id="b9fa2-273">23</span></span> | <span data-ttu-id="b9fa2-274">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="b9fa2-274">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="b9fa2-275">01</span><span class="sxs-lookup"><span data-stu-id="b9fa2-275">01</span></span> | <span data-ttu-id="b9fa2-276">20170122</span><span class="sxs-lookup"><span data-stu-id="b9fa2-276">20170122</span></span> | <span data-ttu-id="b9fa2-277">P2</span><span class="sxs-lookup"><span data-stu-id="b9fa2-277">P2</span></span> | <span data-ttu-id="b9fa2-278">13</span><span class="sxs-lookup"><span data-stu-id="b9fa2-278">13</span></span> | <span data-ttu-id="b9fa2-279">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="b9fa2-279">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="b9fa2-280">01</span><span class="sxs-lookup"><span data-stu-id="b9fa2-280">01</span></span> | <span data-ttu-id="b9fa2-281">20170122</span><span class="sxs-lookup"><span data-stu-id="b9fa2-281">20170122</span></span> | <span data-ttu-id="b9fa2-282">P3</span><span class="sxs-lookup"><span data-stu-id="b9fa2-282">P3</span></span> | <span data-ttu-id="b9fa2-283">231</span><span class="sxs-lookup"><span data-stu-id="b9fa2-283">231</span></span> | <span data-ttu-id="b9fa2-284">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="b9fa2-284">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="b9fa2-285">hello inkommande datamängd med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-285">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="b9fa2-286">Mer specifikt:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-286">More specifically:</span></span>

- <span data-ttu-id="b9fa2-287">`structure`avsnittet definierar hello anpassat kolumnnamn och hello motsvarande datatyp vid konvertering av tootabular data.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-287">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="b9fa2-288">Det här avsnittet är **valfria** om du inte behöver toodo kolumnmappningen.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-288">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="b9fa2-289">Se [mappa dataset kolumner toodestination dataset källkolumner](data-factory-map-columns.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-289">See [Map source dataset columns toodestination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="b9fa2-290">`jsonNodeReference`Anger tooiterate och extrahera data från hello-objekt med samma mönster under hello **matris** orderlines.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-290">`jsonNodeReference` indicates tooiterate and extract data from hello objects with hello same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="b9fa2-291">`jsonPathDefinition`Anger hello JSON-sökvägen för varje kolumn som anger om tooextract hello data från.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-291">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="b9fa2-292">I det här exemplet är ”ordernumber”, ”orderdate” och ”ort” under rotobjektet med JSON-sökvägen som börjar med ”$.”, medan ”order_pd” och ”order_price” definieras med sökvägen som härletts från hello matriselement utan ”$”.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-292">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from hello array element without "$.".</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

<span data-ttu-id="b9fa2-293">**Observera följande punkter hello:**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-293">**Note hello following points:**</span></span>

* <span data-ttu-id="b9fa2-294">Om hello `structure` och `jsonPathDefinition` har inte definierats i hello Data Factory dataset hello Kopieringsaktiviteten identifierar hello schema från första hello-objektet och förenkla hello hela objektet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-294">If hello `structure` and `jsonPathDefinition` are not defined in hello Data Factory dataset, hello Copy Activity detects hello schema from hello first object and flatten hello whole object.</span></span>
* <span data-ttu-id="b9fa2-295">Om hello JSON-indata har en matris, som standard konverterar hello Kopieringsaktiviteten hello hela Matrisvärde till en sträng.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-295">If hello JSON input has an array, by default hello Copy Activity converts hello entire array value into a string.</span></span> <span data-ttu-id="b9fa2-296">Du kan välja tooextract data från den med hjälp av `jsonNodeReference` och/eller `jsonPathDefinition`, eller hoppa över det genom att inte ange det i `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-296">You can choose tooextract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="b9fa2-297">Om det finns duplicerade namn på Hej samma nivå, hello Kopieringsaktiviteten hämtar hello senast.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-297">If there are duplicate names at hello same level, hello Copy Activity picks hello last one.</span></span>
* <span data-ttu-id="b9fa2-298">Egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-298">Property names are case-sensitive.</span></span> <span data-ttu-id="b9fa2-299">Två egenskaper med samma namn men med olika skiftlägen behandlas som två olika egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-299">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="b9fa2-300">**Fall 2: Skriver data tooJSON-fil**</span><span class="sxs-lookup"><span data-stu-id="b9fa2-300">**Case 2: Writing data tooJSON file**</span></span>

<span data-ttu-id="b9fa2-301">Om du har hello följande tabell i SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-301">If you have hello following table in SQL Database:</span></span>

| <span data-ttu-id="b9fa2-302">id</span><span class="sxs-lookup"><span data-stu-id="b9fa2-302">id</span></span> | <span data-ttu-id="b9fa2-303">order_date</span><span class="sxs-lookup"><span data-stu-id="b9fa2-303">order_date</span></span> | <span data-ttu-id="b9fa2-304">order_price</span><span class="sxs-lookup"><span data-stu-id="b9fa2-304">order_price</span></span> | <span data-ttu-id="b9fa2-305">order_by</span><span class="sxs-lookup"><span data-stu-id="b9fa2-305">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b9fa2-306">1</span><span class="sxs-lookup"><span data-stu-id="b9fa2-306">1</span></span> | <span data-ttu-id="b9fa2-307">20170119</span><span class="sxs-lookup"><span data-stu-id="b9fa2-307">20170119</span></span> | <span data-ttu-id="b9fa2-308">2000</span><span class="sxs-lookup"><span data-stu-id="b9fa2-308">2000</span></span> | <span data-ttu-id="b9fa2-309">David</span><span class="sxs-lookup"><span data-stu-id="b9fa2-309">David</span></span> |
| <span data-ttu-id="b9fa2-310">2</span><span class="sxs-lookup"><span data-stu-id="b9fa2-310">2</span></span> | <span data-ttu-id="b9fa2-311">20170120</span><span class="sxs-lookup"><span data-stu-id="b9fa2-311">20170120</span></span> | <span data-ttu-id="b9fa2-312">3500</span><span class="sxs-lookup"><span data-stu-id="b9fa2-312">3500</span></span> | <span data-ttu-id="b9fa2-313">Patrick</span><span class="sxs-lookup"><span data-stu-id="b9fa2-313">Patrick</span></span> |
| <span data-ttu-id="b9fa2-314">3</span><span class="sxs-lookup"><span data-stu-id="b9fa2-314">3</span></span> | <span data-ttu-id="b9fa2-315">20170121</span><span class="sxs-lookup"><span data-stu-id="b9fa2-315">20170121</span></span> | <span data-ttu-id="b9fa2-316">4000</span><span class="sxs-lookup"><span data-stu-id="b9fa2-316">4000</span></span> | <span data-ttu-id="b9fa2-317">Jason</span><span class="sxs-lookup"><span data-stu-id="b9fa2-317">Jason</span></span> |

<span data-ttu-id="b9fa2-318">och för varje post du förväntar dig toowrite tooa JSON-objekt i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-318">and for each record, you expect toowrite tooa JSON object in hello following format:</span></span>
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

<span data-ttu-id="b9fa2-319">Hej utdatauppsättningen med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-319">hello output dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="b9fa2-320">Mer specifikt `structure` avsnittet definierar hello anpassade egenskapsnamn i målfilen och `nestingSeparator` (standardvärdet är ””.) är används tooidentify hello kapsla lagret från hello namn.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-320">More specifically, `structure` section defines hello customized property names in destination file, `nestingSeparator` (default is ".") are used tooidentify hello nest layer from hello name.</span></span> <span data-ttu-id="b9fa2-321">Det här avsnittet är **valfria** om du vill jämföra med källkolumnsnamnet toochange hello egenskapens namn eller kapsla några av hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-321">This section is **optional** unless you want toochange hello property name comparing with source column name, or nest some of hello properties.</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="avro-format"></a><span data-ttu-id="b9fa2-322">AVRO-formatet</span><span class="sxs-lookup"><span data-stu-id="b9fa2-322">AVRO format</span></span>
<span data-ttu-id="b9fa2-323">Om du vill tooparse hello Avro-filernas eller skriva hello data i Avro-formatet, ange hello `format` `type` egenskapen för**AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-323">If you want tooparse hello Avro files or write hello data in Avro format, set hello `format` `type` property too**AvroFormat**.</span></span> <span data-ttu-id="b9fa2-324">Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-324">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="b9fa2-325">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-325">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="b9fa2-326">toouse Avro-formatet i en Hive-tabell, kan du läsa för[Apache Hive kursen](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-326">toouse Avro format in a Hive table, you can refer too[Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="b9fa2-327">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-327">Note hello following points:</span></span>  

* <span data-ttu-id="b9fa2-328">[Komplexa datatyper](http://avro.apache.org/docs/current/spec.html#schema_complex) stöds inte (registrerar uppräkningar, matriser, kartor, unioner, och fast).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-328">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions, and fixed).</span></span>

## <a name="orc-format"></a><span data-ttu-id="b9fa2-329">ORC-format</span><span class="sxs-lookup"><span data-stu-id="b9fa2-329">ORC format</span></span>
<span data-ttu-id="b9fa2-330">Om du vill tooparse hello ORC-filer eller skriva hello data i ORC-format, ange hello `format` `type` egenskapen för**OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-330">If you want tooparse hello ORC files or write hello data in ORC format, set hello `format` `type` property too**OrcFormat**.</span></span> <span data-ttu-id="b9fa2-331">Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-331">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="b9fa2-332">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-332">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="b9fa2-333">Om du inte kopierar ORC-filer **som-är** mellan lokala och moln datalager, du behöver tooinstall hello 8 JRE (Java Runtime Environment) på din gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-333">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="b9fa2-334">En 64-bitars gateway kräver 64-bitars JRE och en 32-bitars gateway kräver 32-bitars JRE.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-334">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="b9fa2-335">Du hittar båda versionerna [här](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-335">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="b9fa2-336">Välja hello lämplig.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-336">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="b9fa2-337">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-337">Note hello following points:</span></span>

* <span data-ttu-id="b9fa2-338">Komplexa datatyper stöds inte (STRUCT, MAP, LIST, UNION)</span><span class="sxs-lookup"><span data-stu-id="b9fa2-338">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="b9fa2-339">ORC-filen har tre [komprimeringsrelaterade alternativ](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB och SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-339">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="b9fa2-340">Data Factory stöder läsning av data från ORC-filer i alla dessa komprimerade format.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-340">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="b9fa2-341">Den använder hello komprimering codec finns i hello metadata tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-341">It uses hello compression codec is in hello metadata tooread hello data.</span></span> <span data-ttu-id="b9fa2-342">Men när du skriver tooan ORC filen väljer Data Factory ZLIB, som är hello standard för ORC.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-342">However, when writing tooan ORC file, Data Factory chooses ZLIB, which is hello default for ORC.</span></span> <span data-ttu-id="b9fa2-343">För närvarande finns ingen alternativet toooverride detta beteende.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-343">Currently, there is no option toooverride this behavior.</span></span>

## <a name="parquet-format"></a><span data-ttu-id="b9fa2-344">Parkettgolv format</span><span class="sxs-lookup"><span data-stu-id="b9fa2-344">Parquet format</span></span>
<span data-ttu-id="b9fa2-345">Om du vill tooparse hello parkettgolv filer eller skriva hello data i parkettgolv format, ange hello `format` `type` egenskapen för**ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-345">If you want tooparse hello Parquet files or write hello data in Parquet format, set hello `format` `type` property too**ParquetFormat**.</span></span> <span data-ttu-id="b9fa2-346">Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-346">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="b9fa2-347">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-347">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="b9fa2-348">Om du inte kopierar filer parkettgolv **som-är** mellan lokala och moln datalager, du behöver tooinstall hello 8 JRE (Java Runtime Environment) på din gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-348">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="b9fa2-349">En 64-bitars gateway kräver 64-bitars JRE och en 32-bitars gateway kräver 32-bitars JRE.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-349">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="b9fa2-350">Du hittar båda versionerna [här](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="b9fa2-350">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="b9fa2-351">Välja hello lämplig.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-351">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="b9fa2-352">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-352">Note hello following points:</span></span>

* <span data-ttu-id="b9fa2-353">Komplexa datatyper stöds inte (MAP, LIST)</span><span class="sxs-lookup"><span data-stu-id="b9fa2-353">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="b9fa2-354">Parkettgolv filen har följande alternativ för komprimering-relaterade hello: NONE, SNAPPY GZIP och LZO.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-354">Parquet file has hello following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="b9fa2-355">Data Factory stöder läsning av data från ORC-filer i alla dessa komprimerade format.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-355">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="b9fa2-356">Hello komprimerings-codec används i hello metadata tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-356">It uses hello compression codec in hello metadata tooread hello data.</span></span> <span data-ttu-id="b9fa2-357">Men när du skriver tooa parkettgolv filen väljer Data Factory SNAPPY, vilket är standard hello parkettgolv format.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-357">However, when writing tooa Parquet file, Data Factory chooses SNAPPY, which is hello default for Parquet format.</span></span> <span data-ttu-id="b9fa2-358">För närvarande finns ingen alternativet toooverride detta beteende.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-358">Currently, there is no option toooverride this behavior.</span></span>

## <a name="compression-support"></a><span data-ttu-id="b9fa2-359">Komprimeringsstöd för</span><span class="sxs-lookup"><span data-stu-id="b9fa2-359">Compression support</span></span>
<span data-ttu-id="b9fa2-360">Bearbetning av stora datamängder kan det orsaka flaskhalsar i i/o och nätverk.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-360">Processing large data sets can cause I/O and network bottlenecks.</span></span> <span data-ttu-id="b9fa2-361">Därför kan komprimerade data i appbutiker inte bara snabbare dataöverföring hello nätverket och sparar diskutrymme, men också sätta betydande prestandaförbättringar vid bearbetning av stordata.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-361">Therefore, compressed data in stores can not only speed up data transfer across hello network and save disk space, but also bring significant performance improvements in processing big data.</span></span> <span data-ttu-id="b9fa2-362">Komprimering stöds för närvarande för filbaserade datalager, till exempel Azure Blob- eller lokala filsystem.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-362">Currently, compression is supported for file-based data stores such as Azure Blob or On-premises File System.</span></span>  

<span data-ttu-id="b9fa2-363">toospecify komprimering för en dataset används hello **komprimering** egenskap i hello dataset JSON som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-363">toospecify compression for a dataset, use hello **compression** property in hello dataset JSON as in hello following example:</span></span>   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```

<span data-ttu-id="b9fa2-364">Anta att hello exempeldatamängd används som hello utdata från en kopieringsaktiviteten utdata med GZIP codec med optimala hello kopiera aktivitet komprimerar hello och sedan skriva hello komprimerade data i en fil med namnet pagecounts.csv.gz i hello Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-364">Suppose hello sample dataset is used as hello output of a copy activity, hello copy activity compresses hello output data with GZIP codec using optimal ratio and then write hello compressed data into a file named pagecounts.csv.gz in hello Azure Blob Storage.</span></span>

> [!NOTE]
> <span data-ttu-id="b9fa2-365">Inställningarna för komprimering stöds inte för data i hello **AvroFormat**, **OrcFormat**, eller **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-365">Compression settings are not supported for data in hello **AvroFormat**, **OrcFormat**, or **ParquetFormat**.</span></span> <span data-ttu-id="b9fa2-366">Vid läsning av filer i dessa format, Data Factory identifierar och använder hello komprimerings-codec i hello metadata.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-366">When reading files in these formats, Data Factory detects and uses hello compression codec in hello metadata.</span></span> <span data-ttu-id="b9fa2-367">När du skriver toofiles i dessa format väljer Data Factory hello standard komprimerings-codec för detta format.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-367">When writing toofiles in these formats, Data Factory chooses hello default compression codec for that format.</span></span> <span data-ttu-id="b9fa2-368">Till exempel ZLIB för OrcFormat och SNAPPY för ParquetFormat.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-368">For example, ZLIB for OrcFormat and SNAPPY for ParquetFormat.</span></span>   

<span data-ttu-id="b9fa2-369">Hej **komprimering** avsnitt har två egenskaper:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-369">hello **compression** section has two properties:</span></span>  

* <span data-ttu-id="b9fa2-370">**Typ:** hello komprimerings-codec, vilket kan vara **GZIP**, **Deflate**, **BZIP2**, eller **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-370">**Type:** hello compression codec, which can be **GZIP**, **Deflate**, **BZIP2**, or **ZipDeflate**.</span></span>  
* <span data-ttu-id="b9fa2-371">**Nivå:** hello komprimeringsförhållandet, vilket kan vara **Optimal** eller **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-371">**Level:** hello compression ratio, which can be **Optimal** or **Fastest**.</span></span>

  * <span data-ttu-id="b9fa2-372">**Snabbaste:** hello komprimeringsåtgärden ska slutföras så snabbt som möjligt, även om hello resulterande filen inte komprimeras optimalt.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-372">**Fastest:** hello compression operation should complete as quickly as possible, even if hello resulting file is not optimally compressed.</span></span>
  * <span data-ttu-id="b9fa2-373">**Optimal**: hello komprimeringsåtgärden ska optimalt komprimeras, även om hello åtgärden tar en längre tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-373">**Optimal**: hello compression operation should be optimally compressed, even if hello operation takes a longer time toocomplete.</span></span>

    <span data-ttu-id="b9fa2-374">Mer information finns i [komprimeringsnivå](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-374">For more information, see [Compression Level](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) topic.</span></span>

<span data-ttu-id="b9fa2-375">När du anger `compression` egenskap i en inkommande datauppsättning JSON hello-pipeline kan läsa komprimerade data från hello källa, och när du anger hello-egenskapen i en datamängd för utdata JSON hello Kopieringsaktivitet kan skriva komprimerade data toohello mål.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-375">When you specify `compression` property in an input dataset JSON, hello pipeline can read compressed data from hello source; and when you specify hello property in an output dataset JSON, hello copy activity can write compressed data toohello destination.</span></span> <span data-ttu-id="b9fa2-376">Här följer några exempelscenarier:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-376">Here are a few sample scenarios:</span></span>

* <span data-ttu-id="b9fa2-377">Läsa GZIP komprimerade data från en Azure blob, expandera den och skriva resultatet data tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-377">Read GZIP compressed data from an Azure blob, decompress it, and write result data tooan Azure SQL database.</span></span> <span data-ttu-id="b9fa2-378">Du definierar hello inkommande Azure-blobbdatauppsättning med hello `compression` `type` JSON-egenskap som GZIP.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-378">You define hello input Azure Blob dataset with hello `compression` `type` JSON property as GZIP.</span></span>
* <span data-ttu-id="b9fa2-379">Läsa data från en fil med oformaterad text från lokalt filsystem, komprimera formatet GZip och skriva hello komprimerade data tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-379">Read data from a plain-text file from on-premises File System, compress it using GZip format, and write hello compressed data tooan Azure blob.</span></span> <span data-ttu-id="b9fa2-380">Du definierar ett Azure Blob-datamängd för utdata med hello `compression` `type` JSON-egenskap som GZip.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-380">You define an output Azure Blob dataset with hello `compression` `type` JSON property as GZip.</span></span>
* <span data-ttu-id="b9fa2-381">Läs ZIP-filen från FTP-server, expandera den tooget hello filer i och mark filerna i Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-381">Read .zip file from FTP server, decompress it tooget hello files inside, and land those files into Azure Data Lake Store.</span></span> <span data-ttu-id="b9fa2-382">Du definierar en inkommande FTP-datamängd med hello `compression` `type` JSON-egenskap som ZipDeflate.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-382">You define an input FTP dataset with hello `compression` `type` JSON property as ZipDeflate.</span></span>
* <span data-ttu-id="b9fa2-383">Läsa in en GZIP-komprimerade data från en Azure blob, expandera den, komprimera den med hjälp av BZIP2 och skriva resultatet data tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-383">Read a GZIP-compressed data from an Azure blob, decompress it, compress it using BZIP2, and write result data tooan Azure blob.</span></span> <span data-ttu-id="b9fa2-384">Du definierar hello inkommande Azure-blobbdatauppsättning med `compression` `type` ange tooGZIP och hello utdatauppsättningen med `compression` `type` ange tooBZIP2 i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="b9fa2-384">You define hello input Azure Blob dataset with `compression` `type` set tooGZIP and hello output dataset with `compression` `type` set tooBZIP2 in this case.</span></span>   


## <a name="next-steps"></a><span data-ttu-id="b9fa2-385">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9fa2-385">Next steps</span></span>
<span data-ttu-id="b9fa2-386">Se följande artiklar för filbaserade datakällor som stöds av Azure Data Factory hello:</span><span class="sxs-lookup"><span data-stu-id="b9fa2-386">See hello following articles for file-based data stores supported by Azure Data Factory:</span></span>

- [<span data-ttu-id="b9fa2-387">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b9fa2-387">Azure Blob Storage</span></span>](data-factory-azure-blob-connector.md)
- [<span data-ttu-id="b9fa2-388">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="b9fa2-388">Azure Data Lake Store</span></span>](data-factory-azure-datalake-connector.md)
- [<span data-ttu-id="b9fa2-389">FTP</span><span class="sxs-lookup"><span data-stu-id="b9fa2-389">FTP</span></span>](data-factory-ftp-connector.md)
- [<span data-ttu-id="b9fa2-390">HDFS</span><span class="sxs-lookup"><span data-stu-id="b9fa2-390">HDFS</span></span>](data-factory-hdfs-connector.md)
- [<span data-ttu-id="b9fa2-391">Filsystem</span><span class="sxs-lookup"><span data-stu-id="b9fa2-391">File System</span></span>](data-factory-onprem-file-system-connector.md)
- [<span data-ttu-id="b9fa2-392">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="b9fa2-392">Amazon S3</span></span>](data-factory-amazon-simple-storage-service-connector.md)
