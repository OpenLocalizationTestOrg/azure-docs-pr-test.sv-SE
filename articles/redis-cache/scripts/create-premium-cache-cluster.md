---
title: Azure CLI-skript Sample - skapa en Premium Azure Redis-Cache med kluster | Microsoft Docs
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
ms.openlocfilehash: 87d0fe4c3eaa8f7b75343a36a069ecdac8241d74
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a><span data-ttu-id="2a794-103">Skapa en Premium Azure Redis-Cache med kluster</span><span class="sxs-lookup"><span data-stu-id="2a794-103">Create a Premium Azure Redis Cache with clustering</span></span>

<span data-ttu-id="2a794-104">I detta scenario dig du hur du skapar en 6 GB premiumnivån Azure Redis-Cache med aktiverad klustring och två delar.</span><span class="sxs-lookup"><span data-stu-id="2a794-104">In this scenario, you learn how to create a 6 GB Premium tier Azure Redis Cache with clustering enabled and two shards.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="2a794-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2a794-105">Sample script</span></span>

<span data-ttu-id="2a794-106">[!code-azurecli[huvudsakliga](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis-Cache")]</span><span class="sxs-lookup"><span data-stu-id="2a794-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="2a794-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2a794-107">Script explanation</span></span>

<span data-ttu-id="2a794-108">Det här skriptet använder följande kommandon för att skapa en resursgrupp och Premium-nivån redis-cache med aktivera-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a794-108">This script uses the following commands to create a resource group and a Premium tier redis cache with clustering enable.</span></span> <span data-ttu-id="2a794-109">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2a794-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2a794-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="2a794-110">Command</span></span> | <span data-ttu-id="2a794-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2a794-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2a794-112">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="2a794-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2a794-113">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="2a794-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2a794-114">Skapa AZ redis</span><span class="sxs-lookup"><span data-stu-id="2a794-114">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="2a794-115">Skapa Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="2a794-115">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="2a794-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a794-116">Next steps</span></span>

<span data-ttu-id="2a794-117">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2a794-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2a794-118">Ytterligare Azure Redis-Cache CLI skriptexempel finns i den [dokumentation för Azure Redis-Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2a794-118">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>