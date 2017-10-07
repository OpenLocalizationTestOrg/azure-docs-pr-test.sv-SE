---
title: "aaaAzure säkerhetskopia - Använd PowerShell tooback in DPM-arbetsbelastningar | Microsoft Docs"
description: "Lär dig hur toodeploy och hantera Azure Backup för Data Protection Manager (DPM) med hjälp av PowerShell"
services: backup
documentationcenter: 
author: NKolli1
manager: shreeshd
editor: 
ms.assetid: e9bd223c-2398-4eb1-9bf3-50e08970fea7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: adigan;anuragm;trinadhk;markgal
ms.openlocfilehash: 27d2b4b3127b68c9da564697eb61dc3ccbc34b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="4fc49-103">Distribuera och hantera säkerhetskopiering tooAzure för Data Protection Manager (DPM) servrar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="4fc49-103">Deploy and manage backup tooAzure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4fc49-104">ARM</span><span class="sxs-lookup"><span data-stu-id="4fc49-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="4fc49-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="4fc49-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="4fc49-106">Den här artikeln beskrivs hur du toouse PowerShell-toosetup Azure Backup på en DPM-servern och toomanage säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="4fc49-106">This article shows you how toouse PowerShell toosetup Azure Backup on a DPM server, and toomanage backup and recovery.</span></span>

## <a name="setting-up-hello-powershell-environment"></a><span data-ttu-id="4fc49-107">Konfigurera hello PowerShell-miljö</span><span class="sxs-lookup"><span data-stu-id="4fc49-107">Setting up hello PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="4fc49-108">Innan du kan använda PowerShell toomanage säkerhetskopior från Data Protection Manager tooAzure, behöver du toohave hello rätt miljö i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4fc49-108">Before you can use PowerShell toomanage backups from Data Protection Manager tooAzure, you need toohave hello right environment in PowerShell.</span></span> <span data-ttu-id="4fc49-109">Kontrollera att du kör hello efter kommandot tooimport hello rätt moduler och Tillåt toocorrectly referens hello DPM cmdlets hello början av hello PowerShell-session:</span><span class="sxs-lookup"><span data-stu-id="4fc49-109">At hello start of hello PowerShell session, ensure that you run hello following command tooimport hello right modules and allow you toocorrectly reference hello DPM cmdlets:</span></span>

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome toohello DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a><span data-ttu-id="4fc49-110">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="4fc49-110">Setup and Registration</span></span>
<span data-ttu-id="4fc49-111">toobegin:</span><span class="sxs-lookup"><span data-stu-id="4fc49-111">toobegin:</span></span>

1. <span data-ttu-id="4fc49-112">[Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="4fc49-112">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is: 1.0.0)</span></span>
2. <span data-ttu-id="4fc49-113">Aktivera hello Azure Backup-kommandon genom att växla för*AzureResourceManager* läge med hjälp av hello **Switch-AzureMode** cmdleten igen:</span><span class="sxs-lookup"><span data-stu-id="4fc49-113">Enable hello Azure Backup commandlets by switching too*AzureResourceManager* mode by using hello **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="4fc49-114">hello kan följande inställningar och registrering aktiviteter automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4fc49-114">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="4fc49-115">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="4fc49-115">Create a Recovery Services vault</span></span>
* <span data-ttu-id="4fc49-116">Installera hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="4fc49-116">Installing hello Azure Backup agent</span></span>
* <span data-ttu-id="4fc49-117">Registrera med hello Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="4fc49-117">Registering with hello Azure Backup service</span></span>
* <span data-ttu-id="4fc49-118">Nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="4fc49-118">Networking settings</span></span>
* <span data-ttu-id="4fc49-119">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="4fc49-119">Encryption settings</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="4fc49-120">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="4fc49-120">Create a recovery services vault</span></span>
<span data-ttu-id="4fc49-121">hello följande steg leder dig genom att skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-121">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="4fc49-122">Recovery Services-valvet skiljer sig en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-122">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="4fc49-123">Om du använder Azure Backup för hello första gången, måste du använda hello **registrera AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4fc49-123">If you are using Azure Backup for hello first time, you must use hello **Register-AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="4fc49-124">hello Recovery Services-valvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4fc49-124">hello Recovery Services vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="4fc49-125">Du kan använda en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="4fc49-125">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="4fc49-126">När du skapar en ny resursgrupp, ange hello namn och plats för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4fc49-126">When creating a new resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="4fc49-127">Använd hello **ny AzureRmRecoveryServicesVault** cmdlet toocreate ett nytt valv.</span><span class="sxs-lookup"><span data-stu-id="4fc49-127">Use hello **New-AzureRmRecoveryServicesVault** cmdlet toocreate a new vault.</span></span> <span data-ttu-id="4fc49-128">Glöm toospecify hello samma plats för hello valvet som användes för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4fc49-128">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="4fc49-129">Ange hello typ av lagring redundans toouse; Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="4fc49-129">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="4fc49-130">hello följande exempel visar hello - BackupStorageRedundancy alternativet för testVault anges tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="4fc49-130">hello following example shows hello -BackupStorageRedundancy option for testVault is set tooGeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="4fc49-131">Många Azure Backup-cmdletar kräver hello Recovery Services-valvet objekt som indata.</span><span class="sxs-lookup"><span data-stu-id="4fc49-131">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="4fc49-132">Därför är det praktiskt toostore hello säkerhetskopiering Recovery Services-valvet objekt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="4fc49-132">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="4fc49-133">Visa hello valv i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="4fc49-133">View hello vaults in a subscription</span></span>
<span data-ttu-id="4fc49-134">Använd **Get-AzureRmRecoveryServicesVault** tooview hello lista över alla valv i hello nuvarande prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4fc49-134">Use **Get-AzureRmRecoveryServicesVault** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="4fc49-135">Du kan använda det här kommandot toocheck att ett nytt valv skapades eller toosee vilka valv är tillgängliga i hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4fc49-135">You can use this command toocheck that a new  vault was created, or toosee what vaults are available in hello subscription.</span></span>

<span data-ttu-id="4fc49-136">Kör hello kommandot Get-AzureRmRecoveryServicesVault, och alla valv i hello prenumeration visas.</span><span class="sxs-lookup"><span data-stu-id="4fc49-136">Run hello command, Get-AzureRmRecoveryServicesVault, and all vaults in hello subscription are listed.</span></span>

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


## <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="4fc49-137">Hello Azure Backup agent installeras på en DPM-Server</span><span class="sxs-lookup"><span data-stu-id="4fc49-137">Installing hello Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="4fc49-138">Innan du installerar hello Azure Backup-agenten måste toohave hello installer hämtats och finns på hello Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4fc49-138">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="4fc49-139">Du kan hämta hello senaste versionen av hello installer från hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från hello återställningstjänster valvet instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="4fc49-139">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="4fc49-140">Spara hello installer tooan lättillgänglig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="4fc49-140">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="4fc49-141">tooinstall hello agent, kör följande kommando i en upphöjd PowerShell-konsol hello **på hello DPM-servern**:</span><span class="sxs-lookup"><span data-stu-id="4fc49-141">tooinstall hello agent, run hello following command in an elevated PowerShell console **on hello DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="4fc49-142">Detta installerar hello agent med alla hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-142">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="4fc49-143">hello installationen tar några minuter i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="4fc49-143">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="4fc49-144">Om du inte anger hello */nu* alternativet hello **Windows Update** öppnas hello slutet av hello installation toocheck för alla uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="4fc49-144">If you do not specify hello */nu* option hello **Windows Update** window opens at hello end of hello installation toocheck for any updates.</span></span>

<span data-ttu-id="4fc49-145">hello agenten visas i hello listan över installerade program.</span><span class="sxs-lookup"><span data-stu-id="4fc49-145">hello agent shows up in hello list of installed programs.</span></span> <span data-ttu-id="4fc49-146">toosee hello listan över installerade program finns för**Kontrollpanelen** > **program** > **program och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="4fc49-146">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Agenten är installerad](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="4fc49-148">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="4fc49-148">Installation options</span></span>
<span data-ttu-id="4fc49-149">toosee alla hello alternativ som är tillgängliga via hello kommandorad, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4fc49-149">toosee all hello options available via hello commandline, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="4fc49-150">hello tillgängliga alternativ inkluderar:</span><span class="sxs-lookup"><span data-stu-id="4fc49-150">hello available options include:</span></span>

| <span data-ttu-id="4fc49-151">Alternativ</span><span class="sxs-lookup"><span data-stu-id="4fc49-151">Option</span></span> | <span data-ttu-id="4fc49-152">Information</span><span class="sxs-lookup"><span data-stu-id="4fc49-152">Details</span></span> | <span data-ttu-id="4fc49-153">Standard</span><span class="sxs-lookup"><span data-stu-id="4fc49-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4fc49-154">/q</span><span class="sxs-lookup"><span data-stu-id="4fc49-154">/q</span></span> |<span data-ttu-id="4fc49-155">Tyst installation</span><span class="sxs-lookup"><span data-stu-id="4fc49-155">Quiet installation</span></span> |- |
| <span data-ttu-id="4fc49-156">/ p: ”plats”</span><span class="sxs-lookup"><span data-stu-id="4fc49-156">/p:"location"</span></span> |<span data-ttu-id="4fc49-157">Sökvägen toohello installationsmappen för hello Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="4fc49-157">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="4fc49-158">C:\Program Files\Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="4fc49-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="4fc49-159">/ s: ”plats”</span><span class="sxs-lookup"><span data-stu-id="4fc49-159">/s:"location"</span></span> |<span data-ttu-id="4fc49-160">Sökvägen toohello cachemappen för hello Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="4fc49-160">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="4fc49-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="4fc49-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="4fc49-162">/m</span><span class="sxs-lookup"><span data-stu-id="4fc49-162">/m</span></span> |<span data-ttu-id="4fc49-163">Anmäl tooMicrosoft uppdatering</span><span class="sxs-lookup"><span data-stu-id="4fc49-163">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="4fc49-164">/nu</span><span class="sxs-lookup"><span data-stu-id="4fc49-164">/nu</span></span> |<span data-ttu-id="4fc49-165">Sök inte efter uppdateringar när installationen är klar</span><span class="sxs-lookup"><span data-stu-id="4fc49-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="4fc49-166">/d</span><span class="sxs-lookup"><span data-stu-id="4fc49-166">/d</span></span> |<span data-ttu-id="4fc49-167">Avinstallerar Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="4fc49-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="4fc49-168">/pH</span><span class="sxs-lookup"><span data-stu-id="4fc49-168">/ph</span></span> |<span data-ttu-id="4fc49-169">Värden proxyadress</span><span class="sxs-lookup"><span data-stu-id="4fc49-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="4fc49-170">/po</span><span class="sxs-lookup"><span data-stu-id="4fc49-170">/po</span></span> |<span data-ttu-id="4fc49-171">Portnummer för proxyservern värden</span><span class="sxs-lookup"><span data-stu-id="4fc49-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="4fc49-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="4fc49-172">/pu</span></span> |<span data-ttu-id="4fc49-173">Värddatorn Proxyanvändarnamnet</span><span class="sxs-lookup"><span data-stu-id="4fc49-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="4fc49-174">/PW</span><span class="sxs-lookup"><span data-stu-id="4fc49-174">/pw</span></span> |<span data-ttu-id="4fc49-175">Lösenord för proxy</span><span class="sxs-lookup"><span data-stu-id="4fc49-175">Proxy Password</span></span> |- |

## <a name="registering-dpm-tooa-recovery-services-vault"></a><span data-ttu-id="4fc49-176">Registrerar DPM tooa Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="4fc49-176">Registering DPM tooa Recovery Services Vault</span></span>
<span data-ttu-id="4fc49-177">När du har skapat hello Recovery Services-valvet Hämta senaste hello-agenten och hello valvautentiseringsuppgifter och lagra den på en lämplig plats som C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="4fc49-177">After you created hello Recovery Services vault, download hello latest agent and hello vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

<span data-ttu-id="4fc49-178">Hello DPM-servern kör hello [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello datorn med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-178">On hello DPM server, run hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello machine with hello vault.</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a><span data-ttu-id="4fc49-179">Inledande konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="4fc49-179">Initial configuration settings</span></span>
<span data-ttu-id="4fc49-180">När hello DPM-servern har registrerats med hello Recovery Services-valvet, den börjar med standardinställningar för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="4fc49-180">Once hello DPM Server is registered with hello Recovery Services vault, it starts with default subscription settings.</span></span> <span data-ttu-id="4fc49-181">Inställningarna prenumeration omfattar nätverk, kryptering och hello mellanlagringsområdet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-181">These subscription settings include Networking, Encryption and hello Staging area.</span></span> <span data-ttu-id="4fc49-182">toochange prenumerationsinställningar toofirst måste få en referens för hello befintliga (standard)-inställningar med hjälp av hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="4fc49-182">toochange subscription settings you need toofirst get a handle on hello existing (default) settings using hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="4fc49-183">Alla ändringar har gjorts toothis lokala PowerShell-objektet ```$setting``` och sedan hello fullt objekt är allokerat tooDPM och Azure Backup toosave dem med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-183">All modifications are made toothis local PowerShell object ```$setting```  and then hello full object is committed tooDPM and Azure Backup toosave them using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="4fc49-184">Du behöver toouse hello ```–Commit``` flaggan tooensure som hello ändringar sparas.</span><span class="sxs-lookup"><span data-stu-id="4fc49-184">You need toouse hello ```–Commit``` flag tooensure that hello changes are persisted.</span></span> <span data-ttu-id="4fc49-185">hello-inställningarna kommer inte tillämpas och används av Azure Backup såvida inte genomförts.</span><span class="sxs-lookup"><span data-stu-id="4fc49-185">hello settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a><span data-ttu-id="4fc49-186">Nätverk</span><span class="sxs-lookup"><span data-stu-id="4fc49-186">Networking</span></span>
<span data-ttu-id="4fc49-187">Om hello anslutningen för hello DPM datorn toohello Azure Backup-tjänsten på hello internet via en proxyserver, bör hello proxyserverinställningar anges för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4fc49-187">If hello connectivity of hello DPM machine toohello Azure Backup service on hello internet is through a proxy server, then hello proxy server settings should be provided for successful backups.</span></span> <span data-ttu-id="4fc49-188">Detta görs med hjälp av hello ```-ProxyServer```och ```-ProxyPort```, ```-ProxyUsername``` och hello ```ProxyPassword``` parametrar med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-188">This is done by using hello ```-ProxyServer```and ```-ProxyPort```, ```-ProxyUsername``` and hello ```ProxyPassword``` parameters with hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="4fc49-189">I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.</span><span class="sxs-lookup"><span data-stu-id="4fc49-189">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="4fc49-190">Bandbreddsanvändning kan också kontrolleras med alternativ för ```-WorkHourBandwidth``` och ```-NonWorkHourBandwidth``` för en given uppsättning hello veckodagar.</span><span class="sxs-lookup"><span data-stu-id="4fc49-190">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of hello week.</span></span> <span data-ttu-id="4fc49-191">I det här exemplet anger vi inte någon begränsning.</span><span class="sxs-lookup"><span data-stu-id="4fc49-191">In this example, we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-hello-staging-area"></a><span data-ttu-id="4fc49-192">Konfigurera hello mellanlagringsområde</span><span class="sxs-lookup"><span data-stu-id="4fc49-192">Configuring hello staging Area</span></span>
<span data-ttu-id="4fc49-193">hello Azure Backup-agenten körs på hello DPM-servern behöver tillfällig lagring för data som återställs från hello molnet (lokalt mellanlagringsområde).</span><span class="sxs-lookup"><span data-stu-id="4fc49-193">hello Azure Backup agent running on hello DPM server needs temporary storage for data restored from hello cloud (local staging area).</span></span> <span data-ttu-id="4fc49-194">Konfigurera hello mellanlagringsområdet med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet och hello ```-StagingAreaPath``` parameter.</span><span class="sxs-lookup"><span data-stu-id="4fc49-194">Configure hello staging area using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and hello ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="4fc49-195">I hello-exemplet ovan, hello mellanlagringsområde ska anges för*C:\StagingArea* i hello PowerShell-objektet ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="4fc49-195">In hello example above, hello staging area will be set too*C:\StagingArea* in hello PowerShell object ```$setting```.</span></span> <span data-ttu-id="4fc49-196">Se till att hello mappen redan finns, annars hello slutliga genomförande av hello prenumerationsinställningar misslyckas.</span><span class="sxs-lookup"><span data-stu-id="4fc49-196">Ensure that hello specified folder already exists, or else hello final commit of hello subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="4fc49-197">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="4fc49-197">Encryption settings</span></span>
<span data-ttu-id="4fc49-198">hello säkerhetskopierade data skickas tooAzure säkerhetskopiering är krypterad tooprotect hello sekretess hello.</span><span class="sxs-lookup"><span data-stu-id="4fc49-198">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="4fc49-199">Hej krypteringslösenfrasen har hello ”password” toodecrypt hello data vid hello tiden för återställning.</span><span class="sxs-lookup"><span data-stu-id="4fc49-199">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span> <span data-ttu-id="4fc49-200">Det är viktigt tookeep denna information säkert och säker när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="4fc49-200">It is important tookeep this information safe and secure once it is set.</span></span>

<span data-ttu-id="4fc49-201">I hello exemplet nedan hello första kommandot konverterar hello ```passphrase123456789``` tooa säker sträng och tilldelar hello säker toohello variabel med namnet ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="4fc49-201">In hello example below, hello first command converts hello string ```passphrase123456789``` tooa secure string and assigns hello secure string toohello variable named ```$Passphrase```.</span></span> <span data-ttu-id="4fc49-202">hello andra kommandot anger hello säker sträng i ```$Passphrase``` som hello lösenord för kryptering av säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="4fc49-202">hello second command sets hello secure string in ```$Passphrase``` as hello password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="4fc49-203">Hålla hello lösenfras information säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="4fc49-203">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="4fc49-204">Du kommer inte att kunna toorestore data från Azure utan lösenfras.</span><span class="sxs-lookup"><span data-stu-id="4fc49-204">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="4fc49-205">Nu ska du har gjort alla hello krävs ändringar toohello ```$setting``` objekt.</span><span class="sxs-lookup"><span data-stu-id="4fc49-205">At this point, you should have made all hello required changes toohello ```$setting``` object.</span></span> <span data-ttu-id="4fc49-206">Kom ihåg toocommit hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="4fc49-206">Remember toocommit hello changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a><span data-ttu-id="4fc49-207">Skydda data tooAzure säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="4fc49-207">Protect data tooAzure Backup</span></span>
<span data-ttu-id="4fc49-208">I det här avsnittet kan du lägga till en server tooDPM för produktion och skydda hello datalagring toolocal DPM och tooAzure säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4fc49-208">In this section, you will add a production server tooDPM and then protect hello data toolocal DPM storage and then tooAzure Backup.</span></span> <span data-ttu-id="4fc49-209">I hello exemplen visar vi hur tooback av filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="4fc49-209">In hello examples, we will demonstrate how tooback up files and folders.</span></span> <span data-ttu-id="4fc49-210">hello logik kan enkelt att utökade toobackup alla stöd för DPM-datakällor.</span><span class="sxs-lookup"><span data-stu-id="4fc49-210">hello logic can easily be extended toobackup any DPM-supported data source.</span></span> <span data-ttu-id="4fc49-211">Alla DPM-säkerhetskopieringar styrs av en skydd grupp (PG) med fyra delar:</span><span class="sxs-lookup"><span data-stu-id="4fc49-211">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="4fc49-212">**Medlemmar** är en lista över alla hello skyddsobjekt (även kallat *datakällor* i DPM) som du vill tooprotect i hello samma skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="4fc49-212">**Group members** is a list of all hello protectable objects (also known as *Datasources* in DPM) that you want tooprotect in hello same protection group.</span></span> <span data-ttu-id="4fc49-213">Exempelvis kanske tooprotect produktion virtuella datorer i en skyddsgrupp och SQL Server-databaser i en annan skyddsgrupp som de kan ha olika krav för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4fc49-213">For example, you may want tooprotect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="4fc49-214">Innan du kan säkerhetskopiera någon datakälla på en produktionsserver måste toomake att hello DPM-agenten är installerad på hello server och hanteras av DPM.</span><span class="sxs-lookup"><span data-stu-id="4fc49-214">Before you can back up any datasource on a production server you need toomake sure hello DPM Agent is installed on hello server and is managed by DPM.</span></span> <span data-ttu-id="4fc49-215">Hello gör för [installerar hello DPM-agenten](https://technet.microsoft.com/library/bb870935.aspx) och länka den toohello lämpliga DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="4fc49-215">Follow hello steps for [installing hello DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it toohello appropriate DPM Server.</span></span>
2. <span data-ttu-id="4fc49-216">**Dataskyddsmetod** anger hello mål platser - band, disk och molnet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-216">**Data protection method** specifies hello target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="4fc49-217">I vårt exempel skyddar vi data toohello lokal disk och toohello moln.</span><span class="sxs-lookup"><span data-stu-id="4fc49-217">In our example we will protect data toohello local disk and toohello cloud.</span></span>
3. <span data-ttu-id="4fc49-218">En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver toobe tas och hur ofta hello data ska synkroniseras mellan hello DPM-servern och hello produktionsservern.</span><span class="sxs-lookup"><span data-stu-id="4fc49-218">A **backup schedule** that specifies when backups need toobe taken and how often hello data should be synchronized between hello DPM Server and hello production server.</span></span>
4. <span data-ttu-id="4fc49-219">En **bevarandeschema** som anger hur länge tooretain hello Återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="4fc49-219">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="4fc49-220">Skapa en skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="4fc49-220">Creating a protection group</span></span>
<span data-ttu-id="4fc49-221">Börja med att skapa en ny Skyddsgrupp med hello [ny DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-221">Start by creating a new Protection Group using hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="4fc49-222">hello ovan cmdlet skapar en Skyddsgrupp med namnet *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="4fc49-222">hello above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="4fc49-223">En befintlig skyddsgrupp kan också ändras senare tooadd säkerhetskopiering toohello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-223">An existing protection group can also be modified later tooadd backup toohello Azure cloud.</span></span> <span data-ttu-id="4fc49-224">Dock toomake ändringar toohello Skyddsgrupp – nya eller befintliga - vi behöver tooget en referens i en *ändringsbar* objekt med hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-224">However, toomake any changes toohello Protection Group - new or existing - we need tooget a handle on a *modifiable* object using hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a><span data-ttu-id="4fc49-225">Att lägga till gruppen medlemmar toohello Skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="4fc49-225">Adding group members toohello Protection Group</span></span>
<span data-ttu-id="4fc49-226">Varje DPM-agenten vet hello lista över datakällor på hello-server som den är installerad på.</span><span class="sxs-lookup"><span data-stu-id="4fc49-226">Each DPM Agent knows hello list of datasources on hello server that it is installed on.</span></span> <span data-ttu-id="4fc49-227">tooadd datasource-toohello Skyddsgrupp hello DPM-agenten måste toofirst skicka en lista över hello datakällor tillbaka toohello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="4fc49-227">tooadd a datasource toohello Protection Group, hello DPM Agent needs toofirst send a list of hello datasources back toohello DPM server.</span></span> <span data-ttu-id="4fc49-228">En eller flera datakällor är markerade och tillagda toohello Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="4fc49-228">One or more datasources are then selected and added toohello Protection Group.</span></span> <span data-ttu-id="4fc49-229">hello PowerShell steg behövs tooachieve detta är:</span><span class="sxs-lookup"><span data-stu-id="4fc49-229">hello PowerShell steps needed tooachieve this are:</span></span>

1. <span data-ttu-id="4fc49-230">Hämta en lista över alla servrar som hanteras av DPM via hello DPM-agenten.</span><span class="sxs-lookup"><span data-stu-id="4fc49-230">Fetch a list of all servers managed by DPM through hello DPM Agent.</span></span>
2. <span data-ttu-id="4fc49-231">Välj en specifik server.</span><span class="sxs-lookup"><span data-stu-id="4fc49-231">Choose a specific server.</span></span>
3. <span data-ttu-id="4fc49-232">Hämta en lista över alla datakällor på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="4fc49-232">Fetch a list of all datasources on hello server.</span></span>
4. <span data-ttu-id="4fc49-233">Välj en eller flera datakällor och lägga till dem toohello Skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="4fc49-233">Choose one or more datasources and add them toohello Protection Group</span></span>

<span data-ttu-id="4fc49-234">hello listan över servrar på vilka hello DPM-agenten är installerad och hanteras av hello DPM-servern förvärvas med hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-234">hello list of servers on which hello DPM Agent is installed and is being managed by hello DPM Server is acquired with hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="4fc49-235">I det här exemplet kommer att filtrera och bara konfigurera PS med namnet *productionserver01* för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4fc49-235">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="4fc49-236">Hämta nu hello lista över datakällor på ```$server``` med hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-236">Now fetch hello list of datasources on ```$server``` using hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="4fc49-237">I det här exemplet vi filtrering för hello volym * D:\* som vi vill tooconfigure för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4fc49-237">In this example we are filtering for hello volume *D:\* that we want tooconfigure for backup.</span></span> <span data-ttu-id="4fc49-238">Datakällan läggs sedan toohello Skyddsgruppen med hjälp av hello [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-238">This datasource is then added toohello Protection Group using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="4fc49-239">Kom ihåg toouse hello *ändringsbar* skydd gruppobjekt ```$MPG``` toomake hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="4fc49-239">Remember toouse hello *modifiable* protection group object ```$MPG``` toomake hello additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="4fc49-240">Upprepa detta steg så många gånger som krävs, tills du har lagt till alla hello valt datakällor toohello skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="4fc49-240">Repeat this step as many times as required, until you have added all hello chosen datasources toohello protection group.</span></span> <span data-ttu-id="4fc49-241">Du kan också starta med en datakälla och fullständig hello arbetsflöde för att skapa hello Skyddsgruppen och senare lägga till flera datakällor toohello Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="4fc49-241">You can also start with just one datasource, and complete hello workflow for creating hello Protection Group, and at a later point add more datasources toohello Protection Group.</span></span>

### <a name="selecting-hello-data-protection-method"></a><span data-ttu-id="4fc49-242">Att välja hello dataskyddsmetod</span><span class="sxs-lookup"><span data-stu-id="4fc49-242">Selecting hello data protection method</span></span>
<span data-ttu-id="4fc49-243">När hello datakällorna har lagts till toohello Skyddsgrupp, hello nästa steg är toospecify hello skyddsmetod med hello [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-243">Once hello datasources have been added toohello Protection Group, hello next step is toospecify hello protection method using hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="4fc49-244">I det här exemplet är hello Skyddsgrupp inställningar för lokal disk och säkerhetskopiering i molnet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-244">In this example, hello Protection Group is setup for local disk and cloud backup.</span></span> <span data-ttu-id="4fc49-245">Du måste också toospecify hello datakällan som du vill tooprotect toocloud med hello [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet med - Online flagga.</span><span class="sxs-lookup"><span data-stu-id="4fc49-245">You also need toospecify hello datasource that you want tooprotect toocloud using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a><span data-ttu-id="4fc49-246">Ange hello Kvarhållningsintervall</span><span class="sxs-lookup"><span data-stu-id="4fc49-246">Setting hello retention range</span></span>
<span data-ttu-id="4fc49-247">Ange hello kvarhållning för hello säkerhetskopiering punkter med hello [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-247">Set hello retention for hello backup points using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="4fc49-248">När det kan verka udda tooset hello kvarhållning innan hello schemat för säkerhetskopiering har definierats med hello ```Set-DPMPolicyObjective``` cmdlet anger automatiskt ett standardschema för säkerhetskopiering som sedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="4fc49-248">While it might seem odd tooset hello retention before hello backup schedule has been defined, using hello ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="4fc49-249">Det är alltid möjligt tooset hello Säkerhetskopieringsschemat först och hello bevarandeprincip efter.</span><span class="sxs-lookup"><span data-stu-id="4fc49-249">It is always possible tooset hello backup schedule first and hello retention policy after.</span></span>

<span data-ttu-id="4fc49-250">I hello exemplet nedan anger hello cmdlet hello kvarhållning parametrar för säkerhetskopiering till disk.</span><span class="sxs-lookup"><span data-stu-id="4fc49-250">In hello example below, hello cmdlet sets hello retention parameters for disk backups.</span></span> <span data-ttu-id="4fc49-251">Detta behåller säkerhetskopior för 10 dagar och synkronisera data var 6 timme mellan hello produktionsservern och hello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="4fc49-251">This will retain backups for 10 days, and sync data every 6 hours between hello production server and hello DPM server.</span></span> <span data-ttu-id="4fc49-252">Hej ```SynchronizationFrequencyMinutes``` inte definierar hur ofta en säkerhetskopiering skapas, men hur ofta data är kopierade toohello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="4fc49-252">hello ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied toohello DPM server.</span></span>  <span data-ttu-id="4fc49-253">Den här inställningen förhindrar att säkerhetskopiorna blir för stor.</span><span class="sxs-lookup"><span data-stu-id="4fc49-253">This setting prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="4fc49-254">För säkerhetskopiering ska tooAzure (DPM refererar toothem som onlinesäkerhetskopieringar) hello kvarhållningsperioder kan konfigureras för [långsiktigt kvarhållning med en farfar-pappa-Son (offentlig sektor GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="4fc49-254">For backups going tooAzure (DPM refers toothem as Online backups) hello retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="4fc49-255">Det vill säga kan du definiera en kombinerad bevarandeprincip som rör varje dag, vecka, månad och år bevarandeprinciper.</span><span class="sxs-lookup"><span data-stu-id="4fc49-255">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="4fc49-256">I det här exemplet vi skapa en matris som representerar hello komplexa kvarhållning schema som vi vill och konfigurera hello Kvarhållningsintervall med hello [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-256">In this example, we create an array representing hello complex retention scheme that we want, and then configure hello retention range using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a><span data-ttu-id="4fc49-257">Ange schemat för säkerhetskopiering av hello</span><span class="sxs-lookup"><span data-stu-id="4fc49-257">Set hello backup schedule</span></span>
<span data-ttu-id="4fc49-258">DPM anger en säkerhetskopiering standardschemat automatiskt om du anger hello skyddsmålet med hello ```Set-DPMPolicyObjective``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-258">DPM sets a default backup schedule automatically if you specify hello protection objective using hello ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="4fc49-259">toochange hello standardscheman används hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet följt av hello [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-259">toochange hello default schedules, use hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="4fc49-260">I hello ovan exempelvis ```$onlineSch``` är en matris med fyra element som innehåller hello befintligt onlineskydd schema för hello Skyddsgruppen i hello GFS schemat:</span><span class="sxs-lookup"><span data-stu-id="4fc49-260">In hello above example, ```$onlineSch``` is an array with four elements that contains hello existing online protection schedule for hello Protection Group in hello GFS scheme:</span></span>

1. <span data-ttu-id="4fc49-261">```$onlineSch[0]```innehåller hello dagsschema</span><span class="sxs-lookup"><span data-stu-id="4fc49-261">```$onlineSch[0]``` contains hello daily schedule</span></span>
2. <span data-ttu-id="4fc49-262">```$onlineSch[1]```innehåller hello veckoschema</span><span class="sxs-lookup"><span data-stu-id="4fc49-262">```$onlineSch[1]``` contains hello weekly schedule</span></span>
3. <span data-ttu-id="4fc49-263">```$onlineSch[2]```innehåller hello månadsschema</span><span class="sxs-lookup"><span data-stu-id="4fc49-263">```$onlineSch[2]``` contains hello monthly schedule</span></span>
4. <span data-ttu-id="4fc49-264">```$onlineSch[3]```innehåller hello årlig schema</span><span class="sxs-lookup"><span data-stu-id="4fc49-264">```$onlineSch[3]``` contains hello yearly schedule</span></span>

<span data-ttu-id="4fc49-265">Om du behöver toomodify hello veckoschema måste du toorefer toohello ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="4fc49-265">So if you need toomodify hello weekly schedule, you need toorefer toohello ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="4fc49-266">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="4fc49-266">Initial backup</span></span>
<span data-ttu-id="4fc49-267">När säkerhetskopiering av en datakälla för hello första gången, DPM måste skapas första replik som skapar en fullständig kopia av hello datasource toobe skyddas på DPM-replikvolymen.</span><span class="sxs-lookup"><span data-stu-id="4fc49-267">When backing up a datasource for hello first time, DPM needs creates initial replica that creates a full copy of hello datasource toobe protected on DPM replica volume.</span></span> <span data-ttu-id="4fc49-268">Den här aktiviteten kan antingen schemaläggas för en viss tid, eller kan aktiveras manuellt med hjälp av hello [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) -cmdlet med parametern hello ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="4fc49-268">This activity can either be scheduled for a specific time, or can be triggered manually, using hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with hello parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="4fc49-269">Ändra hello storlek på DPM-replikeringen & återställningspunktvolymen</span><span class="sxs-lookup"><span data-stu-id="4fc49-269">Changing hello size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="4fc49-270">Du kan också ändra hello storleken på DPM-replikvolymen och skuggkopia av volymen med hjälp av [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet enligt följande exempel hello: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - protectiongroup $MPG-manuell - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="4fc49-270">You can also change hello size of DPM Replica volume and Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in hello following example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-hello-changes-toohello-protection-group"></a><span data-ttu-id="4fc49-271">Genomför hello ändringar toohello Skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="4fc49-271">Committing hello changes toohello Protection Group</span></span>
<span data-ttu-id="4fc49-272">Slutligen måste hello ändringar toobe allokerat innan DPM kan säkerhetskopiera hello per hello ny Skyddsgrupp konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4fc49-272">Finally, hello changes need toobe committed before DPM can take hello backup per hello new Protection Group configuration.</span></span> <span data-ttu-id="4fc49-273">Du kan göra detta med hjälp av hello [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-273">This can be achieved using hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a><span data-ttu-id="4fc49-274">Visa hello säkerhetskopiering punkter</span><span class="sxs-lookup"><span data-stu-id="4fc49-274">View hello backup points</span></span>
<span data-ttu-id="4fc49-275">Du kan använda hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget en lista över alla återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="4fc49-275">You can use hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget a list of all recovery points for a datasource.</span></span> <span data-ttu-id="4fc49-276">I det här exemplet ska du:</span><span class="sxs-lookup"><span data-stu-id="4fc49-276">In this example, we will:</span></span>

* <span data-ttu-id="4fc49-277">Hämta alla hello PGs på hello DPM-servern och lagras i en matris```$PG```</span><span class="sxs-lookup"><span data-stu-id="4fc49-277">fetch all hello PGs on hello DPM server and stored in an array ```$PG```</span></span>
* <span data-ttu-id="4fc49-278">Hämta hello datakällor motsvarande toohello```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="4fc49-278">get hello datasources corresponding toohello ```$PG[0]```</span></span>
* <span data-ttu-id="4fc49-279">Hämta alla hello Återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="4fc49-279">get all hello recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="4fc49-280">Återställa skyddade data på Azure</span><span class="sxs-lookup"><span data-stu-id="4fc49-280">Restore data protected on Azure</span></span>
<span data-ttu-id="4fc49-281">Återställa data är en kombination av en ```RecoverableItem``` objekt och en ```RecoveryOption``` objekt.</span><span class="sxs-lookup"><span data-stu-id="4fc49-281">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="4fc49-282">I föregående avsnitt hello vi en lista över hello säkerhetskopiering punkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="4fc49-282">In hello previous section, we got a list of hello backup points for a datasource.</span></span>

<span data-ttu-id="4fc49-283">I hello exemplet nedan visar hur toorestore Hyper-V virtuell dator från Azure Backup genom att kombinera säkerhetskopiering punkter med hello mål för återställning.</span><span class="sxs-lookup"><span data-stu-id="4fc49-283">In hello example below, we demonstrate how toorestore a Hyper-V virtual machine from Azure Backup by combining backup points with hello target for recovery.</span></span> <span data-ttu-id="4fc49-284">Det här exemplet innehåller:</span><span class="sxs-lookup"><span data-stu-id="4fc49-284">This example includes:</span></span>

* <span data-ttu-id="4fc49-285">Skapa ett återställningsalternativ med hello [ny DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-285">Creating a recovery option using hello  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="4fc49-286">Hämtning hello punktmatrisen säkerhetskopiering med hello ```Get-DPMRecoveryPoint``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4fc49-286">Fetching hello array of backup points using hello ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="4fc49-287">Om du väljer en säkerhetskopiering punkt toorestore från.</span><span class="sxs-lookup"><span data-stu-id="4fc49-287">Choosing a backup point toorestore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="4fc49-288">hello kommandona kan enkelt utökas för någon typ av datakälla.</span><span class="sxs-lookup"><span data-stu-id="4fc49-288">hello commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fc49-289">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fc49-289">Next steps</span></span>
* <span data-ttu-id="4fc49-290">Mer information om DPM tooAzure säkerhetskopiering finns [introduktion tooDPM säkerhetskopiering](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="4fc49-290">For more information about DPM tooAzure Backup see [Introduction tooDPM Backup](backup-azure-dpm-introduction.md)</span></span>
