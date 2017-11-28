---
title: "aaaReset hello lösenord eller konfiguration av fjärrskrivbord på en virtuell Windows-dator | Microsoft Docs"
description: "Lär dig hur tooreset lösenordet för ett konto eller Remote Desktop services på en virtuell Windows-dator med hjälp av hello Azure-portalen eller Azure PowerShell."
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
ms.openlocfilehash: 5258df7196621f0adb50debd08dd248922a966de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a><span data-ttu-id="e6514-103">Hur tooreset hello Fjärrskrivbordstjänsten eller dess inloggningslösenord i en Windows VM</span><span class="sxs-lookup"><span data-stu-id="e6514-103">How tooreset hello Remote Desktop service or its login password in a Windows VM</span></span>
<span data-ttu-id="e6514-104">Om du inte kan ansluta tooa Windows virtuell dator (VM), kan du återställa hello lokala administratörslösenordet eller återställa hello Remote Desktop-tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e6514-104">If you can't connect tooa Windows virtual machine (VM), you can reset hello local administrator password or reset hello Remote Desktop service configuration.</span></span> <span data-ttu-id="e6514-105">Du kan använda antingen hello Azure portal eller hello åtkomst för VM-tillägget i Azure PowerShell tooreset hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="e6514-105">You can use either hello Azure portal or hello VM Access extension in Azure PowerShell tooreset hello password.</span></span> <span data-ttu-id="e6514-106">Om du använder PowerShell, se till att du har hello [senaste PowerShell-modulen installerad och konfigurerad](/powershell/azure/overview) och är inloggad tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e6514-106">If you are using PowerShell, make sure that you have hello [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in tooyour Azure subscription.</span></span> <span data-ttu-id="e6514-107">Du kan också [utför de här stegen för virtuella datorer som skapats med hello klassiska distributionsmodellen](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="e6514-107">You can also [perform these steps for VMs created with hello Classic deployment model](reset-rdp.md).</span></span>

## <a name="ways-tooreset-configuration-or-credentials"></a><span data-ttu-id="e6514-108">Sätt tooreset konfiguration eller autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="e6514-108">Ways tooreset configuration or credentials</span></span>
<span data-ttu-id="e6514-109">Du kan återställa Remote Desktop services och autentiseringsuppgifter på några olika sätt, beroende på dina behov:</span><span class="sxs-lookup"><span data-stu-id="e6514-109">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="e6514-110">Återställs via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e6514-110">Reset using hello Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="e6514-111">Återställa med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6514-111">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="e6514-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6514-112">Azure portal</span></span>
<span data-ttu-id="e6514-113">tooexpand hello portal-menyn klickar du på hello tre staplar i hello övre vänstra hörnet och klicka sedan på **virtuella datorer**:</span><span class="sxs-lookup"><span data-stu-id="e6514-113">tooexpand hello portal menu, click hello three bars in hello upper left corner and then click **Virtual machines**:</span></span>

![Bläddra efter din Azure VM](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="e6514-115">**Återställa hello kontolösenord för lokal administratör**</span><span class="sxs-lookup"><span data-stu-id="e6514-115">**Reset hello local administrator account password**</span></span>

<span data-ttu-id="e6514-116">Välj din Windows-dator och klicka sedan **stöd + felsökning** > **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e6514-116">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="e6514-117">hello lösenord Återställ bladet visas:</span><span class="sxs-lookup"><span data-stu-id="e6514-117">hello password reset blade is displayed:</span></span>

![Sidan för återställning av lösenord](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

<span data-ttu-id="e6514-119">Ange hello användarnamn och ett nytt lösenord och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="e6514-119">Enter hello username and a new password, then click **Update**.</span></span> <span data-ttu-id="e6514-120">Försök ansluta igen tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="e6514-120">Try connecting tooyour VM again.</span></span>

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="e6514-121">**Återställ hello tjänstkonfigurationen för fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="e6514-121">**Reset hello Remote Desktop service configuration**</span></span>

<span data-ttu-id="e6514-122">Välj din Windows-dator och klicka sedan **stöd + felsökning** > **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e6514-122">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="e6514-123">hello lösenord Återställ bladet visas.</span><span class="sxs-lookup"><span data-stu-id="e6514-123">hello password reset blade is displayed.</span></span> 

![Återställ RDP-konfigurationen](./media/reset-rdp/Portal-RM-RDP-Reset.png)

<span data-ttu-id="e6514-125">Välj **bara återställa konfigurationen** hello nedrullningsbara menyn och sedan klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="e6514-125">Select **Reset configuration only** from hello drop-down menu, then click **Update**.</span></span> <span data-ttu-id="e6514-126">Försök ansluta igen tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="e6514-126">Try connecting tooyour VM again.</span></span>


## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="e6514-127">VMAccess-tillägget och PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6514-127">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="e6514-128">Se till att du har hello [senaste PowerShell-modulen installerad och konfigurerad](/powershell/azure/overview) och är inloggad tooyour Azure-prenumeration med hello `Login-AzureRmAccount` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e6514-128">Make sure that you have hello [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in tooyour Azure subscription with hello `Login-AzureRmAccount` cmdlet.</span></span>

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="e6514-129">**Återställa hello kontolösenord för lokal administratör**</span><span class="sxs-lookup"><span data-stu-id="e6514-129">**Reset hello local administrator account password**</span></span>
<span data-ttu-id="e6514-130">Återställ Hej administratör lösenord eller användarnamn namn med hello [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e6514-130">Reset hello administrator password or user name with hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="e6514-131">Skapa dina kontouppgifter enligt följande:</span><span class="sxs-lookup"><span data-stu-id="e6514-131">Create your account credentials as follows:</span></span>

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> <span data-ttu-id="e6514-132">Om du anger ett annat namn än hello aktuella lokalt administratörskonto på den virtuella datorn hello VMAccess-tillägget byter hello lokalt administratörskonto, tilldelar kontots lösenord toothat och utfärdar en händelse för fjärrskrivbord utloggning.</span><span class="sxs-lookup"><span data-stu-id="e6514-132">If you type a different name than hello current local administrator account on your VM, hello VMAccess extension renames hello local administrator account, assigns your specified password toothat account, and issues a Remote Desktop logoff event.</span></span> <span data-ttu-id="e6514-133">Om hello lokala administratörskontot på den virtuella datorn inaktiveras, kan den hello VMAccess-tillägget.</span><span class="sxs-lookup"><span data-stu-id="e6514-133">If hello local administrator account on your VM is disabled, hello VMAccess extension enables it.</span></span>

<span data-ttu-id="e6514-134">följande exempel uppdateringar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup` toohello autentiseringsuppgifter som anges.</span><span class="sxs-lookup"><span data-stu-id="e6514-134">hello following example updates hello VM named `myVM` in hello resource group named `myResourceGroup` toohello credentials specified.</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="e6514-135">**Återställ hello tjänstkonfigurationen för fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="e6514-135">**Reset hello Remote Desktop service configuration**</span></span>
<span data-ttu-id="e6514-136">Återställ fjärråtkomst tooyour VM med hello [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e6514-136">Reset remote access tooyour VM with hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="e6514-137">hello följande exempel återställs hello access-tillägg med namnet `myVMAccess` på hello virtuella datorn med namnet `myVM` i hello `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="e6514-137">hello following example resets hello access extension named `myVMAccess` on hello VM named `myVM` in hello `myResourceGroup` resource group:</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> <span data-ttu-id="e6514-138">En virtuell dator kan ha endast en enskild VM åtkomst agent när som helst.</span><span class="sxs-lookup"><span data-stu-id="e6514-138">At any point, a VM can have only a single VM access agent.</span></span> <span data-ttu-id="e6514-139">agentegenskaper för tooset hello VM åtkomst, hello `-ForceRerun` alternativet kan användas.</span><span class="sxs-lookup"><span data-stu-id="e6514-139">tooset hello VM access agent properties successfully, hello `-ForceRerun` option can be used.</span></span> <span data-ttu-id="e6514-140">När du använder `-ForceRerun`, kontrollera toouse hello samma namn för hello VM-åtkomst på agenten som används i alla tidigare angivna kommandon.</span><span class="sxs-lookup"><span data-stu-id="e6514-140">When using `-ForceRerun`, make sure toouse hello same name for hello VM access agent as used in any previous commands.</span></span>

<span data-ttu-id="e6514-141">Om du fortfarande inte kan ansluta via fjärranslutning tooyour virtuell dator, se flera steg tootry på [Felsöka fjärrskrivbord anslutningar tooa Windows-baserad virtuell Azure-dator](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e6514-141">If you still can't connect remotely tooyour virtual machine, see more steps tootry at [Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="next-steps"></a><span data-ttu-id="e6514-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6514-142">Next steps</span></span>
<span data-ttu-id="e6514-143">Om hello access-tillägg för virtuella Azure-datorn svarar inte och är tooreset hello lösenord, kan du [Återställ hello lokala Windows-lösenord offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e6514-143">If hello Azure VM access extension does not respond and you are unable tooreset hello password, you can [reset hello local Windows password offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e6514-144">Den här metoden kräver tooconnect hello virtuell hårddisk av hello problematiska VM tooanother VM är en mer avancerad process.</span><span class="sxs-lookup"><span data-stu-id="e6514-144">This method is a more advanced process and requires you tooconnect hello virtual hard disk of hello problematic VM tooanother VM.</span></span> <span data-ttu-id="e6514-145">Följ hello stegen i den här artikeln först och endast försök hello offline lösenord reset-metoden som en sista utväg.</span><span class="sxs-lookup"><span data-stu-id="e6514-145">Follow hello steps documented in this article first, and only attempt hello offline password reset method as a last resort.</span></span>

[<span data-ttu-id="e6514-146">Azure VM-tillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="e6514-146">Azure VM extensions and features</span></span>](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="e6514-147">Ansluta tooan virtuella Azure-datorn med RDP eller SSH</span><span class="sxs-lookup"><span data-stu-id="e6514-147">Connect tooan Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="e6514-148">Felsöka fjärrskrivbord anslutningar tooa Windows-baserad virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="e6514-148">Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine</span></span>](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

