---
title: "aaaReset åtkomst tooan virtuella Azure Linux-datorer | Microsoft Docs"
description: "Hur toomanage användare och återställa åtkomst på virtuella Linux-datorer med hjälp av hello VMAccess-tillägget och hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.openlocfilehash: 2f8db01b9fac20bf547d8b1926e5c0b3c5d18280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-20"></a><span data-ttu-id="9c2c2-103">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Linux-datorer med hjälp av hello VMAccess-tillägget med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9c2c2-103">Manage users, SSH, and check or repair disks on Linux VMs using hello VMAccess Extension with hello Azure CLI 2.0</span></span>
<span data-ttu-id="9c2c2-104">hello disk på Linux-VM visas fel.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-104">hello disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="9c2c2-105">Du på något sätt återställa hello rotlösenordet för Linux-VM eller tagits bort av misstag din privata SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-105">You somehow reset hello root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="9c2c2-106">Om detta har inträffat i hello dagarnas hello datacenter, skulle du behöver toodrive det och öppna sedan hello KVM tooget hello server-konsolen.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-106">If that happened back in hello days of hello datacenter, you would need toodrive there and then open hello KVM tooget at hello server console.</span></span> <span data-ttu-id="9c2c2-107">Se hello Azure VMAccess-tillägget som den KVM-växel som gör att du tooaccess hello konsolen tooreset åtkomst tooLinux eller genomför diskunderhåll nivå.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-107">Think of hello Azure VMAccess extension as that KVM switch that allows you tooaccess hello console tooreset access tooLinux or perform disk level maintenance.</span></span>

<span data-ttu-id="9c2c2-108">Den här artikeln visar hur toouse hello Azure VMAccess-tillägget toocheck eller reparera en disk, återställa användaråtkomst, hantera användarkonton och Återställ hello SSH-konfigurationen på Linux.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-108">This article shows you how toouse hello Azure VMAccess Extension toocheck or repair a disk, reset user access, manage user accounts, or reset hello SSH configuration on Linux.</span></span> <span data-ttu-id="9c2c2-109">Du kan också utföra dessa steg med hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9c2c2-109">You can also perform these steps with hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="ways-toouse-hello-vmaccess-extension"></a><span data-ttu-id="9c2c2-110">Sätt toouse hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="9c2c2-110">Ways toouse hello VMAccess Extension</span></span>
<span data-ttu-id="9c2c2-111">Det finns två sätt som du kan använda hello VMAccess-tillägget på din virtuella Linux-datorer:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-111">There are two ways that you can use hello VMAccess Extension on your Linux VMs:</span></span>

* <span data-ttu-id="9c2c2-112">Använd hello Azure CLI 2.0 och hello krävs parametrar.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-112">Use hello Azure CLI 2.0 and hello required parameters.</span></span>
* <span data-ttu-id="9c2c2-113">[Använd rådata JSON-filer som hello VMAccess-tillägget processen](#use-json-files-and-the-vmaccess-extension) och sedan vidta åtgärder för.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-113">[Use raw JSON files that hello VMAccess Extension process](#use-json-files-and-the-vmaccess-extension) and then act on.</span></span>

<span data-ttu-id="9c2c2-114">Hej följande exempel används [az vm användaren](/cli/azure/vm/user) kommandon.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-114">hello following examples use [az vm user](/cli/azure/vm/user) commands.</span></span> <span data-ttu-id="9c2c2-115">tooperform dessa steg, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9c2c2-115">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="reset-ssh-key"></a><span data-ttu-id="9c2c2-116">Återställ SSH-nyckel</span><span class="sxs-lookup"><span data-stu-id="9c2c2-116">Reset SSH key</span></span>
<span data-ttu-id="9c2c2-117">hello följande exempel återställs hello SSH-nyckeln för hello användare `azureuser` på hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-117">hello following example resets hello SSH key for hello user `azureuser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a><span data-ttu-id="9c2c2-118">Återställa lösenord</span><span class="sxs-lookup"><span data-stu-id="9c2c2-118">Reset password</span></span>
<span data-ttu-id="9c2c2-119">hello följande exempel återställs hello användarlösenord hello `azureuser` på hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-119">hello following example resets hello password for hello user `azureuser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a><span data-ttu-id="9c2c2-120">Starta om SSH</span><span class="sxs-lookup"><span data-stu-id="9c2c2-120">Restart SSH</span></span>
<span data-ttu-id="9c2c2-121">hello följande exempel startar hello SSH-daemon och återställer hello SSH toodefault konfigurationsvärden på en virtuell dator med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-121">hello following example restarts hello SSH daemon and resets hello SSH configuration toodefault values on a VM named `myVM`:</span></span>

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a><span data-ttu-id="9c2c2-122">Skapa en användare</span><span class="sxs-lookup"><span data-stu-id="9c2c2-122">Create a user</span></span>
<span data-ttu-id="9c2c2-123">hello följande exempel skapas en användare med namnet `myNewUser` använder en SSH-nyckel för autentisering på hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-123">hello following example creates a user named `myNewUser` using an SSH key for authentication on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a><span data-ttu-id="9c2c2-124">Tar bort en användare</span><span class="sxs-lookup"><span data-stu-id="9c2c2-124">Delete a user</span></span>
<span data-ttu-id="9c2c2-125">hello följande exempel tar bort en användare med namnet `myNewUser` på hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-125">hello following example deletes a user named `myNewUser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-hello-vmaccess-extension"></a><span data-ttu-id="9c2c2-126">Använd JSON-filer och hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="9c2c2-126">Use JSON files and hello VMAccess Extension</span></span>
<span data-ttu-id="9c2c2-127">följande exempel hello använda raw JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-127">hello following examples use raw JSON files.</span></span> <span data-ttu-id="9c2c2-128">Använd [az vm-tillägget set](/cli/azure/vm/extension#set) toothen anropa JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-128">Use [az vm extension set](/cli/azure/vm/extension#set) toothen call your JSON files.</span></span> <span data-ttu-id="9c2c2-129">Filerna JSON kallas även från Azure-mallar.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-129">These JSON files can also be called from Azure templates.</span></span> 

### <a name="reset-user-access"></a><span data-ttu-id="9c2c2-130">Återställ användaråtkomst</span><span class="sxs-lookup"><span data-stu-id="9c2c2-130">Reset user access</span></span>
<span data-ttu-id="9c2c2-131">Om du har förlorat åtkomst tooroot på Linux-VM, kan du starta en VMAccess skriptet tooreset SSH-nyckeln eller lösenordet för en användare.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-131">If you have lost access tooroot on your Linux VM, you can launch a VMAccess script tooreset a user's SSH key or password.</span></span>

<span data-ttu-id="9c2c2-132">tooreset hello offentlig SSH-nyckel för en användare skapar en fil med namnet `reset_ssh_key.json` och lägga till inställningarna i hello följande format.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-132">tooreset hello SSH public key of a user, create a file named `reset_ssh_key.json` and add settings in hello following format.</span></span> <span data-ttu-id="9c2c2-133">Ersätt värdena för hello `username` och `ssh_key` parametrar:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-133">Substitute your own values for hello `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

<span data-ttu-id="9c2c2-134">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-134">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

<span data-ttu-id="9c2c2-135">tooreset ett lösenord, skapa en fil med namnet `reset_user_password.json` och lägga till inställningarna i hello följande format.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-135">tooreset a user password, create a file named `reset_user_password.json` and add settings in hello following format.</span></span> <span data-ttu-id="9c2c2-136">Ersätt värdena för hello `username` och `password` parametrar:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-136">Substitute your own values for hello `username` and `password` parameters:</span></span>

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

<span data-ttu-id="9c2c2-137">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-137">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a><span data-ttu-id="9c2c2-138">Starta om SSH</span><span class="sxs-lookup"><span data-stu-id="9c2c2-138">Restart SSH</span></span>
<span data-ttu-id="9c2c2-139">toorestart hello SSH-daemon och återställa hello SSH konfigurationsvärden toodefault, skapa en fil med namnet `reset_sshd.json`.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-139">toorestart hello SSH daemon and reset hello SSH configuration toodefault values, create a file named `reset_sshd.json`.</span></span> <span data-ttu-id="9c2c2-140">Lägg till hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-140">Add hello following content:</span></span>

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="9c2c2-141">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-141">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a><span data-ttu-id="9c2c2-142">Hantera användare</span><span class="sxs-lookup"><span data-stu-id="9c2c2-142">Manage users</span></span>

<span data-ttu-id="9c2c2-143">toocreate en användare som använder en SSH-nyckel för autentisering, skapa en fil med namnet `create_new_user.json` och lägga till inställningarna i hello följande format.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-143">toocreate a user that uses an SSH key for authentication, create a file named `create_new_user.json` and add settings in hello following format.</span></span> <span data-ttu-id="9c2c2-144">Ersätt värdena för hello `username` och `ssh_key` parametrar:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-144">Substitute your own values for hello `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="9c2c2-145">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-145">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

<span data-ttu-id="9c2c2-146">toodelete en användare skapar en fil med namnet `delete_user.json` och Lägg till hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-146">toodelete a user, create a file named `delete_user.json` and add hello following content.</span></span> <span data-ttu-id="9c2c2-147">Ersätt värdet för hello `remove_user` parameter:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-147">Substitute your own value for hello `remove_user` parameter:</span></span>

```json
{
  "remove_user":"myNewUser"
}
```

<span data-ttu-id="9c2c2-148">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-148">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-hello-disk"></a><span data-ttu-id="9c2c2-149">Kontrollera eller reparera hello disk</span><span class="sxs-lookup"><span data-stu-id="9c2c2-149">Check or repair hello disk</span></span>
<span data-ttu-id="9c2c2-150">Med hjälp av VMAccess kan också kontrollera och reparera en disk som du lagt till toohello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-150">Using VMAccess you can also check and repair a disk that you added toohello Linux VM.</span></span>

<span data-ttu-id="9c2c2-151">toocheck och reparera hello disk, skapa en fil med namnet `disk_check_repair.json` och lägga till inställningarna i hello följande format.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-151">toocheck and then repair hello disk, create a file named `disk_check_repair.json` and add settings in hello following format.</span></span> <span data-ttu-id="9c2c2-152">Ersätt värdet för hello namnet på `repair_disk`:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-152">Substitute your own value for hello name of `repair_disk`:</span></span>

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

<span data-ttu-id="9c2c2-153">Kör hello VMAccess skriptet med:</span><span class="sxs-lookup"><span data-stu-id="9c2c2-153">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a><span data-ttu-id="9c2c2-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c2c2-154">Next steps</span></span>
<span data-ttu-id="9c2c2-155">Uppdatera Linux är med hjälp av hello Azure VMAccess-tillägget en metod toomake ändringar på en Linux-VM som körs.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-155">Updating Linux using hello Azure VMAccess Extension is one method toomake changes on a running Linux VM.</span></span> <span data-ttu-id="9c2c2-156">Du kan också använda verktyg som molntjänster init och Azure Resource Manager mallar toomodify din Linux VM på Start.</span><span class="sxs-lookup"><span data-stu-id="9c2c2-156">You can also use tools like cloud-init and Azure Resource Manager templates toomodify your Linux VM on boot.</span></span>

[<span data-ttu-id="9c2c2-157">Tillägg för virtuell dator och funktioner för Linux</span><span class="sxs-lookup"><span data-stu-id="9c2c2-157">Virtual machine extensions and features for Linux</span></span>](extensions-features.md)

[<span data-ttu-id="9c2c2-158">Skapa Azure Resource Manager-mallar med Linux VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="9c2c2-158">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="9c2c2-159">Med hjälp av molnet init toocustomize en Linux VM under skapande av</span><span class="sxs-lookup"><span data-stu-id="9c2c2-159">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md)

