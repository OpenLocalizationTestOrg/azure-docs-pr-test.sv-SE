---
title: "Använd Ansible för att skapa en grundläggande Linux VM i Azure | Microsoft Docs"
description: "Lär dig hur du använder Ansible för att skapa och hantera en grundläggande Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 184a30c91de0d4141d6bd8a8b9db93c539e083b5
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Skapa en grundläggande virtuell dator i Azure med Ansible
Ansible kan du automatisera distributionen och konfigurationen av resurser i din miljö. Du kan använda Ansible för att hantera dina virtuella datorer (VM) i Azure, samma som någon annan resurs. Den här artikeln visar hur du skapar en grundläggande virtuell dator med Ansible. Du kan också lära dig hur du [skapa en fullständig VM-miljö med Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Förutsättningar
Om du vill hantera Azure-resurser med Ansible, behöver du följande:

- Ansible och Azure SDK för Python-moduler som har installerats på värdsystemet.
    - Installera Ansible på [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), och [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Konfigurera om du använder dessa autentiseringsuppgifter för Azure och Ansible.
    - [Skapa autentiseringsuppgifter för Azure och konfigurera Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. 
    - Om du behöver uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.


## <a name="create-supporting-azure-resources"></a>Skapa stöd för Azure-resurser
I det här exemplet skapar du en runbook som distribuerar en virtuell dator i en befintlig infrastruktur. Börja med att skapa resursgrupp med [az gruppen skapa](/cli/azure/vm#create). I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa ett virtuellt nätverk för den virtuella datorn med [az network vnet skapa](/cli/azure/network/vnet#create). I följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och ett undernät med namnet *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Skapa och köra Ansible playbook
Skapa en Ansible playbook med namnet *azure_create_vm.yml* och klistra in följande innehåll. Det här exemplet skapar en enda virtuell dator och konfigurerar SSH-autentiseringsuppgifter. Ange dina egna fullständig offentliga nyckel data i den *key_data* koppla på följande sätt:

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

Om du vill skapa den virtuella datorn med Ansible, kör du playbook på följande sätt:

```bash
ansible-playbook azure_create_vm.yml
```

Utdata liknar följande exempel som visar den virtuella datorn har skapats:

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
Det här exemplet skapar en virtuell dator i en befintlig resursgrupp och med ett virtuellt nätverk som redan har distribuerats. En mer ingående exempel på hur du använder Ansible för att skapa stödjande resurser, till exempel ett virtuellt nätverk och Nätverkssäkerhetsgruppen regler finns [skapa en fullständig VM-miljö med Ansible](ansible-create-complete-vm.md).
