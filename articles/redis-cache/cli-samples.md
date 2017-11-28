---
title: aaaAzure Redis-CLI cache prover | Microsoft Docs
description: "Azure CLI-exempel för Azure Redis-Cache."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8d2de145-50c0-4f76-bf8f-fdf679f03698
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: a2eb6f7267951faca599a43ff29b46beb05fea6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-redis-cache"></a><span data-ttu-id="a1307-103">Azure CLI-exempel för Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="a1307-103">Azure CLI Samples for Azure Redis Cache</span></span>

<span data-ttu-id="a1307-104">hello innehåller följande tabell länkar toobash skript som skapats med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a1307-104">hello following table includes links toobash scripts built using hello Azure CLI.</span></span>

| | |
|---|---|
|<span data-ttu-id="a1307-105">**Skapa cachen**</span><span class="sxs-lookup"><span data-stu-id="a1307-105">**Create cache**</span></span>||
| [<span data-ttu-id="a1307-106">Skapa ett cacheminne</span><span class="sxs-lookup"><span data-stu-id="a1307-106">Create a cache</span></span>](./scripts/create-cache.md) | <span data-ttu-id="a1307-107">Skapar en resursgrupp och en grundläggande nivå Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="a1307-107">Creates a resource group and a basic tier Azure Redis Cache.</span></span> |
| [<span data-ttu-id="a1307-108">Skapa en premium-cache med kluster</span><span class="sxs-lookup"><span data-stu-id="a1307-108">Create a premium cache with clustering</span></span>](./scripts/create-premium-cache-cluster.md) | <span data-ttu-id="a1307-109">Skapar en resursgrupp och premium-nivån cache med aktiverad klustring.</span><span class="sxs-lookup"><span data-stu-id="a1307-109">Creates a resource group and a premium tier cache with clustering enabled.</span></span>|
| [<span data-ttu-id="a1307-110">Hämta information om cache</span><span class="sxs-lookup"><span data-stu-id="a1307-110">Get cache details</span></span>](./scripts/show-cache.md) | <span data-ttu-id="a1307-111">Hämtar information om Azure Redis-Cache-instans, inklusive Etableringsstatus.</span><span class="sxs-lookup"><span data-stu-id="a1307-111">Gets details of an Azure Redis Cache instance, including provisioning status.</span></span> |
| [<span data-ttu-id="a1307-112">Hämta hello värdnamnet, portarna och nycklarna</span><span class="sxs-lookup"><span data-stu-id="a1307-112">Get hello hostname, ports, and keys</span></span>](./scripts/cache-keys-ports.md) | <span data-ttu-id="a1307-113">Hämtar hello värdnamnet, portarna och nycklarna för en Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="a1307-113">Gets hello hostname, ports, and keys for an Azure Redis Cache instance.</span></span> |
|<span data-ttu-id="a1307-114">**Webbprogrammet plus cache**</span><span class="sxs-lookup"><span data-stu-id="a1307-114">**Web app plus cache**</span></span>||
| [<span data-ttu-id="a1307-115">Ansluta en web app tooa redis-cache</span><span class="sxs-lookup"><span data-stu-id="a1307-115">Connect a web app tooa redis cache</span></span>](./../app-service-web/scripts/app-service-cli-app-service-redis.md) | <span data-ttu-id="a1307-116">Skapar ett Azure webbapp och ett redis-cache sedan lägger till hello redis information toohello appinställningar.</span><span class="sxs-lookup"><span data-stu-id="a1307-116">Creates an Azure web app and a redis cache, then adds hello redis connection details toohello app settings.</span></span> |
|<span data-ttu-id="a1307-117">**Ta bort cache**</span><span class="sxs-lookup"><span data-stu-id="a1307-117">**Delete cache**</span></span>||
| [<span data-ttu-id="a1307-118">Ta bort en cache</span><span class="sxs-lookup"><span data-stu-id="a1307-118">Delete a cache</span></span>](./scripts/delete-cache.md) | <span data-ttu-id="a1307-119">Tar bort en instans av Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="a1307-119">Deletes an Azure Redis Cache instance</span></span>  |
| | |

<span data-ttu-id="a1307-120">Läs mer om Azure CLI 2.0 [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) och [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a1307-120">For more information about Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>