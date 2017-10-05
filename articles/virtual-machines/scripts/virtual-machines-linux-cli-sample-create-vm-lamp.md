---
title: "Azure CLI Script exempel - distribuera LYKTA stacken i en Skaluppsättning för Utjämning av nätverksbelastning virtuellt Machin | Microsoft Docs"
description: "Använda tillägget för anpassat skript för att distribuera LYKTA stacken i en = belastningsutjämnade virtuella skaluppsättningen på Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 4007e8c85c0ff24bf5030881eac666582714eae3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Distribuera LYKTA stacken i en belastningsutjämnad virtuella datorns skaluppsättning

Det här exemplet skapar en skaluppsättning för virtuell dator och tillämpar ett tillägg som kör ett anpassat skript för att distribuera LYKTA stacken på varje virtuell dator i skaluppsättning.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "skapa virtuella skala med LYKTA stack")]

## <a name="connect"></a>Anslut

Använd den här koden för att se hur du ansluter till din virtuella dator och din skala anges.

[!code-azurecli[huvudsakliga](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "åtkomst till virtuella datorns skaluppsättning")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen och skaluppsättning för virtuella datorer och alla relaterade resurser.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ vmss](https://docs.microsoft.com/cli/azure/vmss#create) | Skapar en skaluppsättning för virtuell dator |
| [Skapa AZ nätverket lb regel](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Lägg till en slutpunkt för Utjämning av nätverksbelastning |
| [AZ vmss tillägget uppsättningen](https://docs.microsoft.com/cli/azure/vmss/extension#set) | Skapa tillägg som körs skriptet på distribution av en virtuell dator |
| [AZ vmss update-instanser](https://docs.microsoft.com/cli/azure/vmss#update-instances) | Kör skriptet på VM-instanser som har distribuerats innan tillägget har tillämpats på skaluppsättning. |
| [AZ vmss skala](https://docs.microsoft.com/cli/azure/vmss#scale) | Skala upp skalan genom att lägga till flera VM-instanser. Anpassat skript körs på dessa när de distribueras. |
| [AZ nätverket offentliga ip-lista](https://docs.microsoft.com/cli/azure/network/public-ip#list) | Hämta IP-adresserna för de virtuella datorerna som skapats av exemplet. |
| [AZ nätverket lb visa](https://docs.microsoft.com/cli/azure/network/lb#show) | Hämta klient- och servergränssnitten portar som används av belastningsutjämnaren. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
