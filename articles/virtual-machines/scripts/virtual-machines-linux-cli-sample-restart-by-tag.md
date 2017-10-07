---
title: aaaAzure CLI skriptexempel - starta om VMs | Microsoft Docs
description: 'Skriptexempel Azure CLI - omstart av virtuella datorer efter tagg och -ID: t'
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
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: a1ae07bd1d2be906553bef817385a4a333a10eca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restart-vms"></a>Starta om virtuella datorer

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Det här exemplet visar några olika sätt tooget vissa virtuella datorer och starta om dem.

hello startar om alla hello virtuella datorer i resursgruppen hello först.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

hello andra hämtar hello märkta virtuella datorer med `az resouce list` filtrerar toohello resurser som virtuella datorer och startar om dessa virtuella datorer.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Det här exemplet fungerar i ett Bash-gränssnitt. Alternativen på Azure CLI skriptkörning på Windows-klient kan se [kör Windows hello Azure CLI](../windows/cli-options.md).


## <a name="sample-script"></a>Exempelskript

hello exemplet har tre skript.
Hej först en bestämmelser hello virtuella datorer.
Hello Nej vänta alternativet används så hello kommando returnerar utan att vänta på varje VM-toobe etableras.
hello väntar andra hello VMs toobe helt etablerad.
hello tredje skript startar om alla hello VMs som etablerades och sedan bara hello märkta virtuella datorer.

### <a name="provision-hello-vms"></a>Etablera hello virtuella datorer

Det här skriptet skapar en resursgrupp och skapar sedan tre virtuella datorer toorestart.
Två av dem märks.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision hello VMs")]

### <a name="wait"></a>Vänta

Det här skriptet kontrollerar hello Etableringsstatus var 20 sekunder tills alla tre virtuella datorer är etablerad, eller en av dem misslyckas tooprovision.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for hello VMs toobe provisioned")]

### <a name="restart-hello-vms"></a>Starta om hello virtuella datorer

Det här skriptet startar om alla hello virtuella datorer i resursgruppen hello och startas sedan om bara hello märkta virtuella datorer.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupper, virtuella datorer och alla relaterade resurser.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Skapar hello virtuella datorer.  |
| [AZ vm lista](https://docs.microsoft.com/cli/azure/vm#list) | Används med `--query` tooensure hello VMs etableras innan du startar om dem och sedan tooget hello-ID för hello VMs toorestart dem. |
| [lista över resurser AZ](https://docs.microsoft.com/cli/azure/vm#list) | Används med `--query` tooget hello-ID för hello virtuella datorer med hello-tagg. |
| [AZ vm-omstart](https://docs.microsoft.com/cli/azure/vm#list) | Startar om hello virtuella datorer. |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
