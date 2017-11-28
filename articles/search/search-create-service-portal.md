---
title: "aaaCreate en Azure Search-tjänst i hello portal | Microsoft Docs"
description: "Etablera en Azure Search-tjänst i hello-portalen."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: f1c7197a1467e32bd4b360b165c9059e6bb0e496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-service-in-hello-portal"></a><span data-ttu-id="a8723-103">Skapa en Azure Search-tjänst i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="a8723-103">Create an Azure Search service in hello portal</span></span>

<span data-ttu-id="a8723-104">Den här artikeln förklarar hur toocreate eller etablera ett Azure Search-tjänst i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8723-104">This article explains how toocreate or provision an Azure Search service in hello portal.</span></span> <span data-ttu-id="a8723-105">PowerShell instruktioner finns i [hantera Azure Search med PowerShell](search-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a8723-105">For PowerShell instructions, see [Manage Azure Search with PowerShell](search-manage-powershell.md).</span></span>

## <a name="subscribe-free-or-paid"></a><span data-ttu-id="a8723-106">Prenumerera (ledigt eller betald)</span><span class="sxs-lookup"><span data-stu-id="a8723-106">Subscribe (free or paid)</span></span>

<span data-ttu-id="a8723-107">[Öppna ett kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) och använda kostnadsfria krediter tootry ut betald Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="a8723-107">[Open a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) and use free credits tootry out paid Azure services.</span></span> <span data-ttu-id="a8723-108">Efter att krediten är slut, hålla hello konto och fortsätta toouse kostnadsfria Azure-tjänster, till exempel Websites.</span><span class="sxs-lookup"><span data-stu-id="a8723-108">After credits are used up, keep hello account and continue toouse free Azure services, such as Websites.</span></span> <span data-ttu-id="a8723-109">Ditt kreditkort debiteras aldrig såvida du inte uttryckligen ändrar dina inställningar och be toobe debiteras.</span><span class="sxs-lookup"><span data-stu-id="a8723-109">Your credit card is never charged unless you explicitly change your settings and ask toobe charged.</span></span>

<span data-ttu-id="a8723-110">Du kan också [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="a8723-110">Alternatively, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span> <span data-ttu-id="a8723-111">En MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="a8723-111">An MSDN subscription gives you credits every month you can use for paid Azure services.</span></span> 

## <a name="find-azure-search"></a><span data-ttu-id="a8723-112">Hitta Azure Search</span><span class="sxs-lookup"><span data-stu-id="a8723-112">Find Azure Search</span></span>
1. <span data-ttu-id="a8723-113">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a8723-113">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a8723-114">Klicka hello plustecken (”+”) i hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="a8723-114">Click hello plus sign ("+") in hello top left corner.</span></span>
3. <span data-ttu-id="a8723-115">Välj **webb + mobilt** > **Azure Search**.</span><span class="sxs-lookup"><span data-stu-id="a8723-115">Select **Web + Mobile** > **Azure Search**.</span></span>

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-hello-service-and-url-endpoint"></a><span data-ttu-id="a8723-116">Namnet hello-tjänsten och URL-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a8723-116">Name hello service and URL endpoint</span></span>

<span data-ttu-id="a8723-117">Ett tjänstnamn är en del av hello URL-slutpunkt mot vilken API-anrop har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="a8723-117">A service name is part of hello URL endpoint against which API calls are issued.</span></span> <span data-ttu-id="a8723-118">Skriv ditt namn i hello **URL** fältet.</span><span class="sxs-lookup"><span data-stu-id="a8723-118">Type your service name in hello **URL** field.</span></span> 

<span data-ttu-id="a8723-119">Kraven för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a8723-119">Service name requirements:</span></span>
   * <span data-ttu-id="a8723-120">2 och 60 tecken</span><span class="sxs-lookup"><span data-stu-id="a8723-120">2 and 60 characters in length</span></span>
   * <span data-ttu-id="a8723-121">gemena bokstäver, siffror eller bindestreck (”-”)</span><span class="sxs-lookup"><span data-stu-id="a8723-121">lowercase letters, digits, or dashes ("-")</span></span>
   * <span data-ttu-id="a8723-122">Inga streck (”-”) som hello första 2 tecken eller sista tecken</span><span class="sxs-lookup"><span data-stu-id="a8723-122">no dash ("-") as hello first 2 characters or last single character</span></span>
   * <span data-ttu-id="a8723-123">streck i följd (”--”)</span><span class="sxs-lookup"><span data-stu-id="a8723-123">no consecutive dashes ("--")</span></span>

## <a name="select-a-subscription"></a><span data-ttu-id="a8723-124">Välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8723-124">Select a subscription</span></span>
<span data-ttu-id="a8723-125">Om du har mer än en prenumeration väljer du en som också har data eller file storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="a8723-125">If you have more than one subscription, choose one that also has data or file storage services.</span></span> <span data-ttu-id="a8723-126">Azure Search kan automatisk identifiering av Azure Table och Blob storage, SQL Database och Azure Cosmos DB för indexering *indexerare*, men endast för tjänster i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a8723-126">Azure Search can auto-detect Azure Table and Blob storage, SQL Database, and Azure Cosmos DB for indexing via *indexers*, but only for services in hello same subscription.</span></span>

## <a name="select-a-resource-group"></a><span data-ttu-id="a8723-127">Välj en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a8723-127">Select a resource group</span></span>
<span data-ttu-id="a8723-128">En resursgrupp är en samling Azure-tjänster och resurser som används tillsammans.</span><span class="sxs-lookup"><span data-stu-id="a8723-128">A resource group is a collection of Azure services and resources used together.</span></span> <span data-ttu-id="a8723-129">Till exempel om du använder Azure Search tooindex en SQL-databas, sedan båda tjänsterna bör vara en del av hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a8723-129">For example, if you are using Azure Search tooindex a SQL database, then both services should be part of hello same resource group.</span></span>

> [!TIP]
> <span data-ttu-id="a8723-130">En resursgrupp också tar du bort hello tjänster i den.</span><span class="sxs-lookup"><span data-stu-id="a8723-130">Deleting a resource group also deletes hello services within it.</span></span> <span data-ttu-id="a8723-131">För prototyp projekt använder flera tjänster, placerar dem i hello underlättar samma resursgrupp Rensa när hello projektet är över.</span><span class="sxs-lookup"><span data-stu-id="a8723-131">For prototype projects utilizing multiple services, putting all of them in hello same resource group makes cleanup easier after hello project is over.</span></span> 

## <a name="select-a-hosting-location"></a><span data-ttu-id="a8723-132">Välj en värdplats</span><span class="sxs-lookup"><span data-stu-id="a8723-132">Select a hosting location</span></span> 
<span data-ttu-id="a8723-133">Azure Search kan finnas i datacenter runt hälsningsmeddelande som en Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a8723-133">As an Azure service, Azure Search can be hosted in datacenters around hello world.</span></span> <span data-ttu-id="a8723-134">Observera att [priser kan skilja sig](https://azure.microsoft.com/pricing/details/search/) efter geografi.</span><span class="sxs-lookup"><span data-stu-id="a8723-134">Note that [prices can differ](https://azure.microsoft.com/pricing/details/search/) by geography.</span></span>

## <a name="select-a-pricing-tier-sku"></a><span data-ttu-id="a8723-135">Välj en prisnivå (SKU)</span><span class="sxs-lookup"><span data-stu-id="a8723-135">Select a pricing tier (SKU)</span></span>
<span data-ttu-id="a8723-136">[Azure Search är för närvarande finns flera prisnivåer](https://azure.microsoft.com/pricing/details/search/): ledigt, Basic eller Standard.</span><span class="sxs-lookup"><span data-stu-id="a8723-136">[Azure Search is currently offered in multiple pricing tiers](https://azure.microsoft.com/pricing/details/search/): Free, Basic, or Standard.</span></span> <span data-ttu-id="a8723-137">Varje nivå har sin egen [kapacitet och gränser](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="a8723-137">Each tier has its own [capacity and limits](search-limits-quotas-capacity.md).</span></span> <span data-ttu-id="a8723-138">Se [Välj en prisnivå nivå eller SKU](search-sku-tier.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="a8723-138">See [Choose a pricing tier or SKU](search-sku-tier.md) for guidance.</span></span>

<span data-ttu-id="a8723-139">I den här genomgången, har vi valt hello standardnivån för vår tjänst.</span><span class="sxs-lookup"><span data-stu-id="a8723-139">In this walkthrough, we have chosen hello Standard tier for our service.</span></span>

## <a name="create-your-service"></a><span data-ttu-id="a8723-140">Skapa din tjänst</span><span class="sxs-lookup"><span data-stu-id="a8723-140">Create your service</span></span>

<span data-ttu-id="a8723-141">Kom ihåg toopin service toohello instrumentpanelen för enkel åtkomst när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="a8723-141">Remember toopin your service toohello dashboard for easy access whenever you sign in.</span></span>

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a><span data-ttu-id="a8723-142">Skala din tjänst</span><span class="sxs-lookup"><span data-stu-id="a8723-142">Scale your service</span></span>
<span data-ttu-id="a8723-143">Det kan ta några minuter toocreate en tjänst (15 minuter eller mer beroende på hello nivån).</span><span class="sxs-lookup"><span data-stu-id="a8723-143">It can take a few minutes toocreate a service (15 minutes or more depending on hello tier).</span></span> <span data-ttu-id="a8723-144">När tjänsten har etablerats du skala det toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="a8723-144">After your service is provisioned, you can scale it toomeet your needs.</span></span> <span data-ttu-id="a8723-145">Eftersom du har valt hello standardnivån för din Azure Search-tjänst kan du skala din tjänst i två dimensioner: repliker och partitioner.</span><span class="sxs-lookup"><span data-stu-id="a8723-145">Because you chose hello Standard tier for your Azure Search service, you can scale your service in two dimensions: replicas and partitions.</span></span> <span data-ttu-id="a8723-146">Hade du valt hello grundläggande nivån, du kan bara lägga till repliker.</span><span class="sxs-lookup"><span data-stu-id="a8723-146">Had you chosen hello Basic tier, you can only add replicas.</span></span> <span data-ttu-id="a8723-147">Skalning är inte tillgängligt om du har etablerat hello kostnadsfria tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8723-147">If you provisioned hello free service, scale is not available.</span></span>

<span data-ttu-id="a8723-148">***Partitioner*** Tillåt din service toostore och söka igenom flera dokument.</span><span class="sxs-lookup"><span data-stu-id="a8723-148">***Partitions*** allow your service toostore and search through more documents.</span></span>

<span data-ttu-id="a8723-149">***Repliker*** Tillåt service-toohandle en högre belastning av sökfrågor.</span><span class="sxs-lookup"><span data-stu-id="a8723-149">***Replicas*** allow your service toohandle a higher load of search queries.</span></span>

> [!Important]
> <span data-ttu-id="a8723-150">En tjänst måste ha [2 repliker för skrivskyddade SLA och 3 repliker för läsning och skrivning SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="a8723-150">A service must have [2 replicas for read-only SLA and 3 replicas for read/write SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

1. <span data-ttu-id="a8723-151">Gå tooyour search-tjänsten bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8723-151">Go tooyour search service blade in hello Azure portal.</span></span>
2. <span data-ttu-id="a8723-152">Hello vänster-navigeringsfönstret och välj **inställningar** > **skala**.</span><span class="sxs-lookup"><span data-stu-id="a8723-152">In hello left-navigation pane, select **Settings** > **Scale**.</span></span>
3. <span data-ttu-id="a8723-153">Använd hello slidebar tooadd repliker eller partitioner.</span><span class="sxs-lookup"><span data-stu-id="a8723-153">Use hello slidebar tooadd Replicas or Partitions.</span></span>

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> <span data-ttu-id="a8723-154">Varje nivå har olika [gränser](search-limits-quotas-capacity.md) på hello Totalt antal Search-enheter som tillåts i en enskild tjänst (repliker * partitioner = Totalt antal Search-enheter).</span><span class="sxs-lookup"><span data-stu-id="a8723-154">Each tier has different [limits](search-limits-quotas-capacity.md) on hello total number of Search Units allowed in a single service (Replicas * Partitions = Total Search Units).</span></span>

## <a name="when-tooadd-a-second-service"></a><span data-ttu-id="a8723-155">När tooadd en andra tjänst</span><span class="sxs-lookup"><span data-stu-id="a8723-155">When tooadd a second service</span></span>

<span data-ttu-id="a8723-156">En stor majoritet av kunder använder en tjänst som etablerats på en nivå som ger hello [Högerklicka balans mellan resurser](search-sku-tier.md).</span><span class="sxs-lookup"><span data-stu-id="a8723-156">A large majority of customers use just one service provisioned at a tier that provides hello [right balance of resources](search-sku-tier.md).</span></span> <span data-ttu-id="a8723-157">En tjänst kan vara värd för flera index, ämne toohello [begränsningarna hello-nivå som du väljer](search-capacity-planning.md), med varje index isolerade från varandra.</span><span class="sxs-lookup"><span data-stu-id="a8723-157">One service can host multiple indexes, subject toohello [maximum limits of hello tier you select](search-capacity-planning.md), with each index isolated from another.</span></span> <span data-ttu-id="a8723-158">I Azure Search-begäranden och kan bara vara riktad tooone index, minimera hello risken för oavsiktligt eller avsiktligt datahämtning från andra index i hello samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="a8723-158">In Azure Search, requests can only be directed tooone index, minimizing hello chance of accidental or intentional data retrieval from other indexes in hello same service.</span></span>

<span data-ttu-id="a8723-159">Även om de flesta kunder använder en tjänst, kan det vara nödvändigt om operativa krav inkludera hello följande service redundans:</span><span class="sxs-lookup"><span data-stu-id="a8723-159">Although most customers use just one service, service redundancy might be necessary if operational requirements include hello following:</span></span>

+ <span data-ttu-id="a8723-160">Katastrofåterställning (data center avbrott).</span><span class="sxs-lookup"><span data-stu-id="a8723-160">Disaster recovery (data center outage).</span></span> <span data-ttu-id="a8723-161">Azure Search ger inte några omedelbara redundans i ett avbrott hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="a8723-161">Azure Search does not provide instant failover in hello event of an outage.</span></span> <span data-ttu-id="a8723-162">Vägledning och rekommendationer finns [tjänsten administration](search-manage.md).</span><span class="sxs-lookup"><span data-stu-id="a8723-162">For recommendations and guidance, see [Service administration](search-manage.md).</span></span>
+ <span data-ttu-id="a8723-163">Din undersökning av multitenans modellering har fastställt att ytterligare tjänster hello optimal design.</span><span class="sxs-lookup"><span data-stu-id="a8723-163">Your investigation of multi-tenancy modeling has determined that additional services is hello optimal design.</span></span> <span data-ttu-id="a8723-164">Mer information finns i [Design för multitenans](search-modeling-multitenant-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="a8723-164">For more information, see [Design for multi-tenancy](search-modeling-multitenant-saas-applications.md).</span></span>
+ <span data-ttu-id="a8723-165">Du kan behöva en instans av Azure Search för globalt distribuerade program i flera regioner toominimize fördröjningen för programmets internationell trafik.</span><span class="sxs-lookup"><span data-stu-id="a8723-165">For globally deployed applications, you might require an instance of Azure Search in multiple regions toominimize latency of your application’s international traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="a8723-166">Du kan särskilja indexering och frågar arbetsbelastningar; i Azure Search Därför skulle du skapa flera tjänster för åtskilda arbetsbelastningar aldrig.</span><span class="sxs-lookup"><span data-stu-id="a8723-166">In Azure Search, you cannot segregate indexing and querying workloads; thus, you would never create multiple services for segregated workloads.</span></span> <span data-ttu-id="a8723-167">Ett index efterfrågas alltid på hello tjänst där den skapades (du kan inte skapa ett index i en tjänst och kopiera den tooanother).</span><span class="sxs-lookup"><span data-stu-id="a8723-167">An index is always queried on hello service in which it was created (you cannot create an index in one service and copy it tooanother).</span></span>
>

<span data-ttu-id="a8723-168">En andra tjänst krävs inte för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="a8723-168">A second service is not required for high availability.</span></span> <span data-ttu-id="a8723-169">Hög tillgänglighet för frågor uppnås när du använder 2 eller flera repliker i hello samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="a8723-169">High availability for queries is achieved when you use 2 or more replicas in hello same service.</span></span> <span data-ttu-id="a8723-170">Replik uppdateringar är sekventiella, vilket innebär att minst en fungerar när en tjänstuppdatering lyfts. Läs mer om drifttid [serviceavtal](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="a8723-170">Replica updates are sequential, which means at least one is operational when a service update is rolled out. For more information about uptime, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8723-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8723-171">Next steps</span></span>
<span data-ttu-id="a8723-172">När du etablerar en Azure Search-tjänsten är du redo för[definiera ett index](search-what-is-an-index.md) så att du kan ladda upp och söka efter data.</span><span class="sxs-lookup"><span data-stu-id="a8723-172">After provisioning an Azure Search service, you are ready too[define an index](search-what-is-an-index.md) so you can upload and search your data.</span></span>

<span data-ttu-id="a8723-173">tooaccess hello tjänst från kod eller skript, ange hello URL (*Tjänstenamn*. search.windows.net) och en nyckel.</span><span class="sxs-lookup"><span data-stu-id="a8723-173">tooaccess hello service from code or script, provide hello URL (*service-name*.search.windows.net) and a key.</span></span> <span data-ttu-id="a8723-174">Admin nycklar ger fullständig åtkomst; frågan nycklar bevilja läsåtkomst.</span><span class="sxs-lookup"><span data-stu-id="a8723-174">Admin keys grant full access; query keys grant read-only access.</span></span> <span data-ttu-id="a8723-175">Se [hur toouse Azure Sök i .NET](search-howto-dotnet-sdk.md) tooget igång.</span><span class="sxs-lookup"><span data-stu-id="a8723-175">See [How toouse Azure Search in .NET](search-howto-dotnet-sdk.md) tooget started.</span></span>

<span data-ttu-id="a8723-176">Se [bygga och fråga ditt första index](search-get-started-portal.md) en snabb genomgång av portal-baserade.</span><span class="sxs-lookup"><span data-stu-id="a8723-176">See [Build and query your first index](search-get-started-portal.md) for a quick portal-based tutorial.</span></span>

