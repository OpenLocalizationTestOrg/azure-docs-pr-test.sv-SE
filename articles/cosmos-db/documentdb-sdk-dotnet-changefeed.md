---
title: "aaaAzure DocumentDB .NET SDK ändra Feed Processor & resurser | Microsoft Docs"
description: "Läs mer om hello ändra Feed Processor API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello DocumentDB .NET SDK ändra Feed Processor."
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
ms.openlocfilehash: 7c001cc77f41c01445fb53328e9d99fd3d312c58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a><span data-ttu-id="90484-103">DocumentDB .NET ändra Feed Processor SDK: Hämta och viktig information</span><span class="sxs-lookup"><span data-stu-id="90484-103">DocumentDB .NET Change Feed Processor SDK: Download and release notes</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="90484-104">.NET</span><span class="sxs-lookup"><span data-stu-id="90484-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="90484-105">.NET ändra Feed</span><span class="sxs-lookup"><span data-stu-id="90484-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="90484-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="90484-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="90484-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="90484-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="90484-108">Java</span><span class="sxs-lookup"><span data-stu-id="90484-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="90484-109">Python</span><span class="sxs-lookup"><span data-stu-id="90484-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="90484-110">REST</span><span class="sxs-lookup"><span data-stu-id="90484-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="90484-111">REST-resursprovider</span><span class="sxs-lookup"><span data-stu-id="90484-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="90484-112">SQL</span><span class="sxs-lookup"><span data-stu-id="90484-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="90484-113">**SDK-hämtningen**</span><span class="sxs-lookup"><span data-stu-id="90484-113">**SDK download**</span></span></td><td>[<span data-ttu-id="90484-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="90484-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)</td></tr>

<tr><td><span data-ttu-id="90484-115">**API-dokumentationen**</span><span class="sxs-lookup"><span data-stu-id="90484-115">**API documentation**</span></span></td><td>[<span data-ttu-id="90484-116">Ändra Feed Processor biblioteket API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="90484-116">Change Feed Processor library API reference documentation</span></span>](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="90484-117">**Kom igång**</span><span class="sxs-lookup"><span data-stu-id="90484-117">**Get started**</span></span></td><td>[<span data-ttu-id="90484-118">Kom igång med hello ändra Feed Processor .NET DocumentDB SDK</span><span class="sxs-lookup"><span data-stu-id="90484-118">Get started with hello DocumentDB Change Feed Processor .NET SDK</span></span>](change-feed.md)</td></tr>

<tr><td><span data-ttu-id="90484-119">**Aktuella framework som stöds**</span><span class="sxs-lookup"><span data-stu-id="90484-119">**Current supported framework**</span></span></td><td>[<span data-ttu-id="90484-120">Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="90484-120">Microsoft .NET Framework 4.5</span></span>](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="90484-121">Viktig information</span><span class="sxs-lookup"><span data-stu-id="90484-121">Release notes</span></span>

### <a name="a-name110110"></a><span data-ttu-id="90484-122"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="90484-122"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="90484-123">Lägga till en metod tooobtain en uppskattning av återstående arbete toobe behandlas i hello ändra Feed.</span><span class="sxs-lookup"><span data-stu-id="90484-123">Added a method tooobtain an estimate of remaining work toobe processed in hello Change Feed.</span></span>
* <span data-ttu-id="90484-124">Kompatibel med [.NET DocumentDB SDK](documentdb-sdk-dotnet.md) versioner 1.13.2 och högre.</span><span class="sxs-lookup"><span data-stu-id="90484-124">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.13.2 and above.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="90484-125"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="90484-125"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="90484-126">GA-SDK</span><span class="sxs-lookup"><span data-stu-id="90484-126">GA SDK</span></span>
* <span data-ttu-id="90484-127">Kompatibel med [.NET DocumentDB SDK](documentdb-sdk-dotnet.md) versioner 1.14.1 och nedan.</span><span class="sxs-lookup"><span data-stu-id="90484-127">Compatible with [DocumentDB .NET SDK](documentdb-sdk-dotnet.md) versions 1.14.1 and below.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="90484-128">Versionen & pensionering datum</span><span class="sxs-lookup"><span data-stu-id="90484-128">Release & Retirement dates</span></span>
<span data-ttu-id="90484-129">Microsoft meddelar notification minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.</span><span class="sxs-lookup"><span data-stu-id="90484-129">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="90484-130">Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som vi rekommenderar att du alltid uppgradera toohello senaste SDK version så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="90484-130">New features and functionality and optimizations are only added toohello current SDK, as such it is recommended that you always upgrade toohello latest SDK version as early as possible.</span></span> 

<span data-ttu-id="90484-131">Alla begäran tooCosmos databasen med en pensionerad SDK avvisas av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="90484-131">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

<br/>

| <span data-ttu-id="90484-132">Version</span><span class="sxs-lookup"><span data-stu-id="90484-132">Version</span></span> | <span data-ttu-id="90484-133">Utgivningsdatum</span><span class="sxs-lookup"><span data-stu-id="90484-133">Release Date</span></span> | <span data-ttu-id="90484-134">Datumet för tillbakadragandet</span><span class="sxs-lookup"><span data-stu-id="90484-134">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="90484-135">1.1.0</span><span class="sxs-lookup"><span data-stu-id="90484-135">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="90484-136">13 augusti 2017</span><span class="sxs-lookup"><span data-stu-id="90484-136">August 13, 2017</span></span> |--- |
| [<span data-ttu-id="90484-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="90484-137">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="90484-138">07 juli 2017</span><span class="sxs-lookup"><span data-stu-id="90484-138">July 07, 2017</span></span> |--- |


## <a name="faq"></a><span data-ttu-id="90484-139">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="90484-139">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="90484-140">Se även</span><span class="sxs-lookup"><span data-stu-id="90484-140">See also</span></span>
<span data-ttu-id="90484-141">toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.</span><span class="sxs-lookup"><span data-stu-id="90484-141">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

