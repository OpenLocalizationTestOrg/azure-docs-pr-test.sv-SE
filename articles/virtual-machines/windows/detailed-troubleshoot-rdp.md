---
title: "aaaDetailed fjärrskrivbord felsökning i Azure | Microsoft Docs"
description: "Granska detaljerad felsökning av remote desktop fel där du kan inte tooa virtuella Windows-datorer i Azure"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: "Det går inte att ansluta tooremote skrivbordet, Felsöka fjärrskrivbord fjärrskrivbord kan inte ansluta skrivbord fjärrfel, felsökning: fjärrskrivbord, remote desktop problem"
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: fcb0d06aa66b748f3ebbbbe3431471d3cbe7c60d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-toowindows-vms-in-azure"></a>Detaljerad felsökning för anslutning till fjärrskrivbord utfärdar tooWindows virtuella datorer i Azure
Den här artikeln innehåller detaljerad felsökning toodiagnose och fixa komplexa Remote Desktop-fel för Windows-baserade virtuella Azure-datorer.

> [!IMPORTANT]
> tooeliminate hello vanliga Remote Desktop-fel, kontrollera att tooread [hello grundläggande felsökningsartikel för fjärrskrivbord](troubleshoot-rdp-connection.md) innan du fortsätter.

Du kan stöta på ett felmeddelande som inte överensstämmer med någon av hello felmeddelanden som beskrivs i Fjärrskrivbord [hello grundläggande fjärrskrivbord felsökningsguide för](troubleshoot-rdp-connection.md). Följ dessa steg toodetermine varför hello RDP (Remote Desktop) klienten är tooconnect toohello RDP-tjänsten på hello Azure VM.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och på **Get Support**. Information om hur du använder Azure-supporten finns hello [Microsoft Azure Support FAQ](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Komponenter i en fjärrskrivbordsanslutning
hello följande komponenter ingår i en RDP-anslutning:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Innan du fortsätter kan det hjälpa toomentally granska vad som har ändrats sedan hello senaste lyckade Fjärrskrivbord anslutning toohello VM. Exempel:

* Hej hello VM eller hello tjänst i molnet som innehåller hello VM offentliga IP-adress (kallas även hello virtuella IP-adressen [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) har ändrats. hello RDP-fel kan bero på att din DNS-klientens cacheminne har fortfarande hello *gamla IP-adressen* registrerats för hello DNS-namn. Rensa din DNS-klientens cacheminne och försök ansluta igen hello VM. Eller försök ansluta direkt med hello ny VIP.
* Du använder ett program från tredje part toomanage anslutningar till fjärrskrivbord i stället för att använda hello anslutning som genererats av hello Azure-portalen. Kontrollera att programmet hello-konfigurationen innehåller hello rätt TCP-port för hello Remote Desktop-trafik. Du kan kontrollera den här porten för en klassisk virtuell dator i hello [Azure-portalen](https://portal.azure.com), genom att klicka på hello VM Inställningar > slutpunkter.

## <a name="preliminary-steps"></a>Preliminära steg
Innan du fortsätter toohello detaljerad felsökning,

* Kontrollera hello status hello virtuell dator i hello Azure portal för eventuella uppenbara problem.
* Följ hello [snabbkorrigering anvisningar för vanliga RDP-fel i grundläggande felsökning av hello guide](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).

Försök att ansluta igen toohello virtuella datorn via fjärrskrivbord efter de här stegen.

## <a name="detailed-troubleshooting-steps"></a>Detaljerade felsökningsanvisningar
hello fjärrskrivbordsklienten kanske inte kan tooreach hello Remote Desktop-tjänsten på hello Azure VM på grund av tooissues på hello följande källor:

* [Fjärrskrivbordsklient dator](#source-1-remote-desktop-client-computer)
* [Gränsenheten för organisationen intranät](#source-2-organization-intranet-edge-device)
* [Molnet tjänstslutpunkten och åtkomstkontrollista (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Nätverkssäkerhetsgrupper](#source-4-network-security-groups)
* [Windows-baserade Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>: 1 fjärrskrivbordsklienten källdator
Kontrollera att datorn kan göra Fjärrskrivbord anslutningar tooanother lokalt, Windows-baserad dator.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Om det går inte att kontrollera hello följande inställningar på datorn:

* En inställning för lokal brandvägg som blockerar trafik för fjärrskrivbord.
* Lokalt installerat klientprogramvaran för proxy som hindrar anslutningar till fjärrskrivbord.
* Lokalt installerad programvara som förhindrar anslutning till fjärrskrivbord för nätverksövervakning.
* Andra typer av säkerhetsprogram som övervaka trafik eller tillåta/neka vissa typer av trafik som förhindrar anslutning till fjärrskrivbord.

I dessa fall kan tillfälligt inaktivera hello programvara och försök tooconnect tooan lokala dator via fjärrskrivbord. Om det här sättet kan ta reda på hello faktiska orsak kan fungera med din administratör toocorrect hello programvara inställningar tooallow fjärrskrivbord nätverksanslutningar.

## <a name="source-2-organization-intranet-edge-device"></a>Källan 2: Gränsenheten organisation intranät
Kontrollera att en dator direkt ansluten toohello Internet kan göra Fjärrskrivbord anslutningar tooyour virtuella Azure-datorn.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Om du inte har en dator som är direkt ansluten toohello Internet, skapa och testa med en ny Azure virtuell dator i en resurs eller molnomfång tjänst. Mer information finns i [skapa en virtuell dator som kör Windows i Azure](../virtual-machines-windows-hero-tutorial.md). Du kan ta bort hello virtuell dator och hello resursgrupp eller hello molntjänst när hello testa.

Om du kan skapa en fjärrskrivbordsanslutning med en dator som är direkt ansluten toohello Internet, kontrollera din organisation intranät insticksenhet för:

* En intern brandvägg blockerar HTTPS-anslutningar toohello Internet.
* En proxyserver som förhindrar anslutning till fjärrskrivbord.
* Intrångsidentifiering eller program som körs på enheter i nätverket kant som förhindrar anslutning till fjärrskrivbord för nätverksövervakning.

Arbeta med din administratör toocorrect hello nätverksinställningarna för din organisation intranät edge enheten tooallow HTTPS-baserade Fjärrskrivbord anslutningar toohello Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Källa 3: Molnet tjänstslutpunkten och ACL
För virtuella datorer som skapades med hjälp av hello klassiska distributionsmodellen, kontrollera att en annan virtuell Azure-dator som är i hello samma molntjänst eller virtuella nätverk kan du Fjärrskrivbord anslutningar tooyour Azure VM.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> För virtuella datorer som skapats i Resource Manager, hoppar du över för[källa 4: Nätverkssäkerhetsgrupper](#source-4-network-security-groups).

Om du inte har en annan dator i Hej samma molntjänst eller virtuella nätverk, skapar du en. Gör så hello i [skapa en virtuell dator som kör Windows i Azure](../virtual-machines-windows-hero-tutorial.md). Ta bort hello testa virtuell dator när hello test har slutförts.

Om du kan ansluta via fjärrskrivbord tooa virtuell dator i hello samma cloud service eller virtuella nätverk, söka efter dessa inställningar:

* hello slutpunktskonfigurationen för Remote Desktop-trafik på hello målet VM: hello privata TCP-port för hello slutpunkten måste matcha hello TCP-port på vilken hello Virtuella datorns fjärrskrivbord tjänsten lyssnar (standard är 3389).
* hello ACL för hello fjärrskrivbord trafik slutpunkt på hello målet VM: ACL: er kan du toospecify tillåts eller nekas inkommande trafik från hello Internet baserat på dess IP-adress. Felkonfigurerad ACL: er kan förhindra att inkommande fjärrskrivbord trafik toohello slutpunkt. Kontrollera din ACL: er tooensure som inkommande trafik från din offentliga IP-adresser i din proxyserver eller andra gränsservern tillåts. Mer information finns i [vad är ett nätverk åtkomstkontrollista (ACL)?](../../virtual-network/virtual-networks-acl.md)

toocheck om hello-slutpunkten är hello källan hello problemet, ta bort hello aktuella slutpunkt och skapa en ny, välja en slumpmässigt vald port i intervallet för hello 49152 – 65535 för hello Externt portnummer. Mer information finns i [hur tooset slutpunkter tooa virtuella datorn](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>Datakällan 4: Nätverkssäkerhetsgrupper
Nätverkssäkerhetsgrupper kan mer detaljerad kontroll över tillåtna inkommande och utgående trafik. Du kan skapa regler som utsträckning undernät och molntjänster i Azure-nätverk.

Använd [IP-flöde Kontrollera](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm om en regel i en Nätverkssäkerhetsgrupp blockerar trafik tooor från en virtuell dator. Du kan också granska effektiva säkerhetsgrupp regler tooensure inkommande ”Tillåt” NSG regel finns och prioriteras för RDP-porten (standard 3389). Mer information finns i [med effektiva säkerhetsregler tootroubleshoot VM infrastrukturtrafiken rör](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

## <a name="source-5-windows-based-azure-vm"></a>Källa 5: Windows-baserade Azure VM
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

Följ anvisningarna för hello i [i den här artikeln](reset-rdp.md). Den här artikeln återställs hello Remote Desktop-tjänsten på hello virtuell dator:

* Aktivera Standardregeln för hello ”Remote Desktop” Windows-brandväggen (TCP-port 3389).
* Aktivera anslutning till fjärrskrivbord genom att ange hello HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections registret värdet too0.

Försök hello anslutningen från datorn igen. Om du ändå inte kan tooconnect via fjärrskrivbord kan du söka efter hello följande möjliga problem:

* hello Remote Desktop-tjänsten körs inte på hello målet VM.
* hello Remote Desktop-tjänsten lyssnar inte på TCP-port 3389.
* Windows-brandväggen eller en annan lokal brandvägg har en utgående regel som hindrar Remote Desktop-trafik.
* Intrångsidentifiering eller program som körs på hello Azure-dator för nätverksövervakning förhindrar anslutning till fjärrskrivbord.

För virtuella datorer skapas med hello klassiska distributionsmodellen kan använda du en fjärransluten Azure PowerShell-session toohello virtuella Azure-datorn. Du måste först, tooinstall ett certifikat för hello virtuella datorns värd Molntjänsten. Gå för[konfigurera Secure PowerShell fjärråtkomst tooAzure virtuella datorer](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) och hämta hello **InstallWinRMCertAzureVM.ps1** skriptet filen tooyour lokal dator.

Installera Azure PowerShell om du inte redan har gjort. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

Öppna en Azure PowerShell-kommandotolk och ändra hello aktuella toohello sökvägen för hello **InstallWinRMCertAzureVM.ps1** skriptfilen. Du måste ange hello rätt körningsprincipen toorun Azure PowerShell-skript. Kör hello **Get-ExecutionPolicy** kommandot toodetermine din aktuella principnivån. Information om hur du anger rätt nivå för hello finns [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Därefter Fyll i din Azure-prenumeration, hello molntjänstnamnet, och din virtuella namn (ta bort Hej < och > tecken) och kör sedan följande kommandon.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of hello cloud service that contains hello target virtual machine>"
$vmName="<Name of hello target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

Du kan hämta hello rätt prenumerationsnamn från hello *SubscriptionName* -egenskapen för hello visningen av hello **Get-AzureSubscription** kommando. Du kan hämta hello molntjänstnamnet för hello virtuell dator från hello *ServiceName* kolumn i hello visningen av hello **Get-AzureVM** kommando.

Kontrollera att du har hello nytt certifikat. Öppna en snapin-modulen för certifikat för den aktuella användaren i hello och titta i hello **Trusted Root Certification Authorities\Certificates** mapp. Du bör se ett certifikat med hello DNS-namnet på Molntjänsten i hello utfärdat toocolumn (exempel: cloudservice4testing.cloudapp.net).

Sedan initiera Azure PowerShell-fjärrsession med hjälp av dessa kommandon.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

När du har angett giltiga autentiseringsuppgifter, bör du se något liknande toohello följande Azure PowerShell-prompten:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

hello första delen av det här meddelandet är din molntjänstnamnet som innehåller hello mål VM, vilket kan skilja sig från ”cloudservice4testing.cloudapp.net”. Du kan nu utfärda Azure PowerShell-kommandon för det här molnet tooinvestigate hello serviceproblem nämns och korrigera hello konfiguration.

### <a name="toomanually-correct-hello-remote-desktop-services-listening-tcp-port"></a>toomanually rätt hello Fjärrskrivbordstjänster lyssnande TCP-port
Kör kommandot i Kommandotolken hello remote Azure PowerShell-session.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

hello PortNumber egenskapen visar hello aktuella portnumret. Om det behövs ändrar du hello fjärrskrivbord port number tillbaka tooits standardvärdet (port 3389) med hjälp av det här kommandot.

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Kontrollera att hello-porten har ändrats too3389 med hjälp av det här kommandot.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Avsluta hello Azure PowerShell-fjärrsession med hjälp av det här kommandot.

```powershell
Exit-PSSession
```

Kontrollera att hello fjärrskrivbord slutpunkt för hello Azure VM också använder TCP-port 3398 som dess Intern port. Starta om hello Azure VM och försök hello fjärrskrivbordsanslutning igen.

## <a name="additional-resources"></a>Ytterligare resurser
[Hur tooreset ett lösenord eller hello fjärrskrivbord service för Windows-datorer](reset-rdp.md)

[Hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview)

[Felsökning av SSH (Secure Shell) anslutningar tooa Linux-baserade virtuell Azure-dator](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Felsöka åtkomst tooan program som körs på en virtuell Azure-dator](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

