---
title: "aaaList Azure Storage-resurser med hello Storage-klientbibliotek för C++ | Microsoft Docs"
description: "Lär dig hur toouse hello visar en lista över API: er i Microsoft Azure Storage-klientbibliotek för C++ tooenumerate behållare, blobbar, köer, tabeller och enheter."
documentationcenter: .net
services: storage
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: 33563639-2945-4567-9254-bc4a7e80698f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.openlocfilehash: d20a86688938704c24339aa9a1145786783fea5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="list-azure-storage-resources-in-c"></a><span data-ttu-id="c0e91-103">Visa en lista med Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="c0e91-103">List Azure Storage resources in C++</span></span>
<span data-ttu-id="c0e91-104">Lista över åtgärder är viktiga toomany utvecklingsscenarier med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c0e91-104">Listing operations are key toomany development scenarios with Azure Storage.</span></span> <span data-ttu-id="c0e91-105">Den här artikeln beskriver hur toomost effektivt räkna upp objekt i Azure Storage med hjälp av hello visar en lista över API: er som anges i hello Microsoft Azure Storage-klientbibliotek för C++.</span><span class="sxs-lookup"><span data-stu-id="c0e91-105">This article describes how toomost efficiently enumerate objects in Azure Storage using hello listing APIs provided in hello Microsoft Azure Storage Client Library for C++.</span></span>

> [!NOTE]
> <span data-ttu-id="c0e91-106">Den här handboken riktar sig till hello Azure Storage-klientbibliotek för C++ version 2.x som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="c0e91-106">This guide targets hello Azure Storage Client Library for C++ version 2.x, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

<span data-ttu-id="c0e91-107">hello Storage-klientbiblioteket tillhandahåller flera olika metoder toolist eller fråga objekt i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c0e91-107">hello Storage Client Library provides a variety of methods toolist or query objects in Azure Storage.</span></span> <span data-ttu-id="c0e91-108">Den här artikeln tar hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="c0e91-108">This article addresses hello following scenarios:</span></span>

* <span data-ttu-id="c0e91-109">Lista behållare i ett konto</span><span class="sxs-lookup"><span data-stu-id="c0e91-109">List containers in an account</span></span>
* <span data-ttu-id="c0e91-110">Lista över blobbar i en behållare eller virtuella blob-katalog</span><span class="sxs-lookup"><span data-stu-id="c0e91-110">List blobs in a container or virtual blob directory</span></span>
* <span data-ttu-id="c0e91-111">Lista över köer på ett konto</span><span class="sxs-lookup"><span data-stu-id="c0e91-111">List queues in an account</span></span>
* <span data-ttu-id="c0e91-112">Lista över tabeller i ett konto</span><span class="sxs-lookup"><span data-stu-id="c0e91-112">List tables in an account</span></span>
* <span data-ttu-id="c0e91-113">Fråga entiteter i en tabell</span><span class="sxs-lookup"><span data-stu-id="c0e91-113">Query entities in a table</span></span>

<span data-ttu-id="c0e91-114">Var och en av dessa metoder visas med olika överlagringar för olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="c0e91-114">Each of these methods is shown using different overloads for different scenarios.</span></span>

## <a name="asynchronous-versus-synchronous"></a><span data-ttu-id="c0e91-115">Asynkron och synkron</span><span class="sxs-lookup"><span data-stu-id="c0e91-115">Asynchronous versus synchronous</span></span>
<span data-ttu-id="c0e91-116">Eftersom hello Storage-klientbibliotek för C++ bygger på hello [C++ REST biblioteket](https://github.com/Microsoft/cpprestsdk), vi stöds asynkrona åtgärder med hjälp av [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span><span class="sxs-lookup"><span data-stu-id="c0e91-116">Because hello Storage Client Library for C++ is built on top of hello [C++ REST library](https://github.com/Microsoft/cpprestsdk), we inherently support asynchronous operations by using [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span></span> <span data-ttu-id="c0e91-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c0e91-117">For example:</span></span>

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

<span data-ttu-id="c0e91-118">Synkrona åtgärder omsluter hello motsvarande asynkrona åtgärder:</span><span class="sxs-lookup"><span data-stu-id="c0e91-118">Synchronous operations wrap hello corresponding asynchronous operations:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

<span data-ttu-id="c0e91-119">Om du arbetar med flera trådade program eller tjänster rekommenderar vi att du använder hello asynkrona API: er direkt i stället för att skapa en tråd toocall hello synkronisering API: er, vilket kan påverka prestandan avsevärt.</span><span class="sxs-lookup"><span data-stu-id="c0e91-119">If you are working with multiple threading applications or services, we recommend that you use hello async APIs directly instead of creating a thread toocall hello sync APIs, which significantly impacts your performance.</span></span>

## <a name="segmented-listing"></a><span data-ttu-id="c0e91-120">Segmenterade lista</span><span class="sxs-lookup"><span data-stu-id="c0e91-120">Segmented listing</span></span>
<span data-ttu-id="c0e91-121">hello skala molnlagring kräver segmenterade lista.</span><span class="sxs-lookup"><span data-stu-id="c0e91-121">hello scale of cloud storage requires segmented listing.</span></span> <span data-ttu-id="c0e91-122">Exempelvis kan du ha över en miljon blobbar i en Azure blob-behållaren eller över en miljard entiteter i en tabell i Azure.</span><span class="sxs-lookup"><span data-stu-id="c0e91-122">For example, you can have over a million blobs in an Azure blob container or over a billion entities in an Azure Table.</span></span> <span data-ttu-id="c0e91-123">Detta är inte teoretisk siffror, men verkliga kundens användning fall.</span><span class="sxs-lookup"><span data-stu-id="c0e91-123">These are not theoretical numbers, but real customer usage cases.</span></span>

<span data-ttu-id="c0e91-124">Det är därför opraktiska toolist alla objekt i ett enda svar.</span><span class="sxs-lookup"><span data-stu-id="c0e91-124">It is therefore impractical toolist all objects in a single response.</span></span> <span data-ttu-id="c0e91-125">I stället kan du visa objekt med hjälp av sidindelning.</span><span class="sxs-lookup"><span data-stu-id="c0e91-125">Instead, you can list objects using paging.</span></span> <span data-ttu-id="c0e91-126">Varje hello visar en lista över API: er har en *segmenterat* överlagra.</span><span class="sxs-lookup"><span data-stu-id="c0e91-126">Each of hello listing APIs has a *segmented* overload.</span></span>

<span data-ttu-id="c0e91-127">hello-svar för en segmenterade åtgärd innehåller:</span><span class="sxs-lookup"><span data-stu-id="c0e91-127">hello response for a segmented listing operation includes:</span></span>

* <span data-ttu-id="c0e91-128"><i>_segment</i>, som innehåller hello uppsättning resultat returneras för en enda anrop toohello visar en lista över API.</span><span class="sxs-lookup"><span data-stu-id="c0e91-128"><i>_segment</i>, which contains hello set of results returned for a single call toohello listing API.</span></span>
* <span data-ttu-id="c0e91-129">*continuation_token*, som har överförts toohello nästa anrop i ordning tooget hello nästa sida i resultatet.</span><span class="sxs-lookup"><span data-stu-id="c0e91-129">*continuation_token*, which is passed toohello next call in order tooget hello next page of results.</span></span> <span data-ttu-id="c0e91-130">När det finns inga fler resultat tooreturn, är hello fortsättningstoken null.</span><span class="sxs-lookup"><span data-stu-id="c0e91-130">When there are no more results tooreturn, hello continuation token is null.</span></span>

<span data-ttu-id="c0e91-131">Till exempel en typisk anropet toolist alla blobbar i en behållare kan se ut som hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="c0e91-131">For example, a typical call toolist all blobs in a container may look like hello following code snippet.</span></span> <span data-ttu-id="c0e91-132">hello koden finns i vår [exempel](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span><span class="sxs-lookup"><span data-stu-id="c0e91-132">hello code is available in our [samples](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span></span>

```cpp
// List blobs in hello blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_diretory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

<span data-ttu-id="c0e91-133">Observera att hello antalet resultat som returneras i en sida kan styras efter hello parametern *max_results* i hello överlagring för varje API, till exempel:</span><span class="sxs-lookup"><span data-stu-id="c0e91-133">Note that hello number of results returned in a page can be controlled by hello parameter *max_results* in hello overload of each API, for example:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

<span data-ttu-id="c0e91-134">Om du inte anger hello *max_results* parametern, hello standard maximivärdet för too5000 resultat returneras i en enda sida.</span><span class="sxs-lookup"><span data-stu-id="c0e91-134">If you do not specify hello *max_results* parameter, hello default maximum value of up too5000 results is returned in a single page.</span></span>

<span data-ttu-id="c0e91-135">Observera också att en fråga mot Azure Table storage kan returnera några poster eller färre poster än hello värdet hello *max_results* parameter som du angett, även om hello fortsättningstoken inte är tom.</span><span class="sxs-lookup"><span data-stu-id="c0e91-135">Also note that a query against Azure Table storage may return no records, or fewer records than hello value of hello *max_results* parameter that you specified, even if hello continuation token is not empty.</span></span> <span data-ttu-id="c0e91-136">En orsak kan vara att hello frågan inte kunde slutföras i fem sekunder.</span><span class="sxs-lookup"><span data-stu-id="c0e91-136">One reason might be that hello query could not complete in five seconds.</span></span> <span data-ttu-id="c0e91-137">Så länge hello fortsättningstoken inte är tom hello frågan ska fortsätta och din kod anta inte hello storlek på segment resultat.</span><span class="sxs-lookup"><span data-stu-id="c0e91-137">As long as hello continuation token is not empty, hello query should continue, and your code should not assume hello size of segment results.</span></span>

<span data-ttu-id="c0e91-138">hello bör kodning mönster för de flesta fall är segmenterat lista, som innehåller explicita fortskrider lista eller en fråga och hur hello-tjänsten svarar tooeach begäran.</span><span class="sxs-lookup"><span data-stu-id="c0e91-138">hello recommended coding pattern for most scenarios is segmented listing, which provides explicit progress of listing or querying, and how hello service responds tooeach request.</span></span> <span data-ttu-id="c0e91-139">Särskilt för C++-program eller tjänster kan på lägre nivå kontroll över hello lista pågår hjälpa kontrollen minne och prestanda.</span><span class="sxs-lookup"><span data-stu-id="c0e91-139">Particularly for C++ applications or services, lower-level control of hello listing progress may help control memory and performance.</span></span>

## <a name="greedy-listing"></a><span data-ttu-id="c0e91-140">Girig lista</span><span class="sxs-lookup"><span data-stu-id="c0e91-140">Greedy listing</span></span>
<span data-ttu-id="c0e91-141">Tidigare versioner av hello Storage-klientbibliotek för C++ (versioner 0.5.0 Förhandsgranska och tidigare) med icke-segmenterade listan API: er för tabeller och köer, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c0e91-141">Earlier versions of hello Storage Client Library for C++ (versions 0.5.0 Preview and earlier) included non-segmented listing APIs for tables and queues, as in hello following example:</span></span>

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

<span data-ttu-id="c0e91-142">Dessa metoder har implementerats som omslutningar av segmenterade API: er.</span><span class="sxs-lookup"><span data-stu-id="c0e91-142">These methods were implemented as wrappers of segmented APIs.</span></span> <span data-ttu-id="c0e91-143">För varje svar för segmenterade lista hello kod läggs hello resultat tooa vector och returneras alla resultat när hello fullständig behållare har genomsökts.</span><span class="sxs-lookup"><span data-stu-id="c0e91-143">For each response of segmented listing, hello code appended hello results tooa vector and returned all results after hello full containers were scanned.</span></span>

<span data-ttu-id="c0e91-144">Den här metoden kan fungera när hello storage-konto eller tabell som innehåller ett litet antal objekt.</span><span class="sxs-lookup"><span data-stu-id="c0e91-144">This approach might work when hello storage account or table contains a small number of objects.</span></span> <span data-ttu-id="c0e91-145">Men med en ökning i hello antalet objekt kan hello-minne som krävs öka utan begränsning, eftersom alla resultat kvar i minnet.</span><span class="sxs-lookup"><span data-stu-id="c0e91-145">However, with an increase in hello number of objects, hello memory required could increase without limit, because all results remained in memory.</span></span> <span data-ttu-id="c0e91-146">En åtgärd kan ta mycket lång tid under vilken hello anroparen har ingen information om förloppet.</span><span class="sxs-lookup"><span data-stu-id="c0e91-146">One listing operation can take a very long time, during which hello caller had no information about its progress.</span></span>

<span data-ttu-id="c0e91-147">Dessa girig visar en lista över API: er i hello SDK finns i C#, Java, eller inte hello JavaScript Node.js-miljö.</span><span class="sxs-lookup"><span data-stu-id="c0e91-147">These greedy listing APIs in hello SDK do not exist in C#, Java, or hello JavaScript Node.js environment.</span></span> <span data-ttu-id="c0e91-148">tooavoid hello problem med att använda API: erna girig, vi bort dem i version 0.6.0 förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="c0e91-148">tooavoid hello potential issues of using these greedy APIs, we removed them in version 0.6.0 Preview.</span></span>

<span data-ttu-id="c0e91-149">Om koden anropar dessa girig API: er:</span><span class="sxs-lookup"><span data-stu-id="c0e91-149">If your code is calling these greedy APIs:</span></span>

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

<span data-ttu-id="c0e91-150">Du bör ändra koden toouse hello segmenterat visar en lista över API: er:</span><span class="sxs-lookup"><span data-stu-id="c0e91-150">Then you should modify your code toouse hello segmented listing APIs:</span></span>

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

<span data-ttu-id="c0e91-151">Genom att ange hello *max_results* parametern för hello segment du balanserar mellan hello antal begäranden och minne användning toomeet prestandaöverväganden för ditt program.</span><span class="sxs-lookup"><span data-stu-id="c0e91-151">By specifying hello *max_results* parameter of hello segment, you can balance between hello numbers of requests and memory usage toomeet performance considerations for your application.</span></span>

<span data-ttu-id="c0e91-152">Dessutom om du använder med segmenterade lista API: er, men hello data lagras i en lokal samling i en ”girig” style, även rekommenderar vi starkt att du refactor din kod toohandle som lagrar data i en lokal insamling noggrant i större skala.</span><span class="sxs-lookup"><span data-stu-id="c0e91-152">Additionally, if you're using segmented listing APIs, but store hello data in a local collection in a "greedy" style, we also strongly recommend that you refactor your code toohandle storing data in a local collection carefully at scale.</span></span>

## <a name="lazy-listing"></a><span data-ttu-id="c0e91-153">Enkel lista</span><span class="sxs-lookup"><span data-stu-id="c0e91-153">Lazy listing</span></span>
<span data-ttu-id="c0e91-154">Girig lista aktiveras potentiella problem, är men praktiskt om hello behållaren finns för många objekt.</span><span class="sxs-lookup"><span data-stu-id="c0e91-154">Although greedy listing raised potential issues, it is convenient if there are not too many objects in hello container.</span></span>

<span data-ttu-id="c0e91-155">Om du använder också C# eller Oracle Java SDK: er, bör du känna till hello Enumerable-programmodellen, vilket ger en lazy--format som lista, där hello vid en viss förskjutning är bara hämta data om det behövs.</span><span class="sxs-lookup"><span data-stu-id="c0e91-155">If you're also using C# or Oracle Java SDKs, you should be familiar with hello Enumerable programming model, which offers a lazy-style listing, where hello data at a certain offset is only fetched if it is required.</span></span> <span data-ttu-id="c0e91-156">Hej iterator-baserade mallen innehåller också ett liknande sätt i C++.</span><span class="sxs-lookup"><span data-stu-id="c0e91-156">In C++, hello iterator-based template also provides a similar approach.</span></span>

<span data-ttu-id="c0e91-157">En typisk lazy lista API, med hjälp av **list_blobs** exempel ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="c0e91-157">A typical lazy listing API, using **list_blobs** as an example, looks like this:</span></span>

```cpp
list_blob_item_iterator list_blobs() const;
```

<span data-ttu-id="c0e91-158">En typisk kodstycke som använder hello lazy lista mönster kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="c0e91-158">A typical code snippet that uses hello lazy listing pattern might look like this:</span></span>

```cpp
// List blobs in hello blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

<span data-ttu-id="c0e91-159">Observera att lazy lista endast är tillgängliga i synkront läge.</span><span class="sxs-lookup"><span data-stu-id="c0e91-159">Note that lazy listing is only available in synchronous mode.</span></span>

<span data-ttu-id="c0e91-160">Jämfört med girig listan hämtar lazy lista data bara när det behövs.</span><span class="sxs-lookup"><span data-stu-id="c0e91-160">Compared with greedy listing, lazy listing fetches data only when necessary.</span></span> <span data-ttu-id="c0e91-161">Under hello omfattar hämtar den data från Azure Storage endast när hello nästa iterator flyttar till nästa segment.</span><span class="sxs-lookup"><span data-stu-id="c0e91-161">Under hello covers, it fetches data from Azure Storage only when hello next iterator moves into next segment.</span></span> <span data-ttu-id="c0e91-162">Därför minnesanvändning kontrolleras med en begränsad storlek och hello åtgärden går snabbt.</span><span class="sxs-lookup"><span data-stu-id="c0e91-162">Therefore, memory usage is controlled with a bounded size, and hello operation is fast.</span></span>

<span data-ttu-id="c0e91-163">Lazy lista API: er finns i hello Storage-klientbibliotek för C++ i version 2.2.0.</span><span class="sxs-lookup"><span data-stu-id="c0e91-163">Lazy listing APIs are included in hello Storage Client Library for C++ in version 2.2.0.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c0e91-164">Slutsats</span><span class="sxs-lookup"><span data-stu-id="c0e91-164">Conclusion</span></span>
<span data-ttu-id="c0e91-165">I den här artikeln beskrivs vi olika överlagringar för att lista API: er för olika objekt i hello Storage-klientbibliotek för C++.</span><span class="sxs-lookup"><span data-stu-id="c0e91-165">In this article, we discussed different overloads for listing APIs for various objects in hello Storage Client Library for C++ .</span></span> <span data-ttu-id="c0e91-166">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="c0e91-166">toosummarize:</span></span>

* <span data-ttu-id="c0e91-167">Async APIs rekommenderas under scenarier med flera trådmodell.</span><span class="sxs-lookup"><span data-stu-id="c0e91-167">Async APIs are strongly recommended under multiple threading scenarios.</span></span>
* <span data-ttu-id="c0e91-168">Segmenterade lista rekommenderas för de flesta scenarier.</span><span class="sxs-lookup"><span data-stu-id="c0e91-168">Segmented listing is recommended for most scenarios.</span></span>
* <span data-ttu-id="c0e91-169">Lazy lista anges i hello-biblioteket som en lämplig omslutning i synkron scenarier.</span><span class="sxs-lookup"><span data-stu-id="c0e91-169">Lazy listing is provided in hello library as a convenient wrapper in synchronous scenarios.</span></span>
* <span data-ttu-id="c0e91-170">Girig lista rekommenderas inte och har tagits bort från hello-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="c0e91-170">Greedy listing is not recommended and has been removed from hello library.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0e91-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0e91-171">Next steps</span></span>
<span data-ttu-id="c0e91-172">Mer information om Azure Storage och klientbibliotek för C++ finns hello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="c0e91-172">For more information about Azure Storage and Client Library for C++, see hello following resources.</span></span>

* [<span data-ttu-id="c0e91-173">Hur toouse Blob Storage från C++</span><span class="sxs-lookup"><span data-stu-id="c0e91-173">How toouse Blob Storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="c0e91-174">Hur toouse Table Storage från C++</span><span class="sxs-lookup"><span data-stu-id="c0e91-174">How toouse Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="c0e91-175">Hur toouse Queue Storage från C++</span><span class="sxs-lookup"><span data-stu-id="c0e91-175">How toouse Queue Storage from C++</span></span>](storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="c0e91-176">Azure Storage-klientbibliotek för C++-API-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="c0e91-176">Azure Storage Client Library for C++ API documentation.</span></span>](http://azure.github.io/azure-storage-cpp/)
* [<span data-ttu-id="c0e91-177">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="c0e91-177">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="c0e91-178">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="c0e91-178">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)

