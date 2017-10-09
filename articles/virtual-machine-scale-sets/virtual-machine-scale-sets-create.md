---
title: "aaaCreate en virtuell dator i Azure skaluppsättning | Microsoft Docs"
description: Skapa och distribuera en Linux- eller Windows Azure virtuella skala med Azure CLI, PowerShell, en mall eller Visual Studio.
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 73de25c1dd2424e64655b3accfea848926e72f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a><span data-ttu-id="d2edb-103">Skapa och distribuera en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2edb-103">Create and deploy a virtual machine scale set</span></span>
<span data-ttu-id="d2edb-104">Skaluppsättningar för den virtuella datorn gör det lättare för dig toodeploy och hantera identiska virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="d2edb-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="d2edb-105">Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg.</span><span class="sxs-lookup"><span data-stu-id="d2edb-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="d2edb-106">Läs mer om skaluppsättningar [skalningsuppsättningar i virtuella](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-106">For more information about scale sets, see [Virtual machine scale sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="d2edb-107">Den här kursen visar hur toocreate en virtuell dator skaluppsättning **utan** med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d2edb-107">This tutorial shows you how toocreate a virtual machine scale set **without** using hello Azure portal.</span></span> <span data-ttu-id="d2edb-108">Information om hur toouse hello Azure-portalen finns [hur toocreate en virtuell dator skaluppsättning med hello Azure-portalen](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-108">For information about how toouse hello Azure portal, see [How toocreate a virtual machine scale set with hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

>[!NOTE]
><span data-ttu-id="d2edb-109">Mer information om Azure Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-109">For more information about Azure Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="sign-in-tooazure"></a><span data-ttu-id="d2edb-110">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="d2edb-110">Sign in tooAzure</span></span>

<span data-ttu-id="d2edb-111">Om du använder Azure CLI 2.0 eller Azure PowerShell toocreate en skala har angetts måste du först toosign tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d2edb-111">If you're using Azure CLI 2.0 or Azure PowerShell toocreate a scale set, you first need toosign in tooyour subscription.</span></span>

<span data-ttu-id="d2edb-112">Mer information om hur tooinstall, konfigurera, och logga in tooAzure med Azure CLI eller PowerShell, se [komma igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) eller [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d2edb-112">For more information about how tooinstall, set up, and sign in tooAzure with Azure CLI or PowerShell, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) or [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="d2edb-113">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d2edb-113">Create a resource group</span></span>

<span data-ttu-id="d2edb-114">Du måste först toocreate en resursgrupp som hello skaluppsättning för virtuell dator som är associerad med.</span><span class="sxs-lookup"><span data-stu-id="d2edb-114">You first need toocreate a resource group that hello virtual machine scale set is associated with.</span></span>

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a><span data-ttu-id="d2edb-115">Skapa från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d2edb-115">Create from Azure CLI</span></span>

<span data-ttu-id="d2edb-116">Med Azure CLI, kan du skapa en virtuell dator-skala med minsta möjliga ansträngning.</span><span class="sxs-lookup"><span data-stu-id="d2edb-116">With Azure CLI, you can create a virtual machine scale set with minimal effort.</span></span> <span data-ttu-id="d2edb-117">Om du utelämnar standardvärden tillhandahålls de åt dig.</span><span class="sxs-lookup"><span data-stu-id="d2edb-117">If you omit default values, they are provided for you.</span></span> <span data-ttu-id="d2edb-118">Till exempel om du inte anger någon information om virtuellt nätverk skapas ett virtuellt nätverk för dig.</span><span class="sxs-lookup"><span data-stu-id="d2edb-118">For example, if you don't specify any virtual network information, a virtual network is created for you.</span></span> <span data-ttu-id="d2edb-119">Om du utelämnar hello följande delar skapas de automatiskt:</span><span class="sxs-lookup"><span data-stu-id="d2edb-119">If you omit hello following parts, they are created for you:</span></span> 
- <span data-ttu-id="d2edb-120">Belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="d2edb-120">A load balancer</span></span>
- <span data-ttu-id="d2edb-121">Ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="d2edb-121">A virtual network</span></span>
- <span data-ttu-id="d2edb-122">En offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="d2edb-122">A public IP address</span></span>

<span data-ttu-id="d2edb-123">När du väljer hello avbildning av virtuell dator som du vill toouse på hello skaluppsättning för virtuell dator, har du några alternativ:</span><span class="sxs-lookup"><span data-stu-id="d2edb-123">When choosing hello virtual machine image that you want toouse on hello virtual machine scale set, you have a few choices:</span></span>

- <span data-ttu-id="d2edb-124">URN</span><span class="sxs-lookup"><span data-stu-id="d2edb-124">URN</span></span>  
<span data-ttu-id="d2edb-125">hello identifierare för en resurs:</span><span class="sxs-lookup"><span data-stu-id="d2edb-125">hello identifier of a resource:</span></span>  
<span data-ttu-id="d2edb-126">**Win2012R2Datacenter**</span><span class="sxs-lookup"><span data-stu-id="d2edb-126">**Win2012R2Datacenter**</span></span>

- <span data-ttu-id="d2edb-127">URN alias</span><span class="sxs-lookup"><span data-stu-id="d2edb-127">URN alias</span></span>  
<span data-ttu-id="d2edb-128">hello eget namn för en URN:</span><span class="sxs-lookup"><span data-stu-id="d2edb-128">hello friendly name of a URN:</span></span>  
<span data-ttu-id="d2edb-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span><span class="sxs-lookup"><span data-stu-id="d2edb-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span></span>

- <span data-ttu-id="d2edb-130">Anpassad resurs-id</span><span class="sxs-lookup"><span data-stu-id="d2edb-130">Custom resource id</span></span>  
<span data-ttu-id="d2edb-131">hello sökvägen tooan Azure-resurs:</span><span class="sxs-lookup"><span data-stu-id="d2edb-131">hello path tooan Azure resource:</span></span>  
<span data-ttu-id="d2edb-132">**/subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**</span><span class="sxs-lookup"><span data-stu-id="d2edb-132">**/subscriptions/subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**</span></span>

- <span data-ttu-id="d2edb-133">Webbresurs</span><span class="sxs-lookup"><span data-stu-id="d2edb-133">Web resource</span></span>  
<span data-ttu-id="d2edb-134">hello sökvägen tooan HTTP URI:</span><span class="sxs-lookup"><span data-stu-id="d2edb-134">hello path tooan HTTP URI:</span></span>  
<span data-ttu-id="d2edb-135">**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**</span><span class="sxs-lookup"><span data-stu-id="d2edb-135">**http://contoso.blob.core.windows.net/vhds/osdiskimage.vhd**</span></span>

>[!TIP]
><span data-ttu-id="d2edb-136">Du kan hämta en lista över tillgängliga avbildningar med `az vm image list`.</span><span class="sxs-lookup"><span data-stu-id="d2edb-136">You can get a list of available images with `az vm image list`.</span></span>

<span data-ttu-id="d2edb-137">toocreate en virtuell dator skaluppsättning måste du ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="d2edb-137">toocreate a virtual machine scale set, you must specify hello following:</span></span>

- <span data-ttu-id="d2edb-138">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d2edb-138">Resource group</span></span> 
- <span data-ttu-id="d2edb-139">Namn</span><span class="sxs-lookup"><span data-stu-id="d2edb-139">Name</span></span>
- <span data-ttu-id="d2edb-140">Avbildning av operativsystemet</span><span class="sxs-lookup"><span data-stu-id="d2edb-140">Operating system image</span></span>
- <span data-ttu-id="d2edb-141">Autentiseringsinformation</span><span class="sxs-lookup"><span data-stu-id="d2edb-141">Authentication information</span></span> 
 
<span data-ttu-id="d2edb-142">hello följande exempel skapar en grundläggande virtuella skaluppsättning (det här steget kan ta några minuter).</span><span class="sxs-lookup"><span data-stu-id="d2edb-142">hello following example creates a basic virtual machine scale set (this step might take a few minutes).</span></span>

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

<span data-ttu-id="d2edb-143">När hello-kommandot har slutförts har du nu ange skapade virtuella nivå.</span><span class="sxs-lookup"><span data-stu-id="d2edb-143">Once hello command finishes you will now have your virtual machine scale set created.</span></span> <span data-ttu-id="d2edb-144">Du kanske måste tooget hello IP-adressen för hello virtuell dator så att du kan ansluta tooit.</span><span class="sxs-lookup"><span data-stu-id="d2edb-144">You may need tooget hello IP address of hello virtual machine so that you can connect tooit.</span></span> <span data-ttu-id="d2edb-145">Du kan hämta en mängd olika typer av information om hello virtual machine (inklusive hello IP-adress) med följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="d2edb-145">You can get a lot of different information about hello virtual machine (including hello IP address) with hello following command.</span></span> 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a><span data-ttu-id="d2edb-146">Skapa från PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2edb-146">Create from PowerShell</span></span>

<span data-ttu-id="d2edb-147">PowerShell är mer komplicerad toouse än Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d2edb-147">PowerShell is more complicated toouse than Azure CLI.</span></span> <span data-ttu-id="d2edb-148">Medan Azure CLI tillhandahåller standardvärden för nätverksrelaterade resurser (till exempel belastningsutjämnare, IP-adresser och virtuella nätverk), PowerShell inte.</span><span class="sxs-lookup"><span data-stu-id="d2edb-148">While Azure CLI provides defaults for networking-related resources (such as load balancers, IP addresses, and virtual networks), PowerShell does not.</span></span> <span data-ttu-id="d2edb-149">Refererar till en bild med PowerShell är en något mer komplicerad för.</span><span class="sxs-lookup"><span data-stu-id="d2edb-149">Referencing an image with PowerShell is a slightly more complicated too.</span></span> <span data-ttu-id="d2edb-150">Du kan hämta bilder med hello följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="d2edb-150">You can get images with hello following cmdlets:</span></span>

1. <span data-ttu-id="d2edb-151">Get-AzureRMVMImagePublisher</span><span class="sxs-lookup"><span data-stu-id="d2edb-151">Get-AzureRMVMImagePublisher</span></span>
2. <span data-ttu-id="d2edb-152">Get-AzureRMVMImageOffer</span><span class="sxs-lookup"><span data-stu-id="d2edb-152">Get-AzureRMVMImageOffer</span></span>
3. <span data-ttu-id="d2edb-153">Get-AzureRmVMImageSku</span><span class="sxs-lookup"><span data-stu-id="d2edb-153">Get-AzureRmVMImageSku</span></span>

<span data-ttu-id="d2edb-154">hello cmdlets arbete kan skickas i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="d2edb-154">hello cmdlets work can be piped in sequence.</span></span> <span data-ttu-id="d2edb-155">Här är ett exempel på hur tooget alla bilder för hello **västra USA 2** region med en utgivare med namnet hello **microsoft** i den.</span><span class="sxs-lookup"><span data-stu-id="d2edb-155">Here is an example of how tooget all images for hello **West US 2** region with a publisher that has hello name **microsoft** in it.</span></span>

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

<span data-ttu-id="d2edb-156">hello arbetsflöde för att skapa en skaluppsättning för virtuell dator är följande:</span><span class="sxs-lookup"><span data-stu-id="d2edb-156">hello workflow for creating a virtual machine scale set is as follows:</span></span>

1. <span data-ttu-id="d2edb-157">Skapa ett konfigurationsobjekt som innehåller information om hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="d2edb-157">Create a config object that holds information about hello scale set.</span></span>
2. <span data-ttu-id="d2edb-158">Referens hello grundläggande OS-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="d2edb-158">Reference hello base OS image.</span></span>
3. <span data-ttu-id="d2edb-159">Konfigurera inställningar för operativsystem hello: autentisering, VM-namnprefix och användaren/pass.</span><span class="sxs-lookup"><span data-stu-id="d2edb-159">Configure hello operating system settings: authentication, VM name prefix, and user/pass.</span></span>
4. <span data-ttu-id="d2edb-160">Konfigurera nätverk.</span><span class="sxs-lookup"><span data-stu-id="d2edb-160">Configure networking.</span></span>
5. <span data-ttu-id="d2edb-161">Skapa hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="d2edb-161">Create hello scale set.</span></span>

<span data-ttu-id="d2edb-162">Det här exemplet skapar en grundläggande två instans skala för en dator som har Windows Server 2016 installerad.</span><span class="sxs-lookup"><span data-stu-id="d2edb-162">This example creates a basic two-instance scale set for a computer that has Windows Server 2016 installed.</span></span>

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create hello virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach hello virtual network toohello IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a><span data-ttu-id="d2edb-163">Med hjälp av en virtuell datoravbildning</span><span class="sxs-lookup"><span data-stu-id="d2edb-163">Using a custom virtual machine image</span></span>
<span data-ttu-id="d2edb-164">Om du skapar en skala från en egen anpassad avbildning i stället för att referera till en avbildning av virtuell dator från galleriet hello hello _Set AzureRmVmssStorageProfile_ kommando skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="d2edb-164">If you are creating a scale set from your own custom image, instead of referencing a virtual machine image from hello gallery, hello _Set-AzureRmVmssStorageProfile_ command would look like this:</span></span>
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a><span data-ttu-id="d2edb-165">Skapa från en mall</span><span class="sxs-lookup"><span data-stu-id="d2edb-165">Create from a template</span></span>

<span data-ttu-id="d2edb-166">Du kan distribuera en virtuell dator skala in med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="d2edb-166">You can deploy a virtual machine scale set by using an Azure Resource Manager template.</span></span> <span data-ttu-id="d2edb-167">Du kan skapa egna mallar eller använda en från hello [mall databasen](https://azure.microsoft.com/resources/templates/?term=vmss).</span><span class="sxs-lookup"><span data-stu-id="d2edb-167">You can create your own template or use one from hello [template repository](https://azure.microsoft.com/resources/templates/?term=vmss).</span></span> <span data-ttu-id="d2edb-168">De här mallarna kan distribueras direkt tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d2edb-168">These templates can be deployed directly tooyour Azure subscription.</span></span>

>[!NOTE]
><span data-ttu-id="d2edb-169">toocreate egna mallar, skapa en JSON-textfil.</span><span class="sxs-lookup"><span data-stu-id="d2edb-169">toocreate your own template, you create a JSON text file.</span></span> <span data-ttu-id="d2edb-170">Allmän information om hur toocreate och anpassa en mall, se [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-170">For general information about how toocreate and customize a template, see [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="d2edb-171">En exempelmall finns [på GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span><span class="sxs-lookup"><span data-stu-id="d2edb-171">A sample template is available [on GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span></span> <span data-ttu-id="d2edb-172">Mer information om hur toocreate och använda som exempel, se [minsta lönsam skaluppsättning](.\virtual-machine-scale-sets-mvss-start.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-172">For more information about how toocreate and use that sample, see [Minimum viable scale set](.\virtual-machine-scale-sets-mvss-start.md).</span></span>

## <a name="create-from-visual-studio"></a><span data-ttu-id="d2edb-173">Skapa från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2edb-173">Create from Visual Studio</span></span>

<span data-ttu-id="d2edb-174">Du kan skapa ett Azure resursgruppsprojektet och lägga till en virtuell dator skala mallen tooit med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d2edb-174">With Visual Studio, you can create an Azure resource group project and add a virtual machine scale set template tooit.</span></span> <span data-ttu-id="d2edb-175">Du kan välja om du vill tooimport det från GitHub eller hello Azure Web Application Gallery.</span><span class="sxs-lookup"><span data-stu-id="d2edb-175">You can choose whether you want tooimport it from GitHub or hello Azure Web Application Gallery.</span></span> <span data-ttu-id="d2edb-176">En distribution PowerShell-skript skapas också för dig.</span><span class="sxs-lookup"><span data-stu-id="d2edb-176">A deployment PowerShell script is also generated for you.</span></span> <span data-ttu-id="d2edb-177">Mer information finns i [hur toocreate en virtuell dator skaluppsättning med Visual Studio](virtual-machine-scale-sets-vs-create.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-177">For more information, see [How toocreate a virtual machine scale set with Visual Studio](virtual-machine-scale-sets-vs-create.md).</span></span>

## <a name="create-from-hello-azure-portal"></a><span data-ttu-id="d2edb-178">Skapa från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d2edb-178">Create from hello Azure portal</span></span>

<span data-ttu-id="d2edb-179">hello Azure-portalen är ett bekvämt sätt tooquickly skapa en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="d2edb-179">hello Azure portal provides a convenient way tooquickly create a scale set.</span></span> <span data-ttu-id="d2edb-180">Mer information finns i [hur toocreate en virtuell dator skaluppsättning med hello Azure-portalen](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-180">For more information, see [How toocreate a virtual machine scale set with hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2edb-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2edb-181">Next steps</span></span>

<span data-ttu-id="d2edb-182">Lär dig mer om [datadiskar](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-182">Learn more about [data disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="d2edb-183">Lär dig hur för[hanterar dina appar](virtual-machine-scale-sets-deploy-app.md).</span><span class="sxs-lookup"><span data-stu-id="d2edb-183">Learn how too[manage your apps](virtual-machine-scale-sets-deploy-app.md).</span></span>
