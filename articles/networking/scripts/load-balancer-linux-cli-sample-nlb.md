---
title: "aaaAzure skriptexempel CLI - belastningen belastningsutjämna trafik tooVMs för hög tillgänglighet | Microsoft Docs"
description: "Skriptexempel Azure CLI - belastningen belastningsutjämna trafik tooVMs för hög tillgänglighet"
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
ms.openlocfilehash: 0954b5c261512724dfb9c6e7be123c9d45624f4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-toovms-for-high-availability"></a>Läsa in belastningsutjämna trafik tooVMs för hög tillgänglighet

Det här exemplet i skriptet skapar allt som behövs för toorun flera Ubuntu virtuella datorer som konfigurerats i en hög tillgänglighet och läsa in den belastningsutjämnade konfiguration. När du köra hello-skriptet kommer du ha tre virtuella datorer kopplade tooan Azure Tillgänglighetsuppsättning och kan nås via en Azure belastningsutjämnare. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ network vnet](https://docs.microsoft.com/cli/azure/network/vnet#create) | Skapar ett virtuellt Azure-nätverk och undernät. |
| [Skapa AZ nätverket offentliga-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn. |
| [Skapa AZ nätverket lb](https://docs.microsoft.com/cli/azure/network/lb#create) | Skapar en Azure belastningsutjämnare. |
| [Skapa AZ nätverket lb avsökning](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Skapar en belastningsutjämningsavsökning. En belastningsutjämningsavsökning är används toomonitor varje virtuell dator i hello belastningen belastningsutjämnaren uppsättningen. Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM. |
| [Skapa AZ nätverket lb regel](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en regel för belastningsutjämnare. I det här exemplet skapas en regel för port 80. HTTP-trafik anländer till hello belastningsutjämnare, är det routade tooport 80 en hello virtuella datorer i hello LB-uppsättningen. |
| [Skapa AZ nätverket lb inkommande nat-regel](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | Skapar en regel för belastningsutjämnare NAT (Network Address Translation).  NAT-regler mappa en port i hello belastningen belastningsutjämnaren tooa port på en virtuell dator. I det här exemplet skapas en NAT-regel för SSH-trafik tooeach VM i hello belastningen belastningsutjämnaren uppsättningen.  |
| [Skapa AZ nätverket nsg](https://docs.microsoft.com/cli/azure/network/nsg#create) | Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan hello internet och hello virtuella datorer. |
| [Skapa AZ nätverket nsg regel](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Skapar en NSG regeln tooallow inkommande trafik. I det här exemplet används port 22 för SSH-trafik. |
| [Skapa AZ nätverket nic](https://docs.microsoft.com/cli/azure/network/nic#create) | Skapar ett virtuellt nätverkskort och bifogar den toohello virtuella nätverk och undernät NSG. |
| [Skapa AZ vm tillgänglighetsuppsättning](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en tillgänglighetsuppsättning. Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida hello virtuella datorer mellan fysiska resurser så att om fel uppstår, hello hela uppsättningen inte sker. |
| [Skapa AZ vm](/cli/azure/vm#create) | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure-nätverk CLI skriptexempel finns i hello [Azure nätverk dokumentationen](../cli-samples.md).
