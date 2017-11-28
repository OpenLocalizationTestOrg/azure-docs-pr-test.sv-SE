---
title: "Visa en lista med Azure Storage-resurser med Storage-klientbibliotek för C++ | Microsoft Docs"
description: "Lär dig hur du använder lista API: er i Microsoft Azure Storage-klientbibliotek för C++ att räkna upp behållare, blobbar, köer, tabeller och enheter."
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
ms.openlocfilehash: 4a4ac7938989f821c092379aff3085f5e440d1c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="list-azure-storage-resources-in-c"></a><span data-ttu-id="9464f-103">Visa en lista med Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="9464f-103">List Azure Storage resources in C++</span></span>
<span data-ttu-id="9464f-104">Lista över åtgärder är nyckeln till många utvecklingsscenarier för med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9464f-104">Listing operations are key to many development scenarios with Azure Storage.</span></span> <span data-ttu-id="9464f-105">Den här artikeln beskrivs mer effektivt att räkna upp objekt i Azure Storage med hjälp av listan API: er som finns i Microsoft Azure Storage-klientbibliotek för C++.</span><span class="sxs-lookup"><span data-stu-id="9464f-105">This article describes how to most efficiently enumerate objects in Azure Storage using the listing APIs provided in the Microsoft Azure Storage Client Library for C++.</span></span>

> [!NOTE]
> <span data-ttu-id="9464f-106">Den här handboken riktar sig till Azure Storage-klientbibliotek för C++ version 2.x som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="9464f-106">This guide targets the Azure Storage Client Library for C++ version 2.x, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

<span data-ttu-id="9464f-107">Storage-klientbiblioteket tillhandahåller en mängd olika metoder för att lista eller fråga objekt i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9464f-107">The Storage Client Library provides a variety of methods to list or query objects in Azure Storage.</span></span> <span data-ttu-id="9464f-108">Den här artikeln tar följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="9464f-108">This article addresses the following scenarios:</span></span>

* <span data-ttu-id="9464f-109">Lista behållare i ett konto</span><span class="sxs-lookup"><span data-stu-id="9464f-109">List containers in an account</span></span>
* <span data-ttu-id="9464f-110">Lista över blobbar i en behållare eller virtuella blob-katalog</span><span class="sxs-lookup"><span data-stu-id="9464f-110">List blobs in a container or virtual blob directory</span></span>
* <span data-ttu-id="9464f-111">Lista över köer på ett konto</span><span class="sxs-lookup"><span data-stu-id="9464f-111">List queues in an account</span></span>
* <span data-ttu-id="9464f-112">Lista över tabeller i ett konto</span><span class="sxs-lookup"><span data-stu-id="9464f-112">List tables in an account</span></span>
* <span data-ttu-id="9464f-113">Fråga entiteter i en tabell</span><span class="sxs-lookup"><span data-stu-id="9464f-113">Query entities in a table</span></span>

<span data-ttu-id="9464f-114">Var och en av dessa metoder visas med olika överlagringar för olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="9464f-114">Each of these methods is shown using different overloads for different scenarios.</span></span>

## <a name="asynchronous-versus-synchronous"></a><span data-ttu-id="9464f-115">Asynkron och synkron</span><span class="sxs-lookup"><span data-stu-id="9464f-115">Asynchronous versus synchronous</span></span>
<span data-ttu-id="9464f-116">Eftersom Storage-klientbibliotek för C++ är byggt ovanpå det [C++ REST biblioteket](https://github.com/Microsoft/cpprestsdk), vi stöds asynkrona åtgärder med hjälp av [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span><span class="sxs-lookup"><span data-stu-id="9464f-116">Because the Storage Client Library for C++ is built on top of the [C++ REST library](https://github.com/Microsoft/cpprestsdk), we inherently support asynchronous operations by using [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span></span> <span data-ttu-id="9464f-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="9464f-117">For example:</span></span>

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

<span data-ttu-id="9464f-118">Synkrona åtgärder omsluta de motsvarande asynkrona åtgärderna:</span><span class="sxs-lookup"><span data-stu-id="9464f-118">Synchronous operations wrap the corresponding asynchronous operations:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

<span data-ttu-id="9464f-119">Om du arbetar med flera trådade program eller tjänster rekommenderar vi att du använder async-API: er direkt i stället för att skapa en tråd för att anropa synkronisering API: er, vilket kan påverka prestandan avsevärt.</span><span class="sxs-lookup"><span data-stu-id="9464f-119">If you are working with multiple threading applications or services, we recommend that you use the async APIs directly instead of creating a thread to call the sync APIs, which significantly impacts your performance.</span></span>

## <a name="segmented-listing"></a><span data-ttu-id="9464f-120">Segmenterade lista</span><span class="sxs-lookup"><span data-stu-id="9464f-120">Segmented listing</span></span>
<span data-ttu-id="9464f-121">Skalan för molnlagring kräver segmenterade lista.</span><span class="sxs-lookup"><span data-stu-id="9464f-121">The scale of cloud storage requires segmented listing.</span></span> <span data-ttu-id="9464f-122">Exempelvis kan du ha över en miljon blobbar i en Azure blob-behållaren eller över en miljard entiteter i en tabell i Azure.</span><span class="sxs-lookup"><span data-stu-id="9464f-122">For example, you can have over a million blobs in an Azure blob container or over a billion entities in an Azure Table.</span></span> <span data-ttu-id="9464f-123">Detta är inte teoretisk siffror, men verkliga kundens användning fall.</span><span class="sxs-lookup"><span data-stu-id="9464f-123">These are not theoretical numbers, but real customer usage cases.</span></span>

<span data-ttu-id="9464f-124">Det är därför opraktiska att lista alla objekt i ett enda svar.</span><span class="sxs-lookup"><span data-stu-id="9464f-124">It is therefore impractical to list all objects in a single response.</span></span> <span data-ttu-id="9464f-125">I stället kan du visa objekt med hjälp av sidindelning.</span><span class="sxs-lookup"><span data-stu-id="9464f-125">Instead, you can list objects using paging.</span></span> <span data-ttu-id="9464f-126">Varje lista API: er har en *segmenterat* överlagra.</span><span class="sxs-lookup"><span data-stu-id="9464f-126">Each of the listing APIs has a *segmented* overload.</span></span>

<span data-ttu-id="9464f-127">Svaret för segmenterade lista innehåller:</span><span class="sxs-lookup"><span data-stu-id="9464f-127">The response for a segmented listing operation includes:</span></span>

* <span data-ttu-id="9464f-128"><i>_segment</i>, som innehåller de resultat som returnerats för ett enda anrop till API: et lista.</span><span class="sxs-lookup"><span data-stu-id="9464f-128"><i>_segment</i>, which contains the set of results returned for a single call to the listing API.</span></span>
* <span data-ttu-id="9464f-129">*continuation_token*, som skickas till nästa anrop för att få nästa sida i resultaten.</span><span class="sxs-lookup"><span data-stu-id="9464f-129">*continuation_token*, which is passed to the next call in order to get the next page of results.</span></span> <span data-ttu-id="9464f-130">När det finns inga fler resultat ska returneras, är fortsättningstoken null.</span><span class="sxs-lookup"><span data-stu-id="9464f-130">When there are no more results to return, the continuation token is null.</span></span>

<span data-ttu-id="9464f-131">Ett typiskt anrop för att lista alla blobbar i en behållare kan till exempel se ut som följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="9464f-131">For example, a typical call to list all blobs in a container may look like the following code snippet.</span></span> <span data-ttu-id="9464f-132">Koden är tillgängliga i vår [exempel](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span><span class="sxs-lookup"><span data-stu-id="9464f-132">The code is available in our [samples](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span></span>

```cpp
// List blobs in the blob container
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

<span data-ttu-id="9464f-133">Observera att antalet resultat som returneras i en sida kan styras av parametern *max_results* i överlagringen för varje API, till exempel:</span><span class="sxs-lookup"><span data-stu-id="9464f-133">Note that the number of results returned in a page can be controlled by the parameter *max_results* in the overload of each API, for example:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

<span data-ttu-id="9464f-134">Om du inte anger den *max_results* parametern, som är standard maximivärdet för upp till 5 000 resultat returneras i en enda sida.</span><span class="sxs-lookup"><span data-stu-id="9464f-134">If you do not specify the *max_results* parameter, the default maximum value of up to 5000 results is returned in a single page.</span></span>

<span data-ttu-id="9464f-135">Observera också att en fråga mot Azure Table storage kan returnera några poster eller färre poster än värdet för den *max_results* parameter som du angett, även om fortsättningstoken inte är tom.</span><span class="sxs-lookup"><span data-stu-id="9464f-135">Also note that a query against Azure Table storage may return no records, or fewer records than the value of the *max_results* parameter that you specified, even if the continuation token is not empty.</span></span> <span data-ttu-id="9464f-136">En orsak kan vara att frågan inte kunde slutföras i fem sekunder.</span><span class="sxs-lookup"><span data-stu-id="9464f-136">One reason might be that the query could not complete in five seconds.</span></span> <span data-ttu-id="9464f-137">Så länge fortsättningstoken inte är tom frågan ska fortsätta och din kod anta inte att storleken på segment resultat.</span><span class="sxs-lookup"><span data-stu-id="9464f-137">As long as the continuation token is not empty, the query should continue, and your code should not assume the size of segment results.</span></span>

<span data-ttu-id="9464f-138">Det rekommenderade kodning mönstret för de flesta fall är segmenterat lista, som innehåller explicita fortskrider lista eller fråga och hur tjänsten svarar på varje begäran.</span><span class="sxs-lookup"><span data-stu-id="9464f-138">The recommended coding pattern for most scenarios is segmented listing, which provides explicit progress of listing or querying, and how the service responds to each request.</span></span> <span data-ttu-id="9464f-139">Särskilt för C++-program eller tjänster kan på lägre nivå kontroll över lista förloppet hjälpa kontrollen minne och prestanda.</span><span class="sxs-lookup"><span data-stu-id="9464f-139">Particularly for C++ applications or services, lower-level control of the listing progress may help control memory and performance.</span></span>

## <a name="greedy-listing"></a><span data-ttu-id="9464f-140">Girig lista</span><span class="sxs-lookup"><span data-stu-id="9464f-140">Greedy listing</span></span>
<span data-ttu-id="9464f-141">Tidigare versioner av Storage-klientbibliotek för C++ (versioner 0.5.0 Förhandsgranska och tidigare) med icke-segmenterade listan API: er för tabeller och köer, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="9464f-141">Earlier versions of the Storage Client Library for C++ (versions 0.5.0 Preview and earlier) included non-segmented listing APIs for tables and queues, as in the following example:</span></span>

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

<span data-ttu-id="9464f-142">Dessa metoder har implementerats som omslutningar av segmenterade API: er.</span><span class="sxs-lookup"><span data-stu-id="9464f-142">These methods were implemented as wrappers of segmented APIs.</span></span> <span data-ttu-id="9464f-143">För varje svar för segmenterade lista koden läggs resultaten till en vector och returneras alla resultat när fullständig behållarna har genomsökts.</span><span class="sxs-lookup"><span data-stu-id="9464f-143">For each response of segmented listing, the code appended the results to a vector and returned all results after the full containers were scanned.</span></span>

<span data-ttu-id="9464f-144">Den här metoden kan fungera när lagringskontot eller tabell som innehåller ett litet antal objekt.</span><span class="sxs-lookup"><span data-stu-id="9464f-144">This approach might work when the storage account or table contains a small number of objects.</span></span> <span data-ttu-id="9464f-145">Men med en ökning av antalet objekt kan det minne som krävs öka utan begränsning, eftersom alla resultat kvar i minnet.</span><span class="sxs-lookup"><span data-stu-id="9464f-145">However, with an increase in the number of objects, the memory required could increase without limit, because all results remained in memory.</span></span> <span data-ttu-id="9464f-146">En åtgärd kan ta mycket lång tid under vilken anroparen har ingen information om förloppet.</span><span class="sxs-lookup"><span data-stu-id="9464f-146">One listing operation can take a very long time, during which the caller had no information about its progress.</span></span>

<span data-ttu-id="9464f-147">Dessa girig lista API: er i SDK finns inte i C#, Java och JavaScript Node.js-miljö.</span><span class="sxs-lookup"><span data-stu-id="9464f-147">These greedy listing APIs in the SDK do not exist in C#, Java, or the JavaScript Node.js environment.</span></span> <span data-ttu-id="9464f-148">Om du vill undvika potentiella problem med att använda API: erna girig vi bort dem i version 0.6.0 förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="9464f-148">To avoid the potential issues of using these greedy APIs, we removed them in version 0.6.0 Preview.</span></span>

<span data-ttu-id="9464f-149">Om koden anropar dessa girig API: er:</span><span class="sxs-lookup"><span data-stu-id="9464f-149">If your code is calling these greedy APIs:</span></span>

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

<span data-ttu-id="9464f-150">Du bör ändra koden för att använda segmenterade lista API: er:</span><span class="sxs-lookup"><span data-stu-id="9464f-150">Then you should modify your code to use the segmented listing APIs:</span></span>

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

<span data-ttu-id="9464f-151">Genom att ange den *max_results* parametern för segmentet du balanserar mellan antal begäranden och minnesanvändning att uppfylla prestandaöverväganden för ditt program.</span><span class="sxs-lookup"><span data-stu-id="9464f-151">By specifying the *max_results* parameter of the segment, you can balance between the numbers of requests and memory usage to meet performance considerations for your application.</span></span>

<span data-ttu-id="9464f-152">Dessutom om du använder segmenterade lista API: er och lagra data i en lokal samling i en ”girig” style också rekommenderar vi starkt att du refactor din kod för att hantera lagring av data i en lokal samling noggrant i större skala.</span><span class="sxs-lookup"><span data-stu-id="9464f-152">Additionally, if you're using segmented listing APIs, but store the data in a local collection in a "greedy" style, we also strongly recommend that you refactor your code to handle storing data in a local collection carefully at scale.</span></span>

## <a name="lazy-listing"></a><span data-ttu-id="9464f-153">Enkel lista</span><span class="sxs-lookup"><span data-stu-id="9464f-153">Lazy listing</span></span>
<span data-ttu-id="9464f-154">Girig lista aktiveras potentiella problem, är men praktiskt om det inte är för många objekt i behållaren.</span><span class="sxs-lookup"><span data-stu-id="9464f-154">Although greedy listing raised potential issues, it is convenient if there are not too many objects in the container.</span></span>

<span data-ttu-id="9464f-155">Om du använder också C# eller Oracle Java SDK: er, bör du känna till Enumerable programmeringsmiljö, vilket ger en lazy--format som lista, där data vid en viss förskjutning hämtas endast om det behövs.</span><span class="sxs-lookup"><span data-stu-id="9464f-155">If you're also using C# or Oracle Java SDKs, you should be familiar with the Enumerable programming model, which offers a lazy-style listing, where the data at a certain offset is only fetched if it is required.</span></span> <span data-ttu-id="9464f-156">Iterator-baserade mallen innehåller också ett liknande sätt i C++.</span><span class="sxs-lookup"><span data-stu-id="9464f-156">In C++, the iterator-based template also provides a similar approach.</span></span>

<span data-ttu-id="9464f-157">En typisk lazy lista API, med hjälp av **list_blobs** exempel ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="9464f-157">A typical lazy listing API, using **list_blobs** as an example, looks like this:</span></span>

```cpp
list_blob_item_iterator list_blobs() const;
```

<span data-ttu-id="9464f-158">En typisk kodstycke som använder lazy lista mönster kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="9464f-158">A typical code snippet that uses the lazy listing pattern might look like this:</span></span>

```cpp
// List blobs in the blob container
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

<span data-ttu-id="9464f-159">Observera att lazy lista endast är tillgängliga i synkront läge.</span><span class="sxs-lookup"><span data-stu-id="9464f-159">Note that lazy listing is only available in synchronous mode.</span></span>

<span data-ttu-id="9464f-160">Jämfört med girig listan hämtar lazy lista data bara när det behövs.</span><span class="sxs-lookup"><span data-stu-id="9464f-160">Compared with greedy listing, lazy listing fetches data only when necessary.</span></span> <span data-ttu-id="9464f-161">Under försättsbladen hämtar den data från Azure Storage endast när nästa iterator flyttar till nästa segment.</span><span class="sxs-lookup"><span data-stu-id="9464f-161">Under the covers, it fetches data from Azure Storage only when the next iterator moves into next segment.</span></span> <span data-ttu-id="9464f-162">Därför minnesanvändning kontrolleras med en begränsad storlek och åtgärden går snabbt.</span><span class="sxs-lookup"><span data-stu-id="9464f-162">Therefore, memory usage is controlled with a bounded size, and the operation is fast.</span></span>

<span data-ttu-id="9464f-163">Lazy lista API: er finns i Storage-klientbibliotek för C++ i version 2.2.0.</span><span class="sxs-lookup"><span data-stu-id="9464f-163">Lazy listing APIs are included in the Storage Client Library for C++ in version 2.2.0.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9464f-164">Slutsats</span><span class="sxs-lookup"><span data-stu-id="9464f-164">Conclusion</span></span>
<span data-ttu-id="9464f-165">I den här artikeln beskrivs vi olika överlagringar för att lista API: er för olika objekt i Storage-klientbibliotek för C++.</span><span class="sxs-lookup"><span data-stu-id="9464f-165">In this article, we discussed different overloads for listing APIs for various objects in the Storage Client Library for C++ .</span></span> <span data-ttu-id="9464f-166">Sammanfattningsvis:</span><span class="sxs-lookup"><span data-stu-id="9464f-166">To summarize:</span></span>

* <span data-ttu-id="9464f-167">Async APIs rekommenderas under scenarier med flera trådmodell.</span><span class="sxs-lookup"><span data-stu-id="9464f-167">Async APIs are strongly recommended under multiple threading scenarios.</span></span>
* <span data-ttu-id="9464f-168">Segmenterade lista rekommenderas för de flesta scenarier.</span><span class="sxs-lookup"><span data-stu-id="9464f-168">Segmented listing is recommended for most scenarios.</span></span>
* <span data-ttu-id="9464f-169">Lazy lista anges i biblioteket som en lämplig omslutning i synkron scenarier.</span><span class="sxs-lookup"><span data-stu-id="9464f-169">Lazy listing is provided in the library as a convenient wrapper in synchronous scenarios.</span></span>
* <span data-ttu-id="9464f-170">Girig lista rekommenderas inte och har tagits bort från biblioteket.</span><span class="sxs-lookup"><span data-stu-id="9464f-170">Greedy listing is not recommended and has been removed from the library.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9464f-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9464f-171">Next steps</span></span>
<span data-ttu-id="9464f-172">Mer information om Azure Storage och klientbibliotek för C++ finns i följande resurser.</span><span class="sxs-lookup"><span data-stu-id="9464f-172">For more information about Azure Storage and Client Library for C++, see the following resources.</span></span>

* [<span data-ttu-id="9464f-173">Hur du använder Blob Storage från C++</span><span class="sxs-lookup"><span data-stu-id="9464f-173">How to use Blob Storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="9464f-174">Använda Table Storage från C++</span><span class="sxs-lookup"><span data-stu-id="9464f-174">How to use Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="9464f-175">Använda Queue Storage från C++</span><span class="sxs-lookup"><span data-stu-id="9464f-175">How to use Queue Storage from C++</span></span>](storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="9464f-176">Azure Storage-klientbibliotek för C++-API-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="9464f-176">Azure Storage Client Library for C++ API documentation.</span></span>](http://azure.github.io/azure-storage-cpp/)
* [<span data-ttu-id="9464f-177">Azure Storage Teamblogg</span><span class="sxs-lookup"><span data-stu-id="9464f-177">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="9464f-178">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="9464f-178">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)

