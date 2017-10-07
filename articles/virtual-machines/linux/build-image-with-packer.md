---
title: "aaaHow toocreate Linux Azure VM-avbildningar med förpackaren | Microsoft Docs"
description: "Lär dig hur toouse förpackaren toocreate avbildningar av virtuella Linux-datorer i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 5990598859e73efac477884bc8de5fd5138bf6e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-packer-toocreate-linux-virtual-machine-images-in-azure"></a>Hur toouse förpackaren toocreate Linux virtuella bilder i Azure
Varje virtuell dator (VM) i Azure skapas från en avbildning som definierar hello distribution för Linux och OS-version. Avbildningar kan innehålla förinstallerade program och konfigurationer. hello Azure Marketplace innehåller många första och tredje parts avbildningar för programmet miljöer och de vanligaste distributioner eller du kan skapa dina egna anpassade avbildningar som är anpassade tooyour behov. Den här artikeln beskrivs hur toouse hello Öppna källa för [förpackaren](https://www.packer.io/) toodefine och skapa anpassade avbildningar i Azure.


## <a name="create-azure-resource-group"></a>Skapa Azure-resursgrupp
Under hello build processen skapar förpackaren tillfälliga Azure-resurser som den skapar Virtuella hello källdatorn. toocapture som datakällan VM för användning som en avbildning måste du definiera en resursgrupp. hello lagras utdata från hello förpackaren skapandeprocess i den här resursgruppen.

Skapa en resursgrupp med [az group create](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Skapa autentiseringsuppgifter för Azure
Packare autentiserar med Azure med hjälp av ett huvudnamn för tjänsten. En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som förpackaren. Du styr och definiera hello behörigheter toowhat operations hello tjänstens huvudnamn kan utföra i Azure.

Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata hello autentiseringsuppgifter som förpackaren måste:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

Ett exempel på hello utdata från hello föregående kommandon är följande:

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

tooauthenticate tooAzure du måste också tooobtain din Azure-prenumeration med ID med [az konto visa](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

Du kan använda hello utdata från dessa två kommandon i hello nästa steg.


## <a name="define-packer-template"></a>Definiera förpackaren mall
toobuild bilder du skapar en mall som en JSON-fil. Du definierar builders i hello mall och provisioners som utför hello faktiska Byggprocessen. Förpackaren har en [provisioner för Azure](https://www.packer.io/docs/builders/azure.html) som du kan toodefine Azure resurser, till exempel hello huvudnamn Tjänstereferenser skapade i föregående steg hello.

Skapa en fil med namnet *ubuntu.json* och klistra in hello följande innehåll. Ange egna värden för hello följande:

| Parameter                           | Där tooobtain |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Första raden i utdata från `az ad sp` skapa kommando - *appId* |
| *client_secret*                     | Andra raden på utdata från `az ad sp` skapa kommando - *lösenord* |
| *tenant_id*                         | Tredje raden på utdata från `az ad sp` skapa kommando - *klient* |
| *PRENUMERATIONSID*                   | Utdata från `az account show` kommando |
| *managed_image_resource_group_name* | Namnet på resursgruppen som du skapade i hello första steg |
| *managed_image_name*                | Namn för hello hanterad avbildning som har skapats |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

Den här mallen bygger en avbildning Ubuntu 16.04 LTS, installerar NGINX sedan deprovisions hello VM.

> [!NOTE]
> Om du expanderar på den här mallen tooprovision användarens autentiseringsuppgifter, justera hello provisioner kommandot att deprovisions hello Azure-agenten tooread `-deprovision` snarare än `deprovision+user`.
> Hej `+user` flaggan tar bort alla användarkonton från Virtuella hello källdatorn.


## <a name="build-packer-image"></a>Skapa förpackaren bild
Om du inte redan har förpackaren installerad på din lokala dator [följa hello förpackaren Installationsinstruktioner](https://www.packer.io/docs/install/index.html).

Skapa hello bilden genom att ange din förpackaren mallfilen på följande sätt:

```bash
./packer build ubuntu.json
```

Ett exempel på hello utdata från hello föregående kommandon är följande:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting hello VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH toobecome available...
==> azure-arm: Connected tooSSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! hello waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able toologin as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying hello machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting hello temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. hello artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```


## <a name="create-vm-from-azure-image"></a>Skapa virtuell dator från Azure-avbildning
Nu kan du skapa en virtuell dator från avbildningen med [az vm skapa](/cli/azure/vm#create). Ange hello bilden som du skapade med hello `--image` parameter. hello följande exempel skapas en virtuell dator med namnet *myVM* från *myPackerImage* och genererar SSH-nycklar, om de inte redan finns:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Det tar några minuter toocreate hello VM. När du har skapat hello VM anteckna hello `publicIpAddress` visas av hello Azure CLI. Den här adressen är används tooaccess hello NGINX plats via en webbläsare.

tooallow web tooreach trafik den virtuella datorn, öppna port 80 från hello Internet med [az vm öppna port](/cli/azure/vm#open-port):

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Testa virtuell dator och NGINX
Nu kan du öppna en webbläsare och ange `http://publicIpAddress` i hello adressfältet. Ange dina egna offentliga IP-adress från hello VM skapa processen. hello visas standard NGINX som i följande exempel hello:

![NGINX-standardwebbplats](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Nästa steg
I det här exemplet används förpackaren toocreate en VM-avbildning med NGINX som redan har installerats. Du kan använda den här Virtuella datorn tillsammans med befintliga arbetsflöden, till exempel toodeploy app-tooVMs skapas från hello avbildningen med Ansible, Chef eller Puppet.

Ytterligare exempel förpackaren mallar för andra Linux-distributioner, se [GitHub-repo](https://github.com/hashicorp/packer/tree/master/examples/azure).