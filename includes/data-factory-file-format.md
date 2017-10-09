## <a name="specifying-formats"></a><span data-ttu-id="7af05-101">Ange format</span><span class="sxs-lookup"><span data-stu-id="7af05-101">Specifying formats</span></span>
<span data-ttu-id="7af05-102">Azure Data Factory stöder hello följande formattyper:</span><span class="sxs-lookup"><span data-stu-id="7af05-102">Azure Data Factory supports hello following format types:</span></span>

* [<span data-ttu-id="7af05-103">Textformat</span><span class="sxs-lookup"><span data-stu-id="7af05-103">Text Format</span></span>](#specifying-textformat)
* [<span data-ttu-id="7af05-104">JSON-Format</span><span class="sxs-lookup"><span data-stu-id="7af05-104">JSON Format</span></span>](#specifying-jsonformat)
* [<span data-ttu-id="7af05-105">Avro-format</span><span class="sxs-lookup"><span data-stu-id="7af05-105">Avro Format</span></span>](#specifying-avroformat)
* [<span data-ttu-id="7af05-106">ORC-format</span><span class="sxs-lookup"><span data-stu-id="7af05-106">ORC Format</span></span>](#specifying-orcformat)
* [<span data-ttu-id="7af05-107">Parquet-format</span><span class="sxs-lookup"><span data-stu-id="7af05-107">Parquet Format</span></span>](#specifying-parquetformat)

### <a name="specifying-textformat"></a><span data-ttu-id="7af05-108">Ange TextFormat</span><span class="sxs-lookup"><span data-stu-id="7af05-108">Specifying TextFormat</span></span>
<span data-ttu-id="7af05-109">Om du vill tooparse hello textfiler eller skriva hello data i textformat, ange hello `format` `type` egenskapen för**TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="7af05-109">If you want tooparse hello text files or write hello data in text format, set hello `format` `type` property too**TextFormat**.</span></span> <span data-ttu-id="7af05-110">Du kan också ange hello följande **valfria** egenskaper i hello `format` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7af05-110">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="7af05-111">Se [TextFormat exempel](#textformat-example) avsnittet om hur tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="7af05-111">See [TextFormat example](#textformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="7af05-112">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7af05-112">Property</span></span> | <span data-ttu-id="7af05-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7af05-113">Description</span></span> | <span data-ttu-id="7af05-114">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7af05-114">Allowed values</span></span> | <span data-ttu-id="7af05-115">Krävs</span><span class="sxs-lookup"><span data-stu-id="7af05-115">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7af05-116">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="7af05-116">columnDelimiter</span></span> |<span data-ttu-id="7af05-117">hello tecknet används tooseparate kolumner i en fil.</span><span class="sxs-lookup"><span data-stu-id="7af05-117">hello character used tooseparate columns in a file.</span></span> <span data-ttu-id="7af05-118">Du kan överväga att toouse en sällsynt icke utskrivbara tecken som inte är troligt finns i dina data: till exempel ange ”\u0001” som representerar Start av rubriken (SOH).</span><span class="sxs-lookup"><span data-stu-id="7af05-118">You can consider toouse a rare unprintable char which not likely exists in your data: e.g. specify "\u0001" which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="7af05-119">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="7af05-119">Only one character is allowed.</span></span> <span data-ttu-id="7af05-120">Hej **standard** värdet är **kommatecken (',')**.</span><span class="sxs-lookup"><span data-stu-id="7af05-120">hello **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="7af05-121">toouse ett Unicode-tecken finns för[Unicode-tecken](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello motsvarande kod för den.</span><span class="sxs-lookup"><span data-stu-id="7af05-121">toouse an Unicode character, refer too[Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello corresponding code for it.</span></span> |<span data-ttu-id="7af05-122">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-122">No</span></span> |
| <span data-ttu-id="7af05-123">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="7af05-123">rowDelimiter</span></span> |<span data-ttu-id="7af05-124">hello tecknet används tooseparate rader i en fil.</span><span class="sxs-lookup"><span data-stu-id="7af05-124">hello character used tooseparate rows in a file.</span></span> |<span data-ttu-id="7af05-125">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="7af05-125">Only one character is allowed.</span></span> <span data-ttu-id="7af05-126">Hej **standard** värdet är något av följande värden på Läs hello: **[”\r\n”, ”\r”, ”\n”]** och **”\r\n”** vid skrivning.</span><span class="sxs-lookup"><span data-stu-id="7af05-126">hello **default** value is any of hello following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="7af05-127">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-127">No</span></span> |
| <span data-ttu-id="7af05-128">escapeChar</span><span class="sxs-lookup"><span data-stu-id="7af05-128">escapeChar</span></span> |<span data-ttu-id="7af05-129">hello specialtecken används tooescape avgränsare för en kolumn i hello innehållet i indatafilen.</span><span class="sxs-lookup"><span data-stu-id="7af05-129">hello special character used tooescape a column delimiter in hello content of input file.</span></span> <br/><br/><span data-ttu-id="7af05-130">Du kan inte ange både escapeChar och quoteChar för en tabell.</span><span class="sxs-lookup"><span data-stu-id="7af05-130">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="7af05-131">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="7af05-131">Only one character is allowed.</span></span> <span data-ttu-id="7af05-132">Inget standardvärde.</span><span class="sxs-lookup"><span data-stu-id="7af05-132">No default value.</span></span> <br/><br/><span data-ttu-id="7af05-133">Exempel: Om du har kommatecken (', ') som hello kolumnen avgränsare, men du vill att toohave hello kommatecken tecknet i texten hello (exempel: ”Hello, world”), kan du definiera '$' som hello escape-tecken och använda strängen ”$Hello, world” i hello källan.</span><span class="sxs-lookup"><span data-stu-id="7af05-133">Example: if you have comma (',') as hello column delimiter but you want toohave hello comma character in hello text (example: "Hello, world"), you can define ‘$’ as hello escape character and use string "Hello$, world" in hello source.</span></span> |<span data-ttu-id="7af05-134">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-134">No</span></span> |
| <span data-ttu-id="7af05-135">quoteChar</span><span class="sxs-lookup"><span data-stu-id="7af05-135">quoteChar</span></span> |<span data-ttu-id="7af05-136">hello tecknet används tooquote ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="7af05-136">hello character used tooquote a string value.</span></span> <span data-ttu-id="7af05-137">hello kolumn- och avgränsare i hello citattecken skulle behandlas som en del av hello strängvärde.</span><span class="sxs-lookup"><span data-stu-id="7af05-137">hello column and row delimiters inside hello quote characters would be treated as part of hello string value.</span></span> <span data-ttu-id="7af05-138">Den här egenskapen är tillämpliga tooboth indata och utdata datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="7af05-138">This property is applicable tooboth input and output datasets.</span></span><br/><br/><span data-ttu-id="7af05-139">Du kan inte ange både escapeChar och quoteChar för en tabell.</span><span class="sxs-lookup"><span data-stu-id="7af05-139">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="7af05-140">Endast ett tecken är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="7af05-140">Only one character is allowed.</span></span> <span data-ttu-id="7af05-141">Inget standardvärde.</span><span class="sxs-lookup"><span data-stu-id="7af05-141">No default value.</span></span> <br/><br/><span data-ttu-id="7af05-142">Till exempel om du har kommatecken (', ') hello kolumnen avgränsare, men du vill toohave kommatecken tecknet i texten hello (exempel: < Hello, world >), kan du definiera ”(dubbla citattecken) som hello citattecken och Använd hello sträng” Hello, world ”i hello källan.</span><span class="sxs-lookup"><span data-stu-id="7af05-142">For example, if you have comma (',') as hello column delimiter but you want toohave comma character in hello text (example: <Hello, world>), you can define " (double quote) as hello quote character and use hello string "Hello, world" in hello source.</span></span> |<span data-ttu-id="7af05-143">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-143">No</span></span> |
| <span data-ttu-id="7af05-144">nullValue</span><span class="sxs-lookup"><span data-stu-id="7af05-144">nullValue</span></span> |<span data-ttu-id="7af05-145">Ett eller flera tecken används toorepresent ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="7af05-145">One or more characters used toorepresent a null value.</span></span> |<span data-ttu-id="7af05-146">Ett eller flera tecken.</span><span class="sxs-lookup"><span data-stu-id="7af05-146">One or more characters.</span></span> <span data-ttu-id="7af05-147">Hej **standard** värden är **”\N” och ”NULL”** vid läsning och **”\N”** vid skrivning.</span><span class="sxs-lookup"><span data-stu-id="7af05-147">hello **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="7af05-148">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-148">No</span></span> |
| <span data-ttu-id="7af05-149">encodingName</span><span class="sxs-lookup"><span data-stu-id="7af05-149">encodingName</span></span> |<span data-ttu-id="7af05-150">Ange hello kodningsnamn.</span><span class="sxs-lookup"><span data-stu-id="7af05-150">Specify hello encoding name.</span></span> |<span data-ttu-id="7af05-151">Ett giltigt kodningsnamn.</span><span class="sxs-lookup"><span data-stu-id="7af05-151">A valid encoding name.</span></span> <span data-ttu-id="7af05-152">Se [Egenskapen Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="7af05-152">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="7af05-153">Exempel: windows-1250 or shift_jis.</span><span class="sxs-lookup"><span data-stu-id="7af05-153">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="7af05-154">Hej **standard** värdet är **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="7af05-154">hello **default** value is **UTF-8**.</span></span> |<span data-ttu-id="7af05-155">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-155">No</span></span> |
| <span data-ttu-id="7af05-156">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="7af05-156">firstRowAsHeader</span></span> |<span data-ttu-id="7af05-157">Anger om tooconsider hello första raden som rubrik.</span><span class="sxs-lookup"><span data-stu-id="7af05-157">Specifies whether tooconsider hello first row as a header.</span></span> <span data-ttu-id="7af05-158">För en indatauppsättning läser Data Factory den första raden som en rubrik.</span><span class="sxs-lookup"><span data-stu-id="7af05-158">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="7af05-159">För en utdatauppsättning skriver Data Factory den första raden som en rubrik.</span><span class="sxs-lookup"><span data-stu-id="7af05-159">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="7af05-160">Exempelscenarier finns i avsnittet med [användningsscenarier för `firstRowAsHeader` och `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="7af05-160">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="7af05-161">True</span><span class="sxs-lookup"><span data-stu-id="7af05-161">True</span></span><br/><span data-ttu-id="7af05-162">**False (standard)**</span><span class="sxs-lookup"><span data-stu-id="7af05-162">**False (default)**</span></span> |<span data-ttu-id="7af05-163">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-163">No</span></span> |
| <span data-ttu-id="7af05-164">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="7af05-164">skipLineCount</span></span> |<span data-ttu-id="7af05-165">Anger hello antal rader tooskip när data lästes från indatafiler.</span><span class="sxs-lookup"><span data-stu-id="7af05-165">Indicates hello number of rows tooskip when reading data from input files.</span></span> <span data-ttu-id="7af05-166">Om både skipLineCount och firstRowAsHeader anges, hello rader hoppas över först och sedan hello rubrikinformation läses från hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="7af05-166">If both skipLineCount and firstRowAsHeader are specified, hello lines are skipped first and then hello header information is read from hello input file.</span></span> <br/><br/><span data-ttu-id="7af05-167">Exempelscenarier finns i avsnittet med [användningsscenarier för `firstRowAsHeader` och `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="7af05-167">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="7af05-168">Integer</span><span class="sxs-lookup"><span data-stu-id="7af05-168">Integer</span></span> |<span data-ttu-id="7af05-169">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-169">No</span></span> |
| <span data-ttu-id="7af05-170">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="7af05-170">treatEmptyAsNull</span></span> |<span data-ttu-id="7af05-171">Anger om tootreat null eller en tom sträng som en null-värde när data lästes från en indatafil.</span><span class="sxs-lookup"><span data-stu-id="7af05-171">Specifies whether tootreat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="7af05-172">**True (standard)**</span><span class="sxs-lookup"><span data-stu-id="7af05-172">**True (default)**</span></span><br/><span data-ttu-id="7af05-173">False</span><span class="sxs-lookup"><span data-stu-id="7af05-173">False</span></span> |<span data-ttu-id="7af05-174">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-174">No</span></span> |

#### <a name="textformat-example"></a><span data-ttu-id="7af05-175">TextFormat-exempel</span><span class="sxs-lookup"><span data-stu-id="7af05-175">TextFormat example</span></span>
<span data-ttu-id="7af05-176">hello visar följande exempel några av hello formategenskaper för TextFormat.</span><span class="sxs-lookup"><span data-stu-id="7af05-176">hello following sample shows some of hello format properties for TextFormat.</span></span>

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

<span data-ttu-id="7af05-177">toouse en `escapeChar` i stället för `quoteChar`, Ersätt hello raden med `quoteChar` med hello följande escapeChar:</span><span class="sxs-lookup"><span data-stu-id="7af05-177">toouse an `escapeChar` instead of `quoteChar`, replace hello line with `quoteChar` with hello following escapeChar:</span></span>

```json
"escapeChar": "$",
```

#### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="7af05-178">Användningsscenarier för firstRowAsHeader och skipLineCount</span><span class="sxs-lookup"><span data-stu-id="7af05-178">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="7af05-179">Du kopierar från en icke-källa tooa textfil och vill tooadd en huvudrad som innehåller hello schemat metadata (till exempel: SQL-schemat).</span><span class="sxs-lookup"><span data-stu-id="7af05-179">You are copying from a non-file source tooa text file and would like tooadd a header line containing hello schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="7af05-180">Ange `firstRowAsHeader` som true i hello utdatauppsättningen för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="7af05-180">Specify `firstRowAsHeader` as true in hello output dataset for this scenario.</span></span>
* <span data-ttu-id="7af05-181">Du kopierar från en textfil som innehåller ett huvud rad tooa-fil som tar emot och vill toodrop rad.</span><span class="sxs-lookup"><span data-stu-id="7af05-181">You are copying from a text file containing a header line tooa non-file sink and would like toodrop that line.</span></span> <span data-ttu-id="7af05-182">Ange `firstRowAsHeader` som true i hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="7af05-182">Specify `firstRowAsHeader` as true in hello input dataset.</span></span>
* <span data-ttu-id="7af05-183">Du kopierar från en textfil och vill tooskip några rader hello början som innehåller ingen information för data eller huvudet.</span><span class="sxs-lookup"><span data-stu-id="7af05-183">You are copying from a text file and want tooskip a few lines at hello beginning that contain no data or header information.</span></span> <span data-ttu-id="7af05-184">Ange `skipLineCount` tooindicate hello antalet rader toobe hoppas över.</span><span class="sxs-lookup"><span data-stu-id="7af05-184">Specify `skipLineCount` tooindicate hello number of lines toobe skipped.</span></span> <span data-ttu-id="7af05-185">Om hello resten av hello-filen innehåller en rubrikrad, du kan också ange `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="7af05-185">If hello rest of hello file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="7af05-186">Om båda `skipLineCount` och `firstRowAsHeader` anges hello rader hoppas över först och sedan hello rubrikinformation läses från hello indatafilen</span><span class="sxs-lookup"><span data-stu-id="7af05-186">If both `skipLineCount` and `firstRowAsHeader` are specified, hello lines are skipped first and then hello header information is read from hello input file</span></span>

### <a name="specifying-jsonformat"></a><span data-ttu-id="7af05-187">Ange JsonFormat</span><span class="sxs-lookup"><span data-stu-id="7af05-187">Specifying JsonFormat</span></span>
<span data-ttu-id="7af05-188">för**import/export av JSON-filer som-är i/från Azure Cosmos DB**, se [Import/export JSON-dokument](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) avsnitt i hello Azure DB som Cosmos-koppling med information.</span><span class="sxs-lookup"><span data-stu-id="7af05-188">too**import/export JSON files as-is into/from Azure Cosmos DB**, see [Import/export JSON documents](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) section in hello Azure Cosmos DB connector with details.</span></span>

<span data-ttu-id="7af05-189">Om du vill tooparse hello JSON-filer eller skriva hello data i JSON-format, ange hello `format` `type` egenskapen för**JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="7af05-189">If you want tooparse hello JSON files or write hello data in JSON format, set hello `format` `type` property too**JsonFormat**.</span></span> <span data-ttu-id="7af05-190">Du kan också ange hello följande **valfria** egenskaper i hello `format` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7af05-190">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="7af05-191">Se [JsonFormat exempel](#jsonformat-example) avsnittet om hur tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="7af05-191">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="7af05-192">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7af05-192">Property</span></span> | <span data-ttu-id="7af05-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7af05-193">Description</span></span> | <span data-ttu-id="7af05-194">Krävs</span><span class="sxs-lookup"><span data-stu-id="7af05-194">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7af05-195">filePattern</span><span class="sxs-lookup"><span data-stu-id="7af05-195">filePattern</span></span> |<span data-ttu-id="7af05-196">Ange hello mönstret för data som lagras i varje JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="7af05-196">Indicate hello pattern of data stored in each JSON file.</span></span> <span data-ttu-id="7af05-197">Tillåtna värden är: **setOfObjects** och **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="7af05-197">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="7af05-198">Hej **standard** värdet är **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="7af05-198">hello **default** value is **setOfObjects**.</span></span> <span data-ttu-id="7af05-199">Detaljerad information om dessa mönster finns i avsnittet om [JSON-filmönster](#json-file-patterns).</span><span class="sxs-lookup"><span data-stu-id="7af05-199">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="7af05-200">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-200">No</span></span> |
| <span data-ttu-id="7af05-201">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="7af05-201">jsonNodeReference</span></span> | <span data-ttu-id="7af05-202">Om du vill tooiterate och extrahera data från hello-objekt i en matris fältet med hello samma mönster, ange hello JSON-sökvägen i matrisen.</span><span class="sxs-lookup"><span data-stu-id="7af05-202">If you want tooiterate and extract data from hello objects inside an array field with hello same pattern, specify hello JSON path of that array.</span></span> <span data-ttu-id="7af05-203">Den här egenskapen stöds endast när du kopierar data från JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="7af05-203">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="7af05-204">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-204">No</span></span> |
| <span data-ttu-id="7af05-205">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="7af05-205">jsonPathDefinition</span></span> | <span data-ttu-id="7af05-206">Ange hello JSON sökvägsuttryck för varje kolumnmappning med ett anpassat kolumnnamn (start med gemener).</span><span class="sxs-lookup"><span data-stu-id="7af05-206">Specify hello JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="7af05-207">Den här egenskapen stöds endast när du kopierar data från JSON-filer, och du kan extrahera data från objekt eller matriser.</span><span class="sxs-lookup"><span data-stu-id="7af05-207">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="7af05-208">Börja med $ root; för fält under rotobjektet för fält i hello matris som valts av `jsonNodeReference` start från hello matriselement-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7af05-208">For fields under root object, start with root $; for fields inside hello array chosen by `jsonNodeReference` property, start from hello array element.</span></span> <span data-ttu-id="7af05-209">Se [JsonFormat exempel](#jsonformat-example) avsnittet om hur tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="7af05-209">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span> | <span data-ttu-id="7af05-210">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-210">No</span></span> |
| <span data-ttu-id="7af05-211">encodingName</span><span class="sxs-lookup"><span data-stu-id="7af05-211">encodingName</span></span> |<span data-ttu-id="7af05-212">Ange hello kodningsnamn.</span><span class="sxs-lookup"><span data-stu-id="7af05-212">Specify hello encoding name.</span></span> <span data-ttu-id="7af05-213">Hello lista över giltiga kodning namn, se: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7af05-213">For hello list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="7af05-214">Exempel: windows-1250 or shift_jis.</span><span class="sxs-lookup"><span data-stu-id="7af05-214">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="7af05-215">Hej **standard** värde är: **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="7af05-215">hello **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="7af05-216">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-216">No</span></span> |
| <span data-ttu-id="7af05-217">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="7af05-217">nestingSeparator</span></span> |<span data-ttu-id="7af05-218">Tecken som används tooseparate kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="7af05-218">Character that is used tooseparate nesting levels.</span></span> <span data-ttu-id="7af05-219">hello standardvärdet är '.' (punkt).</span><span class="sxs-lookup"><span data-stu-id="7af05-219">hello default value is '.' (dot).</span></span> |<span data-ttu-id="7af05-220">Nej</span><span class="sxs-lookup"><span data-stu-id="7af05-220">No</span></span> |

#### <a name="json-file-patterns"></a><span data-ttu-id="7af05-221">JSON-filmönster</span><span class="sxs-lookup"><span data-stu-id="7af05-221">JSON file patterns</span></span>

<span data-ttu-id="7af05-222">Kopieringsaktiviteten kan parsa under JSON-filmönster:</span><span class="sxs-lookup"><span data-stu-id="7af05-222">Copy activity can parse below patterns of JSON files:</span></span>

- <span data-ttu-id="7af05-223">**Typ I: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="7af05-223">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="7af05-224">Varje fil som innehåller ett enskilt objekt eller flera radavgränsade/sammanfogade objekt.</span><span class="sxs-lookup"><span data-stu-id="7af05-224">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="7af05-225">När det här alternativet väljs i en utdatauppsättning genererar kopieringsaktiviteten en enskild JSON-fil med ett objekt per rad (radavgränsade).</span><span class="sxs-lookup"><span data-stu-id="7af05-225">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="7af05-226">**Exempel på JSON med enskilda objekt**</span><span class="sxs-lookup"><span data-stu-id="7af05-226">**single object JSON example**</span></span>

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

    * <span data-ttu-id="7af05-227">**Exempel med radavgränsad JSON**</span><span class="sxs-lookup"><span data-stu-id="7af05-227">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="7af05-228">**Exempel med sammanfogad JSON**</span><span class="sxs-lookup"><span data-stu-id="7af05-228">**concatenated JSON example**</span></span>

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

- <span data-ttu-id="7af05-229">**Typ II: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="7af05-229">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="7af05-230">Varje fil innehåller en matris med objekt.</span><span class="sxs-lookup"><span data-stu-id="7af05-230">Each file contains an array of objects.</span></span>

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

#### <a name="jsonformat-example"></a><span data-ttu-id="7af05-231">JsonFormat-exempel</span><span class="sxs-lookup"><span data-stu-id="7af05-231">JsonFormat example</span></span>

<span data-ttu-id="7af05-232">**Fall 1: Kopiera data från JSON-filer**</span><span class="sxs-lookup"><span data-stu-id="7af05-232">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="7af05-233">Nedan följer två typer av prover när du kopierar data från JSON-filer och hello generiska punkter toonote:</span><span class="sxs-lookup"><span data-stu-id="7af05-233">See below two types of samples when copying data from JSON files, and hello generic points toonote:</span></span>

<span data-ttu-id="7af05-234">**Exempel 1: hämta data från objektet och matrisen**</span><span class="sxs-lookup"><span data-stu-id="7af05-234">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="7af05-235">I det här exemplet du förväntar dig en JSON-rotobjektet mappar toosingle post i tabellform resultat.</span><span class="sxs-lookup"><span data-stu-id="7af05-235">In this sample, you expect one root JSON object maps toosingle record in tabular result.</span></span> <span data-ttu-id="7af05-236">Om du har en JSON-fil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="7af05-236">If you have a JSON file with hello following content:</span></span>  

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
<span data-ttu-id="7af05-237">och du vill toocopy den till en Azure SQL-tabellen i hello följande format genom att extrahera data från både objekt och matrisen:</span><span class="sxs-lookup"><span data-stu-id="7af05-237">and you want toocopy it into an Azure SQL table in hello following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="7af05-238">id</span><span class="sxs-lookup"><span data-stu-id="7af05-238">id</span></span> | <span data-ttu-id="7af05-239">deviceType</span><span class="sxs-lookup"><span data-stu-id="7af05-239">deviceType</span></span> | <span data-ttu-id="7af05-240">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="7af05-240">targetResourceType</span></span> | <span data-ttu-id="7af05-241">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="7af05-241">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="7af05-242">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="7af05-242">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="7af05-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="7af05-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="7af05-244">PC</span><span class="sxs-lookup"><span data-stu-id="7af05-244">PC</span></span> | <span data-ttu-id="7af05-245">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="7af05-245">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="7af05-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="7af05-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="7af05-247">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="7af05-247">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="7af05-248">hello inkommande datamängd med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar).</span><span class="sxs-lookup"><span data-stu-id="7af05-248">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="7af05-249">Mer specifikt:</span><span class="sxs-lookup"><span data-stu-id="7af05-249">More specifically:</span></span>

- <span data-ttu-id="7af05-250">`structure`avsnittet definierar hello anpassat kolumnnamn och hello motsvarande datatyp vid konvertering av tootabular data.</span><span class="sxs-lookup"><span data-stu-id="7af05-250">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="7af05-251">Det här avsnittet är **valfria** om du inte behöver toodo kolumnmappningen.</span><span class="sxs-lookup"><span data-stu-id="7af05-251">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="7af05-252">Mer information finns i avsnittet [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) (Ange strukturdefinition för rektangulära datauppsättningar).</span><span class="sxs-lookup"><span data-stu-id="7af05-252">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="7af05-253">`jsonPathDefinition`Anger hello JSON-sökvägen för varje kolumn som anger om tooextract hello data från.</span><span class="sxs-lookup"><span data-stu-id="7af05-253">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="7af05-254">toocopy data från en matris, som du kan använda **matris [x] .property** tooextract värdet för hello anges egenskapen från hello x objekt eller du kan använda  **matris [*] .property** toofind hello-värde från vilket objekt som innehåller sådan egenskap.</span><span class="sxs-lookup"><span data-stu-id="7af05-254">toocopy data from array, you can use **array[x].property** tooextract value of hello given property from hello xth object, or you can use **array[*].property** toofind hello value from any object containing such property.</span></span>

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

<span data-ttu-id="7af05-255">**Exempel 2: mellan tillämpa flera objekt med samma mönster från en matris hello**</span><span class="sxs-lookup"><span data-stu-id="7af05-255">**Sample 2: cross apply multiple objects with hello same pattern from array**</span></span>

<span data-ttu-id="7af05-256">I det här exemplet du förväntar dig tootransform en JSON-objekt i roten till flera poster i tabellform resultat.</span><span class="sxs-lookup"><span data-stu-id="7af05-256">In this sample, you expect tootransform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="7af05-257">Om du har en JSON-fil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="7af05-257">If you have a JSON file with hello following content:</span></span>  

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
<span data-ttu-id="7af05-258">och du vill toocopy den till en Azure SQL-tabellen i hello följande format genom att förenkla hello data i hello matris och korskoppling med hello vanliga rot-information:</span><span class="sxs-lookup"><span data-stu-id="7af05-258">and you want toocopy it into an Azure SQL table in hello following format, by flattening hello data inside hello array and cross join with hello common root info:</span></span>

| <span data-ttu-id="7af05-259">ordernumber</span><span class="sxs-lookup"><span data-stu-id="7af05-259">ordernumber</span></span> | <span data-ttu-id="7af05-260">orderdate</span><span class="sxs-lookup"><span data-stu-id="7af05-260">orderdate</span></span> | <span data-ttu-id="7af05-261">order_pd</span><span class="sxs-lookup"><span data-stu-id="7af05-261">order_pd</span></span> | <span data-ttu-id="7af05-262">order_price</span><span class="sxs-lookup"><span data-stu-id="7af05-262">order_price</span></span> | <span data-ttu-id="7af05-263">city</span><span class="sxs-lookup"><span data-stu-id="7af05-263">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="7af05-264">01</span><span class="sxs-lookup"><span data-stu-id="7af05-264">01</span></span> | <span data-ttu-id="7af05-265">20170122</span><span class="sxs-lookup"><span data-stu-id="7af05-265">20170122</span></span> | <span data-ttu-id="7af05-266">P1</span><span class="sxs-lookup"><span data-stu-id="7af05-266">P1</span></span> | <span data-ttu-id="7af05-267">23</span><span class="sxs-lookup"><span data-stu-id="7af05-267">23</span></span> | <span data-ttu-id="7af05-268">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="7af05-268">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="7af05-269">01</span><span class="sxs-lookup"><span data-stu-id="7af05-269">01</span></span> | <span data-ttu-id="7af05-270">20170122</span><span class="sxs-lookup"><span data-stu-id="7af05-270">20170122</span></span> | <span data-ttu-id="7af05-271">P2</span><span class="sxs-lookup"><span data-stu-id="7af05-271">P2</span></span> | <span data-ttu-id="7af05-272">13</span><span class="sxs-lookup"><span data-stu-id="7af05-272">13</span></span> | <span data-ttu-id="7af05-273">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="7af05-273">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="7af05-274">01</span><span class="sxs-lookup"><span data-stu-id="7af05-274">01</span></span> | <span data-ttu-id="7af05-275">20170122</span><span class="sxs-lookup"><span data-stu-id="7af05-275">20170122</span></span> | <span data-ttu-id="7af05-276">P3</span><span class="sxs-lookup"><span data-stu-id="7af05-276">P3</span></span> | <span data-ttu-id="7af05-277">231</span><span class="sxs-lookup"><span data-stu-id="7af05-277">231</span></span> | <span data-ttu-id="7af05-278">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="7af05-278">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="7af05-279">hello inkommande datamängd med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar).</span><span class="sxs-lookup"><span data-stu-id="7af05-279">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="7af05-280">Mer specifikt:</span><span class="sxs-lookup"><span data-stu-id="7af05-280">More specifically:</span></span>

- <span data-ttu-id="7af05-281">`structure`avsnittet definierar hello anpassat kolumnnamn och hello motsvarande datatyp vid konvertering av tootabular data.</span><span class="sxs-lookup"><span data-stu-id="7af05-281">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="7af05-282">Det här avsnittet är **valfria** om du inte behöver toodo kolumnmappningen.</span><span class="sxs-lookup"><span data-stu-id="7af05-282">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="7af05-283">Mer information finns i avsnittet [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) (Ange strukturdefinition för rektangulära datauppsättningar).</span><span class="sxs-lookup"><span data-stu-id="7af05-283">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="7af05-284">`jsonNodeReference`Anger tooiterate och extrahera data från hello-objekt med samma mönster under hello **matris** orderlines.</span><span class="sxs-lookup"><span data-stu-id="7af05-284">`jsonNodeReference` indicates tooiterate and extract data from hello objects with hello same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="7af05-285">`jsonPathDefinition`Anger hello JSON-sökvägen för varje kolumn som anger om tooextract hello data från.</span><span class="sxs-lookup"><span data-stu-id="7af05-285">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="7af05-286">I det här exemplet är ”ordernumber”, ”orderdate” och ”ort” under rotobjektet med JSON-sökvägen som börjar med ”$.”, medan ”order_pd” och ”order_price” definieras med sökvägen som härletts från hello matriselement utan ”$”.</span><span class="sxs-lookup"><span data-stu-id="7af05-286">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from hello array element without "$.".</span></span>

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

<span data-ttu-id="7af05-287">**Observera följande punkter hello:**</span><span class="sxs-lookup"><span data-stu-id="7af05-287">**Note hello following points:**</span></span>

* <span data-ttu-id="7af05-288">Om hello `structure` och `jsonPathDefinition` har inte definierats i hello Data Factory dataset hello Kopieringsaktiviteten identifierar hello schema från första hello-objektet och förenkla hello hela objektet.</span><span class="sxs-lookup"><span data-stu-id="7af05-288">If hello `structure` and `jsonPathDefinition` are not defined in hello Data Factory dataset, hello Copy Activity detects hello schema from hello first object and flatten hello whole object.</span></span>
* <span data-ttu-id="7af05-289">Om hello JSON-indata har en matris, som standard konverterar hello Kopieringsaktiviteten hello hela Matrisvärde till en sträng.</span><span class="sxs-lookup"><span data-stu-id="7af05-289">If hello JSON input has an array, by default hello Copy Activity converts hello entire array value into a string.</span></span> <span data-ttu-id="7af05-290">Du kan välja tooextract data från den med hjälp av `jsonNodeReference` och/eller `jsonPathDefinition`, eller hoppa över det genom att inte ange det i `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="7af05-290">You can choose tooextract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="7af05-291">Om det finns duplicerade namn på Hej samma nivå, hello Kopieringsaktiviteten hämtar hello senast.</span><span class="sxs-lookup"><span data-stu-id="7af05-291">If there are duplicate names at hello same level, hello Copy Activity picks hello last one.</span></span>
* <span data-ttu-id="7af05-292">Egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="7af05-292">Property names are case-sensitive.</span></span> <span data-ttu-id="7af05-293">Två egenskaper med samma namn men med olika skiftlägen behandlas som två olika egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7af05-293">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="7af05-294">**Fall 2: Skriver data tooJSON-fil**</span><span class="sxs-lookup"><span data-stu-id="7af05-294">**Case 2: Writing data tooJSON file**</span></span>

<span data-ttu-id="7af05-295">Anta att du har följande tabell i SQL Database:</span><span class="sxs-lookup"><span data-stu-id="7af05-295">If you have below table in SQL Database:</span></span>

| <span data-ttu-id="7af05-296">id</span><span class="sxs-lookup"><span data-stu-id="7af05-296">id</span></span> | <span data-ttu-id="7af05-297">order_date</span><span class="sxs-lookup"><span data-stu-id="7af05-297">order_date</span></span> | <span data-ttu-id="7af05-298">order_price</span><span class="sxs-lookup"><span data-stu-id="7af05-298">order_price</span></span> | <span data-ttu-id="7af05-299">order_by</span><span class="sxs-lookup"><span data-stu-id="7af05-299">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7af05-300">1</span><span class="sxs-lookup"><span data-stu-id="7af05-300">1</span></span> | <span data-ttu-id="7af05-301">20170119</span><span class="sxs-lookup"><span data-stu-id="7af05-301">20170119</span></span> | <span data-ttu-id="7af05-302">2000</span><span class="sxs-lookup"><span data-stu-id="7af05-302">2000</span></span> | <span data-ttu-id="7af05-303">David</span><span class="sxs-lookup"><span data-stu-id="7af05-303">David</span></span> |
| <span data-ttu-id="7af05-304">2</span><span class="sxs-lookup"><span data-stu-id="7af05-304">2</span></span> | <span data-ttu-id="7af05-305">20170120</span><span class="sxs-lookup"><span data-stu-id="7af05-305">20170120</span></span> | <span data-ttu-id="7af05-306">3500</span><span class="sxs-lookup"><span data-stu-id="7af05-306">3500</span></span> | <span data-ttu-id="7af05-307">Patrick</span><span class="sxs-lookup"><span data-stu-id="7af05-307">Patrick</span></span> |
| <span data-ttu-id="7af05-308">3</span><span class="sxs-lookup"><span data-stu-id="7af05-308">3</span></span> | <span data-ttu-id="7af05-309">20170121</span><span class="sxs-lookup"><span data-stu-id="7af05-309">20170121</span></span> | <span data-ttu-id="7af05-310">4000</span><span class="sxs-lookup"><span data-stu-id="7af05-310">4000</span></span> | <span data-ttu-id="7af05-311">Jason</span><span class="sxs-lookup"><span data-stu-id="7af05-311">Jason</span></span> |

<span data-ttu-id="7af05-312">och för varje post du förväntar dig toowrite tooa JSON-objekt i nedan format:</span><span class="sxs-lookup"><span data-stu-id="7af05-312">and for each record, you expect toowrite tooa JSON object in below format:</span></span>
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

<span data-ttu-id="7af05-313">Hej utdatauppsättningen med **JsonFormat** typ definieras enligt följande: (partiell definition med endast hello relevanta delar).</span><span class="sxs-lookup"><span data-stu-id="7af05-313">hello output dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="7af05-314">Mer specifikt `structure` avsnittet definierar hello anpassade egenskapsnamn i målfilen och `nestingSeparator` (standardvärdet är ””.) kommer att använda tooidentify hello kapsla lagret från hello namn.</span><span class="sxs-lookup"><span data-stu-id="7af05-314">More specifically, `structure` section defines hello customized property names in destination file, `nestingSeparator` (default is ".") will be used tooidentify hello nest layer from hello name.</span></span> <span data-ttu-id="7af05-315">Det här avsnittet är **valfria** om du vill jämföra med källkolumnsnamnet toochange hello egenskapens namn eller kapsla några av hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7af05-315">This section is **optional** unless you want toochange hello property name comparing with source column name, or nest some of hello properties.</span></span>

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

### <a name="specifying-avroformat"></a><span data-ttu-id="7af05-316">Ange AvroFormat</span><span class="sxs-lookup"><span data-stu-id="7af05-316">Specifying AvroFormat</span></span>
<span data-ttu-id="7af05-317">Om du vill tooparse hello Avro-filernas eller skriva hello data i Avro-formatet, ange hello `format` `type` egenskapen för**AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="7af05-317">If you want tooparse hello Avro files or write hello data in Avro format, set hello `format` `type` property too**AvroFormat**.</span></span> <span data-ttu-id="7af05-318">Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7af05-318">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="7af05-319">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7af05-319">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="7af05-320">toouse Avro-formatet i en Hive-tabell, kan du läsa för[Apache Hive kursen](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="7af05-320">toouse Avro format in a Hive table, you can refer too[Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="7af05-321">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="7af05-321">Note hello following points:</span></span>  

* <span data-ttu-id="7af05-322">[Komplexa datatyper](http://avro.apache.org/docs/current/spec.html#schema_complex) stöds inte (poster, uppräkningar, matriser, mappningar, unioner och fasta).</span><span class="sxs-lookup"><span data-stu-id="7af05-322">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions and fixed).</span></span>

### <a name="specifying-orcformat"></a><span data-ttu-id="7af05-323">Ange OrcFormat</span><span class="sxs-lookup"><span data-stu-id="7af05-323">Specifying OrcFormat</span></span>
<span data-ttu-id="7af05-324">Om du vill tooparse hello ORC-filer eller skriva hello data i ORC-format, ange hello `format` `type` egenskapen för**OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="7af05-324">If you want tooparse hello ORC files or write hello data in ORC format, set hello `format` `type` property too**OrcFormat**.</span></span> <span data-ttu-id="7af05-325">Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7af05-325">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="7af05-326">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7af05-326">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="7af05-327">Om du inte kopierar ORC-filer **som-är** mellan lokala och moln datalager, du behöver tooinstall hello 8 JRE (Java Runtime Environment) på din gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="7af05-327">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="7af05-328">En 64-bitars gateway kräver 64-bitars JRE och en 32-bitars gateway kräver 32-bitars JRE.</span><span class="sxs-lookup"><span data-stu-id="7af05-328">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="7af05-329">Du hittar båda versionerna [här](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="7af05-329">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="7af05-330">Välja hello lämplig.</span><span class="sxs-lookup"><span data-stu-id="7af05-330">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="7af05-331">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="7af05-331">Note hello following points:</span></span>

* <span data-ttu-id="7af05-332">Komplexa datatyper stöds inte (STRUCT, MAP, LIST, UNION)</span><span class="sxs-lookup"><span data-stu-id="7af05-332">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="7af05-333">ORC-filen har tre [komprimeringsrelaterade alternativ](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB och SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="7af05-333">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="7af05-334">Data Factory stöder läsning av data från ORC-filer i alla dessa komprimerade format.</span><span class="sxs-lookup"><span data-stu-id="7af05-334">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="7af05-335">Den använder hello komprimering codec finns i hello metadata tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="7af05-335">It uses hello compression codec is in hello metadata tooread hello data.</span></span> <span data-ttu-id="7af05-336">Men när du skriver tooan ORC filen väljer Data Factory ZLIB, som är hello standard för ORC.</span><span class="sxs-lookup"><span data-stu-id="7af05-336">However, when writing tooan ORC file, Data Factory chooses ZLIB, which is hello default for ORC.</span></span> <span data-ttu-id="7af05-337">För närvarande finns ingen alternativet toooverride detta beteende.</span><span class="sxs-lookup"><span data-stu-id="7af05-337">Currently, there is no option toooverride this behavior.</span></span>

### <a name="specifying-parquetformat"></a><span data-ttu-id="7af05-338">Ange ParquetFormat</span><span class="sxs-lookup"><span data-stu-id="7af05-338">Specifying ParquetFormat</span></span>
<span data-ttu-id="7af05-339">Om du vill tooparse hello parkettgolv filer eller skriva hello data i parkettgolv format, ange hello `format` `type` egenskapen för**ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="7af05-339">If you want tooparse hello Parquet files or write hello data in Parquet format, set hello `format` `type` property too**ParquetFormat**.</span></span> <span data-ttu-id="7af05-340">Du behöver inte toospecify alla egenskaper i avsnittet för hello-Format i hello typeProperties avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7af05-340">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="7af05-341">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7af05-341">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="7af05-342">Om du inte kopierar filer parkettgolv **som-är** mellan lokala och moln datalager, du behöver tooinstall hello 8 JRE (Java Runtime Environment) på din gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="7af05-342">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="7af05-343">En 64-bitars gateway kräver 64-bitars JRE och en 32-bitars gateway kräver 32-bitars JRE.</span><span class="sxs-lookup"><span data-stu-id="7af05-343">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="7af05-344">Du hittar båda versionerna [här](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="7af05-344">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="7af05-345">Välja hello lämplig.</span><span class="sxs-lookup"><span data-stu-id="7af05-345">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="7af05-346">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="7af05-346">Note hello following points:</span></span>

* <span data-ttu-id="7af05-347">Komplexa datatyper stöds inte (MAP, LIST)</span><span class="sxs-lookup"><span data-stu-id="7af05-347">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="7af05-348">Parkettgolv filen har följande alternativ för komprimering-relaterade hello: NONE, SNAPPY GZIP och LZO.</span><span class="sxs-lookup"><span data-stu-id="7af05-348">Parquet file has hello following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="7af05-349">Data Factory stöder läsning av data från ORC-filer i alla dessa komprimerade format.</span><span class="sxs-lookup"><span data-stu-id="7af05-349">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="7af05-350">Hello komprimerings-codec används i hello metadata tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="7af05-350">It uses hello compression codec in hello metadata tooread hello data.</span></span> <span data-ttu-id="7af05-351">Men när du skriver tooa parkettgolv filen väljer Data Factory SNAPPY, vilket är standard hello parkettgolv format.</span><span class="sxs-lookup"><span data-stu-id="7af05-351">However, when writing tooa Parquet file, Data Factory chooses SNAPPY, which is hello default for Parquet format.</span></span> <span data-ttu-id="7af05-352">För närvarande finns ingen alternativet toooverride detta beteende.</span><span class="sxs-lookup"><span data-stu-id="7af05-352">Currently, there is no option toooverride this behavior.</span></span>
