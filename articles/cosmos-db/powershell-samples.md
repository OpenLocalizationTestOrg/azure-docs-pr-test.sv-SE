---
title: "aaaAzure PowerShell-exempel för Azure Cosmos DB | Microsoft Docs"
description: Azure PowerShell-exempel - skript toohelp du skapar och hanterar Azure DB som Cosmos-konton.
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
ms.openlocfilehash: 8815e775f720226e98cc8dcf7743e481c213272b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a><span data-ttu-id="d3fab-103">Azure PowerShell-exempel för Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d3fab-103">Azure PowerShell samples for Azure Cosmos DB</span></span>

<span data-ttu-id="d3fab-104">hello följande tabell innehåller länkar toosample Azure PowerShell-skript för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d3fab-104">hello following table includes links toosample Azure PowerShell scripts for Azure Cosmos DB.</span></span>

| |  |
|---|---|
|<span data-ttu-id="d3fab-105">**Skapa ett Azure DB som Cosmos-konto**</span><span class="sxs-lookup"><span data-stu-id="d3fab-105">**Create an Azure Cosmos DB account**</span></span>||
|[<span data-ttu-id="d3fab-106">Skapa ett DocumentDB-API-konto</span><span class="sxs-lookup"><span data-stu-id="d3fab-106">Create a DocumentDB API account</span></span>](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="d3fab-107">Skapar en enda Azure Cosmos DB konto toouse hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="d3fab-107">Creates a single Azure Cosmos DB account toouse with hello DocumentDB API.</span></span> |
|<span data-ttu-id="d3fab-108">**Skala Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="d3fab-108">**Scale Azure Cosmos DB**</span></span>||
|[<span data-ttu-id="d3fab-109">Replikera Azure DB som Cosmos-kontot i flera områden och konfigurera redundans prioriteter</span><span class="sxs-lookup"><span data-stu-id="d3fab-109">Replicate Azure Cosmos DB account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="d3fab-110">Replikerar globalt kontodata i flera regioner med angivna redundans prioritet.</span><span class="sxs-lookup"><span data-stu-id="d3fab-110">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="d3fab-111">**Skydda Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="d3fab-111">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="d3fab-112">Hämta nycklar</span><span class="sxs-lookup"><span data-stu-id="d3fab-112">Get account keys</span></span>](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="d3fab-113">Hämtar hello primära och sekundära master skrivning och primära och sekundära skrivskyddade nycklar för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="d3fab-113">Gets hello primary and secondary master write keys and primary and secondary read-only keys for hello account.</span></span>|
| [<span data-ttu-id="d3fab-114">Hämta anslutningssträngen för MongoDB</span><span class="sxs-lookup"><span data-stu-id="d3fab-114">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="d3fab-115">Hämtar hello anslutning sträng tooconnect din MongoDB app tooyour Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="d3fab-115">Gets hello connection string tooconnect your MongoDB app tooyour Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="d3fab-116">Återskapa nycklar</span><span class="sxs-lookup"><span data-stu-id="d3fab-116">Regenerate account keys</span></span>](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="d3fab-117">Återskapar hello master eller skrivskyddad nyckel för hello-konto.</span><span class="sxs-lookup"><span data-stu-id="d3fab-117">Regenerates hello master or read-only key for hello account.</span></span>|
|[<span data-ttu-id="d3fab-118">Skapa en brandvägg</span><span class="sxs-lookup"><span data-stu-id="d3fab-118">Create a firewall</span></span>](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="d3fab-119">Skapar ett inkommande IP-access control princip toolimit toohello åtkomstkonto från godkända uppsättning datorer eller molntjänster.</span><span class="sxs-lookup"><span data-stu-id="d3fab-119">Creates an inbound IP access control policy toolimit access toohello account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="d3fab-120">**Hög tillgänglighet, katastrofåterställning, säkerhetskopiering och återställning**</span><span class="sxs-lookup"><span data-stu-id="d3fab-120">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="d3fab-121">Konfigurera principen för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="d3fab-121">Configure failover policy</span></span>](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="d3fab-122">Anger hello redundans prioritet för varje region där hello-konto replikeras.</span><span class="sxs-lookup"><span data-stu-id="d3fab-122">Sets hello failover priority of each region in which hello account is replicated.</span></span>|
|||