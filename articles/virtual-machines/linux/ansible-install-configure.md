---
title: "aaaInstall och konfigurera Ansible för användning med virtuella Azure-datorer | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera Ansible för att hantera Azure-resurser på Ubuntu och CentOS SLES"
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
ms.openlocfilehash: b33d1893909b6134a5474617c9af2d6e4f627c05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-ansible-toomanage-virtual-machines-in-azure"></a><span data-ttu-id="74a2f-103">Installera och konfigurera Ansible toomanage virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="74a2f-103">Install and configure Ansible toomanage virtual machines in Azure</span></span>
<span data-ttu-id="74a2f-104">Den här artikeln beskrivs hur hello tooinstall Ansible och Azure SDK för Python-moduler som krävs för några av de vanligaste Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="74a2f-104">This article details how tooinstall Ansible and required Azure Python SDK modules for some of hello most common Linux distros.</span></span> <span data-ttu-id="74a2f-105">Du kan installera Ansible på andra distributioner genom att justera hello installerade paket toofit en viss plattform.</span><span class="sxs-lookup"><span data-stu-id="74a2f-105">You can install Ansible on other distros by adjusting hello installed packages toofit your particular platform.</span></span> <span data-ttu-id="74a2f-106">toocreate Azure resurser på ett säkert sätt du också lära dig hur toocreate och definiera autentiseringsuppgifter för Ansible toouse.</span><span class="sxs-lookup"><span data-stu-id="74a2f-106">toocreate Azure resources in a secure manner, you also learn how toocreate and define credentials for Ansible toouse.</span></span> 

<span data-ttu-id="74a2f-107">Flera installationsalternativ och anvisningar för ytterligare plattformar finns hello [Ansible installationsguide](https://docs.ansible.com/ansible/intro_installation.html).</span><span class="sxs-lookup"><span data-stu-id="74a2f-107">For more installation options and steps for additional platforms, see hello [Ansible install guide](https://docs.ansible.com/ansible/intro_installation.html).</span></span>


## <a name="install-ansible"></a><span data-ttu-id="74a2f-108">Installera Ansible</span><span class="sxs-lookup"><span data-stu-id="74a2f-108">Install Ansible</span></span>
<span data-ttu-id="74a2f-109">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="74a2f-109">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="74a2f-110">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAnsible* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="74a2f-110">hello following example creates a resource group named *myResourceGroupAnsible* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

<span data-ttu-id="74a2f-111">Nu skapa en virtuell dator och installera Ansible för en hello följande distributioner:</span><span class="sxs-lookup"><span data-stu-id="74a2f-111">Now create a VM and install Ansible for one of hello following distros:</span></span>

- [<span data-ttu-id="74a2f-112">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="74a2f-112">Ubuntu 16.04 LTS</span></span>](#ubuntu1604-lts)
- [<span data-ttu-id="74a2f-113">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="74a2f-113">CentOS 7.3</span></span>](#centos-73)
- [<span data-ttu-id="74a2f-114">SLES 12,2 SP2</span><span class="sxs-lookup"><span data-stu-id="74a2f-114">SLES 12.2 SP2</span></span>](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="74a2f-115">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="74a2f-115">Ubuntu 16.04 LTS</span></span>
<span data-ttu-id="74a2f-116">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="74a2f-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="74a2f-117">hello följande exempel skapas en virtuell dator med namnet *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="74a2f-117">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="74a2f-118">SSH tooyour VM med hello `publicIpAddress` anges i hello utdata från hello VM Skapa-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="74a2f-118">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="74a2f-119">På den virtuella datorn krävs hello paket installeras för hello Azure Python SDK-moduler och Ansible på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="74a2f-119">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

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

<span data-ttu-id="74a2f-120">Gå nu vidare för[skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="74a2f-120">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="centos-73"></a><span data-ttu-id="74a2f-121">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="74a2f-121">CentOS 7.3</span></span>
<span data-ttu-id="74a2f-122">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="74a2f-122">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="74a2f-123">hello följande exempel skapas en virtuell dator med namnet *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="74a2f-123">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="74a2f-124">SSH tooyour VM med hello `publicIpAddress` anges i hello utdata från hello VM Skapa-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="74a2f-124">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="74a2f-125">På den virtuella datorn krävs hello paket installeras för hello Azure Python SDK-moduler och Ansible på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="74a2f-125">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

<span data-ttu-id="74a2f-126">Gå nu vidare för[skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="74a2f-126">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="sles-122-sp2"></a><span data-ttu-id="74a2f-127">SLES 12,2 SP2</span><span class="sxs-lookup"><span data-stu-id="74a2f-127">SLES 12.2 SP2</span></span>
<span data-ttu-id="74a2f-128">Skapa en virtuell dator med [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="74a2f-128">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="74a2f-129">hello följande exempel skapas en virtuell dator med namnet *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="74a2f-129">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="74a2f-130">SSH tooyour VM med hello `publicIpAddress` anges i hello utdata från hello VM Skapa-åtgärd:</span><span class="sxs-lookup"><span data-stu-id="74a2f-130">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="74a2f-131">På den virtuella datorn krävs hello paket installeras för hello Azure Python SDK-moduler och Ansible på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="74a2f-131">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

<span data-ttu-id="74a2f-132">Gå nu vidare för[skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="74a2f-132">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


## <a name="create-azure-credentials"></a><span data-ttu-id="74a2f-133">Skapa autentiseringsuppgifter för Azure</span><span class="sxs-lookup"><span data-stu-id="74a2f-133">Create Azure credentials</span></span>
<span data-ttu-id="74a2f-134">Ansible kommunicerar med Azure med ett användarnamn och lösenord eller ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="74a2f-134">Ansible communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="74a2f-135">En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som Ansible.</span><span class="sxs-lookup"><span data-stu-id="74a2f-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Ansible.</span></span> <span data-ttu-id="74a2f-136">Du styr och definiera hello behörigheter toowhat operations hello tjänstens huvudnamn kan utföra i Azure.</span><span class="sxs-lookup"><span data-stu-id="74a2f-136">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span> <span data-ttu-id="74a2f-137">tooimprove säkerhet över bara att ange ett användarnamn och lösenord, det här exemplet skapar en grundläggande service principal.</span><span class="sxs-lookup"><span data-stu-id="74a2f-137">tooimprove security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="74a2f-138">Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata hello autentiseringsuppgifter som Ansible måste:</span><span class="sxs-lookup"><span data-stu-id="74a2f-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that Ansible needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="74a2f-139">Ett exempel på hello utdata från hello föregående kommandon är följande:</span><span class="sxs-lookup"><span data-stu-id="74a2f-139">An example of hello output from hello preceding commands is as follows:</span></span>

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

<span data-ttu-id="74a2f-140">tooauthenticate tooAzure du måste också tooobtain din Azure-prenumeration med ID med [az konto visa](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="74a2f-140">tooauthenticate tooAzure, you also need tooobtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="74a2f-141">Du kan använda hello utdata från dessa två kommandon i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="74a2f-141">You use hello output from these two commands in hello next step.</span></span>


## <a name="create-ansible-credentials-file"></a><span data-ttu-id="74a2f-142">Skapa Ansible autentiseringsuppgifter fil</span><span class="sxs-lookup"><span data-stu-id="74a2f-142">Create Ansible credentials file</span></span>
<span data-ttu-id="74a2f-143">tooprovide autentiseringsuppgifter tooAnsible kan du definiera miljövariabler eller skapa en fil med lokala autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="74a2f-143">tooprovide credentials tooAnsible, you define environment variables or create a local credentials file.</span></span> <span data-ttu-id="74a2f-144">Mer information om hur toodefine Ansible autentiseringsuppgifter finns [att tillhandahålla autentiseringsuppgifter tooAzure moduler](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span><span class="sxs-lookup"><span data-stu-id="74a2f-144">For more information about how toodefine Ansible credentials, see [Providing Credentials tooAzure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span></span> 

<span data-ttu-id="74a2f-145">En utvecklingsmiljö kan skapa en *autentiseringsuppgifter* filen för Ansible på din Virtuella värddator, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="74a2f-145">For a development environment, create a *credentials* file for Ansible on your host VM as follows:</span></span>

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

<span data-ttu-id="74a2f-146">Hej *autentiseringsuppgifter* själva filen kombinerar hello prenumerations-ID med hello utdata för att skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="74a2f-146">hello *credentials* file itself combines hello subscription ID with hello output of creating a service principal.</span></span> <span data-ttu-id="74a2f-147">Utdata från hello tidigare [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) kommandot är hello samma ordning som behövs för *client_id*, *hemlighet*, och *klient* .</span><span class="sxs-lookup"><span data-stu-id="74a2f-147">Output from hello previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is hello same order as needed for *client_id*, *secret*, and *tenant*.</span></span> <span data-ttu-id="74a2f-148">Hej följande exempel *autentiseringsuppgifter* filen visar värdena matchar hello tidigare utdata.</span><span class="sxs-lookup"><span data-stu-id="74a2f-148">hello following example *credentials* file shows these values matching hello previous output.</span></span> <span data-ttu-id="74a2f-149">Ange egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="74a2f-149">Enter your own values as follows:</span></span>

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a><span data-ttu-id="74a2f-150">Använda Ansible miljövariabler</span><span class="sxs-lookup"><span data-stu-id="74a2f-150">Use Ansible environment variables</span></span>
<span data-ttu-id="74a2f-151">Om du ska toouse verktyg som Ansible torn eller Jenkins, kan du definiera miljövariabler på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="74a2f-151">If you are going toouse tools such as Ansible Tower or Jenkins, you can define environment variables as follows.</span></span> <span data-ttu-id="74a2f-152">Dessa variabler kombinera hello prenumerations-ID med hello utdata från att skapa en tjänst huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="74a2f-152">These variables combine hello subscription ID with hello output from creating a service principal.</span></span> <span data-ttu-id="74a2f-153">Utdata från hello tidigare [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) kommandot är hello samma ordning som behövs för *AZURE_CLIENT_ID*, *AZURE_SECRET*, och *AZURE_ KLIENT*.</span><span class="sxs-lookup"><span data-stu-id="74a2f-153">Output from hello previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is hello same order as needed for *AZURE_CLIENT_ID*, *AZURE_SECRET*, and *AZURE_TENANT*.</span></span> 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a><span data-ttu-id="74a2f-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74a2f-154">Next steps</span></span>
<span data-ttu-id="74a2f-155">Nu har du Ansible och hello Azure Python SDK moduler som har installerats och autentiseringsuppgifter som har definierats för Ansible toouse som krävs.</span><span class="sxs-lookup"><span data-stu-id="74a2f-155">You now have Ansible and hello required Azure Python SDK modules installed, and credentials defined for Ansible toouse.</span></span> <span data-ttu-id="74a2f-156">Lär dig hur för[skapa en virtuell dator med Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="74a2f-156">Learn how too[create a VM with Ansible](ansible-create-vm.md).</span></span> <span data-ttu-id="74a2f-157">Du kan också lära dig hur för[skapa en fullständig Azure-VM och stödja resurser med Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="74a2f-157">You can also learn how too[create a complete Azure VM and supporting resources with Ansible](ansible-create-complete-vm.md).</span></span>
