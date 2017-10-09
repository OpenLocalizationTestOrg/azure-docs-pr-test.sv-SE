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
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-hello-classic-deployment-model"></a>Hur skapas med hjälp av hello klassiska distributionsmodellen hello tooreset Fjärrskrivbordstjänsten eller dess inloggningslösenord i en Windows VM
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Du kan också [utför de här stegen för virtuella datorer som skapats med hello Resource Manager-distributionsmodellen](../reset-rdp.md).

Om du inte kan ansluta tooa Windows virtuell dator (VM), kan du återställa hello lokala administratörslösenordet eller återställa hello Remote Desktop-tjänstkonfigurationen. Du kan använda antingen hello Azure portal eller hello åtkomst för VM-tillägget i Azure PowerShell tooreset hello lösenord.

## <a name="ways-tooreset-configuration-or-credentials"></a>Sätt tooreset konfiguration eller autentiseringsuppgifter
Du kan återställa Remote Desktop services och autentiseringsuppgifter på några olika sätt, beroende på dina behov:

- [Återställs via hello Azure-portalen](#azure-portal)
- [Återställa med hjälp av Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
Du kan använda hello [Azure-portalen](https://portal.azure.com) tooreset hello Remote Desktop-tjänsten. tooexpand hello portal-menyn klickar du på hello tre staplar i hello övre vänstra hörnet och klicka sedan på **virtuella datorer (klassisk)**:

![Bläddra efter din Azure VM](./media/reset-rdp/Portal-Select-Classic-VM.png)

Välj din Windows-dator och klicka sedan på **Återställ fjärråtkomst...** . hello följande dialogruta visas tooreset hello fjärrskrivbord konfiguration:

![Återställ RDP-konfigurationssidan](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Du kan också återställa hello användarnamn och lösenord för hello lokala administratörskontot. Klicka på den virtuella datorn **stöd + felsökning** > **Återställ lösenord**. hello lösenord Återställ bladet visas:

![Sidan för återställning av lösenord](./media/reset-rdp/Portal-PW-Reset-Windows.png)

När du anger hello nytt användarnamn och lösenord, klickar du på **spara**.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess-tillägget och PowerShell
Kontrollera att hello VM-agenten är installerad på hello virtuell dator. Hej VMAccess-tillägget behöver inte toobe installerad innan du kan använda den, så länge hello VM-Agent är tillgänglig. Kontrollera att hello VM-agenten har installerats med hjälp av följande kommando hello. (Ersätt ”myCloudService” och ”myVM” av hello namnen på Molntjänsten och den virtuella datorn respektive. Du kan lära dig dessa namn genom att köra `Get-AzureVM` utan några parametrar.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Om hello **write-host** kommando visar **SANT**, hello VM-agenten är installerad. Om det visar **FALSKT**, se hello instruktioner och en länk toohello hämta i hello [Agent för VM-tillägg - del 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogginlägg.

Om du har skapat hello virtuell dator med hjälp av hello portal, kontrollera om `$vm.GetInstance().ProvisionGuestAgent` returnerar **SANT**. Om inte, kan du ändra det genom att använda det här kommandot:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Det här kommandot förhindrar hello följande fel när du kör hello **Set AzureVMExtension** i hello nästa steg: ”etablera Gästagenten måste aktiveras på hello VM-objekt innan du anger IaaS-VM Access-tillägg”.

### <a name="reset-hello-local-administrator-account-password"></a>**Återställa hello kontolösenord för lokal administratör**
Skapar autentiseringsuppgifter inloggning med hello-kontonamnet för aktuell lokal administratör och ett nytt lösenord och kör sedan hello `Set-AzureVMAccessExtension` på följande sätt.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Om du anger ett annat namn än hello aktuella kontot hello VMAccess-tillägget byter hello lokalt administratörskonto, tilldelar hello toothat-konto för lösenord och utfärdar ett fjärrskrivbord utloggning. Om hello lokala administratörskontot ska inaktiveras, kan den hello VMAccess-tillägget.

Dessa kommandon kan du också återställa hello Remote Desktop-tjänstkonfigurationen.

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Återställ hello tjänstkonfigurationen för fjärrskrivbord**
tooreset hello fjärrskrivbord tjänstkonfiguration, kör hello följande kommando:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

Hej VMAccess-tillägget körs två kommandon på hello virtuell dator:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Detta kommando aktiverar hello inbyggda Windows-brandväggen gruppen som tillåter inkommande fjärrskrivbord trafik som använder TCP-port 3389.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Detta kommando ställer hello fDenyTSConnections registret värdet too0, aktivera anslutningar till fjärrskrivbord.

## <a name="next-steps"></a>Nästa steg
Om hello access-tillägg för virtuella Azure-datorn svarar inte och är tooreset hello lösenord, kan du [Återställ hello lokala Windows-lösenord offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Den här metoden kräver tooconnect hello virtuell hårddisk av hello problematiska VM tooanother VM är en mer avancerad process. Följ hello stegen i den här artikeln först och endast försök hello offline lösenord reset-metoden som en sista utväg.

[Azure VM-tillägg och funktioner](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Ansluta tooan virtuella Azure-datorn med RDP eller SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Felsöka fjärrskrivbord anslutningar tooa Windows-baserad virtuell Azure-dator](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

