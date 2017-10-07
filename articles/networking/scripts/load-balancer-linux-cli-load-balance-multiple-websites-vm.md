---
title: "aaaAzure CLI skriptexempel - belastningsutjämning för flera webbplatser med hello Azure CLI | Microsoft Docs"
description: "Azure CLI skriptexempel - belastningsutjämning för flera webbplatser toohello samma virtuella dator"
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
ms.openlocfilehash: 136da5d1783fb9f9dc87f1ffad8eec7b95c6bd7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a>Belastningsutjämning för flera webbplatser

Det här exemplet i skriptet skapar ett virtuellt nätverk med två virtuella datorer (VM) som är medlemmar i en tillgänglighetsuppsättning. En belastningsutjämnare dirigerar trafik för två separata IP-adresser toohello två virtuella datorer. Efter köra hello skriptet kan du distribuera web server programvara toohello virtuella datorer och vara värd för flera webbplatser, var och en med sin egen IP-adress.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella nätverk, läsa in belastningsutjämning och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ network vnet](https://docs.microsoft.com/cli/azure/network/vnet#create) | Skapar ett virtuellt Azure-nätverk och undernät. |
| [Skapa AZ nätverket offentliga-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Skapar en offentlig IP-adress med en statisk IP-adress och en tillhörande DNS-namn. |
| [Skapa AZ nätverket lb](https://docs.microsoft.com/cli/azure/network/lb#create) | Skapar en Azure belastningsutjämnare. |
| [Skapa AZ nätverket lb avsökning](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Skapar en belastningsutjämningsavsökning. En belastningsutjämningsavsökning är används toomonitor varje virtuell dator i hello belastningen belastningsutjämnaren uppsättningen. Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM. |
| [Skapa AZ nätverket lb regel](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en regel för belastningsutjämnare. I det här exemplet skapas en regel för port 80. HTTP-trafik anländer till hello belastningsutjämnare, är det routade tooport 80 en hello virtuella datorer i hello belastningen belastningsutjämnaren uppsättningen. |
| [Skapa AZ nätverket lb klientdels-ip](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | Skapa en IP-adress för klientdel för hello belastningsutjämnaren. |
| [Skapa AZ lb-nätverksadresspool](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | Skapar en backend-adresspool. |
| [Skapa AZ nätverket nic](https://docs.microsoft.com/cli/azure/network/nic#create) | Skapar ett virtuellt nätverkskort och bifogar den toohello virtuellt nätverk och undernät. |
| [Skapa AZ vm tillgänglighetsuppsättning](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Skapar en tillgänglighetsuppsättning. Tillgänglighetsuppsättningar garantera programmets drifttid genom att sprida hello virtuella datorer mellan fysiska resurser så att om fel uppstår, hello hela uppsättningen inte sker. |
| [Skapa AZ nätverket nic ip-konfiguration](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | Skapar en IP-confiuration. Du måste ha hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic funktionen är aktiverad för din prenumeration. Bara en konfiguration kan utses som hello primära IP-konfiguration per NIC, med hjälp av hello--gör primär flaggan. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
