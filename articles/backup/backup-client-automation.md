---
title: aaaUse PowerShell tooback in Windows Server tooAzure | Microsoft Docs
description: "Lär dig hur toodeploy och hantera Azure Backup med hjälp av PowerShell"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: f13224f53abd6fbd132fee4347b0b99e8f5e2678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="7a7c1-103">Distribuera och hantera säkerhetskopiering tooAzure för Windows Server och Windows-klient med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a7c1-103">Deploy and manage backup tooAzure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7a7c1-104">ARM</span><span class="sxs-lookup"><span data-stu-id="7a7c1-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="7a7c1-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="7a7c1-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="7a7c1-106">Den här artikeln beskrivs hur du toouse PowerShell för att konfigurera Azure Backup på Windows Server eller en Windows-klient och hantera säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-106">This article shows you how toouse PowerShell for setting up Azure Backup on Windows Server or a Windows client, and managing backup and recovery.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="7a7c1-107">Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a7c1-107">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="7a7c1-108">Den här artikeln fokuserar på hello Azure Resource Manager (ARM) och hello MS Online Backup PowerShell-cmdlets som möjliggör toouse Recovery Services-valv i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-108">This article focuses on hello Azure Resource Manager (ARM) and hello MS Online Backup PowerShell cmdlets that enable you toouse a Recovery Services vault in a resource group.</span></span>

<span data-ttu-id="7a7c1-109">Azure PowerShell 1.0 gavs ut i oktober 2015.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-109">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="7a7c1-110">Den här versionen lyckades hello 0.9.8 versionen och medfört några viktiga ändringar, särskilt i mönstret för hello hello-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-110">This release succeeded hello 0.9.8 release and brought about some significant changes, especially in hello naming pattern of hello cmdlets.</span></span> <span data-ttu-id="7a7c1-111">1.0 cmdlets Följ hello namngivningsmönstret {verb}-AzureRm {substantiv}; medan hello 0.9.8 inte 0.9.8-namn inkluderar **Rm** (till exempel New-AzureRmResourceGroup istället för New-AzureResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="7a7c1-111">1.0 cmdlets follow hello naming pattern {verb}-AzureRm{noun}; whereas, hello 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="7a7c1-112">När du använder Azure PowerShell 0.9.8, måste du först aktivera hello Resource Manager-läget genom att köra hello **Switch-AzureMode AzureResourceManager** kommando.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-112">When using Azure PowerShell 0.9.8, you must first enable hello Resource Manager mode by running hello **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="7a7c1-113">Det här kommandot behövs inte i 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-113">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="7a7c1-114">Om du vill toouse skripten skrivna för hello 0.9.8 miljö i hello 1.0 eller senare miljö kan du noggrant bör uppdatera och testa hello skript i en förproduktionsmiljö innan du använder dem i produktion tooavoid oväntat påverkan.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-114">If you want toouse your scripts written for hello 0.9.8 environment, in hello 1.0 or later environment, you should carefully update and test hello scripts in a pre-production environment before using them in production tooavoid unexpected impact.</span></span>

<span data-ttu-id="7a7c1-115">[Hämta hello senaste PowerShell versionen](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7a7c1-115">[Download hello latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="7a7c1-116">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="7a7c1-116">Create a recovery services vault</span></span>
<span data-ttu-id="7a7c1-117">hello följande steg leder dig genom att skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-117">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="7a7c1-118">Recovery Services-valvet skiljer sig en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-118">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="7a7c1-119">Om du använder Azure Backup för hello första gången, måste du använda hello **registrera AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-119">If you are using Azure Backup for hello first time, you must use hello **Register-AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="7a7c1-120">hello Recovery Services-valvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-120">hello Recovery Services vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="7a7c1-121">Du kan använda en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-121">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="7a7c1-122">När du skapar en ny resursgrupp, ange hello namn och plats för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-122">When creating a new resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. <span data-ttu-id="7a7c1-123">Använd hello **ny AzureRmRecoveryServicesVault** cmdlet toocreate hello nytt valv.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-123">Use hello **New-AzureRmRecoveryServicesVault** cmdlet toocreate hello new vault.</span></span> <span data-ttu-id="7a7c1-124">Glöm toospecify hello samma plats för hello valvet som användes för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-124">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. <span data-ttu-id="7a7c1-125">Ange hello typ av lagring redundans toouse; Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="7a7c1-125">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="7a7c1-126">hello följande exempel visar hello - BackupStorageRedundancy alternativet för testVault anges tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-126">hello following example shows hello -BackupStorageRedundancy option for testVault is set tooGeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="7a7c1-127">Många Azure Backup-cmdletar kräver hello Recovery Services-valvet objekt som indata.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-127">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="7a7c1-128">Därför är det praktiskt toostore hello säkerhetskopiering Recovery Services-valvet objekt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-128">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="7a7c1-129">Visa hello valv i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a7c1-129">View hello vaults in a subscription</span></span>
<span data-ttu-id="7a7c1-130">Använd **Get-AzureRmRecoveryServicesVault** tooview hello lista över alla valv i hello nuvarande prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-130">Use **Get-AzureRmRecoveryServicesVault** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="7a7c1-131">Du kan använda det här kommandot toocheck att ett nytt valv skapades eller toosee vilka valv är tillgängliga i hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-131">You can use this command toocheck that a new  vault was created, or toosee what vaults are available in hello subscription.</span></span>

<span data-ttu-id="7a7c1-132">Kör hello kommando **Get-AzureRmRecoveryServicesVault**, och alla valv i hello prenumeration visas.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-132">Run hello command, **Get-AzureRmRecoveryServicesVault**, and all vaults in hello subscription are listed.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-hello-azure-backup-agent"></a><span data-ttu-id="7a7c1-133">Installera hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="7a7c1-133">Installing hello Azure Backup agent</span></span>
<span data-ttu-id="7a7c1-134">Innan du installerar hello Azure Backup-agenten måste toohave hello installer hämtats och finns på hello Windows Server.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-134">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="7a7c1-135">Du kan hämta hello senaste versionen av hello installer från hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från hello återställningstjänster valvet instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-135">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="7a7c1-136">Spara hello installer tooan lättillgänglig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-136">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="7a7c1-137">Du kan också använda PowerShell tooget hello installationshämtaren:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-137">Alternatively, use PowerShell tooget hello downloader:</span></span>
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

<span data-ttu-id="7a7c1-138">tooinstall hello agent, kör följande kommando i en upphöjd PowerShell-konsol hello:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-138">tooinstall hello agent, run hello following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="7a7c1-139">Detta installerar hello agent med alla hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-139">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="7a7c1-140">hello installationen tar några minuter i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-140">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="7a7c1-141">Om du inte anger hello */nu* alternativet sedan hello **Windows Update** öppnas hello slutet av hello installation toocheck för alla uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-141">If you do not specify hello */nu* option then hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span> <span data-ttu-id="7a7c1-142">När du har installerat visas hello agent i hello listan över installerade program.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-142">Once installed, hello agent will show in hello list of installed programs.</span></span>

<span data-ttu-id="7a7c1-143">toosee hello listan över installerade program finns för**Kontrollpanelen** > **program** > **program och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-143">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Agenten är installerad](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="7a7c1-145">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="7a7c1-145">Installation options</span></span>
<span data-ttu-id="7a7c1-146">toosee alla hello alternativ som är tillgängliga via hello kommandoradsverktyg, använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-146">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="7a7c1-147">hello tillgängliga alternativ inkluderar:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-147">hello available options include:</span></span>

| <span data-ttu-id="7a7c1-148">Alternativ</span><span class="sxs-lookup"><span data-stu-id="7a7c1-148">Option</span></span> | <span data-ttu-id="7a7c1-149">Information</span><span class="sxs-lookup"><span data-stu-id="7a7c1-149">Details</span></span> | <span data-ttu-id="7a7c1-150">Standard</span><span class="sxs-lookup"><span data-stu-id="7a7c1-150">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7a7c1-151">/q</span><span class="sxs-lookup"><span data-stu-id="7a7c1-151">/q</span></span> |<span data-ttu-id="7a7c1-152">Tyst installation</span><span class="sxs-lookup"><span data-stu-id="7a7c1-152">Quiet installation</span></span> |- |
| <span data-ttu-id="7a7c1-153">/ p: ”plats”</span><span class="sxs-lookup"><span data-stu-id="7a7c1-153">/p:"location"</span></span> |<span data-ttu-id="7a7c1-154">Sökvägen toohello installationsmappen för hello Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-154">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="7a7c1-155">C:\Program Files\Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="7a7c1-155">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="7a7c1-156">/ s: ”plats”</span><span class="sxs-lookup"><span data-stu-id="7a7c1-156">/s:"location"</span></span> |<span data-ttu-id="7a7c1-157">Sökvägen toohello cachemappen för hello Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-157">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="7a7c1-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="7a7c1-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="7a7c1-159">/m</span><span class="sxs-lookup"><span data-stu-id="7a7c1-159">/m</span></span> |<span data-ttu-id="7a7c1-160">Anmäl tooMicrosoft uppdatering</span><span class="sxs-lookup"><span data-stu-id="7a7c1-160">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="7a7c1-161">/nu</span><span class="sxs-lookup"><span data-stu-id="7a7c1-161">/nu</span></span> |<span data-ttu-id="7a7c1-162">Sök inte efter uppdateringar när installationen är klar</span><span class="sxs-lookup"><span data-stu-id="7a7c1-162">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="7a7c1-163">/d</span><span class="sxs-lookup"><span data-stu-id="7a7c1-163">/d</span></span> |<span data-ttu-id="7a7c1-164">Avinstallerar Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="7a7c1-164">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="7a7c1-165">/pH</span><span class="sxs-lookup"><span data-stu-id="7a7c1-165">/ph</span></span> |<span data-ttu-id="7a7c1-166">Värden proxyadress</span><span class="sxs-lookup"><span data-stu-id="7a7c1-166">Proxy Host Address</span></span> |- |
| <span data-ttu-id="7a7c1-167">/po</span><span class="sxs-lookup"><span data-stu-id="7a7c1-167">/po</span></span> |<span data-ttu-id="7a7c1-168">Portnummer för proxyservern värden</span><span class="sxs-lookup"><span data-stu-id="7a7c1-168">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="7a7c1-169">/Pu</span><span class="sxs-lookup"><span data-stu-id="7a7c1-169">/pu</span></span> |<span data-ttu-id="7a7c1-170">Värddatorn Proxyanvändarnamnet</span><span class="sxs-lookup"><span data-stu-id="7a7c1-170">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="7a7c1-171">/PW</span><span class="sxs-lookup"><span data-stu-id="7a7c1-171">/pw</span></span> |<span data-ttu-id="7a7c1-172">Lösenord för proxy</span><span class="sxs-lookup"><span data-stu-id="7a7c1-172">Proxy Password</span></span> |- |

## <a name="registering-windows-server-or-windows-client-machine-tooa-recovery-services-vault"></a><span data-ttu-id="7a7c1-173">Registrering av Windows Server eller Windows-klienten datorn tooa Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="7a7c1-173">Registering Windows Server or Windows client machine tooa Recovery Services Vault</span></span>
<span data-ttu-id="7a7c1-174">När du har skapat hello Recovery Services-valvet Hämta senaste hello-agenten och hello valvautentiseringsuppgifter och lagra den på en lämplig plats som C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-174">After you created hello Recovery Services vault, download hello latest agent and hello vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

<span data-ttu-id="7a7c1-175">Kör på hello Windows Server eller klientdator för Windows hello [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello datorn med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-175">On hello Windows Server or Windows client machine, run hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello machine with hello vault.</span></span>
<span data-ttu-id="7a7c1-176">Detta och andra cmdletar som används för säkerhetskopiering, är du vilka hello Mars AgentInstaller läggas till som en del av installationsprocessen hello från hello MSONLINE modul.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-176">This, and other cmdlets used for backup, are from hello MSONLINE module which hello Mars AgentInstaller added as part of hello installation process.</span></span> 

<span data-ttu-id="7a7c1-177">hello installationsprogrammet för agenten inte uppdatera hello $Env: PSModulePath variabeln.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-177">hello Agent installer does not update hello $Env:PSModulePath variable.</span></span> <span data-ttu-id="7a7c1-178">Det innebär att modulen automatisk inläsning misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-178">This means module auto-load fails.</span></span> <span data-ttu-id="7a7c1-179">tooresolve detta kan du göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-179">tooresolve this you can do hello following:</span></span>

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

<span data-ttu-id="7a7c1-180">Alternativt kan du manuellt läsa in modulen hello i skriptet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-180">Alternatively, you can manually load hello module in your script as follows:</span></span>

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

<span data-ttu-id="7a7c1-181">När du läser in hello Online Backup-cmdletar, kan du registrera hello valvautentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-181">Once you load hello Online Backup cmdlets, you register hello vault credentials:</span></span>


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="7a7c1-182">Du inte använda relativa sökvägar toospecify hello valvautentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-182">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="7a7c1-183">Du måste ange en absolut sökväg som en inkommande toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-183">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="7a7c1-184">Nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="7a7c1-184">Networking settings</span></span>
<span data-ttu-id="7a7c1-185">När hello anslutningen för hello Windows datorn toohello internet är via en proxyserver, kan hello proxyinställningar också anges toohello agent.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-185">When hello connectivity of hello Windows machine toohello internet is through a proxy server, hello proxy settings can also be provided toohello agent.</span></span> <span data-ttu-id="7a7c1-186">I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-186">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="7a7c1-187">Bandbreddsanvändning kan också kontrolleras med hello-alternativen för ```work hour bandwidth``` och ```non-work hour bandwidth``` för en given uppsättning hello veckodagar.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-187">Bandwidth usage can also be controlled with hello options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of hello week.</span></span>

<span data-ttu-id="7a7c1-188">Ange information för proxy och bandbredd av hello görs med hjälp av hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-188">Setting hello proxy and bandwidth details is done using hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="7a7c1-189">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="7a7c1-189">Encryption settings</span></span>
<span data-ttu-id="7a7c1-190">hello säkerhetskopierade data skickas tooAzure säkerhetskopiering är krypterad tooprotect hello sekretess hello.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-190">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="7a7c1-191">Hej krypteringslösenfrasen har hello ”password” toodecrypt hello data vid hello tiden för återställning.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-191">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="7a7c1-192">Hålla hello lösenfras information säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-192">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="7a7c1-193">Det är inte kan toorestore data från Azure utan lösenfras.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-193">You are not be able toorestore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="7a7c1-194">Säkerhetskopiera filer och mappar</span><span class="sxs-lookup"><span data-stu-id="7a7c1-194">Back up files and folders</span></span>
<span data-ttu-id="7a7c1-195">Alla säkerhetskopieringar från Windows-servrar och klienter tooAzure Säkerhetskopiering styrs av en princip.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-195">All backups from Windows Servers and clients tooAzure Backup are governed by a policy.</span></span> <span data-ttu-id="7a7c1-196">hello princip består av tre delar:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-196">hello policy comprises three parts:</span></span>

1. <span data-ttu-id="7a7c1-197">En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver toobe tas och synkroniseras med hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-197">A **backup schedule** that specifies when backups need toobe taken and synchronized with hello service.</span></span>
2. <span data-ttu-id="7a7c1-198">En **bevarandeschema** som anger hur länge tooretain hello Återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-198">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>
3. <span data-ttu-id="7a7c1-199">En **filen inkludering/undantag specifikationen** som avgör vad bör säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-199">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="7a7c1-200">I det här dokumentet eftersom vi automatisera säkerhetskopiering, vi antar ingenting inte har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-200">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="7a7c1-201">Vi börjar med att skapa en ny säkerhetskopieringsprincip med hello [ny OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-201">We begin by creating a new backup policy using hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="7a7c1-202">På den här gången hello princip är tom och andra cmdletar är nödvändiga toodefine vad som ska inkluderas eller uteslutas, när säkerhetskopieringar ska köras och där hello säkerhetskopior kommer att lagras.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-202">At this time hello policy is empty and other cmdlets are needed toodefine what items will be included or excluded, when backups will run, and where hello backups will be stored.</span></span>

### <a name="configuring-hello-backup-schedule"></a><span data-ttu-id="7a7c1-203">Konfigurera schemat för säkerhetskopiering av hello</span><span class="sxs-lookup"><span data-stu-id="7a7c1-203">Configuring hello backup schedule</span></span>
<span data-ttu-id="7a7c1-204">Hej först hello 3 delar av en princip är hello säkerhetskopieringsschema som skapas med hjälp av hello [ny OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-204">hello first of hello 3 parts of a policy is hello backup schedule, which is created using hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="7a7c1-205">hello schemat för säkerhetskopiering definierar när säkerhetskopieringar behöver toobe som vidtagits.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-205">hello backup schedule defines when backups need toobe taken.</span></span> <span data-ttu-id="7a7c1-206">När du skapar ett schema måste toospecify 2 indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-206">When creating a schedule you need toospecify 2 input parameters:</span></span>

* <span data-ttu-id="7a7c1-207">**Hello veckodagar** att hello säkerhetskopiering ska köras.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-207">**Days of hello week** that hello backup should run.</span></span> <span data-ttu-id="7a7c1-208">Du kan köra hello säkerhetskopieringsjobb på bara en dag eller varje dag i veckan hello eller en kombination i mellan.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-208">You can run hello backup job on just one day, or every day of hello week, or any combination in between.</span></span>
* <span data-ttu-id="7a7c1-209">**Tidpunkter på dagen hello** när hello säkerhetskopiering ska köras.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-209">**Times of hello day** when hello backup should run.</span></span> <span data-ttu-id="7a7c1-210">Du kan definiera upp too3 olika tidpunkter på hello dagen när hello säkerhetskopiering ska utlösas.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-210">You can define up too3 different times of hello day when hello backup will be triggered.</span></span>

<span data-ttu-id="7a7c1-211">Du kan till exempel konfigurera en princip för säkerhetskopiering som körs klockan 4 varje lördag och söndag.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-211">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="7a7c1-212">schemat för säkerhetskopiering av hello måste toobe som är associerade med en princip, och detta kan uppnås genom att använda hello [Set OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-212">hello backup schedule needs toobe associated with a policy, and this can be achieved by using hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="7a7c1-213">Konfigurera en bevarandeprincip</span><span class="sxs-lookup"><span data-stu-id="7a7c1-213">Configuring a retention policy</span></span>
<span data-ttu-id="7a7c1-214">hello bevarandeprincip definierar hur länge recovery återställningspunkter som skapats från säkerhetskopieringsjobb bevaras.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-214">hello retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="7a7c1-215">När du skapar en ny bevarandeprincip med hello [ny OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) kan du ange hello antalet dagar som hello Återställningspunkter för säkerhetskopiering måste toobe behålls med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-215">When creating a new retention policy using hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify hello number of days that hello backup recovery points need toobe retained with Azure Backup.</span></span> <span data-ttu-id="7a7c1-216">hello exemplet nedan anger en bevarandeprincip 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-216">hello example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="7a7c1-217">hello bevarandeprincip måste associeras med hello huvudsakliga princip med hello cmdlet [Set OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="7a7c1-217">hello retention policy must be associated with hello main policy using hello cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-toobe-backed-up"></a><span data-ttu-id="7a7c1-218">Inklusive och utan filer toobe säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="7a7c1-218">Including and excluding files toobe backed up</span></span>
<span data-ttu-id="7a7c1-219">En ```OBFileSpec``` objektet definierar hello filer toobe inkluderas och exkluderas i en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-219">An ```OBFileSpec``` object defines hello files toobe included and excluded in a backup.</span></span> <span data-ttu-id="7a7c1-220">Det här är en uppsättning regler som att scope ut hello skyddade filer och mappar på en dator.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-220">This is a set of rules that scope out hello protected files and folders on a machine.</span></span> <span data-ttu-id="7a7c1-221">Du kan använda så många filen inkluderande eller exkluderande regler som krävs och koppla dem till en princip.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-221">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="7a7c1-222">När du skapar ett nytt OBFileSpec-objekt, kan du:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-222">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="7a7c1-223">Ange hello filer och mappar toobe ingår</span><span class="sxs-lookup"><span data-stu-id="7a7c1-223">Specify hello files and folders toobe included</span></span>
* <span data-ttu-id="7a7c1-224">Ange hello filer och mappar toobe undantas</span><span class="sxs-lookup"><span data-stu-id="7a7c1-224">Specify hello files and folders toobe excluded</span></span>
* <span data-ttu-id="7a7c1-225">Ange rekursiva säkerhetskopiering av data i en mapp (eller) om endast hello översta filer i hello angiven mapp ska säkerhetskopieras upp.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-225">Specify recursive backup of data in a folder (or) whether only hello top-level files in hello specified folder should be backed up.</span></span>

<span data-ttu-id="7a7c1-226">hello senare uppnås med hjälp av hello - rekursiv flaggan i hello ny OBFileSpec kommandot.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-226">hello latter is achieved by using hello -NonRecursive flag in hello New-OBFileSpec command.</span></span>

<span data-ttu-id="7a7c1-227">I hello exemplet nedan vi säkerhetskopiera volym C: och D: och utesluta hello OS-binärfiler i hello Windows-mappen och alla temporära mappar.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-227">In hello example below, we'll back up volume C: and D: and exclude hello OS binaries in hello Windows folder and any temporary folders.</span></span> <span data-ttu-id="7a7c1-228">toodo så skapar vi två filen anvisningar med hjälp av hello [ny OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - en för inkludering och en för exnkludering.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-228">toodo so we'll create two file specifications using hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="7a7c1-229">När hello filen specifikationer har skapats, de är associerade med hello-princip med hello [Lägg till OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-229">Once hello file specifications have been created, they're associated with hello policy using hello [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-hello-policy"></a><span data-ttu-id="7a7c1-230">Hello-principer tillämpas</span><span class="sxs-lookup"><span data-stu-id="7a7c1-230">Applying hello policy</span></span>
<span data-ttu-id="7a7c1-231">Nu hello principobjektet är klar och har en associerad schemat för säkerhetskopiering, bevarandeprincip och en inkludering/undantagslista av filer.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-231">Now hello policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="7a7c1-232">Den här principen kan nu allokeras för Azure Backup toouse.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-232">This policy can now be committed for Azure Backup toouse.</span></span> <span data-ttu-id="7a7c1-233">Innan du installerar hello nyskapad princip Se till att det inte finns några befintliga principer för säkerhetskopiering som associeras med hello-servern med hjälp av hello [ta bort OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-233">Before you apply hello newly created policy ensure that there are no existing backup policies associated with hello server by using hello [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="7a7c1-234">Tar bort hello princip uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-234">Removing hello policy will prompt for confirmation.</span></span> <span data-ttu-id="7a7c1-235">tooskip hello bekräftelse använda hello ```-Confirm:$false``` flagga hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-235">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="7a7c1-236">Genomför hello principobjektet görs med hjälp av hello [Set OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-236">Committing hello policy object is done using hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="7a7c1-237">Detta kommer också ombeds bekräfta.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-237">This will also ask for confirmation.</span></span> <span data-ttu-id="7a7c1-238">tooskip hello bekräftelse använda hello ```-Confirm:$false``` flagga hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-238">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want toosave this backup policy ? [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

<span data-ttu-id="7a7c1-239">Du kan visa hello information om hello säkerhetskopieringsprincip med hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-239">You can view hello details of hello existing backup policy using hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="7a7c1-240">Du kan gå nedåt ytterligare med hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet för hello schemat för säkerhetskopiering och hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet för hello bevarandeprinciper</span><span class="sxs-lookup"><span data-stu-id="7a7c1-240">You can drill-down further using hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for hello backup schedule and hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for hello retention policies</span></span>

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="7a7c1-241">Utför en ad hoc-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7a7c1-241">Performing an ad-hoc backup</span></span>
<span data-ttu-id="7a7c1-242">När du har angett en princip för säkerhetskopiering sker hello säkerhetskopior per hello schema.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-242">Once a backup policy has been set hello backups will occur per hello schedule.</span></span> <span data-ttu-id="7a7c1-243">Utlöser en ad hoc-säkerhetskopiering kan också använda hello [Start OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-243">Triggering an ad-hoc backup is also possible using hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing hello metadata VHD...
Data transfer is in progress. It might take longer since it is hello first backup and all data needs toobe transferred...
Data transfer completed and all backed up data is in hello cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="7a7c1-244">Återställa data från Azure Backup</span><span class="sxs-lookup"><span data-stu-id="7a7c1-244">Restore data from Azure Backup</span></span>
<span data-ttu-id="7a7c1-245">Det här avsnittet får du stegvisa hello stegen för att automatisera återställning av data från Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-245">This section will guide you through hello steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="7a7c1-246">Detta omfattar så hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-246">Doing so involves hello following steps:</span></span>

1. <span data-ttu-id="7a7c1-247">Välj hello källvolymen</span><span class="sxs-lookup"><span data-stu-id="7a7c1-247">Pick hello source volume</span></span>
2. <span data-ttu-id="7a7c1-248">Välj en säkerhetskopiering punkt toorestore</span><span class="sxs-lookup"><span data-stu-id="7a7c1-248">Choose a backup point toorestore</span></span>
3. <span data-ttu-id="7a7c1-249">Välj ett objekt toorestore</span><span class="sxs-lookup"><span data-stu-id="7a7c1-249">Choose an item toorestore</span></span>
4. <span data-ttu-id="7a7c1-250">Utlösaren hello återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="7a7c1-250">Trigger hello restore process</span></span>

### <a name="picking-hello-source-volume"></a><span data-ttu-id="7a7c1-251">Plocka hello källvolymen</span><span class="sxs-lookup"><span data-stu-id="7a7c1-251">Picking hello source volume</span></span>
<span data-ttu-id="7a7c1-252">I ordning toorestore ett objekt från Azure Backup måste du först tooidentify hello källan för hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-252">In order toorestore an item from Azure Backup, you first need tooidentify hello source of hello item.</span></span> <span data-ttu-id="7a7c1-253">Eftersom vi körs hello kommandon hello kontexten för en Windows Server eller en Windows-klient måste identifieras redan hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-253">Since we're executing hello commands in hello context of a Windows Server or a Windows client, hello machine is already identified.</span></span> <span data-ttu-id="7a7c1-254">hello nästa steg för att identifiera hello källa är tooidentify hello volymen som innehåller den.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-254">hello next step in identifying hello source is tooidentify hello volume containing it.</span></span> <span data-ttu-id="7a7c1-255">En lista över volymer eller källor säkerhetskopieras från den här datorn kan hämtas genom att köra hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-255">A list of volumes or sources being backed up from this machine can be retrieved by executing hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="7a7c1-256">Det här kommandot returnerar en matris med alla hello källor säkerhetskopierade från den här servern eller-klienten.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-256">This command returns an array of all hello sources backed up from this server/client.</span></span>

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-toorestore"></a><span data-ttu-id="7a7c1-257">Att välja en plats för säkerhetskopiering från vilken toorestore</span><span class="sxs-lookup"><span data-stu-id="7a7c1-257">Choosing a backup point from which toorestore</span></span>
<span data-ttu-id="7a7c1-258">Hämta en lista över säkerhetskopiering punkter genom att köra hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdleten med lämpliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-258">You retreive a list of backup points by executing hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="7a7c1-259">I vårt exempel väljer du hello senaste säkerhetskopieringspunkt för hello källvolymen *D:* och använda den toorecover en viss fil.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-259">In our example, we’ll choose hello latest backup point for hello source volume *D:* and use it toorecover a specific file.</span></span>

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
<span data-ttu-id="7a7c1-260">hello objektet ```$rps``` är en matris med säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-260">hello object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="7a7c1-261">hello första elementet är hello senaste tidpunkten och hello nte element är hello äldsta.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-261">hello first element is hello latest point and hello Nth element is hello oldest point.</span></span> <span data-ttu-id="7a7c1-262">toochoose hello senaste tidpunkten, kommer vi att använda ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-262">toochoose hello latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-toorestore"></a><span data-ttu-id="7a7c1-263">Om du väljer ett objekt toorestore</span><span class="sxs-lookup"><span data-stu-id="7a7c1-263">Choosing an item toorestore</span></span>
<span data-ttu-id="7a7c1-264">tooidentify hello exakt filen eller mappen toorestore rekursivt använda hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-264">tooidentify hello exact file or folder toorestore, recursively use hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="7a7c1-265">Sätt hello mapphierarkin kan visas med enbart hello ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-265">That way hello folder hierarchy can be browsed solely using hello ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="7a7c1-266">I det här exemplet, om vi vill toorestore hello filen *finances.xls* vi kan referera till den med hjälp av hello objektet ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-266">In this example, if we want toorestore hello file *finances.xls* we can reference that using hello object ```$filesFolders[1]```.</span></span>

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

<span data-ttu-id="7a7c1-267">Du kan också söka efter objekt toorestore med hello ```Get-OBRecoverableItem``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-267">You can also search for items toorestore using hello ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="7a7c1-268">I vårt exempel toosearch för *finances.xls* vi gick att hämta en referens på hello-filen genom att köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-268">In our example, toosearch for *finances.xls* we could get a handle on hello file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a><span data-ttu-id="7a7c1-269">Utlösande hello återställningsprocessen</span><span class="sxs-lookup"><span data-stu-id="7a7c1-269">Triggering hello restore process</span></span>
<span data-ttu-id="7a7c1-270">återställningsprocessen för tootrigger hello vi måste först toospecify hello återställningsalternativ.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-270">tootrigger hello restore process, we first need toospecify hello recovery options.</span></span> <span data-ttu-id="7a7c1-271">Detta kan göras med hjälp av hello [ny OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-271">This can be done by using hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="7a7c1-272">I det här exemplet antar vi att som vi vill toorestore hello filer för*C:\temp*. Vi förutsätter också att vi vill tooskip filer som redan finns på hello målmappen *C:\temp*. toocreate sådana ett återställningsalternativ använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-272">For this example, let's assume that we want toorestore hello files too*C:\temp*. Let's also assume that we want tooskip files that already exist on hello destination folder *C:\temp*. toocreate such a recovery option, use hello following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="7a7c1-273">Nu utlösa hello återställningsprocessen med hello [Start OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) på hello valt ```$item``` från hello utdata från hello ```Get-OBRecoverableItem``` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-273">Now trigger hello restore process by using hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on hello selected ```$item``` from hello output of hello ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a><span data-ttu-id="7a7c1-274">Avinstallera hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="7a7c1-274">Uninstalling hello Azure Backup agent</span></span>
<span data-ttu-id="7a7c1-275">Avinstallera hello Azure Backup-agenten kan göras med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-275">Uninstalling hello Azure Backup agent can be done by using hello following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="7a7c1-276">Avinstallera hello agent binärfiler från hello datorn har vissa konsekvenser tooconsider:</span><span class="sxs-lookup"><span data-stu-id="7a7c1-276">Uninstalling hello agent binaries from hello machine has some consequences tooconsider:</span></span>

* <span data-ttu-id="7a7c1-277">Hello filfilter tas bort från datorn hello och spårning av ändringar har stoppats.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-277">It removes hello file-filter from hello machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="7a7c1-278">All principinformation tas bort från hello dator, men hello principinformation fortsätter toobe lagras i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-278">All policy information is removed from hello machine, but hello policy information continues toobe stored in hello service.</span></span>
* <span data-ttu-id="7a7c1-279">Alla säkerhetskopieringsscheman tas bort och vidtas inga ytterligare säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-279">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="7a7c1-280">Hello data lagras i Azure förblir och behålls enligt hello kvarhållning princip installationen av du.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-280">However, hello data stored in Azure remains and is retained as per hello retention policy setup by you.</span></span> <span data-ttu-id="7a7c1-281">Äldre återställningspunkter rensas automatiskt bort.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-281">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="7a7c1-282">Fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="7a7c1-282">Remote management</span></span>
<span data-ttu-id="7a7c1-283">Alla hello management runt hello Azure Backup-agenten, principer och datakällor kan göras via fjärranslutning via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-283">All hello management around hello Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="7a7c1-284">hello-datorn som ska hanteras via en fjärranslutning måste toobe förberetts på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-284">hello machine that will be managed remotely needs toobe prepared correctly.</span></span>

<span data-ttu-id="7a7c1-285">Som standard har hello WinRM-tjänsten konfigurerats för manuell start.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-285">By default, hello WinRM service is configured for manual startup.</span></span> <span data-ttu-id="7a7c1-286">hello starttyp måste anges för*automatisk* och hello-tjänsten ska startas.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-286">hello startup type must be set too*Automatic* and hello service should be started.</span></span> <span data-ttu-id="7a7c1-287">tooverify som hello WinRM-tjänsten körs, hello statusegenskapen hello värdet bör vara *kör*.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-287">tooverify that hello WinRM service is running, hello value of hello Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="7a7c1-288">PowerShell ska konfigureras för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-288">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="7a7c1-289">hello-dator kan nu fjärrhanteras - från hello installationen av hello agent.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-289">hello machine can now be managed remotely - starting from hello installation of hello agent.</span></span> <span data-ttu-id="7a7c1-290">Följande skript hello kopierar agent hello toohello fjärrdatorn och installerar den.</span><span class="sxs-lookup"><span data-stu-id="7a7c1-290">For example, hello following script copies hello agent toohello remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="7a7c1-291">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a7c1-291">Next steps</span></span>
<span data-ttu-id="7a7c1-292">Mer information om Azure Backup för Windows Server eller-klienten finns</span><span class="sxs-lookup"><span data-stu-id="7a7c1-292">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="7a7c1-293">Introduktion tooAzure säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7a7c1-293">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="7a7c1-294">Säkerhetskopiera Windows-servrar</span><span class="sxs-lookup"><span data-stu-id="7a7c1-294">Back up Windows Servers</span></span>](backup-configure-vault.md)
