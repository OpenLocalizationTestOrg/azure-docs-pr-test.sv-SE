---
title: "aaaAzure skriptexempel PowerShell - belastningen belastningsutjämna trafik tooVMs för hög tillgänglighet | Microsoft Docs"
description: "Azure PowerShell skriptexempel - belastningen belastningsutjämna trafik tooVMs för hög tillgänglighet"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 332c39e1aad071ca6f7ce420ccc1f423ef647d2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-toovms-for-high-availability"></a>Läsa in belastningsutjämna trafik tooVMs för hög tillgänglighet

Det här exemplet i skriptet skapar allt som behövs för toorun flera Windows-datorer som konfigurerats i en hög tillgänglighet och läsa in den belastningsutjämnade konfiguration. När du köra hello-skriptet kommer du ha tre virtuella datorer kopplade tooan Azure Tillgänglighetsuppsättning och kan nås via en Azure belastningsutjämnare.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Skapar en konfiguration av undernät. Den här konfigurationen används med hello processen för att skapa virtuella nätverk. |
| [Ny AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Skapar ett virtuellt Azure-nätverk och undernät. |
| [Ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn. |
| [Ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Skapar en Azure belastningsutjämnare. |
| [Ny AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Skapar en belastningsutjämningsavsökning. En belastningsutjämningsavsökning är används toomonitor varje virtuell dator i hello belastningen belastningsutjämnaren uppsättningen. Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM. |
| [Ny AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Skapar en regel för belastningsutjämning. I det här exemplet skapas en regel för port 80. HTTP-trafik anländer till hello belastningsutjämnare, är det routade tooport 80 en hello virtuella datorer i hello belastningen belastningsutjämnaren uppsättningen. |
| [Ny AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Skapar en belastningsutjämningsregel för NAT (Network Address Translation).  NAT-regler mappa en port i hello belastningen belastningsutjämnaren tooa port på en virtuell dator. I det här exemplet skapas en NAT-regel för SSH-trafik tooeach VM i hello belastningen belastningsutjämnaren uppsättningen.  |
| [Ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan hello internet och hello virtuella datorer. |
| [Ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Skapar en NSG regeln tooallow inkommande trafik. I det här exemplet används port 22 för SSH-trafik. |
| [Ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Skapar ett virtuellt nätverkskort och bifogar den toohello virtuella nätverk och undernät NSG. |
| [Ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Skapar en tillgänglighetsuppsättning. Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida hello virtuella datorer mellan fysiska resurser så att om fel uppstår, hello hela uppsättningen inte sker. |
| [Ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Skapar en VM-konfiguration. Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter. hello konfiguration används under skapande av virtuell dator. |
| [Ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).

Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
