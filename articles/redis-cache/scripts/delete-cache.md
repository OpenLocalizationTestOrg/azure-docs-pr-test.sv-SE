---
title: Azure CLI skriptexempel - ta bort en Azure Redis-Cache | Microsoft Docs
description: Azure CLI skriptexempel - ta bort en Azure Redis-Cache
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: f959823b3a7c5b0262f693ecad1e6efc4eec4f35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="delete-an-azure-redis-cache"></a><span data-ttu-id="418dd-103">Ta bort en Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="418dd-103">Delete an Azure Redis Cache</span></span>

<span data-ttu-id="418dd-104">Lär dig hur du tar bort en Azure Redis-Cache i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="418dd-104">In this scenario, you learn how to delete an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="418dd-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="418dd-105">Sample script</span></span>

<span data-ttu-id="418dd-106">[!code-azurecli[huvudsakliga](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis-Cache")]</span><span class="sxs-lookup"><span data-stu-id="418dd-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="418dd-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="418dd-107">Script explanation</span></span>

<span data-ttu-id="418dd-108">Det här skriptet använder följande kommandon för att ta bort en Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="418dd-108">This script uses the following commands to delete an Azure Redis Cache instance.</span></span> <span data-ttu-id="418dd-109">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="418dd-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="418dd-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="418dd-110">Command</span></span> | <span data-ttu-id="418dd-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="418dd-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="418dd-112">ta bort AZ-redis</span><span class="sxs-lookup"><span data-stu-id="418dd-112">az redis delete</span></span>](https://docs.microsoft.com/cli/azure/redis#delete) | <span data-ttu-id="418dd-113">Ta bort Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="418dd-113">Delete Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="418dd-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="418dd-114">Next steps</span></span>

<span data-ttu-id="418dd-115">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="418dd-115">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="418dd-116">Ytterligare Azure Redis-Cache CLI skriptexempel finns i den [dokumentation för Azure Redis-Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="418dd-116">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>