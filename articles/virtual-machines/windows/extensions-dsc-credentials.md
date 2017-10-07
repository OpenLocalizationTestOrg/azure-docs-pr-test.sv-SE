---
title: "aaaPassing autentiseringsuppgifter tooAzure använder DSC | Microsoft Docs"
description: "Översikt för säker överföring av autentiseringsuppgifter tooAzure virtuella datorer med hjälp av PowerShell Desired State Configuration"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: 306ecd3fd481f49a0beca5052fc7531a52999330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="passing-credentials-toohello-azure-dsc-extension-handler"></a>Skicka autentiseringsuppgifter toohello Azure DSC-tillägg-hanterare
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Den här artikeln beskriver hello Desired State Configuration-tillägg för Azure. En översikt över hanteraren för hello DSC-tillägg finns på [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>Skicka i autentiseringsuppgifter
Du kanske måste tooset som en del av konfigurationsprocessen för hello användarkonton, åtkomst till tjänster, eller installera ett program i en användarkontext. toodo detta, behöver du tooprovide autentiseringsuppgifter. 

DSC tillåter parametriserade konfigurationer där autentiseringsuppgifterna skickades till hello konfiguration och lagras på ett säkert sätt i MOF-filer. hello Azure tillägget hanteraren förenklar hanteringen av autentiseringsuppgifter genom att tillhandahålla automatisk hantering av certifikat. 

Tänk hello följande DSC-konfigurationsskript som skapar ett lokalt användarkonto med hello angivna lösenordet:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Det är viktigt tooinclude *nod localhost* som en del av hello konfiguration. Om den här instruktionen saknas fungerar hello följande inte som hello tillägget hanterare specifikt söker efter hello nod localhost-instruktion. Det är också viktigt tooinclude hello typtilldelning av *[PsCredential]*eftersom den här specifika typen utlöser hello tillägget tooencrypt hello autentiseringsuppgifter. 

Publicera det här skriptet tooblob lagring:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Ange hello Azure DSC-tillägg och ange hello autentiseringsuppgifter:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Hur autentiseringsuppgifter skyddas
Kör den här koden efterfrågar autentiseringsuppgifter. När det tillhandahålls lagras i minnet kort. När den publiceras med `Set-AzureVmDscExtension` cmdlet, den skickas över HTTPS toohello VM, där Azure lagrar den krypterade på disk som använder hello lokala VM-certifikat. Är en kort dekrypterade i minnet och omkrypterade toopass den tooDSC.

Det här beteendet skiljer sig [använder säker konfigurationer utan hello tillägget hanterare](https://msdn.microsoft.com/powershell/dsc/securemof). hello Azure-miljön ger ett sätt tootransmit konfigurationsdata på ett säkert sätt via certifikat. När du använder Hanteraren för hello DSC-tillägg, det finns inget behov av tooprovide $CertificatePath eller en $CertificateID / $Thumbprint post i ConfigurationData.

## <a name="next-steps"></a>Nästa steg
Mer information om hello Azure DSC-tillägg-hanteraren finns [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Granska hello [Azure Resource Manager-mall för hello DSC-tillägg](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 

toofind ytterligare funktioner som du kan hantera med PowerShell DSC [Bläddra hello PowerShell-galleriet](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) mer DSC-resurser.

