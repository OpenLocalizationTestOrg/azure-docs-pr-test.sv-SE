---
title: "Återställ lösenordet eller fjärrskrivbord konfigurationen på en virtuell Windows-dator | Microsoft Docs"
description: "Lär dig hur du återställer lösenordet för ett konto eller Remote Desktop services på en virtuell Windows-dator med hjälp av Azure-portalen eller Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 2e002e3f336422b8fa1eceece889cd083e355a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a><span data-ttu-id="c3eb4-103">Så här återställer du tjänsten Remote Desktop eller dess inloggningslösenord i en Windows VM</span><span class="sxs-lookup"><span data-stu-id="c3eb4-103">How to reset the Remote Desktop service or its login password in a Windows VM</span></span>
<span data-ttu-id="c3eb4-104">Om du inte kan ansluta till en Windows-dator (VM), kan du återställa det lokala administratörslösenordet eller Återställ konfigurationen för tjänsten Remote Desktop.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-104">If you can't connect to a Windows virtual machine (VM), you can reset the local administrator password or reset the Remote Desktop service configuration.</span></span> <span data-ttu-id="c3eb4-105">Du kan använda Azure-portalen eller tillägget för virtuell dator åtkomst i Azure PowerShell för att återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-105">You can use either the Azure portal or the VM Access extension in Azure PowerShell to reset the password.</span></span> <span data-ttu-id="c3eb4-106">Om du använder PowerShell, se till att du har den [senaste PowerShell-modulen installerad och konfigurerad](/powershell/azure/overview) och är inloggad på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-106">If you are using PowerShell, make sure that you have the [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in to your Azure subscription.</span></span> <span data-ttu-id="c3eb4-107">Du kan också [utför de här stegen för virtuella datorer som skapats med den klassiska distributionsmodellen](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="c3eb4-107">You can also [perform these steps for VMs created with the Classic deployment model](reset-rdp.md).</span></span>

## <a name="ways-to-reset-configuration-or-credentials"></a><span data-ttu-id="c3eb4-108">Olika sätt att återställa konfigurationen eller autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="c3eb4-108">Ways to reset configuration or credentials</span></span>
<span data-ttu-id="c3eb4-109">Du kan återställa Remote Desktop services och autentiseringsuppgifter på några olika sätt, beroende på dina behov:</span><span class="sxs-lookup"><span data-stu-id="c3eb4-109">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="c3eb4-110">Återställa med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="c3eb4-110">Reset using the Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="c3eb4-111">Återställa med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3eb4-111">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="c3eb4-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c3eb4-112">Azure portal</span></span>
<span data-ttu-id="c3eb4-113">Om du vill expandera portalmenyn tre staplarna i det övre vänstra hörnet och klicka sedan på **virtuella datorer**:</span><span class="sxs-lookup"><span data-stu-id="c3eb4-113">To expand the portal menu, click the three bars in the upper left corner and then click **Virtual machines**:</span></span>

![Bläddra efter din Azure VM](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="c3eb4-115">**Återställa lösenord för lokala administratörskontot**</span><span class="sxs-lookup"><span data-stu-id="c3eb4-115">**Reset the local administrator account password**</span></span>

<span data-ttu-id="c3eb4-116">Välj din Windows-dator och klicka sedan **stöd + felsökning** > **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-116">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="c3eb4-117">Bladet för återställning av lösenord visas:</span><span class="sxs-lookup"><span data-stu-id="c3eb4-117">The password reset blade is displayed:</span></span>

![Sidan för återställning av lösenord](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

<span data-ttu-id="c3eb4-119">Ange användarnamnet och lösenordet och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-119">Enter the username and a new password, then click **Update**.</span></span> <span data-ttu-id="c3eb4-120">Försök att ansluta till den virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-120">Try connecting to your VM again.</span></span>

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="c3eb4-121">**Återställ konfigurationen för tjänsten Remote Desktop**</span><span class="sxs-lookup"><span data-stu-id="c3eb4-121">**Reset the Remote Desktop service configuration**</span></span>

<span data-ttu-id="c3eb4-122">Välj din Windows-dator och klicka sedan **stöd + felsökning** > **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-122">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="c3eb4-123">Bladet för återställning av lösenord visas.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-123">The password reset blade is displayed.</span></span> 

![Återställ RDP-konfigurationen](./media/reset-rdp/Portal-RM-RDP-Reset.png)

<span data-ttu-id="c3eb4-125">Välj **bara återställa konfigurationen** i den nedrullningsbara menyn Klicka **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-125">Select **Reset configuration only** from the drop-down menu, then click **Update**.</span></span> <span data-ttu-id="c3eb4-126">Försök att ansluta till den virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-126">Try connecting to your VM again.</span></span>


## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="c3eb4-127">VMAccess-tillägget och PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3eb4-127">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="c3eb4-128">Se till att du har den [senaste PowerShell-modulen installerad och konfigurerad](/powershell/azure/overview) och är inloggad på Azure-prenumerationen med den `Login-AzureRmAccount` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-128">Make sure that you have the [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in to your Azure subscription with the `Login-AzureRmAccount` cmdlet.</span></span>

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="c3eb4-129">**Återställa lösenord för lokala administratörskontot**</span><span class="sxs-lookup"><span data-stu-id="c3eb4-129">**Reset the local administrator account password**</span></span>
<span data-ttu-id="c3eb4-130">Återställ administratör lösenord eller användarnamn namn med den [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-130">Reset the administrator password or user name with the [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="c3eb4-131">Skapa dina kontouppgifter enligt följande:</span><span class="sxs-lookup"><span data-stu-id="c3eb4-131">Create your account credentials as follows:</span></span>

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> <span data-ttu-id="c3eb4-132">Om du anger ett annat namn än det aktuella lokala administratörskontot på den virtuella datorn VMAccess-tillägget byter namn på det lokala administratörskontot, tilldelar det angivna lösenordet för kontot och utfärdar en händelse för fjärrskrivbord utloggning.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-132">If you type a different name than the current local administrator account on your VM, the VMAccess extension renames the local administrator account, assigns your specified password to that account, and issues a Remote Desktop logoff event.</span></span> <span data-ttu-id="c3eb4-133">Om det lokala administratörskontot på den virtuella datorn inaktiveras, kan den VMAccess-tillägget.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-133">If the local administrator account on your VM is disabled, the VMAccess extension enables it.</span></span>

<span data-ttu-id="c3eb4-134">I följande exempel uppdateras den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup` till de angivna autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-134">The following example updates the VM named `myVM` in the resource group named `myResourceGroup` to the credentials specified.</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="c3eb4-135">**Återställ konfigurationen för tjänsten Remote Desktop**</span><span class="sxs-lookup"><span data-stu-id="c3eb4-135">**Reset the Remote Desktop service configuration**</span></span>
<span data-ttu-id="c3eb4-136">Återställ fjärråtkomst till den virtuella datorn med den [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-136">Reset remote access to your VM with the [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="c3eb4-137">I följande exempel återställs access-tillägg med namnet `myVMAccess` på den virtuella datorn med namnet `myVM` i den `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="c3eb4-137">The following example resets the access extension named `myVMAccess` on the VM named `myVM` in the `myResourceGroup` resource group:</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> <span data-ttu-id="c3eb4-138">En virtuell dator kan ha endast en enskild VM åtkomst agent när som helst.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-138">At any point, a VM can have only a single VM access agent.</span></span> <span data-ttu-id="c3eb4-139">Att ange den VM agentegenskaper, den `-ForceRerun` alternativet kan användas.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-139">To set the VM access agent properties successfully, the `-ForceRerun` option can be used.</span></span> <span data-ttu-id="c3eb4-140">När du använder `-ForceRerun`, se till att använda samma namn för virtuell dator åtkomst agenten som används i alla tidigare angivna kommandon.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-140">When using `-ForceRerun`, make sure to use the same name for the VM access agent as used in any previous commands.</span></span>

<span data-ttu-id="c3eb4-141">Om du fortfarande inte kan ansluta via en fjärranslutning till den virtuella datorn, se fler steg försöka på [felsöka fjärrskrivbordsanslutningar till en Windows-baserad Azure virtuella](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c3eb4-141">If you still can't connect remotely to your virtual machine, see more steps to try at [Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="next-steps"></a><span data-ttu-id="c3eb4-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3eb4-142">Next steps</span></span>
<span data-ttu-id="c3eb4-143">Om tillägget för virtuell dator i Azure åtkomst svarar inte och det inte går att återställa lösenordet, kan du [återställa det lokala Windows-lösenordet offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c3eb4-143">If the Azure VM access extension does not respond and you are unable to reset the password, you can [reset the local Windows password offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="c3eb4-144">Den här metoden är en process i mer avancerade och du måste ansluta den virtuella hårddisken av problematiska VM till en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-144">This method is a more advanced process and requires you to connect the virtual hard disk of the problematic VM to another VM.</span></span> <span data-ttu-id="c3eb4-145">Följ stegen i den här artikeln först och endast försök offline lösenordsmetoden återställning som en sista utväg.</span><span class="sxs-lookup"><span data-stu-id="c3eb4-145">Follow the steps documented in this article first, and only attempt the offline password reset method as a last resort.</span></span>

[<span data-ttu-id="c3eb4-146">Azure VM-tillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="c3eb4-146">Azure VM extensions and features</span></span>](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="c3eb4-147">Ansluta till en virtuell Azure-dator med RDP eller SSH</span><span class="sxs-lookup"><span data-stu-id="c3eb4-147">Connect to an Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="c3eb4-148">Felsöka anslutningar till fjärrskrivbord till en Windows-baserad Azure virtuell-dator</span><span class="sxs-lookup"><span data-stu-id="c3eb4-148">Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine</span></span>](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

