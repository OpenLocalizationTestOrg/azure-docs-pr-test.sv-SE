---
title: "aaaUse Ansible toocreate en fullständig Linux VM i Azure | Microsoft Docs"
description: "Lär dig hur toouse Ansible toocreate och hantera en fullständig miljö med Linux virtuella datorer i Azure"
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
ms.openlocfilehash: 970b0427f39fc23240f9faab868196ca4f444e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Skapa en miljö med fullständiga Linux virtuella datorer i Azure med Ansible
Ansible kan du tooautomate hello distributionen och konfigurationen av resurser i din miljö. Du kan använda Ansible toomanage dina virtuella datorer (VM) i Azure, hello samma precis som alla andra resurser. Den här artikeln beskrivs hur du toocreate en fullständig Linux-miljö och ge support för resurser med Ansible. Du kan också lära dig hur för[skapa en grundläggande virtuell dator med Ansible](ansible-create-vm.md).


## <a name="prerequisites"></a>Krav
toomanage Azure resurser med Ansible måste hello följande:

- Ansible och hello Azure Python SDK-moduler som installerats på värdsystemet.
    - Installera Ansible på [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), och [SLES 12,2 SP2](ansible-install-configure.md#sles-122-sp2)
- Autentiseringsuppgifter för Azure och Ansible konfigurerats toouse dem.
    - [Skapa autentiseringsuppgifter för Azure och konfigurera Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. 
    - Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.


## <a name="create-virtual-network"></a>Skapa det virtuella nätverket
hello följande avsnitt i en Ansible playbook skapar ett virtuellt nätverk med namnet *myVnet* i hello *10.0.0.0/16* adressutrymmet:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

tooadd ett undernät hello efter avsnittet skapar ett undernät med namnet *mySubnet* i hello *myVnet* virtuellt nätverk:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Skapa offentlig IP-adress
tooaccess resurser över hello Internet, skapa och tilldela en offentlig IP-adress tooyour VM. hello följande avsnitt i en Ansible playbook skapar en offentlig IP-adress med namnet *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Skapa Nätverkssäkerhetsgrupp
Nätverkssäkerhetsgrupper styra hello flödet av nätverkstrafiken till och från den virtuella datorn. hello följande avsnitt i en Ansible playbook skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* och definierar en regel tooallow SSH-trafik på TCP-port 22:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: TCP
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a>Skapa virtuella nätverkskort
Ett virtuellt nätverkskort (NIC) ansluter din VM tooa givet virtuellt nätverk, offentlig IP-adress och nätverkssäkerhetsgruppen. hello följande avsnitt i en Ansible playbook skapar ett virtuellt nätverkskort med namnet *myNIC* anslutna toohello virtuella nätverk resurser du har skapat:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a>Skapa en virtuell dator
hello sista steget är toocreate en virtuell dator och använda alla hello resurser skapas. hello följande avsnitt i en Ansible playbook skapar en virtuell dator med namnet *myVM* och bifogar hello virtuellt nätverkskort med namnet *myNIC*. Ange dina egna offentliga nyckel data i hello *key_data* koppla på följande sätt:

```yaml
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
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.3'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Slutföra Ansible playbook
toobring dessa avsnitt tillsammans, skapa en Ansible playbook med namnet *azure_create_complete_vm.yml* och klistra in hello följande innehåll:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
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
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

Ansible måste en resurs grupp toodeploy alla resurser i. Skapa en resursgrupp med [az group create](/cli/azure/vm#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

toocreate hello fullständig VM-miljö med Ansible, köra hello playbook på följande sätt:

```bash
ansible-playbook azure_create_complete_vm.yml
```

hello utdata ser ut ungefär toohello följande exempel som visar hello VM har skapats:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Nästa steg
Det här exemplet skapas en fullständig VM-miljö inklusive hello krävs virtuella nätverksresurser. En mer direkt exempel toocreate en virtuell dator i befintliga nätverksresurser med standardalternativ, se [skapa en virtuell dator](ansible-create-vm.md).
