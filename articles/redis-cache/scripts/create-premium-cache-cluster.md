---
title: aaaAzure CLI skriptexempel - skapa en Premium Azure Redis-Cache med kluster | Microsoft Docs
description: "Azure CLI-skript Sample - skapa en nivå för Premium Azure Redis-Cache med kluster"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: ca34d40059b282cb2abc7e3e2b8771226029744c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a><span data-ttu-id="7aeda-103">Skapa en Premium Azure Redis-Cache med kluster</span><span class="sxs-lookup"><span data-stu-id="7aeda-103">Create a Premium Azure Redis Cache with clustering</span></span>

<span data-ttu-id="7aeda-104">I detta scenario dig du hur toocreate 6 GB premiumnivån Azure Redis-Cache med kluster aktiverad och två delar.</span><span class="sxs-lookup"><span data-stu-id="7aeda-104">In this scenario, you learn how toocreate a 6 GB Premium tier Azure Redis Cache with clustering enabled and two shards.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="7aeda-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="7aeda-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="7aeda-106">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="7aeda-106">Script explanation</span></span>

<span data-ttu-id="7aeda-107">Det här skriptet använder följande kommandon toocreate hello en resursgrupp och Premium-nivån redis-cache med kluster aktivera.</span><span class="sxs-lookup"><span data-stu-id="7aeda-107">This script uses hello following commands toocreate a resource group and a Premium tier redis cache with clustering enable.</span></span> <span data-ttu-id="7aeda-108">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7aeda-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7aeda-109">Kommando</span><span class="sxs-lookup"><span data-stu-id="7aeda-109">Command</span></span> | <span data-ttu-id="7aeda-110">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7aeda-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7aeda-111">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="7aeda-111">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7aeda-112">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="7aeda-112">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7aeda-113">Skapa AZ redis</span><span class="sxs-lookup"><span data-stu-id="7aeda-113">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="7aeda-114">Skapa Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="7aeda-114">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="7aeda-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7aeda-115">Next steps</span></span>

<span data-ttu-id="7aeda-116">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7aeda-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7aeda-117">Ytterligare Azure Redis-Cache CLI skriptexempel finns i hello [dokumentation för Azure Redis-Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7aeda-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
