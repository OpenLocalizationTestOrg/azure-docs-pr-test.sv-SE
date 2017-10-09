---
title: "aaaAzure översikt över virtuella | Microsoft Docs"
description: "Översikt över Azure-dator"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: nepeters
ms.openlocfilehash: 178766925673419cd661dbb460b8427bbfaf54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-agent-overview"></a>Översikt över Azure virtuella datorns Agent

hello virtuella Microsoft Azure-agenten (VM-Agent) är en säker, enkel process som hanterar VM interaktion med hello Azure-Infrastrukturkontrollanten. hello VM-agenten har en primär roll i att aktivera och köra tillägg för virtuell dator i Azure. VM-tillägg som aktiverar efter distributionskonfigurationen av virtuella datorer, till exempel installera och konfigurera programvara. Tillägg för virtuell dator kan du även aktivera återställningsfunktioner, till exempel återställer hello administratörslösenord för en virtuell dator. Tillägg för virtuell dator utan hello Azure VM-agenten kan inte köras.

Det här dokumentet beskriver installation, identifiering och borttagning av hello Azure Virtual Machine-agenten.

## <a name="install-hello-vm-agent"></a>Installera hello VM-Agent

### <a name="azure-gallery-image"></a>Bild av Azure-galleriet

hello Azure VM-agenten installeras som standard på alla Windows-dator som distribueras från en avbildning i Azure-galleriet. När du distribuerar en Azure-galleriet bild från hello Portal, PowerShell, kommandoradsgränssnittet eller en Azure Resource Manager-mall installeras hello Azure VM-agenten är också. 

### <a name="manual-installation"></a>Manuell installation

hello Windows VM-agenten kan installeras manuellt med hjälp av Windows installer-paket. Manuell installation kan vara nödvändigt när du skapar en virtuell datoravbildning som ska distribueras i Azure. toomanually installera hello Windows VM-agenten, hämta installationsprogrammet för hello VM-agenten från den här platsen [Windows Azure VM-agenten hämta](http://go.microsoft.com/fwlink/?LinkID=394789). 

hello VM-agenten kan installeras genom att dubbelklicka på hello windows installer-filen. Kör hello följande kommando för en automatiserad eller obevakad installation av hello VM-agenten.

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-hello-vm-agent"></a>Identifiera hello VM-Agent

### <a name="powershell"></a>PowerShell

hello Azure Resource Manager PowerShell-modulen kan vara används tooretrieve information om Azure virtuella datorer. Kör `Get-AzureRmVM` returnerar lite inklusive hello Etableringsstatus för hello Azure VM-agenten.

```PowerShell
Get-AzureRmVM
```

hello följande är bara en delmängd av hello `Get-AzureRmVM` utdata. Meddelande hello `ProvisionVMAgent` egenskapen inuti `OSProfile`, den här egenskapen kan vara används toodetermine om hello VM-agenten har distribuerade toohello virtuella datorn.

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Följande skript hello kan vara används tooreturn en kortfattad lista över namn på virtuella datorer och hello tillståndet för hello VM-agenten.

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Manuell identifiering

När du loggade in tooa Windows Azure VM vara Aktivitetshanteraren används tooexamine processer som körs. toocheck för hello Azure VM-agenten, öppna Aktivitetshanteraren > Klicka på fliken för hello information och leta efter ett processnamn `WindowsAzureGuestAgent.exe`. hello förekomst av den här processen anger den hello VM-agenten är installerad.

## <a name="upgrade-hello-vm-agent"></a>Uppgradera hello VM-Agent

hello Azure VM-agenten för Windows uppgraderas automatiskt. Nya virtuella datorer är distribuerade tooAzure, kan de få hello senaste VM-agenten. Anpassade VM-avbildningar ska vara uppdaterade tooinclude hello nya VM-agenten.
