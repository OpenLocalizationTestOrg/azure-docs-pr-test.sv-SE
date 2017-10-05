---
title: "Azure Backup - Använd PowerShell för att säkerhetskopiera DPM-arbetsbelastningar | Microsoft Docs"
description: "Lär dig hur du distribuerar och hanterar Azure Backup för Data Protection Manager (DPM) med hjälp av PowerShell"
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
ms.openlocfilehash: 2e3b4a094511a59cfa02917efc2e3e053840af0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="904fe-103">Distribuera och hantera säkerhetskopiering till Azure för DPM-servrar (Data Protection Manager) med PowerShell</span><span class="sxs-lookup"><span data-stu-id="904fe-103">Deploy and manage backup to Azure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="904fe-104">ARM</span><span class="sxs-lookup"><span data-stu-id="904fe-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="904fe-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="904fe-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="904fe-106">Den här artikeln visar hur du använder PowerShell för att konfigurera Azure Backup på en DPM-server och för att hantera säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="904fe-106">This article shows you how to use PowerShell to setup Azure Backup on a DPM server, and to manage backup and recovery.</span></span>

## <a name="setting-up-the-powershell-environment"></a><span data-ttu-id="904fe-107">Ställa in PowerShell-miljö</span><span class="sxs-lookup"><span data-stu-id="904fe-107">Setting up the PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="904fe-108">Innan du kan använda PowerShell för att hantera säkerhetskopiering från Data Protection Manager till Azure, måste du har rätt miljö i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="904fe-108">Before you can use PowerShell to manage backups from Data Protection Manager to Azure, you need to have the right environment in PowerShell.</span></span> <span data-ttu-id="904fe-109">Se till att du kör följande kommando för att importera modulerna som är rätt och att du kan korrekt refererar till DPM-cmdlets i början av PowerShell-session:</span><span class="sxs-lookup"><span data-stu-id="904fe-109">At the start of the PowerShell session, ensure that you run the following command to import the right modules and allow you to correctly reference the DPM cmdlets:</span></span>

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

## <a name="setup-and-registration"></a><span data-ttu-id="904fe-110">Installation och registrering</span><span class="sxs-lookup"><span data-stu-id="904fe-110">Setup and Registration</span></span>
<span data-ttu-id="904fe-111">Börja:</span><span class="sxs-lookup"><span data-stu-id="904fe-111">To begin:</span></span>

1. <span data-ttu-id="904fe-112">[Hämta senaste PowerShell](https://github.com/Azure/azure-powershell/releases) (lägsta version som krävs är: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="904fe-112">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is: 1.0.0)</span></span>
2. <span data-ttu-id="904fe-113">Aktivera Azure Backup-kommandon genom att växla till *AzureResourceManager* läge med hjälp av den **Switch-AzureMode** cmdleten igen:</span><span class="sxs-lookup"><span data-stu-id="904fe-113">Enable the Azure Backup commandlets by switching to *AzureResourceManager* mode by using the **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="904fe-114">Följande uppgifter för installation och registrering kan automatiseras med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="904fe-114">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="904fe-115">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="904fe-115">Create a Recovery Services vault</span></span>
* <span data-ttu-id="904fe-116">Installera Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="904fe-116">Installing the Azure Backup agent</span></span>
* <span data-ttu-id="904fe-117">Registreras med Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="904fe-117">Registering with the Azure Backup service</span></span>
* <span data-ttu-id="904fe-118">Nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="904fe-118">Networking settings</span></span>
* <span data-ttu-id="904fe-119">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="904fe-119">Encryption settings</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="904fe-120">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="904fe-120">Create a recovery services vault</span></span>
<span data-ttu-id="904fe-121">Följande steg leder dig genom att skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="904fe-121">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="904fe-122">Recovery Services-valvet skiljer sig en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="904fe-122">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="904fe-123">Om du använder Azure Backup för första gången, måste du använda den **registrera AzureRMResourceProvider** för att registrera providern Azure Recovery-tjänsten med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="904fe-123">If you are using Azure Backup for the first time, you must use the **Register-AzureRMResourceProvider** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="904fe-124">Recovery Services-valvet är en ARM-resurs, så du måste placera det inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="904fe-124">The Recovery Services vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="904fe-125">Du kan använda en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="904fe-125">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="904fe-126">När du skapar en ny resursgrupp måste ange namn och plats för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="904fe-126">When creating a new resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="904fe-127">Använd den **ny AzureRmRecoveryServicesVault** för att skapa ett nytt valv.</span><span class="sxs-lookup"><span data-stu-id="904fe-127">Use the **New-AzureRmRecoveryServicesVault** cmdlet to create a new vault.</span></span> <span data-ttu-id="904fe-128">Se till att ange samma plats för valvet som användes för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="904fe-128">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="904fe-129">Ange vilken typ av lagring redundans ska användas. Du kan använda [lokalt Redundant lagring (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) eller [Geo-Redundant lagring (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="904fe-129">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="904fe-130">I följande exempel visas alternativet - BackupStorageRedundancy för testVault anges till GeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="904fe-130">The following example shows the -BackupStorageRedundancy option for testVault is set to GeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="904fe-131">Många Azure Backup-cmdletar kräver Recovery Services-valvet objekt som indata.</span><span class="sxs-lookup"><span data-stu-id="904fe-131">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="904fe-132">Därför är det praktiskt att lagra säkerhetskopian Recovery Services-valvet objekt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="904fe-132">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="904fe-133">Visa valv i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="904fe-133">View the vaults in a subscription</span></span>
<span data-ttu-id="904fe-134">Använd **Get-AzureRmRecoveryServicesVault** att visa listan över alla valv i den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="904fe-134">Use **Get-AzureRmRecoveryServicesVault** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="904fe-135">Du kan använda det här kommandot för att kontrollera att ett nytt valv skapades eller för att se vilka valv är tillgängliga i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="904fe-135">You can use this command to check that a new  vault was created, or to see what vaults are available in the subscription.</span></span>

<span data-ttu-id="904fe-136">Kör kommandot Get-AzureRmRecoveryServicesVault, och i alla valv i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="904fe-136">Run the command, Get-AzureRmRecoveryServicesVault, and all vaults in the subscription are listed.</span></span>

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


## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="904fe-137">Installation av Azure Backup-agenten på en DPM-Server</span><span class="sxs-lookup"><span data-stu-id="904fe-137">Installing the Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="904fe-138">Innan du installerar Azure Backup-agenten som du behöver ha installationsprogrammet hämtade och finns på Windows Server.</span><span class="sxs-lookup"><span data-stu-id="904fe-138">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="904fe-139">Du kan hämta den senaste versionen av installationsprogrammet från den [Microsoft Download Center](http://aka.ms/azurebackup_agent) eller från instrumentpanelssida Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="904fe-139">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="904fe-140">Spara installationsprogrammet i en lättillgänglig plats som * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="904fe-140">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="904fe-141">Om du vill installera agenten, kör du följande kommando i en upphöjd PowerShell-konsol **på DPM-servern**:</span><span class="sxs-lookup"><span data-stu-id="904fe-141">To install the agent, run the following command in an elevated PowerShell console **on the DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="904fe-142">Detta installerar agent med alla standardalternativ.</span><span class="sxs-lookup"><span data-stu-id="904fe-142">This installs the agent with all the default options.</span></span> <span data-ttu-id="904fe-143">Installationen tar några minuter i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="904fe-143">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="904fe-144">Om du inte anger den */nu* alternativet den **Windows Update** öppnas i slutet av installationen att söka efter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="904fe-144">If you do not specify the */nu* option the **Windows Update** window opens at the end of the installation to check for any updates.</span></span>

<span data-ttu-id="904fe-145">Agenten visas i listan över installerade program.</span><span class="sxs-lookup"><span data-stu-id="904fe-145">The agent shows up in the list of installed programs.</span></span> <span data-ttu-id="904fe-146">Listan över installerade program, gå till **Kontrollpanelen** > **program** > **program och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="904fe-146">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Agenten är installerad](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="904fe-148">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="904fe-148">Installation options</span></span>
<span data-ttu-id="904fe-149">Om du vill se alla alternativ som är tillgängliga via kommandoraden använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="904fe-149">To see all the options available via the commandline, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="904fe-150">Tillgängliga alternativ inkluderar:</span><span class="sxs-lookup"><span data-stu-id="904fe-150">The available options include:</span></span>

| <span data-ttu-id="904fe-151">Alternativ</span><span class="sxs-lookup"><span data-stu-id="904fe-151">Option</span></span> | <span data-ttu-id="904fe-152">Information</span><span class="sxs-lookup"><span data-stu-id="904fe-152">Details</span></span> | <span data-ttu-id="904fe-153">Standard</span><span class="sxs-lookup"><span data-stu-id="904fe-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="904fe-154">/q</span><span class="sxs-lookup"><span data-stu-id="904fe-154">/q</span></span> |<span data-ttu-id="904fe-155">Tyst installation</span><span class="sxs-lookup"><span data-stu-id="904fe-155">Quiet installation</span></span> |- |
| <span data-ttu-id="904fe-156">/ p: ”plats”</span><span class="sxs-lookup"><span data-stu-id="904fe-156">/p:"location"</span></span> |<span data-ttu-id="904fe-157">Sökvägen till installationsmappen för Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="904fe-157">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="904fe-158">C:\Program Files\Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="904fe-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="904fe-159">/ s: ”plats”</span><span class="sxs-lookup"><span data-stu-id="904fe-159">/s:"location"</span></span> |<span data-ttu-id="904fe-160">Sökväg till cachemappen för Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="904fe-160">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="904fe-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="904fe-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="904fe-162">/m</span><span class="sxs-lookup"><span data-stu-id="904fe-162">/m</span></span> |<span data-ttu-id="904fe-163">Delta i Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="904fe-163">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="904fe-164">/nu</span><span class="sxs-lookup"><span data-stu-id="904fe-164">/nu</span></span> |<span data-ttu-id="904fe-165">Sök inte efter uppdateringar när installationen är klar</span><span class="sxs-lookup"><span data-stu-id="904fe-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="904fe-166">/d</span><span class="sxs-lookup"><span data-stu-id="904fe-166">/d</span></span> |<span data-ttu-id="904fe-167">Avinstallerar Microsoft Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="904fe-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="904fe-168">/pH</span><span class="sxs-lookup"><span data-stu-id="904fe-168">/ph</span></span> |<span data-ttu-id="904fe-169">Värden proxyadress</span><span class="sxs-lookup"><span data-stu-id="904fe-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="904fe-170">/po</span><span class="sxs-lookup"><span data-stu-id="904fe-170">/po</span></span> |<span data-ttu-id="904fe-171">Portnummer för proxyservern värden</span><span class="sxs-lookup"><span data-stu-id="904fe-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="904fe-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="904fe-172">/pu</span></span> |<span data-ttu-id="904fe-173">Värddatorn Proxyanvändarnamnet</span><span class="sxs-lookup"><span data-stu-id="904fe-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="904fe-174">/PW</span><span class="sxs-lookup"><span data-stu-id="904fe-174">/pw</span></span> |<span data-ttu-id="904fe-175">Lösenord för proxy</span><span class="sxs-lookup"><span data-stu-id="904fe-175">Proxy Password</span></span> |- |

## <a name="registering-dpm-to-a-recovery-services-vault"></a><span data-ttu-id="904fe-176">Registrera DPM till en Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="904fe-176">Registering DPM to a Recovery Services Vault</span></span>
<span data-ttu-id="904fe-177">När du har skapat Recovery Services-valvet, ladda ned den senaste agenten och autentiseringsuppgifter för valv och lagra den på en lämplig plats som C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="904fe-177">After you created the Recovery Services vault, download the latest agent and the vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

<span data-ttu-id="904fe-178">På DPM-servern och kör den [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) för att registrera datorn med valvet.</span><span class="sxs-lookup"><span data-stu-id="904fe-178">On the DPM server, run the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet to register the machine with the vault.</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a><span data-ttu-id="904fe-179">Inledande konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="904fe-179">Initial configuration settings</span></span>
<span data-ttu-id="904fe-180">När DPM-servern registreras med Recovery Services-valvet, startar med standardinställningar för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="904fe-180">Once the DPM Server is registered with the Recovery Services vault, it starts with default subscription settings.</span></span> <span data-ttu-id="904fe-181">Inställningarna prenumeration omfattar nätverk, kryptering och mellanlagringsområdet.</span><span class="sxs-lookup"><span data-stu-id="904fe-181">These subscription settings include Networking, Encryption and the Staging area.</span></span> <span data-ttu-id="904fe-182">Du ändrar inställningarna måste du först få grepp om de befintliga inställningarna för (standard) med hjälp av den [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="904fe-182">To change subscription settings you need to first get a handle on the existing (default) settings using the [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="904fe-183">Alla ändringar som görs till den här lokala PowerShell-objektet ```$setting``` och sedan fullständig objektet strävar efter att DPM och Azure Backup för att spara dem med hjälp av den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-183">All modifications are made to this local PowerShell object ```$setting```  and then the full object is committed to DPM and Azure Backup to save them using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="904fe-184">Du måste använda den ```–Commit``` flagga för att säkerställa att ändringarna sparas.</span><span class="sxs-lookup"><span data-stu-id="904fe-184">You need to use the ```–Commit``` flag to ensure that the changes are persisted.</span></span> <span data-ttu-id="904fe-185">Inställningarna kommer inte tillämpas och används av Azure Backup såvida inte genomförts.</span><span class="sxs-lookup"><span data-stu-id="904fe-185">The settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a><span data-ttu-id="904fe-186">Nätverk</span><span class="sxs-lookup"><span data-stu-id="904fe-186">Networking</span></span>
<span data-ttu-id="904fe-187">Om anslutningen till DPM-datorn till Azure Backup-tjänsten på internet via en proxyserver, måste inställningarna för proxyservern anges för lyckade säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="904fe-187">If the connectivity of the DPM machine to the Azure Backup service on the internet is through a proxy server, then the proxy server settings should be provided for successful backups.</span></span> <span data-ttu-id="904fe-188">Detta görs med hjälp av den ```-ProxyServer```och ```-ProxyPort```, ```-ProxyUsername``` och ```ProxyPassword``` parametrar med den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-188">This is done by using the ```-ProxyServer```and ```-ProxyPort```, ```-ProxyUsername``` and the ```ProxyPassword``` parameters with the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="904fe-189">I det här exemplet har ingen proxyserver så vi uttryckligen avmarkera alla proxy-relaterad information.</span><span class="sxs-lookup"><span data-stu-id="904fe-189">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="904fe-190">Bandbreddsanvändning kan också kontrolleras med alternativ för ```-WorkHourBandwidth``` och ```-NonWorkHourBandwidth``` för en given uppsättning dagar i veckan.</span><span class="sxs-lookup"><span data-stu-id="904fe-190">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of the week.</span></span> <span data-ttu-id="904fe-191">I det här exemplet anger vi inte någon begränsning.</span><span class="sxs-lookup"><span data-stu-id="904fe-191">In this example, we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a><span data-ttu-id="904fe-192">Konfigurera mellanlagringsområdet</span><span class="sxs-lookup"><span data-stu-id="904fe-192">Configuring the staging Area</span></span>
<span data-ttu-id="904fe-193">Azure Backup-agenten som körs på DPM-servern behöver tillfällig lagring för data som återställs från molnet (lokalt mellanlagringsområde).</span><span class="sxs-lookup"><span data-stu-id="904fe-193">The Azure Backup agent running on the DPM server needs temporary storage for data restored from the cloud (local staging area).</span></span> <span data-ttu-id="904fe-194">Konfigurera mellanlagringsområdet med hjälp av den [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet och ```-StagingAreaPath``` parameter.</span><span class="sxs-lookup"><span data-stu-id="904fe-194">Configure the staging area using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and the ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="904fe-195">I exemplet ovan mellanlagringsområdet anges till *C:\StagingArea* i PowerShell-objektet ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="904fe-195">In the example above, the staging area will be set to *C:\StagingArea* in the PowerShell object ```$setting```.</span></span> <span data-ttu-id="904fe-196">Se till att den angivna mappen finns redan, annars misslyckas det slutliga genomförandet av prenumerationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="904fe-196">Ensure that the specified folder already exists, or else the final commit of the subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="904fe-197">Krypteringsinställningar</span><span class="sxs-lookup"><span data-stu-id="904fe-197">Encryption settings</span></span>
<span data-ttu-id="904fe-198">Den säkerhetskopiera informationen som skickas till Azure Backup krypteras för att skydda sekretessen för data.</span><span class="sxs-lookup"><span data-stu-id="904fe-198">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="904fe-199">Krypteringslösenfrasen är ”password” att dekryptera data vid tidpunkten för återställning.</span><span class="sxs-lookup"><span data-stu-id="904fe-199">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span> <span data-ttu-id="904fe-200">Det är viktigt att behålla den här informationen säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="904fe-200">It is important to keep this information safe and secure once it is set.</span></span>

<span data-ttu-id="904fe-201">I exemplet nedan det första kommandot konverterar strängen ```passphrase123456789``` till en säker sträng och tilldelar säker sträng till variabeln med namnet ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="904fe-201">In the example below, the first command converts the string ```passphrase123456789``` to a secure string and assigns the secure string to the variable named ```$Passphrase```.</span></span> <span data-ttu-id="904fe-202">Det andra kommandot anger säker sträng i ```$Passphrase``` som lösenord för kryptering av säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="904fe-202">the second command sets the secure string in ```$Passphrase``` as the password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="904fe-203">Behåll informationen lösenfras säkert när den har angetts.</span><span class="sxs-lookup"><span data-stu-id="904fe-203">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="904fe-204">Du kommer inte att återställa data från Azure utan lösenfras.</span><span class="sxs-lookup"><span data-stu-id="904fe-204">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="904fe-205">Nu ska du har gjort alla nödvändiga ändringar av den ```$setting``` objekt.</span><span class="sxs-lookup"><span data-stu-id="904fe-205">At this point, you should have made all the required changes to the ```$setting``` object.</span></span> <span data-ttu-id="904fe-206">Kom ihåg att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="904fe-206">Remember to commit the changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a><span data-ttu-id="904fe-207">Skydda data till Azure Backup</span><span class="sxs-lookup"><span data-stu-id="904fe-207">Protect data to Azure Backup</span></span>
<span data-ttu-id="904fe-208">I det här avsnittet kan du lägga till en produktionsservern till DPM och skydda sedan data till lokala DPM-lagring och sedan till Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="904fe-208">In this section, you will add a production server to DPM and then protect the data to local DPM storage and then to Azure Backup.</span></span> <span data-ttu-id="904fe-209">I exemplen visar vi hur du säkerhetskopierar filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="904fe-209">In the examples, we will demonstrate how to back up files and folders.</span></span> <span data-ttu-id="904fe-210">Logiken kan enkelt utökas för att säkerhetskopiera alla datakällor som stöds av DPM.</span><span class="sxs-lookup"><span data-stu-id="904fe-210">The logic can easily be extended to backup any DPM-supported data source.</span></span> <span data-ttu-id="904fe-211">Alla DPM-säkerhetskopieringar styrs av en skydd grupp (PG) med fyra delar:</span><span class="sxs-lookup"><span data-stu-id="904fe-211">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="904fe-212">**Medlemmar** är en lista över alla skyddsobjekt (även kallat *datakällor* i DPM) som du vill skydda i samma skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="904fe-212">**Group members** is a list of all the protectable objects (also known as *Datasources* in DPM) that you want to protect in the same protection group.</span></span> <span data-ttu-id="904fe-213">Du kanske vill skydda produktion virtuella datorer i en skyddsgrupp och SQL Server-databaser i en annan skyddsgrupp som de kan ha olika krav för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="904fe-213">For example, you may want to protect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="904fe-214">Innan du kan säkerhetskopiera någon datakälla på en produktionsserver måste du kontrollera att DPM-agenten är installerad på servern och hanteras av DPM.</span><span class="sxs-lookup"><span data-stu-id="904fe-214">Before you can back up any datasource on a production server you need to make sure the DPM Agent is installed on the server and is managed by DPM.</span></span> <span data-ttu-id="904fe-215">Följ anvisningarna för [installera DPM-agenten](https://technet.microsoft.com/library/bb870935.aspx) och länkas till rätt DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="904fe-215">Follow the steps for [installing the DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it to the appropriate DPM Server.</span></span>
2. <span data-ttu-id="904fe-216">**Dataskyddsmetod** anger de målplatserna - band, disk och molnet.</span><span class="sxs-lookup"><span data-stu-id="904fe-216">**Data protection method** specifies the target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="904fe-217">I vårt exempel skyddar vi data till den lokala disken och till molnet.</span><span class="sxs-lookup"><span data-stu-id="904fe-217">In our example we will protect data to the local disk and to the cloud.</span></span>
3. <span data-ttu-id="904fe-218">En **Säkerhetskopieringsschemat** som anger när säkerhetskopieringar behöver tas och hur ofta data ska synkroniseras mellan DPM-servern och produktionsservern.</span><span class="sxs-lookup"><span data-stu-id="904fe-218">A **backup schedule** that specifies when backups need to be taken and how often the data should be synchronized between the DPM Server and the production server.</span></span>
4. <span data-ttu-id="904fe-219">En **bevarandeschema** som anger hur lång tid att behålla återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="904fe-219">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="904fe-220">Skapa en skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="904fe-220">Creating a protection group</span></span>
<span data-ttu-id="904fe-221">Börja med att skapa en ny Skyddsgrupp med hjälp av [ny DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-221">Start by creating a new Protection Group using the [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="904fe-222">Cmdleten ovan skapar en Skyddsgrupp med namnet *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="904fe-222">The above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="904fe-223">En befintlig skyddsgrupp kan också ändras senare om du vill lägga till säkerhetskopiering till Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="904fe-223">An existing protection group can also be modified later to add backup to the Azure cloud.</span></span> <span data-ttu-id="904fe-224">Dock göra några ändringar i skyddsgruppen - ny eller befintlig - vi behöver skaffa en referens till en *ändringsbar* objekt med hjälp av [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-224">However, to make any changes to the Protection Group - new or existing - we need to get a handle on a *modifiable* object using the [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a><span data-ttu-id="904fe-225">Lägga till medlemmar i skyddsgruppen</span><span class="sxs-lookup"><span data-stu-id="904fe-225">Adding group members to the Protection Group</span></span>
<span data-ttu-id="904fe-226">Varje DPM-agenten vet listan med datakällor på den server som den är installerad på.</span><span class="sxs-lookup"><span data-stu-id="904fe-226">Each DPM Agent knows the list of datasources on the server that it is installed on.</span></span> <span data-ttu-id="904fe-227">DPM-agenten måste först skicka en lista med datakällorna på DPM-servern för att lägga till en datakälla i skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="904fe-227">To add a datasource to the Protection Group, the DPM Agent needs to first send a list of the datasources back to the DPM server.</span></span> <span data-ttu-id="904fe-228">En eller flera datakällor är sedan valt och lägga till Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="904fe-228">One or more datasources are then selected and added to the Protection Group.</span></span> <span data-ttu-id="904fe-229">PowerShell-stegen som behövs för att uppnå detta är:</span><span class="sxs-lookup"><span data-stu-id="904fe-229">The PowerShell steps needed to achieve this are:</span></span>

1. <span data-ttu-id="904fe-230">Hämta en lista över alla servrar som hanterade med DPM via DPM-agenten.</span><span class="sxs-lookup"><span data-stu-id="904fe-230">Fetch a list of all servers managed by DPM through the DPM Agent.</span></span>
2. <span data-ttu-id="904fe-231">Välj en specifik server.</span><span class="sxs-lookup"><span data-stu-id="904fe-231">Choose a specific server.</span></span>
3. <span data-ttu-id="904fe-232">Hämta en lista över alla datakällor på servern.</span><span class="sxs-lookup"><span data-stu-id="904fe-232">Fetch a list of all datasources on the server.</span></span>
4. <span data-ttu-id="904fe-233">Välj en eller flera datakällor och lägga till dem i Skyddsgruppen</span><span class="sxs-lookup"><span data-stu-id="904fe-233">Choose one or more datasources and add them to the Protection Group</span></span>

<span data-ttu-id="904fe-234">I listan med servrar där DPM-agenten är installerad och hanteras av DPM-servern förvärvas med den [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-234">The list of servers on which the DPM Agent is installed and is being managed by the DPM Server is acquired with the [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="904fe-235">I det här exemplet kommer att filtrera och bara konfigurera PS med namnet *productionserver01* för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="904fe-235">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="904fe-236">Nu att hämta listan över datakällor på ```$server``` med hjälp av den [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-236">Now fetch the list of datasources on ```$server``` using the [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="904fe-237">I det här exemplet vi filtrering för volymen * D:\* som vi vill konfigurera för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="904fe-237">In this example we are filtering for the volume *D:\* that we want to configure for backup.</span></span> <span data-ttu-id="904fe-238">Den här datakällan läggs sedan till Skyddsgruppen med hjälp av den [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-238">This datasource is then added to the Protection Group using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="904fe-239">Kom ihåg att använda den *ändringsbar* skydd gruppobjekt ```$MPG``` att tilläggen.</span><span class="sxs-lookup"><span data-stu-id="904fe-239">Remember to use the *modifiable* protection group object ```$MPG``` to make the additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="904fe-240">Upprepa detta steg så många gånger som krävs, tills du har lagt till de valda datakällorna i skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="904fe-240">Repeat this step as many times as required, until you have added all the chosen datasources to the protection group.</span></span> <span data-ttu-id="904fe-241">Du kan också starta med en datakälla och slutföra arbetsflödet för att skapa Skyddsgruppen och senare lägga till flera datakällor i Skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="904fe-241">You can also start with just one datasource, and complete the workflow for creating the Protection Group, and at a later point add more datasources to the Protection Group.</span></span>

### <a name="selecting-the-data-protection-method"></a><span data-ttu-id="904fe-242">Välja dataskyddsmetod</span><span class="sxs-lookup"><span data-stu-id="904fe-242">Selecting the data protection method</span></span>
<span data-ttu-id="904fe-243">När datakällorna har lagts till Skyddsgruppen, nästa steg är att ange metoden skydd med den [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-243">Once the datasources have been added to the Protection Group, the next step is to specify the protection method using the [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="904fe-244">I det här exemplet är Skyddsgruppen inställningar för lokal disk och säkerhetskopiering i molnet.</span><span class="sxs-lookup"><span data-stu-id="904fe-244">In this example, the Protection Group is setup for local disk and cloud backup.</span></span> <span data-ttu-id="904fe-245">Du måste också ange datasource som du vill skydda till molnet med den [Lägg till DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet med - Online flagga.</span><span class="sxs-lookup"><span data-stu-id="904fe-245">You also need to specify the datasource that you want to protect to cloud using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a><span data-ttu-id="904fe-246">Ange kvarhållningsintervallet</span><span class="sxs-lookup"><span data-stu-id="904fe-246">Setting the retention range</span></span>
<span data-ttu-id="904fe-247">Loggperiod för säkerhetskopieringen återställningspunkter med de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-247">Set the retention for the backup points using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="904fe-248">När det kan verka udda loggperiod innan schemat för säkerhetskopiering har definierats med den ```Set-DPMPolicyObjective``` cmdlet anger automatiskt ett standardschema för säkerhetskopiering som sedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="904fe-248">While it might seem odd to set the retention before the backup schedule has been defined, using the ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="904fe-249">Det är alltid möjligt att ange säkerhetskopieringen schemalägga först och bevarandeprincip efter.</span><span class="sxs-lookup"><span data-stu-id="904fe-249">It is always possible to set the backup schedule first and the retention policy after.</span></span>

<span data-ttu-id="904fe-250">I exemplet nedan anger cmdleten kvarhållning parametrarna för säkerhetskopiering till disk.</span><span class="sxs-lookup"><span data-stu-id="904fe-250">In the example below, the cmdlet sets the retention parameters for disk backups.</span></span> <span data-ttu-id="904fe-251">Detta behåller säkerhetskopior för 10 dagar och synkronisera data var 6 timme mellan produktionsservern och DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="904fe-251">This will retain backups for 10 days, and sync data every 6 hours between the production server and the DPM server.</span></span> <span data-ttu-id="904fe-252">Den ```SynchronizationFrequencyMinutes``` inte definierar hur ofta en säkerhetskopiering skapas, men hur ofta data kopieras till DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="904fe-252">The ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied to the DPM server.</span></span>  <span data-ttu-id="904fe-253">Den här inställningen förhindrar att säkerhetskopiorna blir för stor.</span><span class="sxs-lookup"><span data-stu-id="904fe-253">This setting prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="904fe-254">För säkerhetskopiering ska Azure (DPM refererar till dem som onlinesäkerhetskopieringar) i kvarhållningsperioder kan konfigureras för [långsiktigt kvarhållning med en farfar-pappa-Son (offentlig sektor GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="904fe-254">For backups going to Azure (DPM refers to them as Online backups) the retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="904fe-255">Det vill säga kan du definiera en kombinerad bevarandeprincip som rör varje dag, vecka, månad och år bevarandeprinciper.</span><span class="sxs-lookup"><span data-stu-id="904fe-255">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="904fe-256">I det här exemplet vi skapa en matris som representerar det komplexa kvarhållning schema som vi vill och sedan konfigurera kvarhållning intervall med den [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-256">In this example, we create an array representing the complex retention scheme that we want, and then configure the retention range using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a><span data-ttu-id="904fe-257">Ange schemat för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="904fe-257">Set the backup schedule</span></span>
<span data-ttu-id="904fe-258">DPM anger en säkerhetskopiering standardschemat automatiskt om du anger en skydd mål med hjälp av den ```Set-DPMPolicyObjective``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-258">DPM sets a default backup schedule automatically if you specify the protection objective using the ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="904fe-259">Du kan ändra standardscheman den [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet följt av den [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-259">To change the default schedules, use the [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by the [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="904fe-260">I exemplet ovan ```$onlineSch``` är en matris med fyra element som innehåller befintliga schema för onlineskydd för Skyddsgruppen i GFS schemat:</span><span class="sxs-lookup"><span data-stu-id="904fe-260">In the above example, ```$onlineSch``` is an array with four elements that contains the existing online protection schedule for the Protection Group in the GFS scheme:</span></span>

1. <span data-ttu-id="904fe-261">```$onlineSch[0]```innehåller dagsschema</span><span class="sxs-lookup"><span data-stu-id="904fe-261">```$onlineSch[0]``` contains the daily schedule</span></span>
2. <span data-ttu-id="904fe-262">```$onlineSch[1]```innehåller veckoschemat</span><span class="sxs-lookup"><span data-stu-id="904fe-262">```$onlineSch[1]``` contains the weekly schedule</span></span>
3. <span data-ttu-id="904fe-263">```$onlineSch[2]```innehåller månadsschema</span><span class="sxs-lookup"><span data-stu-id="904fe-263">```$onlineSch[2]``` contains the monthly schedule</span></span>
4. <span data-ttu-id="904fe-264">```$onlineSch[3]```innehåller årlig schemat</span><span class="sxs-lookup"><span data-stu-id="904fe-264">```$onlineSch[3]``` contains the yearly schedule</span></span>

<span data-ttu-id="904fe-265">Om du behöver ändra veckoschemat måste referera till den ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="904fe-265">So if you need to modify the weekly schedule, you need to refer to the ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="904fe-266">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="904fe-266">Initial backup</span></span>
<span data-ttu-id="904fe-267">När en datakälla för första gången, måste DPM säkerhetskopierar skapas första replik som skapar en fullständig kopia av datasource som ska skyddas på DPM-replikvolymen.</span><span class="sxs-lookup"><span data-stu-id="904fe-267">When backing up a datasource for the first time, DPM needs creates initial replica that creates a full copy of the datasource to be protected on DPM replica volume.</span></span> <span data-ttu-id="904fe-268">Den här aktiviteten kan antingen schemaläggas för en viss tid, eller kan aktiveras manuellt med hjälp av den [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet med parametern ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="904fe-268">This activity can either be scheduled for a specific time, or can be triggered manually, using the [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with the parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="904fe-269">Ändra storlek på DPM-replikeringen & återställningspunktvolymen</span><span class="sxs-lookup"><span data-stu-id="904fe-269">Changing the size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="904fe-270">Du kan också ändra storlek på DPM-replikvolymen och skuggkopia av volymen med hjälp av [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet enligt följande exempel: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - DataSource $DS - protectiongroup $MPG-manuell - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="904fe-270">You can also change the size of DPM Replica volume and Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in the following example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-the-changes-to-the-protection-group"></a><span data-ttu-id="904fe-271">Bekräfta ändringarna i skyddsgruppen</span><span class="sxs-lookup"><span data-stu-id="904fe-271">Committing the changes to the Protection Group</span></span>
<span data-ttu-id="904fe-272">Slutligen måste ändringarna genomföras innan DPM kan säkerhetskopiera per den nya Skyddsgruppen-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="904fe-272">Finally, the changes need to be committed before DPM can take the backup per the new Protection Group configuration.</span></span> <span data-ttu-id="904fe-273">Du kan göra detta med hjälp av den [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-273">This can be achieved using the [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a><span data-ttu-id="904fe-274">Visa säkerhetskopiering punkter</span><span class="sxs-lookup"><span data-stu-id="904fe-274">View the backup points</span></span>
<span data-ttu-id="904fe-275">Du kan använda den [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) att hämta en lista över alla återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="904fe-275">You can use the [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet to get a list of all recovery points for a datasource.</span></span> <span data-ttu-id="904fe-276">I det här exemplet ska du:</span><span class="sxs-lookup"><span data-stu-id="904fe-276">In this example, we will:</span></span>

* <span data-ttu-id="904fe-277">Hämta alla PGs på DPM-servern och lagras i en matris```$PG```</span><span class="sxs-lookup"><span data-stu-id="904fe-277">fetch all the PGs on the DPM server and stored in an array ```$PG```</span></span>
* <span data-ttu-id="904fe-278">Hämta datakällorna som motsvarar den```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="904fe-278">get the datasources corresponding to the ```$PG[0]```</span></span>
* <span data-ttu-id="904fe-279">Hämta alla återställningspunkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="904fe-279">get all the recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="904fe-280">Återställa skyddade data på Azure</span><span class="sxs-lookup"><span data-stu-id="904fe-280">Restore data protected on Azure</span></span>
<span data-ttu-id="904fe-281">Återställa data är en kombination av en ```RecoverableItem``` objekt och en ```RecoveryOption``` objekt.</span><span class="sxs-lookup"><span data-stu-id="904fe-281">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="904fe-282">I det föregående avsnittet vi en lista över säkerhetskopiering punkter för en datakälla.</span><span class="sxs-lookup"><span data-stu-id="904fe-282">In the previous section, we got a list of the backup points for a datasource.</span></span>

<span data-ttu-id="904fe-283">I exemplet nedan visar vi hur du återställer en virtuell dator för Hyper-V från Azure Backup genom att kombinera säkerhetskopiering punkter med mål för återställning.</span><span class="sxs-lookup"><span data-stu-id="904fe-283">In the example below, we demonstrate how to restore a Hyper-V virtual machine from Azure Backup by combining backup points with the target for recovery.</span></span> <span data-ttu-id="904fe-284">Det här exemplet innehåller:</span><span class="sxs-lookup"><span data-stu-id="904fe-284">This example includes:</span></span>

* <span data-ttu-id="904fe-285">Skapa en återställningspunkt alternativet med hjälp av den [ny DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-285">Creating a recovery option using the  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="904fe-286">Hämtar punktmatrisen säkerhetskopiering med hjälp av den ```Get-DPMRecoveryPoint``` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="904fe-286">Fetching the array of backup points using the ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="904fe-287">Om du väljer en säkerhetskopieringspunkt att återställa från.</span><span class="sxs-lookup"><span data-stu-id="904fe-287">Choosing a backup point to restore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="904fe-288">Kommandona kan enkelt utökas för någon typ av datakälla.</span><span class="sxs-lookup"><span data-stu-id="904fe-288">The commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="904fe-289">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="904fe-289">Next steps</span></span>
* <span data-ttu-id="904fe-290">Mer information om DPM till Azure Backup finns [introduktion till DPM-säkerhetskopiering](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="904fe-290">For more information about DPM to Azure Backup see [Introduction to DPM Backup](backup-azure-dpm-introduction.md)</span></span>
