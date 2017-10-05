---
title: "Återställa åtkomst till en Azure Linux-dator | Microsoft Docs"
description: "Hantera användare och återställa åtkomst på virtuella Linux-datorer med hjälp av VMAccess-tillägget och Azure CLI 2.0"
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
ms.openlocfilehash: 587c73278a9a92776276a811c5c4c8d3db773de3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a><span data-ttu-id="b89af-103">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Linux-datorer med hjälp av VMAccess-tillägget med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b89af-103">Manage users, SSH, and check or repair disks on Linux VMs using the VMAccess Extension with the Azure CLI 2.0</span></span>
<span data-ttu-id="b89af-104">Disken på Linux-VM visas fel.</span><span class="sxs-lookup"><span data-stu-id="b89af-104">The disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="b89af-105">Du på något sätt återställa rotlösenordet för Linux-VM eller tagits bort av misstag din privata SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="b89af-105">You somehow reset the root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="b89af-106">Om detta har inträffat i dagar för datacenter, behöver du köra det och öppna sedan KVM få vid servern.</span><span class="sxs-lookup"><span data-stu-id="b89af-106">If that happened back in the days of the datacenter, you would need to drive there and then open the KVM to get at the server console.</span></span> <span data-ttu-id="b89af-107">Se Azure VMAccess-tillägget som den KVM-växel som gör att du kan använda konsolen för att återställa åtkomst till Linux eller genomför diskunderhåll nivå.</span><span class="sxs-lookup"><span data-stu-id="b89af-107">Think of the Azure VMAccess extension as that KVM switch that allows you to access the console to reset access to Linux or perform disk level maintenance.</span></span>

<span data-ttu-id="b89af-108">Den här artikeln visar hur du använder Azure VMAccess-tillägget för att kontrollera eller reparera en disk, återställa användaråtkomst, hantera användarkonton eller återställa SSH-konfigurationen på Linux.</span><span class="sxs-lookup"><span data-stu-id="b89af-108">This article shows you how to use the Azure VMAccess Extension to check or repair a disk, reset user access, manage user accounts, or reset the SSH configuration on Linux.</span></span> <span data-ttu-id="b89af-109">Du kan också utföra dessa steg med [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b89af-109">You can also perform these steps with the [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="ways-to-use-the-vmaccess-extension"></a><span data-ttu-id="b89af-110">Sätt att använda VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="b89af-110">Ways to use the VMAccess Extension</span></span>
<span data-ttu-id="b89af-111">Det finns två sätt som du kan använda VMAccess-tillägget på din virtuella Linux-datorer:</span><span class="sxs-lookup"><span data-stu-id="b89af-111">There are two ways that you can use the VMAccess Extension on your Linux VMs:</span></span>

* <span data-ttu-id="b89af-112">Använda Azure CLI 2.0 och de obligatoriska parametrarna.</span><span class="sxs-lookup"><span data-stu-id="b89af-112">Use the Azure CLI 2.0 and the required parameters.</span></span>
* <span data-ttu-id="b89af-113">[Använd raw JSON-filer som bearbetar VMAccess-tillägget](#use-json-files-and-the-vmaccess-extension) och sedan vidta åtgärder för.</span><span class="sxs-lookup"><span data-stu-id="b89af-113">[Use raw JSON files that the VMAccess Extension process](#use-json-files-and-the-vmaccess-extension) and then act on.</span></span>

<span data-ttu-id="b89af-114">I följande exempel används [az vm användaren](/cli/azure/vm/user) kommandon.</span><span class="sxs-lookup"><span data-stu-id="b89af-114">The following examples use [az vm user](/cli/azure/vm/user) commands.</span></span> <span data-ttu-id="b89af-115">Om du vill utföra dessa steg behöver du senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b89af-115">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="reset-ssh-key"></a><span data-ttu-id="b89af-116">Återställ SSH-nyckel</span><span class="sxs-lookup"><span data-stu-id="b89af-116">Reset SSH key</span></span>
<span data-ttu-id="b89af-117">I följande exempel återställer SSH-nyckeln för användaren `azureuser` på den virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="b89af-117">The following example resets the SSH key for the user `azureuser` on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a><span data-ttu-id="b89af-118">Återställa lösenord</span><span class="sxs-lookup"><span data-stu-id="b89af-118">Reset password</span></span>
<span data-ttu-id="b89af-119">I följande exempel återställer lösenordet för användaren `azureuser` på den virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="b89af-119">The following example resets the password for the user `azureuser` on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a><span data-ttu-id="b89af-120">Starta om SSH</span><span class="sxs-lookup"><span data-stu-id="b89af-120">Restart SSH</span></span>
<span data-ttu-id="b89af-121">I följande exempel startar om SSH-daemon och återställer SSH-konfigurationen till standardvärdena på en virtuell dator med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="b89af-121">The following example restarts the SSH daemon and resets the SSH configuration to default values on a VM named `myVM`:</span></span>

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a><span data-ttu-id="b89af-122">Skapa en användare</span><span class="sxs-lookup"><span data-stu-id="b89af-122">Create a user</span></span>
<span data-ttu-id="b89af-123">I följande exempel skapas en användare med namnet `myNewUser` använder en SSH-nyckel för autentisering på den virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="b89af-123">The following example creates a user named `myNewUser` using an SSH key for authentication on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a><span data-ttu-id="b89af-124">Tar bort en användare</span><span class="sxs-lookup"><span data-stu-id="b89af-124">Delete a user</span></span>
<span data-ttu-id="b89af-125">I följande exempel tar bort en användare med namnet `myNewUser` på den virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="b89af-125">The following example deletes a user named `myNewUser` on the VM named `myVM`:</span></span>

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a><span data-ttu-id="b89af-126">Använd JSON-filer och VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="b89af-126">Use JSON files and the VMAccess Extension</span></span>
<span data-ttu-id="b89af-127">I följande exempel används raw JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="b89af-127">The following examples use raw JSON files.</span></span> <span data-ttu-id="b89af-128">Använd [az vm-tillägget set](/cli/azure/vm/extension#set) att anropa JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="b89af-128">Use [az vm extension set](/cli/azure/vm/extension#set) to then call your JSON files.</span></span> <span data-ttu-id="b89af-129">Filerna JSON kallas även från Azure-mallar.</span><span class="sxs-lookup"><span data-stu-id="b89af-129">These JSON files can also be called from Azure templates.</span></span> 

### <a name="reset-user-access"></a><span data-ttu-id="b89af-130">Återställ användaråtkomst</span><span class="sxs-lookup"><span data-stu-id="b89af-130">Reset user access</span></span>
<span data-ttu-id="b89af-131">Om du har förlorat åtkomsten till roten på Linux-VM, kan du starta ett VMAccess-skript för att återställa SSH-nyckeln eller lösenordet för en användare.</span><span class="sxs-lookup"><span data-stu-id="b89af-131">If you have lost access to root on your Linux VM, you can launch a VMAccess script to reset a user's SSH key or password.</span></span>

<span data-ttu-id="b89af-132">Om du vill återställa den offentliga SSH-nyckeln för en användare skapar en fil med namnet `reset_ssh_key.json` och lägga till inställningarna i följande format.</span><span class="sxs-lookup"><span data-stu-id="b89af-132">To reset the SSH public key of a user, create a file named `reset_ssh_key.json` and add settings in the following format.</span></span> <span data-ttu-id="b89af-133">Ersätt värdena för den `username` och `ssh_key` parametrar:</span><span class="sxs-lookup"><span data-stu-id="b89af-133">Substitute your own values for the `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

<span data-ttu-id="b89af-134">Kör skriptet VMAccess med:</span><span class="sxs-lookup"><span data-stu-id="b89af-134">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

<span data-ttu-id="b89af-135">Om du vill återställa en användarlösenord, skapar du en fil med namnet `reset_user_password.json` och lägga till inställningarna i följande format.</span><span class="sxs-lookup"><span data-stu-id="b89af-135">To reset a user password, create a file named `reset_user_password.json` and add settings in the following format.</span></span> <span data-ttu-id="b89af-136">Ersätt värdena för den `username` och `password` parametrar:</span><span class="sxs-lookup"><span data-stu-id="b89af-136">Substitute your own values for the `username` and `password` parameters:</span></span>

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

<span data-ttu-id="b89af-137">Kör skriptet VMAccess med:</span><span class="sxs-lookup"><span data-stu-id="b89af-137">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a><span data-ttu-id="b89af-138">Starta om SSH</span><span class="sxs-lookup"><span data-stu-id="b89af-138">Restart SSH</span></span>
<span data-ttu-id="b89af-139">Om du vill starta om SSH-daemon och återställa SSH-konfigurationen till standardvärdena, skapar du en fil med namnet `reset_sshd.json`.</span><span class="sxs-lookup"><span data-stu-id="b89af-139">To restart the SSH daemon and reset the SSH configuration to default values, create a file named `reset_sshd.json`.</span></span> <span data-ttu-id="b89af-140">Lägg till följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="b89af-140">Add the following content:</span></span>

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="b89af-141">Kör skriptet VMAccess med:</span><span class="sxs-lookup"><span data-stu-id="b89af-141">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a><span data-ttu-id="b89af-142">Hantera användare</span><span class="sxs-lookup"><span data-stu-id="b89af-142">Manage users</span></span>

<span data-ttu-id="b89af-143">Om du vill skapa en användare som använder en SSH-nyckel för autentisering, skapar du en fil med namnet `create_new_user.json` och lägga till inställningarna i följande format.</span><span class="sxs-lookup"><span data-stu-id="b89af-143">To create a user that uses an SSH key for authentication, create a file named `create_new_user.json` and add settings in the following format.</span></span> <span data-ttu-id="b89af-144">Ersätt värdena för den `username` och `ssh_key` parametrar:</span><span class="sxs-lookup"><span data-stu-id="b89af-144">Substitute your own values for the `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="b89af-145">Kör skriptet VMAccess med:</span><span class="sxs-lookup"><span data-stu-id="b89af-145">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

<span data-ttu-id="b89af-146">Ta bort en användare genom att skapa en fil med namnet `delete_user.json` och Lägg till följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="b89af-146">To delete a user, create a file named `delete_user.json` and add the following content.</span></span> <span data-ttu-id="b89af-147">Ersätt värdet för den `remove_user` parameter:</span><span class="sxs-lookup"><span data-stu-id="b89af-147">Substitute your own value for the `remove_user` parameter:</span></span>

```json
{
  "remove_user":"myNewUser"
}
```

<span data-ttu-id="b89af-148">Kör skriptet VMAccess med:</span><span class="sxs-lookup"><span data-stu-id="b89af-148">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a><span data-ttu-id="b89af-149">Kontrollera eller reparera disken</span><span class="sxs-lookup"><span data-stu-id="b89af-149">Check or repair the disk</span></span>
<span data-ttu-id="b89af-150">Med hjälp av VMAccess du också kontrollera och reparera en disk som du har lagt till Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="b89af-150">Using VMAccess you can also check and repair a disk that you added to the Linux VM.</span></span>

<span data-ttu-id="b89af-151">Om du vill kontrollera och reparera disken, skapa en fil med namnet `disk_check_repair.json` och lägga till inställningarna i följande format.</span><span class="sxs-lookup"><span data-stu-id="b89af-151">To check and then repair the disk, create a file named `disk_check_repair.json` and add settings in the following format.</span></span> <span data-ttu-id="b89af-152">Ersätt värdet för namnet på `repair_disk`:</span><span class="sxs-lookup"><span data-stu-id="b89af-152">Substitute your own value for the name of `repair_disk`:</span></span>

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

<span data-ttu-id="b89af-153">Kör skriptet VMAccess med:</span><span class="sxs-lookup"><span data-stu-id="b89af-153">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a><span data-ttu-id="b89af-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b89af-154">Next steps</span></span>
<span data-ttu-id="b89af-155">Uppdatera Linux med hjälp av Azure VMAccess-tillägget är en metod för att göra ändringar på en Linux-VM som körs.</span><span class="sxs-lookup"><span data-stu-id="b89af-155">Updating Linux using the Azure VMAccess Extension is one method to make changes on a running Linux VM.</span></span> <span data-ttu-id="b89af-156">Du kan också använda verktyg som molntjänster init och Azure Resource Manager-mallar för att ändra din Linux VM på Start.</span><span class="sxs-lookup"><span data-stu-id="b89af-156">You can also use tools like cloud-init and Azure Resource Manager templates to modify your Linux VM on boot.</span></span>

[<span data-ttu-id="b89af-157">Tillägg för virtuell dator och funktioner för Linux</span><span class="sxs-lookup"><span data-stu-id="b89af-157">Virtual machine extensions and features for Linux</span></span>](extensions-features.md)

[<span data-ttu-id="b89af-158">Skapa Azure Resource Manager-mallar med Linux VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="b89af-158">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="b89af-159">Med hjälp av molnet init för att anpassa en Linux VM under skapandet</span><span class="sxs-lookup"><span data-stu-id="b89af-159">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md)

