---
title: aaaAzure CLI skriptexempel - installera IIS | Microsoft Docs
description: Azure CLI skriptexempel - installera IIS
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 2fabc9522f424cab4c672084ba8bedfd623c87cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a>Snabbt skapa en virtuell dator med hello Azure CLI

Det här skriptet skapar en Azure-dator som kör Windows Server 2016 och använder hello Azure virtuella tillägget för anpassat skript tooinstall IIS. Du kan komma åt hello IIS-standardwebbplatsen på hello offentliga IP-adress hello virtuella datorn när du har kört hello skript.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm#create) | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [AZ vm öppna-port](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Skapar en network security group regeln tooallow inkommande trafik. I det här exemplet används port 80 för HTTP-trafik. |
| [Azure vm-tillägget uppsättningen](https://docs.microsoft.com/cli/azure/vm/extension#set) | Lägger till och kör en virtuell dator tillägget tooa VM. I det här exemplet är hello tillägget för anpassat skript används tooinstall IIS.|
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
