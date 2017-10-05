---
title: "Använd Ansible för att skapa en fullständig Linux VM i Azure | Microsoft Docs"
description: "Lär dig hur du använder Ansible för att skapa och hantera en fullständig miljö med Linux virtuella datorer i Azure"
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
ms.openlocfilehash: b2fcc288b40c12a9b3f966156ee2eedf4acca313
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a><span data-ttu-id="49248-103">Skapa en miljö med fullständiga Linux virtuella datorer i Azure med Ansible</span><span class="sxs-lookup"><span data-stu-id="49248-103">Create a complete Linux virtual machine environment in Azure with Ansible</span></span>
<span data-ttu-id="49248-104">Ansible kan du automatisera distributionen och konfigurationen av resurser i din miljö.</span><span class="sxs-lookup"><span data-stu-id="49248-104">Ansible allows you to automate the deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="49248-105">Du kan använda Ansible för att hantera dina virtuella datorer (VM) i Azure, samma som någon annan resurs.</span><span class="sxs-lookup"><span data-stu-id="49248-105">You can use Ansible to manage your virtual machines (VMs) in Azure, the same as you would any other resource.</span></span> <span data-ttu-id="49248-106">Den här artikeln visar hur du skapar en komplett Linux-miljö och ge support för resurser med Ansible.</span><span class="sxs-lookup"><span data-stu-id="49248-106">This article shows you how to create a complete Linux environment and supporting resources with Ansible.</span></span> <span data-ttu-id="49248-107">Du kan också lära dig hur du [skapa en grundläggande virtuell dator med Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="49248-107">You can also learn how to [Create a basic VM with Ansible](ansible-create-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="49248-108">Krav</span><span class="sxs-lookup"><span data-stu-id="49248-108">Prerequisites</span></span>
<span data-ttu-id="49248-109">Om du vill hantera Azure-resurser med Ansible, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="49248-109">To manage Azure resources with Ansible, you need the following:</span></span>

- <span data-ttu-id="49248-110">Ansible och Azure SDK för Python-moduler som har installerats på värdsystemet.</span><span class="sxs-lookup"><span data-stu-id="49248-110">Ansible and the Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="49248-111">Installera Ansible på [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), och [SLES 12,2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="49248-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="49248-112">Konfigurera om du använder dessa autentiseringsuppgifter för Azure och Ansible.</span><span class="sxs-lookup"><span data-stu-id="49248-112">Azure credentials, and Ansible configured to use them.</span></span>
    - [<span data-ttu-id="49248-113">Skapa autentiseringsuppgifter för Azure och konfigurera Ansible</span><span class="sxs-lookup"><span data-stu-id="49248-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="49248-114">Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="49248-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="49248-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="49248-115">Run `az --version` to find the version.</span></span> 
    - <span data-ttu-id="49248-116">Om du behöver uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="49248-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="49248-117">Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="49248-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-virtual-network"></a><span data-ttu-id="49248-118">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="49248-118">Create virtual network</span></span>
<span data-ttu-id="49248-119">Följande avsnitt i en Ansible playbook skapar ett virtuellt nätverk med namnet *myVnet* i den *10.0.0.0/16* adressutrymmet:</span><span class="sxs-lookup"><span data-stu-id="49248-119">The following section in an Ansible playbook creates a virtual network named *myVnet* in the *10.0.0.0/16* address space:</span></span>

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

<span data-ttu-id="49248-120">Om du vill lägga till ett undernät, nedan skapar ett undernät med namnet *mySubnet* i den *myVnet* virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="49248-120">To add a subnet, the following section creates a subnet named *mySubnet* in the *myVnet* virtual network:</span></span>

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a><span data-ttu-id="49248-121">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="49248-121">Create public IP address</span></span>
<span data-ttu-id="49248-122">Skapa och tilldela en offentlig IP-adress till den virtuella datorn kan komma åt resurser via Internet.</span><span class="sxs-lookup"><span data-stu-id="49248-122">To access resources across the Internet, create and assign a public IP address to your VM.</span></span> <span data-ttu-id="49248-123">Följande avsnitt i en Ansible playbook skapar en offentlig IP-adress med namnet *myPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="49248-123">The following section in an Ansible playbook creates a public IP address named *myPublicIP*:</span></span>

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a><span data-ttu-id="49248-124">Skapa Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="49248-124">Create Network Security Group</span></span>
<span data-ttu-id="49248-125">Nätverkssäkerhetsgrupper styra flödet i nätverkstrafiken till och från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="49248-125">Network Security Groups control the flow of network traffic in and out of your VM.</span></span> <span data-ttu-id="49248-126">Följande avsnitt i en Ansible playbook skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* och definierar en regel för att tillåta SSH-trafik på TCP-port 22:</span><span class="sxs-lookup"><span data-stu-id="49248-126">The following section in an Ansible playbook creates a network security group named *myNetworkSecurityGroup* and defines a rule to allow SSH traffic on TCP port 22:</span></span>

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


## <a name="create-virtual-network-interface-card"></a><span data-ttu-id="49248-127">Skapa virtuella nätverkskort</span><span class="sxs-lookup"><span data-stu-id="49248-127">Create virtual network interface card</span></span>
<span data-ttu-id="49248-128">Ett virtuellt nätverkskort (NIC) ansluter den virtuella datorn till ett givet virtuellt nätverk, offentlig IP-adress och nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="49248-128">A virtual network interface card (NIC) connects your VM to a given virtual network, public IP address, and network security group.</span></span> <span data-ttu-id="49248-129">Följande avsnitt i en Ansible playbook skapar ett virtuellt nätverkskort med namnet *myNIC* anslutna till de virtuella nätverksresurser som du har skapat:</span><span class="sxs-lookup"><span data-stu-id="49248-129">The following section in an Ansible playbook creates a virtual NIC named *myNIC* connected to the virtual networking resources you have created:</span></span>

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


## <a name="create-virtual-machine"></a><span data-ttu-id="49248-130">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="49248-130">Create virtual machine</span></span>
<span data-ttu-id="49248-131">Det sista steget är att skapa en virtuell dator och använda de resurser som skapades.</span><span class="sxs-lookup"><span data-stu-id="49248-131">The final step is to create a VM and use all the resources created.</span></span> <span data-ttu-id="49248-132">Följande avsnitt i en Ansible playbook skapar en virtuell dator med namnet *myVM* och bifogar det virtuella nätverkskortet med namnet *myNIC*.</span><span class="sxs-lookup"><span data-stu-id="49248-132">The following section in an Ansible playbook creates a VM named *myVM* and attaches the virtual NIC named *myNIC*.</span></span> <span data-ttu-id="49248-133">Ange dina egna offentliga nyckel data i den *key_data* koppla på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="49248-133">Enter your own public key data in the *key_data* pair as follows:</span></span>

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

## <a name="complete-ansible-playbook"></a><span data-ttu-id="49248-134">Slutföra Ansible playbook</span><span class="sxs-lookup"><span data-stu-id="49248-134">Complete Ansible playbook</span></span>
<span data-ttu-id="49248-135">Skapa en Ansible playbook med namnet för att samordna dessa avsnitt *azure_create_complete_vm.yml* och klistra in följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="49248-135">To bring all these sections together, create an Ansible playbook named *azure_create_complete_vm.yml* and paste the following contents:</span></span>

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

<span data-ttu-id="49248-136">Ansible måste distribuera alla resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="49248-136">Ansible needs a resource group to deploy all your resources into.</span></span> <span data-ttu-id="49248-137">Skapa en resursgrupp med [az group create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="49248-137">Create a resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="49248-138">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="49248-138">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="49248-139">För att skapa fullständiga Virtuella miljön med Ansible, kör du playbook på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="49248-139">To create the complete VM environment with Ansible, run the playbook as follows:</span></span>

```bash
ansible-playbook azure_create_complete_vm.yml
```

<span data-ttu-id="49248-140">Utdata liknar följande exempel som visar den virtuella datorn har skapats:</span><span class="sxs-lookup"><span data-stu-id="49248-140">The output looks similar to the following example that shows the VM has been successfully created:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="49248-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49248-141">Next steps</span></span>
<span data-ttu-id="49248-142">Det här exemplet skapas en fullständig VM-miljö inklusive de nödvändiga resurserna för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="49248-142">This example creates a complete VM environment including the required virtual networking resources.</span></span> <span data-ttu-id="49248-143">En mer direkt exempel skapa en virtuell dator i befintliga nätverksresurser med standardalternativen finns [skapa en virtuell dator](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="49248-143">For a more direct example to create a VM into existing network resources with default options, see [Create a VM](ansible-create-vm.md).</span></span>