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
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a><span data-ttu-id="7cfc8-103">Skapa en grundläggande virtuell dator i Azure med Ansible</span><span class="sxs-lookup"><span data-stu-id="7cfc8-103">Create a basic virtual machine in Azure with Ansible</span></span>
<span data-ttu-id="7cfc8-104">Ansible kan du tooautomate hello distributionen och konfigurationen av resurser i din miljö.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-104">Ansible allows you tooautomate hello deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="7cfc8-105">Du kan använda Ansible toomanage dina virtuella datorer (VM) i Azure, hello samma precis som alla andra resurser.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-105">You can use Ansible toomanage your virtual machines (VMs) in Azure, hello same as you would any other resource.</span></span> <span data-ttu-id="7cfc8-106">Den här artikeln beskrivs hur du toocreate en grundläggande virtuell dator med Ansible.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-106">This article shows you how toocreate a basic VM with Ansible.</span></span> <span data-ttu-id="7cfc8-107">Du kan också lära dig hur för[skapa en fullständig VM-miljö med Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="7cfc8-107">You can also learn how too[Create a complete VM environment with Ansible](ansible-create-complete-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7cfc8-108">Krav</span><span class="sxs-lookup"><span data-stu-id="7cfc8-108">Prerequisites</span></span>
<span data-ttu-id="7cfc8-109">toomanage Azure resurser med Ansible måste hello följande:</span><span class="sxs-lookup"><span data-stu-id="7cfc8-109">toomanage Azure resources with Ansible, you need hello following:</span></span>

- <span data-ttu-id="7cfc8-110">Ansible och hello Azure Python SDK-moduler som installerats på värdsystemet.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-110">Ansible and hello Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="7cfc8-111">Installera Ansible på [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), och [SLES 12,2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="7cfc8-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="7cfc8-112">Autentiseringsuppgifter för Azure och Ansible konfigurerats toouse dem.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-112">Azure credentials, and Ansible configured toouse them.</span></span>
    - [<span data-ttu-id="7cfc8-113">Skapa autentiseringsuppgifter för Azure och konfigurera Ansible</span><span class="sxs-lookup"><span data-stu-id="7cfc8-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="7cfc8-114">Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="7cfc8-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-115">Run `az --version` toofind hello version.</span></span> 
    - <span data-ttu-id="7cfc8-116">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7cfc8-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="7cfc8-117">Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-supporting-azure-resources"></a><span data-ttu-id="7cfc8-118">Skapa stöd för Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="7cfc8-118">Create supporting Azure resources</span></span>
<span data-ttu-id="7cfc8-119">I det här exemplet skapar vi en runbook som distribuerar en virtuell dator i en befintlig infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-119">In this example, we create a runbook that deploys a VM into an existing infrastructure.</span></span> <span data-ttu-id="7cfc8-120">Börja med att skapa resursgrupp med [az gruppen skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="7cfc8-120">First, create resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="7cfc8-121">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="7cfc8-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="7cfc8-122">Skapa ett virtuellt nätverk för den virtuella datorn med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="7cfc8-122">Create a virtual network for your VM with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="7cfc8-123">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och ett undernät med namnet *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="7cfc8-123">hello following example creates a virtual network named *myVnet* and a subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a><span data-ttu-id="7cfc8-124">Skapa och köra Ansible playbook</span><span class="sxs-lookup"><span data-stu-id="7cfc8-124">Create and run Ansible playbook</span></span>
<span data-ttu-id="7cfc8-125">Skapa en Ansible playbook med namnet **azure_create_vm.yml** och klistra in hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-125">Create an Ansible playbook named **azure_create_vm.yml** and paste hello following contents.</span></span> <span data-ttu-id="7cfc8-126">Det här exemplet skapar en enda virtuell dator och konfigurerar SSH-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-126">This example creates a single VM and configures SSH credentials.</span></span> <span data-ttu-id="7cfc8-127">Ange dina egna offentliga nyckel data i hello *key_data* koppla på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7cfc8-127">Enter your own public key data in hello *key_data* pair as follows:</span></span>

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

<span data-ttu-id="7cfc8-128">toocreate hello virtuell dator med Ansible, köra hello playbook på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7cfc8-128">toocreate hello VM with Ansible, run hello playbook as follows:</span></span>

```bash
ansible-playbook azure_create_vm.yml
```

<span data-ttu-id="7cfc8-129">hello utdata ser ut ungefär toohello följande exempel som visar hello VM har skapats:</span><span class="sxs-lookup"><span data-stu-id="7cfc8-129">hello output looks similar toohello following example that shows hello VM has been successfully created:</span></span>

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a><span data-ttu-id="7cfc8-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cfc8-130">Next steps</span></span>
<span data-ttu-id="7cfc8-131">Det här exemplet skapar en virtuell dator i en befintlig resursgrupp och med ett virtuellt nätverk som redan har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="7cfc8-131">This example creates a VM in an existing resource group and with a virtual network already deployed.</span></span> <span data-ttu-id="7cfc8-132">För en mer ingående exempel på hur toouse Ansible toocreate stödjande resurser, till exempel ett virtuellt nätverk och Nätverkssäkerhetsgruppen regler, se [skapa en fullständig VM-miljö med Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="7cfc8-132">For a more detailed example on how toouse Ansible toocreate supporting resources such as a virtual network and Network Security Group rules, see [Create a complete VM environment with Ansible](ansible-create-complete-vm.md).</span></span>
