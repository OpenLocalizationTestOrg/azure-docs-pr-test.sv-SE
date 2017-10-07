---
title: "aaaAzure PowerShell skriptexempel - vägtrafik via en virtuell nätverksenhet | Microsoft Docs"
description: "Azure PowerShell skriptexempel - vägtrafik via en virtuell nätverksenhet för brandväggen."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 3b999f3289d654c00d5becb973e2883896457d52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Vidarebefordra trafik via en virtuell nätverksenhet

Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät. Dessutom skapas en virtuell dator med IP-vidarebefordring aktiverade tooroute trafik mellan hello två undernät. Du kan distribuera programvara för nätverk, till exempel en brandvägg programmet toohello VM när du har kört hello skript.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper. Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Skapar en Azure-nätverket och frontend-undernät. |
| [Ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Skapar backend- och DMZ undernät. |
| [Ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Skapar en offentlig IP-adress tooaccess hello VM från hello Internet. |
| [Ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Skapar ett virtuellt nätverksgränssnitt och aktivera IP-vidarebefordring för den. |
| [Ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Skapar en nätverkssäkerhetsgrupp (NSG). |
| [Ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Skapar NSG-regler som tillåter HTTP och HTTPS-portar inkommande toohello VM. |
| [Ange AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Associerats hello NSG: er och väg tabeller toosubnets. |
| [Ny AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Skapar en routningstabell för alla flöden. |
| [Ny AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Skapar vägar tooroute trafik mellan undernät och hello Internet via hello VM. |
| [Ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Skapar en virtuell dator och bifogar hello NIC tooit. Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter. |
| [Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Tar bort en resursgrupp och alla resurser som den innehåller. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).

Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
