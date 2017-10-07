---
title: "aaaAzure PowerShell skriptexempel - belastningsutjämning för flera webbplatser med Azure PowerShell | Microsoft Docs"
description: "Azure PowerShell skriptexempel - belastningsutjämning för flera webbplatser toohello samma virtuella dator"
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
ms.openlocfilehash: 316818964eb6928fe4163ef69eb7f05da2dc9636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a>Belastningsutjämning för flera webbplatser

Det här exemplet i skriptet skapar ett virtuellt nätverk med två virtuella datorer (VM) som är medlemmar i en tillgänglighetsuppsättning. En belastningsutjämnare dirigerar trafik för två separata IP-adresser toohello två virtuella datorer. Efter köra hello skriptet kan du distribuera web server programvara toohello virtuella datorer och vara värd för flera webbplatser, var och en med sin egen IP-adress.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella nätverk, läsa in belastningsutjämning och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Skapar ett Azure tillgänglighet ange tooprovide hög tillgänglighet. |
| [Ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Skapar en konfiguration av undernät. Den här konfigurationen används med hello processen för att skapa virtuella nätverk. |
| [Ny AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Skapar ett virtuellt nätverk. |
| [Ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Skapar en offentlig IP-adress. |
| [Ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Skapar en klientdel IP-konfigurationen för en belastningsutjämnare. |
| [Ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Skapar en backend-pool-adresskonfiguration för en belastningsutjämnare. |
| [Ny AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Skapar ett NLB-avsökning. En NLB-avsökning har använt toomonitor varje virtuell dator i hello NLB uppsättningen. Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM. |
| [Ny AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Skapar en regel för Utjämning av nätverksbelastning. I det här exemplet skapas en regel för port 80. HTTP-trafik anländer till hello Utjämning av nätverksbelastning, är det routade tooport 80 en hello virtuella datorer i hello NLB uppsättningen. |
| [Ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Skapar en belastningsutjämnare. |
| [Ny AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Definierar avancerade funktioner för ett virtuellt nätverksgränssnitt. |
| [Ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Skapar ett nätverksgränssnitt. |
| [Ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Skapar en VM-konfiguration. Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter. hello konfiguration används under skapande av virtuell dator. |
| [Ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Skapa en virtuell dator. |
|[Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tar bort en resursgrupp och alla resurser som ingår i. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).

Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
