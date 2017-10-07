---
title: "Skapa en Linux-baserad virtuell dator med hjälp av Azure CLI 1.0 | Microsoft Docs"
description: "Skapa en virtuell Linux-dator i Azure med hjälp av Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
ms.assetid: facb1115-2b4e-4ef3-9905-330e42beb686
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2016
ms.author: v-livech
ms.openlocfilehash: ec1b34e4f539d2e95bb1f99fca3a6a0ec682ef51
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-linux-vm-using-the-azure-cli-10"></a><span data-ttu-id="45692-103">Skapa en virtuell Linux-dator med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="45692-103">Create a Linux VM using the Azure CLI 1.0</span></span>

<span data-ttu-id="45692-104">I den här artikeln lär du dig hur du snabbt distribuerar en virtuell Linux-dator (VM) i Azure med hjälp av kommandot `azure vm quick-create` i Azures kommandoradsgränssnitt (CLI).</span><span class="sxs-lookup"><span data-stu-id="45692-104">This article shows how to quickly deploy a Linux virtual machine (VM) on Azure by using the `azure vm quick-create` command in the Azure command-line interface (CLI).</span></span> <span data-ttu-id="45692-105">Kommandot `quick-create` distribuerar en virtuell dator inuti en grundläggande, säker infrastruktur som du kan använda som prototyp eller för att snabbt testa ett koncept.</span><span class="sxs-lookup"><span data-stu-id="45692-105">The `quick-create` command deploys a VM inside a basic, secure infrastructure that you can use to prototype or test a concept rapidly.</span></span>

> [!NOTE]
<span data-ttu-id="45692-106">Om du vill skapa en virtuell dator med hjälp av Azure CLI 2.0 läser du [Skapa en virtuell dator med Azure CLI](../windows/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45692-106">To create a VM using the Azure CLI 2.0, see [Create a VM with the Azure CLI](../windows/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="45692-107">Du kan också snabbt distribuera en virtuell Linux-dator med hjälp av [Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45692-107">You can also quickly deploy a Linux VM by using the [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="45692-108">Artikeln kräver ett [SSH offentliga och privata nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45692-108">The article requires an [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="quick-commands"></a><span data-ttu-id="45692-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="45692-109">Quick commands</span></span>

<span data-ttu-id="45692-110">I följande exempel visas hur du distribuerar en CoreOS VM och kopplar din SSH(Secure Shell)-nyckel (dina argument kan vara annorlunda):</span><span class="sxs-lookup"><span data-stu-id="45692-110">The following example shows how to deploy a CoreOS VM and attach your Secure Shell (SSH) key (your arguments might be different):</span></span>

```azurecli
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="45692-111">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="45692-111">Detailed walkthrough</span></span>

<span data-ttu-id="45692-112">Den här genomgången distribuerar en virtuell UbuntuLTS-dator, steg för steg, med förklaringar om vad varje steg gör.</span><span class="sxs-lookup"><span data-stu-id="45692-112">The following walkthrough has an UbuntuLTS VM being deployed, step by step, with explanations of what each step is doing.</span></span>

## <a name="vm-quick-create-aliases"></a><span data-ttu-id="45692-113">VM-snabbregistrering av alias</span><span class="sxs-lookup"><span data-stu-id="45692-113">VM quick-create aliases</span></span>

<span data-ttu-id="45692-114">Ett snabbt sätt att välja en distribution är att använda Azure CLI-alias som mappas till de vanligaste OS-distributionerna.</span><span class="sxs-lookup"><span data-stu-id="45692-114">A quick way to choose a distribution is to use the Azure CLI aliases mapped to the most common OS distributions.</span></span> <span data-ttu-id="45692-115">I följande tabell visas tillgängliga alias (från och med Azure CLI version 0.10).</span><span class="sxs-lookup"><span data-stu-id="45692-115">The following table lists the aliases (as of Azure CLI version 0.10).</span></span> <span data-ttu-id="45692-116">Alla distributioner som använder `quick-create` virtuella datorer som backas upp av lagring på solid state-hårddisk (SSD), som ger snabbare etablering och högpresterande åtkomst till disken som standard.</span><span class="sxs-lookup"><span data-stu-id="45692-116">All deployments that use `quick-create` default to VMs that are backed by solid-state drive (SSD) storage, which offers faster provisioning and high-performance disk access.</span></span> <span data-ttu-id="45692-117">(Dessa alias representerar en bråkdel av de tillgängliga distributionerna i Azure.</span><span class="sxs-lookup"><span data-stu-id="45692-117">(These aliases represent a tiny portion of the available distributions on Azure.</span></span> <span data-ttu-id="45692-118">Du kan hitta fler bilder på Azure Marketplace genom att [söka efter en bild i PowerShell](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [på internet](https://azure.microsoft.com/marketplace/virtual-machines/) eller [ladda upp en egen anpassad bild](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)</span><span class="sxs-lookup"><span data-stu-id="45692-118">Find more images in the Azure Marketplace by [searching for an image in PowerShell](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [on the web](https://azure.microsoft.com/marketplace/virtual-machines/), or [upload your own custom image](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)</span></span>

| <span data-ttu-id="45692-119">Alias</span><span class="sxs-lookup"><span data-stu-id="45692-119">Alias</span></span> | <span data-ttu-id="45692-120">Utgivare</span><span class="sxs-lookup"><span data-stu-id="45692-120">Publisher</span></span> | <span data-ttu-id="45692-121">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="45692-121">Offer</span></span> | <span data-ttu-id="45692-122">SKU</span><span class="sxs-lookup"><span data-stu-id="45692-122">SKU</span></span> | <span data-ttu-id="45692-123">Version</span><span class="sxs-lookup"><span data-stu-id="45692-123">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="45692-124">CentOS</span><span class="sxs-lookup"><span data-stu-id="45692-124">CentOS</span></span> |<span data-ttu-id="45692-125">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="45692-125">OpenLogic</span></span> |<span data-ttu-id="45692-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="45692-126">CentOS</span></span> |<span data-ttu-id="45692-127">7.2</span><span class="sxs-lookup"><span data-stu-id="45692-127">7.2</span></span> |<span data-ttu-id="45692-128">senaste</span><span class="sxs-lookup"><span data-stu-id="45692-128">latest</span></span> |
| <span data-ttu-id="45692-129">CoreOS</span><span class="sxs-lookup"><span data-stu-id="45692-129">CoreOS</span></span> |<span data-ttu-id="45692-130">CoreOS</span><span class="sxs-lookup"><span data-stu-id="45692-130">CoreOS</span></span> |<span data-ttu-id="45692-131">CoreOS</span><span class="sxs-lookup"><span data-stu-id="45692-131">CoreOS</span></span> |<span data-ttu-id="45692-132">Stable</span><span class="sxs-lookup"><span data-stu-id="45692-132">Stable</span></span> |<span data-ttu-id="45692-133">senaste</span><span class="sxs-lookup"><span data-stu-id="45692-133">latest</span></span> |
| <span data-ttu-id="45692-134">Debian</span><span class="sxs-lookup"><span data-stu-id="45692-134">Debian</span></span> |<span data-ttu-id="45692-135">credativ</span><span class="sxs-lookup"><span data-stu-id="45692-135">credativ</span></span> |<span data-ttu-id="45692-136">Debian</span><span class="sxs-lookup"><span data-stu-id="45692-136">Debian</span></span> |<span data-ttu-id="45692-137">8</span><span class="sxs-lookup"><span data-stu-id="45692-137">8</span></span> |<span data-ttu-id="45692-138">senaste</span><span class="sxs-lookup"><span data-stu-id="45692-138">latest</span></span> |
| <span data-ttu-id="45692-139">openSUSE</span><span class="sxs-lookup"><span data-stu-id="45692-139">openSUSE</span></span> |<span data-ttu-id="45692-140">SUSE</span><span class="sxs-lookup"><span data-stu-id="45692-140">SUSE</span></span> |<span data-ttu-id="45692-141">openSUSE</span><span class="sxs-lookup"><span data-stu-id="45692-141">openSUSE</span></span> |<span data-ttu-id="45692-142">13.2</span><span class="sxs-lookup"><span data-stu-id="45692-142">13.2</span></span> |<span data-ttu-id="45692-143">senaste</span><span class="sxs-lookup"><span data-stu-id="45692-143">latest</span></span> |
| <span data-ttu-id="45692-144">RHEL</span><span class="sxs-lookup"><span data-stu-id="45692-144">RHEL</span></span> |<span data-ttu-id="45692-145">Red Hat</span><span class="sxs-lookup"><span data-stu-id="45692-145">Red Hat</span></span> |<span data-ttu-id="45692-146">RHEL</span><span class="sxs-lookup"><span data-stu-id="45692-146">RHEL</span></span> |<span data-ttu-id="45692-147">7.2</span><span class="sxs-lookup"><span data-stu-id="45692-147">7.2</span></span> |<span data-ttu-id="45692-148">senaste</span><span class="sxs-lookup"><span data-stu-id="45692-148">latest</span></span> |
| <span data-ttu-id="45692-149">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="45692-149">UbuntuLTS</span></span> |<span data-ttu-id="45692-150">Canonical</span><span class="sxs-lookup"><span data-stu-id="45692-150">Canonical</span></span> |<span data-ttu-id="45692-151">Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="45692-151">Ubuntu Server</span></span> |<span data-ttu-id="45692-152">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="45692-152">14.04.4-LTS</span></span> |<span data-ttu-id="45692-153">senaste</span><span class="sxs-lookup"><span data-stu-id="45692-153">latest</span></span> |

<span data-ttu-id="45692-154">I de följande avsnitten används `UbuntuLTS` alias för **ImageURN**-alternativet (`-Q`) för att distribuera en Ubuntu 14.04.4 LTS Server.</span><span class="sxs-lookup"><span data-stu-id="45692-154">The following sections use the `UbuntuLTS` alias for the **ImageURN** option (`-Q`) to deploy an Ubuntu 14.04.4 LTS Server.</span></span>

<span data-ttu-id="45692-155">Det tidigare `quick-create` exemplet anropade bara `-M` flaggan för att identifiera den offentliga SSH-nyckeln att överföra vid inaktivering av SSH-lösenord, så uppmanas du ange följande argument:</span><span class="sxs-lookup"><span data-stu-id="45692-155">The previous `quick-create` example only called out the `-M` flag to identify the SSH public key to upload while disabling SSH passwords, so you are prompted for the following arguments:</span></span>

* <span data-ttu-id="45692-156">resursgruppens namn (valfri sträng är vanligtvis bra för din första Azure-resursgrupp)</span><span class="sxs-lookup"><span data-stu-id="45692-156">resource group name (any string is typically fine for your first Azure resource group)</span></span>
* <span data-ttu-id="45692-157">VM-namn</span><span class="sxs-lookup"><span data-stu-id="45692-157">VM name</span></span>
* <span data-ttu-id="45692-158">plats (`westus` eller `westeurope` är lämpliga standardvärden)</span><span class="sxs-lookup"><span data-stu-id="45692-158">location (`westus` or `westeurope` are good defaults)</span></span>
* <span data-ttu-id="45692-159">linux (så att Azure vet vilka operativsystem du vill ha)</span><span class="sxs-lookup"><span data-stu-id="45692-159">linux (to let Azure know which OS you want)</span></span>
* <span data-ttu-id="45692-160">användarnamn</span><span class="sxs-lookup"><span data-stu-id="45692-160">username</span></span>

<span data-ttu-id="45692-161">Följande exempel anger alla värden så att ingen ytterligare uppmaning krävs.</span><span class="sxs-lookup"><span data-stu-id="45692-161">The following example specifies all the values so that no further prompting is required.</span></span> <span data-ttu-id="45692-162">Så länge som du har en `~/.ssh/id_rsa.pub` som offentlig nyckelfil i ssh-rsaformat, fungerar det.</span><span class="sxs-lookup"><span data-stu-id="45692-162">So long as you have an `~/.ssh/id_rsa.pub` as a ssh-rsa format public key file, it works as is:</span></span>

```azurecli
azure vm quick-create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --admin-username myAdminUser \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --image-urn UbuntuLTS
```

<span data-ttu-id="45692-163">Resultatet bör likna följande utdata:</span><span class="sxs-lookup"><span data-stu-id="45692-163">The output should look like the following output block:</span></span>

```azurecli
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a><span data-ttu-id="45692-164">Logga in på den nya virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="45692-164">Log in to the new VM</span></span>
<span data-ttu-id="45692-165">Logga in på din virtuella dator med den offentliga IP-adressen listad i utdata.</span><span class="sxs-lookup"><span data-stu-id="45692-165">Log in to your VM by using the public IP address listed in the output.</span></span> <span data-ttu-id="45692-166">Du kan också använda det fullständigt kvalificerade domännamnet (FQDN) som är listat:</span><span class="sxs-lookup"><span data-stu-id="45692-166">You can also use the fully qualified domain name (FQDN) that's listed:</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

<span data-ttu-id="45692-167">Inloggningsprocessen bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="45692-167">The login process should look something like the following output block:</span></span>

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a><span data-ttu-id="45692-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45692-168">Next steps</span></span>
<span data-ttu-id="45692-169">`azure vm quick-create`-kommandot är ett sätt att snabbt distribuera en virtuell dator så att du kan logga in i ett bash-gränssnitt och komma igång med ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="45692-169">The `azure vm quick-create` command is the way to quickly deploy a VM so you can log in to a bash shell and get working.</span></span> <span data-ttu-id="45692-170">Men att använda `vm quick-create` ger dig inte omfattande kontroll eller gör det möjligt för dig att skapa en mer komplex miljö.</span><span class="sxs-lookup"><span data-stu-id="45692-170">However, using `vm quick-create` does not give you extensive control nor does it enable you to create a more complex environment.</span></span>  <span data-ttu-id="45692-171">Om du vill distribuera en virtuell Linux-dator som är anpassad efter din infrastruktur kan du följa vilken som helst av dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="45692-171">To deploy a Linux VM that's customized for your infrastructure, you can follow any of these articles:</span></span>

* [<span data-ttu-id="45692-172">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="45692-172">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="45692-173">Skapa en SSH-skyddad virtuell Linux-dator i Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="45692-173">Create an SSH Secured Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="45692-174">Du kan också [använda `docker-machine` Azure-drivrutinen med olika kommandon för att snabbt skapa en virtuell Linux-dator som Docker-värd](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45692-174">You can also [use the `docker-machine` Azure driver with various commands to quickly create a Linux VM as a docker host](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
