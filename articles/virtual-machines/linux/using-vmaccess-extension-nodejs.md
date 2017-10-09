---
title: "aaaReset åtkomst på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget | Microsoft Docs"
description: "Återställa åtkomst på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 2636655f3f7d14ba30e1dc62c319e4e278521ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-10"></a><span data-ttu-id="15256-103">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="15256-103">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension with hello Azure CLI 1.0</span></span>
<span data-ttu-id="15256-104">Den här artikeln visar hur toouse hello Azure VMAcesss tillägget toocheck eller reparera en disk, återställa användaråtkomst, hantera användarkonton eller återställa hello SSHD konfigurationen på Linux.</span><span class="sxs-lookup"><span data-stu-id="15256-104">This article shows you how toouse hello Azure VMAcesss Extension toocheck or repair a disk, reset user access, manage user accounts, or reset hello SSHD configuration on Linux.</span></span> <span data-ttu-id="15256-105">hello artikel kräver:</span><span class="sxs-lookup"><span data-stu-id="15256-105">hello article requires:</span></span>

* <span data-ttu-id="15256-106">ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)).</span><span class="sxs-lookup"><span data-stu-id="15256-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="15256-107">Hej [Azure CLI](../../cli-install-nodejs.md) loggat in med `azure login`.</span><span class="sxs-lookup"><span data-stu-id="15256-107">hello [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="15256-108">hello Azure CLI *måste vara i* läget Azure Resource Manager `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="15256-108">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="15256-109">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="15256-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="15256-110">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="15256-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="15256-111">[Azure CLI 1.0](#quick-commands)– våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="15256-111">[Azure CLI 1.0](#quick-commands)– our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="15256-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="15256-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="15256-113">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="15256-113">Quick commands</span></span>
<span data-ttu-id="15256-114">Det finns två sätt toouse VMAccess på din virtuella Linux-datorer:</span><span class="sxs-lookup"><span data-stu-id="15256-114">There are two ways toouse VMAccess on your Linux VMs:</span></span>

* <span data-ttu-id="15256-115">Med hjälp av hello Azure CLI 1.0 och hello obligatoriska parametrar.</span><span class="sxs-lookup"><span data-stu-id="15256-115">Using hello Azure CLI 1.0 and hello required parameters.</span></span>
* <span data-ttu-id="15256-116">Använder rådata JSON-filer som VMAccess bearbetar och sedan vidta åtgärder för.</span><span class="sxs-lookup"><span data-stu-id="15256-116">Using raw JSON files that VMAccess processes and then act on.</span></span>

<span data-ttu-id="15256-117">För hello snabbkommando avsnittet vi toouse hello Azure CLI 1.0 `azure vm reset-access` metod.</span><span class="sxs-lookup"><span data-stu-id="15256-117">For hello quick command section, we are going toouse hello Azure CLI 1.0 `azure vm reset-access` method.</span></span> <span data-ttu-id="15256-118">Följande kommandoexempel Ersätt i hello hello värden som innehåller ”exempel” Hej värden från din egen miljö.</span><span class="sxs-lookup"><span data-stu-id="15256-118">In hello following command examples, replace hello values that contain "example" with hello values from your own environment.</span></span>

## <a name="create-a-resource-group-and-linux-vm"></a><span data-ttu-id="15256-119">Skapa en resursgrupp och en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="15256-119">Create a Resource Group and Linux VM</span></span>
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a><span data-ttu-id="15256-120">Skapa en Debian virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15256-120">Create a Debian VM</span></span>
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a><span data-ttu-id="15256-121">Återställ rotlösenordet</span><span class="sxs-lookup"><span data-stu-id="15256-121">Reset root password</span></span>
<span data-ttu-id="15256-122">tooreset hello rotlösenordet:</span><span class="sxs-lookup"><span data-stu-id="15256-122">tooreset hello root password:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a><span data-ttu-id="15256-123">Återställning av SSH-nyckel</span><span class="sxs-lookup"><span data-stu-id="15256-123">SSH key reset</span></span>
<span data-ttu-id="15256-124">tooreset hello SSH-nyckel för en rot-användare:</span><span class="sxs-lookup"><span data-stu-id="15256-124">tooreset hello SSH key of a non-root user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a><span data-ttu-id="15256-125">Skapa en användare</span><span class="sxs-lookup"><span data-stu-id="15256-125">Create a user</span></span>
<span data-ttu-id="15256-126">toocreate en användare:</span><span class="sxs-lookup"><span data-stu-id="15256-126">toocreate a user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a><span data-ttu-id="15256-127">Ta bort en användare</span><span class="sxs-lookup"><span data-stu-id="15256-127">Remove a user</span></span>
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a><span data-ttu-id="15256-128">Återställ SSHD</span><span class="sxs-lookup"><span data-stu-id="15256-128">Reset SSHD</span></span>
<span data-ttu-id="15256-129">tooreset hello SSHD konfiguration:</span><span class="sxs-lookup"><span data-stu-id="15256-129">tooreset hello SSHD configuration:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a><span data-ttu-id="15256-130">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="15256-130">Detailed walkthrough</span></span>
### <a name="vmaccess-defined"></a><span data-ttu-id="15256-131">VMAccess definierats:</span><span class="sxs-lookup"><span data-stu-id="15256-131">VMAccess defined:</span></span>
<span data-ttu-id="15256-132">hello disk på Linux-VM visas fel.</span><span class="sxs-lookup"><span data-stu-id="15256-132">hello disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="15256-133">Du på något sätt återställa hello rotlösenordet för Linux-VM eller tagits bort av misstag din privata SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="15256-133">You somehow reset hello root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="15256-134">Om detta har inträffat i hello dagarnas hello datacenter, skulle du behöver toodrive det och öppna sedan hello KVM tooget hello server-konsolen.</span><span class="sxs-lookup"><span data-stu-id="15256-134">If that happened back in hello days of hello datacenter, you would need toodrive there and then open hello KVM tooget at hello server console.</span></span> <span data-ttu-id="15256-135">Se hello Azure VMAccess-tillägget som den KVM-växel som gör att du tooaccess hello konsolen tooreset åtkomst tooLinux eller genomför diskunderhåll nivå.</span><span class="sxs-lookup"><span data-stu-id="15256-135">Think of hello Azure VMAccess extension as that KVM switch that allows you tooaccess hello console tooreset access tooLinux or perform disk level maintenance.</span></span>

<span data-ttu-id="15256-136">Detaljerad genomgång för hello vi toouse hello långt format för VMAccess som använder rådata JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="15256-136">For hello detailed walkthrough, we are going toouse hello long form of VMAccess, which uses raw JSON files.</span></span>  <span data-ttu-id="15256-137">Filerna VMAccess JSON kallas även från Azure-mallar.</span><span class="sxs-lookup"><span data-stu-id="15256-137">These VMAccess JSON files can also be called from Azure templates.</span></span>

### <a name="using-vmaccess-toocheck-or-repair-hello-disk-of-a-linux-vm"></a><span data-ttu-id="15256-138">Med hjälp av VMAccess toocheck eller reparera hello disk för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="15256-138">Using VMAccess toocheck or repair hello disk of a Linux VM</span></span>
<span data-ttu-id="15256-139">Med hjälp av VMAccess kan du göra en fsck körs på hello disk under Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="15256-139">Using VMAccess you can do a fsck run on hello disk under your Linux VM.</span></span>  <span data-ttu-id="15256-140">Du kan också göra en kontroll och en diskreparationen med hjälp av en VMAccess.</span><span class="sxs-lookup"><span data-stu-id="15256-140">You can also do a disk check and a disk repair using a VMAccess.</span></span>

<span data-ttu-id="15256-141">Använd skriptet VMAccess toocheck och reparera hello disk:</span><span class="sxs-lookup"><span data-stu-id="15256-141">toocheck, and then repair hello disk use this VMAccess script:</span></span>

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

<span data-ttu-id="15256-142">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="15256-142">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-tooreset-user-access-toolinux"></a><span data-ttu-id="15256-143">Med hjälp av VMAccess tooreset användaren åtkomst tooLinux</span><span class="sxs-lookup"><span data-stu-id="15256-143">Using VMAccess tooreset user access tooLinux</span></span>
<span data-ttu-id="15256-144">Om du har förlorat åtkomst tooroot på Linux-VM, kan du starta ett VMAccess skript tooreset hello rotlösenord.</span><span class="sxs-lookup"><span data-stu-id="15256-144">If you have lost access tooroot on your Linux VM, you can launch a VMAccess script tooreset hello root password.</span></span>

<span data-ttu-id="15256-145">tooreset hello rotlösenordet, Använd det här VMAccess-skriptet:</span><span class="sxs-lookup"><span data-stu-id="15256-145">tooreset hello root password, use this VMAccess script:</span></span>

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

<span data-ttu-id="15256-146">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="15256-146">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

<span data-ttu-id="15256-147">tooreset hello SSH-nyckel för en rot-användare, Använd VMAccess skriptet:</span><span class="sxs-lookup"><span data-stu-id="15256-147">tooreset hello SSH key of a non-root user, use this VMAccess script:</span></span>

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

<span data-ttu-id="15256-148">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="15256-148">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-toomanage-user-accounts-on-linux"></a><span data-ttu-id="15256-149">Med hjälp av VMAccess toomanage användarkonton på Linux</span><span class="sxs-lookup"><span data-stu-id="15256-149">Using VMAccess toomanage user accounts on Linux</span></span>
<span data-ttu-id="15256-150">VMAccess är ett Python-skript som kan använda toomanage användare på ditt Linux VM utan att logga in och använda sudo eller hello rotkontot.</span><span class="sxs-lookup"><span data-stu-id="15256-150">VMAccess is a Python script that can be used toomanage users on your Linux VM without logging in and using sudo or hello root account.</span></span>

<span data-ttu-id="15256-151">toocreate en användare kan använda det här VMAccess-skriptet:</span><span class="sxs-lookup"><span data-stu-id="15256-151">toocreate a user, use this VMAccess script:</span></span>

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="15256-152">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="15256-152">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

<span data-ttu-id="15256-153">toodelete en användare kan använda det här VMAccess-skriptet:</span><span class="sxs-lookup"><span data-stu-id="15256-153">toodelete a user, use this VMAccess script:</span></span>

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

<span data-ttu-id="15256-154">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="15256-154">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-tooreset-hello-sshd-configuration"></a><span data-ttu-id="15256-155">Med VMAccess tooreset hello SSHD konfiguration</span><span class="sxs-lookup"><span data-stu-id="15256-155">Using VMAccess tooreset hello SSHD configuration</span></span>
<span data-ttu-id="15256-156">Om du gör ändringar toohello Linux virtuella datorer SSHD konfiguration och Stäng hello SSH-anslutning innan du kontrollerar hello ändringar du kan förhindras att SSH'ing igen.</span><span class="sxs-lookup"><span data-stu-id="15256-156">If you make changes toohello Linux VMs SSHD configuration and close hello SSH connection before verifying hello changes, you may be prevented from SSH'ing back in.</span></span>  <span data-ttu-id="15256-157">VMAccess kan vara används tooreset hello SSHD configuration tillbaka tooa fungerande konfiguration utan loggas via SSH.</span><span class="sxs-lookup"><span data-stu-id="15256-157">VMAccess can be used tooreset hello SSHD configuration back tooa known good configuration without being logged in over SSH.</span></span>

<span data-ttu-id="15256-158">tooreset hello SSHD konfiguration använder det här VMAccess-skriptet:</span><span class="sxs-lookup"><span data-stu-id="15256-158">tooreset hello SSHD configuration use this VMAccess script:</span></span>

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="15256-159">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="15256-159">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a><span data-ttu-id="15256-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15256-160">Next steps</span></span>
<span data-ttu-id="15256-161">Uppdatera Linux är med hjälp av Azure VMAccess tillägg en metod toomake ändringar på en Linux-VM som körs.</span><span class="sxs-lookup"><span data-stu-id="15256-161">Updating Linux using Azure VMAccess Extensions is one method toomake changes on a running Linux VM.</span></span>  <span data-ttu-id="15256-162">Du kan också använda verktyg som molntjänster init och Azure-mallar toomodify din Linux VM på Start.</span><span class="sxs-lookup"><span data-stu-id="15256-162">You can also use tools like cloud-init and Azure Templates toomodify your Linux VM on boot.</span></span>

[<span data-ttu-id="15256-163">Om virtuella datortillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="15256-163">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="15256-164">Skapa Azure Resource Manager-mallar med Linux VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="15256-164">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="15256-165">Med hjälp av molnet init toocustomize en Linux VM under skapande av</span><span class="sxs-lookup"><span data-stu-id="15256-165">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

