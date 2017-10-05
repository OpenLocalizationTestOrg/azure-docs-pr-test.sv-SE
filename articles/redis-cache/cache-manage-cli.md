---
title: "Hantera Azure Redis-Cache med hjälp av Azure CLI | Microsoft Docs"
description: "Lär dig hur du installerar Azure CLI på valfri plattform, hur du ansluter till ditt Azure-konto och skapa och hantera ett Redis-cache från Azure CLI."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: ba078a870a3998568170cc197bd6698b97b7fadb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a><span data-ttu-id="c0c1e-103">Skapa och hantera Azure Redis-Cache med hjälp av Azure-kommandoradsgränssnittet (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="c0c1e-103">How to create and manage Azure Redis Cache using the Azure Command-Line Interface (Azure CLI)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0c1e-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0c1e-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="c0c1e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c0c1e-105">Azure CLI</span></span>](cache-manage-cli.md)
>
>

<span data-ttu-id="c0c1e-106">Azure CLI är ett bra sätt att hantera dina Azure-infrastrukturen från valfri plattform.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-106">The Azure CLI is a great way to manage your Azure infrastructure from any platform.</span></span> <span data-ttu-id="c0c1e-107">Den här artikeln visar hur du skapar och hanterar din Azure Redis-Cache-instanser som använder Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-107">This article shows you how to create and manage your Azure Redis Cache instances using the Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="c0c1e-108">Den här artikeln gäller för en tidigare version av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-108">This article applies to a previous version of Azure CLI.</span></span> <span data-ttu-id="c0c1e-109">De senaste Azure CLI 2.0 exempelskript finns [Azure Redis-CLI cache exempel](cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c0c1e-109">For the latest Azure CLI 2.0 sample scripts, see [Azure CLI Redis cache samples](cli-samples.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c0c1e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c0c1e-110">Prerequisites</span></span>
<span data-ttu-id="c0c1e-111">Om du vill skapa och hantera Azure Redis-Cache-instanser som använder Azure CLI, måste du slutföra följande steg.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-111">To create and manage Azure Redis Cache instances using Azure CLI, you must complete the following steps.</span></span>

* <span data-ttu-id="c0c1e-112">Du måste ha ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-112">You must have an Azure account.</span></span> <span data-ttu-id="c0c1e-113">Om du inte har någon, kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) i en liten stund.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-113">If you don't have one, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a few moments.</span></span>
* <span data-ttu-id="c0c1e-114">[Installera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c0c1e-114">[Install the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="c0c1e-115">Anslut Azure CLI-installationen med ett personligt konto i Azure, eller med ett arbets eller skolan Azure-konto och logga in från Azure CLI med hjälp av den `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-115">Connect your Azure CLI installation with a personal Azure account, or with a work or school Azure account, and log in from the Azure CLI using the `azure login` command.</span></span> <span data-ttu-id="c0c1e-116">Om du vill förstå skillnaderna och väljer, se [Anslut till en Azure-prenumeration från Azure-kommandoradsgränssnittet (Azure CLI)](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="c0c1e-116">To understand the differences and choose, see [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="c0c1e-117">Innan du kan köra följande kommandon för att växla Azure CLI i Resource Manager-läge genom att köra den `azure config mode arm` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-117">Before running any of the following commands, switch the Azure CLI into Resource Manager mode by running the `azure config mode arm` command.</span></span> <span data-ttu-id="c0c1e-118">Mer information finns i [använda Azure CLI för att hantera Azure-resurser och resursgrupper](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c0c1e-118">For more information, see [Use the Azure CLI to manage Azure resources and resource groups](../xplat-cli-azure-resource-manager.md).</span></span>

## <a name="redis-cache-properties"></a><span data-ttu-id="c0c1e-119">Redis-Cache-egenskaper</span><span class="sxs-lookup"><span data-stu-id="c0c1e-119">Redis Cache properties</span></span>
<span data-ttu-id="c0c1e-120">Följande egenskaper används när du skapar och uppdaterar Redis-Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-120">The following properties are used when creating and updating Redis Cache instances.</span></span>

| <span data-ttu-id="c0c1e-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c0c1e-121">Property</span></span> | <span data-ttu-id="c0c1e-122">Växel</span><span class="sxs-lookup"><span data-stu-id="c0c1e-122">Switch</span></span> | <span data-ttu-id="c0c1e-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c0c1e-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c0c1e-124">namn</span><span class="sxs-lookup"><span data-stu-id="c0c1e-124">name</span></span> |<span data-ttu-id="c0c1e-125">-n,--namn</span><span class="sxs-lookup"><span data-stu-id="c0c1e-125">-n, --name</span></span> |<span data-ttu-id="c0c1e-126">Namnet på Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-126">Name of the Redis Cache.</span></span> |
| <span data-ttu-id="c0c1e-127">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c0c1e-127">resource group</span></span> |<span data-ttu-id="c0c1e-128">-g,--resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-128">-g, --resource-group</span></span> |<span data-ttu-id="c0c1e-129">Namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-129">Name of the Resource Group.</span></span> |
| <span data-ttu-id="c0c1e-130">location</span><span class="sxs-lookup"><span data-stu-id="c0c1e-130">location</span></span> |<span data-ttu-id="c0c1e-131">-l,--plats</span><span class="sxs-lookup"><span data-stu-id="c0c1e-131">-l, --location</span></span> |<span data-ttu-id="c0c1e-132">Plats för att skapa cache.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-132">Location to create cache.</span></span> |
| <span data-ttu-id="c0c1e-133">Storlek</span><span class="sxs-lookup"><span data-stu-id="c0c1e-133">size</span></span> |<span data-ttu-id="c0c1e-134">-z,--storlek</span><span class="sxs-lookup"><span data-stu-id="c0c1e-134">-z, --size</span></span> |<span data-ttu-id="c0c1e-135">Storleken på Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-135">Size of the Redis Cache.</span></span> <span data-ttu-id="c0c1e-136">Giltiga värden: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span><span class="sxs-lookup"><span data-stu-id="c0c1e-136">Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
| <span data-ttu-id="c0c1e-137">SKU</span><span class="sxs-lookup"><span data-stu-id="c0c1e-137">sku</span></span> |<span data-ttu-id="c0c1e-138">-x,--sku</span><span class="sxs-lookup"><span data-stu-id="c0c1e-138">-x, --sku</span></span> |<span data-ttu-id="c0c1e-139">Redis-SKU.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-139">Redis SKU.</span></span> <span data-ttu-id="c0c1e-140">Ska vara någon av: [Basic, Standard, Premium]</span><span class="sxs-lookup"><span data-stu-id="c0c1e-140">Should be one of : [Basic, Standard, Premium]</span></span> |
| <span data-ttu-id="c0c1e-141">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="c0c1e-141">EnableNonSslPort</span></span> |<span data-ttu-id="c0c1e-142">e-,--aktivera icke-ssl-port</span><span class="sxs-lookup"><span data-stu-id="c0c1e-142">-e, --enable-non-ssl-port</span></span> |<span data-ttu-id="c0c1e-143">EnableNonSslPort-egenskapen för Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-143">EnableNonSslPort property of the Redis Cache.</span></span> <span data-ttu-id="c0c1e-144">Lägg till den här flaggan om du vill aktivera icke-SSL-Port för ditt cacheminne</span><span class="sxs-lookup"><span data-stu-id="c0c1e-144">Add this flag if you want to enable the Non SSL Port for your cache</span></span> |
| <span data-ttu-id="c0c1e-145">Redis-konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0c1e-145">Redis Configuration</span></span> |<span data-ttu-id="c0c1e-146">-c,--redis-konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0c1e-146">-c, --redis-configuration</span></span> |<span data-ttu-id="c0c1e-147">Redis-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-147">Redis Configuration.</span></span> <span data-ttu-id="c0c1e-148">Ange en JSON-formaterad sträng konfigurationsnycklar och värden här.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-148">Enter a JSON formatted string of configuration keys and values here.</span></span> <span data-ttu-id="c0c1e-149">Format ”: {” ”:” ””, ””: ”}”</span><span class="sxs-lookup"><span data-stu-id="c0c1e-149">Format:"{"":"","":""}"</span></span> |
| <span data-ttu-id="c0c1e-150">Redis-konfiguration</span><span class="sxs-lookup"><span data-stu-id="c0c1e-150">Redis Configuration</span></span> |<span data-ttu-id="c0c1e-151">-f,--redis-konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="c0c1e-151">-f, --redis-configuration-file</span></span> |<span data-ttu-id="c0c1e-152">Redis-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-152">Redis Configuration.</span></span> <span data-ttu-id="c0c1e-153">Ange en sökväg till en fil som innehåller konfigurationsnycklar och värden här.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-153">Enter the path of a file containing configuration keys and values here.</span></span> <span data-ttu-id="c0c1e-154">Formatet för posten fil: {””: ”” ”,” ”:”}</span><span class="sxs-lookup"><span data-stu-id="c0c1e-154">Format for the file entry: {"":"","":""}</span></span> |
| <span data-ttu-id="c0c1e-155">Fragmentera antal</span><span class="sxs-lookup"><span data-stu-id="c0c1e-155">Shard Count</span></span> |<span data-ttu-id="c0c1e-156">-r,--Fragmentera antal</span><span class="sxs-lookup"><span data-stu-id="c0c1e-156">-r, --shard-count</span></span> |<span data-ttu-id="c0c1e-157">Antalet delar för att skapa på Premium klustret Cache med kluster.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-157">Number of Shards to create on a Premium Cluster Cache with clustering.</span></span> |
| <span data-ttu-id="c0c1e-158">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="c0c1e-158">Virtual Network</span></span> |<span data-ttu-id="c0c1e-159">-v,--virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="c0c1e-159">-v, --virtual-network</span></span> |<span data-ttu-id="c0c1e-160">När värd för ditt cacheminne i ett virtuellt nätverk, anger du exakt ARM resurs-ID för det virtuella nätverket för att distribuera redis-cache i.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-160">When hosting your cache in a VNET, specifies the exact ARM resource ID of the virtual network to deploy the redis cache in.</span></span> <span data-ttu-id="c0c1e-161">Exempel på format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="c0c1e-161">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="c0c1e-162">Nyckeltyp</span><span class="sxs-lookup"><span data-stu-id="c0c1e-162">key type</span></span> |<span data-ttu-id="c0c1e-163">-t,--typ av nyckel</span><span class="sxs-lookup"><span data-stu-id="c0c1e-163">-t, --key-type</span></span> |<span data-ttu-id="c0c1e-164">Typ av nyckel att förnya.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-164">Type of key to renew.</span></span> <span data-ttu-id="c0c1e-165">Giltiga värden: [primära, sekundära]</span><span class="sxs-lookup"><span data-stu-id="c0c1e-165">Valid values: [Primary, Secondary]</span></span> |
| <span data-ttu-id="c0c1e-166">StaticIP</span><span class="sxs-lookup"><span data-stu-id="c0c1e-166">StaticIP</span></span> |<span data-ttu-id="c0c1e-167">-p,--statiska ip-< statiska IP-></span><span class="sxs-lookup"><span data-stu-id="c0c1e-167">-p, --static-ip <static-ip></span></span> |<span data-ttu-id="c0c1e-168">När värd för ditt cacheminne i ett virtuellt nätverk, anger du en unik IP-adress i undernätet för cachen.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-168">When hosting your cache in a VNET, specifies a unique IP address in the subnet for the cache.</span></span> <span data-ttu-id="c0c1e-169">Om inte är en valt i undernätet.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-169">If not provided, one is chosen for you from the subnet.</span></span> |
| <span data-ttu-id="c0c1e-170">Undernät</span><span class="sxs-lookup"><span data-stu-id="c0c1e-170">Subnet</span></span> |<span data-ttu-id="c0c1e-171">t,--undernät<subnet></span><span class="sxs-lookup"><span data-stu-id="c0c1e-171">t, --subnet <subnet></span></span> |<span data-ttu-id="c0c1e-172">När värd för ditt cacheminne i ett virtuellt nätverk, anger du namnet på undernätet där du vill distribuera cachen.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-172">When hosting your cache in a VNET, specifies the name of the subnet in which to deploy the cache.</span></span> |
| <span data-ttu-id="c0c1e-173">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="c0c1e-173">VirtualNetwork</span></span> |<span data-ttu-id="c0c1e-174">-v,--virtuellt nätverk < virtuella nätverk ></span><span class="sxs-lookup"><span data-stu-id="c0c1e-174">-v, --virtual-network <virtual-network></span></span> |<span data-ttu-id="c0c1e-175">När värd för ditt cacheminne i ett virtuellt nätverk, anger du exakt ARM resurs-ID för det virtuella nätverket för att distribuera redis-cache i.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-175">When hosting your cache in a VNET, specifies the exact ARM resource ID of the virtual network to deploy the redis cache in.</span></span> <span data-ttu-id="c0c1e-176">Exempel på format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="c0c1e-176">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="c0c1e-177">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0c1e-177">Subscription</span></span> |<span data-ttu-id="c0c1e-178">-s, -prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0c1e-178">-s, --subscription</span></span> |<span data-ttu-id="c0c1e-179">Prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-179">The subscription identifier.</span></span> |

## <a name="see-all-redis-cache-commands"></a><span data-ttu-id="c0c1e-180">Visa alla kommandon som Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-180">See all Redis Cache commands</span></span>
<span data-ttu-id="c0c1e-181">Om du vill se alla Redis-Cache-kommandon och deras parametrar i `azure rediscache -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-181">To see all Redis Cache commands and their parameters, use the `azure rediscache -h` command.</span></span>

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a><span data-ttu-id="c0c1e-182">Skapa en Redis-cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-182">Create a Redis Cache</span></span>
<span data-ttu-id="c0c1e-183">Om du vill skapa ett Redis-Cache, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-183">To create a Redis Cache, use the following command:</span></span>

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

<span data-ttu-id="c0c1e-184">Mer information om det här kommandot Kör den `azure rediscache create -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-184">For more information about this command, run the `azure rediscache create -h` command.</span></span>

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a><span data-ttu-id="c0c1e-185">Ta bort en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-185">Delete an existing Redis Cache</span></span>
<span data-ttu-id="c0c1e-186">Om du vill ta bort ett Redis-Cache, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-186">To delete a Redis Cache, use the following command:</span></span>

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

<span data-ttu-id="c0c1e-187">Mer information om det här kommandot Kör den `azure rediscache delete -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-187">For more information about this command, run the `azure rediscache delete -h` command.</span></span>

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a><span data-ttu-id="c0c1e-188">Visa en lista med alla Redis-cache i din prenumeration eller resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c0c1e-188">List all Redis Caches within your Subscription or Resource Group</span></span>
<span data-ttu-id="c0c1e-189">Om du vill visa en lista med alla Redis-cache i din prenumeration eller resursgrupp, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-189">To list all Redis Caches within your Subscription or Resource Group, use the following command:</span></span>

    azure rediscache list [options]

<span data-ttu-id="c0c1e-190">Mer information om det här kommandot Kör den `azure rediscache list -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-190">For more information about this command, run the `azure rediscache list -h` command.</span></span>

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a><span data-ttu-id="c0c1e-191">Visa egenskaperna för en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-191">Show properties of an existing Redis Cache</span></span>
<span data-ttu-id="c0c1e-192">Om du vill visa egenskaperna för en befintlig Redis-Cache, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-192">To show properties of an existing Redis Cache, use the following command:</span></span>

    azure rediscache show [--name <name> --resource-group <resource-group>]

<span data-ttu-id="c0c1e-193">Mer information om det här kommandot Kör den `azure rediscache show -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-193">For more information about this command, run the `azure rediscache show -h` command.</span></span>

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a><span data-ttu-id="c0c1e-194">Ändra inställningar för en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-194">Change settings of an existing Redis Cache</span></span>
<span data-ttu-id="c0c1e-195">Om du vill ändra inställningarna för en befintlig Redis-Cache, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-195">To change settings of an existing Redis Cache, use the following command:</span></span>

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

<span data-ttu-id="c0c1e-196">Mer information om det här kommandot Kör den `azure rediscache set -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-196">For more information about this command, run the `azure rediscache set -h` command.</span></span>

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a><span data-ttu-id="c0c1e-197">Förnya autentiseringsnyckel för en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-197">Renew the authentication key for an existing Redis Cache</span></span>
<span data-ttu-id="c0c1e-198">Om du vill förnya autentiseringsnyckeln för en befintlig Redis-Cache, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-198">To renew the authentication key for an existing Redis Cache, use the following command:</span></span>

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

<span data-ttu-id="c0c1e-199">Ange `Primary` eller `Secondary` för `key-type`.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-199">Specify `Primary` or `Secondary` for `key-type`.</span></span>

<span data-ttu-id="c0c1e-200">Mer information om det här kommandot Kör den `azure rediscache renew-key -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-200">For more information about this command, run the `azure rediscache renew-key -h` command.</span></span>

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a><span data-ttu-id="c0c1e-201">Lista över primära och sekundära nycklarna i en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c0c1e-201">List Primary and Secondary keys of an existing Redis Cache</span></span>
<span data-ttu-id="c0c1e-202">Lista över primära och sekundära nycklarna för en befintlig Redis-Cache, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0c1e-202">To list Primary and Secondary keys of an existing Redis Cache, use the following command:</span></span>

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

<span data-ttu-id="c0c1e-203">Mer information om det här kommandot Kör den `azure rediscache list-keys -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0c1e-203">For more information about this command, run the `azure rediscache list-keys -h` command.</span></span>

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
