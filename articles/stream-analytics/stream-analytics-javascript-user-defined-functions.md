---
title: "aaaAzure Stream Analytics JavaScript användardefinierade funktioner | Microsoft Docs"
description: "Utföra en avancerad fråga säkerhetsnivån med JavaScript användardefinierade funktioner"
keywords: "JavaScript, användardefinierade funktioner, udf"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 28eeb8f6437c23989e8887687b950361fed4414c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a><span data-ttu-id="af640-104">Azure Stream Analytics JavaScript användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="af640-104">Azure Stream Analytics JavaScript user-defined functions</span></span>
<span data-ttu-id="af640-105">Azure Stream Analytics stöder användardefinierade funktioner som skrivits i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="af640-105">Azure Stream Analytics supports user-defined functions written in JavaScript.</span></span> <span data-ttu-id="af640-106">Med hello omfattande uppsättning **sträng**, **RegExp**, **matematiska**, **matris**, och **datum** metoder som Ger JavaScript, komplexa Datatransformationer med Stream Analytics-jobb blir enklare toocreate.</span><span class="sxs-lookup"><span data-stu-id="af640-106">With hello rich set of **String**, **RegExp**, **Math**, **Array**, and **Date** methods that JavaScript provides, complex data transformations with Stream Analytics jobs become easier toocreate.</span></span>

## <a name="javascript-user-defined-functions"></a><span data-ttu-id="af640-107">JavaScript användardefinierade funktioner</span><span class="sxs-lookup"><span data-stu-id="af640-107">JavaScript user-defined functions</span></span>
<span data-ttu-id="af640-108">JavaScript användardefinierade funktioner stöder tillståndslösa, endast beräkning skalärfunktioner som inte kräver extern anslutning.</span><span class="sxs-lookup"><span data-stu-id="af640-108">JavaScript user-defined functions support stateless, compute-only scalar functions that do not require external connectivity.</span></span> <span data-ttu-id="af640-109">hello returnera värdet för en funktion kan endast vara ett skalärvärde (enkel).</span><span class="sxs-lookup"><span data-stu-id="af640-109">hello return value of a function can only be a scalar (single) value.</span></span> <span data-ttu-id="af640-110">När du lägger till ett JavaScript användardefinierad funktion tooa jobb, kan du använda hello funktionen var som helst i hello-frågan som en inbyggd skalärfunktion.</span><span class="sxs-lookup"><span data-stu-id="af640-110">After you add a JavaScript user-defined function tooa job, you can use hello function anywhere in hello query, like a built-in scalar function.</span></span>

<span data-ttu-id="af640-111">Här följer några scenarier där användbara JavaScript användardefinierade funktioner:</span><span class="sxs-lookup"><span data-stu-id="af640-111">Here are some scenarios where you might find JavaScript user-defined functions useful:</span></span>
* <span data-ttu-id="af640-112">Tolkning och manipulera strängar som har funktioner för reguljära uttryck, till exempel **Regexp_Replace()** och **Regexp_Extract()**</span><span class="sxs-lookup"><span data-stu-id="af640-112">Parsing and manipulating strings that have regular expression functions, for example, **Regexp_Replace()** and **Regexp_Extract()**</span></span>
* <span data-ttu-id="af640-113">Avkoda och kodning data, till exempel binary-hex-konvertering</span><span class="sxs-lookup"><span data-stu-id="af640-113">Decoding and encoding data, for example, binary-to-hex conversion</span></span>
* <span data-ttu-id="af640-114">Utföra matematiska beräkningar med JavaScript **matematiska** funktioner</span><span class="sxs-lookup"><span data-stu-id="af640-114">Performing mathematic computations with JavaScript **Math** functions</span></span>
* <span data-ttu-id="af640-115">Utför åtgärder i matrisen som sortera, koppling, Sök och fill</span><span class="sxs-lookup"><span data-stu-id="af640-115">Performing array operations like sort, join, find, and fill</span></span>

<span data-ttu-id="af640-116">Här följer några saker som du inte kan göra med en JavaScript-användardefinierad funktion i Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="af640-116">Here are some things that you cannot do with a JavaScript user-defined function in Stream Analytics:</span></span>
* <span data-ttu-id="af640-117">Anropet ut externa REST-slutpunkter, till exempel utför omvänd sökning av IP- eller datahämtning referensdata från en extern källa</span><span class="sxs-lookup"><span data-stu-id="af640-117">Call out external REST endpoints, for example, performing reverse IP lookup or pulling reference data from an external source</span></span>
* <span data-ttu-id="af640-118">Utför anpassade händelsen format serialisering eller avserialisering på indata/utdata</span><span class="sxs-lookup"><span data-stu-id="af640-118">Perform custom event format serialization or deserialization on inputs/outputs</span></span>
* <span data-ttu-id="af640-119">Skapa anpassade mängder</span><span class="sxs-lookup"><span data-stu-id="af640-119">Create custom aggregates</span></span>

<span data-ttu-id="af640-120">Även om fungerar som **Date.GetDate()** eller **Math.random()** inte är blockerade i hello funktioner definition du bör undvika att använda dem.</span><span class="sxs-lookup"><span data-stu-id="af640-120">Although functions like **Date.GetDate()** or **Math.random()** are not blocked in hello functions definition, you should avoid using them.</span></span> <span data-ttu-id="af640-121">Dessa funktioner **inte** returnerade hello samma varje gång du anropar dem och hello Azure Stream Analytics-tjänsten inte har en journal av funktionsanrop och kan medföra returnerade resultat.</span><span class="sxs-lookup"><span data-stu-id="af640-121">These functions **do not** return hello same result every time you call them, and hello Azure Stream Analytics service does not keep a journal of function invocations and returned results.</span></span> <span data-ttu-id="af640-122">Om en funktion returnerar olika resultat på hello samma händelser, garanteras inte repeterbarhet när ett jobb startas av dig eller hello Stream Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="af640-122">If a function returns different result on hello same events, repeatability is not guaranteed when a job is restarted by you or by hello Stream Analytics service.</span></span>

## <a name="add-a-javascript-user-defined-function-in-hello-azure-portal"></a><span data-ttu-id="af640-123">Lägg till JavaScript användardefinierad funktion i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="af640-123">Add a JavaScript user-defined function in hello Azure portal</span></span>
<span data-ttu-id="af640-124">toocreate en enkel JavaScript användardefinierad funktion under ett befintligt Stream Analytics-jobb, utföra de här stegen:</span><span class="sxs-lookup"><span data-stu-id="af640-124">toocreate a simple JavaScript user-defined function under an existing Stream Analytics job, do these steps:</span></span>

1.  <span data-ttu-id="af640-125">Hello Azure-portalen, hitta Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="af640-125">In hello Azure portal, find your Stream Analytics job.</span></span>
2.  <span data-ttu-id="af640-126">Under **jobbet TOPOLOGI**, Välj din funktion.</span><span class="sxs-lookup"><span data-stu-id="af640-126">Under **JOB TOPOLOGY**, select your function.</span></span> <span data-ttu-id="af640-127">En tom lista över funktioner visas.</span><span class="sxs-lookup"><span data-stu-id="af640-127">An empty list of functions appears.</span></span>
3.  <span data-ttu-id="af640-128">toocreate en ny användardefinierad funktion, Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="af640-128">toocreate a new user-defined function, select **Add**.</span></span>
4.  <span data-ttu-id="af640-129">På hello **nya funktionen** bladet för **funktionstyp**väljer **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="af640-129">On hello **New Function** blade, for **Function Type**, select **JavaScript**.</span></span> <span data-ttu-id="af640-130">En standardmall för funktionen visas i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="af640-130">A default function template appears in hello editor.</span></span>
5.  <span data-ttu-id="af640-131">För hello **UDF alias**, ange **hex2Int**, och ändra hello funktionen implementering på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="af640-131">For hello **UDF alias**, enter **hex2Int**, and change hello function implementation as follows:</span></span>

    ```
    // Convert Hex value toointeger.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  <span data-ttu-id="af640-132">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="af640-132">Select **Save**.</span></span> <span data-ttu-id="af640-133">Funktionen visas i hello lista över funktioner.</span><span class="sxs-lookup"><span data-stu-id="af640-133">Your function appears in hello list of functions.</span></span>
7.  <span data-ttu-id="af640-134">Välj ny hello **hex2Int** fungerar och kontrollera hello funktionsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="af640-134">Select hello new **hex2Int** function, and check hello function definition.</span></span> <span data-ttu-id="af640-135">Alla funktioner har en **UDF** prefix tillagda toohello funktionen alias.</span><span class="sxs-lookup"><span data-stu-id="af640-135">All functions have a **UDF** prefix added toohello function alias.</span></span> <span data-ttu-id="af640-136">Du behöver för*hello prefixet* när du anropar funktionen hello i Stream Analytics-fråga.</span><span class="sxs-lookup"><span data-stu-id="af640-136">You need too*include hello prefix* when you call hello function in your Stream Analytics query.</span></span> <span data-ttu-id="af640-137">I så fall måste du anropa **UDF.hex2Int**.</span><span class="sxs-lookup"><span data-stu-id="af640-137">In this case, you call **UDF.hex2Int**.</span></span>

## <a name="call-a-javascript-user-defined-function-in-a-query"></a><span data-ttu-id="af640-138">Anropa en användardefinierad JavaScript-funktion i en fråga</span><span class="sxs-lookup"><span data-stu-id="af640-138">Call a JavaScript user-defined function in a query</span></span>

1. <span data-ttu-id="af640-139">I hello frågan editor under **jobbet TOPOLOGI**väljer **frågan**.</span><span class="sxs-lookup"><span data-stu-id="af640-139">In hello query editor, under **JOB TOPOLOGY**, select **Query**.</span></span>
2.  <span data-ttu-id="af640-140">Redigera frågan och sedan anropa hello användardefinierad funktion, så här:</span><span class="sxs-lookup"><span data-stu-id="af640-140">Edit your query, and then call hello user-defined function, like this:</span></span>

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  <span data-ttu-id="af640-141">tooupload hello exempeldatafil, högerklicka på hello jobbet indata.</span><span class="sxs-lookup"><span data-stu-id="af640-141">tooupload hello sample data file, right-click hello job input.</span></span>
4.  <span data-ttu-id="af640-142">tootest frågan, Välj **Test**.</span><span class="sxs-lookup"><span data-stu-id="af640-142">tootest your query, select **Test**.</span></span>


## <a name="supported-javascript-objects"></a><span data-ttu-id="af640-143">Stöds JavaScript-objekt</span><span class="sxs-lookup"><span data-stu-id="af640-143">Supported JavaScript objects</span></span>
<span data-ttu-id="af640-144">Azure Stream Analytics JavaScript användardefinierade funktioner stöder standard, inbyggda JavaScript-objekt.</span><span class="sxs-lookup"><span data-stu-id="af640-144">Azure Stream Analytics JavaScript user-defined functions support standard, built-in JavaScript objects.</span></span> <span data-ttu-id="af640-145">En lista över de här objekten finns [globala objekt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span><span class="sxs-lookup"><span data-stu-id="af640-145">For a list of these objects, see [Global Objects](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span></span>

### <a name="stream-analytics-and-javascript-type-conversion"></a><span data-ttu-id="af640-146">Stream Analytics och JavaScript typkonvertering</span><span class="sxs-lookup"><span data-stu-id="af640-146">Stream Analytics and JavaScript type conversion</span></span>

<span data-ttu-id="af640-147">Det finns skillnader i hello-typer som har stöd för hello Stream Analytics-frågespråket och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="af640-147">There are differences in hello types that hello Stream Analytics query language and JavaScript support.</span></span> <span data-ttu-id="af640-148">Den här tabellen visar hello konvertering mappningar mellan hello två:</span><span class="sxs-lookup"><span data-stu-id="af640-148">This table lists hello conversion mappings between hello two:</span></span>

<span data-ttu-id="af640-149">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af640-149">Stream Analytics</span></span> | <span data-ttu-id="af640-150">JavaScript</span><span class="sxs-lookup"><span data-stu-id="af640-150">JavaScript</span></span>
--- | ---
<span data-ttu-id="af640-151">bigint</span><span class="sxs-lookup"><span data-stu-id="af640-151">bigint</span></span> | <span data-ttu-id="af640-152">Antal (JavaScript kan endast representera heltal in tooprecisely 2 ^ 53)</span><span class="sxs-lookup"><span data-stu-id="af640-152">Number (JavaScript can only represent integers up tooprecisely 2^53)</span></span>
<span data-ttu-id="af640-153">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="af640-153">DateTime</span></span> | <span data-ttu-id="af640-154">Datum (JavaScript endast stöder millisekunder)</span><span class="sxs-lookup"><span data-stu-id="af640-154">Date (JavaScript only supports milliseconds)</span></span>
<span data-ttu-id="af640-155">dubbla</span><span class="sxs-lookup"><span data-stu-id="af640-155">double</span></span> | <span data-ttu-id="af640-156">Tal</span><span class="sxs-lookup"><span data-stu-id="af640-156">Number</span></span>
<span data-ttu-id="af640-157">nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="af640-157">nvarchar(MAX)</span></span> | <span data-ttu-id="af640-158">Sträng</span><span class="sxs-lookup"><span data-stu-id="af640-158">String</span></span>
<span data-ttu-id="af640-159">Post</span><span class="sxs-lookup"><span data-stu-id="af640-159">Record</span></span> | <span data-ttu-id="af640-160">Objekt</span><span class="sxs-lookup"><span data-stu-id="af640-160">Object</span></span>
<span data-ttu-id="af640-161">Matris</span><span class="sxs-lookup"><span data-stu-id="af640-161">Array</span></span> | <span data-ttu-id="af640-162">Matris</span><span class="sxs-lookup"><span data-stu-id="af640-162">Array</span></span>
<span data-ttu-id="af640-163">NULL</span><span class="sxs-lookup"><span data-stu-id="af640-163">NULL</span></span> | <span data-ttu-id="af640-164">Null</span><span class="sxs-lookup"><span data-stu-id="af640-164">Null</span></span>


<span data-ttu-id="af640-165">Här är JavaScript-Stream Analytics-konvertering:</span><span class="sxs-lookup"><span data-stu-id="af640-165">Here are JavaScript-to-Stream Analytics conversions:</span></span>


<span data-ttu-id="af640-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="af640-166">JavaScript</span></span> | <span data-ttu-id="af640-167">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af640-167">Stream Analytics</span></span>
--- | ---
<span data-ttu-id="af640-168">Tal</span><span class="sxs-lookup"><span data-stu-id="af640-168">Number</span></span> | <span data-ttu-id="af640-169">Bigint (om hello numret är runda och mellan lång. MinValue och lång. MaxValue; Annars är det dubbla)</span><span class="sxs-lookup"><span data-stu-id="af640-169">Bigint (if hello number is round and between long.MinValue and long.MaxValue; otherwise, it's double)</span></span>
<span data-ttu-id="af640-170">Date</span><span class="sxs-lookup"><span data-stu-id="af640-170">Date</span></span> | <span data-ttu-id="af640-171">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="af640-171">DateTime</span></span>
<span data-ttu-id="af640-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="af640-172">String</span></span> | <span data-ttu-id="af640-173">nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="af640-173">nvarchar(MAX)</span></span>
<span data-ttu-id="af640-174">Objekt</span><span class="sxs-lookup"><span data-stu-id="af640-174">Object</span></span> | <span data-ttu-id="af640-175">Post</span><span class="sxs-lookup"><span data-stu-id="af640-175">Record</span></span>
<span data-ttu-id="af640-176">Matris</span><span class="sxs-lookup"><span data-stu-id="af640-176">Array</span></span> | <span data-ttu-id="af640-177">Matris</span><span class="sxs-lookup"><span data-stu-id="af640-177">Array</span></span>
<span data-ttu-id="af640-178">Null, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="af640-178">Null, Undefined</span></span> | <span data-ttu-id="af640-179">NULL</span><span class="sxs-lookup"><span data-stu-id="af640-179">NULL</span></span>
<span data-ttu-id="af640-180">En annan typ (till exempel en funktion eller fel)</span><span class="sxs-lookup"><span data-stu-id="af640-180">Any other type (for example, a function or error)</span></span> | <span data-ttu-id="af640-181">Stöds inte (resulterar i körningsfel)</span><span class="sxs-lookup"><span data-stu-id="af640-181">Not supported (results in runtime error)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="af640-182">Felsökning</span><span class="sxs-lookup"><span data-stu-id="af640-182">Troubleshooting</span></span>
<span data-ttu-id="af640-183">JavaScript-körningsfel betraktas som allvarligt och är anslutna via hello aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="af640-183">JavaScript runtime errors are considered fatal, and are surfaced through hello Activity log.</span></span> <span data-ttu-id="af640-184">tooretrieve hello logg i hello Azure-portalen, gå tooyour jobb och välj **aktivitetsloggen**.</span><span class="sxs-lookup"><span data-stu-id="af640-184">tooretrieve hello log, in hello Azure portal, go tooyour job and select **Activity log**.</span></span>


## <a name="other-javascript-user-defined-function-patterns"></a><span data-ttu-id="af640-185">Andra JavaScript användardefinierad funktion mönster</span><span class="sxs-lookup"><span data-stu-id="af640-185">Other JavaScript user-defined function patterns</span></span>

### <a name="write-nested-json-toooutput"></a><span data-ttu-id="af640-186">Skriva den kapslade JSON toooutput</span><span class="sxs-lookup"><span data-stu-id="af640-186">Write nested JSON toooutput</span></span>
<span data-ttu-id="af640-187">Om du har en uppföljning bearbetningssteg som använder en Stream Analytics-jobbet utdata som indata, och det krävs en JSON-format, kan du skriva toooutput en JSON-sträng.</span><span class="sxs-lookup"><span data-stu-id="af640-187">If you have a follow-up processing step that uses a Stream Analytics job output as input, and it requires a JSON format, you can write a JSON string toooutput.</span></span> <span data-ttu-id="af640-188">hello nästa exempel anropar hello **JSON.stringify()** fungerar toopack alla namn/värde-par av hello indata och skriva dem som ett enda strängvärde i utdata.</span><span class="sxs-lookup"><span data-stu-id="af640-188">hello next example calls hello **JSON.stringify()** function toopack all name/value pairs of hello input, and then write them as a single string value in output.</span></span>

<span data-ttu-id="af640-189">**Definition av JavaScript användardefinierad funktion:**</span><span class="sxs-lookup"><span data-stu-id="af640-189">**JavaScript user-defined function definition:**</span></span>

```
function main(x) {
return JSON.stringify(x);
}
```

<span data-ttu-id="af640-190">**Exempelfråga:**</span><span class="sxs-lookup"><span data-stu-id="af640-190">**Sample query:**</span></span>
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a><span data-ttu-id="af640-191">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="af640-191">Get help</span></span>
<span data-ttu-id="af640-192">För mer hjälp kan du prova vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="af640-192">For additional help, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="af640-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af640-193">Next steps</span></span>
* [<span data-ttu-id="af640-194">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af640-194">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="af640-195">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af640-195">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="af640-196">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="af640-196">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="af640-197">Azure Stream Analytics query language-referens</span><span class="sxs-lookup"><span data-stu-id="af640-197">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="af640-198">Azure Stream Analytics management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="af640-198">Azure Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
