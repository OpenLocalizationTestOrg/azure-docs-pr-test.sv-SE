---
title: "aaaAzure CLI skriptexempel - vägtrafik via en virtuell nätverksenhet | Microsoft Docs"
description: "Azure CLI skriptexempel - vägtrafik via en virtuell nätverksenhet för brandväggen."
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
ms.openlocfilehash: 981d6073be04a7ebaf96b657fbab8a378e7a995e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Vidarebefordra trafik via en virtuell nätverksenhet

Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät. Dessutom skapas en virtuell dator med IP-vidarebefordring aktiverade tooroute trafik mellan hello två undernät. Du kan distribuera programvara för nätverk, till exempel en brandvägg programmet toohello VM när du har kört hello skript.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Exempelskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

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
| [Skapa AZ undernät](/cli/azure/network/vnet/subnet#create) | Skapar backend- och DMZ undernät. |
| [Skapa AZ nätverket offentliga-ip](/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress tooaccess hello VM från hello Internet. |
| [Skapa AZ nätverket nic](/cli/azure/network/nic#create) | Skapar ett virtuellt nätverksgränssnitt och aktivera IP-vidarebefordring för den. |
| [Skapa AZ nätverket nsg](/cli/azure/network/nsg#create) | Skapar en nätverkssäkerhetsgrupp (NSG). |
| [Skapa AZ nätverket nsg regel](/cli/azure/network/nsg/rule#create) | Skapar NSG-regler som tillåter HTTP och HTTPS-portar inkommande toohello VM. |
| [AZ network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update)| Associerats hello NSG: er och väg tabeller toosubnets. |
| [Skapa AZ-routningstabellen för nätverk](/cli/azure/network/route-table#create)| Skapar en routningstabell för alla flöden. |
| [Skapa AZ nätverksväg routningstabellen](/cli/azure/network/route-table/route#create)| Skapar vägar tooroute trafik mellan undernät och hello Internet via hello VM. |
| [Skapa AZ vm](/cli/azure/vm#create) | Skapar en virtuell dator och bifogar hello NIC tooit. Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter. |
| [ta bort grupp AZ](/cli/azure/group#delete) | Tar bort en resursgrupp och alla resurser som den innehåller. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).

Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md)
