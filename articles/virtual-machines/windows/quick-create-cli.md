---
title: aaaAzure Snabbstart - skapa Windows VM CLI | Microsoft Docs
description: "Lär dig snabbt toocreate en Windows-datorer med hello Azure CLI."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 029bdecec219b12b80b958ceeedda214f1b13149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-cli"></a>Skapa en virtuell Windows-dator med hello Azure CLI

hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Den här guiden information med hjälp av hello Azure CLI toodeploy en virtuell dator som kör Windows Server 2016. När installationen är klar använder vi ansluter toohello server och installera IIS.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med [az group create](/cli/azure/group#create). En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Skapa en virtuell dator

Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). 

hello följande exempel skapas en virtuell dator med namnet *myVM*. Det här exemplet används *azureuser* för en administrativ användarnamn och *myPassword12* som hello lösenord. Uppdatera dessa värden toosomething lämpliga tooyour miljö. Dessa värden krävs när du skapar en anslutning med hello virtuell dator.

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel. Anteckna hello `publicIpAaddress`. Den här adressen är används tooaccess hello VM.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Öppna port 80 för webbtrafik 

Som standard tillåts bara en RDP-anslutningar i tooWindows virtuella datorer i Azure. Om den här virtuella datorn ska toobe en webbserver, måste tooopen port 80 från hello Internet. Använd hello [az vm öppna port](/cli/azure/vm#open-port) kommandot tooopen hello önskad port.  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-toovirtual-machine"></a>Ansluta toovirtual datorn

Använd hello följande kommando toocreate en fjärrskrivbordssession med hello virtuell dator. Ersätt hello IP-adress med hello offentliga IP-adressen för den virtuella datorn. När du uppmanas ange hello-autentiseringsuppgifter som används när du skapar hello virtuell dator.

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a>Installera IIS med PowerShell

Nu när du har loggat in toohello Azure VM, kan du använda en rad med PowerShell tooinstall IIS och aktivera hello lokala brandväggen regeln tooallow webbtrafik. Öppna en PowerShell-kommandotolk och kör följande kommando hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a>Visa hello välkomstsidan för IIS

Du kan använda en webbläsare för din choice tooview hello IIS välkomstsidan med IIS installerat och port 80 som nu är öppet på den virtuella datorn från hello Internet. Vara säker på att toouse hello offentliga IP-adressen du dokumenterade över toovisit hello standardsida. 

![Standardwebbplatsen i IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Rensa resurser

När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver. toolearn mer om Azure-datorer, fortsätta toohello självstudier för virtuella Windows-datorer.

> [!div class="nextstepaction"]
> [Självstudier om virtuella Azure Windows-datorer](./tutorial-manage-vm.md)
