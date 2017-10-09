---
title: "aaaUsing moln init toocustomize en Linux VM när du skapar i Azure | Microsoft Docs"
description: Hur toouse moln init toocustomize en Linux VM under genereringen av med hello Azure CLI 1.0
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: b9f480bd04029956d0593bbef931795733cbc2f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation-with-hello-azure-cli-10"></a><span data-ttu-id="01648-103">Använda molnet init toocustomize en Linux VM under genereringen av med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="01648-103">Use cloud-init toocustomize a Linux VM during creation with hello Azure CLI 1.0</span></span>
<span data-ttu-id="01648-104">Den här artikeln visar hur toomake ett moln init skriptet tooset hello värdnamn, Uppdatera installerade paket och hantera användarkonton.</span><span class="sxs-lookup"><span data-stu-id="01648-104">This article shows how toomake a cloud-init script tooset hello hostname, update installed packages, and manage user accounts.</span></span>  <span data-ttu-id="01648-105">hello molnet init skript anropas under hello skapa en virtuell dator från Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="01648-105">hello cloud-init scripts are called during hello VM creation from Azure CLI.</span></span>  <span data-ttu-id="01648-106">hello artikel kräver:</span><span class="sxs-lookup"><span data-stu-id="01648-106">hello article requires:</span></span>

* <span data-ttu-id="01648-107">ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)).</span><span class="sxs-lookup"><span data-stu-id="01648-107">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="01648-108">Hej [Azure CLI](../../cli-install-nodejs.md) loggat in med `azure login`.</span><span class="sxs-lookup"><span data-stu-id="01648-108">hello [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="01648-109">hello Azure CLI *måste vara i* läget Azure Resource Manager `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="01648-109">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="01648-110">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="01648-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="01648-111">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="01648-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="01648-112">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="01648-112">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="01648-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="01648-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="01648-114">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="01648-114">Quick Commands</span></span>
<span data-ttu-id="01648-115">Skapa ett moln init.txt skript som anger hello värdnamn, uppdaterar alla paket och lägger till en sudo användare tooLinux.</span><span class="sxs-lookup"><span data-stu-id="01648-115">Create a cloud-init.txt script that sets hello hostname, updates all packages, and adds a sudo user tooLinux.</span></span>

```sh
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
<span data-ttu-id="01648-116">Skapa en resurs grupp toolaunch virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="01648-116">Create a resource group toolaunch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="01648-117">Skapa en Linux VM som använder molnet init tooconfigure vid start.</span><span class="sxs-lookup"><span data-stu-id="01648-117">Create a Linux VM using cloud-init tooconfigure it during boot.</span></span>

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="01648-118">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="01648-118">Detailed walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="01648-119">Introduktion</span><span class="sxs-lookup"><span data-stu-id="01648-119">Introduction</span></span>
<span data-ttu-id="01648-120">När du startar en ny Linux VM får du en standard Linux VM utan något anpassad eller redo för dina behov.</span><span class="sxs-lookup"><span data-stu-id="01648-120">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="01648-121">[Molnet init](https://cloudinit.readthedocs.org) är ett standardiserat sätt tooinject ett skript eller konfiguration inställningar i den Linux VM som operativsystemet startas för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="01648-121">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way tooinject a script or configuration settings into that Linux VM as it is booting up for hello first time.</span></span>

<span data-ttu-id="01648-122">I Azure, det finns tre olika sätt toomake ändringar på en Linux VM när den distribuerats eller startas.</span><span class="sxs-lookup"><span data-stu-id="01648-122">On Azure, there are a three different ways toomake changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="01648-123">Mata in skript med hjälp av molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="01648-123">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="01648-124">Mata in skript med hjälp av hello Azure [VMAccess-tillägget](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01648-124">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="01648-125">En Azure-mall med hjälp av molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="01648-125">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="01648-126">En Azure-mall med hjälp av [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01648-126">An Azure template using [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="01648-127">tooinject skript när som helst efter omstart:</span><span class="sxs-lookup"><span data-stu-id="01648-127">tooinject scripts at any time after boot:</span></span>

* <span data-ttu-id="01648-128">SSH toorun direkt kommandon</span><span class="sxs-lookup"><span data-stu-id="01648-128">SSH toorun commands directly</span></span>
* <span data-ttu-id="01648-129">Mata in skript med hjälp av hello Azure [VMAccess-tillägget](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively eller i en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="01648-129">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="01648-130">Verktyg för konfigurationshantering som Ansible, Salt, Chef eller Puppet.</span><span class="sxs-lookup"><span data-stu-id="01648-130">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="01648-131">: VMAccess-tillägget körs ett skript som roten i hello samma sätt med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="01648-131">: VMAccess Extension executes a script as root in hello same way using SSH can.</span></span>  <span data-ttu-id="01648-132">Med hjälp av hello VM-tillägget kan dock flera funktioner som Azure-erbjudanden som kan vara användbara beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="01648-132">However, using hello VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="01648-133">Molnet init tillgänglighet på Virtuella Azure-Snabbregistrering avbildningen alias:</span><span class="sxs-lookup"><span data-stu-id="01648-133">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="01648-134">Alias</span><span class="sxs-lookup"><span data-stu-id="01648-134">Alias</span></span> | <span data-ttu-id="01648-135">Utgivare</span><span class="sxs-lookup"><span data-stu-id="01648-135">Publisher</span></span> | <span data-ttu-id="01648-136">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="01648-136">Offer</span></span> | <span data-ttu-id="01648-137">SKU</span><span class="sxs-lookup"><span data-stu-id="01648-137">SKU</span></span> | <span data-ttu-id="01648-138">Version</span><span class="sxs-lookup"><span data-stu-id="01648-138">Version</span></span> | <span data-ttu-id="01648-139">molnet initiering</span><span class="sxs-lookup"><span data-stu-id="01648-139">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="01648-140">CentOS</span><span class="sxs-lookup"><span data-stu-id="01648-140">CentOS</span></span> |<span data-ttu-id="01648-141">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="01648-141">OpenLogic</span></span> |<span data-ttu-id="01648-142">Centos</span><span class="sxs-lookup"><span data-stu-id="01648-142">Centos</span></span> |<span data-ttu-id="01648-143">7.2</span><span class="sxs-lookup"><span data-stu-id="01648-143">7.2</span></span> |<span data-ttu-id="01648-144">senaste</span><span class="sxs-lookup"><span data-stu-id="01648-144">latest</span></span> |<span data-ttu-id="01648-145">Nej</span><span class="sxs-lookup"><span data-stu-id="01648-145">no</span></span> |
| <span data-ttu-id="01648-146">CoreOS</span><span class="sxs-lookup"><span data-stu-id="01648-146">CoreOS</span></span> |<span data-ttu-id="01648-147">CoreOS</span><span class="sxs-lookup"><span data-stu-id="01648-147">CoreOS</span></span> |<span data-ttu-id="01648-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="01648-148">CoreOS</span></span> |<span data-ttu-id="01648-149">Stable</span><span class="sxs-lookup"><span data-stu-id="01648-149">Stable</span></span> |<span data-ttu-id="01648-150">senaste</span><span class="sxs-lookup"><span data-stu-id="01648-150">latest</span></span> |<span data-ttu-id="01648-151">Ja</span><span class="sxs-lookup"><span data-stu-id="01648-151">yes</span></span> |
| <span data-ttu-id="01648-152">Debian</span><span class="sxs-lookup"><span data-stu-id="01648-152">Debian</span></span> |<span data-ttu-id="01648-153">credativ</span><span class="sxs-lookup"><span data-stu-id="01648-153">credativ</span></span> |<span data-ttu-id="01648-154">Debian</span><span class="sxs-lookup"><span data-stu-id="01648-154">Debian</span></span> |<span data-ttu-id="01648-155">8</span><span class="sxs-lookup"><span data-stu-id="01648-155">8</span></span> |<span data-ttu-id="01648-156">senaste</span><span class="sxs-lookup"><span data-stu-id="01648-156">latest</span></span> |<span data-ttu-id="01648-157">Nej</span><span class="sxs-lookup"><span data-stu-id="01648-157">no</span></span> |
| <span data-ttu-id="01648-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="01648-158">openSUSE</span></span> |<span data-ttu-id="01648-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="01648-159">SUSE</span></span> |<span data-ttu-id="01648-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="01648-160">openSUSE</span></span> |<span data-ttu-id="01648-161">13.2</span><span class="sxs-lookup"><span data-stu-id="01648-161">13.2</span></span> |<span data-ttu-id="01648-162">senaste</span><span class="sxs-lookup"><span data-stu-id="01648-162">latest</span></span> |<span data-ttu-id="01648-163">Nej</span><span class="sxs-lookup"><span data-stu-id="01648-163">no</span></span> |
| <span data-ttu-id="01648-164">RHEL</span><span class="sxs-lookup"><span data-stu-id="01648-164">RHEL</span></span> |<span data-ttu-id="01648-165">Redhat</span><span class="sxs-lookup"><span data-stu-id="01648-165">Redhat</span></span> |<span data-ttu-id="01648-166">RHEL</span><span class="sxs-lookup"><span data-stu-id="01648-166">RHEL</span></span> |<span data-ttu-id="01648-167">7.2</span><span class="sxs-lookup"><span data-stu-id="01648-167">7.2</span></span> |<span data-ttu-id="01648-168">senaste</span><span class="sxs-lookup"><span data-stu-id="01648-168">latest</span></span> |<span data-ttu-id="01648-169">Nej</span><span class="sxs-lookup"><span data-stu-id="01648-169">no</span></span> |
| <span data-ttu-id="01648-170">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="01648-170">UbuntuLTS</span></span> |<span data-ttu-id="01648-171">Canonical</span><span class="sxs-lookup"><span data-stu-id="01648-171">Canonical</span></span> |<span data-ttu-id="01648-172">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="01648-172">UbuntuServer</span></span> |<span data-ttu-id="01648-173">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="01648-173">14.04.4-LTS</span></span> |<span data-ttu-id="01648-174">senaste</span><span class="sxs-lookup"><span data-stu-id="01648-174">latest</span></span> |<span data-ttu-id="01648-175">Ja</span><span class="sxs-lookup"><span data-stu-id="01648-175">yes</span></span> |

<span data-ttu-id="01648-176">Microsoft kan arbeta med våra partners tooget moln initiering ingår och arbeta i hello bilder de ger tooAzure.</span><span class="sxs-lookup"><span data-stu-id="01648-176">Microsoft is working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span>

## <a name="adding-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a><span data-ttu-id="01648-177">Lägga till ett moln init skriptet toohello skapa en virtuell dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="01648-177">Adding a cloud-init script toohello VM creation with hello Azure CLI</span></span>
<span data-ttu-id="01648-178">toolaunch ett moln init-skript när du skapar en virtuell dator i Azure, ange hello molnet init-fil med hello Azure CLI `--custom-data` växla.</span><span class="sxs-lookup"><span data-stu-id="01648-178">toolaunch a cloud-init script when creating a VM in Azure, specify hello cloud-init file using hello Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="01648-179">Skapa en resurs grupp toolaunch virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="01648-179">Create a resource group toolaunch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="01648-180">Skapa en Linux VM som använder molnet init tooconfigure vid start.</span><span class="sxs-lookup"><span data-stu-id="01648-180">Create a Linux VM using cloud-init tooconfigure it during boot.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a><span data-ttu-id="01648-181">Skapa ett moln init skriptet tooset hello värdnamn för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="01648-181">Creating a cloud-init script tooset hello hostname of a Linux VM</span></span>
<span data-ttu-id="01648-182">En av hello enklaste och mest viktiga inställningar för alla Linux-VM är hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="01648-182">One of hello simplest and most important settings for any Linux VM would be hello hostname.</span></span> <span data-ttu-id="01648-183">Vi kan enkelt ange molnet initiering med det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="01648-183">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="01648-184">Exempel molnet init skript som heter `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="01648-184">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```sh
#cloud-config
hostname: myservername
```

<span data-ttu-id="01648-185">Under hello första start av hello VM moln init skriptet anger hello värdnamn för`myservername`.</span><span class="sxs-lookup"><span data-stu-id="01648-185">During hello initial startup of hello VM, this cloud-init script sets hello hostname too`myservername`.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

<span data-ttu-id="01648-186">Logga in och kontrollera hello värdnamnet för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="01648-186">Login and verify hello hostname of hello new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-tooupdate-linux"></a><span data-ttu-id="01648-187">Skapa en moln-init skriptet tooupdate Linux</span><span class="sxs-lookup"><span data-stu-id="01648-187">Creating a cloud-init script tooupdate Linux</span></span>
<span data-ttu-id="01648-188">För säkerhet kan du din Ubuntu VM tooupdate på hello första start.</span><span class="sxs-lookup"><span data-stu-id="01648-188">For security, you want your Ubuntu VM tooupdate on hello first boot.</span></span>  <span data-ttu-id="01648-189">Med hjälp av molnet init kan vi göra att med hello följer skript, beroende på hur du använder hello Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="01648-189">Using cloud-init we can do that with hello follow script, depending on hello Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a><span data-ttu-id="01648-190">Molnet init exempelskriptet `cloud_config_apt_upgrade.txt` för hello Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="01648-190">Example cloud-init script `cloud_config_apt_upgrade.txt` for hello Debian Family</span></span>
```sh
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="01648-191">Efter Linux har startats, alla hello installerade paket uppdateras `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="01648-191">After Linux has booted, all hello installed packages are updated via `apt-get`.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="01648-192">Logga in och kontrollera att alla paket har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="01648-192">Login and verify all packages are updated.</span></span>

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

## <a name="creating-a-cloud-init-script-tooadd-a-user-toolinux"></a><span data-ttu-id="01648-193">Skapa en moln-init skriptet tooadd tooLinux för en användare</span><span class="sxs-lookup"><span data-stu-id="01648-193">Creating a cloud-init script tooadd a user tooLinux</span></span>
<span data-ttu-id="01648-194">En av hello första uppgifter på alla nya Linux-VM är tooadd en användare för dig själv eller tooavoid med `root`.</span><span class="sxs-lookup"><span data-stu-id="01648-194">One of hello first tasks on any new Linux VM is tooadd a user for yourself or tooavoid using `root`.</span></span> <span data-ttu-id="01648-195">SSH-nycklar är bästa praxis för säkerhet och användbarhet och de läggs toohello `~/.ssh/authorized_keys` fil med det här molnet init-skriptet.</span><span class="sxs-lookup"><span data-stu-id="01648-195">SSH keys are best practice for security and for usability and they are added toohello `~/.ssh/authorized_keys` file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="01648-196">Molnet init exempelskriptet `cloud_config_add_users.txt` Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="01648-196">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

<span data-ttu-id="01648-197">Efter Linux har startats, är alla hello listas användare skapas och tillagda toohello sudo grupp.</span><span class="sxs-lookup"><span data-stu-id="01648-197">After Linux has booted, all hello listed users are created and added toohello sudo group.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="01648-198">Logga in och kontrollera hello nyligen skapade användaren.</span><span class="sxs-lookup"><span data-stu-id="01648-198">Login and verify hello newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="01648-199">Resultat</span><span class="sxs-lookup"><span data-stu-id="01648-199">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="01648-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="01648-200">Next Steps</span></span>
<span data-ttu-id="01648-201">Initiering molnet har blivit ett standardiserat sätt toomodify din Linux VM på Start.</span><span class="sxs-lookup"><span data-stu-id="01648-201">Cloud-init is becoming one standard way toomodify your Linux VM on boot.</span></span> <span data-ttu-id="01648-202">Azure har också VM-tillägg som gör att du toomodify din LinuxVM på Start eller medan den körs.</span><span class="sxs-lookup"><span data-stu-id="01648-202">Azure also has VM extensions, which allow you toomodify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="01648-203">Du kan till exempel använda hello Azure VMAccessExtension tooreset SSH eller användarinformation medan hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="01648-203">For example, you can use hello Azure VMAccessExtension tooreset SSH or user information while hello VM is running.</span></span> <span data-ttu-id="01648-204">Med molnet initiering behöver du en omstart tooreset hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="01648-204">With cloud-init, you would need a reboot tooreset hello password.</span></span>

[<span data-ttu-id="01648-205">Om virtuella datortillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="01648-205">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="01648-206">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="01648-206">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

