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
# <a name="passing-credentials-toohello-azure-dsc-extension-handler"></a><span data-ttu-id="969e2-103">Skicka autentiseringsuppgifter toohello Azure DSC-tillägg-hanterare</span><span class="sxs-lookup"><span data-stu-id="969e2-103">Passing credentials toohello Azure DSC extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="969e2-104">Den här artikeln beskriver hello Desired State Configuration-tillägg för Azure.</span><span class="sxs-lookup"><span data-stu-id="969e2-104">This article covers hello Desired State Configuration extension for Azure.</span></span> <span data-ttu-id="969e2-105">En översikt över hanteraren för hello DSC-tillägg finns på [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="969e2-105">An overview of hello DSC extension handler can be found at [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="passing-in-credentials"></a><span data-ttu-id="969e2-106">Skicka i autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="969e2-106">Passing in credentials</span></span>
<span data-ttu-id="969e2-107">Du kanske måste tooset som en del av konfigurationsprocessen för hello användarkonton, åtkomst till tjänster, eller installera ett program i en användarkontext.</span><span class="sxs-lookup"><span data-stu-id="969e2-107">As a part of hello configuration process, you may need tooset up user accounts, access services, or install a program in a user context.</span></span> <span data-ttu-id="969e2-108">toodo detta, behöver du tooprovide autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="969e2-108">toodo these things, you need tooprovide credentials.</span></span> 

<span data-ttu-id="969e2-109">DSC tillåter parametriserade konfigurationer där autentiseringsuppgifterna skickades till hello konfiguration och lagras på ett säkert sätt i MOF-filer.</span><span class="sxs-lookup"><span data-stu-id="969e2-109">DSC allows for parameterized configurations in which credentials are passed into hello configuration and securely stored in MOF files.</span></span> <span data-ttu-id="969e2-110">hello Azure tillägget hanteraren förenklar hanteringen av autentiseringsuppgifter genom att tillhandahålla automatisk hantering av certifikat.</span><span class="sxs-lookup"><span data-stu-id="969e2-110">hello Azure Extension Handler simplifies credential management by providing automatic management of certificates.</span></span> 

<span data-ttu-id="969e2-111">Tänk hello följande DSC-konfigurationsskript som skapar ett lokalt användarkonto med hello angivna lösenordet:</span><span class="sxs-lookup"><span data-stu-id="969e2-111">Consider hello following DSC configuration script that creates a local user account with hello specified password:</span></span>

<span data-ttu-id="969e2-112">*user_configuration.ps1*</span><span class="sxs-lookup"><span data-stu-id="969e2-112">*user_configuration.ps1*</span></span>

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

<span data-ttu-id="969e2-113">Det är viktigt tooinclude *nod localhost* som en del av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="969e2-113">It is important tooinclude *node localhost* as part of hello configuration.</span></span> <span data-ttu-id="969e2-114">Om den här instruktionen saknas fungerar hello följande inte som hello tillägget hanterare specifikt söker efter hello nod localhost-instruktion.</span><span class="sxs-lookup"><span data-stu-id="969e2-114">If this statement is missing, hello following steps do not work as hello extension handler specifically looks for hello node localhost statement.</span></span> <span data-ttu-id="969e2-115">Det är också viktigt tooinclude hello typtilldelning av *[PsCredential]*eftersom den här specifika typen utlöser hello tillägget tooencrypt hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="969e2-115">It is also important tooinclude hello typecast *[PsCredential]*, as this specific type triggers hello extension tooencrypt hello credential.</span></span> 

<span data-ttu-id="969e2-116">Publicera det här skriptet tooblob lagring:</span><span class="sxs-lookup"><span data-stu-id="969e2-116">Publish this script tooblob storage:</span></span>

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

<span data-ttu-id="969e2-117">Ange hello Azure DSC-tillägg och ange hello autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="969e2-117">Set hello Azure DSC extension and provide hello credential:</span></span>

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a><span data-ttu-id="969e2-118">Hur autentiseringsuppgifter skyddas</span><span class="sxs-lookup"><span data-stu-id="969e2-118">How credentials are secured</span></span>
<span data-ttu-id="969e2-119">Kör den här koden efterfrågar autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="969e2-119">Running this code prompts for a credential.</span></span> <span data-ttu-id="969e2-120">När det tillhandahålls lagras i minnet kort.</span><span class="sxs-lookup"><span data-stu-id="969e2-120">Once it is provided, it is stored in memory briefly.</span></span> <span data-ttu-id="969e2-121">När den publiceras med `Set-AzureVmDscExtension` cmdlet, den skickas över HTTPS toohello VM, där Azure lagrar den krypterade på disk som använder hello lokala VM-certifikat.</span><span class="sxs-lookup"><span data-stu-id="969e2-121">When it is published with `Set-AzureVmDscExtension` cmdlet, it is transmitted over HTTPS toohello VM, where Azure stores it encrypted on disk, using hello local VM certificate.</span></span> <span data-ttu-id="969e2-122">Är en kort dekrypterade i minnet och omkrypterade toopass den tooDSC.</span><span class="sxs-lookup"><span data-stu-id="969e2-122">Then it is briefly decrypted in memory and re-encrypted toopass it tooDSC.</span></span>

<span data-ttu-id="969e2-123">Det här beteendet skiljer sig [använder säker konfigurationer utan hello tillägget hanterare](https://msdn.microsoft.com/powershell/dsc/securemof).</span><span class="sxs-lookup"><span data-stu-id="969e2-123">This behavior is different than [using secure configurations without hello extension handler](https://msdn.microsoft.com/powershell/dsc/securemof).</span></span> <span data-ttu-id="969e2-124">hello Azure-miljön ger ett sätt tootransmit konfigurationsdata på ett säkert sätt via certifikat.</span><span class="sxs-lookup"><span data-stu-id="969e2-124">hello Azure environment gives a way tootransmit configuration data securely via certificates.</span></span> <span data-ttu-id="969e2-125">När du använder Hanteraren för hello DSC-tillägg, det finns inget behov av tooprovide $CertificatePath eller en $CertificateID / $Thumbprint post i ConfigurationData.</span><span class="sxs-lookup"><span data-stu-id="969e2-125">When using hello DSC extension handler, there is no need tooprovide $CertificatePath or a $CertificateID / $Thumbprint entry in ConfigurationData.</span></span>

## <a name="next-steps"></a><span data-ttu-id="969e2-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="969e2-126">Next steps</span></span>
<span data-ttu-id="969e2-127">Mer information om hello Azure DSC-tillägg-hanteraren finns [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="969e2-127">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="969e2-128">Granska hello [Azure Resource Manager-mall för hello DSC-tillägg](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="969e2-128">Examine hello [Azure Resource Manager template for hello DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="969e2-129">Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="969e2-129">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="969e2-130">toofind ytterligare funktioner som du kan hantera med PowerShell DSC [Bläddra hello PowerShell-galleriet](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) mer DSC-resurser.</span><span class="sxs-lookup"><span data-stu-id="969e2-130">toofind additional functionality you can manage with PowerShell DSC, [browse hello PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

