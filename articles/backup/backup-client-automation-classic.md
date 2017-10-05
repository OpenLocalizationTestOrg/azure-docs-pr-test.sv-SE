---
title: "Använda PowerShell för att hantera Windows Server-säkerhetskopiering i Azure | Microsoft Docs"
description: "Distribuera och hantera Windows Server-säkerhetskopior med hjälp av PowerShell."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: e7e269e2-1f11-41a9-957b-a2155de6a1e0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: saurse;markgal;nkolli;trinadhk
ms.openlocfilehash: a8e20356ae383ee4fa2158ea544d5d0905028124
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="269b5-103">Distribuera och hantera säkerhetskopiering till Azure för Windows Server/Windows-klient med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="269b5-103">Deploy and manage backup to Azure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="269b5-104">ARM</span><span class="sxs-lookup"><span data-stu-id="269b5-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="269b5-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="269b5-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="269b5-106">Den här artikeln beskriver hur du använder PowerShell för att säkerhetskopiera Windows Server eller Windows workstation-data till ett säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="269b5-106">This article explains how to use PowerShell to back up Windows Server or Windows workstation data to a backup vault.</span></span> <span data-ttu-id="269b5-107">Microsoft rekommenderar att du använder Recovery Services-valv för alla nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="269b5-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="269b5-108">Om du har använt Azure Backup och inte har skapat ett säkerhetskopieringsvalv i din prenumeration, använder du i artikeln [distribuera och hantera Data Protection Manager-data till Azure med hjälp av PowerShell](backup-client-automation.md) så att du lagrar data i en Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="269b5-108">If you are a new Azure Backup user and have not created a backup vault in your subscription, use the article, [Deploy and manage Data Protection Manager data to Azure using PowerShell](backup-client-automation.md) so you store your data in a Recovery Services vault.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="269b5-109">Nu kan du uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="269b5-109">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="269b5-110">Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv).</span><span class="sxs-lookup"><span data-stu-id="269b5-110">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="269b5-111">Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="269b5-111">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="269b5-112">Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="269b5-112">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="269b5-113">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="269b5-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="269b5-114">Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="269b5-114">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="269b5-115">Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="269b5-115">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="269b5-116">Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="269b5-116">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="install-azure-powershell"></a><span data-ttu-id="269b5-117">Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="269b5-117">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="269b5-118">Azure PowerShell 1.0 gavs ut i oktober 2015.</span><span class="sxs-lookup"><span data-stu-id="269b5-118">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="269b5-119">Den här versionen lyckades 0.9.8-versionen släpps och medfört några viktiga ändringar, särskilt i namnmönstret cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-119">This release succeeded the 0.9.8 release and brought about some significant changes, especially in the naming pattern of the cmdlets.</span></span> <span data-ttu-id="269b5-120">1.0-cmdletar följer namngivningsmönstret {verb}-AzureRm {substantiv}, medan 0.9.8-namn inte inkluderar **Rm** (till exempel New-AzureRmResourceGroup istället för New-AzureResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="269b5-120">1.0 cmdlets follow the naming pattern {verb}-AzureRm{noun}; whereas, the 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="269b5-121">När du använder Azure PowerShell 0.9.8, måste du först aktivera Resource Manager-läget genom att köra kommandot **Switch-AzureMode AzureResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="269b5-121">When using Azure PowerShell 0.9.8, you must first enable the Resource Manager mode by running the **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="269b5-122">Det här kommandot behövs inte i 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="269b5-122">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="269b5-123">Om du vill använda skript skrivna för 0.9.8-versionen-miljö i miljön 1.0 eller senare, bör du noggrant testa skripten i en förproduktionsmiljö innan du använder dem i produktion för att undvika oväntade påverkan.</span><span class="sxs-lookup"><span data-stu-id="269b5-123">If you want to use your scripts written for the 0.9.8 environment, in the 1.0 or later environment, you should carefully test the scripts in a pre-production environment before using them in production to avoid unexpected impact.</span></span>

<span data-ttu-id="269b5-124">[Hämta den senaste versionen av PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="269b5-124">[Download the latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a><span data-ttu-id="269b5-125">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="269b5-125">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="269b5-126">För kunder som använder Azure Backup för första gången, som du behöver registrera Azure Backup-provider som ska användas med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="269b5-126">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="269b5-127">Detta kan göras genom att köra följande kommando: registrera AzureProvider - ProviderNamespace ”Microsoft.Backup”</span><span class="sxs-lookup"><span data-stu-id="269b5-127">This can be done by running the following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="269b5-128">Du kan skapa ett nytt säkerhetskopieringsvalv med den **ny AzureRMBackupVault** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-128">You can create a new backup vault using the **New-AzureRMBackupVault** cmdlet.</span></span> <span data-ttu-id="269b5-129">Säkerhetskopieringsvalvet är en ARM-resurs, så du måste placera det inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="269b5-129">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="269b5-130">Kör följande kommandon i en kommandotolk med förhöjd Azure PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="269b5-130">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="269b5-131">Använd den **Get-AzureRMBackupVault** för att visa en lista med säkerhetskopieringsvalv i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="269b5-131">Use the **Get-AzureRMBackupVault** cmdlet to list the backup vaults in a subscription.</span></span>

## <a name="installing-the-azure-backup-agent"></a><span data-ttu-id="269b5-132">Installera Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="269b5-132">Installing the Azure Backup agent</span></span>
<span data-ttu-id="269b5-133">Innan du installerar Azure Backup-agenten som du behöver ha installationsprogrammet hämtade och finns på Windows Server.</span><span class="sxs-lookup"><span data-stu-id="269b5-133">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="269b5-134">Du kan hämta den senaste versionen av installationsprogrammet från den [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från det säkerhetskopieringsvalv instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="269b5-134">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the backup vault's Dashboard page.</span></span> <span data-ttu-id="269b5-135">Spara installationsprogrammet i en lättillgänglig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="269b5-135">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="269b5-136">Om du vill installera agenten, kör du följande kommando i en upphöjd PowerShell-konsol:</span><span class="sxs-lookup"><span data-stu-id="269b5-136">To install the agent, run the following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="269b5-137">Detta installerar agent med alla standardalternativ.</span><span class="sxs-lookup"><span data-stu-id="269b5-137">This installs the agent with all the default options.</span></span> <span data-ttu-id="269b5-138">Installationen tar några minuter i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="269b5-138">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="269b5-139">Om du inte anger den */nu* alternativet sedan **Windows Update** öppnas i slutet av installationen att söka efter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="269b5-139">If you do not specify the */nu* option then the **Windows Update** window will open at the end of the installation to check for any updates.</span></span> <span data-ttu-id="269b5-140">När du har installerat visas agenten i listan över installerade program.</span><span class="sxs-lookup"><span data-stu-id="269b5-140">Once installed, the agent will show in the list of installed programs.</span></span>

<span data-ttu-id="269b5-141">Listan över installerade program, gå till **Kontrollpanelen** > **program** > **program och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="269b5-141">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Agenten är installerad](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="269b5-143">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="269b5-143">Installation options</span></span>
<span data-ttu-id="269b5-144">Om du vill se alla alternativ som är tillgängliga via kommandoraden använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="269b5-144">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="269b5-145">Tillgängliga alternativ inkluderar:</span><span class="sxs-lookup"><span data-stu-id="269b5-145">The available options include:</span></span>

| <span data-ttu-id="269b5-146">Alternativ</span><span class="sxs-lookup"><span data-stu-id="269b5-146">Option</span></span> | <span data-ttu-id="269b5-147">Information</span><span class="sxs-lookup"><span data-stu-id="269b5-147">Details</span></span> | <span data-ttu-id="269b5-148">Standard</span><span class="sxs-lookup"><span data-stu-id="269b5-148">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="269b5-149">/q</span><span class="sxs-lookup"><span data-stu-id="269b5-149">/q</span></span> |<span data-ttu-id="269b5-150">Tyst installation</span><span class="sxs-lookup"><span data-stu-id="269b5-150">Quiet installation</span></span> |- |
| <span data-ttu-id="269b5-151">/ p: ”plats”</span><span class="sxs-lookup"><span data-stu-id="269b5-151">/p:"location"</span></span> |<span data-ttu-id="269b5-152">Sökvägen till installationsmappen för Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="269b5-152">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="269b5-153">C:\Program Files\Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="269b5-153">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="269b5-154">/ s: ”plats”</span><span class="sxs-lookup"><span data-stu-id="269b5-154">/s:"location"</span></span> |<span data-ttu-id="269b5-155">Sökväg till cachemappen för Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="269b5-155">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="269b5-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="269b5-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="269b5-157">/m</span><span class="sxs-lookup"><span data-stu-id="269b5-157">/m</span></span> |<span data-ttu-id="269b5-158">Delta i Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="269b5-158">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="269b5-159">/nu</span><span class="sxs-lookup"><span data-stu-id="269b5-159">/nu</span></span> |<span data-ttu-id="269b5-160">Sök inte efter uppdateringar när installationen är klar</span><span class="sxs-lookup"><span data-stu-id="269b5-160">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="269b5-161">/d</span><span class="sxs-lookup"><span data-stu-id="269b5-161">/d</span></span> |<span data-ttu-id="269b5-162">Avinstallerar Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="269b5-162">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="269b5-163">/pH</span><span class="sxs-lookup"><span data-stu-id="269b5-163">/ph</span></span> |<span data-ttu-id="269b5-164">Värden proxyadress</span><span class="sxs-lookup"><span data-stu-id="269b5-164">Proxy Host Address</span></span> |- |
| <span data-ttu-id="269b5-165">/po</span><span class="sxs-lookup"><span data-stu-id="269b5-165">/po</span></span> |<span data-ttu-id="269b5-166">Portnummer för proxyservern värden</span><span class="sxs-lookup"><span data-stu-id="269b5-166">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="269b5-167">/Pu</span><span class="sxs-lookup"><span data-stu-id="269b5-167">/pu</span></span> |<span data-ttu-id="269b5-168">Värddatorn Proxyanvändarnamnet</span><span class="sxs-lookup"><span data-stu-id="269b5-168">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="269b5-169">/PW</span><span class="sxs-lookup"><span data-stu-id="269b5-169">/pw</span></span> |<span data-ttu-id="269b5-170">Lösenord för proxy</span><span class="sxs-lookup"><span data-stu-id="269b5-170">Proxy Password</span></span> |- |

## <a name="registering-with-the-azure-backup-service"></a><span data-ttu-id="269b5-171">Registreras med Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="269b5-171">Registering with the Azure Backup service</span></span>
<span data-ttu-id="269b5-172">Innan du kan registrera med Azure Backup-tjänsten måste du se till att den [krav](backup-configure-vault.md) är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="269b5-172">Before you can register with the Azure Backup service, you need to ensure that the [prerequisites](backup-configure-vault.md) are met.</span></span> <span data-ttu-id="269b5-173">Du måste:</span><span class="sxs-lookup"><span data-stu-id="269b5-173">You must:</span></span>

* <span data-ttu-id="269b5-174">Har ett giltigt Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="269b5-174">Have a valid Azure subscription</span></span>
* <span data-ttu-id="269b5-175">Har ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="269b5-175">Have a backup vault</span></span>

<span data-ttu-id="269b5-176">Om du vill hämta valvautentiseringsuppgifter som kör den **Get-AzureRMBackupVaultCredentials** cmdlet i en Azure PowerShell-konsolen och lagra den i en lämplig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="269b5-176">To download the vault credentials, run the **Get-AzureRMBackupVaultCredentials** cmdlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="269b5-177">Registrera datorn med valvet görs med hjälp av den [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="269b5-177">Registering the machine with the vault is done using the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="269b5-178">Använd inte relativa sökvägar för att ange valvautentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="269b5-178">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="269b5-179">Du måste ange en absolut sökväg som indata till cmdleten.</span><span class="sxs-lookup"><span data-stu-id="269b5-179">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="269b5-180">Nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="269b5-180">Networking settings</span></span>
<span data-ttu-id="269b5-181">När anslutningen för Windows-dator till internet via en proxyserver anges proxyinställningarna också till agenten.</span><span class="sxs-lookup"><span data-stu-id="269b5-181">When the connectivity of the Windows machine to the internet is through a proxy server, the proxy settings can also be provided to the agent.</span></span> <span data-ttu-id="269b5-182">I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.</span><span class="sxs-lookup"><span data-stu-id="269b5-182">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="269b5-183">Du kan även styra bandbreddsanvändningen med alternativ för ```work hour bandwidth``` och ```non-work hour bandwidth``` för en given uppsättning dagar i veckan.</span><span class="sxs-lookup"><span data-stu-id="269b5-183">Bandwidth usage can also be controlled with the options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of the week.</span></span>

<span data-ttu-id="269b5-184">Ange information för proxy och bandbredd görs med hjälp av den [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="269b5-184">Setting the proxy and bandwidth details is done using the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="269b5-185">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="269b5-185">Encryption settings</span></span>
<span data-ttu-id="269b5-186">Den säkerhetskopiera informationen som skickas till Azure Backup krypteras för att skydda sekretessen för data.</span><span class="sxs-lookup"><span data-stu-id="269b5-186">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="269b5-187">Krypteringslösenfrasen är ”password” att dekryptera data vid tidpunkten för återställning.</span><span class="sxs-lookup"><span data-stu-id="269b5-187">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="269b5-188">Behåll informationen lösenfras säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="269b5-188">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="269b5-189">Du kommer inte att återställa data från Azure utan lösenfras.</span><span class="sxs-lookup"><span data-stu-id="269b5-189">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="269b5-190">Säkerhetskopiera filer och mappar</span><span class="sxs-lookup"><span data-stu-id="269b5-190">Back up files and folders</span></span>
<span data-ttu-id="269b5-191">Alla säkerhetskopiorna från Windows-servrar och klienter till Azure Backup styrs av en princip.</span><span class="sxs-lookup"><span data-stu-id="269b5-191">All your backups from Windows Servers and clients to Azure Backup are governed by a policy.</span></span> <span data-ttu-id="269b5-192">Principen består av tre delar:</span><span class="sxs-lookup"><span data-stu-id="269b5-192">The policy comprises three parts:</span></span>

1. <span data-ttu-id="269b5-193">En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver tas och synkroniseras med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="269b5-193">A **backup schedule** that specifies when backups need to be taken and synchronized with the service.</span></span>
2. <span data-ttu-id="269b5-194">En **bevarandeschema** som anger hur lång tid att behålla återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="269b5-194">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>
3. <span data-ttu-id="269b5-195">En **filen inkludering/undantag specifikationen** som avgör vad bör säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="269b5-195">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="269b5-196">I det här dokumentet eftersom vi automatisera säkerhetskopiering, vi antar ingenting inte har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="269b5-196">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="269b5-197">Vi börjar med att skapa en ny princip för säkerhetskopiering med hjälp av [ny OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet och använder den.</span><span class="sxs-lookup"><span data-stu-id="269b5-197">We begin by creating a new backup policy using the [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet and using it.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="269b5-198">Just nu principen är tom och andra cmdletar är behövs för att definiera vilka objekt som ska inkluderade eller exkluderade, när säkerhetskopieringar ska köras, och där säkerhetskopiorna lagras.</span><span class="sxs-lookup"><span data-stu-id="269b5-198">At this time the policy is empty and other cmdlets are needed to define what items will be included or excluded, when backups will run, and where the backups will be stored.</span></span>

### <a name="configuring-the-backup-schedule"></a><span data-ttu-id="269b5-199">Konfigurera schemat för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="269b5-199">Configuring the backup schedule</span></span>
<span data-ttu-id="269b5-200">Först 3 delar av en princip är schemat för säkerhetskopiering, som skapas med hjälp av den [ny OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-200">The first of the 3 parts of a policy is the backup schedule, which is created using the [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="269b5-201">Schemat för säkerhetskopiering definierar när säkerhetskopieringar behöver åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="269b5-201">The backup schedule defines when backups need to be taken.</span></span> <span data-ttu-id="269b5-202">När du skapar ett schema som du måste ange 2 indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="269b5-202">When creating a schedule you need to specify 2 input parameters:</span></span>

* <span data-ttu-id="269b5-203">**Dagar i veckan** som säkerhetskopieringen ska köras.</span><span class="sxs-lookup"><span data-stu-id="269b5-203">**Days of the week** that the backup should run.</span></span> <span data-ttu-id="269b5-204">Du kan köra jobbet på bara en dag eller varje dag i veckan eller en kombination i mellan.</span><span class="sxs-lookup"><span data-stu-id="269b5-204">You can run the backup job on just one day, or every day of the week, or any combination in between.</span></span>
* <span data-ttu-id="269b5-205">**Tider på dagen** när säkerhetskopieringen ska köras.</span><span class="sxs-lookup"><span data-stu-id="269b5-205">**Times of the day** when the backup should run.</span></span> <span data-ttu-id="269b5-206">Du kan ange upp till 3 olika tidpunkter på dagen när säkerhetskopieringen ska utlösas.</span><span class="sxs-lookup"><span data-stu-id="269b5-206">You can define up to 3 different times of the day when the backup will be triggered.</span></span>

<span data-ttu-id="269b5-207">Du kan till exempel konfigurera en princip för säkerhetskopiering som körs klockan 4 varje lördag och söndag.</span><span class="sxs-lookup"><span data-stu-id="269b5-207">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="269b5-208">Schemat för säkerhetskopiering måste vara kopplad till en princip och detta kan uppnås med hjälp av den [Set OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-208">The backup schedule needs to be associated with a policy, and this can be achieved by using the [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="269b5-209">Konfigurera en bevarandeprincip</span><span class="sxs-lookup"><span data-stu-id="269b5-209">Configuring a retention policy</span></span>
<span data-ttu-id="269b5-210">Bevarandeprincipen definierar hur länge återställningspunkter som skapats från säkerhetskopieringsjobb bevaras.</span><span class="sxs-lookup"><span data-stu-id="269b5-210">The retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="269b5-211">När du skapar en ny kvarhållning principen med hjälp av [ny OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) kan du ange antalet dagar som återställningspunkter för säkerhetskopiering ska behållas med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="269b5-211">When creating a new retention policy using the [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify the number of days that the backup recovery points need to be retained with Azure Backup.</span></span> <span data-ttu-id="269b5-212">Exemplet nedan anger en bevarandeprincip 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="269b5-212">The example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="269b5-213">Bevarandeprincipen måste associeras med den huvudsakliga principen med hjälp av cmdlet [Set OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="269b5-213">The retention policy must be associated with the main policy using the cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

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
### <a name="including-and-excluding-files-to-be-backed-up"></a><span data-ttu-id="269b5-214">Inklusive och exklusive filer som ska säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="269b5-214">Including and excluding files to be backed up</span></span>
<span data-ttu-id="269b5-215">En ```OBFileSpec``` objektet definierar filerna som ska inkluderas och exkluderas i en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="269b5-215">An ```OBFileSpec``` object defines the files to be included and excluded in a backup.</span></span> <span data-ttu-id="269b5-216">Det här är en uppsättning regler som omfång för skyddade filer och mappar på en dator.</span><span class="sxs-lookup"><span data-stu-id="269b5-216">This is a set of rules that scope out the protected files and folders on a machine.</span></span> <span data-ttu-id="269b5-217">Du kan använda så många filen inkluderande eller exkluderande regler som krävs och koppla dem till en princip.</span><span class="sxs-lookup"><span data-stu-id="269b5-217">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="269b5-218">När du skapar ett nytt OBFileSpec-objekt, kan du:</span><span class="sxs-lookup"><span data-stu-id="269b5-218">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="269b5-219">Ange vilka filer och mappar som ska tas med</span><span class="sxs-lookup"><span data-stu-id="269b5-219">Specify the files and folders to be included</span></span>
* <span data-ttu-id="269b5-220">Ange vilka filer och mappar som ska uteslutas</span><span class="sxs-lookup"><span data-stu-id="269b5-220">Specify the files and folders to be excluded</span></span>
* <span data-ttu-id="269b5-221">Ange rekursiva säkerhetskopiering av data i en mapp (eller) om endast de översta filerna i den angivna mappen bör säkerhetskopieras upp.</span><span class="sxs-lookup"><span data-stu-id="269b5-221">Specify recursive backup of data in a folder (or) whether only the top-level files in the specified folder should be backed up.</span></span>

<span data-ttu-id="269b5-222">Denna uppnås med hjälp av flaggan - rekursiv i kommandot New OBFileSpec.</span><span class="sxs-lookup"><span data-stu-id="269b5-222">The latter is achieved by using the -NonRecursive flag in the New-OBFileSpec command.</span></span>

<span data-ttu-id="269b5-223">I exemplet nedan vi säkerhetskopiera volym C: och D: och utesluta OS-binärfiler i Windows-mappen och alla temporära mappar.</span><span class="sxs-lookup"><span data-stu-id="269b5-223">In the example below, we'll back up volume C: and D: and exclude the OS binaries in the Windows folder and any temporary folders.</span></span> <span data-ttu-id="269b5-224">För att göra så skapar vi två filen anvisningar med hjälp av den [ny OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - en för inkludering och en för exnkludering.</span><span class="sxs-lookup"><span data-stu-id="269b5-224">To do so we'll create two file specifications using the [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="269b5-225">När filen specifikationer har skapats, de är kopplade till en princip med hjälp av den [Lägg till OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-225">Once the file specifications have been created, they're associated with the policy using the [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

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

### <a name="applying-the-policy"></a><span data-ttu-id="269b5-226">Tillämpa principen</span><span class="sxs-lookup"><span data-stu-id="269b5-226">Applying the policy</span></span>
<span data-ttu-id="269b5-227">Nu principobjektet är klar och har en associerad schemat för säkerhetskopiering, bevarandeprincip och en inkludering/undantagslista av filer.</span><span class="sxs-lookup"><span data-stu-id="269b5-227">Now the policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="269b5-228">Den här principen kan nu allokeras för Azure Backup för att använda.</span><span class="sxs-lookup"><span data-stu-id="269b5-228">This policy can now be committed for Azure Backup to use.</span></span> <span data-ttu-id="269b5-229">Innan du installerar den nya principen se till att det inte finns några befintliga principer för säkerhetskopiering som är kopplade till servern med hjälp av den [ta bort OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-229">Before you apply the newly created policy ensure that there are no existing backup policies associated with the server by using the [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="269b5-230">Ta bort principen uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="269b5-230">Removing the policy will prompt for confirmation.</span></span> <span data-ttu-id="269b5-231">Hoppa över bekräftelse användningen av ```-Confirm:$false``` flaggan med cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-231">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="269b5-232">Genomför principobjektet görs med hjälp av den [Set OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-232">Committing the policy object is done using the [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="269b5-233">Detta kommer också ombeds bekräfta.</span><span class="sxs-lookup"><span data-stu-id="269b5-233">This will also ask for confirmation.</span></span> <span data-ttu-id="269b5-234">Hoppa över bekräftelse användningen av ```-Confirm:$false``` flaggan med cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-234">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
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

<span data-ttu-id="269b5-235">Du kan visa information om en befintlig princip för säkerhetskopiering med hjälp av den [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-235">You can view the details of the existing backup policy using the [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="269b5-236">Du kan gå nedåt ytterligare med den [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) för schemat för säkerhetskopiering och [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet för bevarandeprinciper</span><span class="sxs-lookup"><span data-stu-id="269b5-236">You can drill-down further using the [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for the backup schedule and the [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for the retention policies</span></span>

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

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="269b5-237">Utför en ad hoc-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="269b5-237">Performing an ad-hoc backup</span></span>
<span data-ttu-id="269b5-238">När du har angett en princip för säkerhetskopiering sker säkerhetskopieringar per schemat.</span><span class="sxs-lookup"><span data-stu-id="269b5-238">Once a backup policy has been set the backups will occur per the schedule.</span></span> <span data-ttu-id="269b5-239">Utlöser en ad hoc-säkerhetskopiering är också möjligt med hjälp av den [Start OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="269b5-239">Triggering an ad-hoc backup is also possible using the [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="269b5-240">Återställa data från Azure Backup</span><span class="sxs-lookup"><span data-stu-id="269b5-240">Restore data from Azure Backup</span></span>
<span data-ttu-id="269b5-241">Det här avsnittet hjälper dig genom stegen för att automatisera återställning av data från Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="269b5-241">This section will guide you through the steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="269b5-242">Detta omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="269b5-242">Doing so involves the following steps:</span></span>

1. <span data-ttu-id="269b5-243">Välj källvolymen</span><span class="sxs-lookup"><span data-stu-id="269b5-243">Pick the source volume</span></span>
2. <span data-ttu-id="269b5-244">Välj en säkerhetskopieringspunkt att återställa</span><span class="sxs-lookup"><span data-stu-id="269b5-244">Choose a backup point to restore</span></span>
3. <span data-ttu-id="269b5-245">Välj ett objekt om du vill återställa</span><span class="sxs-lookup"><span data-stu-id="269b5-245">Choose an item to restore</span></span>
4. <span data-ttu-id="269b5-246">Utlösa återställningen</span><span class="sxs-lookup"><span data-stu-id="269b5-246">Trigger the restore process</span></span>

### <a name="picking-the-source-volume"></a><span data-ttu-id="269b5-247">Plocka källvolymen</span><span class="sxs-lookup"><span data-stu-id="269b5-247">Picking the source volume</span></span>
<span data-ttu-id="269b5-248">För att kunna återställa ett objekt från Azure Backup, måste du först identifiera källan för artikeln.</span><span class="sxs-lookup"><span data-stu-id="269b5-248">In order to restore an item from Azure Backup, you first need to identify the source of the item.</span></span> <span data-ttu-id="269b5-249">Eftersom vi körs kommandon i kontexten för en Windows Server eller en Windows-klient måste identifieras redan datorn.</span><span class="sxs-lookup"><span data-stu-id="269b5-249">Since we're executing the commands in the context of a Windows Server or a Windows client, the machine is already identified.</span></span> <span data-ttu-id="269b5-250">Nästa steg för att identifiera källan är att identifiera volymen som innehåller den.</span><span class="sxs-lookup"><span data-stu-id="269b5-250">The next step in identifying the source is to identify the volume containing it.</span></span> <span data-ttu-id="269b5-251">En lista över volymer eller källor säkerhetskopieras från den här datorn kan hämtas genom att köra den [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-251">A list of volumes or sources being backed up from this machine can be retrieved by executing the [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="269b5-252">Det här kommandot returnerar en matris med alla källor säkerhetskopierade från den här servern eller-klienten.</span><span class="sxs-lookup"><span data-stu-id="269b5-252">This command returns an array of all the sources backed up from this server/client.</span></span>

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

### <a name="choosing-a-backup-point-to-restore"></a><span data-ttu-id="269b5-253">Om du väljer en säkerhetskopieringspunkt att återställa</span><span class="sxs-lookup"><span data-stu-id="269b5-253">Choosing a backup point to restore</span></span>
<span data-ttu-id="269b5-254">Listan över säkerhetskopiering punkter kan hämtas genom att köra den [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdleten med lämpliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="269b5-254">The list of backup points can be retrieved by executing the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="269b5-255">I vårt exempel väljer du den senaste säkerhetskopieringskälla tidpunkten för källvolymen *D:* och använda den för att återställa en viss fil.</span><span class="sxs-lookup"><span data-stu-id="269b5-255">In our example, we’ll choose the latest backup point for the source volume *D:* and use it to recover a specific file.</span></span>

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
<span data-ttu-id="269b5-256">Objektet ```$rps``` är en matris med säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="269b5-256">The object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="269b5-257">Det första elementet är den senaste tidpunkten och nte elementet är den äldsta.</span><span class="sxs-lookup"><span data-stu-id="269b5-257">The first element is the latest point and the Nth element is the oldest point.</span></span> <span data-ttu-id="269b5-258">Om du vill välja den senaste tidpunkten, kommer vi att använda ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="269b5-258">To choose the latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-to-restore"></a><span data-ttu-id="269b5-259">Om du väljer ett objekt om du vill återställa</span><span class="sxs-lookup"><span data-stu-id="269b5-259">Choosing an item to restore</span></span>
<span data-ttu-id="269b5-260">Att identifiera exakt filen eller mappen för att återställa rekursivt används den [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-260">To identify the exact file or folder to restore, recursively use the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="269b5-261">På så sätt kan bläddra mapphierarkin enbart med den ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="269b5-261">That way the folder hierarchy can be browsed solely using the ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="269b5-262">I det här exemplet, om vi vill återställa filen *finances.xls* vi kan referera till som använder objektet ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="269b5-262">In this example, if we want to restore the file *finances.xls* we can reference that using the object ```$filesFolders[1]```.</span></span>

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

<span data-ttu-id="269b5-263">Du kan också söka efter objekt som ska återställas med hjälp av den ```Get-OBRecoverableItem``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-263">You can also search for items to restore using the ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="269b5-264">I vårt exempel att söka efter *finances.xls* vi gick att hämta en referens i filen genom att köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="269b5-264">In our example, to search for *finances.xls* we could get a handle on the file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a><span data-ttu-id="269b5-265">Utlösa återställningen</span><span class="sxs-lookup"><span data-stu-id="269b5-265">Triggering the restore process</span></span>
<span data-ttu-id="269b5-266">För att utlösa återställningen behöver vi först ange återställningsalternativ.</span><span class="sxs-lookup"><span data-stu-id="269b5-266">To trigger the restore process, we first need to specify the recovery options.</span></span> <span data-ttu-id="269b5-267">Detta kan göras med hjälp av den [ny OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="269b5-267">This can be done by using the [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="269b5-268">I det här exemplet antar vi att vi vill återställa filerna till *C:\temp*.</span><span class="sxs-lookup"><span data-stu-id="269b5-268">For this example, let's assume that we want to restore the files to *C:\temp*.</span></span> <span data-ttu-id="269b5-269">Vi förutsätter också att vi vill hoppa över filer som redan finns i målmappen *C:\temp*.</span><span class="sxs-lookup"><span data-stu-id="269b5-269">Let's also assume that we want to skip files that already exist on the destination folder *C:\temp*.</span></span> <span data-ttu-id="269b5-270">Om du vill skapa ett återställningsalternativ, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="269b5-270">To create such a recovery option, use the following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="269b5-271">Aktivera nu återställning med hjälp av den [Start OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) på den valda ```$item``` från utdata från den ```Get-OBRecoverableItem``` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="269b5-271">Now trigger restore by using the [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on the selected ```$item``` from the output of the ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a><span data-ttu-id="269b5-272">Avinstallera Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="269b5-272">Uninstalling the Azure Backup agent</span></span>
<span data-ttu-id="269b5-273">Avinstallera Azure Backup-agenten kan göras med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="269b5-273">Uninstalling the Azure Backup agent can be done by using the following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="269b5-274">Avinstallera binärfilerna för agenten från datorn påverkar vissa att tänka på:</span><span class="sxs-lookup"><span data-stu-id="269b5-274">Uninstalling the agent binaries from the machine has some consequences to consider:</span></span>

* <span data-ttu-id="269b5-275">Fil-filter tas bort från datorn och spårning av ändringar har stoppats.</span><span class="sxs-lookup"><span data-stu-id="269b5-275">It removes the file-filter from the machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="269b5-276">All principinformation tas bort från datorn, men principinformation fortsätter lagras i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="269b5-276">All policy information is removed from the machine, but the policy information continues to be stored in the service.</span></span>
* <span data-ttu-id="269b5-277">Alla säkerhetskopieringsscheman tas bort och vidtas inga ytterligare säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="269b5-277">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="269b5-278">Data lagras i Azure förblir och behålls enligt kvarhållning princip installationen av du.</span><span class="sxs-lookup"><span data-stu-id="269b5-278">However, the data stored in Azure remains and is retained as per the retention policy setup by you.</span></span> <span data-ttu-id="269b5-279">Äldre återställningspunkter rensas automatiskt bort.</span><span class="sxs-lookup"><span data-stu-id="269b5-279">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="269b5-280">Fjärrhantering</span><span class="sxs-lookup"><span data-stu-id="269b5-280">Remote management</span></span>
<span data-ttu-id="269b5-281">All hantering runt Azure Backup agent, principer och datakällor kan göras via fjärranslutning via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="269b5-281">All the management around the Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="269b5-282">Den dator som ska hanteras via en fjärranslutning måste förberedas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="269b5-282">The machine that will be managed remotely needs to be prepared correctly.</span></span>

<span data-ttu-id="269b5-283">Som standard konfigureras WinRM-tjänsten för manuell start.</span><span class="sxs-lookup"><span data-stu-id="269b5-283">By default, the WinRM service is configured for manual startup.</span></span> <span data-ttu-id="269b5-284">Starttyp måste anges till *automatisk* och tjänsten ska startas.</span><span class="sxs-lookup"><span data-stu-id="269b5-284">The startup type must be set to *Automatic* and the service should be started.</span></span> <span data-ttu-id="269b5-285">Om du vill kontrollera att WinRM-tjänsten körs, värdet för egenskapen Status ska vara *kör*.</span><span class="sxs-lookup"><span data-stu-id="269b5-285">To verify that the WinRM service is running, the value of the Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="269b5-286">PowerShell ska konfigureras för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="269b5-286">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="269b5-287">Datorn kan nu fjärrhanteras - från installationen av agenten.</span><span class="sxs-lookup"><span data-stu-id="269b5-287">The machine can now be managed remotely - starting from the installation of the agent.</span></span> <span data-ttu-id="269b5-288">Följande skript kopierar agenten till fjärrdatorn och installerar den.</span><span class="sxs-lookup"><span data-stu-id="269b5-288">For example, the following script copies the agent to the remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="269b5-289">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="269b5-289">Next steps</span></span>
<span data-ttu-id="269b5-290">Mer information om Azure Backup för Windows Server eller-klienten finns</span><span class="sxs-lookup"><span data-stu-id="269b5-290">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="269b5-291">Introduktion till Azure Backup</span><span class="sxs-lookup"><span data-stu-id="269b5-291">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="269b5-292">Säkerhetskopiera Windows-servrar</span><span class="sxs-lookup"><span data-stu-id="269b5-292">Back up Windows Servers</span></span>](backup-configure-vault.md)
