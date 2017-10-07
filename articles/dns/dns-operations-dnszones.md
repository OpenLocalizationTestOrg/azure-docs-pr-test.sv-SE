---
title: aaaManage DNS-zoner i Azure DNS - PowerShell | Microsoft Docs
description: "Du kan hantera DNS-zoner med hjälp av Azure Powershell. Den här artikeln beskriver hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 261b89f72213aa9784034d47ff9d1c55a4e80d65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-using-powershell"></a><span data-ttu-id="a14cd-104">Hur toomanage DNS-zoner med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="a14cd-104">How toomanage DNS Zones using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a14cd-105">Portal</span><span class="sxs-lookup"><span data-stu-id="a14cd-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="a14cd-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a14cd-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="a14cd-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a14cd-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="a14cd-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a14cd-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="a14cd-109">Den här artikeln visar hur toomanage DNS-zoner med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a14cd-109">This article shows you how toomanage your DNS zones by using Azure PowerShell.</span></span> <span data-ttu-id="a14cd-110">Du kan också hantera DNS-zoner med hello plattformsoberoende [Azure CLI](dns-operations-dnszones-cli.md) eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a14cd-110">You can also manage your DNS zones using hello cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or hello Azure portal.</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a><span data-ttu-id="a14cd-111">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a14cd-111">Create a DNS zone</span></span>

<span data-ttu-id="a14cd-112">En DNS-zon skapas med hjälp av hello `New-AzureRmDnsZone` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a14cd-112">A DNS zone is created by using hello `New-AzureRmDnsZone` cmdlet.</span></span>

<span data-ttu-id="a14cd-113">hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a14cd-113">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="a14cd-114">hello följande exempel visas hur toocreate ett DNS zonen med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*:</span><span class="sxs-lookup"><span data-stu-id="a14cd-114">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="a14cd-115">Hämta en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a14cd-115">Get a DNS zone</span></span>

<span data-ttu-id="a14cd-116">tooretrieve en DNS-zon använder hello `Get-AzureRmDnsZone` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a14cd-116">tooretrieve a DNS zone, use hello `Get-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="a14cd-117">Den här åtgärden returnerar ett DNS-zonen objektet motsvarande tooan befintlig zon i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="a14cd-117">This operation returns a DNS zone object corresponding tooan existing zone in Azure DNS.</span></span> <span data-ttu-id="a14cd-118">hello objekt innehåller information om hello zonen (till exempel hello Antal uppsättningar av poster), men innehåller inte hello postuppsättningar själva (se `Get-AzureRmDnsRecordSet`).</span><span class="sxs-lookup"><span data-stu-id="a14cd-118">hello object contains data about hello zone (such as hello number of record sets), but does not contain hello record sets themselves (see `Get-AzureRmDnsRecordSet`).</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a><span data-ttu-id="a14cd-119">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="a14cd-119">List DNS zones</span></span>

<span data-ttu-id="a14cd-120">Genom att utelämna hello zonnamnet från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a14cd-120">By omitting hello zone name from `Get-AzureRmDnsZone`, you can enumerate all zones in a resource group.</span></span> <span data-ttu-id="a14cd-121">Den här åtgärden returnerar en matris med zonen objekt.</span><span class="sxs-lookup"><span data-stu-id="a14cd-121">This operation returns an array of zone objects.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="a14cd-122">Genom att utelämna både hello zonnamnet och hello resursgruppens namn från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a14cd-122">By omitting both hello zone name and hello resource group name from `Get-AzureRmDnsZone`, you can enumerate all zones in hello Azure subscription.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="a14cd-123">Uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a14cd-123">Update a DNS zone</span></span>

<span data-ttu-id="a14cd-124">Ändrar tooa DNS-zonresurs kan göras med hjälp av `Set-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="a14cd-124">Changes tooa DNS zone resource can be made by using `Set-AzureRmDnsZone`.</span></span> <span data-ttu-id="a14cd-125">Denna cmdlet inte uppdatera alla hello DNS-postuppsättningar i zonen hello (se [hur tooManage DNS-poster](dns-operations-recordsets.md)).</span><span class="sxs-lookup"><span data-stu-id="a14cd-125">This cmdlet does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets.md)).</span></span> <span data-ttu-id="a14cd-126">Den används endast tooupdate egenskaper för hello zonresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="a14cd-126">It's only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="a14cd-127">hello skrivbar zonegenskaperna är för närvarande begränsad toohello [Azure Resource Manager-taggar' för hello zonresurs](dns-zones-records.md#tags).</span><span class="sxs-lookup"><span data-stu-id="a14cd-127">hello writable zone properties are currently limited toohello [Azure Resource Manager ‘tags’ for hello zone resource](dns-zones-records.md#tags).</span></span>

<span data-ttu-id="a14cd-128">Använd någon av följande två sätt tooupdate hello en DNS-zon:</span><span class="sxs-lookup"><span data-stu-id="a14cd-128">Use one of hello following two ways tooupdate a DNS zone:</span></span>

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group"></a><span data-ttu-id="a14cd-129">Ange hello zon med hjälp av hello namn och en resurs i zoner</span><span class="sxs-lookup"><span data-stu-id="a14cd-129">Specify hello zone using hello zone name and resource group</span></span>

<span data-ttu-id="a14cd-130">Den här metoden ersätter hello befintliga zonen taggar med hello värden som har angetts.</span><span class="sxs-lookup"><span data-stu-id="a14cd-130">This approach replaces hello existing zone tags with hello values specified.</span></span>

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-hello-zone-using-a-zone-object"></a><span data-ttu-id="a14cd-131">Ange hello zon med ett $zone-objekt</span><span class="sxs-lookup"><span data-stu-id="a14cd-131">Specify hello zone using a $zone object</span></span>

<span data-ttu-id="a14cd-132">Den här metoden hämtar hello befintlig zon objekt, ändrar hello taggar och genomför ändringar som hello.</span><span class="sxs-lookup"><span data-stu-id="a14cd-132">This approach retrieves hello existing zone object, modifies hello tags, and then commits hello changes.</span></span> <span data-ttu-id="a14cd-133">På så sätt kan kan befintliga taggar bevaras.</span><span class="sxs-lookup"><span data-stu-id="a14cd-133">In this way, existing tags can be preserved.</span></span>

```powershell
# Get hello zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="a14cd-134">När du använder `Set-AzureRmDnsZone` med ett $zone-objekt [Etag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar inte att skrivas över.</span><span class="sxs-lookup"><span data-stu-id="a14cd-134">When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="a14cd-135">Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.</span><span class="sxs-lookup"><span data-stu-id="a14cd-135">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

## <a name="delete-a-dns-zone"></a><span data-ttu-id="a14cd-136">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="a14cd-136">Delete a DNS Zone</span></span>

<span data-ttu-id="a14cd-137">DNS-zoner kan tas bort med hello `Remove-AzureRmDnsZone` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a14cd-137">DNS zones can be deleted using hello `Remove-AzureRmDnsZone` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="a14cd-138">En DNS-zon också tar du bort DNS-poster i zonen hello.</span><span class="sxs-lookup"><span data-stu-id="a14cd-138">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="a14cd-139">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="a14cd-139">This operation cannot be undone.</span></span> <span data-ttu-id="a14cd-140">Om hello DNS-zon används, misslyckas tjänster med hjälp av hello zon när hello zon tas bort.</span><span class="sxs-lookup"><span data-stu-id="a14cd-140">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="a14cd-141">tooprotect mot oavsiktlig zonen radering finns [hur tooprotect DNS zoner och registrerar](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="a14cd-141">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>


<span data-ttu-id="a14cd-142">Använd någon av följande två sätt toodelete hello en DNS-zon:</span><span class="sxs-lookup"><span data-stu-id="a14cd-142">Use one of hello following two ways toodelete a DNS zone:</span></span>

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group-name"></a><span data-ttu-id="a14cd-143">Ange hello zon med hjälp av hello zonnamnet och resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="a14cd-143">Specify hello zone using hello zone name and resource group name</span></span>

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-hello-zone-using-a-zone-object"></a><span data-ttu-id="a14cd-144">Ange hello zon med ett $zone-objekt</span><span class="sxs-lookup"><span data-stu-id="a14cd-144">Specify hello zone using a $zone object</span></span>

<span data-ttu-id="a14cd-145">Du kan ange hello zonen toobe tas bort med hjälp av en `$zone` objektet som returnerades av `Get-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="a14cd-145">You can specify hello zone toobe deleted using a `$zone` object returned by `Get-AzureRmDnsZone`.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="a14cd-146">hello zonen objekt kan även skickas i stället för att skickas som en parameter:</span><span class="sxs-lookup"><span data-stu-id="a14cd-146">hello zone object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

<span data-ttu-id="a14cd-147">Precis som med `Set-AzureRmDnsZone`, att ange hello zon med hjälp av en `$zone` objekt aktiverar Etag kontrollerar tooensure samtidiga ändringar inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="a14cd-147">As with `Set-AzureRmDnsZone`, specifying hello zone using a `$zone` object enables Etag checks tooensure concurrent changes are not deleted.</span></span> <span data-ttu-id="a14cd-148">Använd hello `-Overwrite` växla toosuppress kontrollerna.</span><span class="sxs-lookup"><span data-stu-id="a14cd-148">Use hello `-Overwrite` switch toosuppress these checks.</span></span>

## <a name="confirmation-prompts"></a><span data-ttu-id="a14cd-149">Konfigurera meddelanden</span><span class="sxs-lookup"><span data-stu-id="a14cd-149">Confirmation prompts</span></span>

<span data-ttu-id="a14cd-150">Hej `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, och `Remove-AzureRmDnsZone` cmdlets alla stöder konfigurera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a14cd-150">hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, and `Remove-AzureRmDnsZone` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="a14cd-151">Båda `New-AzureRmDnsZone` och `Set-AzureRmDnsZone` fråga efter bekräftelse om hello `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre.</span><span class="sxs-lookup"><span data-stu-id="a14cd-151">Both `New-AzureRmDnsZone` and `Set-AzureRmDnsZone` prompt for confirmation if hello `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="a14cd-152">På grund av toohello potentiellt hög inverkan av att ta bort en DNS-zon hello `Remove-AzureRmDnsZone` cmdlet uppmanas att bekräfta om hello `$ConfirmPreference` PowerShell variabel har ett värde annat än `None`.</span><span class="sxs-lookup"><span data-stu-id="a14cd-152">Due toohello potentially high impact of deleting a DNS zone, hello `Remove-AzureRmDnsZone` cmdlet prompts for confirmation if hello `$ConfirmPreference` PowerShell variable has any value other than `None`.</span></span>

<span data-ttu-id="a14cd-153">Eftersom hello standardvärdet för `$ConfirmPreference` är `High`, endast `Remove-AzureRmDnsZone` uppmanas att bekräfta som standard.</span><span class="sxs-lookup"><span data-stu-id="a14cd-153">Since hello default value for `$ConfirmPreference` is `High`, only `Remove-AzureRmDnsZone` prompts for confirmation by default.</span></span>

<span data-ttu-id="a14cd-154">Du kan åsidosätta hello aktuella `$ConfirmPreference` inställningen med hello `-Confirm` parameter.</span><span class="sxs-lookup"><span data-stu-id="a14cd-154">You can override hello current `$ConfirmPreference` setting using hello `-Confirm` parameter.</span></span> <span data-ttu-id="a14cd-155">Om du anger `-Confirm` eller `-Confirm:$True` , hello cmdlet uppmanar dig att bekräfta innan den körs.</span><span class="sxs-lookup"><span data-stu-id="a14cd-155">If you specify `-Confirm` or `-Confirm:$True` , hello cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="a14cd-156">Om du anger `-Confirm:$False` , hello cmdleten visas inte efter bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="a14cd-156">If you specify `-Confirm:$False` , hello cmdlet does not prompt you for confirmation.</span></span>

<span data-ttu-id="a14cd-157">Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="a14cd-157">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a14cd-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a14cd-158">Next steps</span></span>

<span data-ttu-id="a14cd-159">Lär dig hur för[hantera uppsättningar av poster och poster](dns-operations-recordsets.md) i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="a14cd-159">Learn how too[manage record sets and records](dns-operations-recordsets.md) in your DNS zone.</span></span>
<br>
<span data-ttu-id="a14cd-160">Lär dig hur för[Delegera din domän tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="a14cd-160">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>
<br>
<span data-ttu-id="a14cd-161">Granska hello [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="a14cd-161">Review hello [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>

