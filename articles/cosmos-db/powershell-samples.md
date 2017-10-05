---
title: "Azure PowerShell-exempel för Azure Cosmos DB | Microsoft Docs"
description: "Azure PowerShell-exempel - skript som hjälper dig att skapa och hantera Azure DB som Cosmos-konton."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7698e03c0dc8d1c6d1e926f45e903a909bfd0c93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a><span data-ttu-id="ecdc9-103">Azure PowerShell-exempel för Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ecdc9-103">Azure PowerShell samples for Azure Cosmos DB</span></span>

<span data-ttu-id="ecdc9-104">Följande tabell innehåller länkar till exempel Azure PowerShell-skript för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-104">The following table includes links to sample Azure PowerShell scripts for Azure Cosmos DB.</span></span>

| |  |
|---|---|
|<span data-ttu-id="ecdc9-105">**Skapa ett Azure DB som Cosmos-konto**</span><span class="sxs-lookup"><span data-stu-id="ecdc9-105">**Create an Azure Cosmos DB account**</span></span>||
|[<span data-ttu-id="ecdc9-106">Skapa ett DocumentDB-API-konto</span><span class="sxs-lookup"><span data-stu-id="ecdc9-106">Create a DocumentDB API account</span></span>](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ecdc9-107">Skapar en enda Azure Cosmos DB-konto som ska användas med DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-107">Creates a single Azure Cosmos DB account to use with the DocumentDB API.</span></span> |
|<span data-ttu-id="ecdc9-108">**Skala Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="ecdc9-108">**Scale Azure Cosmos DB**</span></span>||
|[<span data-ttu-id="ecdc9-109">Replikera Azure DB som Cosmos-kontot i flera områden och konfigurera redundans prioriteter</span><span class="sxs-lookup"><span data-stu-id="ecdc9-109">Replicate Azure Cosmos DB account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="ecdc9-110">Replikerar globalt kontodata i flera regioner med angivna redundans prioritet.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-110">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="ecdc9-111">**Skydda Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="ecdc9-111">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="ecdc9-112">Hämta nycklar</span><span class="sxs-lookup"><span data-stu-id="ecdc9-112">Get account keys</span></span>](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ecdc9-113">Hämtar den primära och sekundära master skrivning och primära och sekundära skrivskyddade nycklar för kontot.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-113">Gets the primary and secondary master write keys and primary and secondary read-only keys for the account.</span></span>|
| [<span data-ttu-id="ecdc9-114">Hämta anslutningssträngen för MongoDB</span><span class="sxs-lookup"><span data-stu-id="ecdc9-114">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ecdc9-115">Hämtar anslutningssträngen till Anslut MongoDB-appen till ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-115">Gets the connection string to connect your MongoDB app to your Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="ecdc9-116">Återskapa nycklar</span><span class="sxs-lookup"><span data-stu-id="ecdc9-116">Regenerate account keys</span></span>](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="ecdc9-117">Återskapar nyckeln master eller skrivskyddat för kontot.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-117">Regenerates the master or read-only key for the account.</span></span>|
|[<span data-ttu-id="ecdc9-118">Skapa en brandvägg</span><span class="sxs-lookup"><span data-stu-id="ecdc9-118">Create a firewall</span></span>](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ecdc9-119">Skapar en inkommande IP-principer för åtkomstkontroll för att begränsa åtkomst till kontot från en godkänd uppsättning datorer eller molntjänster.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-119">Creates an inbound IP access control policy to limit access to the account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="ecdc9-120">**Hög tillgänglighet, katastrofåterställning, säkerhetskopiering och återställning**</span><span class="sxs-lookup"><span data-stu-id="ecdc9-120">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="ecdc9-121">Konfigurera principen för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="ecdc9-121">Configure failover policy</span></span>](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="ecdc9-122">Anger redundans prioriteten för varje region där kontot replikeras.</span><span class="sxs-lookup"><span data-stu-id="ecdc9-122">Sets the failover priority of each region in which the account is replicated.</span></span>|
|||