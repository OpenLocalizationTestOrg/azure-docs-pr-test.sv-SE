---
title: "aaaAzure CLI skriptexempel - Filter VM-nätverkstrafik | Microsoft Docs"
description: "Azure CLI-skript exempel – filtrera inkommande och utgående VM-nätverkstrafik."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: c2f14e54bc96c99420b4300d1c24a457ac8c948c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrera inkommande och utgående nätverkstrafik för VM

Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät. Inkommande nätverkstrafik toohello frontend undernät är begränsad tooHTTP, HTTPS och SSH, medan utgående trafik toohello Internet från hello backend-undernät inte är tillåtet. När du har kört hello skript har du en virtuell dator med två nätverkskort. Varje nätverkskort är anslutet tooa olika undernät.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper. Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ network vnet](/cli/azure/network/vnet#create) | Skapar en Azure-nätverket och frontend-undernät. |
| [Skapa AZ undernät](/cli/azure/network/vnet/subnet#create) | Skapar en backend-undernät. |
| [AZ network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) | Associerar NSG: er toosubnets. |
| [Skapa AZ nätverket offentliga-ip](/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress tooaccess hello VM från hello Internet. |
| [Skapa AZ nätverket nic](/cli/azure/network/nic#create) | Skapar virtuella nätverksgränssnitt och bifogar dem toohello virtuellt frontend och backend-undernät. |
| [Skapa AZ nätverket nsg](/cli/azure/network/nsg#create) | Skapar nätverkssäkerhetsgrupper (NSG) som är associerade toohello frontend och backend-undernät. |
| [Skapa AZ nätverket nsg regel](/cli/azure/network/nsg/rule#create) |Skapar NSG-regler som tillåter eller blockerar specifika portar toospecific undernät. |
| [Skapa AZ vm](/cli/azure/vm#create) | Skapar virtuella datorer och bifogar ett NIC-tooeach VM. Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter. |
| [ta bort grupp AZ](/cli/azure/group#delete) | Tar bort en resursgrupp och alla resurser som den innehåller. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).

Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md)
