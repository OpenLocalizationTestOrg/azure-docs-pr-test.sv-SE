---
title: "Azure-säkerhetskopiering: Använd PowerShell för att säkerhetskopiera DPM-arbetsbelastningar | Microsoft Docs"
description: "Lär dig hur du distribuerar och hanterar Azure Backup för Data Protection Manager (DPM) med hjälp av PowerShell"
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
ms.openlocfilehash: 943a12dcba49a114d206b9dab968da332ea99926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="1eca2-103">Distribuera och hantera säkerhetskopiering till Azure för DPM-servrar (Data Protection Manager) med PowerShell</span><span class="sxs-lookup"><span data-stu-id="1eca2-103">Deploy and manage backup to Azure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1eca2-104">ARM</span><span class="sxs-lookup"><span data-stu-id="1eca2-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="1eca2-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="1eca2-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="1eca2-106">Den här artikeln beskriver hur du använder PowerShell för att säkerhetskopiera och återställa DPM-data från ett säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="1eca2-106">This article explains how to use PowerShell to back up and recover DPM data from a backup vault.</span></span> <span data-ttu-id="1eca2-107">Microsoft rekommenderar att du använder Recovery Services-valv för alla nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="1eca2-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="1eca2-108">Om du är en ny Azure Backup-användare kan använda i artikeln [distribuera och hantera Data Protection Manager-data till Azure med hjälp av PowerShell](backup-dpm-automation.md), så att du lagrar data i en Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-108">If you are a new Azure Backup user, use the article, [Deploy and manage Data Protection Manager data to Azure using PowerShell](backup-dpm-automation.md), so you store your data in a Recovery Services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1eca2-109">Nu kan du uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="1eca2-109">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="1eca2-110">Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv).</span><span class="sxs-lookup"><span data-stu-id="1eca2-110">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="1eca2-111">Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="1eca2-111">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="1eca2-112">Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="1eca2-112">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="1eca2-113">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="1eca2-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="1eca2-114">Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="1eca2-114">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="1eca2-115">Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-115">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="1eca2-116">Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="1eca2-116">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="setting-up-the-powershell-environment"></a><span data-ttu-id="1eca2-117">Ställa in PowerShell-miljö</span><span class="sxs-lookup"><span data-stu-id="1eca2-117">Setting up the PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="1eca2-118">Innan du kan använda PowerShell för att hantera säkerhetskopiering från Data Protection Manager till Azure, behöver du har rätt miljö i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1eca2-118">Before you can use PowerShell to manage backups from Data Protection Manager to Azure, you will need to have the right environment in PowerShell.</span></span> <span data-ttu-id="1eca2-119">Se till att du kör följande kommando för att importera modulerna som är rätt och att du kan korrekt refererar till DPM-cmdlets i början av PowerShell-session:</span><span class="sxs-lookup"><span data-stu-id="1eca2-119">At the start of the PowerShell session, ensure that you run the following command to import the right modules and allow you to correctly reference the DPM cmdlets:</span></span>

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a><span data-ttu-id="1eca2-120">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="1eca2-120">Setup and Registration</span></span>
<span data-ttu-id="1eca2-121">Börja:</span><span class="sxs-lookup"><span data-stu-id="1eca2-121">To begin:</span></span>

1. <span data-ttu-id="1eca2-122">[Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1eca2-122">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="1eca2-123">Aktivera Azure Backup-kommandon genom att växla till *AzureResourceManager* läge med hjälp av den **Switch-AzureMode** cmdleten igen:</span><span class="sxs-lookup"><span data-stu-id="1eca2-123">Enable the Azure Backup commandlets by switching to *AzureResourceManager* mode by using the **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="1eca2-124">Följande uppgifter för installation och registrering kan automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="1eca2-124">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="1eca2-125">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="1eca2-125">Create a backup vault</span></span>
* <span data-ttu-id="1eca2-126">Installera Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="1eca2-126">Installing the Azure Backup agent</span></span>
* <span data-ttu-id="1eca2-127">Registreras med Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="1eca2-127">Registering with the Azure Backup service</span></span>
* <span data-ttu-id="1eca2-128">Nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="1eca2-128">Networking settings</span></span>
* <span data-ttu-id="1eca2-129">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="1eca2-129">Encryption settings</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="1eca2-130">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="1eca2-130">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="1eca2-131">För kunder som använder Azure Backup för första gången, som du behöver registrera Azure Backup-provider som ska användas med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1eca2-131">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="1eca2-132">Detta kan göras genom att köra följande kommando: registrera AzureProvider - ProviderNamespace ”Microsoft.Backup”</span><span class="sxs-lookup"><span data-stu-id="1eca2-132">This can be done by running the following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="1eca2-133">Du kan skapa ett nytt säkerhetskopieringsvalv med den **ny AzureRMBackupVault** cmdleten igen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-133">You can create a new backup vault using the **New-AzureRMBackupVault** commandlet.</span></span> <span data-ttu-id="1eca2-134">Säkerhetskopieringsvalvet är en ARM-resurs, så du måste placera det inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1eca2-134">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="1eca2-135">Kör följande kommandon i en kommandotolk med förhöjd Azure PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="1eca2-135">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

<span data-ttu-id="1eca2-136">Du kan hämta en lista över alla säkerhetskopieringsvalv i en viss prenumeration med hjälp av den **Get-AzureRMBackupVault** cmdleten igen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-136">You can get a list of all the backup vaults in a given subscription using the **Get-AzureRMBackupVault** commandlet.</span></span>

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="1eca2-137">Installation av Azure Backup-agenten på en DPM-Server</span><span class="sxs-lookup"><span data-stu-id="1eca2-137">Installing the Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="1eca2-138">Innan du installerar Azure Backup-agenten som du behöver ha installationsprogrammet hämtade och finns på Windows Server.</span><span class="sxs-lookup"><span data-stu-id="1eca2-138">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="1eca2-139">Du kan hämta den senaste versionen av installationsprogrammet från den [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från det säkerhetskopieringsvalv instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="1eca2-139">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the backup vault's Dashboard page.</span></span> <span data-ttu-id="1eca2-140">Spara installationsprogrammet i en lättillgänglig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="1eca2-140">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="1eca2-141">Om du vill installera agenten, kör du följande kommando i en upphöjd PowerShell-konsol **på DPM-servern**:</span><span class="sxs-lookup"><span data-stu-id="1eca2-141">To install the agent, run the following command in an elevated PowerShell console **on the DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="1eca2-142">Detta installerar agent med alla standardalternativ.</span><span class="sxs-lookup"><span data-stu-id="1eca2-142">This installs the agent with all the default options.</span></span> <span data-ttu-id="1eca2-143">Installationen tar några minuter i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="1eca2-143">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="1eca2-144">Om du inte anger den */nu* alternativet den **Windows Update** öppnas i slutet av installationen att söka efter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="1eca2-144">If you do not specify the */nu* option the **Windows Update** window will open at the end of the installation to check for any updates.</span></span>

<span data-ttu-id="1eca2-145">Agenten visas i listan över installerade program.</span><span class="sxs-lookup"><span data-stu-id="1eca2-145">The agent will show in the list of installed programs.</span></span> <span data-ttu-id="1eca2-146">Listan över installerade program, gå till **Kontrollpanelen** > **program** > **program och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="1eca2-146">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Agenten är installerad](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a><span data-ttu-id="1eca2-148">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="1eca2-148">Installation options</span></span>
<span data-ttu-id="1eca2-149">Om du vill se alla alternativ som är tillgängliga via kommandoraden använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1eca2-149">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="1eca2-150">Tillgängliga alternativ inkluderar:</span><span class="sxs-lookup"><span data-stu-id="1eca2-150">The available options include:</span></span>

| <span data-ttu-id="1eca2-151">Alternativ</span><span class="sxs-lookup"><span data-stu-id="1eca2-151">Option</span></span> | <span data-ttu-id="1eca2-152">Information</span><span class="sxs-lookup"><span data-stu-id="1eca2-152">Details</span></span> | <span data-ttu-id="1eca2-153">Standard</span><span class="sxs-lookup"><span data-stu-id="1eca2-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1eca2-154">/q</span><span class="sxs-lookup"><span data-stu-id="1eca2-154">/q</span></span> |<span data-ttu-id="1eca2-155">Tyst installation</span><span class="sxs-lookup"><span data-stu-id="1eca2-155">Quiet installation</span></span> |- |
| <span data-ttu-id="1eca2-156">/ p: ”plats”</span><span class="sxs-lookup"><span data-stu-id="1eca2-156">/p:"location"</span></span> |<span data-ttu-id="1eca2-157">Sökvägen till installationsmappen för Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="1eca2-157">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="1eca2-158">C:\Program Files\Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="1eca2-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="1eca2-159">/ s: ”plats”</span><span class="sxs-lookup"><span data-stu-id="1eca2-159">/s:"location"</span></span> |<span data-ttu-id="1eca2-160">Sökväg till cachemappen för Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="1eca2-160">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="1eca2-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="1eca2-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="1eca2-162">/m</span><span class="sxs-lookup"><span data-stu-id="1eca2-162">/m</span></span> |<span data-ttu-id="1eca2-163">Delta i Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="1eca2-163">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="1eca2-164">/nu</span><span class="sxs-lookup"><span data-stu-id="1eca2-164">/nu</span></span> |<span data-ttu-id="1eca2-165">Sök inte efter uppdateringar när installationen är klar</span><span class="sxs-lookup"><span data-stu-id="1eca2-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="1eca2-166">/d</span><span class="sxs-lookup"><span data-stu-id="1eca2-166">/d</span></span> |<span data-ttu-id="1eca2-167">Avinstallerar Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="1eca2-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="1eca2-168">/pH</span><span class="sxs-lookup"><span data-stu-id="1eca2-168">/ph</span></span> |<span data-ttu-id="1eca2-169">Värden proxyadress</span><span class="sxs-lookup"><span data-stu-id="1eca2-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="1eca2-170">/po</span><span class="sxs-lookup"><span data-stu-id="1eca2-170">/po</span></span> |<span data-ttu-id="1eca2-171">Portnummer för proxyservern värden</span><span class="sxs-lookup"><span data-stu-id="1eca2-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="1eca2-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="1eca2-172">/pu</span></span> |<span data-ttu-id="1eca2-173">Värddatorn Proxyanvändarnamnet</span><span class="sxs-lookup"><span data-stu-id="1eca2-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="1eca2-174">/PW</span><span class="sxs-lookup"><span data-stu-id="1eca2-174">/pw</span></span> |<span data-ttu-id="1eca2-175">Lösenord för proxy</span><span class="sxs-lookup"><span data-stu-id="1eca2-175">Proxy Password</span></span> |- |

### <a name="registering-with-the-azure-backup-service"></a><span data-ttu-id="1eca2-176">Registreras med Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="1eca2-176">Registering with the Azure Backup service</span></span>
<span data-ttu-id="1eca2-177">Innan du kan registrera med Azure Backup-tjänsten måste du se till att den [krav](backup-azure-dpm-introduction.md) är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="1eca2-177">Before you can register with the Azure Backup service, you need to ensure that the [prerequisites](backup-azure-dpm-introduction.md) are met.</span></span> <span data-ttu-id="1eca2-178">Du måste:</span><span class="sxs-lookup"><span data-stu-id="1eca2-178">You must:</span></span>

* <span data-ttu-id="1eca2-179">Har ett giltigt Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1eca2-179">Have a valid Azure subscription</span></span>
* <span data-ttu-id="1eca2-180">Har ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="1eca2-180">Have a backup vault</span></span>

<span data-ttu-id="1eca2-181">Om du vill hämta valvautentiseringsuppgifter som kör den **Get-AzureBackupVaultCredentials** kommandot i en Azure PowerShell-konsolen och lagra den i en lämplig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="1eca2-181">To download the vault credentials, run the **Get-AzureBackupVaultCredentials** commandlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="1eca2-182">Registrera datorn med valvet görs med hjälp av den [Start DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="1eca2-182">Registering the machine with the vault is done using the [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

<span data-ttu-id="1eca2-183">DPM-servern med namnet ”TestingServer” med Microsoft Azure-valvet med hjälp av de angivna valvautentiseringsuppgifter registreras.</span><span class="sxs-lookup"><span data-stu-id="1eca2-183">This will register the DPM Server named “TestingServer” with Microsoft Azure Vault using the specified vault credentials.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1eca2-184">Använd inte relativa sökvägar för att ange valvautentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-184">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="1eca2-185">Du måste ange en absolut sökväg som indata till cmdleten.</span><span class="sxs-lookup"><span data-stu-id="1eca2-185">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

### <a name="initial-configuration-settings"></a><span data-ttu-id="1eca2-186">Inledande konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="1eca2-186">Initial configuration settings</span></span>
<span data-ttu-id="1eca2-187">När DPM-servern registreras med Azure Backup-valvet, startar den med standardinställningar för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-187">Once the DPM Server is registered with the Azure Backup vault, it will start with default subscription settings.</span></span> <span data-ttu-id="1eca2-188">Inställningarna prenumeration omfattar nätverk, kryptering och mellanlagringsområdet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-188">These subscription settings include Networking, Encryption and the Staging area.</span></span> <span data-ttu-id="1eca2-189">Om du vill ändra prenumerationsinställningar måste du först få grepp om de befintliga inställningarna för (standard) med hjälp av den [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="1eca2-189">To begin changing the subscription settings you need to first get a handle on the existing (default) settings using the [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="1eca2-190">Alla ändringar som görs till den här lokala PowerShell-objektet ```$setting``` och sedan fullständig objektet strävar efter att DPM och Azure Backup för att spara dem med hjälp av den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-190">All modifications are made to this local PowerShell object ```$setting```  and then the full object is committed to DPM and Azure Backup to save them using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="1eca2-191">Du måste använda den ```–Commit``` flagga för att säkerställa att ändringarna sparas.</span><span class="sxs-lookup"><span data-stu-id="1eca2-191">You need to use the ```–Commit``` flag to ensure that the changes are persisted.</span></span> <span data-ttu-id="1eca2-192">Inställningarna kommer inte tillämpas och används av Azure Backup såvida inte genomförts.</span><span class="sxs-lookup"><span data-stu-id="1eca2-192">The settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a><span data-ttu-id="1eca2-193">Nätverk</span><span class="sxs-lookup"><span data-stu-id="1eca2-193">Networking</span></span>
<span data-ttu-id="1eca2-194">Om anslutningen till DPM-datorn till Azure Backup-tjänsten på internet via en proxyserver, måste inställningarna för proxyservern anges för säkerhetskopiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="1eca2-194">If the connectivity of the DPM machine to the Azure Backup service on the internet is through a proxy server, then the proxy server settings should be provided for backups to succeed.</span></span> <span data-ttu-id="1eca2-195">Detta görs med hjälp av den ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` och ```ProxyPassword``` parametrar med den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-195">This is done by using the ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` and the ```ProxyPassword``` parameters with the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="1eca2-196">I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.</span><span class="sxs-lookup"><span data-stu-id="1eca2-196">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="1eca2-197">Bandbreddsanvändning kan också kontrolleras med alternativ för ```-WorkHourBandwidth``` och ```-NonWorkHourBandwidth``` för en given uppsättning dagar i veckan.</span><span class="sxs-lookup"><span data-stu-id="1eca2-197">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of the week.</span></span> <span data-ttu-id="1eca2-198">I det här exemplet anger vi inte någon begränsning.</span><span class="sxs-lookup"><span data-stu-id="1eca2-198">In this example we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a><span data-ttu-id="1eca2-199">Konfigurera mellanlagringsområdet</span><span class="sxs-lookup"><span data-stu-id="1eca2-199">Configuring the staging Area</span></span>
<span data-ttu-id="1eca2-200">Azure Backup-agenten som körs på DPM-servern behöver tillfällig lagring för data som återställs från molnet (lokalt mellanlagringsområde).</span><span class="sxs-lookup"><span data-stu-id="1eca2-200">The Azure Backup agent running on the DPM server needs temporary storage for data restored from the cloud (local staging area).</span></span> <span data-ttu-id="1eca2-201">Konfigurera mellanlagringsområdet med hjälp av den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet och ```-StagingAreaPath``` parameter.</span><span class="sxs-lookup"><span data-stu-id="1eca2-201">Configure the staging area using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and the ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="1eca2-202">I exemplet ovan mellanlagringsområdet anges till *C:\StagingArea* i PowerShell-objektet ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="1eca2-202">In the example above, the staging area will be set to *C:\StagingArea* in the PowerShell object ```$setting```.</span></span> <span data-ttu-id="1eca2-203">Se till att den angivna mappen finns redan, annars misslyckas det slutliga genomförandet av prenumerationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="1eca2-203">Ensure that the specified folder already exists, or else the final commit of the subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="1eca2-204">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="1eca2-204">Encryption settings</span></span>
<span data-ttu-id="1eca2-205">Den säkerhetskopiera informationen som skickas till Azure Backup krypteras för att skydda sekretessen för data.</span><span class="sxs-lookup"><span data-stu-id="1eca2-205">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="1eca2-206">Krypteringslösenfrasen är ”password” att dekryptera data vid tidpunkten för återställning.</span><span class="sxs-lookup"><span data-stu-id="1eca2-206">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span> <span data-ttu-id="1eca2-207">Det är viktigt att behålla den här informationen säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="1eca2-207">It is important to keep this information safe and secure once it is set.</span></span>

<span data-ttu-id="1eca2-208">I exemplet nedan det första kommandot konverterar strängen ```passphrase123456789``` till en säker sträng och tilldelar säker sträng till variabeln med namnet ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="1eca2-208">In the example below, the first command converts the string ```passphrase123456789``` to a secure string and assigns the secure string to the variable named ```$Passphrase```.</span></span> <span data-ttu-id="1eca2-209">Det andra kommandot anger säker sträng i ```$Passphrase``` som lösenord för kryptering av säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="1eca2-209">the second command sets the secure string in ```$Passphrase``` as the password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="1eca2-210">Behåll informationen lösenfras säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="1eca2-210">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="1eca2-211">Du kommer inte att återställa data från Azure utan lösenfras.</span><span class="sxs-lookup"><span data-stu-id="1eca2-211">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="1eca2-212">Nu ska du har gjort alla nödvändiga ändringar av den ```$setting``` objekt.</span><span class="sxs-lookup"><span data-stu-id="1eca2-212">At this point, you should have made all the required changes to the ```$setting``` object.</span></span> <span data-ttu-id="1eca2-213">Kom ihåg att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="1eca2-213">Remember to commit the changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a><span data-ttu-id="1eca2-214">Skydda data till Azure Backup</span><span class="sxs-lookup"><span data-stu-id="1eca2-214">Protect data to Azure Backup</span></span>
<span data-ttu-id="1eca2-215">I det här avsnittet kan du lägga till en produktionsservern till DPM och skydda sedan data till lokala DPM-lagring och sedan till Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="1eca2-215">In this section, you will add a production server to DPM and then protect the data to local DPM storage and then to Azure Backup.</span></span> <span data-ttu-id="1eca2-216">I exemplen visar vi hur du säkerhetskopierar filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="1eca2-216">In the examples we will demonstrate how to back up files and folders.</span></span> <span data-ttu-id="1eca2-217">Logiken kan enkelt utökas för att säkerhetskopiera alla datakällor som stöds av DPM.</span><span class="sxs-lookup"><span data-stu-id="1eca2-217">The logic can easily be extended to backup any DPM-supported data source.</span></span> <span data-ttu-id="1eca2-218">Alla DPM-säkerhetskopieringar styrs av en skydd grupp (PG) med fyra delar:</span><span class="sxs-lookup"><span data-stu-id="1eca2-218">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="1eca2-219">**Medlemmar** är en lista över alla skyddsobjekt (även kallat *datakällor* i DPM) som du vill skydda i samma skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="1eca2-219">**Group members** is a list of all the protectable objects (also known as *Datasources* in DPM) that you want to protect in the same protection group.</span></span> <span data-ttu-id="1eca2-220">Du kanske vill skydda produktion virtuella datorer i en skyddsgrupp och SQL Server-databaser i en annan skyddsgrupp som de kan ha olika krav för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1eca2-220">For example, you may want to protect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="1eca2-221">Innan du kan säkerhetskopiera någon datakälla på en produktionsserver måste du kontrollera att DPM-agenten är installerad på servern och hanteras av DPM.</span><span class="sxs-lookup"><span data-stu-id="1eca2-221">Before you can back up any datasource on a production server you need to make sure the DPM Agent is installed on the server and is managed by DPM.</span></span> <span data-ttu-id="1eca2-222">Följ anvisningarna för [installera DPM-agenten](https://technet.microsoft.com/library/bb870935.aspx) och länkas till rätt DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="1eca2-222">Follow the steps for [installing the DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it to the appropriate DPM Server.</span></span>
2. <span data-ttu-id="1eca2-223">**Dataskyddsmetod** anger de målplatserna - band, disk och molnet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-223">**Data protection method** specifies the target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="1eca2-224">I vårt exempel skyddar vi data till den lokala disken och till molnet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-224">In our example we will protect data to the local disk and to the cloud.</span></span>
3. <span data-ttu-id="1eca2-225">En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver tas och hur ofta data ska synkroniseras mellan DPM-servern och produktionsservern.</span><span class="sxs-lookup"><span data-stu-id="1eca2-225">A **backup schedule** that specifies when backups need to be taken and how often the data should be synchronized between the DPM Server and the production server.</span></span>
4. <span data-ttu-id="1eca2-226">En **bevarandeschema** som anger hur lång tid att behålla återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="1eca2-226">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="1eca2-227">Skapa en skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="1eca2-227">Creating a protection group</span></span>
<span data-ttu-id="1eca2-228">Börja med att skapa en ny Skyddsgrupp med hjälp av [ny DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-228">Start by creating a new Protection Group using the [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="1eca2-229">Cmdleten ovan skapar en Skyddsgrupp med namnet *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="1eca2-229">The above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="1eca2-230">En befintlig skyddsgrupp kan också ändras senare om du vill lägga till säkerhetskopiering till Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-230">An existing protection group can also be modified later to add backup to the Azure cloud.</span></span> <span data-ttu-id="1eca2-231">Dock göra några ändringar i skyddsgruppen - ny eller befintlig - vi behöver skaffa en referens till en *ändringsbar* objekt med hjälp av [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-231">However, to make any changes to the Protection Group - new or existing - we need to get a handle on a *modifiable* object using the [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a><span data-ttu-id="1eca2-232">Lägga till medlemmar i skyddsgruppen</span><span class="sxs-lookup"><span data-stu-id="1eca2-232">Adding group members to the Protection Group</span></span>
<span data-ttu-id="1eca2-233">Varje DPM-agenten vet listan med datakällor på den server som den är installerad på.</span><span class="sxs-lookup"><span data-stu-id="1eca2-233">Each DPM Agent knows the list of datasources on the server that it is installed on.</span></span> <span data-ttu-id="1eca2-234">DPM-agenten måste först skicka en lista med datakällorna på DPM-servern för att lägga till en datakälla i skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-234">To add a datasource to the Protection Group, the DPM Agent needs to first send a list of the datasources back to the DPM server.</span></span> <span data-ttu-id="1eca2-235">En eller flera datakällor är sedan valt och lägga till Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-235">One or more datasources are then selected and added to the Protection Group.</span></span> <span data-ttu-id="1eca2-236">PowerShell-åtgärder som behövs för att få uppnå detta är:</span><span class="sxs-lookup"><span data-stu-id="1eca2-236">The PowerShell steps needed to get achieve this are:</span></span>

1. <span data-ttu-id="1eca2-237">Hämta en lista över alla servrar som hanterade med DPM via DPM-agenten.</span><span class="sxs-lookup"><span data-stu-id="1eca2-237">Fetch a list of all servers managed by DPM through the DPM Agent.</span></span>
2. <span data-ttu-id="1eca2-238">Välj en specifik server.</span><span class="sxs-lookup"><span data-stu-id="1eca2-238">Choose a specific server.</span></span>
3. <span data-ttu-id="1eca2-239">Hämta en lista över alla datakällor på servern.</span><span class="sxs-lookup"><span data-stu-id="1eca2-239">Fetch a list of all datasources on the server.</span></span>
4. <span data-ttu-id="1eca2-240">Välj en eller flera datakällor och lägga till dem i Skyddsgruppen</span><span class="sxs-lookup"><span data-stu-id="1eca2-240">Choose one or more datasources and add them to the Protection Group</span></span>

<span data-ttu-id="1eca2-241">I listan med servrar där DPM-agenten är installerad och hanteras av DPM-servern förvärvas med den [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-241">The list of servers on which the DPM Agent is installed and is being managed by the DPM Server is acquired with the [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="1eca2-242">I det här exemplet kommer att filtrera och bara konfigurera PS med namnet *productionserver01* för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1eca2-242">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="1eca2-243">Nu att hämta listan över datakällor på ```$server``` med hjälp av den [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-243">Now fetch the list of datasources on ```$server``` using the [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="1eca2-244">I det här exemplet vi filtrering för volymen * D:\* som vi vill konfigurera för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1eca2-244">In this example we are filtering for the volume *D:\* which we want to configure for backup.</span></span> <span data-ttu-id="1eca2-245">Den här datakällan läggs sedan till Skyddsgruppen med hjälp av den [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-245">This datasource is then added to the Protection Group using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="1eca2-246">Kom ihåg att använda den *modifable* skydd gruppobjekt ```$MPG``` att tilläggen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-246">Remember to use the *modifable* protection group object ```$MPG``` to make the additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="1eca2-247">Upprepa detta steg så många gånger som krävs, tills du har lagt till de valda datakällorna i skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-247">Repeat this step as many times as required, until you have added all the chosen datasources to the protection group.</span></span> <span data-ttu-id="1eca2-248">Du kan också starta med en datakälla och slutföra arbetsflödet för att skapa Skyddsgruppen och senare lägga till flera datakällor i Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-248">You can also start with just one datasource, and complete the workflow for creating the Protection Group, and at a later point add more datasources to the Protection Group.</span></span>

### <a name="selecting-the-data-protection-method"></a><span data-ttu-id="1eca2-249">Välja dataskyddsmetod</span><span class="sxs-lookup"><span data-stu-id="1eca2-249">Selecting the data protection method</span></span>
<span data-ttu-id="1eca2-250">När datakällorna har lagts till Skyddsgruppen, nästa steg är att ange metoden skydd med den [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-250">Once the datasources have been added to the Protection Group, the next step is to specify the protection method using the [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="1eca2-251">I det här exemplet Skyddsgruppen inte inställningar för lokal disk och säkerhetskopiering i molnet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-251">In this example, the Protection Group will be setup for local disk and cloud backup.</span></span> <span data-ttu-id="1eca2-252">Du måste också ange datasource som du vill skydda till molnet med den [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet med - Online flagga.</span><span class="sxs-lookup"><span data-stu-id="1eca2-252">You also need to specify the datasource that you want to protect to cloud using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a><span data-ttu-id="1eca2-253">Ange kvarhållningsintervallet</span><span class="sxs-lookup"><span data-stu-id="1eca2-253">Setting the retention range</span></span>
<span data-ttu-id="1eca2-254">Loggperiod för säkerhetskopieringen återställningspunkter med de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-254">Set the retention for the backup points using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="1eca2-255">När det kan verka udda loggperiod innan schemat för säkerhetskopiering har definierats med den ```Set-DPMPolicyObjective``` cmdlet anger automatiskt ett standardschema för säkerhetskopiering som sedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="1eca2-255">While it might seem odd to set the retention before the backup schedule has been defined, using the ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="1eca2-256">Det är alltid möjligt att ange säkerhetskopieringen schemalägga först och bevarandeprincip efter.</span><span class="sxs-lookup"><span data-stu-id="1eca2-256">It is always possible to set the backup schedule first and the retention policy after.</span></span>

<span data-ttu-id="1eca2-257">I exemplet nedan anger cmdleten kvarhållning parametrarna för säkerhetskopiering till disk.</span><span class="sxs-lookup"><span data-stu-id="1eca2-257">In the example below, the cmdlet sets the retention parameters for disk backups.</span></span> <span data-ttu-id="1eca2-258">Detta behåller säkerhetskopior för 10 dagar och synkronisera data var 6 timme mellan produktionsservern och DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="1eca2-258">This will retain backups for 10 days, and sync data every 6 hours between the production server and the DPM server.</span></span> <span data-ttu-id="1eca2-259">Den ```SynchronizationFrequencyMinutes``` inte definierar hur ofta en säkerhetskopiering skapas, men hur ofta data kopieras till DPM-servern; Detta förhindrar att säkerhetskopiorna blir för stor.</span><span class="sxs-lookup"><span data-stu-id="1eca2-259">The ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied to the DPM server; this prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="1eca2-260">För säkerhetskopiering ska Azure (DPM refererar till dessa som onlinesäkerhetskopieringar) i kvarhållningsperioder kan konfigureras för [långsiktigt kvarhållning med en farfar-pappa-Son (offentlig sektor GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="1eca2-260">For backups going to Azure (DPM refers to these as Online backups) the retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="1eca2-261">Det vill säga kan du definiera en kombinerad bevarandeprincip som rör varje dag, vecka, månad och år bevarandeprinciper.</span><span class="sxs-lookup"><span data-stu-id="1eca2-261">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="1eca2-262">I det här exemplet vi skapa en matris som representerar det komplexa kvarhållning schema som vi vill och sedan konfigurera kvarhållning intervall med den [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-262">In this example, we create an array representing the complex retention scheme that we want, and then configure the retention range using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a><span data-ttu-id="1eca2-263">Ange schemat för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1eca2-263">Set the backup schedule</span></span>
<span data-ttu-id="1eca2-264">DPM anger en säkerhetskopiering standardschemat automatiskt om du anger en skydd mål med hjälp av den ```Set-DPMPolicyObjective``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-264">DPM sets a default backup schedule automatically if you specify the protection objective using the ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="1eca2-265">Du kan ändra standardscheman den [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet följt av den [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-265">To change the default schedules, use the [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by the [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="1eca2-266">I exemplet ovan, ```$onlineSch``` är en matris med fyra element som innehåller befintliga schema för onlineskydd för Skyddsgruppen i GFS schemat:</span><span class="sxs-lookup"><span data-stu-id="1eca2-266">In the example above, ```$onlineSch``` is an array with four elements that contains the existing online protection schedule for the Protection Group in the GFS scheme:</span></span>

1. <span data-ttu-id="1eca2-267">```$onlineSch[0]```innehåller dagsschema</span><span class="sxs-lookup"><span data-stu-id="1eca2-267">```$onlineSch[0]``` will contain the daily schedule</span></span>
2. <span data-ttu-id="1eca2-268">```$onlineSch[1]```innehåller veckoschemat</span><span class="sxs-lookup"><span data-stu-id="1eca2-268">```$onlineSch[1]``` will contain the weekly schedule</span></span>
3. <span data-ttu-id="1eca2-269">```$onlineSch[2]```innehåller månadsschema</span><span class="sxs-lookup"><span data-stu-id="1eca2-269">```$onlineSch[2]``` will contain the monthly schedule</span></span>
4. <span data-ttu-id="1eca2-270">```$onlineSch[3]```innehåller årlig schemat</span><span class="sxs-lookup"><span data-stu-id="1eca2-270">```$onlineSch[3]``` will contain the yearly schedule</span></span>

<span data-ttu-id="1eca2-271">Om du behöver ändra veckoschemat måste referera till den ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="1eca2-271">So if you need to modify the weekly schedule, you need to refer to the ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="1eca2-272">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="1eca2-272">Initial backup</span></span>
<span data-ttu-id="1eca2-273">När du säkerhetskopierar en datakälla för första gången måste DPM skapa en första replik som kommer att skapa en kopia i datasource som ska skyddas på DPM-replikvolymen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-273">When backing up a datasource for the first time, DPM needs to create an initial replica which will create a copy of the datasource to be protected on DPM replica volume.</span></span> <span data-ttu-id="1eca2-274">Den här aktiviteten kan antingen schemaläggas för en viss tid, eller kan aktiveras manuellt med hjälp av den [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet med parametern ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="1eca2-274">This activity can either be scheduled for a specific time, or can be triggered manually, using the [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with the parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="1eca2-275">Ändra storlek på DPM-replikeringen & återställningspunktvolymen</span><span class="sxs-lookup"><span data-stu-id="1eca2-275">Changing the size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="1eca2-276">Du kan också ändra storleken på DPM-replikvolymen samt skuggkopia av volymen med hjälp av [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet som i det exemplet nedan: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS - protectiongroup $MPG-manuell - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="1eca2-276">You can also change the size of DPM Replica volume as well as Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in the below example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-the-changes-to-the-protection-group"></a><span data-ttu-id="1eca2-277">Bekräfta ändringarna i skyddsgruppen</span><span class="sxs-lookup"><span data-stu-id="1eca2-277">Committing the changes to the Protection Group</span></span>
<span data-ttu-id="1eca2-278">Slutligen måste ändringarna genomföras innan DPM kan säkerhetskopiera per den nya Skyddsgruppen-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1eca2-278">Finally, the changes need to be committed before DPM can take the backup per the new Protection Group configuration.</span></span> <span data-ttu-id="1eca2-279">Detta görs med hjälp av den [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-279">This is done using the [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a><span data-ttu-id="1eca2-280">Visa säkerhetskopiering punkter</span><span class="sxs-lookup"><span data-stu-id="1eca2-280">View the backup points</span></span>
<span data-ttu-id="1eca2-281">Du kan använda den [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) att hämta en lista över alla återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="1eca2-281">You can use the [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet to get a list of all recovery points for a datasource.</span></span> <span data-ttu-id="1eca2-282">I det här exemplet ska du:</span><span class="sxs-lookup"><span data-stu-id="1eca2-282">In this example, we will:</span></span>

* <span data-ttu-id="1eca2-283">Hämta alla PGs på DPM-servern som ska lagras i en matris```$PG```</span><span class="sxs-lookup"><span data-stu-id="1eca2-283">fetch all the PGs on the DPM server which will be stored in an array ```$PG```</span></span>
* <span data-ttu-id="1eca2-284">Hämta datakällorna som motsvarar den```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="1eca2-284">get the datasources corresponding to the ```$PG[0]```</span></span>
* <span data-ttu-id="1eca2-285">Hämta alla återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="1eca2-285">get all the recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="1eca2-286">Återställa skyddade data på Azure</span><span class="sxs-lookup"><span data-stu-id="1eca2-286">Restore data protected on Azure</span></span>
<span data-ttu-id="1eca2-287">Återställa data är en kombination av en ```RecoverableItem``` objekt och en ```RecoveryOption``` objekt.</span><span class="sxs-lookup"><span data-stu-id="1eca2-287">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="1eca2-288">I det föregående avsnittet vi en lista över säkerhetskopiering punkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="1eca2-288">In the previous section, we got a list of the backup points for a datasource.</span></span>

<span data-ttu-id="1eca2-289">I exemplet nedan visar vi hur du återställer en virtuell dator för Hyper-V från Azure Backup genom att kombinera säkerhetskopiering punkter med mål för återställning.</span><span class="sxs-lookup"><span data-stu-id="1eca2-289">In the example below, we demonstrate how to restore a Hyper-V virtual machine from Azure Backup by combining backup points with the target for recovery.</span></span> <span data-ttu-id="1eca2-290">Detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="1eca2-290">This includes:</span></span>

* <span data-ttu-id="1eca2-291">Skapa en återställningspunkt alternativet med hjälp av den [ny DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-291">Creating a recovery option using the  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="1eca2-292">Hämtar punktmatrisen säkerhetskopiering med hjälp av den ```Get-DPMRecoveryPoint``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1eca2-292">Fetching the array of backup points using the ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="1eca2-293">Om du väljer en säkerhetskopieringspunkt att återställa från.</span><span class="sxs-lookup"><span data-stu-id="1eca2-293">Choosing a backup point to restore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="1eca2-294">Kommandona kan enkelt utökas för någon typ av datakälla.</span><span class="sxs-lookup"><span data-stu-id="1eca2-294">The commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1eca2-295">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1eca2-295">Next steps</span></span>
* <span data-ttu-id="1eca2-296">Läs mer om Azure Backup för DPM [introduktion till DPM-säkerhetskopiering](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="1eca2-296">For more information about Azure Backup for DPM see [Introduction to DPM Backup](backup-azure-dpm-introduction.md)</span></span>
