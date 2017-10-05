---
title: Hantera DNS-zoner i Azure DNS - Azure CLI 2.0 | Microsoft Docs
description: "Du kan hantera DNS-zoner som använder Azure CLI 2.0. Den här artikeln visar hur du uppdatera, ta bort och skapa DNS-zoner på Azure DNS."
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
ms.openlocfilehash: 1414baf9e51d648cc3a46c4f8635040b4d276910
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a><span data-ttu-id="a8b06-104">Hur du hanterar DNS-zoner i Azure DNS använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a8b06-104">How to manage DNS Zones in Azure DNS using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a8b06-105">Portal</span><span class="sxs-lookup"><span data-stu-id="a8b06-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="a8b06-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8b06-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="a8b06-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a8b06-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="a8b06-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a8b06-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)


<span data-ttu-id="a8b06-109">Den här guiden visar hur du hanterar DNS-zoner med hjälp av plattformsoberoende Azure CLI, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="a8b06-109">This guide shows how to manage your DNS zones by using the cross-platform Azure CLI, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="a8b06-110">Du kan också hantera DNS-zoner med [Azure PowerShell](dns-operations-dnszones.md) eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="a8b06-111">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="a8b06-111">CLI versions to complete the task</span></span>

<span data-ttu-id="a8b06-112">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="a8b06-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="a8b06-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – vår CLI för distributionsmodellerna klassisk och resurshantering.</span><span class="sxs-lookup"><span data-stu-id="a8b06-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="a8b06-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) – vår nästa generations CLI för distributionsmodellen resurshantering.</span><span class="sxs-lookup"><span data-stu-id="a8b06-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="a8b06-115">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a8b06-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a><span data-ttu-id="a8b06-116">Konfigurera Azure CLI 2.0 för Azure DNS</span><span class="sxs-lookup"><span data-stu-id="a8b06-116">Set up Azure CLI 2.0 for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="a8b06-117">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a8b06-117">Before you begin</span></span>

<span data-ttu-id="a8b06-118">Kontrollera att du har följande innan du påbörjar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-118">Verify that you have the following items before beginning your configuration.</span></span>

* <span data-ttu-id="a8b06-119">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a8b06-119">An Azure subscription.</span></span> <span data-ttu-id="a8b06-120">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8b06-120">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="a8b06-121">Installera den senaste versionen av Azure CLI 2.0, tillgänglig för Windows, Linux och MAC.</span><span class="sxs-lookup"><span data-stu-id="a8b06-121">Install the latest version of the Azure CLI 2.0, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="a8b06-122">Mer information finns på [Installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="a8b06-122">More information is available at [Install the Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

### <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="a8b06-123">Logga in på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="a8b06-123">Sign in to your Azure account</span></span>

<span data-ttu-id="a8b06-124">Öppna ett konsolfönster och autentisera med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a8b06-124">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="a8b06-125">Mer information finns i Logga in i Azure från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a8b06-125">For more information, see Log in to Azure from the Azure CLI</span></span>

```
az login
```

### <a name="select-the-subscription"></a><span data-ttu-id="a8b06-126">Välja prenumerationen</span><span class="sxs-lookup"><span data-stu-id="a8b06-126">Select the subscription</span></span>

<span data-ttu-id="a8b06-127">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="a8b06-127">Check the subscriptions for the account.</span></span>

```
az account list
```

<span data-ttu-id="a8b06-128">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="a8b06-128">Choose which of your Azure subscriptions to use.</span></span>

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="a8b06-129">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a8b06-129">Create a resource group</span></span>

<span data-ttu-id="a8b06-130">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="a8b06-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="a8b06-131">Detta används som standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-131">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="a8b06-132">Men eftersom alla DNS-resurser är globala, inte regionala, så påverkar inte valet av resursgruppens plats Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="a8b06-132">However, because all DNS resources are global, not regional, the choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="a8b06-133">Du kan hoppa över det här steget om du använder en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a8b06-133">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a><span data-ttu-id="a8b06-134">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="a8b06-134">Getting help</span></span>

<span data-ttu-id="a8b06-135">Alla CLI 2.0-kommandon som är relaterade till Azure DNS börja med `az network dns`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-135">All CLI 2.0 commands relating to Azure DNS start with `az network dns`.</span></span> <span data-ttu-id="a8b06-136">Hjälp är tillgänglig för varje kommando med hjälp av den `--help` alternativet (kort form `-h`).</span><span class="sxs-lookup"><span data-stu-id="a8b06-136">Help is available for each command using the `--help` option (short form `-h`).</span></span>  <span data-ttu-id="a8b06-137">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8b06-137">For example:</span></span>

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="a8b06-138">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a8b06-138">Create a DNS zone</span></span>

<span data-ttu-id="a8b06-139">En DNS-zon skapas med hjälp av kommandot `az network dns zone create`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-139">A DNS zone is created using the `az network dns zone create` command.</span></span> <span data-ttu-id="a8b06-140">Om du vill ha hjälp, så gå till `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-140">For help, see `az network dns zone create -h`.</span></span>

<span data-ttu-id="a8b06-141">I följande exempel skapas en DNS-zon som kallas *contoso.com* i resursgruppen kallas *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a8b06-141">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a><span data-ttu-id="a8b06-142">Skapa en DNS-zon med taggar</span><span class="sxs-lookup"><span data-stu-id="a8b06-142">To create a DNS zone with tags</span></span>

<span data-ttu-id="a8b06-143">I följande exempel visas hur du skapar en DNS-zon med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*, med hjälp av den `--tags` parameter (kort form `-t`):</span><span class="sxs-lookup"><span data-stu-id="a8b06-143">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="a8b06-144">Hämta en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a8b06-144">Get a DNS zone</span></span>

<span data-ttu-id="a8b06-145">Använd för att hämta en DNS-zon `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-145">To retrieve a DNS zone, use `az network dns zone show`.</span></span> <span data-ttu-id="a8b06-146">Om du vill ha hjälp, så gå till `az network dns zone show --help`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-146">For help, see `az network dns zone show --help`.</span></span>

<span data-ttu-id="a8b06-147">I följande exempel returneras DNS-zonen *contoso.com* och dess associerade data från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="a8b06-147">The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

<span data-ttu-id="a8b06-148">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="a8b06-148">The following example is the response.</span></span>

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

<span data-ttu-id="a8b06-149">Observera att DNS-poster inte returneras av `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-149">Note that DNS records are not returned by `az network dns zone show`.</span></span> <span data-ttu-id="a8b06-150">Använd om du vill visa en lista med DNS-poster `az network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-150">To list DNS records, use `az network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="a8b06-151">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="a8b06-151">List DNS zones</span></span>

<span data-ttu-id="a8b06-152">För att räkna upp DNS-zoner, Använd `az network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-152">To enumerate DNS zones, use `az network dns zone list`.</span></span> <span data-ttu-id="a8b06-153">Om du vill ha hjälp, så gå till `az network dns zone list --help`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-153">For help, see `az network dns zone list --help`.</span></span>

<span data-ttu-id="a8b06-154">Anger resursgruppens namn visas endast de zonerna i resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="a8b06-154">Specifying the resource group lists only those zones within the resource group:</span></span>

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

<span data-ttu-id="a8b06-155">Om du utesluter resursgruppen visar en lista över alla zoner i prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="a8b06-155">Omitting the resource group lists all zones in the subscription:</span></span>

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="a8b06-156">Uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a8b06-156">Update a DNS zone</span></span>

<span data-ttu-id="a8b06-157">En DNS-zon resurs kan ändras med hjälp av `az network dns zone update`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-157">Changes to a DNS zone resource can be made using `az network dns zone update`.</span></span> <span data-ttu-id="a8b06-158">Om du vill ha hjälp, så gå till `az network dns zone update --help`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-158">For help, see `az network dns zone update --help`.</span></span>

<span data-ttu-id="a8b06-159">Det här kommandot uppdaterar inte någon DNS-postuppsättningar i zonen (se [hur du hanterar DNS-poster](dns-operations-recordsets-cli.md)).</span><span class="sxs-lookup"><span data-stu-id="a8b06-159">This command does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets-cli.md)).</span></span> <span data-ttu-id="a8b06-160">Den används endast för att uppdatera egenskaper för resursen zonen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-160">It is only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="a8b06-161">Dessa egenskaper är för tillfället begränsad till den [Azure Resource Manager ”-taggar'](dns-zones-records.md#tags) för zonen resursen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-161">These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.</span></span>

<span data-ttu-id="a8b06-162">I följande exempel visas hur du uppdaterar taggarna i en DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="a8b06-162">The following example shows how to update the tags on a DNS zone.</span></span> <span data-ttu-id="a8b06-163">De befintliga taggarna ersättas med det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="a8b06-163">The existing tags are replaced by the value specified.</span></span>

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="a8b06-164">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a8b06-164">Delete a DNS zone</span></span>

<span data-ttu-id="a8b06-165">DNS-zoner kan tas bort med `az network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-165">DNS zones can be deleted using `az network dns zone delete`.</span></span> <span data-ttu-id="a8b06-166">Om du vill ha hjälp, så gå till `az network dns zone delete --help`.</span><span class="sxs-lookup"><span data-stu-id="a8b06-166">For help, see `az network dns zone delete --help`.</span></span>

> [!NOTE]
> <span data-ttu-id="a8b06-167">En DNS-zon också tar du bort DNS-poster i zonen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-167">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="a8b06-168">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="a8b06-168">This operation cannot be undone.</span></span> <span data-ttu-id="a8b06-169">Om DNS-zonen misslyckas tjänster med hjälp av zonen när zonen tas bort.</span><span class="sxs-lookup"><span data-stu-id="a8b06-169">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="a8b06-170">För att skydda mot oavsiktlig zonen borttagning finns [Skydda DNS-zoner och poster](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="a8b06-170">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="a8b06-171">Det här kommandot uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="a8b06-171">This command prompts for confirmation.</span></span> <span data-ttu-id="a8b06-172">Det valfria `--yes` växeln förhindrar det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a8b06-172">The optional `--yes` switch suppresses this prompt.</span></span>

<span data-ttu-id="a8b06-173">I följande exempel visas hur du tar bort zonen *contoso.com* från resursgruppen *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="a8b06-173">The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="a8b06-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8b06-174">Next steps</span></span>

<span data-ttu-id="a8b06-175">Lär dig hur du [hantera uppsättningar av poster och poster](dns-getstarted-create-recordset-cli.md) i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="a8b06-175">Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.</span></span>

<span data-ttu-id="a8b06-176">Lär dig hur du [Delegera din domän till Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="a8b06-176">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

