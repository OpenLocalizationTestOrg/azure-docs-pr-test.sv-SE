---
title: "Skriptexempel Azure CLI - belastningsutjämna trafiken till virtuella datorer för hög tillgänglighet | Microsoft Docs"
description: "Skriptexempel Azure CLI - belastningsutjämna trafiken till virtuella datorer för hög tillgänglighet"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 69a7753cc75b028e2bf093053d9a5fc0890562e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Läs in Utjämna trafiken till virtuella datorer för hög tillgänglighet

Det här exemplet i skriptet skapar allt som behövs för att köra flera Ubuntu virtuella datorer som konfigurerats i en hög tillgänglighet och läsa in belastningsutjämnade konfigurationen. När du har kört skriptet har tre virtuella datorer, ansluten till en Azure Tillgänglighetsuppsättning, och kan öppnas via en Azure belastningsutjämnare. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "snabbt skapa virtuell dator")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ network vnet](https://docs.microsoft.com/cli/azure/network/vnet#create) | Skapar ett virtuellt Azure-nätverk och undernät. |
| [Skapa AZ nätverket offentliga-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn. |
| [Skapa AZ nätverket lb](https://docs.microsoft.com/cli/azure/network/lb#create) | Skapar en Azure belastningsutjämnare. |
| [Skapa AZ nätverket lb avsökning](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Skapar en belastningsutjämningsavsökning. En belastningsutjämningsavsökning används för att övervaka varje virtuell dator i uppsättningen av belastningen belastningsutjämnaren. Om någon virtuell dator blir otillgänglig, dirigeras trafik inte till den virtuella datorn. |
| [Skapa AZ nätverket lb regel](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en regel för belastningsutjämnare. I det här exemplet skapas en regel för port 80. När HTTP-trafik anländer på belastningsutjämnaren, dirigeras till port 80 något av de virtuella datorerna i LB-uppsättningen. |
| [Skapa AZ nätverket lb inkommande nat-regel](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | Skapar en regel för belastningsutjämnare NAT (Network Address Translation).  NAT-regler kan du mappa en port för belastningsutjämnaren till en port på en virtuell dator. I det här exemplet skapas en NAT-regel för SSH-trafik på varje virtuell dator i uppsättningen av belastningen belastningsutjämnaren.  |
| [Skapa AZ nätverket nsg](https://docs.microsoft.com/cli/azure/network/nsg#create) | Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan internet och den virtuella datorn. |
| [Skapa AZ nätverket nsg regel](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Skapar en NSG-regel för att tillåta inkommande trafik. I det här exemplet används port 22 för SSH-trafik. |
| [Skapa AZ nätverket nic](https://docs.microsoft.com/cli/azure/network/nic#create) | Skapar ett virtuellt nätverkskort som kopplas till den virtuella nätverk och undernät NSG. |
| [Skapa AZ vm tillgänglighetsuppsättning](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en tillgänglighetsuppsättning. Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida virtuella datorer mellan fysiska resurser så att hela uppsättningen inte sker om fel inträffar. |
| [Skapa AZ vm](/cli/azure/vm#create) | Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också avbildningen av virtuella datorn ska användas och administrativa autentiseringsuppgifter.  |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure-nätverk CLI skriptexempel finns i den [Azure nätverk dokumentationen](../cli-samples.md).