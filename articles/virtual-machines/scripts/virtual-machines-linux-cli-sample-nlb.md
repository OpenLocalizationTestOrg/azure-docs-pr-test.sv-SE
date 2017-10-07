---
title: aaaAzure CLI skriptexempel - skapa en Linux VM med NLB | Microsoft Docs
description: "Azure CLI Script exempel - skapa en virtuell Linux-dator med Utjämning av nätverksbelastning"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 79b245c1268734fead313b34c120f74ab2330d4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-highly-available-vm"></a>Skapa en högtillgänglig virtuell dator

Det här exemplet i skriptet skapar allt som behövs för toorun flera Ubuntu virtuella datorer som konfigurerats i en hög tillgänglighet och läsa in den belastningsutjämnade konfiguration. När du köra hello-skriptet kommer du ha tre virtuella datorer kopplade tooan Azure Tillgänglighetsuppsättning och kan nås via en Azure belastningsutjämnare. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ network vnet](https://docs.microsoft.com/cli/azure/network/vnet#create) | Skapar ett virtuellt Azure-nätverk och undernät. |
| [Skapa AZ nätverket offentliga-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn. |
| [Skapa AZ nätverket lb](https://docs.microsoft.com/cli/azure/network/lb#create) | Skapar ett Azure Utjämning av nätverksbelastning (NLB). |
| [Skapa AZ nätverket lb avsökning](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Skapar ett NLB-avsökning. En NLB-avsökning har använt toomonitor varje virtuell dator i hello NLB uppsättningen. Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM. |
| [Skapa AZ nätverket lb regel](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en regel för Utjämning av nätverksbelastning. I det här exemplet skapas en regel för port 80. HTTP-trafik anländer till hello Utjämning av nätverksbelastning, är det routade tooport 80 en hello virtuella datorer i hello NLB uppsättningen. |
| [Skapa AZ nätverket lb inkommande nat-regel](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | Skapar en regel för NLB (Network Address Translation Translation).  NAT-regler mappa en port för hello NLB tooa port på en virtuell dator. I det här exemplet skapas en NAT-regel för SSH-trafik tooeach VM i hello NLB uppsättningen.  |
| [Skapa AZ nätverket nsg](https://docs.microsoft.com/cli/azure/network/nsg#create) | Skapar en nätverkssäkerhetsgrupp (NSG), vilket är en säkerhetsgräns mellan hello internet och hello virtuella datorer. |
| [Skapa AZ nätverket nsg regel](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Skapar en NSG regeln tooallow inkommande trafik. I det här exemplet används port 22 för SSH-trafik. |
| [Skapa AZ nätverket nic](https://docs.microsoft.com/cli/azure/network/nic#create) | Skapar ett virtuellt nätverkskort och bifogar den toohello virtuella nätverk och undernät NSG. |
| [Skapa AZ vm tillgänglighetsuppsättning](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en tillgänglighetsuppsättning. Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida hello virtuella datorer mellan fysiska resurser så att om fel uppstår, hello hela uppsättningen inte sker. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
