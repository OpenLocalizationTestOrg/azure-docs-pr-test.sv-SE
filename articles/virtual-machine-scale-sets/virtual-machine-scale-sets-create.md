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
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Skapa och distribuera en skaluppsättning för virtuell dator
Skaluppsättningar för den virtuella datorn gör det lättare för dig toodeploy och hantera identiska virtuella datorer som en uppsättning. Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg. Läs mer om skaluppsättningar [skalningsuppsättningar i virtuella](virtual-machine-scale-sets-overview.md).

Den här kursen visar hur toocreate en virtuell dator skaluppsättning **utan** med hello Azure-portalen. Information om hur toouse hello Azure-portalen finns [hur toocreate en virtuell dator skaluppsättning med hello Azure-portalen](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Mer information om Azure Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-tooazure"></a>Logga in tooAzure

Om du använder Azure CLI 2.0 eller Azure PowerShell toocreate en skala har angetts måste du först toosign tooyour prenumeration.

Mer information om hur tooinstall, konfigurera, och logga in tooAzure med Azure CLI eller PowerShell, se [komma igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) eller [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Du måste först toocreate en resursgrupp som hello skaluppsättning för virtuell dator som är associerad med.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Skapa från Azure CLI

Med Azure CLI, kan du skapa en virtuell dator-skala med minsta möjliga ansträngning. Om du utelämnar standardvärden tillhandahålls de åt dig. Till exempel om du inte anger någon information om virtuellt nätverk skapas ett virtuellt nätverk för dig. Om du utelämnar hello följande delar skapas de automatiskt: 
- Belastningsutjämning
- Ett virtuellt nätverk
- En offentlig IP-adress

När du väljer hello avbildning av virtuell dator som du vill toouse på hello skaluppsättning för virtuell dator, har du några alternativ:

- URN  
hello identifierare för en resurs:  
**Win2012R2Datacenter**

- URN alias  
hello eget namn för en URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Anpassad resurs-id  
hello sökvägen tooan Azure-resurs:  
**/subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**

- Webbresurs  
hello sökvägen tooan HTTP URI:  
**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**

>[!TIP]
>Du kan hämta en lista över tillgängliga avbildningar med `az vm image list`.

toocreate en virtuell dator skaluppsättning måste du ange hello följande:

- Resursgrupp 
- Namn
- Avbildning av operativsystemet
- Autentiseringsinformation 
 
hello följande exempel skapar en grundläggande virtuella skaluppsättning (det här steget kan ta några minuter).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

När hello-kommandot har slutförts har du nu ange skapade virtuella nivå. Du kanske måste tooget hello IP-adressen för hello virtuell dator så att du kan ansluta tooit. Du kan hämta en mängd olika typer av information om hello virtual machine (inklusive hello IP-adress) med följande kommando hello. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Skapa från PowerShell

PowerShell är mer komplicerad toouse än Azure CLI. Medan Azure CLI tillhandahåller standardvärden för nätverksrelaterade resurser (till exempel belastningsutjämnare, IP-adresser och virtuella nätverk), PowerShell inte. Refererar till en bild med PowerShell är en något mer komplicerad för. Du kan hämta bilder med hello följande cmdlets:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

hello cmdlets arbete kan skickas i rätt ordning. Här är ett exempel på hur tooget alla bilder för hello **västra USA 2** region med en utgivare med namnet hello **microsoft** i den.

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

hello arbetsflöde för att skapa en skaluppsättning för virtuell dator är följande:

1. Skapa ett konfigurationsobjekt som innehåller information om hello skaluppsättning.
2. Referens hello grundläggande OS-avbildningen.
3. Konfigurera inställningar för operativsystem hello: autentisering, VM-namnprefix och användaren/pass.
4. Konfigurera nätverk.
5. Skapa hello skaluppsättning.

Det här exemplet skapar en grundläggande två instans skala för en dator som har Windows Server 2016 installerad.

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

### <a name="using-a-custom-virtual-machine-image"></a>Med hjälp av en virtuell datoravbildning
Om du skapar en skala från en egen anpassad avbildning i stället för att referera till en avbildning av virtuell dator från galleriet hello hello _Set AzureRmVmssStorageProfile_ kommando skulle se ut så här:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Skapa från en mall

Du kan distribuera en virtuell dator skala in med hjälp av en Azure Resource Manager-mall. Du kan skapa egna mallar eller använda en från hello [mall databasen](https://azure.microsoft.com/resources/templates/?term=vmss). De här mallarna kan distribueras direkt tooyour Azure-prenumeration.

>[!NOTE]
>toocreate egna mallar, skapa en JSON-textfil. Allmän information om hur toocreate och anpassa en mall, se [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

En exempelmall finns [på GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Mer information om hur toocreate och använda som exempel, se [minsta lönsam skaluppsättning](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Skapa från Visual Studio

Du kan skapa ett Azure resursgruppsprojektet och lägga till en virtuell dator skala mallen tooit med Visual Studio. Du kan välja om du vill tooimport det från GitHub eller hello Azure Web Application Gallery. En distribution PowerShell-skript skapas också för dig. Mer information finns i [hur toocreate en virtuell dator skaluppsättning med Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-hello-azure-portal"></a>Skapa från hello Azure-portalen

hello Azure-portalen är ett bekvämt sätt tooquickly skapa en skaluppsättning. Mer information finns i [hur toocreate en virtuell dator skaluppsättning med hello Azure-portalen](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [datadiskar](virtual-machine-scale-sets-attached-disks.md).

Lär dig hur för[hanterar dina appar](virtual-machine-scale-sets-deploy-app.md).
