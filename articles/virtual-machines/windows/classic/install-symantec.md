---
title: "aaaInstall Symantec Endpoint Protection på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera tillägg för hello Symantec Endpoint Protection säkerhet på en ny eller befintlig virtuell Azure-dator som skapats med hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: cb6083366185c26c9f43ff94d835cd90725de5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Hur tooinstall och konfigurera Symantec Endpoint Protection på en Windows VM
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Den här artikeln beskrivs hur du tooinstall och konfigurera hello Symantec Endpoint Protection-klienten på en befintlig virtuell dator (VM) med Windows Server. Fullständig klienten inkluderar tjänster, till exempel virus och spionprogram skydd, brandvägg och intrångsskydd. hello-klienten installeras som ett tillägg för säkerhet via hello VM-agenten.

Om du har en befintlig prenumeration från Symantec för en lokal lösning kan du använda den tooprotect Azure virtuella datorer. Om du inte är en kund ännu kan registrera du dig för en utvärderingsprenumeration. Mer information om den här lösningen finns [Symantec Endpoint Protection på Microsoft Azure-plattformen][Symantec]. Sidan innehåller också länkar toolicensing information och anvisningar för att installera hello klienten om du redan en Symantec-kund.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Installera Symantec Endpoint Protection på en befintlig virtuell dator
Innan du börjar behöver du hello följande:

* hello Azure PowerShell-modulen, version 0.8.2 eller senare på datorn fungerar. Du kan kontrollera hello-versionen av Azure PowerShell som du har installerat med hello **Get-Module azure | format-table version** kommando. Instruktioner och en länk toohello senaste versionen finns [hur tooInstall och konfigurera Azure PowerShell][PS]. Logga in tooyour Azure-prenumeration med `Add-AzureAccount`.
* hello VM-agenten som körs på hello Azure-dator.

Kontrollera först att hello VM-agenten är installerad på hello virtuell dator. Fyll i hello molntjänstnamnet och namn på virtuell dator och kör sedan följande kommandon i en administratörsnivå Azure PowerShell-kommandotolk hello. Ersätt allt inom hello citattecken, inklusive Hej < och > tecken.

> [!TIP]
> Om du inte vet hello Molntjänsten och namn på virtuella datorer, kör **Get-AzureVM** toolist hello namn för alla virtuella datorer i din aktuella prenumeration.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Om hello **write-host** kommando visar **SANT**, hello VM-agenten är installerad. Om det visar **FALSKT**, se hello instruktioner och en länk toohello hämta i hello Azure blogginlägget [Agent för VM-tillägg - del 2][Agent].

Om hello VM-agenten är installerad kan du köra dessa kommandon tooinstall hello Symantec Endpoint Protection-agenten.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

tooverify som hello Symantec security tillägg har installerats och är uppdaterad:

1. Logga in toohello virtuella datorn. Instruktioner finns i [hur tooLog på tooa virtuell dator kör Windows Server][Logon].
2. Windows Server 2008 R2, klickar du på **Start > Symantec Endpoint Protection**. Windows Server 2012 eller Windows Server 2012 R2 från startskärmen hello skriver **Symantec**, och klicka sedan på **Symantec Endpoint Protection**.
3. Från hello **Status** för hello **Status Symantec Endpoint Protection** fönster, tillämpa uppdateringar eller starta om vid behov.

## <a name="additional-resources"></a>Ytterligare resurser
[Hur tooLog på tooa virtuell dator kör Windows Server][Logon]

[Azure VM-tillägg och funktioner][Ext]

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
