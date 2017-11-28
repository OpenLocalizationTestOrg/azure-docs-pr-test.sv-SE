---
title: aaaJoin RedHat Linux VM-tooan Azure Active Directory DS | Microsoft Docs
description: Hur toojoin en befintlig RedHat Enterprise Linux 7 VM tooan Azure Active Directory Domain Service.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: f3ba3c764e253191753f1cc5fc8c3b85c53818af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-redhat-linux-vm-tooan-azure-active-directory-domain-service"></a><span data-ttu-id="80d45-103">Ansluta till en RedHat Linux VM tooan Azure Active Directory Domain Service</span><span class="sxs-lookup"><span data-stu-id="80d45-103">Join a RedHat Linux VM tooan Azure Active Directory Domain Service</span></span>

<span data-ttu-id="80d45-104">Den här artikeln visar hur toojoin en Azure Active Directory Domain Services (AADDS) med Red Hat Enterprise Linux (RHEL) 7 virtuella tooan hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="80d45-104">This article shows you how toojoin a Red Hat Enterprise Linux (RHEL) 7 virtual machine tooan Azure Active Directory Domain Services (AADDS) managed domain.</span></span>  <span data-ttu-id="80d45-105">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="80d45-105">hello requirements are:</span></span>

- [<span data-ttu-id="80d45-106">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="80d45-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)

- [<span data-ttu-id="80d45-107">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="80d45-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

- [<span data-ttu-id="80d45-108">ett Azure Active Directory Domain Services DC</span><span class="sxs-lookup"><span data-stu-id="80d45-108">an Azure Active Directory Domain Services DC</span></span>](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="80d45-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="80d45-109">Quick Commands</span></span>

<span data-ttu-id="80d45-110">_Ersätt alla exempel med dina egna inställningar._</span><span class="sxs-lookup"><span data-stu-id="80d45-110">_Replace any examples with your own settings._</span></span>

### <a name="switch-hello-azure-cli-tooclassic-deployment-mode"></a><span data-ttu-id="80d45-111">Växla hello azure cli tooclassic distribution läge</span><span class="sxs-lookup"><span data-stu-id="80d45-111">Switch hello azure-cli tooclassic deployment mode</span></span>

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a><span data-ttu-id="80d45-112">Söka efter en RHEL, version och avbildning</span><span class="sxs-lookup"><span data-stu-id="80d45-112">Search for a RHEL version and image</span></span>

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a><span data-ttu-id="80d45-113">Skapa en Redhat Linux VM</span><span class="sxs-lookup"><span data-stu-id="80d45-113">Create a Redhat Linux VM</span></span>

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-toohello-vm"></a><span data-ttu-id="80d45-114">SSH toohello VM</span><span class="sxs-lookup"><span data-stu-id="80d45-114">SSH toohello VM</span></span>

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a><span data-ttu-id="80d45-115">YUM uppdateringspaket</span><span class="sxs-lookup"><span data-stu-id="80d45-115">Update YUM packages</span></span>

```bash
sudo yum update
```

### <a name="install-packages-needed"></a><span data-ttu-id="80d45-116">Installera paket som krävs</span><span class="sxs-lookup"><span data-stu-id="80d45-116">Install packages needed</span></span>

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

<span data-ttu-id="80d45-117">Nu när hello krävs paket som är installerade på hello virtuell Linux-dator, är hello nästa uppgift toojoin hello virtuella toohello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="80d45-117">Now that hello required packages are installed on hello Linux virtual machine, hello next task is toojoin hello virtual machine toohello managed domain.</span></span>

### <a name="discover-hello-aad-domain-services-managed-domain"></a><span data-ttu-id="80d45-118">Identifiera hello AAD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="80d45-118">Discover hello AAD Domain Services managed domain</span></span>

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a><span data-ttu-id="80d45-119">Initiera kerberos</span><span class="sxs-lookup"><span data-stu-id="80d45-119">Initialize kerberos</span></span>

<span data-ttu-id="80d45-120">Kontrollera att du anger en användare som tillhör toohello ' AAD DC-administratörsgruppen.</span><span class="sxs-lookup"><span data-stu-id="80d45-120">Ensure that you specify a user who belongs toohello 'AAD DC Administrators' group.</span></span> <span data-ttu-id="80d45-121">Endast dessa användare kan ansluta till datorer toohello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="80d45-121">Only these users can join computers toohello managed domain.</span></span>

```bash
kinit ahmet@mydomain.com
```

### <a name="join-hello-machine-toohello-domain"></a><span data-ttu-id="80d45-122">Anslut till hello datorn toohello domän</span><span class="sxs-lookup"><span data-stu-id="80d45-122">Join hello machine toohello domain</span></span>

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-hello-machine-is-joined-toohello-domain"></a><span data-ttu-id="80d45-123">Kontrollera hello datorn är domänansluten toohello domän</span><span class="sxs-lookup"><span data-stu-id="80d45-123">Verify hello machine is joined toohello domain</span></span>

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="80d45-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80d45-124">Next Steps</span></span>

* [<span data-ttu-id="80d45-125">Red Hat Update infrastruktur (RHUI) för att på begäran Red Hat Enterprise Linux virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="80d45-125">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="80d45-126">Ställ in Key Vault för virtuella datorer i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="80d45-126">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="80d45-127">Distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80d45-127">Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI</span></span>](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
