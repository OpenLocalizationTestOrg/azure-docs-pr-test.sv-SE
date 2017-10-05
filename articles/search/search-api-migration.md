---
title: "Uppgradera till den Azure Söktjänsts-REST API version 2016-09-01 | Microsoft Docs"
description: "Uppgradera till den Azure Söktjänsts-REST API version 2016-09-01"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: f6a189c2e314b91c490583a86d8bacca8ec78a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upgrading-to-the-azure-search-service-rest-api-version-2016-09-01"></a><span data-ttu-id="7464b-103">Uppgradera till den Azure Söktjänsts-REST API version 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="7464b-103">Upgrading to the Azure Search Service REST API version 2016-09-01</span></span>
<span data-ttu-id="7464b-104">Om du använder version 2015-02-28 eller 2015-02-28-Preview av den [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), den här artikeln hjälper dig att uppgradera ditt program att använda den nästa allmänt tillgänglig API-versionen 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="7464b-104">If you're using version 2015-02-28 or 2015-02-28-Preview of the [Azure Search Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), this article will help you upgrade your application to use the next generally available API version, 2016-09-01.</span></span>

<span data-ttu-id="7464b-105">Version 2016-09-01 av REST API innehåller vissa ändringar från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="7464b-105">Version 2016-09-01 of the REST API contains some changes from earlier versions.</span></span> <span data-ttu-id="7464b-106">Dessa är främst bakåtkompatibla så ändrar koden kräver endast minsta möjliga ansträngning, beroende på vilken version du använde före.</span><span class="sxs-lookup"><span data-stu-id="7464b-106">These are mostly backward compatible, so changing your code should require only minimal effort, depending on which version you were using before.</span></span> <span data-ttu-id="7464b-107">Se [steg för att uppgradera](#UpgradeSteps) för instruktioner om hur du ändrar koden för att använda den nya API-versionen.</span><span class="sxs-lookup"><span data-stu-id="7464b-107">See [Steps to upgrade](#UpgradeSteps) for instructions on how to change your code to use the new API version.</span></span>

> [!NOTE]
> <span data-ttu-id="7464b-108">Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive det senaste.</span><span class="sxs-lookup"><span data-stu-id="7464b-108">Your Azure Search service instance supports several REST API versions, including the latest one.</span></span> <span data-ttu-id="7464b-109">Du kan fortsätta att använda en version när det är inte längre den senaste, men vi rekommenderar att du migrerar din kod för att använda den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="7464b-109">You can continue to use a version when it is no longer the latest one, but we recommend that you migrate your code to use the newest version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a><span data-ttu-id="7464b-110">Vad är nytt i version 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="7464b-110">What's new in version 2016-09-01</span></span>
<span data-ttu-id="7464b-111">Version 2016-09-01 är den allmänt tillgängliga andra utgåvan av den Azure Söktjänsts-REST API.</span><span class="sxs-lookup"><span data-stu-id="7464b-111">Version 2016-09-01 is the second generally available release of the Azure Search Service REST API.</span></span> <span data-ttu-id="7464b-112">Nya funktioner i den här API-versionen är:</span><span class="sxs-lookup"><span data-stu-id="7464b-112">New features in this API version include:</span></span>

* <span data-ttu-id="7464b-113">[Anpassade analyzers](https://aka.ms/customanalyzers), vilket gör att du kan ta kontroll över processen konvertera text till indexeras och sökbara token.</span><span class="sxs-lookup"><span data-stu-id="7464b-113">[Custom analyzers](https://aka.ms/customanalyzers), which allow you to take control over the process of converting text into indexable and searchable tokens.</span></span>
* <span data-ttu-id="7464b-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) och [Azure Table Storage](search-howto-indexing-azure-tables.md) indexerare som gör att du enkelt kan importera data från Azure-lagring till Azure Search i ett schema eller på begäran.</span><span class="sxs-lookup"><span data-stu-id="7464b-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexers, which allow you to easily import data from Azure storage into Azure Search on a schedule or on-demand.</span></span>
* <span data-ttu-id="7464b-115">[Fältet mappningar](search-indexer-field-mappings.md), vilket kan du anpassa hur indexerare importerar data till Azure Search.</span><span class="sxs-lookup"><span data-stu-id="7464b-115">[Field mappings](search-indexer-field-mappings.md), which allow you to customize how indexers import data into Azure Search.</span></span>
* <span data-ttu-id="7464b-116">ETags som gör att du kan uppdatera definitionerna av index och indexerare datakällor på samtidighet-säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="7464b-116">ETags, which allow you to update the definitions of indexes, indexers, and data sources in a concurrency-safe manner.</span></span> 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a><span data-ttu-id="7464b-117">Instruktioner för uppgradering</span><span class="sxs-lookup"><span data-stu-id="7464b-117">Steps to upgrade</span></span>
<span data-ttu-id="7464b-118">Om du uppgraderar från version 2015-02-28 behöver du förmodligen inte göra ändringar i din kod än för att ändra versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="7464b-118">If you are upgrading from version 2015-02-28, you probably won't have to make any changes to your code, other than to change the version number.</span></span> <span data-ttu-id="7464b-119">Endast situationer där du kan behöva ändra koden är när:</span><span class="sxs-lookup"><span data-stu-id="7464b-119">The only situations in which you may need to change code are when:</span></span>

* <span data-ttu-id="7464b-120">Det går inte att din kod när okända egenskaper returneras som en API-svar.</span><span class="sxs-lookup"><span data-stu-id="7464b-120">Your code fails when unrecognized properties are returned in an API response.</span></span> <span data-ttu-id="7464b-121">Ditt program bör Ignorera egenskaper som inte förstår som standard.</span><span class="sxs-lookup"><span data-stu-id="7464b-121">By default your application should ignore properties that it does not understand.</span></span>
* <span data-ttu-id="7464b-122">Koden kvarstår API-begäranden och försöker skicka dem till den nya API-versionen.</span><span class="sxs-lookup"><span data-stu-id="7464b-122">Your code persists API requests and tries to resend them to the new API version.</span></span> <span data-ttu-id="7464b-123">T.ex, detta kan inträffa om tillämpningsprogrammet kvarstår fortsättning token som returnerades från Sök-API (leta efter mer information `@search.nextPageParameters` i den [Sök API-referens](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span><span class="sxs-lookup"><span data-stu-id="7464b-123">For example, this might happen if your application persists continuation tokens returned from the Search API (for more information, look for `@search.nextPageParameters` in the [Search API Reference](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span></span>

<span data-ttu-id="7464b-124">Om någon av dessa situationer gäller dig måste att ändra koden i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="7464b-124">If either of these situations apply to you, then you may need to change your code accordingly.</span></span> <span data-ttu-id="7464b-125">I annat fall inga ändringar bör vara nödvändigt om du inte vill börja använda den [nya funktioner](#WhatsNew) version 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="7464b-125">Otherwise, no changes should be necessary unless you want to start using the [new features](#WhatsNew) of version 2016-09-01.</span></span>

<span data-ttu-id="7464b-126">Om du uppgraderar från version 2015-02-28-Preview ovanstående gäller också, men du måste också vara medveten om att vissa funktioner inte är tillgängliga i version 2016 09 01:</span><span class="sxs-lookup"><span data-stu-id="7464b-126">If you are upgrading from version 2015-02-28-Preview, the above also applies, but you must also be aware that some preview features are not available in version 2016-09-01:</span></span>

* <span data-ttu-id="7464b-127">Azure Blob Storage indexeraren-stöd för CSV-filer och blobbar som innehåller JSON-matriser.</span><span class="sxs-lookup"><span data-stu-id="7464b-127">Azure Blob Storage indexer support for CSV files and blobs containing JSON arrays.</span></span>
* <span data-ttu-id="7464b-128">Synonymer</span><span class="sxs-lookup"><span data-stu-id="7464b-128">Synonyms</span></span>
* <span data-ttu-id="7464b-129">”Mer så här” frågor</span><span class="sxs-lookup"><span data-stu-id="7464b-129">"More like this" queries</span></span>

<span data-ttu-id="7464b-130">Om din kod använder dessa funktioner, kan du inte uppgradera till 2016-09-01 utan att ta bort din användning av dessa.</span><span class="sxs-lookup"><span data-stu-id="7464b-130">If your code uses these features, you will not be able to upgrade to 2016-09-01 without removing your usage of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7464b-131">Kontrollera komma ihåg, förhandsgranska API: er är avsedd för testning och utvärdering och ska inte användas i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="7464b-131">Please remember, preview APIs are intended for testing and evaluation, and should not be used in production environments.</span></span>
> 
> 

## <a name="conclusion"></a><span data-ttu-id="7464b-132">Slutsats</span><span class="sxs-lookup"><span data-stu-id="7464b-132">Conclusion</span></span>
<span data-ttu-id="7464b-133">Om du vill ha mer information om hur du använder Azure Search Service REST-API finns nyligen uppdaterat [API-referens](https://msdn.microsoft.com/library/azure/dn798935.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="7464b-133">If you need more details on using the Azure Search Service REST API, see the recently updated [API Reference](https://msdn.microsoft.com/library/azure/dn798935.aspx) on MSDN.</span></span>

<span data-ttu-id="7464b-134">Vi uppskattar din feedback på Azure Search.</span><span class="sxs-lookup"><span data-stu-id="7464b-134">We welcome your feedback on Azure Search.</span></span> <span data-ttu-id="7464b-135">Om du får problem passa på att be om hjälp oss på den [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) eller [StackOverflow](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7464b-135">If you encounter problems, feel free to ask us for help on the [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) or [StackOverflow](http://stackoverflow.com/).</span></span> <span data-ttu-id="7464b-136">Om begär du en fråga om Azure Search på StackOverflow, kontrollerar du att märka den med `azure-search`.</span><span class="sxs-lookup"><span data-stu-id="7464b-136">If you're asking a question about Azure Search on StackOverflow, make sure to tag it with `azure-search`.</span></span>

<span data-ttu-id="7464b-137">Tack för att använda Azure Search!</span><span class="sxs-lookup"><span data-stu-id="7464b-137">Thank you for using Azure Search!</span></span>

