---
title: "aaaInstall Trend Micro djup Security på en virtuell dator | Microsoft Docs"
description: "Den här artikeln beskriver hur tooinstall och konfigurera Trend Micro säkerhet på en virtuell dator som skapats med hello klassiska distributionsmodellen i Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: dc5492db07a37a2296df5da673183a14c6d5b1f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Hur tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en Windows VM
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Den här artikeln beskrivs hur du tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en ny eller befintlig virtuell dator (VM) med Windows Server. Djupgående säkerhet som en tjänst innehåller skydd mot skadlig kod, en brandvägg, ett intrång förebyggande system och integritet övervakning.

hello-klienten installeras som ett tillägg för säkerhet via hello VM-agenten. På en ny virtuell dator installerar du hello djup säkerhet Agent som hello VM-agenten skapas automatiskt av hello Azure-portalen.

En befintlig virtuell dator skapas med hello klassiska portalen, hello Azure CLI eller PowerShell kanske inte har en VM-agent. För en befintlig virtuell dator som inte har hello VM-agenten måste toodownload och installera den först. Den här artikeln täcker båda situationer.

Om du har någon aktuell prenumeration från Trend Micro för en lokal lösning kan du använda den toohelp skydda din virtuella Azure-datorer. Om du inte är en kund ännu kan registrera du dig för en utvärderingsprenumeration. Mer information om den här lösningen finns hello Trend Micro blogginlägget [Microsoft Azure VM-tillägget för djup säkerhetsgrupper](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-hello-deep-security-agent-on-a-new-vm"></a>Installera hello djup säkerhet Agent på en ny virtuell dator

Hej [Azure-portalen](http://portal.azure.com) kan du installera tillägg för hello Trend Micro säkerhet när du använder en bild från hello **Marketplace** toocreate hello virtuell dator. Om du skapar en virtuell dator är med hjälp av hello portal ett enkelt sätt tooadd skydd från Trend Micro.

Med hjälp av en transaktion från hello **Marketplace** öppnas en guide som hjälper dig att konfigurera hello virtuell dator. Du använder hello **inställningar** bladet, hello tredje panelen hello guiden tooinstall hello Trend Micro säkerhet tillägg.  Allmänna instruktioner finns i [skapa en virtuell dator som kör Windows i hello Azure-portalen](tutorial.md).

När du får toohello **inställningar** bladet hello guiden hello följande steg:

1. Klicka på **tillägg**, klicka på **lägga till tillägget** i hello nästa ruta.

   ![Börja lägga till hello-tillägg][1]

2. Välj **djup säkerhet Agent** i hello **ny resurs** fönstret. I hello djup säkerhet Agent klickar **skapa**.

   ![Identifiera Agent djup säkerhet][2]

3. Ange hello **klient-ID** och **klient aktivering lösenord** för hello tillägg. Alternativt kan du ange en **princip säkerhetsidentifieraren**. Klicka på **OK** tooadd hello-klienten.

   ![Ange information om webbprogramtillägg][3]

## <a name="install-hello-deep-security-agent-on-an-existing-vm"></a>Installera hello djup säkerhet Agent på en befintlig virtuell dator
tooinstall hello-agenten på en befintlig virtuell dator, behöver du hello följande objekt:

* hello Azure PowerShell-modulen, version 0.8.2 eller senare, installeras på den lokala datorn. Du kan kontrollera hello-versionen av Azure PowerShell som du har installerat med hjälp av hello **Get-Module azure | format-table version** kommando. Instruktioner och en länk toohello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). Logga in tooyour Azure-prenumeration med `Add-AzureAccount`.
* hello VM-agenten är installerad på hello virtuella måldatorn.

Kontrollera först att hello VM-agenten redan är installerad. Fyll i hello molntjänstnamnet och namn på virtuell dator och kör sedan följande kommandon i en administratörsnivå Azure PowerShell-kommandotolk hello. Ersätt allt inom hello citattecken, inklusive Hej < och > tecken.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Om du inte vet hello Molntjänsten och namn på virtuell dator, kör **Get-AzureVM** toodisplay att hello information för alla virtuella datorer i din aktuella prenumeration.

Om hello **write-host** kommandot returnerar **SANT**, hello VM-agenten är installerad. Om den returnerar **FALSKT**, se hello instruktioner och en länk toohello hämta i hello Azure blogginlägget [Agent för VM-tillägg - del 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Om hello VM-agenten är installerad kan du köra dessa kommandon.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Nästa steg
Det tar några minuter för hello agent toostart körs när den är installerad. Efter det måste tooactivate djup säkerhet på hello virtuella datorn så att den kan hanteras av en djup Security Manager. Se hello följande artiklar för mer information:

* Trends artikel om den här lösningen [Instant-On Cloud Security för Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* En [exempel på Windows PowerShell-skript](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtuell dator
* [Instruktioner](http://go.microsoft.com/fwlink/?LinkId=404099) hello-exempel

## <a name="additional-resources"></a>Ytterligare resurser
[Hur toolog på tooa virtuell dator som kör Windows Server]

[Azure VM-tillägg och funktioner]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Hur toolog på tooa virtuell dator som kör Windows Server]:connect-logon.md
[Azure VM-tillägg och funktioner]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
