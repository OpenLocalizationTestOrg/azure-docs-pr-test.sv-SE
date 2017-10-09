---
title: "aaaManage Azure Redis-Cache med hjälp av Azure CLI | Microsoft Docs"
description: "Lär dig hur tooinstall hello Azure CLI på valfri plattform, hur toouse den tooconnect tooyour Azure-konto och hur toocreate och hantera ett Redis-cache från hello Azure CLI."
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
ms.openlocfilehash: e8ee30090133e6b4edea93dcd13fd171e75989bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-azure-redis-cache-using-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="4560d-103">Hur toocreate och hantera Azure Redis-Cache med hjälp av hello Azure-kommandoradsgränssnittet (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="4560d-103">How toocreate and manage Azure Redis Cache using hello Azure Command-Line Interface (Azure CLI)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4560d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4560d-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="4560d-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4560d-105">Azure CLI</span></span>](cache-manage-cli.md)
>
>

<span data-ttu-id="4560d-106">hello Azure CLI är ett bra sätt toomanage din Azure-infrastrukturen från valfri plattform.</span><span class="sxs-lookup"><span data-stu-id="4560d-106">hello Azure CLI is a great way toomanage your Azure infrastructure from any platform.</span></span> <span data-ttu-id="4560d-107">Den här artikeln beskrivs hur du toocreate och hantera dina Azure Redis-Cache-instanser som använder hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="4560d-107">This article shows you how toocreate and manage your Azure Redis Cache instances using hello Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="4560d-108">Den här artikeln gäller tooa tidigare version av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="4560d-108">This article applies tooa previous version of Azure CLI.</span></span> <span data-ttu-id="4560d-109">Hello senaste Azure CLI 2.0 skriptexempel finns [Azure Redis-CLI cache exempel](cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4560d-109">For hello latest Azure CLI 2.0 sample scripts, see [Azure CLI Redis cache samples](cli-samples.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4560d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4560d-110">Prerequisites</span></span>
<span data-ttu-id="4560d-111">toocreate och hantera Azure Redis-Cache-instanser som använder Azure CLI, måste du slutföra följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="4560d-111">toocreate and manage Azure Redis Cache instances using Azure CLI, you must complete hello following steps.</span></span>

* <span data-ttu-id="4560d-112">Du måste ha ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="4560d-112">You must have an Azure account.</span></span> <span data-ttu-id="4560d-113">Om du inte har någon, kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) i en liten stund.</span><span class="sxs-lookup"><span data-stu-id="4560d-113">If you don't have one, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a few moments.</span></span>
* <span data-ttu-id="4560d-114">[Installera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4560d-114">[Install hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="4560d-115">Anslut Azure CLI-installationen med ett personligt konto i Azure, eller med ett arbets eller skolan Azure-konto och logga in från hello Azure CLI med hello `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-115">Connect your Azure CLI installation with a personal Azure account, or with a work or school Azure account, and log in from hello Azure CLI using hello `azure login` command.</span></span> <span data-ttu-id="4560d-116">toounderstand hello skillnader och välj, se [ansluta tooan Azure-prenumeration från hello Azure-kommandoradsgränssnittet (Azure CLI)](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="4560d-116">toounderstand hello differences and choose, see [Connect tooan Azure subscription from hello Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="4560d-117">Innan du kan köra följande kommandon hello växla hello Azure CLI i Resource Manager-läge genom att köra hello `azure config mode arm` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-117">Before running any of hello following commands, switch hello Azure CLI into Resource Manager mode by running hello `azure config mode arm` command.</span></span> <span data-ttu-id="4560d-118">Mer information finns i [använda hello Azure CLI toomanage Azure resurser och resursgrupper](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4560d-118">For more information, see [Use hello Azure CLI toomanage Azure resources and resource groups](../xplat-cli-azure-resource-manager.md).</span></span>

## <a name="redis-cache-properties"></a><span data-ttu-id="4560d-119">Redis-Cache-egenskaper</span><span class="sxs-lookup"><span data-stu-id="4560d-119">Redis Cache properties</span></span>
<span data-ttu-id="4560d-120">hello används följande egenskaper när du skapar och uppdaterar Redis-Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="4560d-120">hello following properties are used when creating and updating Redis Cache instances.</span></span>

| <span data-ttu-id="4560d-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4560d-121">Property</span></span> | <span data-ttu-id="4560d-122">Växel</span><span class="sxs-lookup"><span data-stu-id="4560d-122">Switch</span></span> | <span data-ttu-id="4560d-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4560d-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4560d-124">namn</span><span class="sxs-lookup"><span data-stu-id="4560d-124">name</span></span> |<span data-ttu-id="4560d-125">-n,--namn</span><span class="sxs-lookup"><span data-stu-id="4560d-125">-n, --name</span></span> |<span data-ttu-id="4560d-126">Namnet på hello Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="4560d-126">Name of hello Redis Cache.</span></span> |
| <span data-ttu-id="4560d-127">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="4560d-127">resource group</span></span> |<span data-ttu-id="4560d-128">-g,--resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4560d-128">-g, --resource-group</span></span> |<span data-ttu-id="4560d-129">Namnet på hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4560d-129">Name of hello Resource Group.</span></span> |
| <span data-ttu-id="4560d-130">location</span><span class="sxs-lookup"><span data-stu-id="4560d-130">location</span></span> |<span data-ttu-id="4560d-131">-l,--plats</span><span class="sxs-lookup"><span data-stu-id="4560d-131">-l, --location</span></span> |<span data-ttu-id="4560d-132">Plats toocreate cache.</span><span class="sxs-lookup"><span data-stu-id="4560d-132">Location toocreate cache.</span></span> |
| <span data-ttu-id="4560d-133">Storlek</span><span class="sxs-lookup"><span data-stu-id="4560d-133">size</span></span> |<span data-ttu-id="4560d-134">-z,--storlek</span><span class="sxs-lookup"><span data-stu-id="4560d-134">-z, --size</span></span> |<span data-ttu-id="4560d-135">Storleken på hello Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="4560d-135">Size of hello Redis Cache.</span></span> <span data-ttu-id="4560d-136">Giltiga värden: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span><span class="sxs-lookup"><span data-stu-id="4560d-136">Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
| <span data-ttu-id="4560d-137">SKU</span><span class="sxs-lookup"><span data-stu-id="4560d-137">sku</span></span> |<span data-ttu-id="4560d-138">-x,--sku</span><span class="sxs-lookup"><span data-stu-id="4560d-138">-x, --sku</span></span> |<span data-ttu-id="4560d-139">Redis-SKU.</span><span class="sxs-lookup"><span data-stu-id="4560d-139">Redis SKU.</span></span> <span data-ttu-id="4560d-140">Ska vara någon av: [Basic, Standard, Premium]</span><span class="sxs-lookup"><span data-stu-id="4560d-140">Should be one of : [Basic, Standard, Premium]</span></span> |
| <span data-ttu-id="4560d-141">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="4560d-141">EnableNonSslPort</span></span> |<span data-ttu-id="4560d-142">e-,--aktivera icke-ssl-port</span><span class="sxs-lookup"><span data-stu-id="4560d-142">-e, --enable-non-ssl-port</span></span> |<span data-ttu-id="4560d-143">EnableNonSslPort egenskap för hello Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="4560d-143">EnableNonSslPort property of hello Redis Cache.</span></span> <span data-ttu-id="4560d-144">Lägg till den här flaggan om du vill tooenable hello inte SSL-Port för ditt cacheminne</span><span class="sxs-lookup"><span data-stu-id="4560d-144">Add this flag if you want tooenable hello Non SSL Port for your cache</span></span> |
| <span data-ttu-id="4560d-145">Redis-konfiguration</span><span class="sxs-lookup"><span data-stu-id="4560d-145">Redis Configuration</span></span> |<span data-ttu-id="4560d-146">-c,--redis-konfiguration</span><span class="sxs-lookup"><span data-stu-id="4560d-146">-c, --redis-configuration</span></span> |<span data-ttu-id="4560d-147">Redis-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4560d-147">Redis Configuration.</span></span> <span data-ttu-id="4560d-148">Ange en JSON-formaterad sträng konfigurationsnycklar och värden här.</span><span class="sxs-lookup"><span data-stu-id="4560d-148">Enter a JSON formatted string of configuration keys and values here.</span></span> <span data-ttu-id="4560d-149">Format ”: {” ”:” ””, ””: ”}”</span><span class="sxs-lookup"><span data-stu-id="4560d-149">Format:"{"":"","":""}"</span></span> |
| <span data-ttu-id="4560d-150">Redis-konfiguration</span><span class="sxs-lookup"><span data-stu-id="4560d-150">Redis Configuration</span></span> |<span data-ttu-id="4560d-151">-f,--redis-konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="4560d-151">-f, --redis-configuration-file</span></span> |<span data-ttu-id="4560d-152">Redis-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4560d-152">Redis Configuration.</span></span> <span data-ttu-id="4560d-153">Ange hello sökväg till en fil som innehåller konfigurationsnycklar och värden här.</span><span class="sxs-lookup"><span data-stu-id="4560d-153">Enter hello path of a file containing configuration keys and values here.</span></span> <span data-ttu-id="4560d-154">Format för hello filpost: {””: ”” ”,” ”:”}</span><span class="sxs-lookup"><span data-stu-id="4560d-154">Format for hello file entry: {"":"","":""}</span></span> |
| <span data-ttu-id="4560d-155">Fragmentera antal</span><span class="sxs-lookup"><span data-stu-id="4560d-155">Shard Count</span></span> |<span data-ttu-id="4560d-156">-r,--Fragmentera antal</span><span class="sxs-lookup"><span data-stu-id="4560d-156">-r, --shard-count</span></span> |<span data-ttu-id="4560d-157">Antal Shards toocreate på Premium klustret Cache med kluster.</span><span class="sxs-lookup"><span data-stu-id="4560d-157">Number of Shards toocreate on a Premium Cluster Cache with clustering.</span></span> |
| <span data-ttu-id="4560d-158">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="4560d-158">Virtual Network</span></span> |<span data-ttu-id="4560d-159">-v,--virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="4560d-159">-v, --virtual-network</span></span> |<span data-ttu-id="4560d-160">När värd för ditt cacheminne i ett VNET anger hello exakt ARM resurs-ID för hello virtuellt nätverk toodeploy hello redis-cache i.</span><span class="sxs-lookup"><span data-stu-id="4560d-160">When hosting your cache in a VNET, specifies hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in.</span></span> <span data-ttu-id="4560d-161">Exempel på format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="4560d-161">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="4560d-162">Nyckeltyp</span><span class="sxs-lookup"><span data-stu-id="4560d-162">key type</span></span> |<span data-ttu-id="4560d-163">-t,--typ av nyckel</span><span class="sxs-lookup"><span data-stu-id="4560d-163">-t, --key-type</span></span> |<span data-ttu-id="4560d-164">Typ av nyckel toorenew.</span><span class="sxs-lookup"><span data-stu-id="4560d-164">Type of key toorenew.</span></span> <span data-ttu-id="4560d-165">Giltiga värden: [primära, sekundära]</span><span class="sxs-lookup"><span data-stu-id="4560d-165">Valid values: [Primary, Secondary]</span></span> |
| <span data-ttu-id="4560d-166">StaticIP</span><span class="sxs-lookup"><span data-stu-id="4560d-166">StaticIP</span></span> |<span data-ttu-id="4560d-167">-p,--statiska ip-< statiska IP-></span><span class="sxs-lookup"><span data-stu-id="4560d-167">-p, --static-ip <static-ip></span></span> |<span data-ttu-id="4560d-168">När värd för ditt cacheminne i ett virtuellt nätverk, anger du en unik IP-adress i hello undernät för hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="4560d-168">When hosting your cache in a VNET, specifies a unique IP address in hello subnet for hello cache.</span></span> <span data-ttu-id="4560d-169">Om inte är en valt från hello undernät.</span><span class="sxs-lookup"><span data-stu-id="4560d-169">If not provided, one is chosen for you from hello subnet.</span></span> |
| <span data-ttu-id="4560d-170">Undernät</span><span class="sxs-lookup"><span data-stu-id="4560d-170">Subnet</span></span> |<span data-ttu-id="4560d-171">t,--undernät<subnet></span><span class="sxs-lookup"><span data-stu-id="4560d-171">t, --subnet <subnet></span></span> |<span data-ttu-id="4560d-172">När värd för ditt cacheminne i ett virtuellt nätverk, anger hello hello undernät i vilken toodeploy hello cache.</span><span class="sxs-lookup"><span data-stu-id="4560d-172">When hosting your cache in a VNET, specifies hello name of hello subnet in which toodeploy hello cache.</span></span> |
| <span data-ttu-id="4560d-173">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="4560d-173">VirtualNetwork</span></span> |<span data-ttu-id="4560d-174">-v,--virtuellt nätverk < virtuella nätverk ></span><span class="sxs-lookup"><span data-stu-id="4560d-174">-v, --virtual-network <virtual-network></span></span> |<span data-ttu-id="4560d-175">När värd för ditt cacheminne i ett VNET anger hello exakt ARM resurs-ID för hello virtuellt nätverk toodeploy hello redis-cache i.</span><span class="sxs-lookup"><span data-stu-id="4560d-175">When hosting your cache in a VNET, specifies hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in.</span></span> <span data-ttu-id="4560d-176">Exempel på format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="4560d-176">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="4560d-177">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="4560d-177">Subscription</span></span> |<span data-ttu-id="4560d-178">-s, -prenumeration</span><span class="sxs-lookup"><span data-stu-id="4560d-178">-s, --subscription</span></span> |<span data-ttu-id="4560d-179">hello prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="4560d-179">hello subscription identifier.</span></span> |

## <a name="see-all-redis-cache-commands"></a><span data-ttu-id="4560d-180">Visa alla kommandon som Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="4560d-180">See all Redis Cache commands</span></span>
<span data-ttu-id="4560d-181">toosee alla Redis-Cache-kommandon och deras parametrar använder hello `azure rediscache -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-181">toosee all Redis Cache commands and their parameters, use hello `azure rediscache -h` command.</span></span>

    C:\>azure rediscache -h
    help:    Commands toomanage your Azure Redis Cache(s)
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
    help:    Renew hello authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a><span data-ttu-id="4560d-182">Skapa en Redis-cache</span><span class="sxs-lookup"><span data-stu-id="4560d-182">Create a Redis Cache</span></span>
<span data-ttu-id="4560d-183">toocreate Redis-Cache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-183">toocreate a Redis Cache, use hello following command:</span></span>

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

<span data-ttu-id="4560d-184">Mer information om det här kommandot Kör hello `azure rediscache create -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-184">For more information about this command, run hello `azure rediscache create -h` command.</span></span>

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
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -l, --location <location>                                Location toocreate cache.
    help:      -z, --size <size>                                        Size of hello Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of hello Redis Cache. Add this flag if you want tooenable hello Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here. Format for hello file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards toocreate on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a><span data-ttu-id="4560d-185">Ta bort en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="4560d-185">Delete an existing Redis Cache</span></span>
<span data-ttu-id="4560d-186">toodelete Redis-Cache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-186">toodelete a Redis Cache, use hello following command:</span></span>

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

<span data-ttu-id="4560d-187">Mer information om det här kommandot Kör hello `azure rediscache delete -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-187">For more information about this command, run hello `azure rediscache delete -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which hello cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a><span data-ttu-id="4560d-188">Visa en lista med alla Redis-cache i din prenumeration eller resursgrupp</span><span class="sxs-lookup"><span data-stu-id="4560d-188">List all Redis Caches within your Subscription or Resource Group</span></span>
<span data-ttu-id="4560d-189">toolist alla Redis-cache i din prenumeration eller resursgrupp, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-189">toolist all Redis Caches within your Subscription or Resource Group, use hello following command:</span></span>

    azure rediscache list [options]

<span data-ttu-id="4560d-190">Mer information om det här kommandot Kör hello `azure rediscache list -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-190">For more information about this command, run hello `azure rediscache list -h` command.</span></span>

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
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a><span data-ttu-id="4560d-191">Visa egenskaperna för en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="4560d-191">Show properties of an existing Redis Cache</span></span>
<span data-ttu-id="4560d-192">tooshow egenskaper för en befintlig Redis-Cache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-192">tooshow properties of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache show [--name <name> --resource-group <resource-group>]

<span data-ttu-id="4560d-193">Mer information om det här kommandot Kör hello `azure rediscache show -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-193">For more information about this command, run hello `azure rediscache show -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a><span data-ttu-id="4560d-194">Ändra inställningar för en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="4560d-194">Change settings of an existing Redis Cache</span></span>
<span data-ttu-id="4560d-195">toochange inställningar för en befintlig Redis-Cache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-195">toochange settings of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

<span data-ttu-id="4560d-196">Mer information om det här kommandot Kör hello `azure rediscache set -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-196">For more information about this command, run hello `azure rediscache set -h` command.</span></span>

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
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-hello-authentication-key-for-an-existing-redis-cache"></a><span data-ttu-id="4560d-197">Förnya hello autentiseringsnyckel för en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="4560d-197">Renew hello authentication key for an existing Redis Cache</span></span>
<span data-ttu-id="4560d-198">toorenew hello autentiseringsnyckel för en befintlig Redis-Cache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-198">toorenew hello authentication key for an existing Redis Cache, use hello following command:</span></span>

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

<span data-ttu-id="4560d-199">Ange `Primary` eller `Secondary` för `key-type`.</span><span class="sxs-lookup"><span data-stu-id="4560d-199">Specify `Primary` or `Secondary` for `key-type`.</span></span>

<span data-ttu-id="4560d-200">Mer information om det här kommandot Kör hello `azure rediscache renew-key -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-200">For more information about this command, run hello `azure rediscache renew-key -h` command.</span></span>

    C:\>azure rediscache renew-key -h
    help:    Renew hello authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key toorenew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a><span data-ttu-id="4560d-201">Lista över primära och sekundära nycklarna i en befintlig Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="4560d-201">List Primary and Secondary keys of an existing Redis Cache</span></span>
<span data-ttu-id="4560d-202">toolist primära och sekundära nycklarna i en befintlig Redis-Cache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4560d-202">toolist Primary and Secondary keys of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

<span data-ttu-id="4560d-203">Mer information om det här kommandot Kör hello `azure rediscache list-keys -h` kommando.</span><span class="sxs-lookup"><span data-stu-id="4560d-203">For more information about this command, run hello `azure rediscache list-keys -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
