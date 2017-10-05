---
title: "Etablera genomströmning för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du ställer in etablerat dataflöde för din Azure Cosmos DB containsers, samlingar, diagram och tabeller."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: d541bb19ba7e5ecb44c9fe91b1e232d4d9c2170e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a><span data-ttu-id="c11d0-103">Ange dataflöde för Azure DB som Cosmos-behållare</span><span class="sxs-lookup"><span data-stu-id="c11d0-103">Set throughput for Azure Cosmos DB containers</span></span>

<span data-ttu-id="c11d0-104">Du kan ange dataflöde för din Azure DB som Cosmos-behållare i Azure-portalen eller genom att använda klient-SDK: er.</span><span class="sxs-lookup"><span data-stu-id="c11d0-104">You can set throughput for your Azure Cosmos DB containers in the Azure portal or by using the client SDKs.</span></span> 

<span data-ttu-id="c11d0-105">I följande tabell visas genomströmning för behållare:</span><span class="sxs-lookup"><span data-stu-id="c11d0-105">The following table lists the throughput available for containers:</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><span data-ttu-id="c11d0-106"><strong>Enskild Partition behållare</strong></span><span class="sxs-lookup"><span data-stu-id="c11d0-106"><strong>Single Partition Container</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="c11d0-107"><strong>Partitionerade behållare</strong></span><span class="sxs-lookup"><span data-stu-id="c11d0-107"><strong>Partitioned Container</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="c11d0-108">Minsta dataflöde</span><span class="sxs-lookup"><span data-stu-id="c11d0-108">Minimum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="c11d0-109">400 frågeenheter per sekund</span><span class="sxs-lookup"><span data-stu-id="c11d0-109">400 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="c11d0-110">2 500 frågeenheter per sekund</span><span class="sxs-lookup"><span data-stu-id="c11d0-110">2,500 request units per second</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="c11d0-111">Maximalt dataflöde</span><span class="sxs-lookup"><span data-stu-id="c11d0-111">Maximum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="c11d0-112">10 000 frågeenheter per sekund</span><span class="sxs-lookup"><span data-stu-id="c11d0-112">10,000 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="c11d0-113">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="c11d0-113">Unlimited</span></span></p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a><span data-ttu-id="c11d0-114">Ange genomflödet med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="c11d0-114">To set the throughput by using the Azure portal</span></span>

1. <span data-ttu-id="c11d0-115">Öppna i ett nytt fönster i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c11d0-115">In a new window, open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c11d0-116">Klicka på fältet till vänster **Azure Cosmos DB**, eller klicka på **fler tjänster** längst ned, bläddrar sedan till **databaser**, och klicka sedan på **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="c11d0-116">On the left bar, click **Azure Cosmos DB**, or click **More Services** at the bottom, then scroll to **Databases**, and then click **Azure Cosmos DB**.</span></span>
3. <span data-ttu-id="c11d0-117">Välj Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="c11d0-117">Select your Cosmos DB account.</span></span>
4. <span data-ttu-id="c11d0-118">I det nya fönstret, klickar du på **Data Explorer (förhandsgranskning)** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c11d0-118">In the new window, click **Data Explorer (Preview)** in the navigation menu.</span></span>
5. <span data-ttu-id="c11d0-119">Expandera din databas och en behållare i det nya fönstret, och klicka sedan på **skala & inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c11d0-119">In the new window, expand your database and container and then click **Scale & Settings**.</span></span>
6. <span data-ttu-id="c11d0-120">I det nya fönstret, skriver du det nya värdet genomflöde i den **genomströmning** rutan och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c11d0-120">In the new window, type the new throughput value in the **Throughput** box, and then click **Save**.</span></span>

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-documentdb-api-for-net"></a><span data-ttu-id="c11d0-121">Ange genomflödet med DocumentDB-API för .NET</span><span class="sxs-lookup"><span data-stu-id="c11d0-121">To set the throughput by using the DocumentDB API for .NET</span></span>

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a><span data-ttu-id="c11d0-122">Genomströmning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="c11d0-122">Throughput FAQ</span></span>

<span data-ttu-id="c11d0-123">**Kan jag in min genomströmning till mindre än 400 RU/s?**</span><span class="sxs-lookup"><span data-stu-id="c11d0-123">**Can I set my throughput to less than 400 RU/s?**</span></span>

<span data-ttu-id="c11d0-124">400 RU/s är den minsta genomflödet som är tillgängliga på Cosmos DB enskilda partitionssamlingar (2500 RU/s är minst för partitionerade samlingar).</span><span class="sxs-lookup"><span data-stu-id="c11d0-124">400 RU/s is the minimum throughput available on Cosmos DB single partition collections (2500 RU/s is the minimum for partitioned collections).</span></span> <span data-ttu-id="c11d0-125">Begära enheter anges i intervall om 100 RU/s men dataflöde kan inte anges till 100 RU/s eller ett värde som är mindre än 400 RU/s.</span><span class="sxs-lookup"><span data-stu-id="c11d0-125">Request units are set in 100 RU/s intervals, but throughput cannot be set to 100 RU/s or any value smaller than 400 RU/s.</span></span> <span data-ttu-id="c11d0-126">Om du letar efter ett kostnadseffektivt sätt att utveckla och testa Cosmos DB, kan du använda den kostnadsfria [Azure Cosmos DB emulatorn](local-emulator.md), som du kan distribuera lokalt utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="c11d0-126">If you're looking for a cost effective method to develop and test Cosmos DB, you can use the free [Azure Cosmos DB Emulator](local-emulator.md), which you can deploy locally at no cost.</span></span> 

<span data-ttu-id="c11d0-127">**Hur ställer jag througput med MongoDB-API**</span><span class="sxs-lookup"><span data-stu-id="c11d0-127">**How do I set througput using the MongoDB API?**</span></span>

<span data-ttu-id="c11d0-128">Det finns inget MongoDB-API-tillägg för att ange genomflöde.</span><span class="sxs-lookup"><span data-stu-id="c11d0-128">There's no MongoDB API extension to set throughput.</span></span> <span data-ttu-id="c11d0-129">Rekommendationen är att använda DocumentDB-API som visas i [ange genomflödet med DocumentDB-API för .NET](#set-throughput-sdk).</span><span class="sxs-lookup"><span data-stu-id="c11d0-129">The recommendation is to use the DocumentDB API, as shown in [To set the throughput by using the DocumentDB API for .NET](#set-throughput-sdk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c11d0-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c11d0-130">Next steps</span></span>

<span data-ttu-id="c11d0-131">Läs mer om etablering och pågående planeten skalning med Cosmos-DB i [partitionering och skalning med Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="c11d0-131">To learn more about provisioning and going planet-scale with Cosmos DB, see [Partitioning and scaling with Cosmos DB](partition-data.md).</span></span>
