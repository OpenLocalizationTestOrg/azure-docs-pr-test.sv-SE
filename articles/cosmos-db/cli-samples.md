---
title: "aaaAzure CLI prover för Azure Cosmos DB | Microsoft Docs"
description: "Azure CLI-exempel - skapa och hantera Azure Cosmos DB konton, databaser, behållare, regioner och brandväggar."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: d6eefc3274e0b66eec4e69166bb7d4ddd58a522e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a><span data-ttu-id="b6995-103">Azure CLI-exempel för Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b6995-103">Azure CLI samples for Azure Cosmos DB</span></span>

<span data-ttu-id="b6995-104">hello följande tabell innehåller länkar toosample Azure CLI-skript för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b6995-104">hello following table includes links toosample Azure CLI scripts for Azure Cosmos DB.</span></span> <span data-ttu-id="b6995-105">För referenssidor för alla Azure Cosmos DB CLI-kommandon finns i hello [referens för Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).</span><span class="sxs-lookup"><span data-stu-id="b6995-105">Reference pages for all Azure Cosmos DB CLI commands are available in hello [Azure CLI 2.0 Reference](https://docs.microsoft.com/cli/azure/cosmosdb).</span></span>

| |  |
|---|---|
|<span data-ttu-id="b6995-106">**Skapa Azure DB som Cosmos-kontot, databas och behållare**</span><span class="sxs-lookup"><span data-stu-id="b6995-106">**Create Azure Cosmos DB account, database, and containers**</span></span>||
|[<span data-ttu-id="b6995-107">Skapa ett DocumentDB-API, diagram och tabellen API-konto</span><span class="sxs-lookup"><span data-stu-id="b6995-107">Create a DocumentDB API, Graph, or Table API account</span></span>](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="b6995-108">Skapar en enda Azure Cosmos DB API-kontot, databas och behållare för användning med hello DocumentDB diagrammet eller API: er för tabellen.</span><span class="sxs-lookup"><span data-stu-id="b6995-108">Creates a single Azure Cosmos DB API account, database, and container for use with hello DocumentDB, Graph, or Table APIs.</span></span> |
| [<span data-ttu-id="b6995-109">Skapa ett konto för MongoDB-API</span><span class="sxs-lookup"><span data-stu-id="b6995-109">Create a MongoDB API account</span></span>](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="b6995-110">Skapar en enda Azure Cosmos DB MongoDB API kontot, databas och samling.</span><span class="sxs-lookup"><span data-stu-id="b6995-110">Creates a single Azure Cosmos DB MongoDB API account, database, and collection.</span></span> |
|<span data-ttu-id="b6995-111">**Skala Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="b6995-111">**Scale Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="b6995-112">Skala behållaren genomflöde</span><span class="sxs-lookup"><span data-stu-id="b6995-112">Scale container throughput</span></span>](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="b6995-113">Ändringar hello etableras througput i en behållare.</span><span class="sxs-lookup"><span data-stu-id="b6995-113">Changes hello provisioned througput on a container.</span></span>|
|[<span data-ttu-id="b6995-114">Replikera Azure Cosmos DB databaskonto i flera områden och konfigurera redundans prioriteter</span><span class="sxs-lookup"><span data-stu-id="b6995-114">Replicate Azure Cosmos DB database account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="b6995-115">Replikerar globalt kontodata i flera regioner med angivna redundans prioritet.</span><span class="sxs-lookup"><span data-stu-id="b6995-115">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="b6995-116">**Skydda Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="b6995-116">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="b6995-117">Hämta nycklar</span><span class="sxs-lookup"><span data-stu-id="b6995-117">Get account keys</span></span>](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="b6995-118">Hämtar hello primära och sekundära master skrivning och primära och sekundära skrivskyddade nycklar för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="b6995-118">Gets hello primary and secondary master write keys and primary and secondary read-only keys for hello account.</span></span>|
| [<span data-ttu-id="b6995-119">Hämta anslutningssträngen för MongoDB</span><span class="sxs-lookup"><span data-stu-id="b6995-119">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="b6995-120">Hämtar hello anslutning sträng tooconnect din MongoDB app tooyour Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="b6995-120">Gets hello connection string tooconnect your MongoDB app tooyour Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="b6995-121">Återskapa nycklar</span><span class="sxs-lookup"><span data-stu-id="b6995-121">Regenerate account keys</span></span>](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="b6995-122">Återskapar hello master eller skrivskyddad nyckel för hello-konto.</span><span class="sxs-lookup"><span data-stu-id="b6995-122">Regenerates hello master or read-only key for hello account.</span></span>|
|[<span data-ttu-id="b6995-123">Skapa en brandvägg</span><span class="sxs-lookup"><span data-stu-id="b6995-123">Create a firewall</span></span>](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="b6995-124">Skapar ett inkommande IP-access control princip toolimit toohello åtkomstkonto från godkända uppsättning datorer eller molntjänster.</span><span class="sxs-lookup"><span data-stu-id="b6995-124">Creates an inbound IP access control policy toolimit access toohello account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="b6995-125">**Hög tillgänglighet, katastrofåterställning, säkerhetskopiering och återställning**</span><span class="sxs-lookup"><span data-stu-id="b6995-125">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="b6995-126">Konfigurera principen för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="b6995-126">Configure failover policy</span></span>](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="b6995-127">Anger hello redundans prioritet för varje region där hello-konto replikeras.</span><span class="sxs-lookup"><span data-stu-id="b6995-127">Sets hello failover priority of each region in which hello account is replicated.</span></span>|
|<span data-ttu-id="b6995-128">**Ansluta Azure Cosmos DB till resurser**</span><span class="sxs-lookup"><span data-stu-id="b6995-128">**Connect Azure Cosmos DB to resources**</span></span>||
|[<span data-ttu-id="b6995-129">Ansluta en web app tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b6995-129">Connect a web app tooAzure Cosmos DB</span></span>](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="b6995-130">Skapa och ansluta en Azure Cosmos-DB-databas och ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="b6995-130">Create and connect an Azure Cosmos DB database and an Azure web app.</span></span>|
|||
