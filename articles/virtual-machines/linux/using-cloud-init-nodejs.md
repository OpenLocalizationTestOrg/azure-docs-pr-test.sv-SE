---
title: "Med hjälp av molnet init för att anpassa en Linux VM under Skapa i Azure | Microsoft Docs"
description: "Hur du använder molntjänster init för att anpassa en Linux VM under genereringen av med Azure CLI 1.0"
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
ms.openlocfilehash: 0b6150bca333188666935b3c9aa02c4b33690db9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation-with-the-azure-cli-10"></a><span data-ttu-id="ba43c-103">Använda molntjänster init för att anpassa en Linux VM under skapande av med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ba43c-103">Use cloud-init to customize a Linux VM during creation with the Azure CLI 1.0</span></span>
<span data-ttu-id="ba43c-104">Den här artikeln visar hur du gör ett moln init-skript för att ange värdnamnet, uppdateringspaket installeras och hanterar användarkonton.</span><span class="sxs-lookup"><span data-stu-id="ba43c-104">This article shows how to make a cloud-init script to set the hostname, update installed packages, and manage user accounts.</span></span>  <span data-ttu-id="ba43c-105">Molnet init-skript anropas under den virtuella dator skapandet Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ba43c-105">The cloud-init scripts are called during the VM creation from Azure CLI.</span></span>  <span data-ttu-id="ba43c-106">Artikeln kräver:</span><span class="sxs-lookup"><span data-stu-id="ba43c-106">The article requires:</span></span>

* <span data-ttu-id="ba43c-107">ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)).</span><span class="sxs-lookup"><span data-stu-id="ba43c-107">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="ba43c-108">[Azure CLI](../../cli-install-nodejs.md) inloggad med `azure login`.</span><span class="sxs-lookup"><span data-stu-id="ba43c-108">the [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="ba43c-109">Azure CLI *måste vara i* Azure Resource Manager-läge `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="ba43c-109">the Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="ba43c-110">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="ba43c-110">CLI versions to complete the task</span></span>
<span data-ttu-id="ba43c-111">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="ba43c-111">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="ba43c-112">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="ba43c-112">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="ba43c-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="ba43c-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="ba43c-114">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="ba43c-114">Quick Commands</span></span>
<span data-ttu-id="ba43c-115">Skapa ett moln init.txt skript som anger värdnamnet, uppdaterar alla paket och lägger till en användare med sudo Linux.</span><span class="sxs-lookup"><span data-stu-id="ba43c-115">Create a cloud-init.txt script that sets the hostname, updates all packages, and adds a sudo user to Linux.</span></span>

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
<span data-ttu-id="ba43c-116">Skapa en resursgrupp om du vill starta virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="ba43c-116">Create a resource group to launch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="ba43c-117">Skapa en Linux VM som använder molnet init konfigureras under start.</span><span class="sxs-lookup"><span data-stu-id="ba43c-117">Create a Linux VM using cloud-init to configure it during boot.</span></span>

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

## <a name="detailed-walkthrough"></a><span data-ttu-id="ba43c-118">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="ba43c-118">Detailed walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="ba43c-119">Introduktion</span><span class="sxs-lookup"><span data-stu-id="ba43c-119">Introduction</span></span>
<span data-ttu-id="ba43c-120">När du startar en ny Linux VM får du en standard Linux VM utan något anpassad eller redo för dina behov.</span><span class="sxs-lookup"><span data-stu-id="ba43c-120">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="ba43c-121">[Molnet init](https://cloudinit.readthedocs.org) är ett standardiserat sätt att mata in ett skript eller konfiguration inställningar i den Linux VM som startar för första gången.</span><span class="sxs-lookup"><span data-stu-id="ba43c-121">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way to inject a script or configuration settings into that Linux VM as it is booting up for the first time.</span></span>

<span data-ttu-id="ba43c-122">I Azure, det finns tre olika sätt att göra ändringar på en Linux VM när den distribuerats eller startas.</span><span class="sxs-lookup"><span data-stu-id="ba43c-122">On Azure, there are a three different ways to make changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="ba43c-123">Mata in skript med hjälp av molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="ba43c-123">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="ba43c-124">Mata in skript med hjälp av Azure [VMAccess-tillägget](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba43c-124">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="ba43c-125">En Azure-mall med hjälp av molnet initiering.</span><span class="sxs-lookup"><span data-stu-id="ba43c-125">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="ba43c-126">En Azure-mall med hjälp av [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba43c-126">An Azure template using [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="ba43c-127">Att mata in skript när som helst efter omstart:</span><span class="sxs-lookup"><span data-stu-id="ba43c-127">To inject scripts at any time after boot:</span></span>

* <span data-ttu-id="ba43c-128">SSH kan köra kommandon direkt</span><span class="sxs-lookup"><span data-stu-id="ba43c-128">SSH to run commands directly</span></span>
* <span data-ttu-id="ba43c-129">Mata in skript med hjälp av Azure [VMAccess-tillägget](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively eller i en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="ba43c-129">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="ba43c-130">Verktyg för konfigurationshantering som Ansible, Salt, Chef eller Puppet.</span><span class="sxs-lookup"><span data-stu-id="ba43c-130">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="ba43c-131">: VMAccess-tillägget körs ett skript som rot på samma sätt som med SSH kan.</span><span class="sxs-lookup"><span data-stu-id="ba43c-131">: VMAccess Extension executes a script as root in the same way using SSH can.</span></span>  <span data-ttu-id="ba43c-132">Med hjälp av VM-tillägget kan dock flera funktioner som Azure-erbjudanden som kan vara användbara beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="ba43c-132">However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="ba43c-133">Molnet init tillgänglighet på Virtuella Azure-Snabbregistrering avbildningen alias:</span><span class="sxs-lookup"><span data-stu-id="ba43c-133">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="ba43c-134">Alias</span><span class="sxs-lookup"><span data-stu-id="ba43c-134">Alias</span></span> | <span data-ttu-id="ba43c-135">Utgivare</span><span class="sxs-lookup"><span data-stu-id="ba43c-135">Publisher</span></span> | <span data-ttu-id="ba43c-136">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="ba43c-136">Offer</span></span> | <span data-ttu-id="ba43c-137">SKU</span><span class="sxs-lookup"><span data-stu-id="ba43c-137">SKU</span></span> | <span data-ttu-id="ba43c-138">Version</span><span class="sxs-lookup"><span data-stu-id="ba43c-138">Version</span></span> | <span data-ttu-id="ba43c-139">molnet initiering</span><span class="sxs-lookup"><span data-stu-id="ba43c-139">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="ba43c-140">CentOS</span><span class="sxs-lookup"><span data-stu-id="ba43c-140">CentOS</span></span> |<span data-ttu-id="ba43c-141">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="ba43c-141">OpenLogic</span></span> |<span data-ttu-id="ba43c-142">Centos</span><span class="sxs-lookup"><span data-stu-id="ba43c-142">Centos</span></span> |<span data-ttu-id="ba43c-143">7.2</span><span class="sxs-lookup"><span data-stu-id="ba43c-143">7.2</span></span> |<span data-ttu-id="ba43c-144">senaste</span><span class="sxs-lookup"><span data-stu-id="ba43c-144">latest</span></span> |<span data-ttu-id="ba43c-145">Nej</span><span class="sxs-lookup"><span data-stu-id="ba43c-145">no</span></span> |
| <span data-ttu-id="ba43c-146">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ba43c-146">CoreOS</span></span> |<span data-ttu-id="ba43c-147">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ba43c-147">CoreOS</span></span> |<span data-ttu-id="ba43c-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ba43c-148">CoreOS</span></span> |<span data-ttu-id="ba43c-149">Stable</span><span class="sxs-lookup"><span data-stu-id="ba43c-149">Stable</span></span> |<span data-ttu-id="ba43c-150">senaste</span><span class="sxs-lookup"><span data-stu-id="ba43c-150">latest</span></span> |<span data-ttu-id="ba43c-151">Ja</span><span class="sxs-lookup"><span data-stu-id="ba43c-151">yes</span></span> |
| <span data-ttu-id="ba43c-152">Debian</span><span class="sxs-lookup"><span data-stu-id="ba43c-152">Debian</span></span> |<span data-ttu-id="ba43c-153">credativ</span><span class="sxs-lookup"><span data-stu-id="ba43c-153">credativ</span></span> |<span data-ttu-id="ba43c-154">Debian</span><span class="sxs-lookup"><span data-stu-id="ba43c-154">Debian</span></span> |<span data-ttu-id="ba43c-155">8</span><span class="sxs-lookup"><span data-stu-id="ba43c-155">8</span></span> |<span data-ttu-id="ba43c-156">senaste</span><span class="sxs-lookup"><span data-stu-id="ba43c-156">latest</span></span> |<span data-ttu-id="ba43c-157">Nej</span><span class="sxs-lookup"><span data-stu-id="ba43c-157">no</span></span> |
| <span data-ttu-id="ba43c-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="ba43c-158">openSUSE</span></span> |<span data-ttu-id="ba43c-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="ba43c-159">SUSE</span></span> |<span data-ttu-id="ba43c-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="ba43c-160">openSUSE</span></span> |<span data-ttu-id="ba43c-161">13.2</span><span class="sxs-lookup"><span data-stu-id="ba43c-161">13.2</span></span> |<span data-ttu-id="ba43c-162">senaste</span><span class="sxs-lookup"><span data-stu-id="ba43c-162">latest</span></span> |<span data-ttu-id="ba43c-163">Nej</span><span class="sxs-lookup"><span data-stu-id="ba43c-163">no</span></span> |
| <span data-ttu-id="ba43c-164">RHEL</span><span class="sxs-lookup"><span data-stu-id="ba43c-164">RHEL</span></span> |<span data-ttu-id="ba43c-165">Redhat</span><span class="sxs-lookup"><span data-stu-id="ba43c-165">Redhat</span></span> |<span data-ttu-id="ba43c-166">RHEL</span><span class="sxs-lookup"><span data-stu-id="ba43c-166">RHEL</span></span> |<span data-ttu-id="ba43c-167">7.2</span><span class="sxs-lookup"><span data-stu-id="ba43c-167">7.2</span></span> |<span data-ttu-id="ba43c-168">senaste</span><span class="sxs-lookup"><span data-stu-id="ba43c-168">latest</span></span> |<span data-ttu-id="ba43c-169">Nej</span><span class="sxs-lookup"><span data-stu-id="ba43c-169">no</span></span> |
| <span data-ttu-id="ba43c-170">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="ba43c-170">UbuntuLTS</span></span> |<span data-ttu-id="ba43c-171">Canonical</span><span class="sxs-lookup"><span data-stu-id="ba43c-171">Canonical</span></span> |<span data-ttu-id="ba43c-172">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="ba43c-172">UbuntuServer</span></span> |<span data-ttu-id="ba43c-173">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="ba43c-173">14.04.4-LTS</span></span> |<span data-ttu-id="ba43c-174">senaste</span><span class="sxs-lookup"><span data-stu-id="ba43c-174">latest</span></span> |<span data-ttu-id="ba43c-175">Ja</span><span class="sxs-lookup"><span data-stu-id="ba43c-175">yes</span></span> |

<span data-ttu-id="ba43c-176">Microsoft arbetar med våra partner att hämta molnet initiering ingår och arbeta med bilder som skickas till Azure.</span><span class="sxs-lookup"><span data-stu-id="ba43c-176">Microsoft is working with our partners to get cloud-init included and working in the images that they provide to Azure.</span></span>

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a><span data-ttu-id="ba43c-177">Lägga till ett moln init-skript i Skapa Virtuella med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ba43c-177">Adding a cloud-init script to the VM creation with the Azure CLI</span></span>
<span data-ttu-id="ba43c-178">Om du vill starta ett moln init-skript när du skapar en virtuell dator i Azure, ange molnet init-filen med hjälp av Azure CLI `--custom-data` växla.</span><span class="sxs-lookup"><span data-stu-id="ba43c-178">To launch a cloud-init script when creating a VM in Azure, specify the cloud-init file using the Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="ba43c-179">Skapa en resursgrupp om du vill starta virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="ba43c-179">Create a resource group to launch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="ba43c-180">Skapa en Linux VM som använder molnet init konfigureras under start.</span><span class="sxs-lookup"><span data-stu-id="ba43c-180">Create a Linux VM using cloud-init to configure it during boot.</span></span>

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

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a><span data-ttu-id="ba43c-181">Skapa ett moln init-skript för att ange värdnamnet för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="ba43c-181">Creating a cloud-init script to set the hostname of a Linux VM</span></span>
<span data-ttu-id="ba43c-182">En av de enklaste och mest viktiga inställningarna för alla Linux-VM är värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="ba43c-182">One of the simplest and most important settings for any Linux VM would be the hostname.</span></span> <span data-ttu-id="ba43c-183">Vi kan enkelt ange molnet initiering med det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="ba43c-183">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="ba43c-184">Exempel molnet init skript som heter `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="ba43c-184">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```sh
#cloud-config
hostname: myservername
```

<span data-ttu-id="ba43c-185">Under den första starten av den virtuella datorn anger skriptet molnet init värdnamnet till `myservername`.</span><span class="sxs-lookup"><span data-stu-id="ba43c-185">During the initial startup of the VM, this cloud-init script sets the hostname to `myservername`.</span></span>

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

<span data-ttu-id="ba43c-186">Logga in och kontrollera värdnamnet för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ba43c-186">Login and verify the hostname of the new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a><span data-ttu-id="ba43c-187">Skapa en moln-init-skript för att uppdatera Linux</span><span class="sxs-lookup"><span data-stu-id="ba43c-187">Creating a cloud-init script to update Linux</span></span>
<span data-ttu-id="ba43c-188">Av säkerhetsskäl vill du din Ubuntu VM att uppdatera startas första gången.</span><span class="sxs-lookup"><span data-stu-id="ba43c-188">For security, you want your Ubuntu VM to update on the first boot.</span></span>  <span data-ttu-id="ba43c-189">Med hjälp av molnet init vi kan göra det med följande skript, beroende på Linux-distribution som du använder.</span><span class="sxs-lookup"><span data-stu-id="ba43c-189">Using cloud-init we can do that with the follow script, depending on the Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a><span data-ttu-id="ba43c-190">Molnet init exempelskriptet `cloud_config_apt_upgrade.txt` Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="ba43c-190">Example cloud-init script `cloud_config_apt_upgrade.txt` for the Debian Family</span></span>
```sh
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="ba43c-191">Efter Linux har startats, installerade paket uppdateras `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="ba43c-191">After Linux has booted, all the installed packages are updated via `apt-get`.</span></span>

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

<span data-ttu-id="ba43c-192">Logga in och kontrollera att alla paket har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="ba43c-192">Login and verify all packages are updated.</span></span>

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a><span data-ttu-id="ba43c-193">Skapa ett moln init-skript för att lägga till en användare till Linux</span><span class="sxs-lookup"><span data-stu-id="ba43c-193">Creating a cloud-init script to add a user to Linux</span></span>
<span data-ttu-id="ba43c-194">En av de första uppgifterna på alla nya Linux-VM är att lägga till en användare själv eller Undvik att använda `root`.</span><span class="sxs-lookup"><span data-stu-id="ba43c-194">One of the first tasks on any new Linux VM is to add a user for yourself or to avoid using `root`.</span></span> <span data-ttu-id="ba43c-195">SSH-nycklar är bästa praxis för säkerhet och användbarhet och de läggs till i `~/.ssh/authorized_keys` fil med det här molnet init-skriptet.</span><span class="sxs-lookup"><span data-stu-id="ba43c-195">SSH keys are best practice for security and for usability and they are added to the `~/.ssh/authorized_keys` file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="ba43c-196">Molnet init exempelskriptet `cloud_config_add_users.txt` Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="ba43c-196">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
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

<span data-ttu-id="ba43c-197">Efter Linux har startats, skapas och läggs till i sudo-gruppen Alla användare.</span><span class="sxs-lookup"><span data-stu-id="ba43c-197">After Linux has booted, all the listed users are created and added to the sudo group.</span></span>

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

<span data-ttu-id="ba43c-198">Logga in och kontrollera den nyligen skapade användaren.</span><span class="sxs-lookup"><span data-stu-id="ba43c-198">Login and verify the newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="ba43c-199">Resultat</span><span class="sxs-lookup"><span data-stu-id="ba43c-199">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="ba43c-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba43c-200">Next Steps</span></span>
<span data-ttu-id="ba43c-201">Initiering molnet har blivit ett sätt som standard för att ändra din Linux VM på Start.</span><span class="sxs-lookup"><span data-stu-id="ba43c-201">Cloud-init is becoming one standard way to modify your Linux VM on boot.</span></span> <span data-ttu-id="ba43c-202">Azure har också VM-tillägg som gör att du kan ändra din LinuxVM på Start eller medan den körs.</span><span class="sxs-lookup"><span data-stu-id="ba43c-202">Azure also has VM extensions, which allow you to modify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="ba43c-203">Du kan till exempel använda Azure-VMAccessExtension för att återställa SSH-eller medan den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="ba43c-203">For example, you can use the Azure VMAccessExtension to reset SSH or user information while the VM is running.</span></span> <span data-ttu-id="ba43c-204">Med molnet initiering måste startas om för att återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="ba43c-204">With cloud-init, you would need a reboot to reset the password.</span></span>

[<span data-ttu-id="ba43c-205">Om virtuella datortillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="ba43c-205">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="ba43c-206">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="ba43c-206">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

