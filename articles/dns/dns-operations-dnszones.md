---
title: Hantera DNS-zoner i Azure DNS - PowerShell | Microsoft Docs
description: "Du kan hantera DNS-zoner med hjälp av Azure Powershell. Den här artikeln beskriver hur du uppdaterar, ta bort och skapa DNS-zoner på Azure DNS"
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
ms.openlocfilehash: 92f1da660d875c76d5d826669d6c1d12018c3d0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a><span data-ttu-id="3269e-104">Hur du hanterar DNS-zoner med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="3269e-104">How to manage DNS Zones using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3269e-105">Portal</span><span class="sxs-lookup"><span data-stu-id="3269e-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="3269e-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3269e-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="3269e-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="3269e-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="3269e-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3269e-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="3269e-109">Den här artikeln visar hur du hanterar DNS-zoner med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3269e-109">This article shows you how to manage your DNS zones by using Azure PowerShell.</span></span> <span data-ttu-id="3269e-110">Du kan också hantera DNS-zoner med flera plattformar [Azure CLI](dns-operations-dnszones-cli.md) eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3269e-110">You can also manage your DNS zones using the cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or the Azure portal.</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a><span data-ttu-id="3269e-111">Skapa en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="3269e-111">Create a DNS zone</span></span>

<span data-ttu-id="3269e-112">En DNS-zon skapas med hjälp av cmdleten `New-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="3269e-112">A DNS zone is created by using the `New-AzureRmDnsZone` cmdlet.</span></span>

<span data-ttu-id="3269e-113">I följande exempel skapas en DNS-zon som kallas *contoso.com* i resursgruppen kallas *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="3269e-113">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="3269e-114">I följande exempel visas hur du skapar en DNS-zon med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*:</span><span class="sxs-lookup"><span data-stu-id="3269e-114">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="3269e-115">Hämta en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="3269e-115">Get a DNS zone</span></span>

<span data-ttu-id="3269e-116">Använd för att hämta en DNS-zon på `Get-AzureRmDnsZone` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="3269e-116">To retrieve a DNS zone, use the `Get-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="3269e-117">Den här åtgärden returnerar ett objekt för DNS-zon som motsvarar en befintlig zon i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="3269e-117">This operation returns a DNS zone object corresponding to an existing zone in Azure DNS.</span></span> <span data-ttu-id="3269e-118">Objektet innehåller information om zonen (till exempel antal uppsättningar av poster), men innehåller inte postuppsättningar själva (se `Get-AzureRmDnsRecordSet`).</span><span class="sxs-lookup"><span data-stu-id="3269e-118">The object contains data about the zone (such as the number of record sets), but does not contain the record sets themselves (see `Get-AzureRmDnsRecordSet`).</span></span>

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

## <a name="list-dns-zones"></a><span data-ttu-id="3269e-119">Lista över DNS-zoner</span><span class="sxs-lookup"><span data-stu-id="3269e-119">List DNS zones</span></span>

<span data-ttu-id="3269e-120">Genom att utelämna zonnamnet från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3269e-120">By omitting the zone name from `Get-AzureRmDnsZone`, you can enumerate all zones in a resource group.</span></span> <span data-ttu-id="3269e-121">Den här åtgärden returnerar en matris med zonen objekt.</span><span class="sxs-lookup"><span data-stu-id="3269e-121">This operation returns an array of zone objects.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="3269e-122">Genom att utelämna både zonnamnet och resursgruppens namn från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3269e-122">By omitting both the zone name and the resource group name from `Get-AzureRmDnsZone`, you can enumerate all zones in the Azure subscription.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="3269e-123">Uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="3269e-123">Update a DNS zone</span></span>

<span data-ttu-id="3269e-124">En DNS-zon resurs kan ändras med hjälp av `Set-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="3269e-124">Changes to a DNS zone resource can be made by using `Set-AzureRmDnsZone`.</span></span> <span data-ttu-id="3269e-125">Denna cmdlet inte uppdatera alla DNS-postuppsättningar i zonen (se [hur du hanterar DNS-poster](dns-operations-recordsets.md)).</span><span class="sxs-lookup"><span data-stu-id="3269e-125">This cmdlet does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets.md)).</span></span> <span data-ttu-id="3269e-126">Den används endast för att uppdatera egenskaper för resursen zonen.</span><span class="sxs-lookup"><span data-stu-id="3269e-126">It's only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="3269e-127">Skrivbar zonegenskaperna är för tillfället begränsad till den [Azure Resource Manager-taggar ”för resursen zonen](dns-zones-records.md#tags).</span><span class="sxs-lookup"><span data-stu-id="3269e-127">The writable zone properties are currently limited to the [Azure Resource Manager ‘tags’ for the zone resource](dns-zones-records.md#tags).</span></span>

<span data-ttu-id="3269e-128">Använd någon av följande två sätt att uppdatera en DNS-zon:</span><span class="sxs-lookup"><span data-stu-id="3269e-128">Use one of the following two ways to update a DNS zone:</span></span>

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a><span data-ttu-id="3269e-129">Ange zonen med namn och resursen zoner</span><span class="sxs-lookup"><span data-stu-id="3269e-129">Specify the zone using the zone name and resource group</span></span>

<span data-ttu-id="3269e-130">Den här metoden ersätter befintliga zonens taggarna med de angivna värdena.</span><span class="sxs-lookup"><span data-stu-id="3269e-130">This approach replaces the existing zone tags with the values specified.</span></span>

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a><span data-ttu-id="3269e-131">Ange zonen med ett $zone-objekt</span><span class="sxs-lookup"><span data-stu-id="3269e-131">Specify the zone using a $zone object</span></span>

<span data-ttu-id="3269e-132">Den här metoden hämtar det befintliga objektet i zonen, ändrar taggar och genomför ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3269e-132">This approach retrieves the existing zone object, modifies the tags, and then commits the changes.</span></span> <span data-ttu-id="3269e-133">På så sätt kan kan befintliga taggar bevaras.</span><span class="sxs-lookup"><span data-stu-id="3269e-133">In this way, existing tags can be preserved.</span></span>

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="3269e-134">När du använder `Set-AzureRmDnsZone` med ett $zone-objekt [Etag kontrollerar](dns-zones-records.md#etags) används för att kontrollera samtidiga ändringar inte att skrivas över.</span><span class="sxs-lookup"><span data-stu-id="3269e-134">When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="3269e-135">Du kan använda det valfria `-Overwrite` växel för att ignorera dessa kontroller.</span><span class="sxs-lookup"><span data-stu-id="3269e-135">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

## <a name="delete-a-dns-zone"></a><span data-ttu-id="3269e-136">Ta bort en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="3269e-136">Delete a DNS Zone</span></span>

<span data-ttu-id="3269e-137">DNS-zoner kan tas bort med den `Remove-AzureRmDnsZone` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="3269e-137">DNS zones can be deleted using the `Remove-AzureRmDnsZone` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="3269e-138">En DNS-zon också tar du bort DNS-poster i zonen.</span><span class="sxs-lookup"><span data-stu-id="3269e-138">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="3269e-139">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="3269e-139">This operation cannot be undone.</span></span> <span data-ttu-id="3269e-140">Om DNS-zonen misslyckas tjänster med hjälp av zonen när zonen tas bort.</span><span class="sxs-lookup"><span data-stu-id="3269e-140">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="3269e-141">För att skydda mot oavsiktlig zonen borttagning finns [Skydda DNS-zoner och poster](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="3269e-141">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>


<span data-ttu-id="3269e-142">Använd någon av följande två sätt att ta bort en DNS-zon:</span><span class="sxs-lookup"><span data-stu-id="3269e-142">Use one of the following two ways to delete a DNS zone:</span></span>

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a><span data-ttu-id="3269e-143">Ange zonen med zonnamnet och resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="3269e-143">Specify the zone using the zone name and resource group name</span></span>

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a><span data-ttu-id="3269e-144">Ange zonen med ett $zone-objekt</span><span class="sxs-lookup"><span data-stu-id="3269e-144">Specify the zone using a $zone object</span></span>

<span data-ttu-id="3269e-145">Du kan ange zonen som ska tas bort med hjälp av en `$zone` objektet som returnerades av `Get-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="3269e-145">You can specify the zone to be deleted using a `$zone` object returned by `Get-AzureRmDnsZone`.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="3269e-146">Objektet zon kan även skickas i stället för att skickas som en parameter:</span><span class="sxs-lookup"><span data-stu-id="3269e-146">The zone object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

<span data-ttu-id="3269e-147">Precis som med `Set-AzureRmDnsZone`, ange en zon med hjälp av en `$zone` objekt gör det möjligt för Etag-kontroller för att säkerställa samtidiga ändringar inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="3269e-147">As with `Set-AzureRmDnsZone`, specifying the zone using a `$zone` object enables Etag checks to ensure concurrent changes are not deleted.</span></span> <span data-ttu-id="3269e-148">Använd den `-Overwrite` växel för att ignorera dessa kontroller.</span><span class="sxs-lookup"><span data-stu-id="3269e-148">Use the `-Overwrite` switch to suppress these checks.</span></span>

## <a name="confirmation-prompts"></a><span data-ttu-id="3269e-149">Konfigurera meddelanden</span><span class="sxs-lookup"><span data-stu-id="3269e-149">Confirmation prompts</span></span>

<span data-ttu-id="3269e-150">Den `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, och `Remove-AzureRmDnsZone` cmdlets alla stöder konfigurera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3269e-150">The `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, and `Remove-AzureRmDnsZone` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="3269e-151">Båda `New-AzureRmDnsZone` och `Set-AzureRmDnsZone` fråga efter bekräftelse om de `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre.</span><span class="sxs-lookup"><span data-stu-id="3269e-151">Both `New-AzureRmDnsZone` and `Set-AzureRmDnsZone` prompt for confirmation if the `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="3269e-152">På grund av potentiellt hög effekten av att ta bort en DNS-zon på `Remove-AzureRmDnsZone` cmdlet uppmanas att bekräfta om den `$ConfirmPreference` PowerShell variabel har ett värde annat än `None`.</span><span class="sxs-lookup"><span data-stu-id="3269e-152">Due to the potentially high impact of deleting a DNS zone, the `Remove-AzureRmDnsZone` cmdlet prompts for confirmation if the `$ConfirmPreference` PowerShell variable has any value other than `None`.</span></span>

<span data-ttu-id="3269e-153">Eftersom standardvärdet för `$ConfirmPreference` är `High`, endast `Remove-AzureRmDnsZone` uppmanas att bekräfta som standard.</span><span class="sxs-lookup"><span data-stu-id="3269e-153">Since the default value for `$ConfirmPreference` is `High`, only `Remove-AzureRmDnsZone` prompts for confirmation by default.</span></span>

<span data-ttu-id="3269e-154">Du kan åsidosätta aktuellt `$ConfirmPreference` inställningen med hjälp av den `-Confirm` parameter.</span><span class="sxs-lookup"><span data-stu-id="3269e-154">You can override the current `$ConfirmPreference` setting using the `-Confirm` parameter.</span></span> <span data-ttu-id="3269e-155">Om du anger `-Confirm` eller `-Confirm:$True` , så uppmanas du att bekräfta innan den körs.</span><span class="sxs-lookup"><span data-stu-id="3269e-155">If you specify `-Confirm` or `-Confirm:$True` , the cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="3269e-156">Om du anger `-Confirm:$False` , cmdleten visas inte efter bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="3269e-156">If you specify `-Confirm:$False` , the cmdlet does not prompt you for confirmation.</span></span>

<span data-ttu-id="3269e-157">Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="3269e-157">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3269e-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3269e-158">Next steps</span></span>

<span data-ttu-id="3269e-159">Lär dig hur du [hantera uppsättningar av poster och poster](dns-operations-recordsets.md) i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="3269e-159">Learn how to [manage record sets and records](dns-operations-recordsets.md) in your DNS zone.</span></span>
<br>
<span data-ttu-id="3269e-160">Lär dig hur du [Delegera din domän till Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="3269e-160">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>
<br>
<span data-ttu-id="3269e-161">Granska de [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="3269e-161">Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>

