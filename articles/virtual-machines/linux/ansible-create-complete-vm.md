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
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a><span data-ttu-id="7c526-103">Skapa en miljö med fullständiga Linux virtuella datorer i Azure med Ansible</span><span class="sxs-lookup"><span data-stu-id="7c526-103">Create a complete Linux virtual machine environment in Azure with Ansible</span></span>
<span data-ttu-id="7c526-104">Ansible kan du tooautomate hello distributionen och konfigurationen av resurser i din miljö.</span><span class="sxs-lookup"><span data-stu-id="7c526-104">Ansible allows you tooautomate hello deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="7c526-105">Du kan använda Ansible toomanage dina virtuella datorer (VM) i Azure, hello samma precis som alla andra resurser.</span><span class="sxs-lookup"><span data-stu-id="7c526-105">You can use Ansible toomanage your virtual machines (VMs) in Azure, hello same as you would any other resource.</span></span> <span data-ttu-id="7c526-106">Den här artikeln beskrivs hur du toocreate en fullständig Linux-miljö och ge support för resurser med Ansible.</span><span class="sxs-lookup"><span data-stu-id="7c526-106">This article shows you how toocreate a complete Linux environment and supporting resources with Ansible.</span></span> <span data-ttu-id="7c526-107">Du kan också lära dig hur för[skapa en grundläggande virtuell dator med Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="7c526-107">You can also learn how too[Create a basic VM with Ansible](ansible-create-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7c526-108">Krav</span><span class="sxs-lookup"><span data-stu-id="7c526-108">Prerequisites</span></span>
<span data-ttu-id="7c526-109">toomanage Azure resurser med Ansible måste hello följande:</span><span class="sxs-lookup"><span data-stu-id="7c526-109">toomanage Azure resources with Ansible, you need hello following:</span></span>

- <span data-ttu-id="7c526-110">Ansible och hello Azure Python SDK-moduler som installerats på värdsystemet.</span><span class="sxs-lookup"><span data-stu-id="7c526-110">Ansible and hello Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="7c526-111">Installera Ansible på [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), och [SLES 12,2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="7c526-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="7c526-112">Autentiseringsuppgifter för Azure och Ansible konfigurerats toouse dem.</span><span class="sxs-lookup"><span data-stu-id="7c526-112">Azure credentials, and Ansible configured toouse them.</span></span>
    - [<span data-ttu-id="7c526-113">Skapa autentiseringsuppgifter för Azure och konfigurera Ansible</span><span class="sxs-lookup"><span data-stu-id="7c526-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="7c526-114">Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7c526-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="7c526-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="7c526-115">Run `az --version` toofind hello version.</span></span> 
    - <span data-ttu-id="7c526-116">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7c526-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="7c526-117">Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="7c526-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-virtual-network"></a><span data-ttu-id="7c526-118">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="7c526-118">Create virtual network</span></span>
<span data-ttu-id="7c526-119">hello följande avsnitt i en Ansible playbook skapar ett virtuellt nätverk med namnet *myVnet* i hello *10.0.0.0/16* adressutrymmet:</span><span class="sxs-lookup"><span data-stu-id="7c526-119">hello following section in an Ansible playbook creates a virtual network named *myVnet* in hello *10.0.0.0/16* address space:</span></span>

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

<span data-ttu-id="7c526-120">tooadd ett undernät hello efter avsnittet skapar ett undernät med namnet *mySubnet* i hello *myVnet* virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="7c526-120">tooadd a subnet, hello following section creates a subnet named *mySubnet* in hello *myVnet* virtual network:</span></span>

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a><span data-ttu-id="7c526-121">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="7c526-121">Create public IP address</span></span>
<span data-ttu-id="7c526-122">tooaccess resurser över hello Internet, skapa och tilldela en offentlig IP-adress tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="7c526-122">tooaccess resources across hello Internet, create and assign a public IP address tooyour VM.</span></span> <span data-ttu-id="7c526-123">hello följande avsnitt i en Ansible playbook skapar en offentlig IP-adress med namnet *myPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="7c526-123">hello following section in an Ansible playbook creates a public IP address named *myPublicIP*:</span></span>

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a><span data-ttu-id="7c526-124">Skapa Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="7c526-124">Create Network Security Group</span></span>
<span data-ttu-id="7c526-125">Nätverkssäkerhetsgrupper styra hello flödet av nätverkstrafiken till och från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7c526-125">Network Security Groups control hello flow of network traffic in and out of your VM.</span></span> <span data-ttu-id="7c526-126">hello följande avsnitt i en Ansible playbook skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* och definierar en regel tooallow SSH-trafik på TCP-port 22:</span><span class="sxs-lookup"><span data-stu-id="7c526-126">hello following section in an Ansible playbook creates a network security group named *myNetworkSecurityGroup* and defines a rule tooallow SSH traffic on TCP port 22:</span></span>

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


## <a name="create-virtual-network-interface-card"></a><span data-ttu-id="7c526-127">Skapa virtuella nätverkskort</span><span class="sxs-lookup"><span data-stu-id="7c526-127">Create virtual network interface card</span></span>
<span data-ttu-id="7c526-128">Ett virtuellt nätverkskort (NIC) ansluter din VM tooa givet virtuellt nätverk, offentlig IP-adress och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="7c526-128">A virtual network interface card (NIC) connects your VM tooa given virtual network, public IP address, and network security group.</span></span> <span data-ttu-id="7c526-129">hello följande avsnitt i en Ansible playbook skapar ett virtuellt nätverkskort med namnet *myNIC* anslutna toohello virtuella nätverk resurser du har skapat:</span><span class="sxs-lookup"><span data-stu-id="7c526-129">hello following section in an Ansible playbook creates a virtual NIC named *myNIC* connected toohello virtual networking resources you have created:</span></span>

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


## <a name="create-virtual-machine"></a><span data-ttu-id="7c526-130">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7c526-130">Create virtual machine</span></span>
<span data-ttu-id="7c526-131">hello sista steget är toocreate en virtuell dator och använda alla hello resurser skapas.</span><span class="sxs-lookup"><span data-stu-id="7c526-131">hello final step is toocreate a VM and use all hello resources created.</span></span> <span data-ttu-id="7c526-132">hello följande avsnitt i en Ansible playbook skapar en virtuell dator med namnet *myVM* och bifogar hello virtuellt nätverkskort med namnet *myNIC*.</span><span class="sxs-lookup"><span data-stu-id="7c526-132">hello following section in an Ansible playbook creates a VM named *myVM* and attaches hello virtual NIC named *myNIC*.</span></span> <span data-ttu-id="7c526-133">Ange dina egna offentliga nyckel data i hello *key_data* koppla på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7c526-133">Enter your own public key data in hello *key_data* pair as follows:</span></span>

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

## <a name="complete-ansible-playbook"></a><span data-ttu-id="7c526-134">Slutföra Ansible playbook</span><span class="sxs-lookup"><span data-stu-id="7c526-134">Complete Ansible playbook</span></span>
<span data-ttu-id="7c526-135">toobring dessa avsnitt tillsammans, skapa en Ansible playbook med namnet *azure_create_complete_vm.yml* och klistra in hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="7c526-135">toobring all these sections together, create an Ansible playbook named *azure_create_complete_vm.yml* and paste hello following contents:</span></span>

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

<span data-ttu-id="7c526-136">Ansible måste en resurs grupp toodeploy alla resurser i.</span><span class="sxs-lookup"><span data-stu-id="7c526-136">Ansible needs a resource group toodeploy all your resources into.</span></span> <span data-ttu-id="7c526-137">Skapa en resursgrupp med [az group create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="7c526-137">Create a resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="7c526-138">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="7c526-138">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="7c526-139">toocreate hello fullständig VM-miljö med Ansible, köra hello playbook på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7c526-139">toocreate hello complete VM environment with Ansible, run hello playbook as follows:</span></span>

```bash
ansible-playbook azure_create_complete_vm.yml
```

<span data-ttu-id="7c526-140">hello utdata ser ut ungefär toohello följande exempel som visar hello VM har skapats:</span><span class="sxs-lookup"><span data-stu-id="7c526-140">hello output looks similar toohello following example that shows hello VM has been successfully created:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7c526-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c526-141">Next steps</span></span>
<span data-ttu-id="7c526-142">Det här exemplet skapas en fullständig VM-miljö inklusive hello krävs virtuella nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="7c526-142">This example creates a complete VM environment including hello required virtual networking resources.</span></span> <span data-ttu-id="7c526-143">En mer direkt exempel toocreate en virtuell dator i befintliga nätverksresurser med standardalternativ, se [skapa en virtuell dator](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="7c526-143">For a more direct example toocreate a VM into existing network resources with default options, see [Create a VM](ansible-create-vm.md).</span></span>
