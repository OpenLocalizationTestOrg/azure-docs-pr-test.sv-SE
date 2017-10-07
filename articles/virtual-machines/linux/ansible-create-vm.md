---
title: "aaaUse Ansible toocreate en grundläggande Linux VM i Azure | Microsoft Docs"
description: "Lär dig hur toouse Ansible toocreate och hantera en grundläggande Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2017
ms.author: iainfou
ms.openlocfilehash: ffe278b3f846924ff9c4d026120565146f951152
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Skapa en grundläggande virtuell dator i Azure med Ansible
Ansible kan du tooautomate hello distributionen och konfigurationen av resurser i din miljö. Du kan använda Ansible toomanage dina virtuella datorer (VM) i Azure, hello samma precis som alla andra resurser. Den här artikeln beskrivs hur du toocreate en grundläggande virtuell dator med Ansible. Du kan också lära dig hur för[skapa en fullständig VM-miljö med Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Krav
toomanage Azure resurser med Ansible måste hello följande:

- Ansible och hello Azure Python SDK-moduler som installerats på värdsystemet.
    - Installera Ansible på [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), och [SLES 12,2 SP2](ansible-install-configure.md#sles-122-sp2)
- Autentiseringsuppgifter för Azure och Ansible konfigurerats toouse dem.
    - [Skapa autentiseringsuppgifter för Azure och konfigurera Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. 
    - Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.


## <a name="create-supporting-azure-resources"></a>Skapa stöd för Azure-resurser
I det här exemplet skapar vi en runbook som distribuerar en virtuell dator i en befintlig infrastruktur. Börja med att skapa resursgrupp med [az gruppen skapa](/cli/azure/vm#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa ett virtuellt nätverk för den virtuella datorn med [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och ett undernät med namnet *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Skapa och köra Ansible playbook
Skapa en Ansible playbook med namnet **azure_create_vm.yml** och klistra in hello efter innehållet. Det här exemplet skapar en enda virtuell dator och konfigurerar SSH-autentiseringsuppgifter. Ange dina egna offentliga nyckel data i hello *key_data* koppla på följande sätt:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

toocreate hello virtuell dator med Ansible, köra hello playbook på följande sätt:

```bash
ansible-playbook azure_create_vm.yml
```

hello utdata ser ut ungefär toohello följande exempel som visar hello VM har skapats:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Nästa steg
Det här exemplet skapar en virtuell dator i en befintlig resursgrupp och med ett virtuellt nätverk som redan har distribuerats. För en mer ingående exempel på hur toouse Ansible toocreate stödjande resurser, till exempel ett virtuellt nätverk och Nätverkssäkerhetsgruppen regler, se [skapa en fullständig VM-miljö med Ansible](ansible-create-complete-vm.md).
