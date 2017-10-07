---
title: aaaCannot ansluta med RDP tooa Windows VM i Azure | Microsoft Docs
description: "Felsökning av problem när du inte kan ansluta tooyour Windows virtuell dator i Azure med hjälp av fjärrskrivbord"
keywords: "Remote desktop-fel, fel anslutning till fjärrskrivbord kan inte ansluta tooVM, felsökning: fjärrskrivbord"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: bbb36136e7a4b187fe8deea621d2b8d46d8aa102
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-remote-desktop-connections-tooan-azure-virtual-machine"></a>Felsöka fjärrskrivbord anslutningar tooan virtuella Azure-datorn
hello Remote Desktop Protocol (RDP) anslutning tooyour Windows-baserad Azure virtuell dator (VM) kan misslyckas av olika skäl, så att du tooaccess den virtuella datorn. hello problemet kan vara med hello Remote Desktop-tjänsten på hello VM, hello nätverksanslutning eller hello fjärrskrivbordsklienten på värddatorn. Den här artikeln hjälper dig igenom några av hello vanligaste metoder tooresolve RDP-anslutningsproblem. 

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Snabbsteg för felsökning
Försöka ansluta igen toohello VM efter varje felsökning steg:

1. Återställ konfigurationen för fjärrskrivbord.
2. Kontrollera Nätverkssäkerhetsgruppen regler / Cloud Services-slutpunkter.
3. Granska loggarna för VM-konsolen.
4. Återställa hello NIC för hello VM.
5. Kontrollera hello VM Resource Health.
6. Återställa lösenordet VM.
7. Starta om den virtuella datorn.
8. Distribuera den virtuella datorn.

Läs om du behöver mer detaljerade anvisningar och förklaringar. Kontrollera den lokala nätverksutrustning som routrar och brandväggar inte blockerar utgående TCP-port 3389, enligt beskrivningen i [detaljerad felsökning RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Om hello **Anslut** knappen för den virtuella datorn är grå ut i hello portal och du inte är ansluten tooAzure via en [Express Route](../../expressroute/expressroute-introduction.md) eller [plats-till-plats VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) anslutning, behöver du toocreate och tilldela den virtuella datorn med en offentlig IP-adress innan du kan använda RDP. Du kan läsa mer om [offentliga IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-tootroubleshoot-rdp-issues"></a>Sätt tootroubleshoot RDP-problem
Du kan felsöka virtuella datorer som skapades med hello Resource Manager-distributionsmodellen med hjälp av en av följande metoder hello:

* [Azure-portalen](#using-the-azure-portal) – bra om du behöver tooquickly återställa hello RDP-konfigurationen eller autentiseringsuppgifter och du inte hello Azure-verktyg installerat.
* [Azure PowerShell](#using-azure-powershell) - om du är nöjd med PowerShell-Kommandotolken snabbt återställa hello RDP konfiguration eller autentiseringsuppgifter, med hjälp av hello Azure PowerShell-cmdlets.

Du kan också hitta steg om felsökning av virtuella datorer som skapats med hjälp av hello [klassiska distributionsmodellen](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-hello-azure-portal"></a>Felsöka med hello Azure-portalen
Försök ansluta tooyour VM igen efter varje steg i felsökningen. Om du fortfarande inte kan ansluta försök hello nästa steg.

1. **Återställer RDP-anslutning**. Den här åtgärden återställer hello RDP-konfigurationen när fjärranslutningar är inaktiverade eller regler för Windows-brandväggen blockerar RDP, t.ex.
   
    Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt hello inställningar fönstret toohello **stöd + felsökning** avsnittet nedre delen av hello-listan. Klicka på hello **Återställ lösenord** knappen. Ange hello **läge** för**bara återställa konfigurationen** och klicka sedan på hello **uppdatering** knappen:
   
    ![Återställ hello RDP-konfigurationen i hello Azure-portalen](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Kontrollera Nätverkssäkerhetsgruppen regler**. Använd [IP-flöde Kontrollera](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm om en regel i en Nätverkssäkerhetsgrupp blockerar trafik tooor från en virtuell dator. Du kan också granska effektiva säkerhetsgrupp regler tooensure inkommande ”Tillåt” NSG regel finns och prioriteras för RDP-porten (standard 3389). Mer information finns i [med effektiva säkerhetsregler tootroubleshoot VM infrastrukturtrafiken rör](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

3. **Granska VM startdiagnostikinställningar**. Den här åtgärden går igenom hello VM konsolen loggar toodetermine om hello VM rapporterar ett problem. Inte alla virtuella datorer har startdiagnostikinställningar aktiverat, så den här åtgärden kan vara valfri.
   
    Specifika felsökningsanvisningar utanför hello omfånget för den här artikeln, men kan tyda på ett större problem som påverkar RDP-anslutning. Mer information om hur du granskar hello loggarna för konsolen och skärmbilden Virtuella finns [Startdiagnostikinställningar för virtuella datorer](boot-diagnostics.md).

4. **Återställ hello NIC för hello VM**. Mer information finns i [hur tooreset NIC för Azure Windows VM](reset-network-interface.md).
5. **Kontrollera hello VM Resource Health**. Den här åtgärden kontrollerar att det inte finns några kända problem med hello Azure-plattformen som kan påverka anslutningen toohello VM.
   
    Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt hello inställningar fönstret toohello **stöd + felsökning** avsnittet nedre delen av hello-listan. Klicka på hello **Resource health** knappen. En felfri VM rapporter som **tillgänglig**:
   
    ![Kontrollera VM resource tillstånd i hello Azure-portalen](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Återställa autentiseringsuppgifter**. Den här åtgärden återställer hello lösenord på ett lokalt administratörskonto när du är osäker på eller har glömt hello autentiseringsuppgifter.
   
    Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt hello inställningar fönstret toohello **stöd + felsökning** avsnittet nedre delen av hello-listan. Klicka på hello **Återställ lösenord** knappen. Se till att hello **läge** har angetts för**Återställ lösenord** och ange ditt användarnamn och ett nytt lösenord. Klicka slutligen på hello **uppdatering** knappen:
   
    ![Återställ hello användarautentiseringsuppgifter i hello Azure-portalen](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Starta om den virtuella datorn**. Den här åtgärden kan korrigera eventuella underliggande problem hello Virtuella datorn har.
   
    Välj den virtuella datorn i hello Azure-portalen och klicka på hello **översikt** fliken. Klicka på hello **starta om** knappen:
   
    ![Starta om hello VM i hello Azure-portalen](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Distribuera den virtuella datorn**. Den här åtgärden återdistribuerar VM tooanother-värden i Azure toocorrect eventuella underliggande problem plattform eller nätverk.
   
    Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt hello inställningar fönstret toohello **stöd + felsökning** avsnittet nedre delen av hello-listan. Klicka på hello **omdistribuera** knappen och klicka sedan på **omdistribuera**:
   
    ![Distribuera om hello VM i hello Azure-portalen](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    När den här åtgärden har slutförts är tillfälliga diskdata förlorade och dynamiska IP-adresser som är associerade med hello VM har uppdaterats.

Om det fortfarande uppstår problem med RDP, kan du [öppna en supportbegäran](https://azure.microsoft.com/support/options/) eller läsa [mer detaljerad felsökning koncept och steg RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Felsöka med hjälp av Azure PowerShell
Om du inte redan har gjort [installera och konfigurera hello senaste Azure PowerShell](/powershell/azure/overview).

hello exemplen använder variabler som `myResourceGroup`, `myVM`, och `myVMAccessExtension`. Ersätt dessa variabelnamn och platser med egna värden.

> [!NOTE]
> Du återställer hello användaruppgifter och hello RDP-konfigurationen med hjälp av hello [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. I följande exempel, hello `myVMAccessExtension` är ett namn som du anger som en del av hello-processen. Om du tidigare har arbetat med hello VMAccessAgent, kan du få hello namnet på befintliga hello-tillägget med hjälp av `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` toocheck hello egenskaper för hello VM. tooview hello namn, leta under avsnittet hello tillägg av hello utdata.

Försök ansluta tooyour VM igen efter varje steg i felsökningen. Om du fortfarande inte kan ansluta försök hello nästa steg.

1. **Återställer RDP-anslutning**. Den här åtgärden återställer hello RDP-konfigurationen när fjärranslutningar är inaktiverade eller regler för Windows-brandväggen blockerar RDP, t.ex.
   
    hello följande exempel återställer hello RDP-anslutning på en virtuell dator med namnet `myVM` i hello `WestUS` plats och i hello resursgrupp med namnet `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Kontrollera Nätverkssäkerhetsgruppen regler**. Den här åtgärden kontrollerar du att en regel i Nätverkssäkerhetsgruppen toopermit RDP-trafik. hello-standardporten för RDP är TCP-port 3389. En regel toopermit RDP-trafik kan inte skapas automatiskt när du skapar den virtuella datorn.
   
    Först tilldela alla hello konfigurationsdata för Nätverkssäkerhetsgruppen-toohello `$rules` variabeln. hello följande exempel hämtar information om hello säkerhetsgrupp för nätverk med namnet `myNetworkSecurityGroup` i hello resursgrupp med namnet `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Nu kan visa hello regler som har konfigurerats för den här Nätverkssäkerhetsgruppen. Kontrollera att det finns en regel tooallow TCP-port 3389 för inkommande anslutningar på följande sätt:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    hello visas följande exempel en giltig säkerhetsregel som tillåter RDP-trafik. Du kan se `Protocol`, `DestinationPortRange`, `Access`, och `Direction` är korrekt konfigurerade:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    Om du inte har en regel som tillåter RDP-trafik [skapa en regel för Nätverkssäkerhetsgruppen](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tillåt TCP-port 3389.
3. **Återställa autentiseringsuppgifter**. Den här åtgärden återställer hello lösenord på hello lokala administratörskontot som du anger när du är osäker på eller har glömt hello autentiseringsuppgifter.
   
    Ange först hello användarnamn och ett nytt lösenord genom att tilldela inloggningsinformation toohello `$cred` variabeln på följande sätt:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Uppdatera nu hello autentiseringsuppgifter på den virtuella datorn. hello följande exempel uppdateras hello autentiseringsuppgifter på en virtuell dator med namnet `myVM` i hello `WestUS` plats och i hello resursgrupp med namnet `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Starta om den virtuella datorn**. Den här åtgärden kan korrigera eventuella underliggande problem hello Virtuella datorn har.
   
    följande exempel omstarter hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:
   
    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Distribuera den virtuella datorn**. Den här åtgärden återdistribuerar VM tooanother-värden i Azure toocorrect eventuella underliggande problem plattform eller nätverk.
   
    följande exempel återdistribuerar hello hello virtuella datorn med namnet `myVM` i hello `WestUS` plats och i hello resursgrupp med namnet `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Om det fortfarande uppstår problem med RDP, kan du [öppna en supportbegäran](https://azure.microsoft.com/support/options/) eller läsa [mer detaljerad felsökning koncept och steg RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-hello-classic-deployment-model"></a>Felsökning av virtuella datorer som skapats med hjälp av hello klassiska distributionsmodellen
Försöka ansluta igen toohello VM efter varje steg i felsökningen.

1. **Återställer RDP-anslutning**. Den här åtgärden återställer hello RDP-konfigurationen när fjärranslutningar är inaktiverade eller regler för Windows-brandväggen blockerar RDP, t.ex.
   
    Välj den virtuella datorn i hello Azure-portalen. Klicka på hello **... Flera** knappen och klicka sedan på **Återställ fjärråtkomst**:
   
    ![Återställ hello RDP-konfigurationen i hello Azure-portalen](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Verifiera molntjänster slutpunkter**. Den här åtgärden kontrollerar du att slutpunkter i Cloud Services toopermit RDP-trafik. hello-standardporten för RDP är TCP-port 3389. En regel toopermit RDP-trafik kan inte skapas automatiskt när du skapar den virtuella datorn.
   
   Välj den virtuella datorn i hello Azure-portalen. Klicka på hello **slutpunkter** knappen tooview hello slutpunkter som för tillfället konfigurerats för den virtuella datorn. Kontrollera att slutpunkter finns som tillåter RDP-trafik på TCP-port 3389.
   
   hello som följande exempel visar giltig slutpunkter som tillåter RDP-trafik:
   
   ![Verifiera molntjänster slutpunkter i hello Azure-portalen](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   Om du inte har en slutpunkt som tillåter RDP-trafik [skapa en slutpunkt för molntjänster](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Tillåt TCP tooprivate port 3389.
3. **Granska VM startdiagnostikinställningar**. Den här åtgärden går igenom hello VM konsolen loggar toodetermine om hello VM rapporterar ett problem. Inte alla virtuella datorer har startdiagnostikinställningar aktiverat, så den här åtgärden kan vara valfri.
   
    Specifika felsökningsanvisningar utanför hello omfånget för den här artikeln, men kan tyda på ett större problem som påverkar RDP-anslutning. Mer information om hur du granskar hello loggarna för konsolen och skärmbilden Virtuella finns [Startdiagnostikinställningar för virtuella datorer](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **Kontrollera hello VM Resource Health**. Den här åtgärden kontrollerar att det inte finns några kända problem med hello Azure-plattformen som kan påverka anslutningen toohello VM.
   
    Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt hello inställningar fönstret toohello **stöd + felsökning** avsnittet nedre delen av hello-listan. Klicka på hello **Resource Health** knappen. En felfri VM rapporter som **tillgänglig**:
   
    ![Kontrollera VM resource tillstånd i hello Azure-portalen](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Återställa autentiseringsuppgifter**. Den här åtgärden återställer hello lösenord på hello lokalt administratörskonto att du anger när du är osäker på eller har glömt hello autentiseringsuppgifter.
   
    Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt hello inställningar fönstret toohello **stöd + felsökning** avsnittet nedre delen av hello-listan. Klicka på hello **Återställ lösenord** knappen. Ange ditt användarnamn och ett nytt lösenord. Klicka slutligen på hello **spara** knappen:
   
    ![Återställ hello användarautentiseringsuppgifter i hello Azure-portalen](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Starta om den virtuella datorn**. Den här åtgärden kan korrigera eventuella underliggande problem hello Virtuella datorn har.
   
    Välj den virtuella datorn i hello Azure-portalen och klicka på hello **översikt** fliken. Klicka på hello **starta om** knappen:
   
    ![Starta om hello VM i hello Azure-portalen](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

Om det fortfarande uppstår problem med RDP, kan du [öppna en supportbegäran](https://azure.microsoft.com/support/options/) eller läsa [mer detaljerad felsökning koncept och steg RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Felsöka specifika RDP-fel
Du kan stöta på ett visst felmeddelande vid tooconnect tooyour virtuella datorn via RDP. hello följande är de vanligaste felmeddelanden för hello:

* [hello Fjärrsessionen kopplades från eftersom det finns inga tillgängliga servrar för fjärrskrivbordslicenser tooprovide en licens](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Fjärrskrivbordet kan inte hitta hello ”datornamn”](troubleshoot-specific-rdp-errors.md#rdpname).
* [Ett autentiseringsfel har uppstått. hello lokala säkerhetskontrollen går inte att kontakta](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Windows-Security-fel: dina autentiseringsuppgifter fungerar inte](troubleshoot-specific-rdp-errors.md#wincred).
* [Den här datorn kan inte ansluta toohello fjärrdatorn](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Ytterligare resurser
Om ingen av dessa fel uppstod och du kan fortfarande inte kan ansluta toohello virtuella datorn via fjärrskrivbord, läsa hello detaljerad [felsökningsguide för fjärrskrivbord](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Felsökningssteg för åtkomst till program som körs på en virtuell dator, finns i [Felsök åtkomst tooan program som körs på en Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Om du har problem med hjälp av SSH (Secure Shell) tooconnect tooa Linux VM i Azure, se [felsökning av SSH-anslutningar tooa Linux VM i Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

