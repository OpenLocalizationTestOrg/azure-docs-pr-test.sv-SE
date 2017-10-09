---
title: "aaaAzure CLI skriptexempel - Get hello värdnamnet, portarna och nycklarna för Azure Redis-Cache | Microsoft Docs"
description: "Azure CLI skriptexempel - Get hello värdnamnet, portarna och nycklarna för en instans av Azure Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: e6e794558087d6568438c439e2bf99fc46eeb8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-hostname-ports-and-keys-for-azure-redis-cache"></a><span data-ttu-id="6d9b6-103">Hämta hello värdnamnet, portarna och nycklarna för Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="6d9b6-103">Get hello hostname, ports, and keys for Azure Redis Cache</span></span>

<span data-ttu-id="6d9b6-104">I det här scenariot kan du lära dig hur tooretrieve hello värdnamnet, portarna och nycklarna användas tooconnect tooan Azure Redis-cacheinstansen.</span><span class="sxs-lookup"><span data-stu-id="6d9b6-104">In this scenario, you learn how tooretrieve hello hostname, ports, and keys used tooconnect tooan Azure Redis Cache instance.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="6d9b6-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="6d9b6-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a><span data-ttu-id="6d9b6-106">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="6d9b6-106">Script explanation</span></span>

<span data-ttu-id="6d9b6-107">Det här skriptet använder hello följande kommandon tooretrieve hello värdnamn, nycklar och portar för en Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="6d9b6-107">This script uses hello following commands tooretrieve hello hostname, keys, and ports of an Azure Redis Cache instance.</span></span> <span data-ttu-id="6d9b6-108">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="6d9b6-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6d9b6-109">Kommando</span><span class="sxs-lookup"><span data-stu-id="6d9b6-109">Command</span></span> | <span data-ttu-id="6d9b6-110">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6d9b6-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6d9b6-111">AZ redis visa</span><span class="sxs-lookup"><span data-stu-id="6d9b6-111">az redis show</span></span>](https://docs.microsoft.com/cli/azure/redis#show) | <span data-ttu-id="6d9b6-112">Hämta information om Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="6d9b6-112">Retrieve details of an Azure Redis Cache instance.</span></span> |
| [<span data-ttu-id="6d9b6-113">AZ redis lista nycklar</span><span class="sxs-lookup"><span data-stu-id="6d9b6-113">az redis list-keys</span></span>](https://docs.microsoft.com/cli/azure/redis#list-keys) | <span data-ttu-id="6d9b6-114">Hämta åtkomstnycklarna för en Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="6d9b6-114">Retrieve access keys for an Azure Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="6d9b6-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6d9b6-115">Next steps</span></span>

<span data-ttu-id="6d9b6-116">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6d9b6-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6d9b6-117">Ytterligare Azure Redis-Cache CLI skriptexempel finns i hello [dokumentation för Azure Redis-Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6d9b6-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
