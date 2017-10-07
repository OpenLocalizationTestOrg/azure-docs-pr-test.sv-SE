---
title: aaaUse moln init toocustomize en Linux VM | Microsoft Docs
description: Hur toouse moln init toocustomize en Linux VM under genereringen av med hello Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e7297e162fc73f0da42f195bec2fcbe23b310c1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation"></a>Använda molntjänster init toocustomize en Linux VM under skapande av
Den här artikeln visar hur toomake ett moln init skriptet tooset hello värdnamn, Uppdatera installerade paket och hantera användarkonton med hello Azure CLI 2.0. hello molnet init skript kallas när du skapar en virtuell dator (VM) från Azure CLI. En mer detaljerad översikt över installera program skriva konfigurationsfiler och att injicera nycklar från Key Vault finns [självstudierna](tutorial-automate-vm-deployment.md). Du kan också utföra dessa steg med hello [Azure CLI 1.0](using-cloud-init-nodejs.md).

## <a name="quick-commands"></a>Snabbkommandon
Skapa ett moln init.txt skript som anger hello värdnamn, uppdaterar alla paket och lägger till en sudo användare tooLinux.

```yaml
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```

Skapa en resurs grupp toolaunch virtuella datorer i med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar hello resursgrupp med namnet *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start med hello `--custom-data` parameter.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång
När du startar en ny Linux VM får du en standard Linux VM utan något anpassad eller redo för dina behov. [Molnet init](https://cloudinit.readthedocs.org) är ett standardiserat sätt tooinject ett skript eller konfiguration inställningar i den Linux VM som operativsystemet startas för hello första gången.

I Azure, det finns några olika sätt toomake ändringar på en Linux VM som den håller på att distribueras eller startas.

* Mata in skript med hjälp av molnet initiering.
* Mata in skript med hjälp av hello Azure [VMAccess-tillägget](using-vmaccess-extension.md).
* En Azure-mall med hjälp av molnet initiering.
* En Azure-mall med hjälp av [CustomScriptExtention](extensions-customscript.md).

tooinject skript när som helst efter omstart:

* SSH toorun direkt kommandon
* Mata in skript med hjälp av hello Azure [VMAccess-tillägget](using-vmaccess-extension.md), imperatively eller i en Azure-mall
* Verktyg för konfigurationshantering som Ansible, Salt, Chef eller Puppet.

> [!NOTE]
> VMAccess-tillägget körs ett skript som roten i hello samma sätt med hjälp av SSH. Med hjälp av hello VM-tillägget kan dock flera funktioner som Azure-erbjudanden som kan vara användbara beroende på ditt scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Molnet init tillgänglighet på Virtuella Azure-Snabbregistrering avbildningen alias:
| Alias | Utgivare | Erbjudande | SKU | Version | molnet initiering |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |senaste |Nej |
| CoreOS |CoreOS |CoreOS |Stable |senaste |Ja |
| Debian |credativ |Debian |8 |senaste |Nej |
| openSUSE |SUSE |openSUSE |13.2 |senaste |Nej |
| RHEL |Redhat |RHEL |7.2 |senaste |Nej |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |senaste |Ja |

Vi kan arbeta med våra partners tooget moln initiering ingår och arbeta i hello bilder de ger tooAzure.

## <a name="add-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a>Lägg till ett moln init skriptet toohello skapa en virtuell dator med hello Azure CLI
toolaunch ett moln init-skript när du skapar en virtuell dator i Azure, ange hello molnet init-fil med hello Azure CLI `--custom-data` växla.

Skapa en resurs grupp toolaunch virtuella datorer i med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar hello resursgrupp med namnet *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a>Skapa ett moln init skriptet tooset hello värdnamn för en Linux-VM
En av hello enklaste och mest viktiga inställningar för alla Linux-VM är hello värdnamn. Vi kan enkelt ange molnet initiering med det här skriptet.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Exempel molnet init skript som heter `cloud_config_hostname.txt`.
```yaml
#cloud-config
hostname: myservername
```

Under hello första start av hello VM moln init skriptet anger hello värdnamn för*myservername*. Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Logga in och kontrollera hello värdnamnet för hello ny virtuell dator.

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a>Skapa ett moln init-skript
För säkerhet kan du din Ubuntu VM tooupdate på hello första start. Med hjälp av molnet init kan vi göra att med hello följer skript, beroende på hur du använder hello Linux-distribution.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a>Molnet init exempelskriptet `cloud_config_apt_upgrade.txt` för hello Debian-familjen
```yaml
#cloud-config
apt_upgrade: true
```

Efter Linux har startats, alla hello installerade paket uppdateras **lgh get**. Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

Logga in och kontrollera att alla paket har uppdaterats.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
hello following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 tooremove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-tooadd-a-user-toolinux"></a>Skapa en moln-init skriptet tooadd tooLinux för en användare
En av hello första uppgifter på alla nya Linux-VM är tooadd en användare för dig själv eller tooavoid med *rot*. SSH-nycklar är bästa praxis för säkerhet och användbarhet och de läggs toohello *~/.ssh/authorized_keys* fil med det här molnet init-skriptet.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Molnet init exempelskriptet `cloud_config_add_users.txt` Debian-familjen
```yaml
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Efter Linux har startats, är alla hello listas användare skapas och tillagda toohello sudo grupp. Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

Logga in och kontrollera hello nyligen skapade användaren.

```bash
ssh myVM
cat /etc/group
```

Resultat

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Nästa steg
Initiering molnet har blivit ett standardiserat sätt toomodify din Linux VM på Start. Azure har också VM-tillägg som gör att du toomodify din LinuxVM på Start eller medan den körs. Du kan till exempel använda hello Azure VMAccessExtension tooreset SSH eller användarinformation medan hello VM körs. Med molnet initiering behöver du en omstart tooreset hello lösenord.

[Om virtuella datortillägg och funktioner](extensions-features.md)

[Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget](using-vmaccess-extension.md)
