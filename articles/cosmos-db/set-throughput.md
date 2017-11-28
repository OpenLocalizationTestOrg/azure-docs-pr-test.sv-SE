---
title: "aaaProvision genomströmning för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur tooset etablerat dataflöde för din Azure Cosmos DB containsers, samlingar, diagram och tabeller."
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
ms.openlocfilehash: c143f4aace466b7109168a50e2eb80ddeca6400e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a><span data-ttu-id="1b985-103">Ange dataflöde för Azure DB som Cosmos-behållare</span><span class="sxs-lookup"><span data-stu-id="1b985-103">Set throughput for Azure Cosmos DB containers</span></span>

<span data-ttu-id="1b985-104">Du kan ange dataflöde för din Azure DB som Cosmos-behållare i hello Azure-portalen eller med hjälp av hello klient-SDK:.</span><span class="sxs-lookup"><span data-stu-id="1b985-104">You can set throughput for your Azure Cosmos DB containers in hello Azure portal or by using hello client SDKs.</span></span> 

<span data-ttu-id="1b985-105">hello i den följande tabellen listas hello genomströmning tillgänglig för behållare:</span><span class="sxs-lookup"><span data-stu-id="1b985-105">hello following table lists hello throughput available for containers:</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><span data-ttu-id="1b985-106"><strong>Enskild Partition behållare</strong></span><span class="sxs-lookup"><span data-stu-id="1b985-106"><strong>Single Partition Container</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1b985-107"><strong>Partitionerade behållare</strong></span><span class="sxs-lookup"><span data-stu-id="1b985-107"><strong>Partitioned Container</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1b985-108">Minsta dataflöde</span><span class="sxs-lookup"><span data-stu-id="1b985-108">Minimum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1b985-109">400 frågeenheter per sekund</span><span class="sxs-lookup"><span data-stu-id="1b985-109">400 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1b985-110">2 500 frågeenheter per sekund</span><span class="sxs-lookup"><span data-stu-id="1b985-110">2,500 request units per second</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="1b985-111">Maximalt dataflöde</span><span class="sxs-lookup"><span data-stu-id="1b985-111">Maximum Throughput</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1b985-112">10 000 frågeenheter per sekund</span><span class="sxs-lookup"><span data-stu-id="1b985-112">10,000 request units per second</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="1b985-113">Obegränsat</span><span class="sxs-lookup"><span data-stu-id="1b985-113">Unlimited</span></span></p></td>
        </tr>
    </tbody>
</table>

## <a name="tooset-hello-throughput-by-using-hello-azure-portal"></a><span data-ttu-id="1b985-114">tooset hello dataflöde med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1b985-114">tooset hello throughput by using hello Azure portal</span></span>

1. <span data-ttu-id="1b985-115">Öppna i ett nytt fönster hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1b985-115">In a new window, open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1b985-116">Klicka på vänster hello-fältet **Azure Cosmos DB**, eller klicka på **fler tjänster** hello längst ned i Bläddra för**databaser**, och klicka sedan på **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="1b985-116">On hello left bar, click **Azure Cosmos DB**, or click **More Services** at hello bottom, then scroll too**Databases**, and then click **Azure Cosmos DB**.</span></span>
3. <span data-ttu-id="1b985-117">Välj Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="1b985-117">Select your Cosmos DB account.</span></span>
4. <span data-ttu-id="1b985-118">I hello nya fönstret, klickar du på **Data Explorer (förhandsgranskning)** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1b985-118">In hello new window, click **Data Explorer (Preview)** in hello navigation menu.</span></span>
5. <span data-ttu-id="1b985-119">Expandera din databas och behållare hello nya fönstret, och klicka sedan på **skala & inställningar**.</span><span class="sxs-lookup"><span data-stu-id="1b985-119">In hello new window, expand your database and container and then click **Scale & Settings**.</span></span>
6. <span data-ttu-id="1b985-120">Ange hello nya genomströmning värde i hello i hello nya fönstret, **genomströmning** rutan och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="1b985-120">In hello new window, type hello new throughput value in hello **Throughput** box, and then click **Save**.</span></span>

<a id="set-throughput-sdk"></a>

## <a name="tooset-hello-throughput-by-using-hello-documentdb-api-for-net"></a><span data-ttu-id="1b985-121">tooset hello dataflöde med hello DocumentDB API för .NET</span><span class="sxs-lookup"><span data-stu-id="1b985-121">tooset hello throughput by using hello DocumentDB API for .NET</span></span>

```C#
//Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput toohello new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a><span data-ttu-id="1b985-122">Genomströmning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="1b985-122">Throughput FAQ</span></span>

<span data-ttu-id="1b985-123">**Kan jag in min genomströmning tooless än 400 RU/s?**</span><span class="sxs-lookup"><span data-stu-id="1b985-123">**Can I set my throughput tooless than 400 RU/s?**</span></span>

<span data-ttu-id="1b985-124">400 RU/s är hello minsta dataflöde på Cosmos DB enskilda partitionssamlingar (2500 RU/s är hello minsta för partitionerade samlingar).</span><span class="sxs-lookup"><span data-stu-id="1b985-124">400 RU/s is hello minimum throughput available on Cosmos DB single partition collections (2500 RU/s is hello minimum for partitioned collections).</span></span> <span data-ttu-id="1b985-125">Begära enheter anges i intervall om 100 RU/s men genomströmning går inte att ange too100 RU/s eller ett värde mindre än 400 RU/s.</span><span class="sxs-lookup"><span data-stu-id="1b985-125">Request units are set in 100 RU/s intervals, but throughput cannot be set too100 RU/s or any value smaller than 400 RU/s.</span></span> <span data-ttu-id="1b985-126">Om du letar efter ett kostnadseffektivt metoden toodevelop och testa Cosmos DB, kan du använda hello ledigt [Azure Cosmos DB emulatorn](local-emulator.md), som du kan distribuera lokalt utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="1b985-126">If you're looking for a cost effective method toodevelop and test Cosmos DB, you can use hello free [Azure Cosmos DB Emulator](local-emulator.md), which you can deploy locally at no cost.</span></span> 

<span data-ttu-id="1b985-127">**Hur ställer jag in througput med hello MongoDB API?**</span><span class="sxs-lookup"><span data-stu-id="1b985-127">**How do I set througput using hello MongoDB API?**</span></span>

<span data-ttu-id="1b985-128">Det finns inga MongoDB API-tillägg tooset genomflöde.</span><span class="sxs-lookup"><span data-stu-id="1b985-128">There's no MongoDB API extension tooset throughput.</span></span> <span data-ttu-id="1b985-129">hello rekommendation är toouse hello DocumentDB API, som visas i [tooset hello dataflöde med hello DocumentDB API för .NET](#set-throughput-sdk).</span><span class="sxs-lookup"><span data-stu-id="1b985-129">hello recommendation is toouse hello DocumentDB API, as shown in [tooset hello throughput by using hello DocumentDB API for .NET](#set-throughput-sdk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b985-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b985-130">Next steps</span></span>

<span data-ttu-id="1b985-131">toolearn mer om etablering och pågående planeten skalning med Cosmos DB finns [partitionering och skalning med Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="1b985-131">toolearn more about provisioning and going planet-scale with Cosmos DB, see [Partitioning and scaling with Cosmos DB](partition-data.md).</span></span>
