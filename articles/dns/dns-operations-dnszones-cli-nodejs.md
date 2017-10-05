---
title: Hantera DNS-zoner i Azure DNS - Azure CLI 1.0 | Microsoft Docs
description: "Du kan hantera DNS-zoner som använder Azure CLI 1.0. Den här artikeln visar hur du uppdatera, ta bort och skapa DNS-zoner på Azure DNS."
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
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a><span data-ttu-id="932e2-104">Hur du hanterar DNS-zoner i Azure DNS med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="932e2-104">How to manage DNS Zones in Azure DNS using the Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="932e2-105">Portal</span><span class="sxs-lookup"><span data-stu-id="932e2-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="932e2-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="932e2-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="932e2-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="932e2-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="932e2-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="932e2-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="932e2-109">Den här guiden visar hur du hanterar DNS-zoner med hjälp av plattformsoberoende Azure CLI 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="932e2-109">This guide shows how to manage your DNS zones by using the cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="932e2-110">Du kan också hantera DNS-zoner med [Azure PowerShell](dns-operations-dnszones.md) eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="932e2-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="932e2-111">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="932e2-111">CLI versions to complete the task</span></span>

<span data-ttu-id="932e2-112">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="932e2-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="932e2-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – vår CLI för distributionsmodellerna klassisk och resurshantering.</span><span class="sxs-lookup"><span data-stu-id="932e2-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="932e2-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) – vår nästa generations CLI för distributionsmodellen resurshantering.</span><span class="sxs-lookup"><span data-stu-id="932e2-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="932e2-115">Introduktion</span><span class="sxs-lookup"><span data-stu-id="932e2-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a><span data-ttu-id="932e2-116">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="932e2-116">Getting help</span></span>

<span data-ttu-id="932e2-117">Alla CLI 1.0-kommandon som är relaterade till Azure DNS börja med `azure network dns`.</span><span class="sxs-lookup"><span data-stu-id="932e2-117">All CLI 1.0 commands relating to Azure DNS start with `azure network dns`.</span></span> <span data-ttu-id="932e2-118">Hjälp är tillgänglig för varje kommando med hjälp av den `--help` alternativet (kort form `-h`).</span><span class="sxs-lookup"><span data-stu-id="932e2-118">Help is available for each command using the `--help` option (short form `-h`).</span></span>  <span data-ttu-id="932e2-119">Exempel:</span><span class="sxs-lookup"><span data-stu-id="932e2-119">For example:</span></span>

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="932e2-120">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="932e2-120">Create a DNS zone</span></span>

<span data-ttu-id="932e2-121">En DNS-zon skapas med hjälp av kommandot `azure network dns zone create`.</span><span class="sxs-lookup"><span data-stu-id="932e2-121">A DNS zone is created using the `azure network dns zone create` command.</span></span> <span data-ttu-id="932e2-122">Om du vill ha hjälp, så gå till `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="932e2-122">For help, see `azure network dns zone create -h`.</span></span>

<span data-ttu-id="932e2-123">I följande exempel skapas en DNS-zon som kallas *contoso.com* i resursgruppen kallas *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="932e2-123">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a><span data-ttu-id="932e2-124">Skapa en DNS-zon med taggar</span><span class="sxs-lookup"><span data-stu-id="932e2-124">To create a DNS zone with tags</span></span>

<span data-ttu-id="932e2-125">I följande exempel visas hur du skapar en DNS-zon med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*, med hjälp av den `--tags` parameter (kort form `-t`):</span><span class="sxs-lookup"><span data-stu-id="932e2-125">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="932e2-126">Hämta en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="932e2-126">Get a DNS zone</span></span>

<span data-ttu-id="932e2-127">Använd för att hämta en DNS-zon `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="932e2-127">To retrieve a DNS zone, use `azure network dns zone show`.</span></span> <span data-ttu-id="932e2-128">Om du vill ha hjälp, så gå till `azure network dns zone show -h`.</span><span class="sxs-lookup"><span data-stu-id="932e2-128">For help, see `azure network dns zone show -h`.</span></span>

<span data-ttu-id="932e2-129">I följande exempel returneras DNS-zonen *contoso.com* och dess associerade data från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="932e2-129">The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

<span data-ttu-id="932e2-130">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="932e2-130">The following example is the response.</span></span>

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
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

<span data-ttu-id="932e2-131">Observera att DNS-poster inte returneras av `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="932e2-131">Note that DNS records are not returned by `azure network dns zone show`.</span></span> <span data-ttu-id="932e2-132">Använd om du vill visa en lista med DNS-poster `azure network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="932e2-132">To list DNS records, use `azure network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="932e2-133">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="932e2-133">List DNS zones</span></span>

<span data-ttu-id="932e2-134">För att räkna upp DNS-zoner, Använd `azure network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="932e2-134">To enumerate DNS zones, use `azure network dns zone list`.</span></span> <span data-ttu-id="932e2-135">Om du vill ha hjälp, så gå till `azure network dns zone list -h`.</span><span class="sxs-lookup"><span data-stu-id="932e2-135">For help, see `azure network dns zone list -h`.</span></span>

<span data-ttu-id="932e2-136">Anger resursgruppens namn visas endast de zonerna i resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="932e2-136">Specifying the resource group lists only those zones within the resource group:</span></span>

```azurecli
azure network dns zone list MyResourceGroup
```

<span data-ttu-id="932e2-137">Om du utesluter resursgruppen visar en lista över alla zoner i prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="932e2-137">Omitting the resource group lists all zones in the subscription:</span></span>

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="932e2-138">Uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="932e2-138">Update a DNS zone</span></span>

<span data-ttu-id="932e2-139">En DNS-zon resurs kan ändras med hjälp av `azure network dns zone set`.</span><span class="sxs-lookup"><span data-stu-id="932e2-139">Changes to a DNS zone resource can be made using `azure network dns zone set`.</span></span> <span data-ttu-id="932e2-140">Om du vill ha hjälp, så gå till `azure network dns zone set -h`.</span><span class="sxs-lookup"><span data-stu-id="932e2-140">For help, see `azure network dns zone set -h`.</span></span>

<span data-ttu-id="932e2-141">Det här kommandot uppdaterar inte någon DNS-postuppsättningar i zonen (se [hur du hanterar DNS-poster](dns-operations-recordsets-cli-nodejs.md)).</span><span class="sxs-lookup"><span data-stu-id="932e2-141">This command does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets-cli-nodejs.md)).</span></span> <span data-ttu-id="932e2-142">Den används endast för att uppdatera egenskaper för resursen zonen.</span><span class="sxs-lookup"><span data-stu-id="932e2-142">It is only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="932e2-143">Dessa egenskaper är för tillfället begränsad till den [Azure Resource Manager ”-taggar'](dns-zones-records.md#tags) för zonen resursen.</span><span class="sxs-lookup"><span data-stu-id="932e2-143">These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.</span></span>

<span data-ttu-id="932e2-144">I följande exempel visas hur du uppdaterar taggarna i en DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="932e2-144">The following example shows how to update the tags on a DNS zone.</span></span> <span data-ttu-id="932e2-145">De befintliga taggarna ersättas med det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="932e2-145">The existing tags are replaced by the value specified.</span></span>

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="932e2-146">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="932e2-146">Delete a DNS Zone</span></span>

<span data-ttu-id="932e2-147">DNS-zoner kan tas bort med `azure network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="932e2-147">DNS zones can be deleted using `azure network dns zone delete`.</span></span> <span data-ttu-id="932e2-148">Om du vill ha hjälp, så gå till `azure network dns zone delete -h`.</span><span class="sxs-lookup"><span data-stu-id="932e2-148">For help, see `azure network dns zone delete -h`.</span></span>

> [!NOTE]
> <span data-ttu-id="932e2-149">En DNS-zon också tar du bort DNS-poster i zonen.</span><span class="sxs-lookup"><span data-stu-id="932e2-149">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="932e2-150">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="932e2-150">This operation cannot be undone.</span></span> <span data-ttu-id="932e2-151">Om DNS-zonen misslyckas tjänster med hjälp av zonen när zonen tas bort.</span><span class="sxs-lookup"><span data-stu-id="932e2-151">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="932e2-152">För att skydda mot oavsiktlig zonen borttagning finns [Skydda DNS-zoner och poster](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="932e2-152">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="932e2-153">Det här kommandot uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="932e2-153">This command prompts for confirmation.</span></span> <span data-ttu-id="932e2-154">Det valfria `--quiet` växla (kort form `-q`) förhindrar det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="932e2-154">The optional `--quiet` switch (short form `-q`) suppresses this prompt.</span></span>

<span data-ttu-id="932e2-155">I följande exempel visas hur du tar bort zonen *contoso.com* från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="932e2-155">The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="932e2-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="932e2-156">Next steps</span></span>

<span data-ttu-id="932e2-157">Lär dig hur du [hantera uppsättningar av poster och poster](dns-getstarted-create-recordset-cli-nodejs.md) i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="932e2-157">Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli-nodejs.md) in your DNS zone.</span></span>

<span data-ttu-id="932e2-158">Lär dig hur du [Delegera din domän till Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="932e2-158">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

