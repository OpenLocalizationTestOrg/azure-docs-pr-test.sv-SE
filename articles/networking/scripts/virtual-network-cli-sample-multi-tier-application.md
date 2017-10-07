---
title: "aaaAzure CLI skript exempel – skapa ett nätverk för flera nivåer program | Microsoft Docs"
description: "Azure CLI-skript exempel – skapa ett virtuellt nätverk för program på flera nivåer."
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
ms.openlocfilehash: deeb3f459499cebd1b8ded6a299eb759d49cf08d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Skapa ett nätverk för flera nivåer program

Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät. Trafik toohello frontend undernät är begränsad tooHTTP och SSH, medan trafik toohello backend-undernät är begränsad tooMySQL, port 3306. Efter körs hello skript har två virtuella datorer i varje undernät som du kan distribuera webbservern och MySQL programvaran till.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Exempelskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

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
| [Skapa AZ nätverket offentliga-ip](/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress tooaccess hello VM från hello Internet. |
| [Skapa AZ nätverket nic](/cli/azure/network/nic#create) | Skapar virtuella nätverksgränssnitt och bifogar dem toohello virtuellt frontend och backend-undernät. |
| [Skapa AZ nätverket nsg](/cli/azure/network/nsg#create) | Skapar nätverkssäkerhetsgrupper (NSG) som är associerade toohello frontend och backend-undernät. |
| [Skapa AZ nätverket nsg regel](/cli/azure/network/nsg/rule#create) |Skapar NSG-regler som tillåter eller blockerar specifika portar toospecific undernät. |
| [Skapa AZ vm](/cli/azure/vm#create) | Skapar virtuella datorer och bifogar ett NIC-tooeach VM. Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter. |
| [ta bort grupp AZ](/cli/azure/group#delete) | Tar bort en resursgrupp och alla resurser som den innehåller. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).

Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md)
