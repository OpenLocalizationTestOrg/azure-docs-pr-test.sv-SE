---
title: aaaManage Azure Redis-Cache med Azure PowerShell | Microsoft Docs
description: "Lär dig hur tooperform administrativa uppgifter för Azure Redis-Cache med hjälp av Azure PowerShell."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 1d526ce65c4bc05345cd6c3ff370211ed562cab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a><span data-ttu-id="a99e6-103">Hantera Azure Redis-Cache med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a99e6-103">Manage Azure Redis Cache with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a99e6-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a99e6-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="a99e6-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a99e6-105">Azure CLI</span></span>](cache-manage-cli.md)
> 
> 

<span data-ttu-id="a99e6-106">Det här avsnittet visar hur tooperform vanliga uppgifter som att skapa, uppdatera och skala dina Azure Redis-Cache-instanser hur tooregenerate snabbtangenter och hur tooview information om dina cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-106">This topic shows you how tooperform common tasks such as create, update, and scale your Azure Redis Cache instances, how tooregenerate access keys, and how tooview information about your caches.</span></span> <span data-ttu-id="a99e6-107">En fullständig lista över Azure Redis-Cache PowerShell-cmdlets finns i [cmdlets för Azure Redis-Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span><span class="sxs-lookup"><span data-stu-id="a99e6-107">For a complete list of Azure Redis Cache PowerShell cmdlets, see [Azure Redis Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="a99e6-108">Läs mer om hello klassiska distributionsmodellen [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span><span class="sxs-lookup"><span data-stu-id="a99e6-108">For more information about hello classic deployment model, see [Azure Resource Manager vs. classic deployment: Understand deployment models and hello state of your resources](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a99e6-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a99e6-109">Prerequisites</span></span>
<span data-ttu-id="a99e6-110">Om du redan har installerat Azure PowerShell, måste du ha Azure PowerShell version 1.0.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a99e6-110">If you have already installed Azure PowerShell, you must have Azure PowerShell version 1.0.0 or later.</span></span> <span data-ttu-id="a99e6-111">Du kan kontrollera hello-versionen av Azure PowerShell som du har installerat med detta kommando i Kommandotolken för hello Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a99e6-111">You can check hello version of Azure PowerShell that you have installed with this command at hello Azure PowerShell command prompt.</span></span>

    Get-Module azure | format-table version


<span data-ttu-id="a99e6-112">Först måste du logga in tooAzure med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="a99e6-112">First, you must log in tooAzure with this command.</span></span>

    Login-AzureRmAccount

<span data-ttu-id="a99e6-113">Ange hello e-postadressen för ditt Azure-konto och lösenordet i dialogrutan för hello Microsoft Azure-inloggning.</span><span class="sxs-lookup"><span data-stu-id="a99e6-113">Specify hello email address of your Azure account and its password in hello Microsoft Azure sign-in dialog.</span></span>

<span data-ttu-id="a99e6-114">Sedan om du har flera Azure-prenumerationer måste tooset din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a99e6-114">Next, if you have multiple Azure subscriptions, you need tooset your Azure subscription.</span></span> <span data-ttu-id="a99e6-115">toosee en lista med din aktuella prenumeration, kör kommandot.</span><span class="sxs-lookup"><span data-stu-id="a99e6-115">toosee a list of your current subscriptions, run this command.</span></span>

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="a99e6-116">toospecify hello prenumeration, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-116">toospecify hello subscription, run hello following command.</span></span> <span data-ttu-id="a99e6-117">I följande exempel hello, hello prenumerationsnamn är `ContosoSubscription`.</span><span class="sxs-lookup"><span data-stu-id="a99e6-117">In hello following example, hello subscription name is `ContosoSubscription`.</span></span>

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

<span data-ttu-id="a99e6-118">Innan du kan använda Windows PowerShell med Azure Resource Manager, måste du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a99e6-118">Before you can use Windows PowerShell with Azure Resource Manager, you need hello following:</span></span>

* <span data-ttu-id="a99e6-119">Windows PowerShell Version 3.0 eller 4.0.</span><span class="sxs-lookup"><span data-stu-id="a99e6-119">Windows PowerShell, Version 3.0 or 4.0.</span></span> <span data-ttu-id="a99e6-120">toofind hello version av Windows PowerShell, skriv:`$PSVersionTable` och kontrollera hello värdet för `PSVersion` 3.0 eller 4.0.</span><span class="sxs-lookup"><span data-stu-id="a99e6-120">toofind hello version of Windows PowerShell, type:`$PSVersionTable` and verify hello value of `PSVersion` is 3.0 or 4.0.</span></span> <span data-ttu-id="a99e6-121">tooinstall en kompatibel version finns [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) eller [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span><span class="sxs-lookup"><span data-stu-id="a99e6-121">tooinstall a compatible version, see [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) or [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span></span>

<span data-ttu-id="a99e6-122">tooget utförlig information för alla cmdletar som du ser i den här kursen används hello Get-Help cmdleten.</span><span class="sxs-lookup"><span data-stu-id="a99e6-122">tooget detailed help for any cmdlet you see in this tutorial, use hello Get-Help cmdlet.</span></span>

    Get-Help <cmdlet-name> -Detailed

<span data-ttu-id="a99e6-123">Till exempel tooget hjälp för hello `New-AzureRmRedisCache` cmdlet, skriv:</span><span class="sxs-lookup"><span data-stu-id="a99e6-123">For example, tooget help for hello `New-AzureRmRedisCache` cmdlet, type:</span></span>

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-tooconnect-tooother-clouds"></a><span data-ttu-id="a99e6-124">Hur tooconnect tooother moln</span><span class="sxs-lookup"><span data-stu-id="a99e6-124">How tooconnect tooother clouds</span></span>
<span data-ttu-id="a99e6-125">Miljön är som standard hello Azure `AzureCloud`, som representerar hello globala Azure-molnet instans.</span><span class="sxs-lookup"><span data-stu-id="a99e6-125">By default hello Azure environment is `AzureCloud`, which represents hello global Azure cloud instance.</span></span> <span data-ttu-id="a99e6-126">tooconnect tooa annan instans, Använd hello `Add-AzureRmAccount` med hello `-Environment` eller -`EnvironmentName` kommandoradsväxeln med hello önskad miljö eller namn.</span><span class="sxs-lookup"><span data-stu-id="a99e6-126">tooconnect tooa different instance, use hello `Add-AzureRmAccount` command with hello `-Environment` or -`EnvironmentName` command line switch with hello desired environment or environment name.</span></span>

<span data-ttu-id="a99e6-127">toosee hello lista över tillgängliga miljöer, kör hello `Get-AzureRmEnvironment` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-127">toosee hello list of available environments, run hello `Get-AzureRmEnvironment` cmdlet.</span></span>

### <a name="tooconnect-toohello-azure-government-cloud"></a><span data-ttu-id="a99e6-128">tooconnect toohello Azure Government-molnet</span><span class="sxs-lookup"><span data-stu-id="a99e6-128">tooconnect toohello Azure Government Cloud</span></span>
<span data-ttu-id="a99e6-129">tooconnect toohello Azure offentliga moln, med någon av följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-129">tooconnect toohello Azure Government Cloud, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

<span data-ttu-id="a99e6-130">eller</span><span class="sxs-lookup"><span data-stu-id="a99e6-130">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

<span data-ttu-id="a99e6-131">toocreate en cachelagring i hello Azure offentliga moln, med någon av följande platser hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-131">toocreate a cache in hello Azure Government Cloud, use one of hello following locations.</span></span>

* <span data-ttu-id="a99e6-132">USA: s regering Virginia</span><span class="sxs-lookup"><span data-stu-id="a99e6-132">USGov Virginia</span></span>
* <span data-ttu-id="a99e6-133">USA: s regering Iowa</span><span class="sxs-lookup"><span data-stu-id="a99e6-133">USGov Iowa</span></span>

<span data-ttu-id="a99e6-134">Mer information om hello Azure Government-molnet finns [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) och [Utvecklarhandbok för Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="a99e6-134">For more information about hello Azure Government Cloud, see [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) and [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>

### <a name="tooconnect-toohello-azure-china-cloud"></a><span data-ttu-id="a99e6-135">tooconnect toohello Azure-molnet för Kina</span><span class="sxs-lookup"><span data-stu-id="a99e6-135">tooconnect toohello Azure China Cloud</span></span>
<span data-ttu-id="a99e6-136">tooconnect toohello Azure Kina molnet, med någon av följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-136">tooconnect toohello Azure China Cloud, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

<span data-ttu-id="a99e6-137">eller</span><span class="sxs-lookup"><span data-stu-id="a99e6-137">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

<span data-ttu-id="a99e6-138">toocreate en cachelagring i hello Azure Kina molnet, med någon av följande platser hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-138">toocreate a cache in hello Azure China Cloud, use one of hello following locations.</span></span>

* <span data-ttu-id="a99e6-139">Östra Kina</span><span class="sxs-lookup"><span data-stu-id="a99e6-139">China East</span></span>
* <span data-ttu-id="a99e6-140">Norra Kina</span><span class="sxs-lookup"><span data-stu-id="a99e6-140">China North</span></span>

<span data-ttu-id="a99e6-141">Mer information om hello Azure Kina molnet finns [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="a99e6-141">For more information about hello Azure China Cloud, see [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span>

### <a name="tooconnect-toomicrosoft-azure-germany"></a><span data-ttu-id="a99e6-142">tooconnect tooMicrosoft Azure Tyskland</span><span class="sxs-lookup"><span data-stu-id="a99e6-142">tooconnect tooMicrosoft Azure Germany</span></span>
<span data-ttu-id="a99e6-143">tooconnect tooMicrosoft Azure Tyskland med någon av följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-143">tooconnect tooMicrosoft Azure Germany, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


<span data-ttu-id="a99e6-144">eller</span><span class="sxs-lookup"><span data-stu-id="a99e6-144">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

<span data-ttu-id="a99e6-145">toocreate en cachelagring i Microsoft Azure Tyskland med någon av följande platser hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-145">toocreate a cache in Microsoft Azure Germany, use one of hello following locations.</span></span>

* <span data-ttu-id="a99e6-146">Centrala Tyskland</span><span class="sxs-lookup"><span data-stu-id="a99e6-146">Germany Central</span></span>
* <span data-ttu-id="a99e6-147">Nordöstra Tyskland</span><span class="sxs-lookup"><span data-stu-id="a99e6-147">Germany Northeast</span></span>

<span data-ttu-id="a99e6-148">Läs mer om Microsoft Azure Tyskland [Microsoft Azure Tyskland](https://azure.microsoft.com/overview/clouds/germany/).</span><span class="sxs-lookup"><span data-stu-id="a99e6-148">For more information about Microsoft Azure Germany, see [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).</span></span>

### <a name="properties-used-for-azure-redis-cache-powershell"></a><span data-ttu-id="a99e6-149">Egenskaper som används för Azure Redis-Cache PowerShell</span><span class="sxs-lookup"><span data-stu-id="a99e6-149">Properties used for Azure Redis Cache PowerShell</span></span>
<span data-ttu-id="a99e6-150">hello följande tabell innehåller egenskaper och beskrivningar för vanliga parametrar när du skapar och hanterar din Azure Redis-Cache-instanser som använder Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a99e6-150">hello following table contains properties and descriptions for commonly used parameters when creating and managing your Azure Redis Cache instances using Azure PowerShell.</span></span>

| <span data-ttu-id="a99e6-151">Parameter</span><span class="sxs-lookup"><span data-stu-id="a99e6-151">Parameter</span></span> | <span data-ttu-id="a99e6-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a99e6-152">Description</span></span> | <span data-ttu-id="a99e6-153">Standard</span><span class="sxs-lookup"><span data-stu-id="a99e6-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a99e6-154">Namn</span><span class="sxs-lookup"><span data-stu-id="a99e6-154">Name</span></span> |<span data-ttu-id="a99e6-155">Namnet på hello cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-155">Name of hello cache</span></span> | |
| <span data-ttu-id="a99e6-156">Plats</span><span class="sxs-lookup"><span data-stu-id="a99e6-156">Location</span></span> |<span data-ttu-id="a99e6-157">Platsen för hello cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-157">Location of hello cache</span></span> | |
| <span data-ttu-id="a99e6-158">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="a99e6-158">ResourceGroupName</span></span> |<span data-ttu-id="a99e6-159">Resursgruppens namn i vilken toocreate hello-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-159">Resource group name in which toocreate hello cache</span></span> | |
| <span data-ttu-id="a99e6-160">Storlek</span><span class="sxs-lookup"><span data-stu-id="a99e6-160">Size</span></span> |<span data-ttu-id="a99e6-161">hello storleken på hello cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-161">hello size of hello cache.</span></span> <span data-ttu-id="a99e6-162">Giltiga värden är: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB</span><span class="sxs-lookup"><span data-stu-id="a99e6-162">Valid values are: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB</span></span> |<span data-ttu-id="a99e6-163">1 GB</span><span class="sxs-lookup"><span data-stu-id="a99e6-163">1GB</span></span> |
| <span data-ttu-id="a99e6-164">ShardCount</span><span class="sxs-lookup"><span data-stu-id="a99e6-164">ShardCount</span></span> |<span data-ttu-id="a99e6-165">hello antal shards toocreate när du skapar en premium-cache med aktiverad klustring.</span><span class="sxs-lookup"><span data-stu-id="a99e6-165">hello number of shards toocreate when creating a premium cache with clustering enabled.</span></span> <span data-ttu-id="a99e6-166">Giltiga värden är: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span><span class="sxs-lookup"><span data-stu-id="a99e6-166">Valid values are: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span></span> | |
| <span data-ttu-id="a99e6-167">SKU</span><span class="sxs-lookup"><span data-stu-id="a99e6-167">SKU</span></span> |<span data-ttu-id="a99e6-168">Anger hello SKU hello-cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-168">Specifies hello SKU of hello cache.</span></span> <span data-ttu-id="a99e6-169">Giltiga värden är: Basic, Standard, Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-169">Valid values are: Basic, Standard, Premium</span></span> |<span data-ttu-id="a99e6-170">Standard</span><span class="sxs-lookup"><span data-stu-id="a99e6-170">Standard</span></span> |
| <span data-ttu-id="a99e6-171">RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="a99e6-171">RedisConfiguration</span></span> |<span data-ttu-id="a99e6-172">Anger inställningar för Redis-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a99e6-172">Specifies Redis configuration settings.</span></span> <span data-ttu-id="a99e6-173">Mer information om varje inställning finns hello följande [RedisConfiguration egenskaper](#redisconfiguration-properties) tabell.</span><span class="sxs-lookup"><span data-stu-id="a99e6-173">For details on each setting, see hello following [RedisConfiguration properties](#redisconfiguration-properties) table.</span></span> | |
| <span data-ttu-id="a99e6-174">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="a99e6-174">EnableNonSslPort</span></span> |<span data-ttu-id="a99e6-175">Anger om hello icke-SSL-porten är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a99e6-175">Indicates whether hello non-SSL port is enabled.</span></span> |<span data-ttu-id="a99e6-176">False</span><span class="sxs-lookup"><span data-stu-id="a99e6-176">False</span></span> |
| <span data-ttu-id="a99e6-177">MaxMemoryPolicy</span><span class="sxs-lookup"><span data-stu-id="a99e6-177">MaxMemoryPolicy</span></span> |<span data-ttu-id="a99e6-178">Den här parametern är inaktuell - Använd RedisConfiguration i stället.</span><span class="sxs-lookup"><span data-stu-id="a99e6-178">This parameter has been deprecated - use RedisConfiguration instead.</span></span> | |
| <span data-ttu-id="a99e6-179">StaticIP</span><span class="sxs-lookup"><span data-stu-id="a99e6-179">StaticIP</span></span> |<span data-ttu-id="a99e6-180">När värd för ditt cacheminne i ett virtuellt nätverk, anger du en unik IP-adress i hello undernät för hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-180">When hosting your cache in a VNET, specifies a unique IP address in hello subnet for hello cache.</span></span> <span data-ttu-id="a99e6-181">Om inte är en valt från hello undernät.</span><span class="sxs-lookup"><span data-stu-id="a99e6-181">If not provided, one is chosen for you from hello subnet.</span></span> | |
| <span data-ttu-id="a99e6-182">Undernät</span><span class="sxs-lookup"><span data-stu-id="a99e6-182">Subnet</span></span> |<span data-ttu-id="a99e6-183">När värd för ditt cacheminne i ett virtuellt nätverk, anger hello hello undernät i vilken toodeploy hello cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-183">When hosting your cache in a VNET, specifies hello name of hello subnet in which toodeploy hello cache.</span></span> | |
| <span data-ttu-id="a99e6-184">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="a99e6-184">VirtualNetwork</span></span> |<span data-ttu-id="a99e6-185">När värd för ditt cacheminne i ett VNET anger hello resurs-ID för hello virtuella nätverk i vilka toodeploy hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-185">When hosting your cache in a VNET, specifies hello resource ID of hello VNET in which toodeploy hello cache.</span></span> | |
| <span data-ttu-id="a99e6-186">KeyType</span><span class="sxs-lookup"><span data-stu-id="a99e6-186">KeyType</span></span> |<span data-ttu-id="a99e6-187">Anger vilka åtkomstnyckel tooregenerate när du förnyar åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-187">Specifies which access key tooregenerate when renewing access keys.</span></span> <span data-ttu-id="a99e6-188">Giltiga värden är: primära, sekundära</span><span class="sxs-lookup"><span data-stu-id="a99e6-188">Valid values are: Primary, Secondary</span></span> | |

### <a name="redisconfiguration-properties"></a><span data-ttu-id="a99e6-189">RedisConfiguration egenskaper</span><span class="sxs-lookup"><span data-stu-id="a99e6-189">RedisConfiguration properties</span></span>
| <span data-ttu-id="a99e6-190">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a99e6-190">Property</span></span> | <span data-ttu-id="a99e6-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a99e6-191">Description</span></span> | <span data-ttu-id="a99e6-192">Prisnivåer</span><span class="sxs-lookup"><span data-stu-id="a99e6-192">Pricing tiers</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a99e6-193">RDB säkerhetskopiering aktiverad</span><span class="sxs-lookup"><span data-stu-id="a99e6-193">rdb-backup-enabled</span></span> |<span data-ttu-id="a99e6-194">Om [Redis-datapersistence](cache-how-to-premium-persistence.md) är aktiverad</span><span class="sxs-lookup"><span data-stu-id="a99e6-194">Whether [Redis data persistence](cache-how-to-premium-persistence.md) is enabled</span></span> |<span data-ttu-id="a99e6-195">Endast Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-195">Premium only</span></span> |
| <span data-ttu-id="a99e6-196">RDB---anslutningssträngen för lagring</span><span class="sxs-lookup"><span data-stu-id="a99e6-196">rdb-storage-connection-string</span></span> |<span data-ttu-id="a99e6-197">Hej anslutning sträng toohello storage-konto för [Redis-datapersistence](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="a99e6-197">hello connection string toohello storage account for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="a99e6-198">Endast Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-198">Premium only</span></span> |
| <span data-ttu-id="a99e6-199">RDB säkerhetskopieringsfrekvens</span><span class="sxs-lookup"><span data-stu-id="a99e6-199">rdb-backup-frequency</span></span> |<span data-ttu-id="a99e6-200">Hej säkerhetskopieringsfrekvensen för [Redis-datapersistence](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="a99e6-200">hello backup frequency for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="a99e6-201">Endast Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-201">Premium only</span></span> |
| <span data-ttu-id="a99e6-202">maxmemory-reserverade</span><span class="sxs-lookup"><span data-stu-id="a99e6-202">maxmemory-reserved</span></span> |<span data-ttu-id="a99e6-203">Konfigurerar hello [minne som reserverats](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) för icke-cache-processer</span><span class="sxs-lookup"><span data-stu-id="a99e6-203">Configures hello [memory reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for non-cache processes</span></span> |<span data-ttu-id="a99e6-204">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-204">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-205">maxmemory-princip</span><span class="sxs-lookup"><span data-stu-id="a99e6-205">maxmemory-policy</span></span> |<span data-ttu-id="a99e6-206">Konfigurerar hello [av principen](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) för hello-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-206">Configures hello [eviction policy](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for hello cache</span></span> |<span data-ttu-id="a99e6-207">Alla prisnivåer</span><span class="sxs-lookup"><span data-stu-id="a99e6-207">All pricing tiers</span></span> |
| <span data-ttu-id="a99e6-208">meddela-keyspace-händelser</span><span class="sxs-lookup"><span data-stu-id="a99e6-208">notify-keyspace-events</span></span> |<span data-ttu-id="a99e6-209">Konfigurerar [keyspace-meddelanden](cache-configure.md#keyspace-notifications-advanced-settings)</span><span class="sxs-lookup"><span data-stu-id="a99e6-209">Configures [keyspace notifications](cache-configure.md#keyspace-notifications-advanced-settings)</span></span> |<span data-ttu-id="a99e6-210">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-210">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-211">hash-max-ziplist-poster</span><span class="sxs-lookup"><span data-stu-id="a99e6-211">hash-max-ziplist-entries</span></span> |<span data-ttu-id="a99e6-212">Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper</span><span class="sxs-lookup"><span data-stu-id="a99e6-212">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="a99e6-213">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-213">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-214">hash--ziplist-värdet för maximalt antal</span><span class="sxs-lookup"><span data-stu-id="a99e6-214">hash-max-ziplist-value</span></span> |<span data-ttu-id="a99e6-215">Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper</span><span class="sxs-lookup"><span data-stu-id="a99e6-215">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="a99e6-216">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-216">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-217">set-max-intset-poster</span><span class="sxs-lookup"><span data-stu-id="a99e6-217">set-max-intset-entries</span></span> |<span data-ttu-id="a99e6-218">Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper</span><span class="sxs-lookup"><span data-stu-id="a99e6-218">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="a99e6-219">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-219">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-220">zset-max-ziplist-poster</span><span class="sxs-lookup"><span data-stu-id="a99e6-220">zset-max-ziplist-entries</span></span> |<span data-ttu-id="a99e6-221">Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper</span><span class="sxs-lookup"><span data-stu-id="a99e6-221">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="a99e6-222">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-222">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-223">zset--ziplist-värdet för maximalt antal</span><span class="sxs-lookup"><span data-stu-id="a99e6-223">zset-max-ziplist-value</span></span> |<span data-ttu-id="a99e6-224">Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper</span><span class="sxs-lookup"><span data-stu-id="a99e6-224">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="a99e6-225">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-225">Standard and Premium</span></span> |
| <span data-ttu-id="a99e6-226">databaser</span><span class="sxs-lookup"><span data-stu-id="a99e6-226">databases</span></span> |<span data-ttu-id="a99e6-227">Konfigurerar hello antalet databaser.</span><span class="sxs-lookup"><span data-stu-id="a99e6-227">Configures hello number of databases.</span></span> <span data-ttu-id="a99e6-228">Den här egenskapen kan endast konfigureras hos skapa cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-228">This property can be configured only at cache creation.</span></span> |<span data-ttu-id="a99e6-229">Standard och Premium</span><span class="sxs-lookup"><span data-stu-id="a99e6-229">Standard and Premium</span></span> |

## <a name="toocreate-a-redis-cache"></a><span data-ttu-id="a99e6-230">toocreate Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-230">toocreate a Redis Cache</span></span>
<span data-ttu-id="a99e6-231">Nya instanser av Azure Redis-Cache skapas med hello [ny AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-231">New Azure Redis Cache instances are created using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a99e6-232">hello första gången du skapar ett Redis-cache i en prenumeration med hjälp av hello Azure-portalen hello portal registrerar hello `Microsoft.Cache` namnområde för den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-232">hello first time you create a Redis cache in a subscription using hello Azure portal, hello portal registers hello `Microsoft.Cache` namespace for that subscription.</span></span> <span data-ttu-id="a99e6-233">Om du försöker toocreate hello först Redis-cache i en prenumeration med hjälp av PowerShell, måste du först registrera namnutrymmet med hjälp av följande kommando; hello Annars cmdlets som `New-AzureRmRedisCache` och `Get-AzureRmRedisCache` misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a99e6-233">If you attempt toocreate hello first Redis cache in a subscription using PowerShell, you must first register that namespace using hello following command; otherwise cmdlets such as `New-AzureRmRedisCache` and `Get-AzureRmRedisCache` fail.</span></span>
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

<span data-ttu-id="a99e6-234">toosee en lista över tillgängliga parametrar och deras beskrivningar för `New-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-234">toosee a list of available parameters and their descriptions for `New-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        hello New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of hello redis cache toocreate.

        -ResourceGroupName <String>
            Name of resource group in which toocreate hello redis cache.

        -Location <String>
            Location in which toocreate hello redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, hello default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        -VirtualNetwork <String>
            hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="a99e6-235">toocreate en cache med standardparametrar, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="a99e6-235">toocreate a cache with default parameters, run hello following command.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

<span data-ttu-id="a99e6-236">`ResourceGroupName`, `Name`, och `Location` parametrar är obligatoriska, men resten av hello är valfria och har standardvärden.</span><span class="sxs-lookup"><span data-stu-id="a99e6-236">`ResourceGroupName`, `Name`, and `Location` are required parameters, but hello rest are optional and have default values.</span></span> <span data-ttu-id="a99e6-237">Kör hello föregående kommando skapar en Standard SKU Azure Redis-Cache-instans med hello angivet namn, plats och resursgruppen som är 1 GB i storlek med hello SSL-porten är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="a99e6-237">Running hello previous command creates a Standard SKU Azure Redis Cache instance with hello specified name, location, and resource group, that is 1 GB in size with hello non-SSL port disabled.</span></span>

<span data-ttu-id="a99e6-238">toocreate premium-cache, ange en storlek på P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), eller P4 (53 GB - 530 GB).</span><span class="sxs-lookup"><span data-stu-id="a99e6-238">toocreate a premium cache, specify a size of P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), or P4 (53 GB - 530 GB).</span></span> <span data-ttu-id="a99e6-239">tooenable klustring, ange ett Fragmentera antal med hello `ShardCount` parameter.</span><span class="sxs-lookup"><span data-stu-id="a99e6-239">tooenable clustering, specify a shard count using hello `ShardCount` parameter.</span></span> <span data-ttu-id="a99e6-240">hello skapar följande exempel en premium P1-cache med 3 delar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-240">hello following example creates a P1 premium cache with 3 shards.</span></span> <span data-ttu-id="a99e6-241">Premium P1-cache är 6 GB i storlek och eftersom vi angett tre delar hello totala storlek är 18 GB (3 x 6 GB).</span><span class="sxs-lookup"><span data-stu-id="a99e6-241">A P1 premium cache is 6 GB in size, and since we specified three shards hello total size is 18 GB (3 x 6 GB).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

<span data-ttu-id="a99e6-242">toospecify värden för hello `RedisConfiguration` parameter, ange hello värden i `{}` som nyckel/värde-par som `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span><span class="sxs-lookup"><span data-stu-id="a99e6-242">toospecify values for hello `RedisConfiguration` parameter, enclose hello values inside `{}` as key/value pairs like `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span></span> <span data-ttu-id="a99e6-243">hello följande exempel skapar en standard 1 GB-cache med `allkeys-random` maxmemory-principen och keyspace-meddelanden som konfigurerats med `KEA`.</span><span class="sxs-lookup"><span data-stu-id="a99e6-243">hello following example creates a standard 1 GB cache with `allkeys-random` maxmemory policy and keyspace notifications configured with `KEA`.</span></span> <span data-ttu-id="a99e6-244">Mer information finns i [Keyspace-meddelanden (avancerade inställningar)](cache-configure.md#keyspace-notifications-advanced-settings) och [minne principer](cache-configure.md#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="a99e6-244">For more information, see [Keyspace notifications (advanced settings)](cache-configure.md#keyspace-notifications-advanced-settings) and [Memory policies](cache-configure.md#memory-policies).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="tooconfigure-hello-databases-setting-during-cache-creation"></a><span data-ttu-id="a99e6-245">tooconfigure hello databaser anger under Skapa cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-245">tooconfigure hello databases setting during cache creation</span></span>
<span data-ttu-id="a99e6-246">Hej `databases` inställningen konfigureras bara under Skapa cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-246">hello `databases` setting can be configured only during cache creation.</span></span> <span data-ttu-id="a99e6-247">hello följande exempel skapar en premium P3 (26 GB)-cache med 48 databaser med hjälp av hello [ny AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-247">hello following example creates a premium P3 (26 GB) cache with 48 databases using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

<span data-ttu-id="a99e6-248">Mer information om hello `databases` egenskap, se [Default Azure Redis-Cache-serverkonfigurationen](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="a99e6-248">For more information on hello `databases` property, see [Default Azure Redis Cache server configuration](cache-configure.md#default-redis-server-configuration).</span></span> <span data-ttu-id="a99e6-249">Mer information om hur du skapar en cache med hello [ny AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, se föregående hello [toocreate Redis-Cache](#to-create-a-redis-cache) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a99e6-249">For more information on creating a cache using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, see hello previous [toocreate a Redis Cache](#to-create-a-redis-cache) section.</span></span>

## <a name="tooupdate-a-redis-cache"></a><span data-ttu-id="a99e6-250">tooupdate ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-250">tooupdate a Redis cache</span></span>
<span data-ttu-id="a99e6-251">Azure Redis-Cache-instanser har uppdaterats med hello [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-251">Azure Redis Cache instances are updated using hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.</span></span>

<span data-ttu-id="a99e6-252">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Set-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-252">toosee a list of available parameters and their descriptions for `Set-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        hello Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooupdate.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. hello default value is null and no change will be made toothe
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="a99e6-253">Hej `Set-AzureRmRedisCache` används tooupdate egenskaper som kan vara `Size`, `Sku`, `EnableNonSslPort`, och hello `RedisConfiguration` värden.</span><span class="sxs-lookup"><span data-stu-id="a99e6-253">hello `Set-AzureRmRedisCache` cmdlet can be used tooupdate properties such as `Size`, `Sku`, `EnableNonSslPort`, and hello `RedisConfiguration` values.</span></span> 

<span data-ttu-id="a99e6-254">hello följande kommando uppdateringar hello maxmemory-princip för hello Redis-Cache med namnet myCache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-254">hello following command updates hello maxmemory-policy for hello Redis Cache named myCache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="tooscale-a-redis-cache"></a><span data-ttu-id="a99e6-255">tooscale ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-255">tooscale a Redis cache</span></span>
<span data-ttu-id="a99e6-256">`Set-AzureRmRedisCache`kan använda tooscale en Azure Redis-cache-instans när hello `Size`, `Sku`, eller `ShardCount` ändras egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="a99e6-256">`Set-AzureRmRedisCache` can be used tooscale an Azure Redis cache instance when hello `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> 

> [!NOTE]
> <span data-ttu-id="a99e6-257">Skala en cache med PowerShell är ämne toohello samma begränsningar och riktlinjer som skalning en cache från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-257">Scaling a cache using PowerShell is subject toohello same limits and guidelines as scaling a cache from hello Azure portal.</span></span> <span data-ttu-id="a99e6-258">Du kan skala tooa olika prisnivån med hello följande begränsningar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-258">You can scale tooa different pricing tier with hello following restrictions.</span></span>
> 
> * <span data-ttu-id="a99e6-259">Du kan skala från en högre prisnivå nivå tooa lägre prisnivån.</span><span class="sxs-lookup"><span data-stu-id="a99e6-259">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
> * <span data-ttu-id="a99e6-260">Du kan skala från en **Premium** cache ned tooa **Standard** eller en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-260">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
> * <span data-ttu-id="a99e6-261">Du kan skala från en **Standard** cache ned tooa **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-261">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
> * <span data-ttu-id="a99e6-262">Du kan skala från en **grundläggande** cachelagra tooa **Standard** cache men du kan inte ändra hello storleken på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="a99e6-262">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="a99e6-263">Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärden toohello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="a99e6-263">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
> * <span data-ttu-id="a99e6-264">Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-264">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="a99e6-265">Du måste skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** i ett efterföljande skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a99e6-265">You must scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
> * <span data-ttu-id="a99e6-266">Du kan skala från en större storlek ned toohello **C0 (250 MB)** storlek.</span><span class="sxs-lookup"><span data-stu-id="a99e6-266">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>
> 
> <span data-ttu-id="a99e6-267">Mer information finns i [hur tooScale Azure Redis-Cache](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="a99e6-267">For more information, see [How tooScale Azure Redis Cache](cache-how-to-scale.md).</span></span>
> 
> 

<span data-ttu-id="a99e6-268">hello följande exempel visas hur tooscale en cache med namnet `myCache` tooa 2,5 GB cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-268">hello following example shows how tooscale a cache named `myCache` tooa 2.5 GB cache.</span></span> <span data-ttu-id="a99e6-269">Observera att det här kommandot fungerar för både en Basic eller Standard cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-269">Note that this command works for both a Basic or a Standard cache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="a99e6-270">Efter det här kommandot har utfärdats, returneras hello status hello-cache (liknande toocalling `Get-AzureRmRedisCache`).</span><span class="sxs-lookup"><span data-stu-id="a99e6-270">After this command is issued, hello status of hello cache is returned (similar toocalling `Get-AzureRmRedisCache`).</span></span> <span data-ttu-id="a99e6-271">Observera att hello `ProvisioningState` är `Scaling`.</span><span class="sxs-lookup"><span data-stu-id="a99e6-271">Note that hello `ProvisioningState` is `Scaling`.</span></span>

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

<span data-ttu-id="a99e6-272">När hello skalning åtgärden är klar, hello `ProvisioningState` ändras för`Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="a99e6-272">When hello scaling operation is complete, hello `ProvisioningState` changes too`Succeeded`.</span></span> <span data-ttu-id="a99e6-273">Du måste vänta tills hello föregående åtgärd har slutförts eller så visas ett felmeddelande liknande toohello följande om du behöver toomake en efterföljande skalning åtgärd som att ändra från grundläggande tooStandard och sedan ändrar hello storlek.</span><span class="sxs-lookup"><span data-stu-id="a99e6-273">If you need toomake a subsequent scaling operation, such as changing from Basic tooStandard and then changing hello size, you must wait until hello previous operation is complete or you receive an error similar toohello following.</span></span>

    Set-AzureRmRedisCache : Conflict: hello resource '...' is not in a stable state, and is currently unable tooaccept hello update request.

## <a name="tooget-information-about-a-redis-cache"></a><span data-ttu-id="a99e6-274">tooget information om ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-274">tooget information about a Redis cache</span></span>
<span data-ttu-id="a99e6-275">Du kan hämta information om en cache med hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-275">You can retrieve information about a cache using hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.</span></span>

<span data-ttu-id="a99e6-276">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Get-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-276">toosee a list of available parameters and their descriptions for `Get-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in hello specified resource group or all caches in hello current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCache cmdlet gets hello details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in hello specified resource group.

        If no parameters are given than it will return details about all caches hello current subscription.

    PARAMETERS
        -Name <String>
            hello name of hello cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns hello details for hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns hello details of hello cache specified by Name. If only hello ResourceGroup
            parameter is provided, then details for all caches in hello resource group are returned.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="a99e6-277">tooreturn information om alla Cacheminnena i hello aktuella prenumeration kör `Get-AzureRmRedisCache` utan några parametrar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-277">tooreturn information about all caches in hello current subscription, run `Get-AzureRmRedisCache` without any parameters.</span></span>

    Get-AzureRmRedisCache

<span data-ttu-id="a99e6-278">tooreturn information om alla cacheminnen i en viss resursgrupp kör `Get-AzureRmRedisCache` med hello `ResourceGroupName` parameter.</span><span class="sxs-lookup"><span data-stu-id="a99e6-278">tooreturn information about all caches in a specific resource group, run `Get-AzureRmRedisCache` with hello `ResourceGroupName` parameter.</span></span>

    Get-AzureRmRedisCache -ResourceGroupName myGroup

<span data-ttu-id="a99e6-279">Kör tooreturn information om en specifik cache `Get-AzureRmRedisCache` med hello `Name` parameter med hello namn hello cache och hello `ResourceGroupName` parameter med hello resursgruppen som innehåller om cachen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-279">tooreturn information about a specific cache, run `Get-AzureRmRedisCache` with hello `Name` parameter containing hello name of hello cache, and hello `ResourceGroupName` parameter with hello resource group containing that cache.</span></span>

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="tooretrieve-hello-access-keys-for-a-redis-cache"></a><span data-ttu-id="a99e6-280">tooretrieve hello åtkomstnycklar för Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-280">tooretrieve hello access keys for a Redis cache</span></span>
<span data-ttu-id="a99e6-281">tooretrieve hello åtkomstnycklarna för ditt cacheminne, kan du använda hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-281">tooretrieve hello access keys for your cache, you can use hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.</span></span>

<span data-ttu-id="a99e6-282">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Get-AzureRmRedisCacheKey`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-282">toosee a list of available parameters and their descriptions for `Get-AzureRmRedisCacheKey`, run hello following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets hello accesskeys for hello specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCacheKey cmdlet gets hello access keys for hello specified cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="a99e6-283">tooretrieve hello nycklar för ditt cacheminne anropet hello `Get-AzureRmRedisCacheKey` cmdlet- och pass i hello namnet på ditt cacheminne hello namnet på hello resursgruppen som innehåller hello cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-283">tooretrieve hello keys for your cache, call hello `Get-AzureRmRedisCacheKey` cmdlet and pass in hello name of your cache hello name of hello resource group that contains hello cache.</span></span>

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="tooregenerate-access-keys-for-your-redis-cache"></a><span data-ttu-id="a99e6-284">tooregenerate åtkomstnycklarna för Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-284">tooregenerate access keys for your Redis cache</span></span>
<span data-ttu-id="a99e6-285">tooregenerate hello åtkomstnycklarna för ditt cacheminne, kan du använda hello [ny AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-285">tooregenerate hello access keys for your cache, you can use hello [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.</span></span>

<span data-ttu-id="a99e6-286">toosee en lista över tillgängliga parametrar och deras beskrivningar för `New-AzureRmRedisCacheKey`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-286">toosee a list of available parameters and their descriptions for `New-AzureRmRedisCacheKey`, run hello following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates hello access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        hello New-AzureRmRedisCacheKey cmdlet regenerate hello access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -KeyType <String>
            Specifies whether tooregenerate hello primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When hello Force parameter is provided, hello specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="a99e6-287">tooregenerate hello primära och sekundära nycklarna för ditt cacheminne anropet hello `New-AzureRmRedisCacheKey` cmdlet pass i hello namn, resursgruppens namn och ange antingen `Primary` eller `Secondary` för hello `KeyType` parameter.</span><span class="sxs-lookup"><span data-stu-id="a99e6-287">tooregenerate hello primary or secondary key for your cache, call hello `New-AzureRmRedisCacheKey` cmdlet and pass in hello name, resource group, and specify either `Primary` or `Secondary` for hello `KeyType` parameter.</span></span> <span data-ttu-id="a99e6-288">I följande exempel hello, återskapas hello sekundära åtkomstnyckeln för cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-288">In hello following example, hello secondary access key for a cache is regenerated.</span></span>

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want tooregenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="toodelete-a-redis-cache"></a><span data-ttu-id="a99e6-289">toodelete ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-289">toodelete a Redis cache</span></span>
<span data-ttu-id="a99e6-290">toodelete Redis-cache använder hello [ta bort AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-290">toodelete a Redis cache, use hello [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.</span></span>

<span data-ttu-id="a99e6-291">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Remove-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-291">toosee a list of available parameters and their descriptions for `Remove-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        hello Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooremove.

        -ResourceGroupName <String>
            Name of hello resource group of hello cache tooremove.

        -Force
            When hello Force parameter is provided, hello cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes hello cache and does not return any value. If hello PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating hello success of hello operatio

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="a99e6-292">I följande exempel hello, hello cache med namnet `myCache` tas bort.</span><span class="sxs-lookup"><span data-stu-id="a99e6-292">In hello following example, hello cache named `myCache` is removed.</span></span>

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want tooremove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="tooimport-a-redis-cache"></a><span data-ttu-id="a99e6-293">tooimport ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-293">tooimport a Redis cache</span></span>
<span data-ttu-id="a99e6-294">Du kan importera data till en Azure Redis-Cache-instans med hello `Import-AzureRmRedisCache` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-294">You can import data into an Azure Redis Cache instance using hello `Import-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a99e6-295">Import/Export är endast tillgängligt för [premiumnivån](cache-premium-tier-intro.md) cachelagrar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-295">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="a99e6-296">Mer information om Import/Export finns [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="a99e6-296">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="a99e6-297">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Import-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-297">toosee a list of available parameters and their descriptions for `Import-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs tooAzure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Import-AzureRmRedisCache cmdlet imports data from hello specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into hello cache.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -Force
            When hello Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If hello PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating hello success of the
            operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="a99e6-298">hello importerar följande kommando data från hello blob som anges av hello SAS-uri i Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-298">hello following command imports data from hello blob specified by hello SAS uri into Azure Redis Cache.</span></span>

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="tooexport-a-redis-cache"></a><span data-ttu-id="a99e6-299">tooexport ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-299">tooexport a Redis cache</span></span>
<span data-ttu-id="a99e6-300">Du kan exportera data från en Azure Redis-Cache-instans med hello `Export-AzureRmRedisCache` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-300">You can export data from an Azure Redis Cache instance using hello `Export-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a99e6-301">Import/Export är endast tillgängligt för [premiumnivån](cache-premium-tier-intro.md) cachelagrar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-301">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="a99e6-302">Mer information om Import/Export finns [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="a99e6-302">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="a99e6-303">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Export-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-303">toosee a list of available parameters and their descriptions for `Export-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache tooa specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache tooa specified container.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Prefix <String>
            Prefix toouse for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="a99e6-304">hello exporterar följande kommando data från en Azure Redis-Cache-instans till hello-behållare som anges av hello SAS-uri.</span><span class="sxs-lookup"><span data-stu-id="a99e6-304">hello following command exports data from an Azure Redis Cache instance into hello container specified by hello SAS uri.</span></span>

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="tooreboot-a-redis-cache"></a><span data-ttu-id="a99e6-305">tooreboot ett Redis-cache</span><span class="sxs-lookup"><span data-stu-id="a99e6-305">tooreboot a Redis cache</span></span>
<span data-ttu-id="a99e6-306">Du kan starta om din Azure Redis-Cache-instans med hello `Reset-AzureRmRedisCache` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a99e6-306">You can reboot your Azure Redis Cache instance using hello `Reset-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a99e6-307">Omstart är endast tillgängligt för [premiumnivån](cache-premium-tier-intro.md) cachelagrar.</span><span class="sxs-lookup"><span data-stu-id="a99e6-307">Reboot is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="a99e6-308">Mer information om omstart ditt cacheminne finns [cachelagra administration - omstart](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="a99e6-308">For more information about rebooting your cache, see [Cache administration - reboot](cache-administration.md#reboot).</span></span>
> 
> 

<span data-ttu-id="a99e6-309">toosee en lista över tillgängliga parametrar och deras beskrivningar för `Reset-AzureRmRedisCache`kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a99e6-309">toosee a list of available parameters and their descriptions for `Reset-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Reset-AzureRmRedisCache cmdlet reboots hello specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -RebootType <String>
            Which node tooreboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard tooreboot when rebooting a premium cache with clustering enabled.

        -Force
            When hello Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="a99e6-310">hello följande kommando startar om båda noderna i hello angetts cache.</span><span class="sxs-lookup"><span data-stu-id="a99e6-310">hello following command reboots both nodes of hello specified cache.</span></span>

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a><span data-ttu-id="a99e6-311">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a99e6-311">Next steps</span></span>
<span data-ttu-id="a99e6-312">toolearn mer information om hur du använder Windows PowerShell med Azure, se hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a99e6-312">toolearn more about using Windows PowerShell with Azure, see hello following resources:</span></span>

* [<span data-ttu-id="a99e6-313">Azure Redis-Cache cmdlet-dokumentationen på MSDN</span><span class="sxs-lookup"><span data-stu-id="a99e6-313">Azure Redis Cache cmdlet documentation on MSDN</span></span>](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* <span data-ttu-id="a99e6-314">[Azure Resource Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Lär dig toouse hello-cmdletar i hello Azure Resource Manager-modulen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-314">[Azure Resource Manager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Learn toouse hello cmdlets in hello Azure Resource Manager module.</span></span>
* <span data-ttu-id="a99e6-315">[Resursnamnet grupper toomanage resurserna i Azure](../azure-resource-manager/resource-group-template-deploy-portal.md): Lär dig hur toocreate och hantera resursgrupper i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a99e6-315">[Using Resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-template-deploy-portal.md): Learn how toocreate and manage resource groups in hello Azure portal.</span></span>
* <span data-ttu-id="a99e6-316">[Azure blogg](http://blogs.msdn.com/windowsazure): Lär dig mer om nya funktioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="a99e6-316">[Azure blog](http://blogs.msdn.com/windowsazure): Learn about new features in Azure.</span></span>
* <span data-ttu-id="a99e6-317">[Windows PowerShell-blogg](http://blogs.msdn.com/powershell): Lär dig mer om nya funktioner i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a99e6-317">[Windows PowerShell blog](http://blogs.msdn.com/powershell): Learn about new features in Windows PowerShell.</span></span>
* <span data-ttu-id="a99e6-318">[”Hej, skriptdoktorn”! Bloggen](http://blogs.technet.com/b/heyscriptingguy/): hämta verkliga tips och råd från hello Windows PowerShell-communityn.</span><span class="sxs-lookup"><span data-stu-id="a99e6-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Get real-world tips and tricks from hello Windows PowerShell community.</span></span>

