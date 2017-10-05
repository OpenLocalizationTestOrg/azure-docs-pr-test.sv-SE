---
title: "Azure DocumentDB .NET ändra Feed Processor SDK & resurser | Microsoft Docs"
description: "Läs mer om ändringen Feed Processor-API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av .NET DocumentDB ändra Feed Processor SDK."
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kirillg
editor: mimig1
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/14/2017
ms.author: maquaran
ms.openlocfilehash: 40c796bc5af1220c46950a6fac062ffdd243e59f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a><span data-ttu-id="ae3ea-103">DocumentDB .NET ändra Feed Processor SDK: Hämta och viktig information</span><span class="sxs-lookup"><span data-stu-id="ae3ea-103">DocumentDB .NET Change Feed Processor SDK: Download and release notes</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ae3ea-104">.NET</span><span class="sxs-lookup"><span data-stu-id="ae3ea-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="ae3ea-105">.NET ändra Feed</span><span class="sxs-lookup"><span data-stu-id="ae3ea-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="ae3ea-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae3ea-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="ae3ea-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="ae3ea-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="ae3ea-108">Java</span><span class="sxs-lookup"><span data-stu-id="ae3ea-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="ae3ea-109">Python</span><span class="sxs-lookup"><span data-stu-id="ae3ea-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="ae3ea-110">REST</span><span class="sxs-lookup"><span data-stu-id="ae3ea-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="ae3ea-111">REST-resursprovider</span><span class="sxs-lookup"><span data-stu-id="ae3ea-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="ae3ea-112">SQL</span><span class="sxs-lookup"><span data-stu-id="ae3ea-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="ae3ea-113">**SDK-hämtningen**</span><span class="sxs-lookup"><span data-stu-id="ae3ea-113">**SDK download**</span></span></td><td>[<span data-ttu-id="ae3ea-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="ae3ea-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)</td></tr>

<tr><td><span data-ttu-id="ae3ea-115">**API-dokumentationen**</span><span class="sxs-lookup"><span data-stu-id="ae3ea-115">**API documentation**</span></span></td><td>[<span data-ttu-id="ae3ea-116">Ändra Feed Processor biblioteket API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="ae3ea-116">Change Feed Processor library API reference documentation</span></span>](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="ae3ea-117">**Kom igång**</span><span class="sxs-lookup"><span data-stu-id="ae3ea-117">**Get started**</span></span></td><td>[<span data-ttu-id="ae3ea-118">Kom igång med ändringen Feed Processor .NET DocumentDB SDK</span><span class="sxs-lookup"><span data-stu-id="ae3ea-118">Get started with the DocumentDB Change Feed Processor .NET SDK</span></span>](change-feed.md)</td></tr>

<tr><td><span data-ttu-id="ae3ea-119">**Aktuella framework som stöds**</span><span class="sxs-lookup"><span data-stu-id="ae3ea-119">**Current supported framework**</span></span></td><td>[<span data-ttu-id="ae3ea-120">Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="ae3ea-120">Microsoft .NET Framework 4.5</span></span>](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="ae3ea-121">Viktig information</span><span class="sxs-lookup"><span data-stu-id="ae3ea-121">Release notes</span></span>

### <a name="a-name110110"></a><span data-ttu-id="ae3ea-122"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="ae3ea-122"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="ae3ea-123">Lägga till en metod för att få en uppskattning av återstående arbete som ska bearbetas i ändra Feed.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-123">Added a method to obtain an estimate of remaining work to be processed in the Change Feed.</span></span>
* <span data-ttu-id="ae3ea-124">Kompatibel med [.NET DocumentDB SDK](documentdb-sdk-dotnet.md) versioner 1.13.2 och högre.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-124">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.13.2 and above.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="ae3ea-125"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="ae3ea-125"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="ae3ea-126">GA-SDK</span><span class="sxs-lookup"><span data-stu-id="ae3ea-126">GA SDK</span></span>
* <span data-ttu-id="ae3ea-127">Kompatibel med [.NET DocumentDB SDK](documentdb-sdk-dotnet.md) versioner 1.14.1 och nedan.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-127">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.14.1 and below.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="ae3ea-128">Versionen & pensionering datum</span><span class="sxs-lookup"><span data-stu-id="ae3ea-128">Release & Retirement dates</span></span>
<span data-ttu-id="ae3ea-129">Microsoft meddelar notification minst **12 månader** innan du tar bort en SDK för att utjämna övergången till en nyare/stöds version.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-129">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="ae3ea-130">Nya funktioner och funktionalitet och optimeringar bara lägga till den aktuella SDK, som vi rekommenderar att du alltid uppgraderar till den senaste SDK-versionen så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-130">New features and functionality and optimizations are only added to the current SDK, as such it is recommended that you always upgrade to the latest SDK version as early as possible.</span></span> 

<span data-ttu-id="ae3ea-131">Alla förfrågningar till Cosmos-databasen med en pensionerad SDK avvisas av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-131">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

<br/>

| <span data-ttu-id="ae3ea-132">Version</span><span class="sxs-lookup"><span data-stu-id="ae3ea-132">Version</span></span> | <span data-ttu-id="ae3ea-133">Utgivningsdatum</span><span class="sxs-lookup"><span data-stu-id="ae3ea-133">Release Date</span></span> | <span data-ttu-id="ae3ea-134">Datumet för tillbakadragandet</span><span class="sxs-lookup"><span data-stu-id="ae3ea-134">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="ae3ea-135">1.1.0</span><span class="sxs-lookup"><span data-stu-id="ae3ea-135">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="ae3ea-136">13 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="ae3ea-136">August 13, 2017</span></span> |--- |
| [<span data-ttu-id="ae3ea-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="ae3ea-137">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="ae3ea-138">07 juli 2017</span><span class="sxs-lookup"><span data-stu-id="ae3ea-138">July 07, 2017</span></span> |--- |


## <a name="faq"></a><span data-ttu-id="ae3ea-139">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="ae3ea-139">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="ae3ea-140">Se även</span><span class="sxs-lookup"><span data-stu-id="ae3ea-140">See also</span></span>
<span data-ttu-id="ae3ea-141">Läs mer om Cosmos-DB i [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.</span><span class="sxs-lookup"><span data-stu-id="ae3ea-141">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

