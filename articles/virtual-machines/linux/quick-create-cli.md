---
title: aaaAzure Snabbstart - skapa VM-CLI | Microsoft Docs
description: "Lär dig snabbt toocreate virtuella datorer med hello Azure CLI."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0d9c686e2f4c7339b29b8c43cd1dd9ee56d7dc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-cli"></a>Skapa en virtuell Linux-dator med hello Azure CLI

hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Den här guiden information med hjälp av hello Azure CLI toodeploy en virtuell dator som kör Ubuntu server. När hello server har distribuerats, en SSH-anslutning skapas och en NGINX-webbserver är installerad.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Skapa en virtuell dator

Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando. 

hello följande exempel skapas en virtuell dator med namnet *myVM* och skapar SSH-nycklar, om de inte redan finns på standardplatsen nyckel. toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel. Anteckna hello `publicIpAddress`. Den här adressen är används tooaccess hello VM.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Öppna port 80 för webbtrafik 

Som standard tillåts enbart SSH-anslutningar till virtuella Linux-datorer distribuerade i Azure. Om den här virtuella datorn ska toobe en webbserver, måste tooopen port 80 från hello Internet. Använd hello [az vm öppna port](/cli/azure/vm#open-port) kommandot tooopen hello önskad port.  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>SSH till den virtuella datorn

Använd hello följande kommando toocreate en SSH-session med hello virtuell dator. Se till att tooreplace  *<publicIpAddress>*  med hello rätt offentliga IP-adressen för den virtuella datorn.  I vårt exempel ovan var vår IP-adress *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>Installera NGINX

Använd hello följande bash skriptet tooupdate paketet källor och installera hello senaste NGINX-paketet. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-nginx-welcome-page"></a>Visa hello NGINX-välkomstsidan

Du kan använda en webbläsare för dina val tooview hello NGINX välkomstsidan med NGINX installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet. Vara säker på att toouse hello *publicIpAddress* du dokumenterade ovan toovisit hello standardsida. 

![NGINX-standardwebbplats](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a>Rensa resurser

När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver. toolearn mer om Azure-datorer, fortsätta toohello självstudier för Linux virtuella datorer.


> [!div class="nextstepaction"]
> [Självstudier om virtuella Azure Linux-datorer](./tutorial-manage-vm.md)
