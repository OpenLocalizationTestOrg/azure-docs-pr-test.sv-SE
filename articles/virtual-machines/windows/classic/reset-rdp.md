---
title: "aaaReset hello lösenord eller konfiguration av fjärrskrivbord på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur tooreset skapa lösenordet för ett konto eller Remote Desktop services på en virtuell Windows-dator med hello klassisk distribution modellen med hello Azure-portalen eller Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 1721a91fc6c89b46df74e76dfcf918b1c4c77a4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-hello-classic-deployment-model"></a><span data-ttu-id="7c494-103">Hur skapas med hjälp av hello klassiska distributionsmodellen hello tooreset Fjärrskrivbordstjänsten eller dess inloggningslösenord i en Windows VM</span><span class="sxs-lookup"><span data-stu-id="7c494-103">How tooreset hello Remote Desktop service or its login password in a Windows VM created using hello Classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7c494-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7c494-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7c494-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7c494-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="7c494-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="7c494-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="7c494-107">Du kan också [utför de här stegen för virtuella datorer som skapats med hello Resource Manager-distributionsmodellen](../reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="7c494-107">You can also [perform these steps for VMs created with hello Resource Manager deployment model](../reset-rdp.md).</span></span>

<span data-ttu-id="7c494-108">Om du inte kan ansluta tooa Windows virtuell dator (VM), kan du återställa hello lokala administratörslösenordet eller återställa hello Remote Desktop-tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7c494-108">If you can't connect tooa Windows virtual machine (VM), you can reset hello local administrator password or reset hello Remote Desktop service configuration.</span></span> <span data-ttu-id="7c494-109">Du kan använda antingen hello Azure portal eller hello åtkomst för VM-tillägget i Azure PowerShell tooreset hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="7c494-109">You can use either hello Azure portal or hello VM Access extension in Azure PowerShell tooreset hello password.</span></span>

## <a name="ways-tooreset-configuration-or-credentials"></a><span data-ttu-id="7c494-110">Sätt tooreset konfiguration eller autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7c494-110">Ways tooreset configuration or credentials</span></span>
<span data-ttu-id="7c494-111">Du kan återställa Remote Desktop services och autentiseringsuppgifter på några olika sätt, beroende på dina behov:</span><span class="sxs-lookup"><span data-stu-id="7c494-111">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="7c494-112">Återställs via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7c494-112">Reset using hello Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="7c494-113">Återställa med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c494-113">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="7c494-114">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7c494-114">Azure portal</span></span>
<span data-ttu-id="7c494-115">Du kan använda hello [Azure-portalen](https://portal.azure.com) tooreset hello Remote Desktop-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c494-115">You can use hello [Azure portal](https://portal.azure.com) tooreset hello Remote Desktop service.</span></span> <span data-ttu-id="7c494-116">tooexpand hello portal-menyn klickar du på hello tre staplar i hello övre vänstra hörnet och klicka sedan på **virtuella datorer (klassisk)**:</span><span class="sxs-lookup"><span data-stu-id="7c494-116">tooexpand hello portal menu, click hello three bars in hello upper left corner and then click **Virtual machines (classic)**:</span></span>

![Bläddra efter din Azure VM](./media/reset-rdp/Portal-Select-Classic-VM.png)

<span data-ttu-id="7c494-118">Välj din Windows-dator och klicka sedan på **Återställ fjärråtkomst...** . hello följande dialogruta visas tooreset hello fjärrskrivbord konfiguration:</span><span class="sxs-lookup"><span data-stu-id="7c494-118">Select your Windows virtual machine and then click **Reset Remote...**. hello following dialog appears tooreset hello Remote Desktop configuration:</span></span>

![Återställ RDP-konfigurationssidan](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

<span data-ttu-id="7c494-120">Du kan också återställa hello användarnamn och lösenord för hello lokala administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="7c494-120">You can also reset hello username and password of hello local administrator account.</span></span> <span data-ttu-id="7c494-121">Klicka på den virtuella datorn **stöd + felsökning** > **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7c494-121">From your VM, click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="7c494-122">hello lösenord Återställ bladet visas:</span><span class="sxs-lookup"><span data-stu-id="7c494-122">hello password reset blade is displayed:</span></span>

![Sidan för återställning av lösenord](./media/reset-rdp/Portal-PW-Reset-Windows.png)

<span data-ttu-id="7c494-124">När du anger hello nytt användarnamn och lösenord, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7c494-124">After you enter hello new user name and password, click **Save**.</span></span>

## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="7c494-125">VMAccess-tillägget och PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c494-125">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="7c494-126">Kontrollera att hello VM-agenten är installerad på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7c494-126">Make sure hello VM Agent is installed on hello virtual machine.</span></span> <span data-ttu-id="7c494-127">Hej VMAccess-tillägget behöver inte toobe installerad innan du kan använda den, så länge hello VM-Agent är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7c494-127">hello VMAccess extension doesn't need toobe installed before you can use it, as long as hello VM Agent is available.</span></span> <span data-ttu-id="7c494-128">Kontrollera att hello VM-agenten har installerats med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="7c494-128">Verify that hello VM Agent is already installed by using hello following command.</span></span> <span data-ttu-id="7c494-129">(Ersätt ”myCloudService” och ”myVM” av hello namnen på Molntjänsten och den virtuella datorn respektive.</span><span class="sxs-lookup"><span data-stu-id="7c494-129">(Replace "myCloudService" and "myVM" by hello names of your cloud service and your VM, respectively.</span></span> <span data-ttu-id="7c494-130">Du kan lära dig dessa namn genom att köra `Get-AzureVM` utan några parametrar.)</span><span class="sxs-lookup"><span data-stu-id="7c494-130">You can learn these names by running `Get-AzureVM` without any parameters.)</span></span>

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="7c494-131">Om hello **write-host** kommando visar **SANT**, hello VM-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="7c494-131">If hello **write-host** command displays **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="7c494-132">Om det visar **FALSKT**, se hello instruktioner och en länk toohello hämta i hello [Agent för VM-tillägg - del 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="7c494-132">If it displays **False**, see hello instructions and a link toohello download in hello [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blog post.</span></span>

<span data-ttu-id="7c494-133">Om du har skapat hello virtuell dator med hjälp av hello portal, kontrollera om `$vm.GetInstance().ProvisionGuestAgent` returnerar **SANT**.</span><span class="sxs-lookup"><span data-stu-id="7c494-133">If you created hello virtual machine by using hello portal, check whether `$vm.GetInstance().ProvisionGuestAgent` returns **True**.</span></span> <span data-ttu-id="7c494-134">Om inte, kan du ändra det genom att använda det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="7c494-134">If not, you can set it by using this command:</span></span>

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

<span data-ttu-id="7c494-135">Det här kommandot förhindrar hello följande fel när du kör hello **Set AzureVMExtension** i hello nästa steg: ”etablera Gästagenten måste aktiveras på hello VM-objekt innan du anger IaaS-VM Access-tillägg”.</span><span class="sxs-lookup"><span data-stu-id="7c494-135">This command prevents hello following error when you're running hello **Set-AzureVMExtension** command in hello next steps: “Provision Guest Agent must be enabled on hello VM object before setting IaaS VM Access Extension.”</span></span>

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="7c494-136">**Återställa hello kontolösenord för lokal administratör**</span><span class="sxs-lookup"><span data-stu-id="7c494-136">**Reset hello local administrator account password**</span></span>
<span data-ttu-id="7c494-137">Skapar autentiseringsuppgifter inloggning med hello-kontonamnet för aktuell lokal administratör och ett nytt lösenord och kör sedan hello `Set-AzureVMAccessExtension` på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="7c494-137">Create a sign-in credential with hello current local administrator account name and a new password, and then run hello `Set-AzureVMAccessExtension` as follows.</span></span>

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

<span data-ttu-id="7c494-138">Om du anger ett annat namn än hello aktuella kontot hello VMAccess-tillägget byter hello lokalt administratörskonto, tilldelar hello toothat-konto för lösenord och utfärdar ett fjärrskrivbord utloggning. Om hello lokala administratörskontot ska inaktiveras, kan den hello VMAccess-tillägget.</span><span class="sxs-lookup"><span data-stu-id="7c494-138">If you type a different name than hello current account, hello VMAccess extension renames hello local administrator account, assigns hello password toothat account, and issues a Remote Desktop sign-out. If hello local administrator account is disabled, hello VMAccess extension enables it.</span></span>

<span data-ttu-id="7c494-139">Dessa kommandon kan du också återställa hello Remote Desktop-tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7c494-139">These commands also reset hello Remote Desktop service configuration.</span></span>

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="7c494-140">**Återställ hello tjänstkonfigurationen för fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="7c494-140">**Reset hello Remote Desktop service configuration**</span></span>
<span data-ttu-id="7c494-141">tooreset hello fjärrskrivbord tjänstkonfiguration, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7c494-141">tooreset hello Remote Desktop service configuration, run hello following command:</span></span>

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

<span data-ttu-id="7c494-142">Hej VMAccess-tillägget körs två kommandon på hello virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="7c494-142">hello VMAccess extension runs two commands on hello virtual machine:</span></span>

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

<span data-ttu-id="7c494-143">Detta kommando aktiverar hello inbyggda Windows-brandväggen gruppen som tillåter inkommande fjärrskrivbord trafik som använder TCP-port 3389.</span><span class="sxs-lookup"><span data-stu-id="7c494-143">This command enables hello built-in Windows Firewall group that allows incoming Remote Desktop traffic, which uses TCP port 3389.</span></span>

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

<span data-ttu-id="7c494-144">Detta kommando ställer hello fDenyTSConnections registret värdet too0, aktivera anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7c494-144">This command sets hello fDenyTSConnections registry value too0, enabling Remote Desktop connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c494-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c494-145">Next steps</span></span>
<span data-ttu-id="7c494-146">Om hello access-tillägg för virtuella Azure-datorn svarar inte och är tooreset hello lösenord, kan du [Återställ hello lokala Windows-lösenord offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7c494-146">If hello Azure VM access extension does not respond and you are unable tooreset hello password, you can [reset hello local Windows password offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="7c494-147">Den här metoden kräver tooconnect hello virtuell hårddisk av hello problematiska VM tooanother VM är en mer avancerad process.</span><span class="sxs-lookup"><span data-stu-id="7c494-147">This method is a more advanced process and requires you tooconnect hello virtual hard disk of hello problematic VM tooanother VM.</span></span> <span data-ttu-id="7c494-148">Följ hello stegen i den här artikeln först och endast försök hello offline lösenord reset-metoden som en sista utväg.</span><span class="sxs-lookup"><span data-stu-id="7c494-148">Follow hello steps documented in this article first, and only attempt hello offline password reset method as a last resort.</span></span>

[<span data-ttu-id="7c494-149">Azure VM-tillägg och funktioner</span><span class="sxs-lookup"><span data-stu-id="7c494-149">Azure VM extensions and features</span></span>](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="7c494-150">Ansluta tooan virtuella Azure-datorn med RDP eller SSH</span><span class="sxs-lookup"><span data-stu-id="7c494-150">Connect tooan Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="7c494-151">Felsöka fjärrskrivbord anslutningar tooa Windows-baserad virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="7c494-151">Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine</span></span>](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

