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
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation"></a><span data-ttu-id="ddad1-103">Använda molntjänster init toocustomize en Linux VM under skapande av</span><span class="sxs-lookup"><span data-stu-id="ddad1-103">Use cloud-init toocustomize a Linux VM during creation</span></span>
<span data-ttu-id="ddad1-104">Den här artikeln visar hur toomake ett moln init skriptet tooset hello värdnamn, Uppdatera installerade paket och hantera användarkonton med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="ddad1-104">This article shows you how toomake a cloud-init script tooset hello hostname, update installed packages, and manage user accounts with hello Azure CLI 2.0.</span></span> <span data-ttu-id="ddad1-105">hello molnet init skript kallas när du skapar en virtuell dator (VM) från Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ddad1-105">hello cloud-init scripts are called when you create a virtual machine (VM) from Azure CLI.</span></span> <span data-ttu-id="ddad1-106">En mer detaljerad översikt över installera program skriva konfigurationsfiler och att injicera nycklar från Key Vault finns [självstudierna](tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ddad1-106">For a more in-depth overview on installing applications, writing configuration files, and injecting keys from Key Vault, see [this tutorial](tutorial-automate-vm-deployment.md).</span></span> <span data-ttu-id="ddad1-107">Du kan också utföra dessa steg med hello [Azure CLI 1.0](using-cloud-init-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ddad1-107">You can also perform these steps with hello [Azure CLI 1.0](using-cloud-init-nodejs.md).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="ddad1-108">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="ddad1-108">Quick commands</span></span>
<span data-ttu-id="ddad1-109">Skapa ett moln init.txt skript som anger hello värdnamn, uppdaterar alla paket och lägger till en sudo användare tooLinux.</span><span class="sxs-lookup"><span data-stu-id="ddad1-109">Create a cloud-init.txt script that sets hello hostname, updates all packages, and adds a sudo user tooLinux.</span></span>

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

<span data-ttu-id="ddad1-110">Skapa en resurs grupp toolaunch virtuella datorer i med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ddad1-110">Create a resource group toolaunch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ddad1-111">hello följande exempel skapar hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ddad1-111">hello following example creates hello resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="ddad1-112">Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start med hello `--custom-data` parameter.</span><span class="sxs-lookup"><span data-stu-id="ddad1-112">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot with hello `--custom-data` parameter.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="ddad1-113">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="ddad1-113">Detailed walkthrough</span></span>
<span data-ttu-id="ddad1-114">När du startar en ny Linux VM får du en standard Linux VM utan något anpassad eller redo för dina behov.</span><span class="sxs-lookup"><span data-stu-id="ddad1-114">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="ddad1-115">[Molnet init](https://cloudinit.readthedocs.org) är ett standardiserat sätt tooinject ett skript eller konfiguration inställningar i den Linux VM som operativsystemet startas för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="ddad1-115">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way tooinject a script or configuration settings into that Linux VM as it is booting up for hello first time.</span></span>

<span data-ttu-id="ddad1-116">I Azure, det finns några olika sätt toomake ändringar på en Linux VM som den håller på att distribueras eller startas.</span><span class="sxs-lookup"><span data-stu-id="ddad1-116">On Azure, there are a few different ways toomake changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="ddad1-117">Mata in skript med hjälp av molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="ddad1-117">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="ddad1-118">Mata in skript med hjälp av hello Azure [VMAccess-tillägget](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ddad1-118">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md).</span></span>
* <span data-ttu-id="ddad1-119">En Azure-mall med hjälp av molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="ddad1-119">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="ddad1-120">En Azure-mall med hjälp av [CustomScriptExtention](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="ddad1-120">An Azure template using [CustomScriptExtention](extensions-customscript.md).</span></span>

<span data-ttu-id="ddad1-121">tooinject skript när som helst efter omstart:</span><span class="sxs-lookup"><span data-stu-id="ddad1-121">tooinject scripts at any time after boot:</span></span>

* <span data-ttu-id="ddad1-122">SSH toorun direkt kommandon</span><span class="sxs-lookup"><span data-stu-id="ddad1-122">SSH toorun commands directly</span></span>
* <span data-ttu-id="ddad1-123">Mata in skript med hjälp av hello Azure [VMAccess-tillägget](using-vmaccess-extension.md), imperatively eller i en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="ddad1-123">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="ddad1-124">Verktyg för konfigurationshantering som Ansible, Salt, Chef eller Puppet.</span><span class="sxs-lookup"><span data-stu-id="ddad1-124">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="ddad1-125">VMAccess-tillägget körs ett skript som roten i hello samma sätt med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="ddad1-125">VMAccess Extension executes a script as root in hello same way using SSH can.</span></span> <span data-ttu-id="ddad1-126">Med hjälp av hello VM-tillägget kan dock flera funktioner som Azure-erbjudanden som kan vara användbara beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="ddad1-126">However, using hello VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="ddad1-127">Molnet init tillgänglighet på Virtuella Azure-Snabbregistrering avbildningen alias:</span><span class="sxs-lookup"><span data-stu-id="ddad1-127">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="ddad1-128">Alias</span><span class="sxs-lookup"><span data-stu-id="ddad1-128">Alias</span></span> | <span data-ttu-id="ddad1-129">Utgivare</span><span class="sxs-lookup"><span data-stu-id="ddad1-129">Publisher</span></span> | <span data-ttu-id="ddad1-130">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="ddad1-130">Offer</span></span> | <span data-ttu-id="ddad1-131">SKU</span><span class="sxs-lookup"><span data-stu-id="ddad1-131">SKU</span></span> | <span data-ttu-id="ddad1-132">Version</span><span class="sxs-lookup"><span data-stu-id="ddad1-132">Version</span></span> | <span data-ttu-id="ddad1-133">molnet initiering</span><span class="sxs-lookup"><span data-stu-id="ddad1-133">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="ddad1-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="ddad1-134">CentOS</span></span> |<span data-ttu-id="ddad1-135">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="ddad1-135">OpenLogic</span></span> |<span data-ttu-id="ddad1-136">Centos</span><span class="sxs-lookup"><span data-stu-id="ddad1-136">Centos</span></span> |<span data-ttu-id="ddad1-137">7.2</span><span class="sxs-lookup"><span data-stu-id="ddad1-137">7.2</span></span> |<span data-ttu-id="ddad1-138">senaste</span><span class="sxs-lookup"><span data-stu-id="ddad1-138">latest</span></span> |<span data-ttu-id="ddad1-139">Nej</span><span class="sxs-lookup"><span data-stu-id="ddad1-139">no</span></span> |
| <span data-ttu-id="ddad1-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ddad1-140">CoreOS</span></span> |<span data-ttu-id="ddad1-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ddad1-141">CoreOS</span></span> |<span data-ttu-id="ddad1-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ddad1-142">CoreOS</span></span> |<span data-ttu-id="ddad1-143">Stable</span><span class="sxs-lookup"><span data-stu-id="ddad1-143">Stable</span></span> |<span data-ttu-id="ddad1-144">senaste</span><span class="sxs-lookup"><span data-stu-id="ddad1-144">latest</span></span> |<span data-ttu-id="ddad1-145">Ja</span><span class="sxs-lookup"><span data-stu-id="ddad1-145">yes</span></span> |
| <span data-ttu-id="ddad1-146">Debian</span><span class="sxs-lookup"><span data-stu-id="ddad1-146">Debian</span></span> |<span data-ttu-id="ddad1-147">credativ</span><span class="sxs-lookup"><span data-stu-id="ddad1-147">credativ</span></span> |<span data-ttu-id="ddad1-148">Debian</span><span class="sxs-lookup"><span data-stu-id="ddad1-148">Debian</span></span> |<span data-ttu-id="ddad1-149">8</span><span class="sxs-lookup"><span data-stu-id="ddad1-149">8</span></span> |<span data-ttu-id="ddad1-150">senaste</span><span class="sxs-lookup"><span data-stu-id="ddad1-150">latest</span></span> |<span data-ttu-id="ddad1-151">Nej</span><span class="sxs-lookup"><span data-stu-id="ddad1-151">no</span></span> |
| <span data-ttu-id="ddad1-152">openSUSE</span><span class="sxs-lookup"><span data-stu-id="ddad1-152">openSUSE</span></span> |<span data-ttu-id="ddad1-153">SUSE</span><span class="sxs-lookup"><span data-stu-id="ddad1-153">SUSE</span></span> |<span data-ttu-id="ddad1-154">openSUSE</span><span class="sxs-lookup"><span data-stu-id="ddad1-154">openSUSE</span></span> |<span data-ttu-id="ddad1-155">13.2</span><span class="sxs-lookup"><span data-stu-id="ddad1-155">13.2</span></span> |<span data-ttu-id="ddad1-156">senaste</span><span class="sxs-lookup"><span data-stu-id="ddad1-156">latest</span></span> |<span data-ttu-id="ddad1-157">Nej</span><span class="sxs-lookup"><span data-stu-id="ddad1-157">no</span></span> |
| <span data-ttu-id="ddad1-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="ddad1-158">RHEL</span></span> |<span data-ttu-id="ddad1-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="ddad1-159">Redhat</span></span> |<span data-ttu-id="ddad1-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="ddad1-160">RHEL</span></span> |<span data-ttu-id="ddad1-161">7.2</span><span class="sxs-lookup"><span data-stu-id="ddad1-161">7.2</span></span> |<span data-ttu-id="ddad1-162">senaste</span><span class="sxs-lookup"><span data-stu-id="ddad1-162">latest</span></span> |<span data-ttu-id="ddad1-163">Nej</span><span class="sxs-lookup"><span data-stu-id="ddad1-163">no</span></span> |
| <span data-ttu-id="ddad1-164">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="ddad1-164">UbuntuLTS</span></span> |<span data-ttu-id="ddad1-165">Canonical</span><span class="sxs-lookup"><span data-stu-id="ddad1-165">Canonical</span></span> |<span data-ttu-id="ddad1-166">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="ddad1-166">UbuntuServer</span></span> |<span data-ttu-id="ddad1-167">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="ddad1-167">14.04.4-LTS</span></span> |<span data-ttu-id="ddad1-168">senaste</span><span class="sxs-lookup"><span data-stu-id="ddad1-168">latest</span></span> |<span data-ttu-id="ddad1-169">Ja</span><span class="sxs-lookup"><span data-stu-id="ddad1-169">yes</span></span> |

<span data-ttu-id="ddad1-170">Vi kan arbeta med våra partners tooget moln initiering ingår och arbeta i hello bilder de ger tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ddad1-170">We are working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span>

## <a name="add-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a><span data-ttu-id="ddad1-171">Lägg till ett moln init skriptet toohello skapa en virtuell dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ddad1-171">Add a cloud-init script toohello VM creation with hello Azure CLI</span></span>
<span data-ttu-id="ddad1-172">toolaunch ett moln init-skript när du skapar en virtuell dator i Azure, ange hello molnet init-fil med hello Azure CLI `--custom-data` växla.</span><span class="sxs-lookup"><span data-stu-id="ddad1-172">toolaunch a cloud-init script when creating a VM in Azure, specify hello cloud-init file using hello Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="ddad1-173">Skapa en resurs grupp toolaunch virtuella datorer i med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ddad1-173">Create a resource group toolaunch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ddad1-174">hello följande exempel skapar hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ddad1-174">hello following example creates hello resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="ddad1-175">Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.</span><span class="sxs-lookup"><span data-stu-id="ddad1-175">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a><span data-ttu-id="ddad1-176">Skapa ett moln init skriptet tooset hello värdnamn för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="ddad1-176">Create a cloud-init script tooset hello hostname of a Linux VM</span></span>
<span data-ttu-id="ddad1-177">En av hello enklaste och mest viktiga inställningar för alla Linux-VM är hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="ddad1-177">One of hello simplest and most important settings for any Linux VM would be hello hostname.</span></span> <span data-ttu-id="ddad1-178">Vi kan enkelt ange molnet initiering med det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="ddad1-178">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="ddad1-179">Exempel molnet init skript som heter `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="ddad1-179">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```yaml
#cloud-config
hostname: myservername
```

<span data-ttu-id="ddad1-180">Under hello första start av hello VM moln init skriptet anger hello värdnamn för*myservername*.</span><span class="sxs-lookup"><span data-stu-id="ddad1-180">During hello initial startup of hello VM, this cloud-init script sets hello hostname too*myservername*.</span></span> <span data-ttu-id="ddad1-181">Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.</span><span class="sxs-lookup"><span data-stu-id="ddad1-181">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="ddad1-182">Logga in och kontrollera hello värdnamnet för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ddad1-182">Login and verify hello hostname of hello new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a><span data-ttu-id="ddad1-183">Skapa ett moln init-skript</span><span class="sxs-lookup"><span data-stu-id="ddad1-183">Create a cloud-init script</span></span>
<span data-ttu-id="ddad1-184">För säkerhet kan du din Ubuntu VM tooupdate på hello första start.</span><span class="sxs-lookup"><span data-stu-id="ddad1-184">For security, you want your Ubuntu VM tooupdate on hello first boot.</span></span> <span data-ttu-id="ddad1-185">Med hjälp av molnet init kan vi göra att med hello följer skript, beroende på hur du använder hello Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="ddad1-185">Using cloud-init we can do that with hello follow script, depending on hello Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a><span data-ttu-id="ddad1-186">Molnet init exempelskriptet `cloud_config_apt_upgrade.txt` för hello Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="ddad1-186">Example cloud-init script `cloud_config_apt_upgrade.txt` for hello Debian Family</span></span>
```yaml
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="ddad1-187">Efter Linux har startats, alla hello installerade paket uppdateras **lgh get**.</span><span class="sxs-lookup"><span data-stu-id="ddad1-187">After Linux has booted, all hello installed packages are updated via **apt-get**.</span></span> <span data-ttu-id="ddad1-188">Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.</span><span class="sxs-lookup"><span data-stu-id="ddad1-188">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="ddad1-189">Logga in och kontrollera att alla paket har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="ddad1-189">Login and verify all packages are updated.</span></span>

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

## <a name="create-a-cloud-init-script-tooadd-a-user-toolinux"></a><span data-ttu-id="ddad1-190">Skapa en moln-init skriptet tooadd tooLinux för en användare</span><span class="sxs-lookup"><span data-stu-id="ddad1-190">Create a cloud-init script tooadd a user tooLinux</span></span>
<span data-ttu-id="ddad1-191">En av hello första uppgifter på alla nya Linux-VM är tooadd en användare för dig själv eller tooavoid med *rot*.</span><span class="sxs-lookup"><span data-stu-id="ddad1-191">One of hello first tasks on any new Linux VM is tooadd a user for yourself or tooavoid using *root*.</span></span> <span data-ttu-id="ddad1-192">SSH-nycklar är bästa praxis för säkerhet och användbarhet och de läggs toohello *~/.ssh/authorized_keys* fil med det här molnet init-skriptet.</span><span class="sxs-lookup"><span data-stu-id="ddad1-192">SSH keys are best practice for security and for usability and they are added toohello *~/.ssh/authorized_keys* file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="ddad1-193">Molnet init exempelskriptet `cloud_config_add_users.txt` Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="ddad1-193">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
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

<span data-ttu-id="ddad1-194">Efter Linux har startats, är alla hello listas användare skapas och tillagda toohello sudo grupp.</span><span class="sxs-lookup"><span data-stu-id="ddad1-194">After Linux has booted, all hello listed users are created and added toohello sudo group.</span></span> <span data-ttu-id="ddad1-195">Skapa en Linux VM med [az vm skapa](/cli/azure/vm#create) med molnet init tooconfigure vid start.</span><span class="sxs-lookup"><span data-stu-id="ddad1-195">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="ddad1-196">Logga in och kontrollera hello nyligen skapade användaren.</span><span class="sxs-lookup"><span data-stu-id="ddad1-196">Login and verify hello newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="ddad1-197">Resultat</span><span class="sxs-lookup"><span data-stu-id="ddad1-197">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="ddad1-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ddad1-198">Next steps</span></span>
<span data-ttu-id="ddad1-199">Initiering molnet har blivit ett standardiserat sätt toomodify din Linux VM på Start.</span><span class="sxs-lookup"><span data-stu-id="ddad1-199">Cloud-init is becoming one standard way toomodify your Linux VM on boot.</span></span> <span data-ttu-id="ddad1-200">Azure har också VM-tillägg som gör att du toomodify din LinuxVM på Start eller medan den körs.</span><span class="sxs-lookup"><span data-stu-id="ddad1-200">Azure also has VM extensions, which allow you toomodify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="ddad1-201">Du kan till exempel använda hello Azure VMAccessExtension tooreset SSH eller användarinformation medan hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="ddad1-201">For example, you can use hello Azure VMAccessExtension tooreset SSH or user information while hello VM is running.</span></span> <span data-ttu-id="ddad1-202">Med molnet initiering behöver du en omstart tooreset hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="ddad1-202">With cloud-init, you would need a reboot tooreset hello password.</span></span>

[<span data-ttu-id="ddad1-203">Om virtuella datortillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="ddad1-203">About virtual machine extensions and features</span></span>](extensions-features.md)

[<span data-ttu-id="ddad1-204">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="ddad1-204">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md)
