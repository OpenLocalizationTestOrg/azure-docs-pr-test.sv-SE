---
title: aaaManage DNS-zoner i Azure DNS - Azure CLI 1.0 | Microsoft Docs
description: "Du kan hantera DNS-zoner som använder Azure CLI 1.0. Den här artikeln visar hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: cb9790cc46626ef7f38a43edb57511104fe6057e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-10"></a><span data-ttu-id="e0a93-104">Hur toomanage DNS-zoner i Azure DNS med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e0a93-104">How toomanage DNS Zones in Azure DNS using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0a93-105">Portal</span><span class="sxs-lookup"><span data-stu-id="e0a93-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="e0a93-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0a93-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="e0a93-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e0a93-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="e0a93-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e0a93-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="e0a93-109">Den här guiden visar hur toomanage DNS-zoner med hjälp av hello plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="e0a93-109">This guide shows how toomanage your DNS zones by using hello cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="e0a93-110">Du kan också hantera DNS-zoner med [Azure PowerShell](dns-operations-dnszones.md) eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0a93-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or hello Azure portal.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="e0a93-111">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="e0a93-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="e0a93-112">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="e0a93-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="e0a93-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="e0a93-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="e0a93-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="e0a93-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="e0a93-115">Introduktion</span><span class="sxs-lookup"><span data-stu-id="e0a93-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a><span data-ttu-id="e0a93-116">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="e0a93-116">Getting help</span></span>

<span data-ttu-id="e0a93-117">Alla CLI 1.0-kommandon som rör tooAzure DNS börja med `azure network dns`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-117">All CLI 1.0 commands relating tooAzure DNS start with `azure network dns`.</span></span> <span data-ttu-id="e0a93-118">Hjälp är tillgänglig för varje kommando med hello `--help` alternativet (kort form `-h`).</span><span class="sxs-lookup"><span data-stu-id="e0a93-118">Help is available for each command using hello `--help` option (short form `-h`).</span></span>  <span data-ttu-id="e0a93-119">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e0a93-119">For example:</span></span>

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="e0a93-120">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="e0a93-120">Create a DNS zone</span></span>

<span data-ttu-id="e0a93-121">En DNS-zon skapas med hjälp av hello `azure network dns zone create` kommando.</span><span class="sxs-lookup"><span data-stu-id="e0a93-121">A DNS zone is created using hello `azure network dns zone create` command.</span></span> <span data-ttu-id="e0a93-122">Om du vill ha hjälp, så gå till `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-122">For help, see `azure network dns zone create -h`.</span></span>

<span data-ttu-id="e0a93-123">hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="e0a93-123">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a><span data-ttu-id="e0a93-124">toocreate en DNS-zon med taggar</span><span class="sxs-lookup"><span data-stu-id="e0a93-124">toocreate a DNS zone with tags</span></span>

<span data-ttu-id="e0a93-125">hello följande exempel visas hur toocreate ett DNS zonen med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*, med hjälp av hello `--tags` Parametern (kort form `-t`):</span><span class="sxs-lookup"><span data-stu-id="e0a93-125">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using hello `--tags` parameter (short form `-t`):</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="e0a93-126">Hämta en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="e0a93-126">Get a DNS zone</span></span>

<span data-ttu-id="e0a93-127">Använd tooretrieve en DNS-zon `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-127">tooretrieve a DNS zone, use `azure network dns zone show`.</span></span> <span data-ttu-id="e0a93-128">Om du vill ha hjälp, så gå till `azure network dns zone show -h`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-128">For help, see `azure network dns zone show -h`.</span></span>

<span data-ttu-id="e0a93-129">hello följande exempel returnerar hello DNS-zonen *contoso.com* och dess associerade data från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="e0a93-129">hello following example returns hello DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

<span data-ttu-id="e0a93-130">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="e0a93-130">hello following example is hello response.</span></span>

```
info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

<span data-ttu-id="e0a93-131">Observera att DNS-poster inte returneras av `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-131">Note that DNS records are not returned by `azure network dns zone show`.</span></span> <span data-ttu-id="e0a93-132">toolist DNS-poster, Använd `azure network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-132">toolist DNS records, use `azure network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="e0a93-133">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="e0a93-133">List DNS zones</span></span>

<span data-ttu-id="e0a93-134">tooenumerate DNS-zoner, Använd `azure network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-134">tooenumerate DNS zones, use `azure network dns zone list`.</span></span> <span data-ttu-id="e0a93-135">Om du vill ha hjälp, så gå till `azure network dns zone list -h`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-135">For help, see `azure network dns zone list -h`.</span></span>

<span data-ttu-id="e0a93-136">Resursgruppen för att ange hello visas zonerna inom hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="e0a93-136">Specifying hello resource group lists only those zones within hello resource group:</span></span>

```azurecli
azure network dns zone list MyResourceGroup
```

<span data-ttu-id="e0a93-137">Utelämna hello resursgrupp visar en lista över alla zoner i prenumerationen hello:</span><span class="sxs-lookup"><span data-stu-id="e0a93-137">Omitting hello resource group lists all zones in hello subscription:</span></span>

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="e0a93-138">Uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="e0a93-138">Update a DNS zone</span></span>

<span data-ttu-id="e0a93-139">Ändringar tooa DNS-zonresurs kan göras med hjälp av `azure network dns zone set`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-139">Changes tooa DNS zone resource can be made using `azure network dns zone set`.</span></span> <span data-ttu-id="e0a93-140">Om du vill ha hjälp, så gå till `azure network dns zone set -h`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-140">For help, see `azure network dns zone set -h`.</span></span>

<span data-ttu-id="e0a93-141">Det här kommandot uppdaterar inte någon hello DNS uppsättningar av poster i zonen hello (se [hur tooManage DNS-poster](dns-operations-recordsets-cli-nodejs.md)).</span><span class="sxs-lookup"><span data-stu-id="e0a93-141">This command does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets-cli-nodejs.md)).</span></span> <span data-ttu-id="e0a93-142">Det är bara använda tooupdate egenskaper för hello zonresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="e0a93-142">It is only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="e0a93-143">Dessa egenskaper är för närvarande begränsad toohello [Azure Resource Manager ”-taggar'](dns-zones-records.md#tags) för hello zonen resurs.</span><span class="sxs-lookup"><span data-stu-id="e0a93-143">These properties are currently limited toohello [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for hello zone resource.</span></span>

<span data-ttu-id="e0a93-144">hello följande exempel visas hur tooupdate hello taggar på en DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="e0a93-144">hello following example shows how tooupdate hello tags on a DNS zone.</span></span> <span data-ttu-id="e0a93-145">hello befintliga taggar ersätts med hello-värde som angetts.</span><span class="sxs-lookup"><span data-stu-id="e0a93-145">hello existing tags are replaced by hello value specified.</span></span>

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="e0a93-146">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="e0a93-146">Delete a DNS Zone</span></span>

<span data-ttu-id="e0a93-147">DNS-zoner kan tas bort med `azure network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-147">DNS zones can be deleted using `azure network dns zone delete`.</span></span> <span data-ttu-id="e0a93-148">Om du vill ha hjälp, så gå till `azure network dns zone delete -h`.</span><span class="sxs-lookup"><span data-stu-id="e0a93-148">For help, see `azure network dns zone delete -h`.</span></span>

> [!NOTE]
> <span data-ttu-id="e0a93-149">En DNS-zon också tar du bort DNS-poster i zonen hello.</span><span class="sxs-lookup"><span data-stu-id="e0a93-149">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="e0a93-150">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="e0a93-150">This operation cannot be undone.</span></span> <span data-ttu-id="e0a93-151">Om hello DNS-zon används, misslyckas tjänster med hjälp av hello zon när hello zon tas bort.</span><span class="sxs-lookup"><span data-stu-id="e0a93-151">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="e0a93-152">tooprotect mot oavsiktlig zonen radering finns [hur tooprotect DNS zoner och registrerar](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="e0a93-152">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="e0a93-153">Det här kommandot uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="e0a93-153">This command prompts for confirmation.</span></span> <span data-ttu-id="e0a93-154">hello valfria `--quiet` växla (kort form `-q`) förhindrar det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="e0a93-154">hello optional `--quiet` switch (short form `-q`) suppresses this prompt.</span></span>

<span data-ttu-id="e0a93-155">hello följande exempel visas hur toodelete hello zonen *contoso.com* från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="e0a93-155">hello following example shows how toodelete hello zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="e0a93-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0a93-156">Next steps</span></span>

<span data-ttu-id="e0a93-157">Lär dig hur för[hantera uppsättningar av poster och poster](dns-getstarted-create-recordset-cli-nodejs.md) i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="e0a93-157">Learn how too[manage record sets and records](dns-getstarted-create-recordset-cli-nodejs.md) in your DNS zone.</span></span>

<span data-ttu-id="e0a93-158">Lär dig hur för[Delegera din domän tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="e0a93-158">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

