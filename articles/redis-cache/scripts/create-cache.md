---
title: aaaAzure CLI skriptexempel - skapa en Azure Redis-Cache | Microsoft Docs
description: Azure CLI Script exempel - skapa en Azure Redis-Cache
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: afd7f6e0-9297-4c98-a95e-597be939cef7
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: 85b007a426fbd4752034ec8663835963d140dd75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-redis-cache"></a><span data-ttu-id="e472e-103">Skapa en Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e472e-103">Create an Azure Redis Cache</span></span>

<span data-ttu-id="e472e-104">I det här scenariot kan du lära dig hur toocreate ett Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e472e-104">In this scenario, you learn how toocreate an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="e472e-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e472e-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="e472e-106">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e472e-106">Script explanation</span></span>

<span data-ttu-id="e472e-107">Det här skriptet använder följande kommandon toocreate hello en resursgrupp och ett redis-cache.</span><span class="sxs-lookup"><span data-stu-id="e472e-107">This script uses hello following commands toocreate a resource group and a redis cache.</span></span> <span data-ttu-id="e472e-108">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e472e-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e472e-109">Kommando</span><span class="sxs-lookup"><span data-stu-id="e472e-109">Command</span></span> | <span data-ttu-id="e472e-110">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e472e-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e472e-111">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="e472e-111">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e472e-112">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e472e-112">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e472e-113">Skapa AZ redis</span><span class="sxs-lookup"><span data-stu-id="e472e-113">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="e472e-114">Skapa Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="e472e-114">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="e472e-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e472e-115">Next steps</span></span>

<span data-ttu-id="e472e-116">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e472e-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e472e-117">Ytterligare Azure Redis-Cache CLI skriptexempel finns i hello [dokumentation för Azure Redis-Cache](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e472e-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
