---
title: aaaManage DNS-zoner i Azure DNS - Azure CLI 2.0 | Microsoft Docs
description: "Du kan hantera DNS-zoner som använder Azure CLI 2.0. Den här artikeln visar hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 3945a558b2db3490e50678d8395a47e55a85c8fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-20"></a><span data-ttu-id="b9667-104">Hur toomanage DNS-zoner i Azure DNS med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b9667-104">How toomanage DNS Zones in Azure DNS using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b9667-105">Portal</span><span class="sxs-lookup"><span data-stu-id="b9667-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="b9667-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b9667-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="b9667-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b9667-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="b9667-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b9667-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)


<span data-ttu-id="b9667-109">Den här guiden visar hur toomanage DNS-zoner med hjälp av hello plattformsoberoende Azure CLI, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="b9667-109">This guide shows how toomanage your DNS zones by using hello cross-platform Azure CLI, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="b9667-110">Du kan också hantera DNS-zoner med [Azure PowerShell](dns-operations-dnszones.md) eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b9667-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or hello Azure portal.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="b9667-111">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="b9667-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="b9667-112">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="b9667-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="b9667-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="b9667-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="b9667-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering.</span><span class="sxs-lookup"><span data-stu-id="b9667-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="b9667-115">Introduktion</span><span class="sxs-lookup"><span data-stu-id="b9667-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a><span data-ttu-id="b9667-116">Konfigurera Azure CLI 2.0 för Azure DNS</span><span class="sxs-lookup"><span data-stu-id="b9667-116">Set up Azure CLI 2.0 for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="b9667-117">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b9667-117">Before you begin</span></span>

<span data-ttu-id="b9667-118">Kontrollera att du har hello följande objekt innan du börjar din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b9667-118">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="b9667-119">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b9667-119">An Azure subscription.</span></span> <span data-ttu-id="b9667-120">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b9667-120">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="b9667-121">Installera hello senaste versionen av hello Azure CLI 2.0, tillgänglig för Windows, Linux och MAC.</span><span class="sxs-lookup"><span data-stu-id="b9667-121">Install hello latest version of hello Azure CLI 2.0, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="b9667-122">Mer information finns på [installera hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="b9667-122">More information is available at [Install hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="b9667-123">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="b9667-123">Sign in tooyour Azure account</span></span>

<span data-ttu-id="b9667-124">Öppna ett konsolfönster och autentisera med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b9667-124">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="b9667-125">Mer information finns i loggen i tooAzure från hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b9667-125">For more information, see Log in tooAzure from hello Azure CLI</span></span>

```
az login
```

### <a name="select-hello-subscription"></a><span data-ttu-id="b9667-126">Välj hello-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b9667-126">Select hello subscription</span></span>

<span data-ttu-id="b9667-127">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="b9667-127">Check hello subscriptions for hello account.</span></span>

```
az account list
```

<span data-ttu-id="b9667-128">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="b9667-128">Choose which of your Azure subscriptions toouse.</span></span>

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="b9667-129">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b9667-129">Create a resource group</span></span>

<span data-ttu-id="b9667-130">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="b9667-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="b9667-131">Detta används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b9667-131">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="b9667-132">Men eftersom alla DNS-resurser är globala, inte regional, har hello valet av resursgruppens plats ingen inverkan på Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="b9667-132">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="b9667-133">Du kan hoppa över det här steget om du använder en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b9667-133">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a><span data-ttu-id="b9667-134">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="b9667-134">Getting help</span></span>

<span data-ttu-id="b9667-135">Alla CLI 2.0-kommandon som rör tooAzure DNS börja med `az network dns`.</span><span class="sxs-lookup"><span data-stu-id="b9667-135">All CLI 2.0 commands relating tooAzure DNS start with `az network dns`.</span></span> <span data-ttu-id="b9667-136">Hjälp är tillgänglig för varje kommando med hello `--help` alternativet (kort form `-h`).</span><span class="sxs-lookup"><span data-stu-id="b9667-136">Help is available for each command using hello `--help` option (short form `-h`).</span></span>  <span data-ttu-id="b9667-137">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b9667-137">For example:</span></span>

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="b9667-138">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="b9667-138">Create a DNS zone</span></span>

<span data-ttu-id="b9667-139">En DNS-zon skapas med hjälp av hello `az network dns zone create` kommando.</span><span class="sxs-lookup"><span data-stu-id="b9667-139">A DNS zone is created using hello `az network dns zone create` command.</span></span> <span data-ttu-id="b9667-140">Om du vill ha hjälp, så gå till `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="b9667-140">For help, see `az network dns zone create -h`.</span></span>

<span data-ttu-id="b9667-141">hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="b9667-141">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a><span data-ttu-id="b9667-142">toocreate en DNS-zon med taggar</span><span class="sxs-lookup"><span data-stu-id="b9667-142">toocreate a DNS zone with tags</span></span>

<span data-ttu-id="b9667-143">hello följande exempel visas hur toocreate ett DNS zonen med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*, med hjälp av hello `--tags` Parametern (kort form `-t`):</span><span class="sxs-lookup"><span data-stu-id="b9667-143">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using hello `--tags` parameter (short form `-t`):</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="b9667-144">Hämta en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="b9667-144">Get a DNS zone</span></span>

<span data-ttu-id="b9667-145">Använd tooretrieve en DNS-zon `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="b9667-145">tooretrieve a DNS zone, use `az network dns zone show`.</span></span> <span data-ttu-id="b9667-146">Om du vill ha hjälp, så gå till `az network dns zone show --help`.</span><span class="sxs-lookup"><span data-stu-id="b9667-146">For help, see `az network dns zone show --help`.</span></span>

<span data-ttu-id="b9667-147">hello följande exempel returnerar hello DNS-zonen *contoso.com* och dess associerade data från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="b9667-147">hello following example returns hello DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

<span data-ttu-id="b9667-148">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="b9667-148">hello following example is hello response.</span></span>

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="b9667-149">Observera att DNS-poster inte returneras av `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="b9667-149">Note that DNS records are not returned by `az network dns zone show`.</span></span> <span data-ttu-id="b9667-150">toolist DNS-poster, Använd `az network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="b9667-150">toolist DNS records, use `az network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="b9667-151">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="b9667-151">List DNS zones</span></span>

<span data-ttu-id="b9667-152">tooenumerate DNS-zoner, Använd `az network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="b9667-152">tooenumerate DNS zones, use `az network dns zone list`.</span></span> <span data-ttu-id="b9667-153">Om du vill ha hjälp, så gå till `az network dns zone list --help`.</span><span class="sxs-lookup"><span data-stu-id="b9667-153">For help, see `az network dns zone list --help`.</span></span>

<span data-ttu-id="b9667-154">Resursgruppen för att ange hello visas zonerna inom hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="b9667-154">Specifying hello resource group lists only those zones within hello resource group:</span></span>

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

<span data-ttu-id="b9667-155">Utelämna hello resursgrupp visar en lista över alla zoner i prenumerationen hello:</span><span class="sxs-lookup"><span data-stu-id="b9667-155">Omitting hello resource group lists all zones in hello subscription:</span></span>

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="b9667-156">Uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="b9667-156">Update a DNS zone</span></span>

<span data-ttu-id="b9667-157">Ändringar tooa DNS-zonresurs kan göras med hjälp av `az network dns zone update`.</span><span class="sxs-lookup"><span data-stu-id="b9667-157">Changes tooa DNS zone resource can be made using `az network dns zone update`.</span></span> <span data-ttu-id="b9667-158">Om du vill ha hjälp, så gå till `az network dns zone update --help`.</span><span class="sxs-lookup"><span data-stu-id="b9667-158">For help, see `az network dns zone update --help`.</span></span>

<span data-ttu-id="b9667-159">Det här kommandot uppdaterar inte någon hello DNS uppsättningar av poster i zonen hello (se [hur tooManage DNS-poster](dns-operations-recordsets-cli.md)).</span><span class="sxs-lookup"><span data-stu-id="b9667-159">This command does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets-cli.md)).</span></span> <span data-ttu-id="b9667-160">Det är bara använda tooupdate egenskaper för hello zonresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="b9667-160">It is only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="b9667-161">Dessa egenskaper är för närvarande begränsad toohello [Azure Resource Manager ”-taggar'](dns-zones-records.md#tags) för hello zonen resurs.</span><span class="sxs-lookup"><span data-stu-id="b9667-161">These properties are currently limited toohello [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for hello zone resource.</span></span>

<span data-ttu-id="b9667-162">hello följande exempel visas hur tooupdate hello taggar på en DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="b9667-162">hello following example shows how tooupdate hello tags on a DNS zone.</span></span> <span data-ttu-id="b9667-163">hello befintliga taggar ersätts med hello-värde som angetts.</span><span class="sxs-lookup"><span data-stu-id="b9667-163">hello existing tags are replaced by hello value specified.</span></span>

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="b9667-164">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="b9667-164">Delete a DNS zone</span></span>

<span data-ttu-id="b9667-165">DNS-zoner kan tas bort med `az network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="b9667-165">DNS zones can be deleted using `az network dns zone delete`.</span></span> <span data-ttu-id="b9667-166">Om du vill ha hjälp, så gå till `az network dns zone delete --help`.</span><span class="sxs-lookup"><span data-stu-id="b9667-166">For help, see `az network dns zone delete --help`.</span></span>

> [!NOTE]
> <span data-ttu-id="b9667-167">En DNS-zon också tar du bort DNS-poster i zonen hello.</span><span class="sxs-lookup"><span data-stu-id="b9667-167">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="b9667-168">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="b9667-168">This operation cannot be undone.</span></span> <span data-ttu-id="b9667-169">Om hello DNS-zon används, misslyckas tjänster med hjälp av hello zon när hello zon tas bort.</span><span class="sxs-lookup"><span data-stu-id="b9667-169">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="b9667-170">tooprotect mot oavsiktlig zonen radering finns [hur tooprotect DNS zoner och registrerar](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="b9667-170">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="b9667-171">Det här kommandot uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="b9667-171">This command prompts for confirmation.</span></span> <span data-ttu-id="b9667-172">hello valfria `--yes` växeln förhindrar det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="b9667-172">hello optional `--yes` switch suppresses this prompt.</span></span>

<span data-ttu-id="b9667-173">hello följande exempel visas hur toodelete hello zonen *contoso.com* från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="b9667-173">hello following example shows how toodelete hello zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="b9667-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9667-174">Next steps</span></span>

<span data-ttu-id="b9667-175">Lär dig hur för[hantera uppsättningar av poster och poster](dns-getstarted-create-recordset-cli.md) i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="b9667-175">Learn how too[manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.</span></span>

<span data-ttu-id="b9667-176">Lär dig hur för[Delegera din domän tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="b9667-176">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

