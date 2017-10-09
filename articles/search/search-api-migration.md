---
title: "aaaUpgrading toohello Azure Söktjänsts-REST API version 2016-09-01 | Microsoft Docs"
description: "Uppgradera toohello Azure Söktjänsts-REST API version 2016-09-01"
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
ms.openlocfilehash: d0276b9cc52996a59f9aa726c27e62c6082eb908
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-service-rest-api-version-2016-09-01"></a><span data-ttu-id="681f2-103">Uppgradera toohello Azure Söktjänsts-REST API version 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="681f2-103">Upgrading toohello Azure Search Service REST API version 2016-09-01</span></span>
<span data-ttu-id="681f2-104">Om du använder version 2015-02-28 eller 2015-02-28-Preview av hello [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), den här artikeln hjälper dig att uppgradera din programmet toouse hello nästa allmänt tillgänglig API-version, 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="681f2-104">If you're using version 2015-02-28 or 2015-02-28-Preview of hello [Azure Search Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), this article will help you upgrade your application toouse hello next generally available API version, 2016-09-01.</span></span>

<span data-ttu-id="681f2-105">Version 2016-09-01 av hello REST API innehåller vissa ändringar från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="681f2-105">Version 2016-09-01 of hello REST API contains some changes from earlier versions.</span></span> <span data-ttu-id="681f2-106">Dessa är främst bakåtkompatibla så ändrar koden kräver endast minsta möjliga ansträngning, beroende på vilken version du använde före.</span><span class="sxs-lookup"><span data-stu-id="681f2-106">These are mostly backward compatible, so changing your code should require only minimal effort, depending on which version you were using before.</span></span> <span data-ttu-id="681f2-107">Se [steg tooupgrade](#UpgradeSteps) anvisningar för hur toochange din kod toouse hello nya API-version.</span><span class="sxs-lookup"><span data-stu-id="681f2-107">See [Steps tooupgrade](#UpgradeSteps) for instructions on how toochange your code toouse hello new API version.</span></span>

> [!NOTE]
> <span data-ttu-id="681f2-108">Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive hello senaste.</span><span class="sxs-lookup"><span data-stu-id="681f2-108">Your Azure Search service instance supports several REST API versions, including hello latest one.</span></span> <span data-ttu-id="681f2-109">Du kan fortsätta toouse en version när den inte längre hello senaste, men vi rekommenderar att du migrerar din kod toouse hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="681f2-109">You can continue toouse a version when it is no longer hello latest one, but we recommend that you migrate your code toouse hello newest version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a><span data-ttu-id="681f2-110">Vad är nytt i version 2016-09-01</span><span class="sxs-lookup"><span data-stu-id="681f2-110">What's new in version 2016-09-01</span></span>
<span data-ttu-id="681f2-111">Version 2016-09-01 är hello andra allmänt tillgänglig version av hello Azure Söktjänsts-REST API.</span><span class="sxs-lookup"><span data-stu-id="681f2-111">Version 2016-09-01 is hello second generally available release of hello Azure Search Service REST API.</span></span> <span data-ttu-id="681f2-112">Nya funktioner i den här API-versionen är:</span><span class="sxs-lookup"><span data-stu-id="681f2-112">New features in this API version include:</span></span>

* <span data-ttu-id="681f2-113">[Anpassade analyzers](https://aka.ms/customanalyzers), vilket gör att du tootake kontroll över hello att konvertera text till indexeras och sökbara token.</span><span class="sxs-lookup"><span data-stu-id="681f2-113">[Custom analyzers](https://aka.ms/customanalyzers), which allow you tootake control over hello process of converting text into indexable and searchable tokens.</span></span>
* <span data-ttu-id="681f2-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) och [Azure Table Storage](search-howto-indexing-azure-tables.md) indexerare som gör att du tooeasily importera data från Azure-lagring till Azure Search i ett schema eller på begäran.</span><span class="sxs-lookup"><span data-stu-id="681f2-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexers, which allow you tooeasily import data from Azure storage into Azure Search on a schedule or on-demand.</span></span>
* <span data-ttu-id="681f2-115">[Fältet mappningar](search-indexer-field-mappings.md), vilket gör att du toocustomize hur indexerare importerar data till Azure Search.</span><span class="sxs-lookup"><span data-stu-id="681f2-115">[Field mappings](search-indexer-field-mappings.md), which allow you toocustomize how indexers import data into Azure Search.</span></span>
* <span data-ttu-id="681f2-116">ETags som gör att du tooupdate hello definitioner av index och indexerare datakällor på ett samtidighet-säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="681f2-116">ETags, which allow you tooupdate hello definitions of indexes, indexers, and data sources in a concurrency-safe manner.</span></span> 

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a><span data-ttu-id="681f2-117">Steg tooupgrade</span><span class="sxs-lookup"><span data-stu-id="681f2-117">Steps tooupgrade</span></span>
<span data-ttu-id="681f2-118">Om du uppgraderar från version 2015-02-28, har du förmodligen inte toomake eventuella ändringar tooyour kod, än toochange hello-versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="681f2-118">If you are upgrading from version 2015-02-28, you probably won't have toomake any changes tooyour code, other than toochange hello version number.</span></span> <span data-ttu-id="681f2-119">hello endast situationer där du kanske behöver toochange koden är när:</span><span class="sxs-lookup"><span data-stu-id="681f2-119">hello only situations in which you may need toochange code are when:</span></span>

* <span data-ttu-id="681f2-120">Det går inte att din kod när okända egenskaper returneras som en API-svar.</span><span class="sxs-lookup"><span data-stu-id="681f2-120">Your code fails when unrecognized properties are returned in an API response.</span></span> <span data-ttu-id="681f2-121">Ditt program bör Ignorera egenskaper som inte förstår som standard.</span><span class="sxs-lookup"><span data-stu-id="681f2-121">By default your application should ignore properties that it does not understand.</span></span>
* <span data-ttu-id="681f2-122">Koden kvarstår API-begäranden och försöker tooresend dem toohello nya API-versionen.</span><span class="sxs-lookup"><span data-stu-id="681f2-122">Your code persists API requests and tries tooresend them toohello new API version.</span></span> <span data-ttu-id="681f2-123">T.ex, detta kan inträffa om tillämpningsprogrammet kvarstår fortsättning token som returnerades från hello Sök-API (leta efter mer information `@search.nextPageParameters` i hello [Sök API-referens](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span><span class="sxs-lookup"><span data-stu-id="681f2-123">For example, this might happen if your application persists continuation tokens returned from hello Search API (for more information, look for `@search.nextPageParameters` in hello [Search API Reference](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span></span>

<span data-ttu-id="681f2-124">Om någon av dessa situationer gäller tooyou kan måste toochange koden i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="681f2-124">If either of these situations apply tooyou, then you may need toochange your code accordingly.</span></span> <span data-ttu-id="681f2-125">I annat fall inga ändringar bör vara nödvändigt om du inte vill använda hello toostart [nya funktioner](#WhatsNew) version 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="681f2-125">Otherwise, no changes should be necessary unless you want toostart using hello [new features](#WhatsNew) of version 2016-09-01.</span></span>

<span data-ttu-id="681f2-126">Om du uppgraderar från version 2015-02-28-Preview hello ovan gäller också, men du måste också vara medveten om att vissa funktioner inte är tillgängliga i version 2016 09 01:</span><span class="sxs-lookup"><span data-stu-id="681f2-126">If you are upgrading from version 2015-02-28-Preview, hello above also applies, but you must also be aware that some preview features are not available in version 2016-09-01:</span></span>

* <span data-ttu-id="681f2-127">Azure Blob Storage indexeraren-stöd för CSV-filer och blobbar som innehåller JSON-matriser.</span><span class="sxs-lookup"><span data-stu-id="681f2-127">Azure Blob Storage indexer support for CSV files and blobs containing JSON arrays.</span></span>
* <span data-ttu-id="681f2-128">Synonymer</span><span class="sxs-lookup"><span data-stu-id="681f2-128">Synonyms</span></span>
* <span data-ttu-id="681f2-129">”Mer så här” frågor</span><span class="sxs-lookup"><span data-stu-id="681f2-129">"More like this" queries</span></span>

<span data-ttu-id="681f2-130">Om din kod använder dessa funktioner, kommer inte att kunna tooupgrade too2016-09-01 utan att ta bort din användning av dessa.</span><span class="sxs-lookup"><span data-stu-id="681f2-130">If your code uses these features, you will not be able tooupgrade too2016-09-01 without removing your usage of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="681f2-131">Kontrollera komma ihåg, förhandsgranska API: er är avsedd för testning och utvärdering och ska inte användas i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="681f2-131">Please remember, preview APIs are intended for testing and evaluation, and should not be used in production environments.</span></span>
> 
> 

## <a name="conclusion"></a><span data-ttu-id="681f2-132">Slutsats</span><span class="sxs-lookup"><span data-stu-id="681f2-132">Conclusion</span></span>
<span data-ttu-id="681f2-133">Om du vill ha mer information om hur du använder hello Azure Söktjänsts-REST API finns hello nyligen uppdaterat [API-referens](https://msdn.microsoft.com/library/azure/dn798935.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="681f2-133">If you need more details on using hello Azure Search Service REST API, see hello recently updated [API Reference](https://msdn.microsoft.com/library/azure/dn798935.aspx) on MSDN.</span></span>

<span data-ttu-id="681f2-134">Vi uppskattar din feedback på Azure Search.</span><span class="sxs-lookup"><span data-stu-id="681f2-134">We welcome your feedback on Azure Search.</span></span> <span data-ttu-id="681f2-135">Om du får problem känna sig fria tooask oss hjälp om hello [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) eller [StackOverflow](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="681f2-135">If you encounter problems, feel free tooask us for help on hello [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) or [StackOverflow](http://stackoverflow.com/).</span></span> <span data-ttu-id="681f2-136">Om begär du en fråga om Azure söka på StackOverflow, se till att tootag med `azure-search`.</span><span class="sxs-lookup"><span data-stu-id="681f2-136">If you're asking a question about Azure Search on StackOverflow, make sure tootag it with `azure-search`.</span></span>

<span data-ttu-id="681f2-137">Tack för att använda Azure Search!</span><span class="sxs-lookup"><span data-stu-id="681f2-137">Thank you for using Azure Search!</span></span>

