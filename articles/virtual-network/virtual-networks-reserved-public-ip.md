---
title: aaaManage Azure reserverad IP-adress (klassisk) - PowerShell | Microsoft Docs
description: "Förstå reserverade IP-adresser (klassisk) och hur toomanage dem med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: c0a77b2ab8b1ab9bef6015c903eb735ea4358a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reserved-ip-addresses-classic"></a><span data-ttu-id="bd416-103">Den reserverade IP-adresser (klassisk)</span><span class="sxs-lookup"><span data-stu-id="bd416-103">Reserved IP addresses (Classic)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bd416-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bd416-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="bd416-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd416-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="bd416-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bd416-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="bd416-107">Mall</span><span class="sxs-lookup"><span data-stu-id="bd416-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="bd416-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="bd416-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

<span data-ttu-id="bd416-109">IP-adresser i Azure är indelade i två kategorier: dynamiska och reserverade.</span><span class="sxs-lookup"><span data-stu-id="bd416-109">IP addresses in Azure fall into two categories: dynamic and reserved.</span></span> <span data-ttu-id="bd416-110">Offentliga IP-adresser som hanteras av Azure är dynamiska som standard.</span><span class="sxs-lookup"><span data-stu-id="bd416-110">Public IP addresses managed by Azure are dynamic by default.</span></span> <span data-ttu-id="bd416-111">Detta innebär att hello IP-adress som används för en viss molntjänst (VIP) eller tooaccess som en virtuell dator eller rollen instans direkt (går) kan ändra från tid tootime när resurserna är avstängd eller stoppats (frigjorts).</span><span class="sxs-lookup"><span data-stu-id="bd416-111">That means that hello IP address used for a given cloud service (VIP) or tooaccess a VM or role instance directly (ILPIP) can change from time tootime, when resources are shut down or stopped (deallocated).</span></span>

<span data-ttu-id="bd416-112">tooprevent IP-adresser från att ändra, du kan reservera en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bd416-112">tooprevent IP addresses from changing, you can reserve an IP address.</span></span> <span data-ttu-id="bd416-113">Reserverade IP-adresser kan endast användas som en VIP säkerställer att hello IP-adress för hello Molntjänsten förblir hello detsamma, även om resurserna är avstängd eller stoppats (frigjorts).</span><span class="sxs-lookup"><span data-stu-id="bd416-113">Reserved IPs can be used only as a VIP, ensuring that hello IP address for hello cloud service remains hello same, even as resources are shut down or stopped (deallocated).</span></span> <span data-ttu-id="bd416-114">Dessutom kan du konvertera befintliga dynamiska IP-adresser används som en VIP tooa reserverad IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bd416-114">Furthermore, you can convert existing dynamic IPs used as a VIP tooa reserved IP address.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bd416-115">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bd416-115">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bd416-116">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="bd416-116">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="bd416-117">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="bd416-117">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="bd416-118">Lär dig hur tooreserve en statisk offentlig IP-adress med hjälp av hello [Resource Manager-distributionsmodellen](virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="bd416-118">Learn how tooreserve a static public IP address using hello [Resource Manager deployment model](virtual-network-ip-addresses-overview-arm.md).</span></span>

<span data-ttu-id="bd416-119">toolearn mer om IP-adresser i Azure kan läsa hello [IP-adresser](virtual-network-ip-addresses-overview-classic.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd416-119">toolearn more about IP addresses in Azure, read hello [IP addresses](virtual-network-ip-addresses-overview-classic.md) article.</span></span>

## <a name="when-do-i-need-a-reserved-ip"></a><span data-ttu-id="bd416-120">När behöver en reserverad IP?</span><span class="sxs-lookup"><span data-stu-id="bd416-120">When do I need a reserved IP?</span></span>
* <span data-ttu-id="bd416-121">**Du vill tooensure som hello IP är reserverad i din prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="bd416-121">**You want tooensure that hello IP is reserved in your subscription**.</span></span> <span data-ttu-id="bd416-122">Om du vill tooreserve en IP-adress som inte har släppts från din prenumeration under några omständigheter, bör du använda en reserverade offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bd416-122">If you want tooreserve an IP address that is not released from your subscription under any circumstance, you should use a reserved public IP.</span></span>  
* <span data-ttu-id="bd416-123">**Du vill att din IP-toostay med Molntjänsten även tvärs över stoppas eller frigöra tillstånd (VM)**.</span><span class="sxs-lookup"><span data-stu-id="bd416-123">**You want your IP toostay with your cloud service even across stopped or deallocated state (VMs)**.</span></span> <span data-ttu-id="bd416-124">Om du vill att din tjänst toobe som nås via en IP-adress som inte ändras även om virtuella datorer i hello cloud service är avstängd eller stoppa (frigjorts).</span><span class="sxs-lookup"><span data-stu-id="bd416-124">If you want your service toobe accessed by using an IP address that doesn't change, even when VMs in hello cloud service are shut down or stop (deallocated).</span></span>
* <span data-ttu-id="bd416-125">**Du vill att utgående trafik från Azure använder en förutsägbar IP-adress tooensure**.</span><span class="sxs-lookup"><span data-stu-id="bd416-125">**You want tooensure that outbound traffic from Azure uses a predictable IP address**.</span></span> <span data-ttu-id="bd416-126">Du kan ha din lokala konfigurerad brandvägg tooallow endast trafik från särskilda IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="bd416-126">You may have your on-premises firewall configured tooallow only traffic from specific IP addresses.</span></span> <span data-ttu-id="bd416-127">Genom att reservera en IP-adress som du vet hello käll-IP-adress och behöver inte tooupdate brandväggsregler på grund av tooan förändring av IP.</span><span class="sxs-lookup"><span data-stu-id="bd416-127">By reserving an IP, you know hello source IP address, and don't need tooupdate your firewall rules due tooan IP change.</span></span>

## <a name="faq"></a><span data-ttu-id="bd416-128">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="bd416-128">FAQ</span></span>
1. <span data-ttu-id="bd416-129">Kan jag använda en reserverad IP-adress för alla Azure-tjänster?</span><span class="sxs-lookup"><span data-stu-id="bd416-129">Can I use a reserved IP for all Azure services?</span></span> <br>
    <span data-ttu-id="bd416-130">Nej.</span><span class="sxs-lookup"><span data-stu-id="bd416-130">No.</span></span> <span data-ttu-id="bd416-131">Reserverade IP-adresser kan bara användas för virtuella datorer och instans molntjänstroller exponeras via en VIP.</span><span class="sxs-lookup"><span data-stu-id="bd416-131">Reserved IPs can only be used for VMs and cloud service instance roles exposed through a VIP.</span></span>
2. <span data-ttu-id="bd416-132">Hur många reserverade IP-adresser kan ha?</span><span class="sxs-lookup"><span data-stu-id="bd416-132">How many reserved IPs can I have?</span></span> <br>
    <span data-ttu-id="bd416-133">Mer information finns i hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd416-133">For details, see hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
3. <span data-ttu-id="bd416-134">Finns det en avgift för reserverade IP-adresser?</span><span class="sxs-lookup"><span data-stu-id="bd416-134">Is there a charge for reserved IPs?</span></span> <br>
    <span data-ttu-id="bd416-135">Ibland.</span><span class="sxs-lookup"><span data-stu-id="bd416-135">Sometimes.</span></span> <span data-ttu-id="bd416-136">Prisinformation, finns hello [reserverade IP-adress prisinformation](http://go.microsoft.com/fwlink/?LinkID=398482) sidan.</span><span class="sxs-lookup"><span data-stu-id="bd416-136">For pricing details, see hello [Reserved IP Address Pricing Details](http://go.microsoft.com/fwlink/?LinkID=398482) page.</span></span>
4. <span data-ttu-id="bd416-137">Hur jag för att reservera en IP-adress?</span><span class="sxs-lookup"><span data-stu-id="bd416-137">How do I reserve an IP address?</span></span> <br>
    <span data-ttu-id="bd416-138">Du kan använda PowerShell hello [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), eller hello [Azure-portalen](https://portal.azure.com) tooreserve en IP-adress i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="bd416-138">You can use PowerShell, hello [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), or hello [Azure portal](https://portal.azure.com) tooreserve an IP address in an Azure region.</span></span> <span data-ttu-id="bd416-139">En reserverad IP-adress är associerade tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd416-139">A reserved IP address is associated tooyour subscription.</span></span>
5. <span data-ttu-id="bd416-140">Kan jag använda en reserverad IP med tillhörighetsgrupp-baserade Vnet?</span><span class="sxs-lookup"><span data-stu-id="bd416-140">Can I use a reserved IP with affinity group-based VNets?</span></span> <br>
    <span data-ttu-id="bd416-141">Nej.</span><span class="sxs-lookup"><span data-stu-id="bd416-141">No.</span></span> <span data-ttu-id="bd416-142">Reserverade IP-adresser stöds endast i regionala virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="bd416-142">Reserved IPs are only supported in regional VNets.</span></span> <span data-ttu-id="bd416-143">Reserverade IP-adresser stöds inte för Vnet som är associerade med tillhörighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="bd416-143">Reserved IPs are not supported for VNets that are associated with affinity groups.</span></span> <span data-ttu-id="bd416-144">Mer information om hur du kopplar ett VNet med en region eller affinitetsgrupp finns hello [om Regional Vnet och Tillhörighetsgrupper](virtual-networks-migrate-to-regional-vnet.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd416-144">For more information about associating a VNet with a region or affinity group, see hello [About Regional VNets and Affinity Groups](virtual-networks-migrate-to-regional-vnet.md) article.</span></span>

## <a name="manage-reserved-vips"></a><span data-ttu-id="bd416-145">Hantera reserverade virtuella IP-adresser</span><span class="sxs-lookup"><span data-stu-id="bd416-145">Manage reserved VIPs</span></span>

<span data-ttu-id="bd416-146">Se till att du har installerat och konfigurerat PowerShell genom att slutföra hello stegen i hello [installerar och konfigurerar PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd416-146">Ensure you have installed and configured PowerShell by completing hello steps in hello [Install and configure PowerShell](/powershell/azure/overview) article.</span></span> 

<span data-ttu-id="bd416-147">Innan du kan använda reserverade IP-adresser måste du lägga till den tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd416-147">Before you can use reserved IPs, you must add it tooyour subscription.</span></span> <span data-ttu-id="bd416-148">toocreate en reserverad IP från hello-pool med offentliga IP-adresser i hello *centrala USA* plats, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bd416-148">toocreate a reserved IP from hello pool of public IP addresses available in hello *Central US* location, run hello following command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

<span data-ttu-id="bd416-149">Observera att du kan ange vilka IP som reserverats.</span><span class="sxs-lookup"><span data-stu-id="bd416-149">Notice, however, that you cannot specify what IP is being reserved.</span></span> <span data-ttu-id="bd416-150">tooview vilka IP-adresser är reserverade i din prenumeration, kör följande PowerShell-kommando hello och Observera hello värden för *ReservedIPName* och *adressen*:</span><span class="sxs-lookup"><span data-stu-id="bd416-150">tooview what IP addresses are reserved in your subscription, run hello following PowerShell command, and notice hello values for *ReservedIPName* and *Address*:</span></span>

```powershell
Get-AzureReservedIP
```

<span data-ttu-id="bd416-151">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="bd416-151">Expected output:</span></span>

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
><span data-ttu-id="bd416-152">När du skapar en reserverad IP-adress med PowerShell kan ange du inte en resurs grupp toocreate hello reserverade IP-adress i.</span><span class="sxs-lookup"><span data-stu-id="bd416-152">When you create a reserved IP address with PowerShell, you cannot specify a resource group toocreate hello reserved IP in.</span></span> <span data-ttu-id="bd416-153">Azure platser till en resursgrupp med namnet *standard-nätverk* automatiskt.</span><span class="sxs-lookup"><span data-stu-id="bd416-153">Azure places it into a resource group named *Default-Networking* automatically.</span></span> <span data-ttu-id="bd416-154">Om du skapar hello reserverad IP med hello [Azure-portalen](http://portal.azure.com), du kan ange valfri resursgrupp som du väljer.</span><span class="sxs-lookup"><span data-stu-id="bd416-154">If you create hello reserved IP using hello [Azure portal](http://portal.azure.com), you can specify any resource group you choose.</span></span> <span data-ttu-id="bd416-155">Om du skapar hello reserverad IP i en resursgrupp än *standard-nätverk* dock när du refererar till hello reserverade IP-Adressen med kommandon som `Get-AzureReservedIP` och `Remove-AzureReservedIP`, måste du referera hello namn *Gruppnamn resursgruppnamn-reserverad-ip*.</span><span class="sxs-lookup"><span data-stu-id="bd416-155">If you create hello reserved IP in a resource group other than *Default-Networking* however, whenever you reference hello reserved IP with commands such as `Get-AzureReservedIP` and `Remove-AzureReservedIP`, you must reference hello name *Group resource-group-name reserved-ip-name*.</span></span>  <span data-ttu-id="bd416-156">Till exempel om du skapar en reserverad IP med namnet *myReservedIP* i en resursgrupp med namnet *myResourceGroup*, måste du referera hello namnet på hello reserverade IP-adress som *grupp myResourceGroup myReservedIP*.</span><span class="sxs-lookup"><span data-stu-id="bd416-156">For example, if you create a reserved IP named *myReservedIP* in a resource group named *myResourceGroup*, you must reference hello name of hello reserved IP as *Group myResourceGroup myReservedIP*.</span></span>   

<span data-ttu-id="bd416-157">När en IP-adress är reserverat förblir associerade tooyour prenumeration tills du tar bort den.</span><span class="sxs-lookup"><span data-stu-id="bd416-157">Once an IP is reserved, it remains associated tooyour subscription until you delete it.</span></span> <span data-ttu-id="bd416-158">toodelete en reserverad IP kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="bd416-158">toodelete a reserved IP, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-hello-ip-address-of-an-existing-cloud-service"></a><span data-ttu-id="bd416-159">Reservera hello IP-adressen för en befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="bd416-159">Reserve hello IP address of an existing cloud service</span></span>
<span data-ttu-id="bd416-160">Du kan reservera hello IP-adressen för en befintlig molntjänst genom att lägga till hello `-ServiceName` parameter.</span><span class="sxs-lookup"><span data-stu-id="bd416-160">You can reserve hello IP address of an existing cloud service by adding hello `-ServiceName` parameter.</span></span> <span data-ttu-id="bd416-161">tooreserve hello IP-adressen för en molnbaserad tjänst *TestService* i hello *centrala USA* plats, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="bd416-161">tooreserve hello IP address of a cloud service *TestService* in hello *Central US* location, run hello following PowerShell command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-tooa-new-cloud-service"></a><span data-ttu-id="bd416-162">Associera en reserverad IP-tooa ny molntjänst</span><span class="sxs-lookup"><span data-stu-id="bd416-162">Associate a reserved IP tooa new cloud service</span></span>
<span data-ttu-id="bd416-163">hello följande skript skapar en ny reserverad IP-adress och sedan kopplar den tooa ny molntjänst med namnet *TestService*.</span><span class="sxs-lookup"><span data-stu-id="bd416-163">hello following script creates a new reserved IP, then associates it tooa new cloud service named *TestService*.</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> <span data-ttu-id="bd416-164">När du skapar en reserverad IP-toouse med en molnbaserad tjänst kan du fortfarande hänvisar toohello VM med *VIP:&lt;portnummer >* för inkommande kommunikation.</span><span class="sxs-lookup"><span data-stu-id="bd416-164">When you create a reserved IP toouse with a cloud service, you still refer toohello VM by using *VIP:&lt;port number>* for inbound communication.</span></span> <span data-ttu-id="bd416-165">Reservera en IP-innebär inte kan du ansluta toohello VM direkt.</span><span class="sxs-lookup"><span data-stu-id="bd416-165">Reserving an IP does not mean you can connect toohello VM directly.</span></span> <span data-ttu-id="bd416-166">hello reserverad IP-adress tilldelas toohello cloud service som hello VM har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="bd416-166">hello reserved IP is assigned toohello cloud service that hello VM has been deployed to.</span></span> <span data-ttu-id="bd416-167">Om du vill tooconnect tooa VM efter IP direkt har tooconfigure en offentlig IP på instansnivå.</span><span class="sxs-lookup"><span data-stu-id="bd416-167">If you want tooconnect tooa VM by IP directly, you have tooconfigure an instance-level public IP.</span></span> <span data-ttu-id="bd416-168">En offentlig IP på instansnivå är en typ av offentlig IP-adress (kallas en går) som är tilldelad direkt tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="bd416-168">An instance-level public IP is a type of public IP (called an ILPIP) that is assigned directly tooyour VM.</span></span> <span data-ttu-id="bd416-169">Det går inte att reservera.</span><span class="sxs-lookup"><span data-stu-id="bd416-169">It cannot be reserved.</span></span> <span data-ttu-id="bd416-170">Mer information finns i hello [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd416-170">For more information, read hello [Instance-level Public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) article.</span></span>
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a><span data-ttu-id="bd416-171">Ta bort en reserverad IP-adress från en aktiv distribution</span><span class="sxs-lookup"><span data-stu-id="bd416-171">Remove a reserved IP from a running deployment</span></span>
<span data-ttu-id="bd416-172">tooremove en reserverad IP läggas till tooa ny molntjänst, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="bd416-172">tooremove a reserved IP added tooa new cloud service, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> <span data-ttu-id="bd416-173">Om du tar bort en reserverad IP-adress från en aktiv distribution tas inte bort hello reservation från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bd416-173">Removing a reserved IP from a running deployment does not remove hello reservation from your subscription.</span></span> <span data-ttu-id="bd416-174">Det gör helt enkelt hello IP toobe används av en annan resurs i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd416-174">It simply frees hello IP toobe used by another resource in your subscription.</span></span>
> 

## <a name="associate-a-reserved-ip-tooa-running-deployment"></a><span data-ttu-id="bd416-175">Associera en reserverad IP-tooa kör distribution</span><span class="sxs-lookup"><span data-stu-id="bd416-175">Associate a reserved IP tooa running deployment</span></span>
<span data-ttu-id="bd416-176">hello följande kommandon skapar en molntjänst med namnet *TestService2* med en ny virtuell dator med namnet *TestVM2*.</span><span class="sxs-lookup"><span data-stu-id="bd416-176">hello following commands create a cloud service named *TestService2* with a new VM named *TestVM2*.</span></span> <span data-ttu-id="bd416-177">hello befintlig reserverad IP med namnet *MyReservedIP* är associerade toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="bd416-177">hello existing reserved IP named *MyReservedIP* is then associated toohello cloud service.</span></span>

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-tooa-cloud-service-by-using-a-service-configuration-file"></a><span data-ttu-id="bd416-178">Koppla en reserverad IP-tooa molntjänst genom att använda en konfigurationsfil för tjänsten</span><span class="sxs-lookup"><span data-stu-id="bd416-178">Associate a reserved IP tooa cloud service by using a service configuration file</span></span>
<span data-ttu-id="bd416-179">Du kan även associera en reserverad IP-tooa molntjänst med hjälp av en tjänst-konfigurationsfil (CSCFG).</span><span class="sxs-lookup"><span data-stu-id="bd416-179">You can also associate a reserved IP tooa cloud service by using a service configuration (CSCFG) file.</span></span> <span data-ttu-id="bd416-180">hello xml med följande exempel visar hur tooconfigure en cloud service toouse ett reserverat VIP med namnet *MyReservedIP*:</span><span class="sxs-lookup"><span data-stu-id="bd416-180">hello following sample xml shows how tooconfigure a cloud service toouse a reserved VIP named *MyReservedIP*:</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a><span data-ttu-id="bd416-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd416-181">Next steps</span></span>
* <span data-ttu-id="bd416-182">Förstå hur [IP-adressering](virtual-network-ip-addresses-overview-classic.md) fungerar i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="bd416-182">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in hello classic deployment model.</span></span>
* <span data-ttu-id="bd416-183">Lär dig mer om [reserverade privata IP-adresser](virtual-networks-reserved-private-ip.md).</span><span class="sxs-lookup"><span data-stu-id="bd416-183">Learn about [reserved private IP addresses](virtual-networks-reserved-private-ip.md).</span></span>
* <span data-ttu-id="bd416-184">Lär dig mer om [instans nivå offentliga IP-går adresser](virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="bd416-184">Learn about [Instance Level Public IP (ILPIP) addresses](virtual-networks-instance-level-public-ip.md).</span></span>

