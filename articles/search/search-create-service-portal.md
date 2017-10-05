---
title: "Skapa en Azure Search-tjänst i portal | Microsoft Docs"
description: "Etablera en Azure Search-tjänst i portalen."
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
ms.openlocfilehash: 58f4eab190e40e16ed261c165ffdfc8155eeb434
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-search-service-in-the-portal"></a><span data-ttu-id="fd1b2-103">Skapa en Azure Search-tjänst i portalen</span><span class="sxs-lookup"><span data-stu-id="fd1b2-103">Create an Azure Search service in the portal</span></span>

<span data-ttu-id="fd1b2-104">Den här artikeln förklarar hur du skapar eller etablera ett Azure Search-tjänsten i portalen.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-104">This article explains how to create or provision an Azure Search service in the portal.</span></span> <span data-ttu-id="fd1b2-105">PowerShell instruktioner finns i [hantera Azure Search med PowerShell](search-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-105">For PowerShell instructions, see [Manage Azure Search with PowerShell](search-manage-powershell.md).</span></span>

## <a name="subscribe-free-or-paid"></a><span data-ttu-id="fd1b2-106">Prenumerera (ledigt eller betald)</span><span class="sxs-lookup"><span data-stu-id="fd1b2-106">Subscribe (free or paid)</span></span>

<span data-ttu-id="fd1b2-107">[Öppna ett kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) och använda kostnadsfria krediter för att testa betald Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-107">[Open a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) and use free credits to try out paid Azure services.</span></span> <span data-ttu-id="fd1b2-108">Efter att krediten är slut, behålla kontot och fortsätta att använda kostnadsfria Azure-tjänster, till exempel Websites.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-108">After credits are used up, keep the account and continue to use free Azure services, such as Websites.</span></span> <span data-ttu-id="fd1b2-109">Ditt kreditkort debiteras aldrig såvida du inte uttryckligen ändrar dina inställningar och ber.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-109">Your credit card is never charged unless you explicitly change your settings and ask to be charged.</span></span>

<span data-ttu-id="fd1b2-110">Du kan också [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-110">Alternatively, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span> <span data-ttu-id="fd1b2-111">En MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-111">An MSDN subscription gives you credits every month you can use for paid Azure services.</span></span> 

## <a name="find-azure-search"></a><span data-ttu-id="fd1b2-112">Hitta Azure Search</span><span class="sxs-lookup"><span data-stu-id="fd1b2-112">Find Azure Search</span></span>
1. <span data-ttu-id="fd1b2-113">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-113">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="fd1b2-114">Klicka på plustecknet (”+”) i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-114">Click the plus sign ("+") in the top left corner.</span></span>
3. <span data-ttu-id="fd1b2-115">Välj **webb + mobilt** > **Azure Search**.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-115">Select **Web + Mobile** > **Azure Search**.</span></span>

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-the-service-and-url-endpoint"></a><span data-ttu-id="fd1b2-116">Namnet på tjänsten och URL-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="fd1b2-116">Name the service and URL endpoint</span></span>

<span data-ttu-id="fd1b2-117">Ett tjänstnamn är en del av URL-slutpunkt mot vilken API-anrop har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-117">A service name is part of the URL endpoint against which API calls are issued.</span></span> <span data-ttu-id="fd1b2-118">Skriv ditt Tjänstenamn i den **URL** fältet.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-118">Type your service name in the **URL** field.</span></span> 

<span data-ttu-id="fd1b2-119">Kraven för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="fd1b2-119">Service name requirements:</span></span>
   * <span data-ttu-id="fd1b2-120">2 och 60 tecken</span><span class="sxs-lookup"><span data-stu-id="fd1b2-120">2 and 60 characters in length</span></span>
   * <span data-ttu-id="fd1b2-121">gemena bokstäver, siffror eller bindestreck (”-”)</span><span class="sxs-lookup"><span data-stu-id="fd1b2-121">lowercase letters, digits, or dashes ("-")</span></span>
   * <span data-ttu-id="fd1b2-122">Inga streck (”-”) som den första 2 tecken eller sista tecken</span><span class="sxs-lookup"><span data-stu-id="fd1b2-122">no dash ("-") as the first 2 characters or last single character</span></span>
   * <span data-ttu-id="fd1b2-123">streck i följd (”--”)</span><span class="sxs-lookup"><span data-stu-id="fd1b2-123">no consecutive dashes ("--")</span></span>

## <a name="select-a-subscription"></a><span data-ttu-id="fd1b2-124">Välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd1b2-124">Select a subscription</span></span>
<span data-ttu-id="fd1b2-125">Om du har mer än en prenumeration väljer du en som också har data eller file storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-125">If you have more than one subscription, choose one that also has data or file storage services.</span></span> <span data-ttu-id="fd1b2-126">Azure Search kan automatisk identifiering av Azure Table och Blob storage, SQL Database och Azure Cosmos DB för indexering *indexerare*, men endast för tjänster i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-126">Azure Search can auto-detect Azure Table and Blob storage, SQL Database, and Azure Cosmos DB for indexing via *indexers*, but only for services in the same subscription.</span></span>

## <a name="select-a-resource-group"></a><span data-ttu-id="fd1b2-127">Välj en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="fd1b2-127">Select a resource group</span></span>
<span data-ttu-id="fd1b2-128">En resursgrupp är en samling Azure-tjänster och resurser som används tillsammans.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-128">A resource group is a collection of Azure services and resources used together.</span></span> <span data-ttu-id="fd1b2-129">Till exempel om du använder Azure Search ska indexera en SQL-databas, måste sedan båda tjänsterna tillhöra samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-129">For example, if you are using Azure Search to index a SQL database, then both services should be part of the same resource group.</span></span>

> [!TIP]
> <span data-ttu-id="fd1b2-130">En resursgrupp också tar du bort tjänster i den.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-130">Deleting a resource group also deletes the services within it.</span></span> <span data-ttu-id="fd1b2-131">För prototyp projekt använder flera tjänster, underlättar placerar dem i samma resursgrupp Rensa när projektet är över.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-131">For prototype projects utilizing multiple services, putting all of them in the same resource group makes cleanup easier after the project is over.</span></span> 

## <a name="select-a-hosting-location"></a><span data-ttu-id="fd1b2-132">Välj en värdplats</span><span class="sxs-lookup"><span data-stu-id="fd1b2-132">Select a hosting location</span></span> 
<span data-ttu-id="fd1b2-133">Azure Search kan finnas i datacenter runtom i världen som en Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-133">As an Azure service, Azure Search can be hosted in datacenters around the world.</span></span> <span data-ttu-id="fd1b2-134">Observera att [priser kan skilja sig](https://azure.microsoft.com/pricing/details/search/) efter geografi.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-134">Note that [prices can differ](https://azure.microsoft.com/pricing/details/search/) by geography.</span></span>

## <a name="select-a-pricing-tier-sku"></a><span data-ttu-id="fd1b2-135">Välj en prisnivå (SKU)</span><span class="sxs-lookup"><span data-stu-id="fd1b2-135">Select a pricing tier (SKU)</span></span>
<span data-ttu-id="fd1b2-136">[Azure Search är för närvarande finns flera prisnivåer](https://azure.microsoft.com/pricing/details/search/): ledigt, Basic eller Standard.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-136">[Azure Search is currently offered in multiple pricing tiers](https://azure.microsoft.com/pricing/details/search/): Free, Basic, or Standard.</span></span> <span data-ttu-id="fd1b2-137">Varje nivå har sin egen [kapacitet och gränser](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-137">Each tier has its own [capacity and limits](search-limits-quotas-capacity.md).</span></span> <span data-ttu-id="fd1b2-138">Se [Välj en prisnivå nivå eller SKU](search-sku-tier.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-138">See [Choose a pricing tier or SKU](search-sku-tier.md) for guidance.</span></span>

<span data-ttu-id="fd1b2-139">I den här genomgången, har vi valt standardnivån för vår tjänst.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-139">In this walkthrough, we have chosen the Standard tier for our service.</span></span>

## <a name="create-your-service"></a><span data-ttu-id="fd1b2-140">Skapa din tjänst</span><span class="sxs-lookup"><span data-stu-id="fd1b2-140">Create your service</span></span>

<span data-ttu-id="fd1b2-141">Kom ihåg att fästa din tjänst på instrumentpanelen för enkel åtkomst när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-141">Remember to pin your service to the dashboard for easy access whenever you sign in.</span></span>

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a><span data-ttu-id="fd1b2-142">Skala din tjänst</span><span class="sxs-lookup"><span data-stu-id="fd1b2-142">Scale your service</span></span>
<span data-ttu-id="fd1b2-143">Det kan ta några minuter att skapa en tjänst (15 minuter eller mer beroende på nivån).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-143">It can take a few minutes to create a service (15 minutes or more depending on the tier).</span></span> <span data-ttu-id="fd1b2-144">När tjänsten har etablerats kan du skala den för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-144">After your service is provisioned, you can scale it to meet your needs.</span></span> <span data-ttu-id="fd1b2-145">Eftersom du har valt standardnivån för din Azure Search-tjänst kan du skala din tjänst i två dimensioner: repliker och partitioner.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-145">Because you chose the Standard tier for your Azure Search service, you can scale your service in two dimensions: replicas and partitions.</span></span> <span data-ttu-id="fd1b2-146">Hade du valt den grundläggande nivån, du kan bara lägga till repliker.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-146">Had you chosen the Basic tier, you can only add replicas.</span></span> <span data-ttu-id="fd1b2-147">Skalning är inte tillgängligt om du har etablerat tjänsten gratis.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-147">If you provisioned the free service, scale is not available.</span></span>

<span data-ttu-id="fd1b2-148">***Partitioner*** Tillåt din tjänst att lagra och söka igenom flera dokument.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-148">***Partitions*** allow your service to store and search through more documents.</span></span>

<span data-ttu-id="fd1b2-149">***Repliker*** Tillåt din tjänst att hantera en högre belastning av sökfrågor.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-149">***Replicas*** allow your service to handle a higher load of search queries.</span></span>

> [!Important]
> <span data-ttu-id="fd1b2-150">En tjänst måste ha [2 repliker för skrivskyddade SLA och 3 repliker för läsning och skrivning SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-150">A service must have [2 replicas for read-only SLA and 3 replicas for read/write SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

1. <span data-ttu-id="fd1b2-151">Gå till din sökning service bladet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-151">Go to your search service blade in the Azure portal.</span></span>
2. <span data-ttu-id="fd1b2-152">I det vänstra navigeringsfönstret, Välj **inställningar** > **skala**.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-152">In the left-navigation pane, select **Settings** > **Scale**.</span></span>
3. <span data-ttu-id="fd1b2-153">Använd slidebar om du vill lägga till repliker eller partitioner.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-153">Use the slidebar to add Replicas or Partitions.</span></span>

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> <span data-ttu-id="fd1b2-154">Varje nivå har olika [gränser](search-limits-quotas-capacity.md) på det totala antalet Search-enheter som tillåts i en enskild tjänst (repliker * partitioner = Totalt antal Search-enheter).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-154">Each tier has different [limits](search-limits-quotas-capacity.md) on the total number of Search Units allowed in a single service (Replicas * Partitions = Total Search Units).</span></span>

## <a name="when-to-add-a-second-service"></a><span data-ttu-id="fd1b2-155">När du ska lägga till en andra tjänst</span><span class="sxs-lookup"><span data-stu-id="fd1b2-155">When to add a second service</span></span>

<span data-ttu-id="fd1b2-156">En stor majoritet av kunder använder en tjänst som etablerats på en nivå som innehåller den [Högerklicka balans mellan resurser](search-sku-tier.md).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-156">A large majority of customers use just one service provisioned at a tier that provides the [right balance of resources](search-sku-tier.md).</span></span> <span data-ttu-id="fd1b2-157">En tjänst kan vara värd för flera index föremål för den [gränsvärden för nivån som du väljer](search-capacity-planning.md), med varje index isolerade från varandra.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-157">One service can host multiple indexes, subject to the [maximum limits of the tier you select](search-capacity-planning.md), with each index isolated from another.</span></span> <span data-ttu-id="fd1b2-158">I Azure Search dirigeras begäranden endast till ett index som minimerar risken för att hämta oavsiktligt eller avsiktligt data från andra index i samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-158">In Azure Search, requests can only be directed to one index, minimizing the chance of accidental or intentional data retrieval from other indexes in the same service.</span></span>

<span data-ttu-id="fd1b2-159">Även om de flesta kunder använder en tjänst, kan service-redundans vara nödvändigt om operativa krav för inkluderar följande:</span><span class="sxs-lookup"><span data-stu-id="fd1b2-159">Although most customers use just one service, service redundancy might be necessary if operational requirements include the following:</span></span>

+ <span data-ttu-id="fd1b2-160">Katastrofåterställning (data center avbrott).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-160">Disaster recovery (data center outage).</span></span> <span data-ttu-id="fd1b2-161">Azure Search ger inte några omedelbara växling vid fel vid ett avbrott.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-161">Azure Search does not provide instant failover in the event of an outage.</span></span> <span data-ttu-id="fd1b2-162">Vägledning och rekommendationer finns [tjänsten administration](search-manage.md).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-162">For recommendations and guidance, see [Service administration](search-manage.md).</span></span>
+ <span data-ttu-id="fd1b2-163">Din undersökning av multitenans modellering har fastställt att ytterligare tjänster är den optimala designen.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-163">Your investigation of multi-tenancy modeling has determined that additional services is the optimal design.</span></span> <span data-ttu-id="fd1b2-164">Mer information finns i [Design för multitenans](search-modeling-multitenant-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-164">For more information, see [Design for multi-tenancy](search-modeling-multitenant-saas-applications.md).</span></span>
+ <span data-ttu-id="fd1b2-165">Du kan behöva en instans av Azure Search i flera områden för att minimera svarstiden för ditt program internationell trafik för globalt distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-165">For globally deployed applications, you might require an instance of Azure Search in multiple regions to minimize latency of your application’s international traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="fd1b2-166">Du kan särskilja indexering och frågar arbetsbelastningar; i Azure Search Därför skulle du skapa flera tjänster för åtskilda arbetsbelastningar aldrig.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-166">In Azure Search, you cannot segregate indexing and querying workloads; thus, you would never create multiple services for segregated workloads.</span></span> <span data-ttu-id="fd1b2-167">Ett index efterfrågas alltid på tjänsten som den skapades (du kan inte skapa ett index i en tjänst och kopiera den till en annan).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-167">An index is always queried on the service in which it was created (you cannot create an index in one service and copy it to another).</span></span>
>

<span data-ttu-id="fd1b2-168">En andra tjänst krävs inte för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-168">A second service is not required for high availability.</span></span> <span data-ttu-id="fd1b2-169">Hög tillgänglighet för frågor uppnås när du använder 2 eller högre repliker i samma tjänst.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-169">High availability for queries is achieved when you use 2 or more replicas in the same service.</span></span> <span data-ttu-id="fd1b2-170">Replik uppdateringar är sekventiella, vilket innebär att minst en fungerar när en tjänstuppdatering lyfts.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-170">Replica updates are sequential, which means at least one is operational when a service update is rolled out.</span></span> <span data-ttu-id="fd1b2-171">Läs mer om drifttid [serviceavtal](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="fd1b2-171">For more information about uptime, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd1b2-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd1b2-172">Next steps</span></span>
<span data-ttu-id="fd1b2-173">När du har etablerat en Azure Search-tjänst, är du redo att [definiera ett index](search-what-is-an-index.md) så att du kan ladda upp och söka efter data.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-173">After provisioning an Azure Search service, you are ready to [define an index](search-what-is-an-index.md) so you can upload and search your data.</span></span>

<span data-ttu-id="fd1b2-174">Ange en URL för att komma åt tjänsten från kod eller skript (*Tjänstenamn*. search.windows.net) och en nyckel.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-174">To access the service from code or script, provide the URL (*service-name*.search.windows.net) and a key.</span></span> <span data-ttu-id="fd1b2-175">Admin nycklar ger fullständig åtkomst; frågan nycklar bevilja läsåtkomst.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-175">Admin keys grant full access; query keys grant read-only access.</span></span> <span data-ttu-id="fd1b2-176">Se [hur du använder Azure Search i .NET](search-howto-dotnet-sdk.md) att komma igång.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-176">See [How to use Azure Search in .NET](search-howto-dotnet-sdk.md) to get started.</span></span>

<span data-ttu-id="fd1b2-177">Se [bygga och fråga ditt första index](search-get-started-portal.md) en snabb genomgång av portal-baserade.</span><span class="sxs-lookup"><span data-stu-id="fd1b2-177">See [Build and query your first index](search-get-started-portal.md) for a quick portal-based tutorial.</span></span>

