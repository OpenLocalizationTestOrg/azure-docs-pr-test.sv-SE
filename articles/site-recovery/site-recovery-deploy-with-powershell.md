---
title: aaaReplicate Hyper-V VMs tooAzure i hello klassiska portalen med PowerShell | Microsoft Docs
description: Automatisera hello replikering av Hyper-V virtuella datorer i VMM-moln med Site Recovery och PowerShell i hello klassiska portalen
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: d6847b46ac227209e6890de4ab603b23f827360f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-tooazure-with-powershell-in-hello-classic-portal"></a><span data-ttu-id="0373f-103">Replikera virtuella Hyper-V-datorer tooAzure med PowerShell i hello klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="0373f-103">Replicate Hyper-V VMs tooAzure with PowerShell in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0373f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0373f-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="0373f-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0373f-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="0373f-106">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="0373f-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="0373f-107">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="0373f-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="0373f-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="0373f-108">Overview</span></span>
<span data-ttu-id="0373f-109">Azure Site Recovery bidrar tooyour disaster recovery (BCDR) strategi för affärskontinuitet och genom att samordna replikering, redundans och återställning av virtuella datorer i ett antal distributionsscenarier.</span><span class="sxs-lookup"><span data-stu-id="0373f-109">Azure Site Recovery contributes tooyour business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="0373f-110">En fullständig lista över distributionen scenarier finns hello [översikt över Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0373f-110">For a full list of deployment scenarios see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="0373f-111">Den här artikeln visar hur toouse PowerShell tooautomate vanliga uppgifter du behöver tooperform när du ställer in Azure Site Recovery tooreplicate Hyper-V virtuella datorer i System Center VMM-moln tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="0373f-111">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooAzure storage.</span></span>

<span data-ttu-id="0373f-112">hello artikeln innehåller kraven för hello scenariot och visar dig hur tooset upp ett Site Recovery-valvet, installera hello Azure Site Recovery-providern på VMM-källservern hello, registrera hello servern i hello valv, lägga till ett Azure storage-konto, installera hello Azure Recovery Services-agenten på Hyper-V-värdservrar, konfigurera skyddsinställningar för VMM-moln som kommer att tillämpas tooall skyddade virtuella datorer och sedan aktivera skyddet för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="0373f-112">hello article includes prerequisites for hello scenario, and shows you how tooset up a Site Recovery vault, install hello Azure Site Recovery Provider on hello source VMM server, register hello server in hello vault, add an Azure storage account, install hello Azure Recovery Services agent on Hyper-V host servers, configure protection settings for VMM clouds that will be applied tooall protected virtual machines, and then enable protection for those virtual machines.</span></span> <span data-ttu-id="0373f-113">Avsluta med att testa hello redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="0373f-113">Finish up by testing hello failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="0373f-114">Om du stöter på problem med att installera det här scenariot, publicera dina frågor på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0373f-114">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="0373f-115">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0373f-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0373f-116">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0373f-116">This article covers using hello Classic deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="0373f-117">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0373f-117">Before you start</span></span>
<span data-ttu-id="0373f-118">Kontrollera att du har dessa krav är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="0373f-118">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="0373f-119">Krav för Azure</span><span class="sxs-lookup"><span data-stu-id="0373f-119">Azure prerequisites</span></span>
* <span data-ttu-id="0373f-120">Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto.</span><span class="sxs-lookup"><span data-stu-id="0373f-120">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="0373f-121">Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0373f-121">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0373f-122">Du behöver ett Azure storage-konto toostore replikerade data.</span><span class="sxs-lookup"><span data-stu-id="0373f-122">You'll need an Azure storage account toostore replicated data.</span></span> <span data-ttu-id="0373f-123">hello-kontot måste geo-replikering aktiverat.</span><span class="sxs-lookup"><span data-stu-id="0373f-123">hello account needs geo-replication enabled.</span></span> <span data-ttu-id="0373f-124">Det bör vara i samma region som hello Azure Site Recovery-valvet hello och associeras med hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0373f-124">It should be in hello same region as hello Azure Site Recovery vault and be associated with hello same subscription.</span></span> <span data-ttu-id="0373f-125">[Lär dig mer om Azure storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0373f-125">[Learn more about Azure storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="0373f-126">Du behöver toomake till virtuella datorer som du vill tooprotect följer [förutsättningar för Azure virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="0373f-126">You'll need toomake sure that virtual machines you want tooprotect comply with [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

### <a name="vmm-prerequisites"></a><span data-ttu-id="0373f-127">VMM – krav</span><span class="sxs-lookup"><span data-stu-id="0373f-127">VMM prerequisites</span></span>
* <span data-ttu-id="0373f-128">Du måste VMM-servern som körs på System Center 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="0373f-128">You'll need  VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="0373f-129">Du behöver minst ett moln på hello VMM-servern som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="0373f-129">You'll need at least one cloud on hello VMM server you want tooprotect.</span></span> <span data-ttu-id="0373f-130">hello moln bör innehålla:</span><span class="sxs-lookup"><span data-stu-id="0373f-130">hello cloud should contain:</span></span>
  * <span data-ttu-id="0373f-131">En eller flera VMM-värdgrupper.</span><span class="sxs-lookup"><span data-stu-id="0373f-131">One or more VMM host groups.</span></span>
  * <span data-ttu-id="0373f-132">En eller flera Hyper-V-värdservrar eller kluster i varje värdgrupp.</span><span class="sxs-lookup"><span data-stu-id="0373f-132">One or more Hyper-V host servers or clusters in each host group .</span></span>
  * <span data-ttu-id="0373f-133">En eller flera virtuella datorer på källservern för hello Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="0373f-133">One or more virtual machines on hello source Hyper-V server.</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="0373f-134">Hyper-V-krav</span><span class="sxs-lookup"><span data-stu-id="0373f-134">Hyper-V prerequisites</span></span>
* <span data-ttu-id="0373f-135">hello Hyper-V-värdservrar måste köra minst **Windows Server 2012** med Hyper-V-rollen eller **Microsoft Hyper-V Server 2012** och har hello senaste uppdateringarna installerade.</span><span class="sxs-lookup"><span data-stu-id="0373f-135">hello host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have hello latest updates installed.</span></span>
* <span data-ttu-id="0373f-136">Om du kör Hyper-V i ett kluster bör du vara medveten om att klusterutjämning inte skapas automatiskt om du har ett statiskt IP-adressbaserat kluster.</span><span class="sxs-lookup"><span data-stu-id="0373f-136">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="0373f-137">Du behöver tooconfigure hello klusterutjämning manuellt.</span><span class="sxs-lookup"><span data-stu-id="0373f-137">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="0373f-138">toodo detta i Serverhanteraren > Klusterhanteraren för växling vid fel, Anslut toohello klustret, klicka på **konfigurera roll** och välj **Hyper-V Replica Broker** i hello **Välj roll**skärmen i guiden hög tillgänglighet för hello.</span><span class="sxs-lookup"><span data-stu-id="0373f-138">toodo this, in Server Manager > Failover Cluster Manager, connect toohello cluster, click **Configure Role** and select **Hyper-V Replica Broker** in hello **Select Role** screen of hello High Availability wizard.</span></span>
* <span data-ttu-id="0373f-139">Alla Hyper-V-värdserver eller kluster som du vill toomanage skydd måste ingå i ett VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="0373f-139">Any Hyper-V host server or cluster for which you want toomanage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="0373f-140">Krav för nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="0373f-140">Network mapping prerequisites</span></span>
<span data-ttu-id="0373f-141">När du skyddar virtuella datorer i Azure mappar nätverksmappningen mellan Virtuella datornätverk på VMM-källservern hello och rikta Azure nätverk tooenable hello följande:</span><span class="sxs-lookup"><span data-stu-id="0373f-141">When you protect virtual machines in Azure network mapping maps between VM networks on hello source VMM server and target Azure networks tooenable hello following:</span></span>

* <span data-ttu-id="0373f-142">Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, oavsett vilken återställningsplan de finns i.</span><span class="sxs-lookup"><span data-stu-id="0373f-142">All machines which fail over on hello same network can connect tooeach other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="0373f-143">Om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan kan virtuella datorer ansluta tooother lokala virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0373f-143">If a network gateway is setup on hello target Azure network, virtual machines can connect tooother on-premises virtual machines.</span></span>
* <span data-ttu-id="0373f-144">Om du inte konfigurerar nätverk mappning endast virtuella datorer som växlar över i hello samma återställningsplan kommer att kunna tooconnect tooeach andra efter växling vid fel tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0373f-144">If you don’t configure network mapping only virtual machines that fail over in hello same recovery plan will be able tooconnect tooeach other after failover tooAzure.</span></span>

<span data-ttu-id="0373f-145">Om du vill toodeploy nätverksmappning behöver hello följande:</span><span class="sxs-lookup"><span data-stu-id="0373f-145">If you want toodeploy network mapping you'll need hello following:</span></span>

* <span data-ttu-id="0373f-146">hello virtuella datorer du vill tooprotect på VMM-källservern hello ska vara anslutna tooa VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-146">hello virtual machines you want tooprotect on hello source VMM server should be connected tooa VM network.</span></span> <span data-ttu-id="0373f-147">Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.</span><span class="sxs-lookup"><span data-stu-id="0373f-147">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
* <span data-ttu-id="0373f-148">En Azure-nätverk toowhich replikerade virtuella datorer kan ansluta efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="0373f-148">An Azure network toowhich replicated virtual machines can connect after failover.</span></span> <span data-ttu-id="0373f-149">Du väljer det här nätverket vid hello tiden för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="0373f-149">You'll select this network at hello time of failover.</span></span> <span data-ttu-id="0373f-150">hello nätverket bör finnas i hello samma region som din Azure Site Recovery-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0373f-150">hello network should be in hello same region as your Azure Site Recovery subscription.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="0373f-151">PowerShell-krav</span><span class="sxs-lookup"><span data-stu-id="0373f-151">PowerShell prerequisites</span></span>
<span data-ttu-id="0373f-152">Kontrollera att du har Azure PowerShell redo toogo.</span><span class="sxs-lookup"><span data-stu-id="0373f-152">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="0373f-153">Om du redan använder PowerShell, behöver du tooupgrade tooversion 0.8.10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0373f-153">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="0373f-154">Information om hur du konfigurerar PowerShell finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="0373f-154">For information about setting up PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="0373f-155">När du har skapat och konfigurerat PowerShell, kan du visa alla hello tillgängliga cmdlets för hello tjänsten [här](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0373f-155">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="0373f-156">toolearn om tips som kan hjälpa dig att använda hello-cmdlets, till exempel hur parametervärden indata och utdata hanteras vanligtvis av Azure PowerShell finns [Kom igång med Azure-Cmdlets](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="0373f-156">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see [Get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="0373f-157">Steg 1: Ange hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="0373f-157">Step 1: Set hello subscription</span></span>
<span data-ttu-id="0373f-158">Kör följande cmdlets i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0373f-158">In PowerShell, run these cmdlets:</span></span>

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

<span data-ttu-id="0373f-159">Ersätt hello element i hello ”< >” med din specifika information.</span><span class="sxs-lookup"><span data-stu-id="0373f-159">Replace hello elements within hello "< >" with your specific information.</span></span>

## <a name="step-2-create-a-site-recovery-vault"></a><span data-ttu-id="0373f-160">Steg 2: Skapa ett Site Recovery-valv</span><span class="sxs-lookup"><span data-stu-id="0373f-160">Step 2: Create a Site Recovery vault</span></span>
<span data-ttu-id="0373f-161">Ersätt hello element i hello ”< >” med din specifika information i PowerShell och köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0373f-161">In PowerShell, replace hello elements within hello "< >" with your specific information, and run these commands:</span></span>

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a><span data-ttu-id="0373f-162">Steg 3: Skapa en valvregistreringsnyckel</span><span class="sxs-lookup"><span data-stu-id="0373f-162">Step 3: Generate a vault registration key</span></span>
<span data-ttu-id="0373f-163">Generera en registreringsnyckel i valvet hello.</span><span class="sxs-lookup"><span data-stu-id="0373f-163">Generate a registration key in hello vault.</span></span> <span data-ttu-id="0373f-164">När du hämtar hello Azure Site Recovery-providern och installera den på hello VMM-servern, ska du använda denna nyckel tooregister hello VMM-server i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="0373f-164">After you download hello Azure Site Recovery Provider and install it on hello VMM server, you'll use this key tooregister hello VMM server in hello vault.</span></span>

1. <span data-ttu-id="0373f-165">Hämta filen hello valvet och ange hello kontext:</span><span class="sxs-lookup"><span data-stu-id="0373f-165">Get hello vault setting file and set hello context:</span></span>

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. <span data-ttu-id="0373f-166">Ange hello valvet kontexten genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-166">Set hello vault context by running hello following commands:</span></span>

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="0373f-167">Steg 4: Installera hello Azure Site Recovery Provider</span><span class="sxs-lookup"><span data-stu-id="0373f-167">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="0373f-168">Skapa en katalog på hello VMM-datorn genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-168">On hello VMM machine, create a directory by running hello following command:</span></span>

   ```

   pushd C:\ASR\

   ```
2. <span data-ttu-id="0373f-169">Extrahera hello filer med hjälp av hello ned providern genom att köra följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="0373f-169">Extract hello files using hello downloaded provider by running hello following command</span></span>

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. <span data-ttu-id="0373f-170">Installera hello-providern med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0373f-170">Install hello provider using hello following commands:</span></span>

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   <span data-ttu-id="0373f-171">Vänta tills hello installation toofinish.</span><span class="sxs-lookup"><span data-stu-id="0373f-171">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="0373f-172">Registrera hello servern i hello valvet med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0373f-172">Register hello server in hello vault using hello following command:</span></span>

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="0373f-173">Steg 5: Skapa ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="0373f-173">Step 5: Create an Azure storage account</span></span>
<span data-ttu-id="0373f-174">Om du inte har ett Azure storage-konto skapar du ett konto med geo-replikering aktiverat genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-174">If you don't have an Azure storage account, create a geo-replication enabled account by running hello following command:</span></span>

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

<span data-ttu-id="0373f-175">Observera att hello storage-konto måste vara i samma region som hello Azure Site Recovery-tjänsten hello och associeras med hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0373f-175">Note that hello storage account must be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription.</span></span>

## <a name="step-6-install-hello-azure-recovery-services-agent"></a><span data-ttu-id="0373f-176">Steg 6: Installera hello Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="0373f-176">Step 6: Install hello Azure Recovery Services Agent</span></span>
<span data-ttu-id="0373f-177">Du vill tooprotect från hello Azure-portalen installera hello Azure Recovery Services-agenten på varje Hyper-V-värdserver i hello VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="0373f-177">From hello Azure portal, install hello Azure Recovery Services agent on each Hyper-V host server located in hello VMM clouds you want tooprotect.</span></span>

<span data-ttu-id="0373f-178">Kör följande kommando på alla värdar i VMM hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-178">Run hello following command on all VMM hosts:</span></span>

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="0373f-179">Steg 7: Konfigurera moln skyddsinställningarna</span><span class="sxs-lookup"><span data-stu-id="0373f-179">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="0373f-180">Skapa en moln skydd profil tooAzure genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-180">Create a cloud protection profile tooAzure by running hello following command:</span></span>

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. <span data-ttu-id="0373f-181">Hämta en skyddsbehållaren genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-181">Get a protection container by running hello following commands:</span></span>

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. <span data-ttu-id="0373f-182">Starta hello associering av hello skyddsbehållaren hello moln:</span><span class="sxs-lookup"><span data-stu-id="0373f-182">Start hello association of hello protection container with hello cloud:</span></span>

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. <span data-ttu-id="0373f-183">När hello jobbet har slutförts, kör du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0373f-183">After hello job has finished, run hello following command:</span></span>

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. <span data-ttu-id="0373f-184">När hello jobbet har bearbetat köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0373f-184">After hello job has finished processing, run hello following command:</span></span>

        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



<span data-ttu-id="0373f-185">toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="0373f-185">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="0373f-186">Steg 8: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="0373f-186">Step 8: Configure network mapping</span></span>
<span data-ttu-id="0373f-187">Innan du börjar nätverksmappning Kontrollera att virtuella datorer på VMM-källservern hello anslutna tooa VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-187">Before you begin network mapping verify that virtual machines on hello source VMM server are connected tooa VM network.</span></span> <span data-ttu-id="0373f-188">Dessutom kan skapa en eller flera virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-188">In addition, create one or more Azure virtual networks.</span></span> <span data-ttu-id="0373f-189">Observera att flera Virtuella datornätverk kan mappas tooa enda Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-189">Note that multiple VM networks can be mapped tooa single Azure network.</span></span>

<span data-ttu-id="0373f-190">Observera att om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="0373f-190">Note that if hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after failover.</span></span> <span data-ttu-id="0373f-191">Om det inte finns en målundernät med ett matchande namn att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-191">If there is not a target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>

<span data-ttu-id="0373f-192">hello första kommandot hämtar servrar för hello aktuella Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="0373f-192">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="0373f-193">hello kommandot lagrar hello Microsoft Azure Site Recovery-servrar i hello $Servers matrisvariabel.</span><span class="sxs-lookup"><span data-stu-id="0373f-193">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

    $Servers = Get-AzureSiteRecoveryServer


<span data-ttu-id="0373f-194">hello andra kommandot hämtar hello site recovery nätverk för hello första servern i hello $Servers matris.</span><span class="sxs-lookup"><span data-stu-id="0373f-194">hello second command gets hello site recovery network for hello first server in hello $Servers array.</span></span> <span data-ttu-id="0373f-195">hello kommandot lagrar hello nätverk i hello $Networks variabeln.</span><span class="sxs-lookup"><span data-stu-id="0373f-195">hello command stores hello networks in hello $Networks variable.</span></span>

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

<span data-ttu-id="0373f-196">hello tredje kommandot hämtar dina Azure-prenumerationer genom att använda hello Get-AzureSubscription cmdlet och sedan lagrar värdet i hello $Subscriptions variabel.</span><span class="sxs-lookup"><span data-stu-id="0373f-196">hello third command gets your Azure subscriptions by using hello Get-AzureSubscription cmdlet, and then stores that value in hello $Subscriptions variable.</span></span>

    $Subscriptions = Get-AzureSubscription



<span data-ttu-id="0373f-197">hello fjärde kommandot hämtar virtuella Azure-nätverk med hjälp av hello Get-AzureVNetSite cmdlet och sedan värdet i hello $AzureVmNetworks variabel.</span><span class="sxs-lookup"><span data-stu-id="0373f-197">hello fourth command gets Azure virtual networks by using hello Get-AzureVNetSite cmdlet, and then that value in hello $AzureVmNetworks variable.</span></span>

    $AzureVmNetworks = Get-AzureVNetSite



<span data-ttu-id="0373f-198">hello slutliga cmdlet skapar en mappning mellan hello primära nätverk och hello Azure virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-198">hello final cmdlet creates a mapping between hello primary network and hello Azure virtual machine network.</span></span> <span data-ttu-id="0373f-199">hello cmdlet anger hello primära nätverket som hello första elementet i $Networks.</span><span class="sxs-lookup"><span data-stu-id="0373f-199">hello cmdlet specifies hello primary network as hello first element of $Networks.</span></span> <span data-ttu-id="0373f-200">hello cmdlet anger ett virtuellt datornätverk som hello första elementet i $AzureVmNetworks med hjälp av ett ID.</span><span class="sxs-lookup"><span data-stu-id="0373f-200">hello cmdlet specifies a virtual machine network as hello first element of $AzureVmNetworks by using its ID.</span></span> <span data-ttu-id="0373f-201">hello-kommandot innehåller din Azure prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="0373f-201">hello command includes your Azure subscription ID.</span></span>

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="0373f-202">Steg 9: Aktivera skydd för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0373f-202">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="0373f-203">Du kan aktivera skydd för virtuella datorer i molnet hello när servrar, moln och nätverk har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="0373f-203">After servers, clouds, and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span> <span data-ttu-id="0373f-204">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-204">Note hello following:</span></span>

<span data-ttu-id="0373f-205">Virtuella datorer måste uppfylla [förutsättningar för Azure virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="0373f-205">Virtual machines must meet [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

<span data-ttu-id="0373f-206">tooenable skydd hello-operativsystem och operativsystem diskegenskaper måste anges för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0373f-206">tooenable protection hello operating system and operating system disk properties must be set for hello virtual machine.</span></span> <span data-ttu-id="0373f-207">När du skapar en virtuell dator i VMM med en mall för virtuell dator kan du ange hello-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0373f-207">When you create a virtual machine in VMM using a virtual machine template you can set hello property.</span></span> <span data-ttu-id="0373f-208">Du kan också ange egenskaperna för befintliga virtuella datorer på hello **allmänna** och **maskinvarukonfiguration** flikar hello egenskaperna för virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0373f-208">You can also set these properties for existing virtual machines on hello **General** and **Hardware Configuration** tabs of hello virtual machine properties.</span></span> <span data-ttu-id="0373f-209">Om du inte anger dessa egenskaper i VMM kommer du att kunna tooconfigure dem i hello Azure Site Recovery-portalen.</span><span class="sxs-lookup"><span data-stu-id="0373f-209">If you don't set these properties in VMM you'll be able tooconfigure them in hello Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="0373f-210">tooenable skydd, kör följande kommando tooget hello skyddsbehållaren hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-210">tooenable protection, run hello following command tooget hello protection container:</span></span>

     <span data-ttu-id="0373f-211">$ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-Name $CloudName</span><span class="sxs-lookup"><span data-stu-id="0373f-211">$ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName</span></span>
2. <span data-ttu-id="0373f-212">Hämta hello skyddad entitet (VM) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-212">Get hello protection entity (VM) by running hello following command:</span></span>

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. <span data-ttu-id="0373f-213">Aktivera hello DR för hello VM genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-213">Enable hello DR for hello VM by running hello following command:</span></span>

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a><span data-ttu-id="0373f-214">Testa distributionen</span><span class="sxs-lookup"><span data-stu-id="0373f-214">Test your deployment</span></span>
<span data-ttu-id="0373f-215">tootest planera distributionen av du kan köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för hello.</span><span class="sxs-lookup"><span data-stu-id="0373f-215">tootest your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for hello plan.</span></span> <span data-ttu-id="0373f-216">Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk.</span><span class="sxs-lookup"><span data-stu-id="0373f-216">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="0373f-217">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="0373f-217">Note that:</span></span>

* <span data-ttu-id="0373f-218">Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello växling vid fel, måste du aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans.</span><span class="sxs-lookup"><span data-stu-id="0373f-218">If you want tooconnect toohello virtual machine in Azure using Remote Desktop after hello failover, enable Remote Desktop Connection on hello virtual machine before you run hello test failover.</span></span>
* <span data-ttu-id="0373f-219">Efter växling vid fel, ska du använda en offentlig IP-adress tooconnect toohello virtuell-dator i Azure med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="0373f-219">After failover, you'll use a public IP address tooconnect toohello virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="0373f-220">Om du vill toodo detta, se till att du inte har några domänprinciper som hindrar dig från anslutande tooa virtuell dator med en offentlig adress.</span><span class="sxs-lookup"><span data-stu-id="0373f-220">If you want toodo this, ensure you don't have any domain policies that prevent you from connecting tooa virtual machine using a public address.</span></span>

<span data-ttu-id="0373f-221">toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="0373f-221">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="create-a-recovery-plan"></a><span data-ttu-id="0373f-222">Skapa en återställningsplan</span><span class="sxs-lookup"><span data-stu-id="0373f-222">Create a recovery plan</span></span>
1. <span data-ttu-id="0373f-223">Skapa en XML-fil som en mall för din återställningsplan med hello data nedan och spara den som ”C:\RPTemplatePath.xml”.</span><span class="sxs-lookup"><span data-stu-id="0373f-223">Create an .xml file as a template for your recovery plan using hello data below, and then save it as "C:\RPTemplatePath.xml".</span></span>
2. <span data-ttu-id="0373f-224">Ändra hello RecoveryPlan nod Id, namn, PrimaryServerId och SecondaryServerId.</span><span class="sxs-lookup"><span data-stu-id="0373f-224">Change hello RecoveryPlan node Id, Name, PrimaryServerId, and SecondaryServerId.</span></span>
3. <span data-ttu-id="0373f-225">Ändra hello ProtectionEntity nod PrimaryProtectionEntityId (vmid från VMM).</span><span class="sxs-lookup"><span data-stu-id="0373f-225">Change hello ProtectionEntity node PrimaryProtectionEntityId (vmid from VMM).</span></span>
4. <span data-ttu-id="0373f-226">Du kan lägga till flera virtuella datorer genom att lägga till fler ProtectionEntity noder.</span><span class="sxs-lookup"><span data-stu-id="0373f-226">You can add more VMs by adding more ProtectionEntity nodes.</span></span>

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. <span data-ttu-id="0373f-227">Fyll i hello data i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="0373f-227">Fill in hello data in hello template:</span></span>

        $TemplatePath = "C:\RPTemplatePath.xml";



1. <span data-ttu-id="0373f-228">Skapa hello RecoveryPlan:</span><span class="sxs-lookup"><span data-stu-id="0373f-228">Create hello RecoveryPlan:</span></span>

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a><span data-ttu-id="0373f-229">Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="0373f-229">Run a test failover</span></span>
1. <span data-ttu-id="0373f-230">Hämta hello RecoveryPlan objekt genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-230">Get hello RecoveryPlan object by running hello following command:</span></span>

     <span data-ttu-id="0373f-231">$RPObject = get-AzureSiteRecoveryRecoveryPlan-Name $RPName;</span><span class="sxs-lookup"><span data-stu-id="0373f-231">$RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;</span></span>
2. <span data-ttu-id="0373f-232">Starta hello testa redundans genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0373f-232">Start hello test failover by running hello following command:</span></span>

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <span data-ttu-id="0373f-233"><a name=monitor></a>Övervakaraktivitet</span><span class="sxs-lookup"><span data-stu-id="0373f-233"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="0373f-234">Använd följande kommandon toomonitor hello aktivitet hello.</span><span class="sxs-lookup"><span data-stu-id="0373f-234">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="0373f-235">Observera att du har toowait mellan jobb för hello bearbetning toofinish.</span><span class="sxs-lookup"><span data-stu-id="0373f-235">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="0373f-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0373f-236">Next steps</span></span>
<span data-ttu-id="0373f-237">[Läs mer](/powershell/azure/overview) om Azure Site Recovery PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0373f-237">[Read more](/powershell/azure/overview) about Azure Site Recovery PowerShell cmdlets.</span></span> <span data-ttu-id="0373f-238"></a>.</span><span class="sxs-lookup"><span data-stu-id="0373f-238"></a>.</span></span>
