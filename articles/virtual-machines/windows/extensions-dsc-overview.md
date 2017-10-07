---
title: "aaaDesired tillståndskonfigurationen Azure översikt | Microsoft Docs"
description: "Översikt för att använda hello Microsoft Azure-tillägget hanterare för PowerShell Desired State Configuration. Inklusive krav, arkitektur, cmdlets..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: b0337a2f1124f35e5e40c1478bd7530427e59d44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-desired-state-configuration-extension-handler"></a>Introduktion toohello Azure Desired State Configuration-tillägget hanterare
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

hello Azure VM-agenten och dess associerade tillägg är en del av hello Microsoft Azure Infrastructure Services. VM-tillägg är programkomponenter som utökar hello VM funktionerna och förenkla olika VM-hanteringsåtgärder. Till exempel hello VMAccess-tillägget kan vara används tooreset administratörens lösenord eller hello anpassat skript tillägget kan vara används tooexecute ett skript på hello VM.

Den här artikeln introducerar hello PowerShell önskad tillstånd Configuration DSC ()-tillägg för virtuella datorer i Azure som en del av hello Azure PowerShell SDK. Du kan använda nya cmdlet: ar tooupload och tillämpa en PowerShell DSC-konfiguration på en Azure VM aktiverad med hello PowerShell DSC-tillägg. hello PowerShell DSC-tillägg-anrop till PowerShell DSC tooenact hello emot DSC-konfigurationen på hello VM. Denna funktion är också tillgängliga via hello Azure-portalen.

## <a name="prerequisites"></a>Krav
**Lokal dator** toointeract med hello Azure VM-tillägget, du behöver toouse antingen hello Azure-portalen eller hello Azure PowerShell SDK. 

**Gästagenten** hello Azure VM som konfigurerats av hello DSC-konfigurationen måste toobe ett operativsystem som stöder antingen Windows Management Framework (WMF) 4.0 eller 5.0. hello fullständig lista över operativsystemversioner som stöds finns på hello [DSC-tillägg versionshistorik](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termer och begrepp
Den här handboken förutsätter förtrogenhet med hello följande begrepp:

Konfiguration – ett dokument för DSC-konfigurationen. 

Nod - mål för DSC-konfigurationen. I det här dokumentet refererar ”nod” alltid tooan Azure VM.

Konfigurationsdata - en .psd1 fil som innehåller miljödata för ett konfigurationsobjekt

## <a name="architectural-overview"></a>Översikt över arkitekturen
hello Azure DSC-tillägg använder hello Azure VM-agenten framework toodeliver, tillämpar och rapportera om DSC-konfigurationer som körs på virtuella Azure-datorer. hello DSC-tillägg förväntar sig en .zip-fil som innehåller minst ett konfigurationsdokument och en uppsättning parametrar som tillhandahålls via hello Azure PowerShell SDK eller hello Azure-portalen.

När tillägget hello anropas för hello första gången, kör installationen. Den här processen installerar en version av hello Windows Management Framework (WMF) med hjälp av följande logik hello:

1. Om Windows Server 2016 hello Azure VM OS är utförs ingen åtgärd. Windows Server 2016 har redan hello senaste versionen av PowerShell som är installerad.
2. Om hello `wmfVersion` egenskapen har angetts, den versionen av hello WMF har installerats om det inte är inte kompatibel med hello VM OS.
3. Om inget `wmfVersion` egenskapen har angetts, hello senaste tillämpliga versionen av hello WMF har installerats.

Installation av hello WMF krävs en omstart. Efter omstart hello tillägget hämtar hello ZIP-fil som anges i hello `modulesUrl` egenskapen. Om den här platsen är i Azure blob storage, en SAS-token kan anges i hello `sasToken` egenskapen tooaccess hello filen. När hello .zip hämtas och packat upp, hello configuration-funktion som definierats i `configurationFunction` körs toogenerate hello MOF-filen. hello-tillägg körs sedan `Start-DscConfiguration -Force` på hello genereras MOF-filen. hello tillägget avbildas utdata och skriver ut toohello Azure Status kanal. Från och med nu på hello hanterar DSC MGM övervakning och korrigering som vanligt. 

## <a name="powershell-cmdlets"></a>PowerShell-cmdletar
PowerShell-cmdlets kan användas med Azure Resource Manager hello klassisk distribution modellen toopackage, publicera och övervaka distributioner av DSC-tillägg. hello följande cmdlet: ar som visas är hello klassisk distribution moduler, men ”Azure” kan ersättas med ”AzureRm” toouse hello Azure Resource Manager-modellen. Till exempel `Publish-AzureVMDscConfiguration` använder hello klassiska distributionsmodellen där `Publish-AzureRmVMDscConfiguration` använder Azure Resource Manager. 

`Publish-AzureVMDscConfiguration`använder en konfigurationsfil, söker efter beroende DSC-resurser och skapar en .zip-fil som innehåller hello konfiguration och DSC resurser nödvändiga tooenact hello konfiguration. Det kan även skapa hello paketet lokalt med hjälp av hello `-ConfigurationArchivePath` parameter. Annars publicerar hello ZIP-filen tooAzure blob-lagring och skyddar den med en SAS-token.

hello ZIP-fil som skapats av denna cmdlet har hello .ps1 konfigurationsskript hello roten i hello arkivmapp. Resurser har hello modulen mappen placeras i hello arkivmapp. 

`Set-AzureVMDscExtension`lägger in hello inställningar som hello PowerShell DSC-tillägg i ett VM-konfigurationsobjekt. I hello klassiska distributionsmodellen hello VM ändringar måste vara tillämpade tooan virtuella Azure-datorn med `Update-AzureVM`. 

`Get-AzureVMDscExtension`hämtar hello DSC-Tilläggsstatus för en viss virtuell dator. 

`Get-AzureVMDscExtensionStatus`hämtar hello status hello DSC-konfigurationen trätt i kraft av hello DSC-tillägg-hanteraren. Den här åtgärden kan utföras på en enda virtuell dator eller grupp med virtuella datorer.

`Remove-AzureVMDscExtension`tar bort hello tillägget hanteraren från en viss virtuell dator. Denna cmdlet har **inte** ta bort hello konfigurationen, avinstallera hello WMF eller ändra hello tillämpas inställningarna på hello virtuell dator. Det tar bara bort hello tillägget hanterare. 

**Viktiga skillnader i ASM och Azure Resource Manager-cmdlets**

* Azure Resource Manager-cmdlets är synkrona. ASM-cmdlets är asynkron.
* ResourceGroupName, VMName, ArchiveStorageAccountName, Version och platsen är alla obligatoriska parametrar i Azure Resource Manager.
* ArchiveResourceGroupName är en ny valfri parameter för Azure Resource Manager. Du kan ange den här parametern när ditt lagringskonto tillhör tooa annan resursgrupp än hello en där hello virtuell dator skapas.
* ConfigurationArchive kallas ArchiveBlobName i Azure Resource Manager
* ContainerName kallas ArchiveContainerName i Azure Resource Manager
* StorageEndpointSuffix kallas ArchiveStorageEndpointSuffix i Azure Resource Manager
* hello automatisk uppdatering växel har lagts tooAzure Resource Manager tooenable automatisk uppdatering hello tillägget hanteraren toohello senaste version som och när den är tillgänglig. Observera att den här parametern har hello potentiella toocause omstarter på hello VM när en ny version av hello WMF släpps. 

## <a name="azure-portal-functionality"></a>Azure portal funktioner
Bläddra tooa VM. Under Inställningar -> Allmänt klickar du på ”Extensions”. Ett nytt fönster skapas. Klicka på ”Lägg till” och välj PowerShell DSC.

hello-portalen måste indata.
**Konfiguration av moduler eller skript**: det här fältet är obligatoriskt. Kräver en .ps1-fil som innehåller ett konfigurationsskript eller en .zip-fil med en .ps1-konfigurationsskript hello roten och alla beroende resurser i modulen mapparna hello .zip. Det kan skapas med hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet som ingår i hello Azure PowerShell SDK. hello ZIP-filen har överförts till ditt blob-lagring för användaren som skyddas av en SAS-token. 

**Konfigurationsfilen Data PSD1**: det här fältet är valfritt. Om din konfiguration kräver en konfigurationsfil för data i .psd1, använder du det här fältet tooselect den och ladda upp den tooyour användaren blob-lagring, där det skyddas av en SAS-token. 

**Modulkvalificerade namn Configuration**: .ps1-filer kan ha flera configuration-funktioner. Ange hello namnet på hello .ps1 konfigurationsskript följt av en '\' och hello namnet på hello configuration funktion. Om skriptet .ps1 har hello namnet ”configuration.ps1” och hello konfigurationen är ”IisInstall”, skriver du:`configuration.ps1\IisInstall`

**Konfigurationen argument**: om hello configuration funktionen tar argument, ange dem i här hello format `argumentName1=value1,argumentName2=value2`. Observera att det här är ett annat format än hur configuration argument accepteras via PowerShell-cmdlets eller Resource Manager-mallar. 

## <a name="getting-started"></a>Komma igång
hello Azure DSC-tillägg tar i dokument för DSC-konfigurationen och utfärdar dem på Azure Virtual Machines. Ett enkelt exempel på en konfiguration som visas nedan. Spara den lokalt som ”IisInstall.ps1”:

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

hello följa steg plats hello IisInstall.ps1 skript på hello angivna VM, köra hello konfiguration och rapportera om status.
###<a name="classic-model"></a>Klassiska modellen
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish hello configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set hello VM toorun hello DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update hello configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Azure Resource Manager-modellen

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish hello configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set hello VM toorun hello DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Loggning
Loggar placeras i:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versionsnummer]

## <a name="next-steps"></a>Nästa steg
Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 

Granska hello [Azure Resource Manager-mall för hello DSC-tillägg](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

toofind ytterligare funktioner som du kan hantera med PowerShell DSC [Bläddra hello PowerShell-galleriet](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) mer DSC-resurser.

Mer information om skicka känslig parametrar i konfigurationer finns [hantera autentiseringsuppgifter på ett säkert sätt med hello DSC-tillägg hanteraren](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

