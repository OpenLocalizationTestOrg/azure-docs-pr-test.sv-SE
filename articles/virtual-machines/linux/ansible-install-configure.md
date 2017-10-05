---
title: "Installera och konfigurera Ansible för användning med virtuella Azure-datorer | Microsoft Docs"
description: "Lär dig hur du installerar och konfigurerar Ansible för att hantera Azure-resurser på Ubuntu och CentOS SLES"
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
ms.openlocfilehash: 52b763274437961dccfc862c8a45fbd57ea9fc4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a><span data-ttu-id="079ff-103">Installera och konfigurera Ansible för att hantera virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="079ff-103">Install and configure Ansible to manage virtual machines in Azure</span></span>
<span data-ttu-id="079ff-104">Den här artikeln beskrivs hur du installerar Ansible och Azure SDK för Python-moduler som krävs för några av de vanligaste Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="079ff-104">This article details how to install Ansible and required Azure Python SDK modules for some of the most common Linux distros.</span></span> <span data-ttu-id="079ff-105">Du kan installera Ansible på andra distributioner genom att justera installerade paket så att de passar din viss plattform.</span><span class="sxs-lookup"><span data-stu-id="079ff-105">You can install Ansible on other distros by adjusting the installed packages to fit your particular platform.</span></span> <span data-ttu-id="079ff-106">Om du vill skapa Azure-resurser på ett säkert sätt du också lära dig hur du skapar och definiera autentiseringsuppgifter för Ansible ska användas.</span><span class="sxs-lookup"><span data-stu-id="079ff-106">To create Azure resources in a secure manner, you also learn how to create and define credentials for Ansible to use.</span></span> 

<span data-ttu-id="079ff-107">Fler alternativ för installation och anvisningar för ytterligare plattformar finns i [Ansible installationsguide](https://docs.ansible.com/ansible/intro_installation.html).</span><span class="sxs-lookup"><span data-stu-id="079ff-107">For more installation options and steps for additional platforms, see the [Ansible install guide](https://docs.ansible.com/ansible/intro_installation.html).</span></span>


## <a name="install-ansible"></a><span data-ttu-id="079ff-108">Installera Ansible</span><span class="sxs-lookup"><span data-stu-id="079ff-108">Install Ansible</span></span>
<span data-ttu-id="079ff-109">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="079ff-109">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="079ff-110">I följande exempel skapas en resursgrupp med namnet *myResourceGroupAnsible* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="079ff-110">The following example creates a resource group named *myResourceGroupAnsible* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

<span data-ttu-id="079ff-111">Nu skapa en virtuell dator och installera Ansible för en av följande distributioner:</span><span class="sxs-lookup"><span data-stu-id="079ff-111">Now create a VM and install Ansible for one of the following distros:</span></span>

- [<span data-ttu-id="079ff-112">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="079ff-112">Ubuntu 16.04 LTS</span></span>](#ubuntu1604-lts)
- [<span data-ttu-id="079ff-113">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="079ff-113">CentOS 7.3</span></span>](#centos-73)
- [<span data-ttu-id="079ff-114">SLES 12,2 SP2</span><span class="sxs-lookup"><span data-stu-id="079ff-114">SLES 12.2 SP2</span></span>](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="079ff-115">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="079ff-115">Ubuntu 16.04 LTS</span></span>
<span data-ttu-id="079ff-116">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="079ff-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="079ff-117">I följande exempel skapas en virtuell dator med namnet *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="079ff-117">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="079ff-118">SSH till virtuell dator med hjälp av den `publicIpAddress` anges i utdata från den virtuella datorn Skapa-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="079ff-118">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="079ff-119">På den virtuella datorn att installera de nödvändiga paketen för Azure Python SDK-moduler och Ansible på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="079ff-119">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Azure SDKs via pip
pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via apt
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update && sudo apt-get install -y ansible
```

<span data-ttu-id="079ff-120">Nu gå vidare till [skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="079ff-120">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="centos-73"></a><span data-ttu-id="079ff-121">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="079ff-121">CentOS 7.3</span></span>
<span data-ttu-id="079ff-122">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="079ff-122">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="079ff-123">I följande exempel skapas en virtuell dator med namnet *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="079ff-123">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="079ff-124">SSH till virtuell dator med hjälp av den `publicIpAddress` anges i utdata från den virtuella datorn Skapa-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="079ff-124">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="079ff-125">På den virtuella datorn att installera de nödvändiga paketen för Azure Python SDK-moduler och Ansible på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="079ff-125">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

<span data-ttu-id="079ff-126">Nu gå vidare till [skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="079ff-126">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="sles-122-sp2"></a><span data-ttu-id="079ff-127">SLES 12,2 SP2</span><span class="sxs-lookup"><span data-stu-id="079ff-127">SLES 12.2 SP2</span></span>
<span data-ttu-id="079ff-128">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="079ff-128">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="079ff-129">I följande exempel skapas en virtuell dator med namnet *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="079ff-129">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="079ff-130">SSH till virtuell dator med hjälp av den `publicIpAddress` anges i utdata från den virtuella datorn Skapa-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="079ff-130">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="079ff-131">På den virtuella datorn att installera de nödvändiga paketen för Azure Python SDK-moduler och Ansible på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="079ff-131">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

<span data-ttu-id="079ff-132">Nu gå vidare till [skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="079ff-132">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


## <a name="create-azure-credentials"></a><span data-ttu-id="079ff-133">Skapa autentiseringsuppgifter för Azure</span><span class="sxs-lookup"><span data-stu-id="079ff-133">Create Azure credentials</span></span>
<span data-ttu-id="079ff-134">Ansible kommunicerar med Azure med ett användarnamn och lösenord eller ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="079ff-134">Ansible communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="079ff-135">En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som Ansible.</span><span class="sxs-lookup"><span data-stu-id="079ff-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Ansible.</span></span> <span data-ttu-id="079ff-136">Du styr och definiera behörigheter för vilka åtgärder som tjänstens huvudnamn kan utföra i Azure.</span><span class="sxs-lookup"><span data-stu-id="079ff-136">You control and define the permissions as to what operations the service principal can perform in Azure.</span></span> <span data-ttu-id="079ff-137">Om du vill förbättra säkerheten bara att ange ett användarnamn och lösenord, kan det här exemplet skapar en grundläggande service principal.</span><span class="sxs-lookup"><span data-stu-id="079ff-137">To improve security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="079ff-138">Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata autentiseringsuppgifterna som Ansible måste:</span><span class="sxs-lookup"><span data-stu-id="079ff-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output the credentials that Ansible needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="079ff-139">Ett exempel på utdata från föregående kommandona är följande:</span><span class="sxs-lookup"><span data-stu-id="079ff-139">An example of the output from the preceding commands is as follows:</span></span>

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

<span data-ttu-id="079ff-140">Om du vill autentisera till Azure måste du också behöva hämta ditt Azure prenumerations-ID med [az konto visa](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="079ff-140">To authenticate to Azure, you also need to obtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="079ff-141">Du kan använda utdata från dessa två kommandon i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="079ff-141">You use the output from these two commands in the next step.</span></span>


## <a name="create-ansible-credentials-file"></a><span data-ttu-id="079ff-142">Skapa Ansible autentiseringsuppgifter fil</span><span class="sxs-lookup"><span data-stu-id="079ff-142">Create Ansible credentials file</span></span>
<span data-ttu-id="079ff-143">För att tillhandahålla autentiseringsuppgifter till Ansible definiera miljövariabler eller skapa en fil med lokala autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="079ff-143">To provide credentials to Ansible, you define environment variables or create a local credentials file.</span></span> <span data-ttu-id="079ff-144">Mer information om hur du definierar Ansible autentiseringsuppgifter finns [att tillhandahålla autentiseringsuppgifter till Azure-moduler](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span><span class="sxs-lookup"><span data-stu-id="079ff-144">For more information about how to define Ansible credentials, see [Providing Credentials to Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span></span> 

<span data-ttu-id="079ff-145">En utvecklingsmiljö kan skapa en *autentiseringsuppgifter* filen för Ansible på din Virtuella värddator, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="079ff-145">For a development environment, create a *credentials* file for Ansible on your host VM as follows:</span></span>

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

<span data-ttu-id="079ff-146">Den *autentiseringsuppgifter* själva filen kombinerar prenumerations-ID med utdata för att skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="079ff-146">The *credentials* file itself combines the subscription ID with the output of creating a service principal.</span></span> <span data-ttu-id="079ff-147">Utdata från den tidigare [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) kommandot är samma ordning som behövs för *client_id*, *hemlighet*, och *klient*.</span><span class="sxs-lookup"><span data-stu-id="079ff-147">Output from the previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is the same order as needed for *client_id*, *secret*, and *tenant*.</span></span> <span data-ttu-id="079ff-148">I följande exempel *autentiseringsuppgifter* filen visar värdena matchar föregående utdata.</span><span class="sxs-lookup"><span data-stu-id="079ff-148">The following example *credentials* file shows these values matching the previous output.</span></span> <span data-ttu-id="079ff-149">Ange egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="079ff-149">Enter your own values as follows:</span></span>

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a><span data-ttu-id="079ff-150">Använda Ansible miljövariabler</span><span class="sxs-lookup"><span data-stu-id="079ff-150">Use Ansible environment variables</span></span>
<span data-ttu-id="079ff-151">Om du tänker använda verktyg som Ansible torn eller Jenkins kan du definiera miljövariabler på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="079ff-151">If you are going to use tools such as Ansible Tower or Jenkins, you can define environment variables as follows.</span></span> <span data-ttu-id="079ff-152">Dessa variabler kombinera prenumerations-ID med utdata från att skapa en tjänst huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="079ff-152">These variables combine the subscription ID with the output from creating a service principal.</span></span> <span data-ttu-id="079ff-153">Utdata från den tidigare [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) kommandot är samma ordning som behövs för *AZURE_CLIENT_ID*, *AZURE_SECRET*, och *AZURE_TENANT*.</span><span class="sxs-lookup"><span data-stu-id="079ff-153">Output from the previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is the same order as needed for *AZURE_CLIENT_ID*, *AZURE_SECRET*, and *AZURE_TENANT*.</span></span> 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a><span data-ttu-id="079ff-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="079ff-154">Next steps</span></span>
<span data-ttu-id="079ff-155">Nu har du Ansible och nödvändiga Azure Python SDK-moduler som installerats och autentiseringsuppgifter som har definierats för Ansible ska användas.</span><span class="sxs-lookup"><span data-stu-id="079ff-155">You now have Ansible and the required Azure Python SDK modules installed, and credentials defined for Ansible to use.</span></span> <span data-ttu-id="079ff-156">Lär dig hur du [skapa en virtuell dator med Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="079ff-156">Learn how to [create a VM with Ansible](ansible-create-vm.md).</span></span> <span data-ttu-id="079ff-157">Du kan också lära dig hur du [skapa en fullständig Azure-VM och stödja resurser med Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="079ff-157">You can also learn how to [create a complete Azure VM and supporting resources with Ansible](ansible-create-complete-vm.md).</span></span>