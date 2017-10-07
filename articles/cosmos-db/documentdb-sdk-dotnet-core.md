---
title: aaaAzure Cosmos DB .NET Core API SDK & resurser | Microsoft Docs
description: "Läs mer om hello .NET Core-API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1269cafe0ea1caaa871404d507b12632dbb3ed82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a><span data-ttu-id="ad9d9-103">Azure Cosmos DB .NET Core SDK: Viktig information och resurser</span><span class="sxs-lookup"><span data-stu-id="ad9d9-103">Azure Cosmos DB .NET Core SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad9d9-104">.NET</span><span class="sxs-lookup"><span data-stu-id="ad9d9-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="ad9d9-105">.NET ändra Feed</span><span class="sxs-lookup"><span data-stu-id="ad9d9-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="ad9d9-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad9d9-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="ad9d9-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="ad9d9-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="ad9d9-108">Java</span><span class="sxs-lookup"><span data-stu-id="ad9d9-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="ad9d9-109">Python</span><span class="sxs-lookup"><span data-stu-id="ad9d9-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="ad9d9-110">REST</span><span class="sxs-lookup"><span data-stu-id="ad9d9-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="ad9d9-111">REST-resursprovider</span><span class="sxs-lookup"><span data-stu-id="ad9d9-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="ad9d9-112">SQL</span><span class="sxs-lookup"><span data-stu-id="ad9d9-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="ad9d9-113">**SDK-hämtningen**</span><span class="sxs-lookup"><span data-stu-id="ad9d9-113">**SDK download**</span></span></td><td>[<span data-ttu-id="ad9d9-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="ad9d9-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td><span data-ttu-id="ad9d9-115">**API-dokumentationen**</span><span class="sxs-lookup"><span data-stu-id="ad9d9-115">**API documentation**</span></span></td><td>[<span data-ttu-id="ad9d9-116">.NET API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="ad9d9-116">.NET API reference documentation</span></span>](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="ad9d9-117">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="ad9d9-117">**Samples**</span></span></td><td>[<span data-ttu-id="ad9d9-118">.NET-kodexempel</span><span class="sxs-lookup"><span data-stu-id="ad9d9-118">.NET code samples</span></span>](documentdb-dotnet-samples.md)</td></tr>

<tr><td><span data-ttu-id="ad9d9-119">**Kom igång**</span><span class="sxs-lookup"><span data-stu-id="ad9d9-119">**Get started**</span></span></td><td>[<span data-ttu-id="ad9d9-120">Kom igång med hello Azure Cosmos DB .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="ad9d9-120">Get started with hello Azure Cosmos DB .NET Core SDK</span></span>](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td><span data-ttu-id="ad9d9-121">**Självstudier för Web app**</span><span class="sxs-lookup"><span data-stu-id="ad9d9-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="ad9d9-122">Utveckling av webbappar med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ad9d9-122">Web application development with Azure Cosmos DB</span></span>](documentdb-dotnet-application.md)</td></tr>

<tr><td><span data-ttu-id="ad9d9-123">**Aktuella framework som stöds**</span><span class="sxs-lookup"><span data-stu-id="ad9d9-123">**Current supported framework**</span></span></td><td>[<span data-ttu-id="ad9d9-124">.NET standard 1.6 och .NET Standard 1.5</span><span class="sxs-lookup"><span data-stu-id="ad9d9-124">.NET Standard 1.6 and .NET Standard 1.5</span></span>](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="ad9d9-125">Viktig information</span><span class="sxs-lookup"><span data-stu-id="ad9d9-125">Release Notes</span></span>

<span data-ttu-id="ad9d9-126">hello Azure Cosmos DB .NET Core SDK har funktionsparitet med hello senaste versionen av hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-126">hello Azure Cosmos DB .NET Core SDK has feature parity with hello latest version of hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="ad9d9-127">hello Azure Cosmos DB .NET Core SDK är inte kompatibel med den universella Windowsplattformen (UWP) appar ännu.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-127">hello Azure Cosmos DB .NET Core SDK is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="ad9d9-128">Om du är intresserad av hello .NET Core SDK som stöder UWP-appar kan skicka e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-128">If you are interested in hello .NET Core SDK that does support UWP apps, send email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

### <a name="a-name150150"></a><span data-ttu-id="ad9d9-129"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-129"><a name="1.5.0"/>1.5.0</span></span> 

* <span data-ttu-id="ad9d9-130">Stöd för PartitionKeyRangeId som en FeedOption scope frågan resultat tooa specifika viktiga partitionsintervallvärdet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-130">Added support for PartitionKeyRangeId as a FeedOption for scoping query results tooa specific partition key range value.</span></span> 
* <span data-ttu-id="ad9d9-131">Stöd har lagts till för StartTime som en ChangeFeedOption toostart söker efter hello ändringar efter den tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-131">Added support for StartTime as a ChangeFeedOption toostart looking for hello changes after that time.</span></span> 

### <a name="a-name141141"></a><span data-ttu-id="ad9d9-132"><a name="1.4.1"/>1.4.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-132"><a name="1.4.1"/>1.4.1</span></span>

*   <span data-ttu-id="ad9d9-133">Ett problem har åtgärdats i hello JsonSerializable klass som kan orsaka en dataspillsundantag.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-133">Fixed an issue in hello JsonSerializable class that may cause a stack overflow exception.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="ad9d9-134"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-134"><a name="1.4.0"/>1.4.0</span></span>

*   <span data-ttu-id="ad9d9-135">Tillagt stöd för att ange anpassade JsonSerializerSettings när en [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instans.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-135">Added support for specifying custom JsonSerializerSettings while instantiating a [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.</span></span>

### <a name="a-name132132"></a><span data-ttu-id="ad9d9-136"><a name="1.3.2"/>1.3.2</span><span class="sxs-lookup"><span data-stu-id="ad9d9-136"><a name="1.3.2"/>1.3.2</span></span>

*   <span data-ttu-id="ad9d9-137">Stöder .NET Standard 1.5 som en hello mål ramverk.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-137">Supporting .NET Standard 1.5 as one of hello target frameworks.</span></span>

### <a name="a-name131131"></a><span data-ttu-id="ad9d9-138"><a name="1.3.1"/>1.3.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-138"><a name="1.3.1"/>1.3.1</span></span>

*   <span data-ttu-id="ad9d9-139">Ett problem som påverkade x64 har åtgärdats datorer som inte stöder SSE4 instruktion och utlösa SEHException när du kör Azure DB som Cosmos-frågor.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-139">Fixed an issue that affected x64 machines that don’t support SSE4 instruction and throw SEHException when running Azure Cosmos DB queries.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="ad9d9-140"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-140"><a name="1.3.0"/>1.3.0</span></span>

*   <span data-ttu-id="ad9d9-141">Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-141">Added support for a new consistency level called ConsistentPrefix.</span></span>
*   <span data-ttu-id="ad9d9-142">Stöd har lagts till för frågan måtten för enskilda partitioner.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-142">Added support for query metrics for individual partitions.</span></span>
*   <span data-ttu-id="ad9d9-143">Stöd har lagts till för att begränsa hello storleken på hello fortsättningstoken för frågor.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-143">Added support for limiting hello size of hello continuation token for queries.</span></span>
*   <span data-ttu-id="ad9d9-144">Tillagt stöd för mer detaljerad spårning för misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-144">Added support for more detailed tracing for failed requests.</span></span>
*   <span data-ttu-id="ad9d9-145">Gjorts prestanda förbättras i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-145">Made some performance improvements in hello SDK.</span></span>

### <a name="a-name122122"></a><span data-ttu-id="ad9d9-146"><a name="1.2.2"/>1.2.2</span><span class="sxs-lookup"><span data-stu-id="ad9d9-146"><a name="1.2.2"/>1.2.2</span></span>

* <span data-ttu-id="ad9d9-147">Ett problem som ignoreras hello PartitionKey värdet som angetts i FeedOptions för sammanställd frågor har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-147">Fixed an issue that ignored hello PartitionKey value provided in FeedOptions for aggregate queries.</span></span>
* <span data-ttu-id="ad9d9-148">Ett problem har åtgärdats i transparent hantering av partition hantering under halva rör sig över flera partitioner Order By Frågekörningen.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-148">Fixed an issue in transparent handling of partition management during mid-flight cross-partition Order By query execution.</span></span>

### <a name="a-name121121"></a><span data-ttu-id="ad9d9-149"><a name="1.2.1"/>1.2.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-149"><a name="1.2.1"/>1.2.1</span></span>

* <span data-ttu-id="ad9d9-150">Ett problem som orsakade deadlocks i vissa hello asynkrona API: er när den används i ASP.NET-kontexten har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-150">Fixed an issue which caused deadlocks in some of hello async APIs when used inside ASP.NET context.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="ad9d9-151"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-151"><a name="1.2.0"/>1.2.0</span></span>

* <span data-ttu-id="ad9d9-152">Åtgärdar toomake SDK mer flexibel tooautomatic växling vid fel under vissa förhållanden.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-152">Fixes toomake SDK more resilient tooautomatic failover under certain conditions.</span></span>

### <a name="a-name112112"></a><span data-ttu-id="ad9d9-153"><a name="1.1.2"/>1.1.2</span><span class="sxs-lookup"><span data-stu-id="ad9d9-153"><a name="1.1.2"/>1.1.2</span></span>

* <span data-ttu-id="ad9d9-154">Korrigering för ett problem som ibland medför en WebException: hello fjärrnamnet kunde inte matchas.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-154">Fix for an issue that occasionally causes a WebException: hello remote name could not be resolved.</span></span>
* <span data-ttu-id="ad9d9-155">Tillagda hello stöd för direkt läsa ett skrivet dokument genom att lägga till nya överlagringar tooReadDocumentAsync API.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-155">Added hello support for directly reading a typed document by adding new overloads tooReadDocumentAsync API.</span></span>

### <a name="a-name111111"></a><span data-ttu-id="ad9d9-156"><a name="1.1.1"/>1.1.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-156"><a name="1.1.1"/>1.1.1</span></span>

* <span data-ttu-id="ad9d9-157">LINQ-stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) lagts till.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-157">Added LINQ support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="ad9d9-158">Åtgärda för en minnesläcka för hello ConnectionPolicy objektet beror på hello användning av händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-158">Fix for a memory leak issue for hello ConnectionPolicy object caused by hello use of event handler.</span></span>
* <span data-ttu-id="ad9d9-159">Korrigering för ett problem där UpsertAttachmentAsync inte fungerade när ETag har använts.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-159">Fix for an issue wherein UpsertAttachmentAsync was not working when ETag was used.</span></span>
* <span data-ttu-id="ad9d9-160">Korrigering för ett problem där mellan partition ordning av frågan fortsättning inte arbetar vid sortering i string-fält.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-160">Fix for an issue wherein cross partition order-by query continuation was not working when sorting on string field.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="ad9d9-161"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-161"><a name="1.1.0"/>1.1.0</span></span>

* <span data-ttu-id="ad9d9-162">Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-162">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="ad9d9-163">Se [aggregering support](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-163">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="ad9d9-164">Sänks minsta dataflöde på partitionerade samlingar från 10,100 RU/s too2500 RU/s.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-164">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="ad9d9-165"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-165"><a name="1.0.0"/>1.0.0</span></span>

<span data-ttu-id="ad9d9-166">hello Azure Cosmos DB .NET Core SDK kan du toobuild snabb, plattformsoberoende [ASP.NET Core](https://www.asp.net/core) och [.NET Core](https://www.microsoft.com/net/core#windows) toorun för appar i Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-166">hello Azure Cosmos DB .NET Core SDK enables you toobuild fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps toorun on Windows, Mac, and Linux.</span></span> <span data-ttu-id="ad9d9-167">hello senaste versionen av hello Azure Cosmos DB .NET Core SDK är fullständigt [Xamarin](https://www.xamarin.com) kompatibel och använda toobuild program som är riktade till iOS, Android och Mono (Linux).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-167">hello latest release of hello Azure Cosmos DB .NET Core SDK is fully [Xamarin](https://www.xamarin.com) compatible and be used toobuild applications that target iOS, Android, and Mono (Linux).</span></span>  

### <a name="a-name010-preview010-preview"></a><span data-ttu-id="ad9d9-168"><a name="0.1.0-preview"/>0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="ad9d9-168"><a name="0.1.0-preview"/>0.1.0-preview</span></span>

<span data-ttu-id="ad9d9-169">hello Azure Cosmos DB .NET Core Preview SDK kan du toobuild snabb, plattformsoberoende [ASP.NET Core](https://www.asp.net/core) och [.NET Core](https://www.microsoft.com/net/core#windows) toorun för appar i Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-169">hello Azure Cosmos DB .NET Core Preview SDK enables you toobuild fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps toorun on Windows, Mac, and Linux.</span></span>

<span data-ttu-id="ad9d9-170">hello Azure Cosmos DB .NET Core Preview SDK har funktionsparitet med hello senaste versionen av hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) och stöder hello följande:</span><span class="sxs-lookup"><span data-stu-id="ad9d9-170">hello Azure Cosmos DB .NET Core Preview SDK has feature parity with hello latest version of hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) and supports hello following:</span></span>
* <span data-ttu-id="ad9d9-171">Alla [anslutningsläge](performance-tips.md#networking): Gateway läge, direkt TCP och direkt HTTPs.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-171">All [connection modes](performance-tips.md#networking): Gateway mode, Direct TCP, and Direct HTTPs.</span></span> 
* <span data-ttu-id="ad9d9-172">Alla [konsekvensnivåer](consistency-levels.md): stark, Session, begränsat föråldrad och Eventual.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-172">All [consistency levels](consistency-levels.md): Strong, Session, Bounded Staleness, and Eventual.</span></span>
* <span data-ttu-id="ad9d9-173">[Partitionerade samlingar](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-173">[Partitioned collections](partition-data.md).</span></span> 
* <span data-ttu-id="ad9d9-174">[Flera regioner databasen konton och geo-replikering](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-174">[Multi-region database accounts and geo-replication](distribute-data-globally.md).</span></span>

<span data-ttu-id="ad9d9-175">Om du har frågor relaterade toothis SDK, efter för[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), eller ett problem i hello [github-lagringsplatsen](https://github.com/Azure/azure-documentdb-dotnet/issues).</span><span class="sxs-lookup"><span data-stu-id="ad9d9-175">If you have questions related toothis SDK, post too[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), or file an issue in hello [github repository](https://github.com/Azure/azure-documentdb-dotnet/issues).</span></span> 

## <a name="release--retirement-dates"></a><span data-ttu-id="ad9d9-176">Versionen & pensionering datum</span><span class="sxs-lookup"><span data-stu-id="ad9d9-176">Release & Retirement Dates</span></span>

| <span data-ttu-id="ad9d9-177">Version</span><span class="sxs-lookup"><span data-stu-id="ad9d9-177">Version</span></span> | <span data-ttu-id="ad9d9-178">Utgivningsdatum</span><span class="sxs-lookup"><span data-stu-id="ad9d9-178">Release Date</span></span> | <span data-ttu-id="ad9d9-179">Datumet för tillbakadragandet</span><span class="sxs-lookup"><span data-stu-id="ad9d9-179">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="ad9d9-180">1.5.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-180">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="ad9d9-181">10 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-181">August 10, 2017</span></span> |--- | 
| [<span data-ttu-id="ad9d9-182">1.4.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-182">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="ad9d9-183">07 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-183">August 07, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-184">1.4.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-184">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="ad9d9-185">02 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-185">August 02, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-186">1.3.2</span><span class="sxs-lookup"><span data-stu-id="ad9d9-186">1.3.2</span></span>](#1.3.2) |<span data-ttu-id="ad9d9-187">12 juni 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-187">June 12, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-188">1.3.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-188">1.3.1</span></span>](#1.3.1) |<span data-ttu-id="ad9d9-189">23 maj 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-189">May 23, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-190">1.3.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-190">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="ad9d9-191">10 maj 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-191">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-192">1.2.2</span><span class="sxs-lookup"><span data-stu-id="ad9d9-192">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="ad9d9-193">19 april 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-193">April 19, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-194">1.2.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-194">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="ad9d9-195">Den 29 mars 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-195">March 29, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-196">1.2.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-196">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="ad9d9-197">25 mars 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-197">March 25, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-198">1.1.2</span><span class="sxs-lookup"><span data-stu-id="ad9d9-198">1.1.2</span></span>](#1.1.2) |<span data-ttu-id="ad9d9-199">20 mars 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-199">March 20, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-200">1.1.1</span><span class="sxs-lookup"><span data-stu-id="ad9d9-200">1.1.1</span></span>](#1.1.1) |<span data-ttu-id="ad9d9-201">14 mars 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-201">March 14, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-202">1.1.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-202">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="ad9d9-203">16 februari 2017</span><span class="sxs-lookup"><span data-stu-id="ad9d9-203">February 16, 2017</span></span> |--- |
| [<span data-ttu-id="ad9d9-204">1.0.0</span><span class="sxs-lookup"><span data-stu-id="ad9d9-204">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="ad9d9-205">21 december 2016</span><span class="sxs-lookup"><span data-stu-id="ad9d9-205">December 21, 2016</span></span> |--- |
| [<span data-ttu-id="ad9d9-206">0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="ad9d9-206">0.1.0-preview</span></span>](#0.1.0-preview) |<span data-ttu-id="ad9d9-207">15 november 2016</span><span class="sxs-lookup"><span data-stu-id="ad9d9-207">November 15, 2016</span></span> |<span data-ttu-id="ad9d9-208">Den 31 december 2016</span><span class="sxs-lookup"><span data-stu-id="ad9d9-208">December 31, 2016</span></span> |

## <a name="see-also"></a><span data-ttu-id="ad9d9-209">Se även</span><span class="sxs-lookup"><span data-stu-id="ad9d9-209">See Also</span></span>
<span data-ttu-id="ad9d9-210">toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.</span><span class="sxs-lookup"><span data-stu-id="ad9d9-210">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

