---
title: "aaaAzure CLI skriptexempel - distribuera hello LYKTA Stack i en Load-Balanced virtuellt Machin Skaluppsättning | Microsoft Docs"
description: "Använd ett anpassat skript för tillägget toodeploy hello LYKTA Stack i en = belastningsutjämnade virtuella skaluppsättningen på Azure."
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
ms.openlocfilehash: d5278db809faaa0997a08b00a53387d754fce3d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Distribuera hello LYKTA stacken i en belastningsutjämnad virtuella datorns skaluppsättning

Det här exemplet skapar en skaluppsättning för virtuell dator och tillämpar ett tillägg som kör ett anpassat skript toodeploy hello LYKTA stacken på varje virtuell dator i skaluppsättning för hello.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Anslut

Använd den här koden toosee hur tooconnect tooyour virtuella datorer och din skala in.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access hello virtual machine scale set")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, hello skaluppsättning och virtuella datorer och alla relaterade resurser hello.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ vmss](https://docs.microsoft.com/cli/azure/vmss#create) | Skapar en skaluppsättning för virtuell dator |
| [Skapa AZ nätverket lb regel](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Lägg till en slutpunkt för Utjämning av nätverksbelastning |
| [AZ vmss tillägget uppsättningen](https://docs.microsoft.com/cli/azure/vmss/extension#set) | Skapa hello-tillägg som kör hello anpassat skript för distribution av en virtuell dator |
| [AZ vmss update-instanser](https://docs.microsoft.com/cli/azure/vmss#update-instances) | Kör hello anpassat skript på hello VM-instanser som har distribuerats innan hello tillägget installerades toohello skaluppsättning. |
| [AZ vmss skala](https://docs.microsoft.com/cli/azure/vmss#scale) | Skala upp hello skala genom att lägga till flera VM-instanser. hello anpassade skript körs om dem när de har distribuerats. |
| [AZ nätverket offentliga ip-lista](https://docs.microsoft.com/cli/azure/network/public-ip#list) | Hämta hello IP-adresser för hello virtuella datorer som skapats av hello exempel. |
| [AZ nätverket lb visa](https://docs.microsoft.com/cli/azure/network/lb#show) | Hämta hello frontend och backend portar som används av hello belastningsutjämnaren. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
