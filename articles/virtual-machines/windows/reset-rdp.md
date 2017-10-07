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
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Hur tooreset hello Fjärrskrivbordstjänsten eller dess inloggningslösenord i en Windows VM
Om du inte kan ansluta tooa Windows virtuell dator (VM), kan du återställa hello lokala administratörslösenordet eller återställa hello Remote Desktop-tjänstkonfigurationen. Du kan använda antingen hello Azure portal eller hello åtkomst för VM-tillägget i Azure PowerShell tooreset hello lösenord. Om du använder PowerShell, se till att du har hello [senaste PowerShell-modulen installerad och konfigurerad](/powershell/azure/overview) och är inloggad tooyour Azure-prenumeration. Du kan också [utför de här stegen för virtuella datorer som skapats med hello klassiska distributionsmodellen](reset-rdp.md).

## <a name="ways-tooreset-configuration-or-credentials"></a>Sätt tooreset konfiguration eller autentiseringsuppgifter
Du kan återställa Remote Desktop services och autentiseringsuppgifter på några olika sätt, beroende på dina behov:

- [Återställs via hello Azure-portalen](#azure-portal)
- [Återställa med hjälp av Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
tooexpand hello portal-menyn klickar du på hello tre staplar i hello övre vänstra hörnet och klicka sedan på **virtuella datorer**:

![Bläddra efter din Azure VM](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-hello-local-administrator-account-password"></a>**Återställa hello kontolösenord för lokal administratör**

Välj din Windows-dator och klicka sedan **stöd + felsökning** > **Återställ lösenord**. hello lösenord Återställ bladet visas:

![Sidan för återställning av lösenord](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Ange hello användarnamn och ett nytt lösenord och klicka sedan på **uppdatering**. Försök ansluta igen tooyour VM.

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Återställ hello tjänstkonfigurationen för fjärrskrivbord**

Välj din Windows-dator och klicka sedan **stöd + felsökning** > **Återställ lösenord**. hello lösenord Återställ bladet visas. 

![Återställ RDP-konfigurationen](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Välj **bara återställa konfigurationen** hello nedrullningsbara menyn och sedan klicka på **uppdatering**. Försök ansluta igen tooyour VM.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess-tillägget och PowerShell
Se till att du har hello [senaste PowerShell-modulen installerad och konfigurerad](/powershell/azure/overview) och är inloggad tooyour Azure-prenumeration med hello `Login-AzureRmAccount` cmdlet.

### <a name="reset-hello-local-administrator-account-password"></a>**Återställa hello kontolösenord för lokal administratör**
Återställ Hej administratör lösenord eller användarnamn namn med hello [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. Skapa dina kontouppgifter enligt följande:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Om du anger ett annat namn än hello aktuella lokalt administratörskonto på den virtuella datorn hello VMAccess-tillägget byter hello lokalt administratörskonto, tilldelar kontots lösenord toothat och utfärdar en händelse för fjärrskrivbord utloggning. Om hello lokala administratörskontot på den virtuella datorn inaktiveras, kan den hello VMAccess-tillägget.

följande exempel uppdateringar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup` toohello autentiseringsuppgifter som anges.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Återställ hello tjänstkonfigurationen för fjärrskrivbord**
Återställ fjärråtkomst tooyour VM med hello [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. hello följande exempel återställs hello access-tillägg med namnet `myVMAccess` på hello virtuella datorn med namnet `myVM` i hello `myResourceGroup` resursgrupp:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> En virtuell dator kan ha endast en enskild VM åtkomst agent när som helst. agentegenskaper för tooset hello VM åtkomst, hello `-ForceRerun` alternativet kan användas. När du använder `-ForceRerun`, kontrollera toouse hello samma namn för hello VM-åtkomst på agenten som används i alla tidigare angivna kommandon.

Om du fortfarande inte kan ansluta via fjärranslutning tooyour virtuell dator, se flera steg tootry på [Felsöka fjärrskrivbord anslutningar tooa Windows-baserad virtuell Azure-dator](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Nästa steg
Om hello access-tillägg för virtuella Azure-datorn svarar inte och är tooreset hello lösenord, kan du [Återställ hello lokala Windows-lösenord offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Den här metoden kräver tooconnect hello virtuell hårddisk av hello problematiska VM tooanother VM är en mer avancerad process. Följ hello stegen i den här artikeln först och endast försök hello offline lösenord reset-metoden som en sista utväg.

[Azure VM-tillägg och funktioner](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Ansluta tooan virtuella Azure-datorn med RDP eller SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Felsöka fjärrskrivbord anslutningar tooa Windows-baserad virtuell Azure-dator](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

