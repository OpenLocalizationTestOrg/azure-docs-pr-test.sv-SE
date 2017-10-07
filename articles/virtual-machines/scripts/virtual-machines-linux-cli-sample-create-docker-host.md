---
title: "aaaAzure CLI skriptexempel - skapa en Docker-värd | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en Docker-värd"
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
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7df2561c428ff5d3ab0a0d53c8e046781996be77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-docker"></a>Skapa en virtuell dator med Docker

Det här skriptet skapar en virtuell dator med Docker aktiverad och startar en dockerbehållare som kör NGINX. När du har kört hello skript du komma åt hello NGINX-webbservern via hello FQDN för hello virtuella Azure-datorn. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate hello distribution. Varje objekt i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm#create) | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och nätverkssäkerhetsgruppen. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [AZ vm öppna-port](https://docs.microsoft.com/cli/azure/vm#open-port) | Skapar en network security group regeln tooallow inkommande trafik. I det här exemplet används port 80 för HTTP-trafik. |
| [Azure vm-tillägget uppsättningen](https://docs.microsoft.com/cli/azure/vm/extension#set) | Lägger till och kör en virtuell dator tillägget tooa VM. I det här exemplet är hello Docker VM-tillägget används tooconfigure en Docker-värd.|
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
