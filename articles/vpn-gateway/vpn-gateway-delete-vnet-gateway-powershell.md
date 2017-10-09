---
title: "Ta bort en virtuell nätverksgateway: PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Ta bort en virtuell nätverksgateway med PowerShell i Resource Manager-distributionsmodellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 4d0f085423d5bd60b24d88649ee1d77bcd1d009f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a><span data-ttu-id="28bc8-103">Ta bort en virtuell nätverksgateway med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="28bc8-103">Delete a virtual network gateway using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="28bc8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="28bc8-104">Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="28bc8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="28bc8-105">PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="28bc8-106">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="28bc8-106">PowerShell (classic)</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

<span data-ttu-id="28bc8-107">Det finns ett par olika metoder som du kan använda när du vill ta bort en virtuell nätverksgateway för en konfiguration för VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="28bc8-107">There are a couple of different approaches you can take when you want to delete a virtual network gateway for a VPN gateway configuration.</span></span>

- <span data-ttu-id="28bc8-108">Om du vill ta bort allt och börja om från början, som i fallet med en testmiljö kan du ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-108">If you want to delete everything and start over, as in the case of a test environment, you can delete the resource group.</span></span> <span data-ttu-id="28bc8-109">När du tar bort en resursgrupp tar bort alla resurser i gruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-109">When you delete a resource group, it deletes all the resources within the group.</span></span> <span data-ttu-id="28bc8-110">Det här är metod rekommenderas endast om du inte vill behålla några resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-110">This is method is only recommended if you don't want to keep any of the resources in the resource group.</span></span> <span data-ttu-id="28bc8-111">Du kan inte selektivt Radera endast några resurser med hjälp av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="28bc8-111">You can't selectively delete only a few resources using this approach.</span></span>

- <span data-ttu-id="28bc8-112">Om du vill behålla vissa av resurserna i resursgruppen blir tar bort en virtuell nätverksgateway något mer komplicerad.</span><span class="sxs-lookup"><span data-stu-id="28bc8-112">If you want to keep some of the resources in your resource group, deleting a virtual network gateway becomes slightly more complicated.</span></span> <span data-ttu-id="28bc8-113">Innan du kan ta bort den virtuella nätverksgatewayen, måste du först ta bort alla resurser som är beroende av denna gateway.</span><span class="sxs-lookup"><span data-stu-id="28bc8-113">Before you can delete the virtual network gateway, you must first delete any resources that are dependent on the gateway.</span></span> <span data-ttu-id="28bc8-114">De steg du följer beror på vilken typ av anslutningar som du skapade och beroende resurser för varje anslutning.</span><span class="sxs-lookup"><span data-stu-id="28bc8-114">The steps you follow depend on the type of connections that you created and the dependent resources for each connection.</span></span>

## <a name="before-beginning"></a><span data-ttu-id="28bc8-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="28bc8-115">Before beginning</span></span>

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a><span data-ttu-id="28bc8-116">1. Hämta det senaste Azure Resource Manager PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="28bc8-116">1. Download the latest Azure Resource Manager PowerShell cmdlets.</span></span>

<span data-ttu-id="28bc8-117">Hämta och installera den senaste versionen av Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="28bc8-117">Download and install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="28bc8-118">Mer information om hämtning och installation av PowerShell-cmdlets finns [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="28bc8-118">For more information about downloading and installing PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="2-connect-to-your-azure-account"></a><span data-ttu-id="28bc8-119">2. Ansluta till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="28bc8-119">2. Connect to your Azure account.</span></span>

<span data-ttu-id="28bc8-120">Öppna PowerShell-konsolen och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="28bc8-120">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="28bc8-121">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="28bc8-121">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="28bc8-122">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="28bc8-122">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="28bc8-123">Om du har mer än en prenumeration kan du ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="28bc8-123">If you have more than one subscription, specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="28bc8-124"><a name="S2S"></a>Ta bort en plats-till-plats VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="28bc8-124"><a name="S2S"></a>Delete a Site-to-Site VPN gateway</span></span>

<span data-ttu-id="28bc8-125">Du måste ta bort varje resurs som gäller för den virtuella nätverksgatewayen för att ta bort en virtuell nätverksgateway för en S2S-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="28bc8-125">To delete a virtual network gateway for a S2S configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="28bc8-126">Resurser måste tas bort i en viss ordning på grund av beroenden.</span><span class="sxs-lookup"><span data-stu-id="28bc8-126">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="28bc8-127">När du arbetar med exemplen nedan, vissa av måste värdena anges, medan andra värden är ett resultat i utdata.</span><span class="sxs-lookup"><span data-stu-id="28bc8-127">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="28bc8-128">Vi använder följande specifika värden i exemplen i exempelsyfte:</span><span class="sxs-lookup"><span data-stu-id="28bc8-128">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="28bc8-129">VNet-name: VNet1</span><span class="sxs-lookup"><span data-stu-id="28bc8-129">VNet name: VNet1</span></span><br>
<span data-ttu-id="28bc8-130">Resursgruppens namn: RG1</span><span class="sxs-lookup"><span data-stu-id="28bc8-130">Resource Group name: RG1</span></span><br>
<span data-ttu-id="28bc8-131">Gateway för virtuella nätverksnamnet: GW1</span><span class="sxs-lookup"><span data-stu-id="28bc8-131">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="28bc8-132">Följande steg gäller för Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-132">The following steps apply to the Resource Manager deployment model.</span></span>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="28bc8-133">1. Hämta den virtuella nätverksgatewayen som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-133">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a><span data-ttu-id="28bc8-134">2. Kontrollera om den virtuella nätverksgatewayen har alla anslutningar.</span><span class="sxs-lookup"><span data-stu-id="28bc8-134">2. Check to see if the virtual network gateway has any connections.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a><span data-ttu-id="28bc8-135">3. Ta bort alla anslutningar.</span><span class="sxs-lookup"><span data-stu-id="28bc8-135">3. Delete all connections.</span></span>

<span data-ttu-id="28bc8-136">Du kan uppmanas att bekräfta borttagningen av anslutning.</span><span class="sxs-lookup"><span data-stu-id="28bc8-136">You may be prompted to confirm the deletion of each of the connections.</span></span>

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a><span data-ttu-id="28bc8-137">4. Ta bort den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-137">4. Delete the virtual network gateway.</span></span>

<span data-ttu-id="28bc8-138">Du kan uppmanas att bekräfta borttagningen av gatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-138">You may be prompted to confirm the deletion of the gateway.</span></span> <span data-ttu-id="28bc8-139">Om du har en P2S-konfiguration till detta virtuella nätverk utöver konfigurationen S2S bort tar bort virtuell nätverksgateway automatiskt alla P2S-klienter utan varning skapas.</span><span class="sxs-lookup"><span data-stu-id="28bc8-139">If you have a P2S configuration to this VNet in addition to your S2S configuration, deleting the virtual network gateway will automatically disconnect all P2S clients without warning.</span></span>


```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="28bc8-140">Nu är har din virtuella nätverksgateway tagits bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-140">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="28bc8-141">Du kan använda i nästa steg att ta bort alla resurser som används inte längre.</span><span class="sxs-lookup"><span data-stu-id="28bc8-141">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="5-delete-the-local-network-gateways"></a><span data-ttu-id="28bc8-142">5 ta bort de lokala nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="28bc8-142">5 Delete the local network gateways.</span></span>

<span data-ttu-id="28bc8-143">Hämta listan över motsvarande lokala nätverksgatewayer.</span><span class="sxs-lookup"><span data-stu-id="28bc8-143">Get the list of the corresponding local network gateways.</span></span>

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

<span data-ttu-id="28bc8-144">Ta bort de lokala nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="28bc8-144">Delete the local network gateways.</span></span> <span data-ttu-id="28bc8-145">Du kan uppmanas att bekräfta borttagningen av var och en av den lokala nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-145">You may be prompted to confirm the deletion of each of the local network gateway.</span></span>

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a><span data-ttu-id="28bc8-146">6. Ta bort resurser för offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="28bc8-146">6. Delete the Public IP address resources.</span></span>

<span data-ttu-id="28bc8-147">Hämta IP-konfigurationer för den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-147">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="28bc8-148">Hämta listan över offentlig IP-adressresurser som används för den här virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-148">Get the list of Public IP address resources used for this virtual network gateway.</span></span> <span data-ttu-id="28bc8-149">Om den virtuella nätverksgatewayen var aktiv-aktiv, visas två offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-149">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="28bc8-150">Ta bort offentlig IP-resurser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-150">Delete the Public IP resources.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="28bc8-151">7. Ta bort gateway-undernät och ställer in konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-151">7. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="28bc8-152"><a name="v2v"></a>Ta bort en VNet-till-VNet VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="28bc8-152"><a name="v2v"></a>Delete a VNet-to-VNet VPN gateway</span></span>

<span data-ttu-id="28bc8-153">Du måste ta bort varje resurs som gäller för den virtuella nätverksgatewayen för att ta bort en virtuell nätverksgateway för en V2V-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="28bc8-153">To delete a virtual network gateway for a V2V configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="28bc8-154">Resurser måste tas bort i en viss ordning på grund av beroenden.</span><span class="sxs-lookup"><span data-stu-id="28bc8-154">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="28bc8-155">När du arbetar med exemplen nedan, vissa av måste värdena anges, medan andra värden är ett resultat i utdata.</span><span class="sxs-lookup"><span data-stu-id="28bc8-155">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="28bc8-156">Vi använder följande specifika värden i exemplen i exempelsyfte:</span><span class="sxs-lookup"><span data-stu-id="28bc8-156">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="28bc8-157">VNet-name: VNet1</span><span class="sxs-lookup"><span data-stu-id="28bc8-157">VNet name: VNet1</span></span><br>
<span data-ttu-id="28bc8-158">Resursgruppens namn: RG1</span><span class="sxs-lookup"><span data-stu-id="28bc8-158">Resource Group name: RG1</span></span><br>
<span data-ttu-id="28bc8-159">Gateway för virtuella nätverksnamnet: GW1</span><span class="sxs-lookup"><span data-stu-id="28bc8-159">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="28bc8-160">Följande steg gäller för Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-160">The following steps apply to the Resource Manager deployment model.</span></span>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="28bc8-161">1. Hämta den virtuella nätverksgatewayen som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-161">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a><span data-ttu-id="28bc8-162">2. Kontrollera om den virtuella nätverksgatewayen har alla anslutningar.</span><span class="sxs-lookup"><span data-stu-id="28bc8-162">2. Check to see if the virtual network gateway has any connections.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
<span data-ttu-id="28bc8-163">Det kan finnas andra anslutningar till den virtuella nätverksgatewayen som ingår i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="28bc8-163">There may be other connections to the virtual network gateway that are part of a different resource group.</span></span> <span data-ttu-id="28bc8-164">Sök efter ytterligare anslutningar i varje ytterligare resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="28bc8-164">Check for additional connections in each additional resource group.</span></span> <span data-ttu-id="28bc8-165">I det här exemplet kontrollerar för anslutningar från RG2.</span><span class="sxs-lookup"><span data-stu-id="28bc8-165">In this example, we are checking for connections from RG2.</span></span> <span data-ttu-id="28bc8-166">Kör detta för varje resursgrupp som du har som kan ha en anslutning till den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-166">Run this for each resource group that you have which may have a connection to the virtual network gateway.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a><span data-ttu-id="28bc8-167">3. Hämta listan över anslutningar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="28bc8-167">3. Get the list of connections in both directions.</span></span>

<span data-ttu-id="28bc8-168">Eftersom detta är en VNet-till-VNet-konfiguration måste listan över anslutningar i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="28bc8-168">Because this is a VNet-to-VNet configuration, you need the list of connections in both directions.</span></span>

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
<span data-ttu-id="28bc8-169">I det här exemplet kontrollerar för anslutningar från RG2.</span><span class="sxs-lookup"><span data-stu-id="28bc8-169">In this example, we are checking for connections from RG2.</span></span> <span data-ttu-id="28bc8-170">Kör detta för varje resursgrupp som du har som kan ha en anslutning till den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-170">Run this for each resource group that you have which may have a connection to the virtual network gateway.</span></span>

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a><span data-ttu-id="28bc8-171">4. Ta bort alla anslutningar.</span><span class="sxs-lookup"><span data-stu-id="28bc8-171">4. Delete all connections.</span></span>

<span data-ttu-id="28bc8-172">Du kan uppmanas att bekräfta borttagningen av anslutning.</span><span class="sxs-lookup"><span data-stu-id="28bc8-172">You may be prompted to confirm the deletion of each of the connections.</span></span>

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a><span data-ttu-id="28bc8-173">5. Ta bort den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-173">5. Delete the virtual network gateway.</span></span>

<span data-ttu-id="28bc8-174">Du kan uppmanas att bekräfta borttagningen av den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-174">You may be prompted to confirm the deletion of the virtual network gateway.</span></span> <span data-ttu-id="28bc8-175">Om du har P2S konfigurationer till ditt Vnet förutom V2V-konfiguration bort om du tar bort de virtuella nätverksgatewayerna automatiskt alla P2S-klienter utan varning skapas.</span><span class="sxs-lookup"><span data-stu-id="28bc8-175">If you have P2S configurations to your VNets in addition to your V2V configuration, deleting the virtual network gateways will automatically disconnect all P2S clients without warning.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="28bc8-176">Nu är har din virtuella nätverksgateway tagits bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-176">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="28bc8-177">Du kan använda i nästa steg att ta bort alla resurser som används inte längre.</span><span class="sxs-lookup"><span data-stu-id="28bc8-177">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="6-delete-the-public-ip-address-resources"></a><span data-ttu-id="28bc8-178">6. Ta bort offentlig IP-adressresurser</span><span class="sxs-lookup"><span data-stu-id="28bc8-178">6. Delete the Public IP address resources</span></span>

<span data-ttu-id="28bc8-179">Hämta IP-konfigurationer för den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-179">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="28bc8-180">Hämta listan över offentlig IP-adressresurser som används för den här virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-180">Get the list of Public IP address resources used for this virtual network gateway.</span></span> <span data-ttu-id="28bc8-181">Om den virtuella nätverksgatewayen var aktiv-aktiv, visas två offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-181">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="28bc8-182">Ta bort offentlig IP-resurser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-182">Delete the Public IP resources.</span></span> <span data-ttu-id="28bc8-183">Du kan uppmanas att bekräfta borttagningen av offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="28bc8-183">You may be prompted to confirm the deletion of the Public IP.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="28bc8-184">7. Ta bort gateway-undernät och ställer in konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-184">7. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="28bc8-185"><a name="deletep2s"></a>Ta bort en punkt-till-plats VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="28bc8-185"><a name="deletep2s"></a>Delete a Point-to-Site VPN gateway</span></span>

<span data-ttu-id="28bc8-186">Om du vill ta bort en virtuell nätverksgateway för en P2S-konfiguration, måste du först ta bort varje resurs som gäller för den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-186">To delete a virtual network gateway for a P2S configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="28bc8-187">Resurser måste tas bort i en viss ordning på grund av beroenden.</span><span class="sxs-lookup"><span data-stu-id="28bc8-187">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="28bc8-188">När du arbetar med exemplen nedan, vissa av måste värdena anges, medan andra värden är ett resultat i utdata.</span><span class="sxs-lookup"><span data-stu-id="28bc8-188">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="28bc8-189">Vi använder följande specifika värden i exemplen i exempelsyfte:</span><span class="sxs-lookup"><span data-stu-id="28bc8-189">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="28bc8-190">VNet-name: VNet1</span><span class="sxs-lookup"><span data-stu-id="28bc8-190">VNet name: VNet1</span></span><br>
<span data-ttu-id="28bc8-191">Resursgruppens namn: RG1</span><span class="sxs-lookup"><span data-stu-id="28bc8-191">Resource Group name: RG1</span></span><br>
<span data-ttu-id="28bc8-192">Gateway för virtuella nätverksnamnet: GW1</span><span class="sxs-lookup"><span data-stu-id="28bc8-192">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="28bc8-193">Följande steg gäller för Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-193">The following steps apply to the Resource Manager deployment model.</span></span>


>[!NOTE]
> <span data-ttu-id="28bc8-194">När du tar bort VPN-gateway kommer att kopplas från alla anslutna klienter från VNet utan varning.</span><span class="sxs-lookup"><span data-stu-id="28bc8-194">When you delete the VPN gateway, all connected clients will be disconnected from the VNet without warning.</span></span>
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="28bc8-195">1. Hämta den virtuella nätverksgatewayen som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-195">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a><span data-ttu-id="28bc8-196">2. Ta bort den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-196">2. Delete the virtual network gateway.</span></span>

<span data-ttu-id="28bc8-197">Du kan uppmanas att bekräfta borttagningen av den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-197">You may be prompted to confirm the deletion of the virtual network gateway.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="28bc8-198">Nu är har din virtuella nätverksgateway tagits bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-198">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="28bc8-199">Du kan använda i nästa steg att ta bort alla resurser som används inte längre.</span><span class="sxs-lookup"><span data-stu-id="28bc8-199">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="3-delete-the-public-ip-address-resources"></a><span data-ttu-id="28bc8-200">3. Ta bort offentlig IP-adressresurser</span><span class="sxs-lookup"><span data-stu-id="28bc8-200">3. Delete the Public IP address resources</span></span>

<span data-ttu-id="28bc8-201">Hämta IP-konfigurationer för den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-201">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="28bc8-202">Hämta listan över offentliga IP-adresser som används för den här virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-202">Get the list of Public IP addresses used for this virtual network gateway.</span></span> <span data-ttu-id="28bc8-203">Om den virtuella nätverksgatewayen var aktiv-aktiv, visas två offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-203">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="28bc8-204">Ta bort den offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-204">Delete the Public IPs.</span></span> <span data-ttu-id="28bc8-205">Du kan uppmanas att bekräfta borttagningen av offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="28bc8-205">You may be prompted to confirm the deletion of the Public IP.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="28bc8-206">4. Ta bort gateway-undernät och ställer in konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-206">4. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="28bc8-207"><a name="delete"></a>Ta bort en VPN-gateway genom att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-207"><a name="delete"></a>Delete a VPN gateway by deleting the resource group</span></span>

<span data-ttu-id="28bc8-208">Om du inte vill inte behålla någon av dina resurser i resursgruppen och du vill börja om från början, kan du ta bort en hela resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-208">If you are not concerned about keeping any of your resources in the resource group and you just want to start over, you can delete an entire resource group.</span></span> <span data-ttu-id="28bc8-209">Detta är ett snabbt sätt att ta bort allt.</span><span class="sxs-lookup"><span data-stu-id="28bc8-209">This is a quick way to remove everything.</span></span> <span data-ttu-id="28bc8-210">Följande steg gäller endast för Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-210">The following steps apply only to the Resource Manager deployment model.</span></span>

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a><span data-ttu-id="28bc8-211">1. Hämta en lista över alla resursgrupper i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="28bc8-211">1. Get a list of all the resource groups in your subscription.</span></span>

```powershell
Get-AzureRmResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a><span data-ttu-id="28bc8-212">2. Leta upp den resursgrupp som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="28bc8-212">2. Locate the resource group that you want to delete.</span></span>

<span data-ttu-id="28bc8-213">Leta upp den resursgrupp som du vill ta bort och visa listan över resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-213">Locate the resource group that you want to delete and view the list of resources in that resource group.</span></span> <span data-ttu-id="28bc8-214">I det här exemplet är namnet på resursgruppen RG1.</span><span class="sxs-lookup"><span data-stu-id="28bc8-214">In the example, the name of the resource group is RG1.</span></span> <span data-ttu-id="28bc8-215">Ändra exempel för att hämta en lista över alla resurser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-215">Modify the example to retrieve a list of all the resources.</span></span>

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a><span data-ttu-id="28bc8-216">3. Kontrollera resurserna i listan.</span><span class="sxs-lookup"><span data-stu-id="28bc8-216">3. Verify the resources in the list.</span></span>

<span data-ttu-id="28bc8-217">När listan returneras kan du granska den för att verifiera att du vill ta bort alla resurser i resursgruppen som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-217">When the list is returned, review it to verify that you want to delete all the resources in the resource group, as well as the resource group itself.</span></span> <span data-ttu-id="28bc8-218">Om du vill behålla vissa av resurserna i resursgruppen använder du stegen i de föregående avsnitten i den här artikeln för att ta bort din gateway.</span><span class="sxs-lookup"><span data-stu-id="28bc8-218">If you want to keep some of the resources in the resource group, use the steps in the earlier sections of this article to delete your gateway.</span></span>

### <a name="4-delete-the-resource-group-and-resources"></a><span data-ttu-id="28bc8-219">4. Ta bort resursgruppen och resurserna.</span><span class="sxs-lookup"><span data-stu-id="28bc8-219">4. Delete the resource group and resources.</span></span>

<span data-ttu-id="28bc8-220">Ändra exemplet och kör om du vill ta bort resursgruppen och alla resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bc8-220">To delete the resource group and all the resource contained in the resource group, modify the example and run.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a><span data-ttu-id="28bc8-221">5. Kontrollera status.</span><span class="sxs-lookup"><span data-stu-id="28bc8-221">5. Check the status.</span></span>

<span data-ttu-id="28bc8-222">Det tar en stund innan Azure för att ta bort alla resurser.</span><span class="sxs-lookup"><span data-stu-id="28bc8-222">It takes some time for Azure to delete all the resources.</span></span> <span data-ttu-id="28bc8-223">Du kan kontrollera statusen för din resursgrupp med denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="28bc8-223">You can check the status of your resource group by using this cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

<span data-ttu-id="28bc8-224">Det resultat som returneras visar ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="28bc8-224">The result that is returned shows 'Succeeded'.</span></span>

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```