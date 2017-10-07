---
title: "Azure-säkerhetskopiering: Använd PowerShell tooback in DPM-arbetsbelastningar | Microsoft Docs"
description: "Lär dig hur toodeploy och hantera Azure Backup för Data Protection Manager (DPM) med hjälp av PowerShell"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 48ebe6b520857836e89749ffb6fe83d1f14c5597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="beb7a-103">Distribuera och hantera säkerhetskopiering tooAzure för Data Protection Manager (DPM) servrar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="beb7a-103">Deploy and manage backup tooAzure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="beb7a-104">ARM</span><span class="sxs-lookup"><span data-stu-id="beb7a-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="beb7a-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="beb7a-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="beb7a-106">Den här artikeln förklarar hur toouse PowerShell tooback upp och återställa DPM-data från ett säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="beb7a-106">This article explains how toouse PowerShell tooback up and recover DPM data from a backup vault.</span></span> <span data-ttu-id="beb7a-107">Microsoft rekommenderar att du använder Recovery Services-valv för alla nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="beb7a-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="beb7a-108">Om du är en ny Azure Backup-användare kan använda hello artikel [distribuera och hantera Data Protection Manager data tooAzure med hjälp av PowerShell](backup-dpm-automation.md), så att du lagrar data i en Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-108">If you are a new Azure Backup user, use hello article, [Deploy and manage Data Protection Manager data tooAzure using PowerShell](backup-dpm-automation.md), so you store your data in a Recovery Services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="beb7a-109">Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="beb7a-109">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="beb7a-110">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="beb7a-110">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="beb7a-111">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="beb7a-111">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="beb7a-112">Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017.</span><span class="sxs-lookup"><span data-stu-id="beb7a-112">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="beb7a-113">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="beb7a-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="beb7a-114">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="beb7a-114">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="beb7a-115">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-115">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="beb7a-116">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="beb7a-116">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="setting-up-hello-powershell-environment"></a><span data-ttu-id="beb7a-117">Konfigurera hello PowerShell-miljö</span><span class="sxs-lookup"><span data-stu-id="beb7a-117">Setting up hello PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="beb7a-118">Innan du kan använda PowerShell toomanage säkerhetskopior från Data Protection Manager tooAzure måste toohave hello rätt miljö i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="beb7a-118">Before you can use PowerShell toomanage backups from Data Protection Manager tooAzure, you will need toohave hello right environment in PowerShell.</span></span> <span data-ttu-id="beb7a-119">Kontrollera att du kör hello efter kommandot tooimport hello rätt moduler och Tillåt toocorrectly referens hello DPM cmdlets hello början av hello PowerShell-session:</span><span class="sxs-lookup"><span data-stu-id="beb7a-119">At hello start of hello PowerShell session, ensure that you run hello following command tooimport hello right modules and allow you toocorrectly reference hello DPM cmdlets:</span></span>

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

## <a name="setup-and-registration"></a><span data-ttu-id="beb7a-120">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="beb7a-120">Setup and Registration</span></span>
<span data-ttu-id="beb7a-121">toobegin:</span><span class="sxs-lookup"><span data-stu-id="beb7a-121">toobegin:</span></span>

1. <span data-ttu-id="beb7a-122">[Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="beb7a-122">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="beb7a-123">Aktivera hello Azure Backup-kommandon genom att växla för*AzureResourceManager* läge med hjälp av hello **Switch-AzureMode** cmdleten igen:</span><span class="sxs-lookup"><span data-stu-id="beb7a-123">Enable hello Azure Backup commandlets by switching too*AzureResourceManager* mode by using hello **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="beb7a-124">hello kan följande inställningar och registrering aktiviteter automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="beb7a-124">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="beb7a-125">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="beb7a-125">Create a backup vault</span></span>
* <span data-ttu-id="beb7a-126">Installera hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="beb7a-126">Installing hello Azure Backup agent</span></span>
* <span data-ttu-id="beb7a-127">Registrera med hello Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="beb7a-127">Registering with hello Azure Backup service</span></span>
* <span data-ttu-id="beb7a-128">Nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="beb7a-128">Networking settings</span></span>
* <span data-ttu-id="beb7a-129">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="beb7a-129">Encryption settings</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="beb7a-130">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="beb7a-130">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="beb7a-131">För kunder som använder Azure Backup för hello första gången, måste tooregister hello Azure Backup-providern toobe används med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="beb7a-131">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="beb7a-132">Detta kan göras genom att köra följande kommando hello: registrera AzureProvider - ProviderNamespace ”Microsoft.Backup”</span><span class="sxs-lookup"><span data-stu-id="beb7a-132">This can be done by running hello following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="beb7a-133">Du kan skapa ett nytt säkerhetskopieringsvalv med hello **ny AzureRMBackupVault** cmdleten igen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-133">You can create a new backup vault using hello **New-AzureRMBackupVault** commandlet.</span></span> <span data-ttu-id="beb7a-134">Hej säkerhetskopieringsvalvet är en ARM-resurs, så du måste tooplace den inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="beb7a-134">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="beb7a-135">Kör följande kommandon hello i en upphöjd Azure PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="beb7a-135">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

<span data-ttu-id="beb7a-136">Du kan hämta en lista över alla hello säkerhetskopieringsvalv i en viss prenumeration med hjälp av hello **Get-AzureRMBackupVault** cmdleten igen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-136">You can get a list of all hello backup vaults in a given subscription using hello **Get-AzureRMBackupVault** commandlet.</span></span>

### <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="beb7a-137">Hello Azure Backup agent installeras på en DPM-Server</span><span class="sxs-lookup"><span data-stu-id="beb7a-137">Installing hello Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="beb7a-138">Innan du installerar hello Azure Backup-agenten måste toohave hello installer hämtats och finns på hello Windows Server.</span><span class="sxs-lookup"><span data-stu-id="beb7a-138">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="beb7a-139">Du kan hämta hello senaste versionen av hello installer från hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från hello säkerhetskopieringsvalvet instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="beb7a-139">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello backup vault's Dashboard page.</span></span> <span data-ttu-id="beb7a-140">Spara hello installer tooan lättillgänglig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="beb7a-140">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="beb7a-141">tooinstall hello agent, kör följande kommando i en upphöjd PowerShell-konsol hello **på hello DPM-servern**:</span><span class="sxs-lookup"><span data-stu-id="beb7a-141">tooinstall hello agent, run hello following command in an elevated PowerShell console **on hello DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="beb7a-142">Detta installerar hello agent med alla hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-142">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="beb7a-143">hello installationen tar några minuter i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="beb7a-143">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="beb7a-144">Om du inte anger hello */nu* alternativet hello **Windows Update** öppnas hello slutet av hello installation toocheck för alla uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="beb7a-144">If you do not specify hello */nu* option hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span>

<span data-ttu-id="beb7a-145">hello agenten visas i hello listan över installerade program.</span><span class="sxs-lookup"><span data-stu-id="beb7a-145">hello agent will show in hello list of installed programs.</span></span> <span data-ttu-id="beb7a-146">toosee hello listan över installerade program finns för**Kontrollpanelen** > **program** > **program och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="beb7a-146">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Agenten är installerad](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a><span data-ttu-id="beb7a-148">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="beb7a-148">Installation options</span></span>
<span data-ttu-id="beb7a-149">toosee alla hello alternativ som är tillgängliga via hello kommandoradsverktyg, använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="beb7a-149">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="beb7a-150">hello tillgängliga alternativ inkluderar:</span><span class="sxs-lookup"><span data-stu-id="beb7a-150">hello available options include:</span></span>

| <span data-ttu-id="beb7a-151">Alternativ</span><span class="sxs-lookup"><span data-stu-id="beb7a-151">Option</span></span> | <span data-ttu-id="beb7a-152">Information</span><span class="sxs-lookup"><span data-stu-id="beb7a-152">Details</span></span> | <span data-ttu-id="beb7a-153">Standard</span><span class="sxs-lookup"><span data-stu-id="beb7a-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="beb7a-154">/q</span><span class="sxs-lookup"><span data-stu-id="beb7a-154">/q</span></span> |<span data-ttu-id="beb7a-155">Tyst installation</span><span class="sxs-lookup"><span data-stu-id="beb7a-155">Quiet installation</span></span> |- |
| <span data-ttu-id="beb7a-156">/ p: ”plats”</span><span class="sxs-lookup"><span data-stu-id="beb7a-156">/p:"location"</span></span> |<span data-ttu-id="beb7a-157">Sökvägen toohello installationsmappen för hello Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="beb7a-157">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="beb7a-158">C:\Program Files\Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="beb7a-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="beb7a-159">/ s: ”plats”</span><span class="sxs-lookup"><span data-stu-id="beb7a-159">/s:"location"</span></span> |<span data-ttu-id="beb7a-160">Sökvägen toohello cachemappen för hello Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="beb7a-160">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="beb7a-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="beb7a-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="beb7a-162">/m</span><span class="sxs-lookup"><span data-stu-id="beb7a-162">/m</span></span> |<span data-ttu-id="beb7a-163">Anmäl tooMicrosoft uppdatering</span><span class="sxs-lookup"><span data-stu-id="beb7a-163">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="beb7a-164">/nu</span><span class="sxs-lookup"><span data-stu-id="beb7a-164">/nu</span></span> |<span data-ttu-id="beb7a-165">Sök inte efter uppdateringar när installationen är klar</span><span class="sxs-lookup"><span data-stu-id="beb7a-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="beb7a-166">/d</span><span class="sxs-lookup"><span data-stu-id="beb7a-166">/d</span></span> |<span data-ttu-id="beb7a-167">Avinstallerar Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="beb7a-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="beb7a-168">/pH</span><span class="sxs-lookup"><span data-stu-id="beb7a-168">/ph</span></span> |<span data-ttu-id="beb7a-169">Värden proxyadress</span><span class="sxs-lookup"><span data-stu-id="beb7a-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="beb7a-170">/po</span><span class="sxs-lookup"><span data-stu-id="beb7a-170">/po</span></span> |<span data-ttu-id="beb7a-171">Portnummer för proxyservern värden</span><span class="sxs-lookup"><span data-stu-id="beb7a-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="beb7a-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="beb7a-172">/pu</span></span> |<span data-ttu-id="beb7a-173">Värddatorn Proxyanvändarnamnet</span><span class="sxs-lookup"><span data-stu-id="beb7a-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="beb7a-174">/PW</span><span class="sxs-lookup"><span data-stu-id="beb7a-174">/pw</span></span> |<span data-ttu-id="beb7a-175">Lösenord för proxy</span><span class="sxs-lookup"><span data-stu-id="beb7a-175">Proxy Password</span></span> |- |

### <a name="registering-with-hello-azure-backup-service"></a><span data-ttu-id="beb7a-176">Registrera med hello Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="beb7a-176">Registering with hello Azure Backup service</span></span>
<span data-ttu-id="beb7a-177">Innan du kan registrera med hello Azure Backup-tjänsten måste tooensure som hello [krav](backup-azure-dpm-introduction.md) är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="beb7a-177">Before you can register with hello Azure Backup service, you need tooensure that hello [prerequisites](backup-azure-dpm-introduction.md) are met.</span></span> <span data-ttu-id="beb7a-178">Du måste:</span><span class="sxs-lookup"><span data-stu-id="beb7a-178">You must:</span></span>

* <span data-ttu-id="beb7a-179">Har ett giltigt Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="beb7a-179">Have a valid Azure subscription</span></span>
* <span data-ttu-id="beb7a-180">Har ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="beb7a-180">Have a backup vault</span></span>

<span data-ttu-id="beb7a-181">toodownload hello valvautentiseringsuppgifter som kör hello **Get-AzureBackupVaultCredentials** kommandot i en Azure PowerShell-konsolen och lagra den i en lämplig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="beb7a-181">toodownload hello vault credentials, run hello **Get-AzureBackupVaultCredentials** commandlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="beb7a-182">Registrera hello-dator med hello valvet görs med hjälp av hello [Start DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="beb7a-182">Registering hello machine with hello vault is done using hello [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

<span data-ttu-id="beb7a-183">Det kommer att registrera hello DPM-servern med namnet ”TestingServer” med Microsoft Azure-valvet med hello angetts valvet autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="beb7a-183">This will register hello DPM Server named “TestingServer” with Microsoft Azure Vault using hello specified vault credentials.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="beb7a-184">Du inte använda relativa sökvägar toospecify hello valvautentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-184">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="beb7a-185">Du måste ange en absolut sökväg som en inkommande toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-185">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

### <a name="initial-configuration-settings"></a><span data-ttu-id="beb7a-186">Inledande konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="beb7a-186">Initial configuration settings</span></span>
<span data-ttu-id="beb7a-187">När hello DPM-servern registreras med hello Azure Backup-valvet, startar den med standardinställningar för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-187">Once hello DPM Server is registered with hello Azure Backup vault, it will start with default subscription settings.</span></span> <span data-ttu-id="beb7a-188">Inställningarna prenumeration omfattar nätverk, kryptering och hello mellanlagringsområdet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-188">These subscription settings include Networking, Encryption and hello Staging area.</span></span> <span data-ttu-id="beb7a-189">toobegin ändra hello prenumerationsinställningar toofirst måste få en referens för hello befintliga (standard)-inställningar med hjälp av hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="beb7a-189">toobegin changing hello subscription settings you need toofirst get a handle on hello existing (default) settings using hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="beb7a-190">Alla ändringar har gjorts toothis lokala PowerShell-objektet ```$setting``` och sedan hello fullt objekt är allokerat tooDPM och Azure Backup toosave dem med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-190">All modifications are made toothis local PowerShell object ```$setting```  and then hello full object is committed tooDPM and Azure Backup toosave them using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="beb7a-191">Du behöver toouse hello ```–Commit``` flaggan tooensure som hello ändringar sparas.</span><span class="sxs-lookup"><span data-stu-id="beb7a-191">You need toouse hello ```–Commit``` flag tooensure that hello changes are persisted.</span></span> <span data-ttu-id="beb7a-192">hello-inställningarna kommer inte tillämpas och används av Azure Backup såvida inte genomförts.</span><span class="sxs-lookup"><span data-stu-id="beb7a-192">hello settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a><span data-ttu-id="beb7a-193">Nätverk</span><span class="sxs-lookup"><span data-stu-id="beb7a-193">Networking</span></span>
<span data-ttu-id="beb7a-194">Om hello anslutningen för hello DPM datorn toohello Azure Backup-tjänsten på hello internet via en proxyserver, bör hello proxyserverinställningar anges för säkerhetskopieringar toosucceed.</span><span class="sxs-lookup"><span data-stu-id="beb7a-194">If hello connectivity of hello DPM machine toohello Azure Backup service on hello internet is through a proxy server, then hello proxy server settings should be provided for backups toosucceed.</span></span> <span data-ttu-id="beb7a-195">Detta görs med hjälp av hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` och hello ```ProxyPassword``` parametrar med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-195">This is done by using hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` and hello ```ProxyPassword``` parameters with hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="beb7a-196">I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.</span><span class="sxs-lookup"><span data-stu-id="beb7a-196">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="beb7a-197">Bandbreddsanvändning kan också kontrolleras med alternativ för ```-WorkHourBandwidth``` och ```-NonWorkHourBandwidth``` för en given uppsättning hello veckodagar.</span><span class="sxs-lookup"><span data-stu-id="beb7a-197">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of hello week.</span></span> <span data-ttu-id="beb7a-198">I det här exemplet anger vi inte någon begränsning.</span><span class="sxs-lookup"><span data-stu-id="beb7a-198">In this example we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-hello-staging-area"></a><span data-ttu-id="beb7a-199">Konfigurera hello mellanlagringsområde</span><span class="sxs-lookup"><span data-stu-id="beb7a-199">Configuring hello staging Area</span></span>
<span data-ttu-id="beb7a-200">hello Azure Backup-agenten körs på hello DPM-servern behöver tillfällig lagring för data som återställs från hello molnet (lokalt mellanlagringsområde).</span><span class="sxs-lookup"><span data-stu-id="beb7a-200">hello Azure Backup agent running on hello DPM server needs temporary storage for data restored from hello cloud (local staging area).</span></span> <span data-ttu-id="beb7a-201">Konfigurera hello mellanlagringsområdet med hello [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet och hello ```-StagingAreaPath``` parameter.</span><span class="sxs-lookup"><span data-stu-id="beb7a-201">Configure hello staging area using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and hello ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="beb7a-202">I hello-exemplet ovan, hello mellanlagringsområde ska anges för*C:\StagingArea* i hello PowerShell-objektet ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="beb7a-202">In hello example above, hello staging area will be set too*C:\StagingArea* in hello PowerShell object ```$setting```.</span></span> <span data-ttu-id="beb7a-203">Se till att hello mappen redan finns, annars hello slutliga genomförande av hello prenumerationsinställningar misslyckas.</span><span class="sxs-lookup"><span data-stu-id="beb7a-203">Ensure that hello specified folder already exists, or else hello final commit of hello subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="beb7a-204">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="beb7a-204">Encryption settings</span></span>
<span data-ttu-id="beb7a-205">hello säkerhetskopierade data skickas tooAzure säkerhetskopiering är krypterad tooprotect hello sekretess hello.</span><span class="sxs-lookup"><span data-stu-id="beb7a-205">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="beb7a-206">Hej krypteringslösenfrasen har hello ”password” toodecrypt hello data vid hello tiden för återställning.</span><span class="sxs-lookup"><span data-stu-id="beb7a-206">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span> <span data-ttu-id="beb7a-207">Det är viktigt tookeep denna information säkert och säker när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="beb7a-207">It is important tookeep this information safe and secure once it is set.</span></span>

<span data-ttu-id="beb7a-208">I hello exemplet nedan hello första kommandot konverterar hello ```passphrase123456789``` tooa säker sträng och tilldelar hello säker toohello variabel med namnet ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="beb7a-208">In hello example below, hello first command converts hello string ```passphrase123456789``` tooa secure string and assigns hello secure string toohello variable named ```$Passphrase```.</span></span> <span data-ttu-id="beb7a-209">hello andra kommandot anger hello säker sträng i ```$Passphrase``` som hello lösenord för kryptering av säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="beb7a-209">hello second command sets hello secure string in ```$Passphrase``` as hello password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="beb7a-210">Hålla hello lösenfras information säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="beb7a-210">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="beb7a-211">Du kommer inte att kunna toorestore data från Azure utan lösenfras.</span><span class="sxs-lookup"><span data-stu-id="beb7a-211">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="beb7a-212">Nu ska du har gjort alla hello krävs ändringar toohello ```$setting``` objekt.</span><span class="sxs-lookup"><span data-stu-id="beb7a-212">At this point, you should have made all hello required changes toohello ```$setting``` object.</span></span> <span data-ttu-id="beb7a-213">Kom ihåg toocommit hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="beb7a-213">Remember toocommit hello changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a><span data-ttu-id="beb7a-214">Skydda data tooAzure säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="beb7a-214">Protect data tooAzure Backup</span></span>
<span data-ttu-id="beb7a-215">I det här avsnittet kan du lägga till en server tooDPM för produktion och skydda hello datalagring toolocal DPM och tooAzure säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="beb7a-215">In this section, you will add a production server tooDPM and then protect hello data toolocal DPM storage and then tooAzure Backup.</span></span> <span data-ttu-id="beb7a-216">I hello exemplen visar vi hur tooback av filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="beb7a-216">In hello examples we will demonstrate how tooback up files and folders.</span></span> <span data-ttu-id="beb7a-217">hello logik kan enkelt att utökade toobackup alla stöd för DPM-datakällor.</span><span class="sxs-lookup"><span data-stu-id="beb7a-217">hello logic can easily be extended toobackup any DPM-supported data source.</span></span> <span data-ttu-id="beb7a-218">Alla DPM-säkerhetskopieringar styrs av en skydd grupp (PG) med fyra delar:</span><span class="sxs-lookup"><span data-stu-id="beb7a-218">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="beb7a-219">**Medlemmar** är en lista över alla hello skyddsobjekt (även kallat *datakällor* i DPM) som du vill tooprotect i hello samma skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="beb7a-219">**Group members** is a list of all hello protectable objects (also known as *Datasources* in DPM) that you want tooprotect in hello same protection group.</span></span> <span data-ttu-id="beb7a-220">Exempelvis kanske tooprotect produktion virtuella datorer i en skyddsgrupp och SQL Server-databaser i en annan skyddsgrupp som de kan ha olika krav för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="beb7a-220">For example, you may want tooprotect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="beb7a-221">Innan du kan säkerhetskopiera någon datakälla på en produktionsserver måste toomake att hello DPM-agenten är installerad på hello server och hanteras av DPM.</span><span class="sxs-lookup"><span data-stu-id="beb7a-221">Before you can back up any datasource on a production server you need toomake sure hello DPM Agent is installed on hello server and is managed by DPM.</span></span> <span data-ttu-id="beb7a-222">Hello gör för [installerar hello DPM-agenten](https://technet.microsoft.com/library/bb870935.aspx) och länka den toohello lämpliga DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="beb7a-222">Follow hello steps for [installing hello DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it toohello appropriate DPM Server.</span></span>
2. <span data-ttu-id="beb7a-223">**Dataskyddsmetod** anger hello mål platser - band, disk och molnet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-223">**Data protection method** specifies hello target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="beb7a-224">I vårt exempel skyddar vi data toohello lokal disk och toohello moln.</span><span class="sxs-lookup"><span data-stu-id="beb7a-224">In our example we will protect data toohello local disk and toohello cloud.</span></span>
3. <span data-ttu-id="beb7a-225">En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver toobe tas och hur ofta hello data ska synkroniseras mellan hello DPM-servern och hello produktionsservern.</span><span class="sxs-lookup"><span data-stu-id="beb7a-225">A **backup schedule** that specifies when backups need toobe taken and how often hello data should be synchronized between hello DPM Server and hello production server.</span></span>
4. <span data-ttu-id="beb7a-226">En **bevarandeschema** som anger hur länge tooretain hello Återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="beb7a-226">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="beb7a-227">Skapa en skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="beb7a-227">Creating a protection group</span></span>
<span data-ttu-id="beb7a-228">Börja med att skapa en ny Skyddsgrupp med hello [ny DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-228">Start by creating a new Protection Group using hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="beb7a-229">hello ovan cmdlet skapar en Skyddsgrupp med namnet *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="beb7a-229">hello above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="beb7a-230">En befintlig skyddsgrupp kan också ändras senare tooadd säkerhetskopiering toohello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-230">An existing protection group can also be modified later tooadd backup toohello Azure cloud.</span></span> <span data-ttu-id="beb7a-231">Dock toomake ändringar toohello Skyddsgrupp – nya eller befintliga - vi behöver tooget en referens i en *ändringsbar* objekt med hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-231">However, toomake any changes toohello Protection Group - new or existing - we need tooget a handle on a *modifiable* object using hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a><span data-ttu-id="beb7a-232">Att lägga till gruppen medlemmar toohello Skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="beb7a-232">Adding group members toohello Protection Group</span></span>
<span data-ttu-id="beb7a-233">Varje DPM-agenten vet hello lista över datakällor på hello-server som den är installerad på.</span><span class="sxs-lookup"><span data-stu-id="beb7a-233">Each DPM Agent knows hello list of datasources on hello server that it is installed on.</span></span> <span data-ttu-id="beb7a-234">tooadd datasource-toohello Skyddsgrupp hello DPM-agenten måste toofirst skicka en lista över hello datakällor tillbaka toohello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="beb7a-234">tooadd a datasource toohello Protection Group, hello DPM Agent needs toofirst send a list of hello datasources back toohello DPM server.</span></span> <span data-ttu-id="beb7a-235">En eller flera datakällor är markerade och tillagda toohello Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-235">One or more datasources are then selected and added toohello Protection Group.</span></span> <span data-ttu-id="beb7a-236">hello PowerShell steg som krävs för tooget uppnå detta är:</span><span class="sxs-lookup"><span data-stu-id="beb7a-236">hello PowerShell steps needed tooget achieve this are:</span></span>

1. <span data-ttu-id="beb7a-237">Hämta en lista över alla servrar som hanteras av DPM via hello DPM-agenten.</span><span class="sxs-lookup"><span data-stu-id="beb7a-237">Fetch a list of all servers managed by DPM through hello DPM Agent.</span></span>
2. <span data-ttu-id="beb7a-238">Välj en specifik server.</span><span class="sxs-lookup"><span data-stu-id="beb7a-238">Choose a specific server.</span></span>
3. <span data-ttu-id="beb7a-239">Hämta en lista över alla datakällor på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="beb7a-239">Fetch a list of all datasources on hello server.</span></span>
4. <span data-ttu-id="beb7a-240">Välj en eller flera datakällor och lägga till dem toohello Skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="beb7a-240">Choose one or more datasources and add them toohello Protection Group</span></span>

<span data-ttu-id="beb7a-241">hello listan över servrar på vilka hello DPM-agenten är installerad och hanteras av hello DPM-servern förvärvas med hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-241">hello list of servers on which hello DPM Agent is installed and is being managed by hello DPM Server is acquired with hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="beb7a-242">I det här exemplet kommer att filtrera och bara konfigurera PS med namnet *productionserver01* för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="beb7a-242">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="beb7a-243">Hämta nu hello lista över datakällor på ```$server``` med hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-243">Now fetch hello list of datasources on ```$server``` using hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="beb7a-244">I det här exemplet vi filtrering för hello volym * D:\* som vi vill tooconfigure för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="beb7a-244">In this example we are filtering for hello volume *D:\* which we want tooconfigure for backup.</span></span> <span data-ttu-id="beb7a-245">Datakällan läggs sedan toohello Skyddsgruppen med hjälp av hello [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-245">This datasource is then added toohello Protection Group using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="beb7a-246">Kom ihåg toouse hello *modifable* skydd gruppobjekt ```$MPG``` toomake hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="beb7a-246">Remember toouse hello *modifable* protection group object ```$MPG``` toomake hello additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="beb7a-247">Upprepa detta steg så många gånger som krävs, tills du har lagt till alla hello valt datakällor toohello skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-247">Repeat this step as many times as required, until you have added all hello chosen datasources toohello protection group.</span></span> <span data-ttu-id="beb7a-248">Du kan också starta med en datakälla och fullständig hello arbetsflöde för att skapa hello Skyddsgruppen och senare lägga till flera datakällor toohello Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-248">You can also start with just one datasource, and complete hello workflow for creating hello Protection Group, and at a later point add more datasources toohello Protection Group.</span></span>

### <a name="selecting-hello-data-protection-method"></a><span data-ttu-id="beb7a-249">Att välja hello dataskyddsmetod</span><span class="sxs-lookup"><span data-stu-id="beb7a-249">Selecting hello data protection method</span></span>
<span data-ttu-id="beb7a-250">När hello datakällorna har lagts till toohello Skyddsgrupp, hello nästa steg är toospecify hello skyddsmetod med hello [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-250">Once hello datasources have been added toohello Protection Group, hello next step is toospecify hello protection method using hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="beb7a-251">I det här exemplet hello Skyddsgruppen inte inställningar för lokal disk och säkerhetskopiering i molnet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-251">In this example, hello Protection Group will be setup for local disk and cloud backup.</span></span> <span data-ttu-id="beb7a-252">Du måste också toospecify hello datakällan som du vill tooprotect toocloud med hello [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet med - Online flagga.</span><span class="sxs-lookup"><span data-stu-id="beb7a-252">You also need toospecify hello datasource that you want tooprotect toocloud using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a><span data-ttu-id="beb7a-253">Ange hello Kvarhållningsintervall</span><span class="sxs-lookup"><span data-stu-id="beb7a-253">Setting hello retention range</span></span>
<span data-ttu-id="beb7a-254">Ange hello kvarhållning för hello säkerhetskopiering punkter med hello [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-254">Set hello retention for hello backup points using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="beb7a-255">När det kan verka udda tooset hello kvarhållning innan hello schemat för säkerhetskopiering har definierats med hello ```Set-DPMPolicyObjective``` cmdlet anger automatiskt ett standardschema för säkerhetskopiering som sedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="beb7a-255">While it might seem odd tooset hello retention before hello backup schedule has been defined, using hello ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="beb7a-256">Det är alltid möjligt tooset hello Säkerhetskopieringsschemat först och hello bevarandeprincip efter.</span><span class="sxs-lookup"><span data-stu-id="beb7a-256">It is always possible tooset hello backup schedule first and hello retention policy after.</span></span>

<span data-ttu-id="beb7a-257">I hello exemplet nedan anger hello cmdlet hello kvarhållning parametrar för säkerhetskopiering till disk.</span><span class="sxs-lookup"><span data-stu-id="beb7a-257">In hello example below, hello cmdlet sets hello retention parameters for disk backups.</span></span> <span data-ttu-id="beb7a-258">Detta behåller säkerhetskopior för 10 dagar och synkronisera data var 6 timme mellan hello produktionsservern och hello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="beb7a-258">This will retain backups for 10 days, and sync data every 6 hours between hello production server and hello DPM server.</span></span> <span data-ttu-id="beb7a-259">Hej ```SynchronizationFrequencyMinutes``` inte definierar hur ofta en säkerhetskopiering skapas, men hur ofta data är kopierade toohello DPM-servern; Detta förhindrar att säkerhetskopiorna blir för stor.</span><span class="sxs-lookup"><span data-stu-id="beb7a-259">hello ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied toohello DPM server; this prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="beb7a-260">För säkerhetskopiering ska tooAzure (DPM refererar toothese som onlinesäkerhetskopieringar) hello kvarhållningsperioder kan konfigureras för [långsiktigt kvarhållning med en farfar-pappa-Son (offentlig sektor GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="beb7a-260">For backups going tooAzure (DPM refers toothese as Online backups) hello retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="beb7a-261">Det vill säga kan du definiera en kombinerad bevarandeprincip som rör varje dag, vecka, månad och år bevarandeprinciper.</span><span class="sxs-lookup"><span data-stu-id="beb7a-261">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="beb7a-262">I det här exemplet vi skapa en matris som representerar hello komplexa kvarhållning schema som vi vill och konfigurera hello Kvarhållningsintervall med hello [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-262">In this example, we create an array representing hello complex retention scheme that we want, and then configure hello retention range using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a><span data-ttu-id="beb7a-263">Ange schemat för säkerhetskopiering av hello</span><span class="sxs-lookup"><span data-stu-id="beb7a-263">Set hello backup schedule</span></span>
<span data-ttu-id="beb7a-264">DPM anger en säkerhetskopiering standardschemat automatiskt om du anger hello skyddsmålet med hello ```Set-DPMPolicyObjective``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-264">DPM sets a default backup schedule automatically if you specify hello protection objective using hello ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="beb7a-265">toochange hello standardscheman används hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet följt av hello [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-265">toochange hello default schedules, use hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="beb7a-266">I hello-exemplet ovan, ```$onlineSch``` är en matris med fyra element som innehåller hello befintligt onlineskydd schema för hello Skyddsgruppen i hello GFS schemat:</span><span class="sxs-lookup"><span data-stu-id="beb7a-266">In hello example above, ```$onlineSch``` is an array with four elements that contains hello existing online protection schedule for hello Protection Group in hello GFS scheme:</span></span>

1. <span data-ttu-id="beb7a-267">```$onlineSch[0]```innehåller hello dagsschema</span><span class="sxs-lookup"><span data-stu-id="beb7a-267">```$onlineSch[0]``` will contain hello daily schedule</span></span>
2. <span data-ttu-id="beb7a-268">```$onlineSch[1]```innehåller hello veckoschema</span><span class="sxs-lookup"><span data-stu-id="beb7a-268">```$onlineSch[1]``` will contain hello weekly schedule</span></span>
3. <span data-ttu-id="beb7a-269">```$onlineSch[2]```innehåller hello månadsschema</span><span class="sxs-lookup"><span data-stu-id="beb7a-269">```$onlineSch[2]``` will contain hello monthly schedule</span></span>
4. <span data-ttu-id="beb7a-270">```$onlineSch[3]```innehåller hello årlig schema</span><span class="sxs-lookup"><span data-stu-id="beb7a-270">```$onlineSch[3]``` will contain hello yearly schedule</span></span>

<span data-ttu-id="beb7a-271">Om du behöver toomodify hello veckoschema måste du toorefer toohello ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="beb7a-271">So if you need toomodify hello weekly schedule, you need toorefer toohello ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="beb7a-272">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="beb7a-272">Initial backup</span></span>
<span data-ttu-id="beb7a-273">När du säkerhetskopierar en datakälla för hello första gången, måste DPM toocreate en första replik som kommer att skapa en kopia av hello datasource toobe skyddas på DPM-replikvolymen.</span><span class="sxs-lookup"><span data-stu-id="beb7a-273">When backing up a datasource for hello first time, DPM needs toocreate an initial replica which will create a copy of hello datasource toobe protected on DPM replica volume.</span></span> <span data-ttu-id="beb7a-274">Den här aktiviteten kan antingen schemaläggas för en viss tid, eller kan aktiveras manuellt med hjälp av hello [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) -cmdlet med parametern hello ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="beb7a-274">This activity can either be scheduled for a specific time, or can be triggered manually, using hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with hello parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="beb7a-275">Ändra hello storlek på DPM-replikeringen & återställningspunktvolymen</span><span class="sxs-lookup"><span data-stu-id="beb7a-275">Changing hello size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="beb7a-276">Du kan också ändra hello storleken på DPM-replikvolymen samt skuggkopia av volymen med hjälp av [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet som hello exemplet nedan: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - protectiongroup $MPG-manuell - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="beb7a-276">You can also change hello size of DPM Replica volume as well as Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in hello below example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-hello-changes-toohello-protection-group"></a><span data-ttu-id="beb7a-277">Genomför hello ändringar toohello Skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="beb7a-277">Committing hello changes toohello Protection Group</span></span>
<span data-ttu-id="beb7a-278">Slutligen måste hello ändringar toobe allokerat innan DPM kan säkerhetskopiera hello per hello ny Skyddsgrupp konfiguration.</span><span class="sxs-lookup"><span data-stu-id="beb7a-278">Finally, hello changes need toobe committed before DPM can take hello backup per hello new Protection Group configuration.</span></span> <span data-ttu-id="beb7a-279">Detta görs med hjälp av hello [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-279">This is done using hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a><span data-ttu-id="beb7a-280">Visa hello säkerhetskopiering punkter</span><span class="sxs-lookup"><span data-stu-id="beb7a-280">View hello backup points</span></span>
<span data-ttu-id="beb7a-281">Du kan använda hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget en lista över alla återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="beb7a-281">You can use hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget a list of all recovery points for a datasource.</span></span> <span data-ttu-id="beb7a-282">I det här exemplet ska du:</span><span class="sxs-lookup"><span data-stu-id="beb7a-282">In this example, we will:</span></span>

* <span data-ttu-id="beb7a-283">Hämta alla hello PGs på hello DPM-server som kommer att lagras i en matris```$PG```</span><span class="sxs-lookup"><span data-stu-id="beb7a-283">fetch all hello PGs on hello DPM server which will be stored in an array ```$PG```</span></span>
* <span data-ttu-id="beb7a-284">Hämta hello datakällor motsvarande toohello```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="beb7a-284">get hello datasources corresponding toohello ```$PG[0]```</span></span>
* <span data-ttu-id="beb7a-285">Hämta alla hello Återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="beb7a-285">get all hello recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="beb7a-286">Återställa skyddade data på Azure</span><span class="sxs-lookup"><span data-stu-id="beb7a-286">Restore data protected on Azure</span></span>
<span data-ttu-id="beb7a-287">Återställa data är en kombination av en ```RecoverableItem``` objekt och en ```RecoveryOption``` objekt.</span><span class="sxs-lookup"><span data-stu-id="beb7a-287">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="beb7a-288">I föregående avsnitt hello vi en lista över hello säkerhetskopiering punkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="beb7a-288">In hello previous section, we got a list of hello backup points for a datasource.</span></span>

<span data-ttu-id="beb7a-289">I hello exemplet nedan visar hur toorestore Hyper-V virtuell dator från Azure Backup genom att kombinera säkerhetskopiering punkter med hello mål för återställning.</span><span class="sxs-lookup"><span data-stu-id="beb7a-289">In hello example below, we demonstrate how toorestore a Hyper-V virtual machine from Azure Backup by combining backup points with hello target for recovery.</span></span> <span data-ttu-id="beb7a-290">Detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="beb7a-290">This includes:</span></span>

* <span data-ttu-id="beb7a-291">Skapa ett återställningsalternativ med hello [ny DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-291">Creating a recovery option using hello  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="beb7a-292">Hämtning hello punktmatrisen säkerhetskopiering med hello ```Get-DPMRecoveryPoint``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="beb7a-292">Fetching hello array of backup points using hello ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="beb7a-293">Om du väljer en säkerhetskopiering punkt toorestore från.</span><span class="sxs-lookup"><span data-stu-id="beb7a-293">Choosing a backup point toorestore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="beb7a-294">hello kommandona kan enkelt utökas för någon typ av datakälla.</span><span class="sxs-lookup"><span data-stu-id="beb7a-294">hello commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="beb7a-295">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="beb7a-295">Next steps</span></span>
* <span data-ttu-id="beb7a-296">Läs mer om Azure Backup för DPM [introduktion tooDPM säkerhetskopiering](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="beb7a-296">For more information about Azure Backup for DPM see [Introduction tooDPM Backup](backup-azure-dpm-introduction.md)</span></span>
