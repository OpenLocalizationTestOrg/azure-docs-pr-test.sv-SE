---
title: Autentiseringsuppgifter skickas till Azure med DSC | Microsoft Docs
description: "Översikt över autentiseringsuppgifter skickas på ett säkert sätt till Azure virtuella datorer med hjälp av PowerShell Desired State Configuration"
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
ms.openlocfilehash: acd768c0219ec23c0453a65c575faf5213d9c616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a><span data-ttu-id="adb54-103">Autentiseringsuppgifter skickas till Azure DSC-tillägg-hanterare</span><span class="sxs-lookup"><span data-stu-id="adb54-103">Passing credentials to the Azure DSC extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="adb54-104">Den här artikeln beskriver Desired State Configuration-tillägget för Azure.</span><span class="sxs-lookup"><span data-stu-id="adb54-104">This article covers the Desired State Configuration extension for Azure.</span></span> <span data-ttu-id="adb54-105">En översikt över hanteraren för DSC-tillägg finns på [introduktion till Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="adb54-105">An overview of the DSC extension handler can be found at [Introduction to the Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="passing-in-credentials"></a><span data-ttu-id="adb54-106">Skicka i autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="adb54-106">Passing in credentials</span></span>
<span data-ttu-id="adb54-107">Som en del av konfigurationsprocessen om du behöver konfigurera användarkonton, åtkomst till tjänster, eller installera ett program i en användarkontext.</span><span class="sxs-lookup"><span data-stu-id="adb54-107">As a part of the configuration process, you may need to set up user accounts, access services, or install a program in a user context.</span></span> <span data-ttu-id="adb54-108">Om du vill göra detta måste du ange autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="adb54-108">To do these things, you need to provide credentials.</span></span> 

<span data-ttu-id="adb54-109">DSC tillåter parametriserade konfigurationer där autentiseringsuppgifterna skickades till konfigurationen och lagras på ett säkert sätt i MOF-filer.</span><span class="sxs-lookup"><span data-stu-id="adb54-109">DSC allows for parameterized configurations in which credentials are passed into the configuration and securely stored in MOF files.</span></span> <span data-ttu-id="adb54-110">Azure-tillägget hanteraren förenklar hanteringen av autentiseringsuppgifter genom att tillhandahålla automatisk hantering av certifikat.</span><span class="sxs-lookup"><span data-stu-id="adb54-110">The Azure Extension Handler simplifies credential management by providing automatic management of certificates.</span></span> 

<span data-ttu-id="adb54-111">Tänk på följande DSC-konfigurationsskript som skapar ett lokalt konto med det angivna lösenordet:</span><span class="sxs-lookup"><span data-stu-id="adb54-111">Consider the following DSC configuration script that creates a local user account with the specified password:</span></span>

<span data-ttu-id="adb54-112">*user_configuration.ps1*</span><span class="sxs-lookup"><span data-stu-id="adb54-112">*user_configuration.ps1*</span></span>

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

<span data-ttu-id="adb54-113">Det är viktigt att ha *nod localhost* som en del av konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="adb54-113">It is important to include *node localhost* as part of the configuration.</span></span> <span data-ttu-id="adb54-114">Om den här instruktionen saknas, fungerar inte följande steg som hanteraren tillägg specifikt söker efter nod localhost-instruktion.</span><span class="sxs-lookup"><span data-stu-id="adb54-114">If this statement is missing, the following steps do not work as the extension handler specifically looks for the node localhost statement.</span></span> <span data-ttu-id="adb54-115">Det är också viktigt att inkludera den typecast *[PsCredential]*eftersom den här specifika typen utlöser tillägget för att kryptera autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="adb54-115">It is also important to include the typecast *[PsCredential]*, as this specific type triggers the extension to encrypt the credential.</span></span> 

<span data-ttu-id="adb54-116">Publicera det här skriptet till blob storage:</span><span class="sxs-lookup"><span data-stu-id="adb54-116">Publish this script to blob storage:</span></span>

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

<span data-ttu-id="adb54-117">Ange Azure DSC-tillägg och ange autentiseringsuppgifterna:</span><span class="sxs-lookup"><span data-stu-id="adb54-117">Set the Azure DSC extension and provide the credential:</span></span>

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a><span data-ttu-id="adb54-118">Hur autentiseringsuppgifter skyddas</span><span class="sxs-lookup"><span data-stu-id="adb54-118">How credentials are secured</span></span>
<span data-ttu-id="adb54-119">Kör den här koden efterfrågar autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="adb54-119">Running this code prompts for a credential.</span></span> <span data-ttu-id="adb54-120">När det tillhandahålls lagras i minnet kort.</span><span class="sxs-lookup"><span data-stu-id="adb54-120">Once it is provided, it is stored in memory briefly.</span></span> <span data-ttu-id="adb54-121">När den publiceras med `Set-AzureVmDscExtension` cmdlet, den överförs via HTTPS till den virtuella datorn, där Azure lagrar den krypterade på disk, med det lokala VM-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="adb54-121">When it is published with `Set-AzureVmDscExtension` cmdlet, it is transmitted over HTTPS to the VM, where Azure stores it encrypted on disk, using the local VM certificate.</span></span> <span data-ttu-id="adb54-122">Det är sedan kort dekrypteras i minnet och krypteras igen om du vill skicka till DSC.</span><span class="sxs-lookup"><span data-stu-id="adb54-122">Then it is briefly decrypted in memory and re-encrypted to pass it to DSC.</span></span>

<span data-ttu-id="adb54-123">Det här beteendet skiljer sig [använder säker konfigurationer utan tillägget hanteraren](https://msdn.microsoft.com/powershell/dsc/securemof).</span><span class="sxs-lookup"><span data-stu-id="adb54-123">This behavior is different than [using secure configurations without the extension handler](https://msdn.microsoft.com/powershell/dsc/securemof).</span></span> <span data-ttu-id="adb54-124">Azure-miljön gör att överföra konfigurationsdata på ett säkert sätt via certifikat.</span><span class="sxs-lookup"><span data-stu-id="adb54-124">The Azure environment gives a way to transmit configuration data securely via certificates.</span></span> <span data-ttu-id="adb54-125">När du använder Hanteraren för DSC-tillägg, det finns ingen anledning att tillhandahålla $CertificatePath eller en $CertificateID / $Thumbprint post i ConfigurationData.</span><span class="sxs-lookup"><span data-stu-id="adb54-125">When using the DSC extension handler, there is no need to provide $CertificatePath or a $CertificateID / $Thumbprint entry in ConfigurationData.</span></span>

## <a name="next-steps"></a><span data-ttu-id="adb54-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="adb54-126">Next steps</span></span>
<span data-ttu-id="adb54-127">Mer information om Azure DSC-tillägg-hanteraren finns [introduktion till Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="adb54-127">For more information on the Azure DSC extension handler, see [Introduction to the Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="adb54-128">Granska de [Azure Resource Manager-mall för DSC-tillägg](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="adb54-128">Examine the [Azure Resource Manager template for the DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="adb54-129">Mer information om PowerShell DSC [finns på PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="adb54-129">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="adb54-130">Du hittar ytterligare funktioner som du kan hantera med PowerShell DSC [Bläddra PowerShell-galleriet](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) mer DSC-resurser.</span><span class="sxs-lookup"><span data-stu-id="adb54-130">To find additional functionality you can manage with PowerShell DSC, [browse the PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

