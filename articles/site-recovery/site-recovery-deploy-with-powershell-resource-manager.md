---
title: aaaReplicate Hyper-V virtuella datorer med PowerShell och Azure Resource Manager | Microsoft Docs
description: Automatisera hello replikering av Hyper-V VMs tooAzure med Azure Site Recovery med PowerShell och Azure Resource Manager.
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 4fb15ce2e9ad54f1dd6f54ff769eb912aa4b0272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="5894b-103">Replikera mellan lokala Hyper-V virtuella datorer och Azure med hjälp av PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5894b-103">Replicate between on-premises Hyper-V virtual machines and Azure by using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5894b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5894b-104">Azure Portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="5894b-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5894b-105">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
> * [<span data-ttu-id="5894b-106">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="5894b-106">Classic Portal</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a><span data-ttu-id="5894b-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="5894b-107">Overview</span></span>
<span data-ttu-id="5894b-108">Azure Site Recovery bidrar tooyour disaster recovery strategi för affärskontinuitet och genom att samordna replikering, redundans och återställning av virtuella datorer i ett antal distributionsscenarier.</span><span class="sxs-lookup"><span data-stu-id="5894b-108">Azure Site Recovery contributes tooyour business continuity and disaster recovery strategy by orchestrating replication, failover, and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="5894b-109">En fullständig lista över scenarier för distribution finns hello [översikt över Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5894b-109">For a full list of deployment scenarios, see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="5894b-110">Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5894b-110">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="5894b-111">De fungerar med två typer av moduler: hello Azure profil modul eller hello Azure Resource Manager-modul.</span><span class="sxs-lookup"><span data-stu-id="5894b-111">It can work with two types of modules: hello Azure Profile module, or hello Azure Resource Manager module.</span></span>

<span data-ttu-id="5894b-112">Site Recovery PowerShell-cmdletar tillgängliga med Azure PowerShell för Azure Resource Manager hjälpa dig att skydda och återställa dina servrar i Azure.</span><span class="sxs-lookup"><span data-stu-id="5894b-112">Site Recovery PowerShell cmdlets, available with Azure PowerShell for Azure Resource Manager, help you protect and recover your servers in Azure.</span></span>

<span data-ttu-id="5894b-113">Den här artikeln beskriver hur toouse Windows PowerShell, tillsammans med Azure Resource Manager, toodeploy Site Recovery tooconfigure och samordna server protection tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5894b-113">This article describes how toouse Windows PowerShell, together with Azure Resource Manager, toodeploy Site Recovery tooconfigure and orchestrate server protection tooAzure.</span></span> <span data-ttu-id="5894b-114">hello-exempel som används i den här artikeln visar hur tooprotect, växla över och återställa virtuella datorer på en värd tooAzure Hyper-V med hjälp av Azure PowerShell med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5894b-114">hello example used in this article shows you how tooprotect, fail over, and recover virtual machines on a Hyper-V host tooAzure, by using Azure PowerShell with Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="5894b-115">hello Site Recovery PowerShell-cmdlets för närvarande kan du tooconfigure hello följande: en tooanother för Virtual Machine Manager-plats, tooAzure en Virtual Machine Manager-plats och tooAzure en Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="5894b-115">hello Site Recovery PowerShell cmdlets currently allow you tooconfigure hello following: one Virtual Machine Manager site tooanother, a Virtual Machine Manager site tooAzure, and a Hyper-V site tooAzure.</span></span>
>
>

<span data-ttu-id="5894b-116">Du behöver inte toobe en expert toouse PowerShell den här artikeln, men du behöver toounderstand hello grundläggande begrepp som moduler, cmdletar och sessioner.</span><span class="sxs-lookup"><span data-stu-id="5894b-116">You don't need toobe a PowerShell expert toouse this article, but you do need toounderstand hello basic concepts, such as modules, cmdlets, and sessions.</span></span> <span data-ttu-id="5894b-117">Mer information om Windows PowerShell finns i [Komma igång med Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span><span class="sxs-lookup"><span data-stu-id="5894b-117">For more information about Windows PowerShell, see [Getting Started with Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span></span>

<span data-ttu-id="5894b-118">Du kan också läsa mer om [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="5894b-118">You can also read more about [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5894b-119">Microsoft-partners som är en del av hello Cloud Solution Providers (CSP) program kan konfigurera och hantera skydd av sina kunders servrar tootheir kunder respektive CSP prenumerationer (klient subscriptions).</span><span class="sxs-lookup"><span data-stu-id="5894b-119">Microsoft partners that are part of hello Cloud Solution Provider (CSP) program can configure and manage protection of their customers' servers tootheir customers' respective CSP subscriptions (tenant subscriptions).</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="5894b-120">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5894b-120">Before you start</span></span>
<span data-ttu-id="5894b-121">Kontrollera att du har dessa krav är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="5894b-121">Make sure you have these prerequisites in place:</span></span>

* <span data-ttu-id="5894b-122">En [Microsoft Azure](https://azure.microsoft.com/) konto.</span><span class="sxs-lookup"><span data-stu-id="5894b-122">A [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="5894b-123">Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5894b-123">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="5894b-124">Dessutom kan du läsa om [priser för Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="5894b-124">In addition, you can read about [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="5894b-125">Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="5894b-125">Azure PowerShell 1.0.</span></span> <span data-ttu-id="5894b-126">Information om den här versionen och hur tooinstall, se [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="5894b-126">For information about this release and how tooinstall it, see [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span></span>
* <span data-ttu-id="5894b-127">Hej [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) och [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduler.</span><span class="sxs-lookup"><span data-stu-id="5894b-127">hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) and [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules.</span></span> <span data-ttu-id="5894b-128">Du kan hämta hello senaste versionerna av dessa moduler från hello [PowerShell-galleriet](https://www.powershellgallery.com/)</span><span class="sxs-lookup"><span data-stu-id="5894b-128">You can get hello latest versions of these modules from hello [PowerShell gallery](https://www.powershellgallery.com/)</span></span>

<span data-ttu-id="5894b-129">Den här artikeln beskrivs hur toouse Azure Powershell med Azure Resource Manager tooconfigure och hantera skydd för dina servrar.</span><span class="sxs-lookup"><span data-stu-id="5894b-129">This article illustrates how toouse Azure Powershell with Azure Resource Manager tooconfigure and manage protection of your servers.</span></span> <span data-ttu-id="5894b-130">hello-exempel som används i den här artikeln beskrivs hur du tooprotect en virtuell dator som körs på en Hyper-V-värd, tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5894b-130">hello example used in this article shows you how tooprotect a virtual machine, running on a Hyper-V host, tooAzure.</span></span> <span data-ttu-id="5894b-131">hello följande förutsättningar är specifika toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="5894b-131">hello following prerequisites are specific toothis example.</span></span> <span data-ttu-id="5894b-132">En mer omfattande uppsättning krav för hello finns olika Site Recovery-scenarier toohello dokumentation som rör toothat scenario.</span><span class="sxs-lookup"><span data-stu-id="5894b-132">For a more comprehensive set of requirements for hello various Site Recovery scenarios, refer toohello documentation pertaining toothat scenario.</span></span>

* <span data-ttu-id="5894b-133">En Hyper-V-värd som kör Windows Server 2012 R2 eller Microsoft Hyper-V Server 2012 R2 som innehåller en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5894b-133">A Hyper-V host running Windows Server 2012 R2 or Microsoft Hyper-V Server 2012 R2 containing one or more virtual machines.</span></span>
* <span data-ttu-id="5894b-134">Hyper-V-servrar ansluten toohello Internet, antingen direkt eller via en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="5894b-134">Hyper-V servers connected toohello Internet, either directly or through a proxy.</span></span>
* <span data-ttu-id="5894b-135">hello virtuella datorer som du vill tooprotect måste uppfylla [förutsättningar för virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="5894b-135">hello virtual machines you want tooprotect should conform with [Virtual Machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

## <a name="step-1-sign-in-tooyour-azure-account"></a><span data-ttu-id="5894b-136">Steg 1: Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="5894b-136">Step 1: Sign in tooyour Azure account</span></span>
1. <span data-ttu-id="5894b-137">Öppna ett PowerShell-konsolen och kör det här kommandot toosign tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5894b-137">Open a PowerShell console and run this command toosign in tooyour Azure account.</span></span> <span data-ttu-id="5894b-138">hello cmdlet öppnar en webbsida som uppmanas du att autentiseringsuppgifterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="5894b-138">hello cmdlet brings up a web page that will prompt you for your account credentials.</span></span>

        Login-AzureRmAccount

    <span data-ttu-id="5894b-139">Du kan alternativt kan också inkludera autentiseringsuppgifterna för ditt konto som en parameter toohello `Login-AzureRmAccount` cmdlet, med hjälp av hello `-Credential` parameter.</span><span class="sxs-lookup"><span data-stu-id="5894b-139">Alternately, you could also include your account credentials as a parameter toohello `Login-AzureRmAccount` cmdlet, by using hello `-Credential` parameter.</span></span>

    <span data-ttu-id="5894b-140">Om du är CSP-partner som arbetar för en klient måste du ange hello kund som en klient med hjälp av deras primära domännamn tenantID eller klienten.</span><span class="sxs-lookup"><span data-stu-id="5894b-140">If you are CSP partner working on behalf of a tenant, specify hello customer as a tenant, by using their tenantID or tenant primary domain name.</span></span>

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. <span data-ttu-id="5894b-141">Ett konto kan ha flera prenumerationer, så du bör koppla hello-prenumeration som du vill toouse med hello-konto.</span><span class="sxs-lookup"><span data-stu-id="5894b-141">An account can have several subscriptions, so you should associate hello subscription you want toouse with hello account.</span></span>

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. <span data-ttu-id="5894b-142">Kontrollera att prenumerationen är registrerad toouse hello Azure providers för återställningstjänster och Site Recovery med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="5894b-142">Verify that your subscription is registered toouse hello Azure providers for Recovery Services and Site Recovery, by using hello following commands:</span></span>

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   <span data-ttu-id="5894b-143">I hello utdata från de här kommandona om hello **RegistrationState** har angetts för**registrerade**, kan du fortsätta tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="5894b-143">In hello output of these commands, if hello **RegistrationState** is set too**Registered**, you can proceed tooStep 2.</span></span> <span data-ttu-id="5894b-144">Om så inte är fallet, bör du registrera hello saknas provider i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5894b-144">If not, you should register hello missing provider in your subscription.</span></span>

   <span data-ttu-id="5894b-145">tooregister hello Azure provider för Site Recovery och Recovery Services genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="5894b-145">tooregister hello Azure provider for Site Recovery and Recovery Services, run hello following commands:</span></span>

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   <span data-ttu-id="5894b-146">Kontrollera att hello providers har registrerats med hjälp av följande kommandon hello: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` och `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span><span class="sxs-lookup"><span data-stu-id="5894b-146">Verify that hello providers registered successfully by using hello following commands: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` and `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span></span>

## <a name="step-2-set-up-hello-recovery-services-vault"></a><span data-ttu-id="5894b-147">Steg 2: Konfigurera hello Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="5894b-147">Step 2: Set up hello Recovery Services vault</span></span>
1. <span data-ttu-id="5894b-148">Skapa en resursgrupp i Azure Resource Manager där du ska skapa hello valvet, eller Använd en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5894b-148">Create an Azure Resource Manager resource group in which you'll create hello vault, or use an existing resource group.</span></span> <span data-ttu-id="5894b-149">Du kan skapa en ny resursgrupp med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5894b-149">You can create a new resource group by using hello following command:</span></span>

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    <span data-ttu-id="5894b-150">hello $ResourceGroupName variabeln innehåller hello namnet på resursgruppen hello önskade toocreate och hello $Geo variabeln innehåller hello Azure region i vilken toocreate hello resursgrupp (till exempel ”södra”).</span><span class="sxs-lookup"><span data-stu-id="5894b-150">where hello $ResourceGroupName variable contains hello name of hello resource group you want toocreate, and hello $Geo variable contains hello Azure region in which toocreate hello resource group (for example, "Brazil South").</span></span>

    <span data-ttu-id="5894b-151">Du kan hämta en lista över resursgrupper i din prenumeration med hjälp av hello `Get-AzureRmResourceGroup` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5894b-151">You can obtain a list of resource groups in your subscription by using hello `Get-AzureRmResourceGroup` cmdlet.</span></span>
2. <span data-ttu-id="5894b-152">Skapa ett nytt Azure Recovery Services-valv enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5894b-152">Create a new Azure Recovery Services vault as follows:</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    <span data-ttu-id="5894b-153">Du kan hämta en lista över befintliga valv med hello `Get-AzureRmRecoveryServicesVault` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5894b-153">You can retrieve a list of existing vaults by using hello `Get-AzureRmRecoveryServicesVault` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="5894b-154">Om du vill tooperform åtgärder på Site Recovery-valv som skapats med hjälp av hello klassiska portalen eller hello Azure Service Management PowerShell-modulen kan du hämta en lista över sådana valv med hello `Get-AzureRmSiteRecoveryVault` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5894b-154">If you wish tooperform operations on Site Recovery vaults created using hello classic portal or hello Azure Service Management PowerShell module, you can retrieve a list of such vaults by using hello `Get-AzureRmSiteRecoveryVault` cmdlet.</span></span> <span data-ttu-id="5894b-155">Du bör skapa ett nytt Recovery Services-valv för alla nya åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5894b-155">You should create a new Recovery Services vault for all new operations.</span></span> <span data-ttu-id="5894b-156">hello Site Recovery-valv du har skapat tidigare stöds, men har inte hello senaste funktionerna.</span><span class="sxs-lookup"><span data-stu-id="5894b-156">hello Site Recovery vaults you've created earlier are supported, but don't have hello latest features.</span></span>
>
>

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="5894b-157">Steg 3: Ange hello Recovery Services-valvet kontext</span><span class="sxs-lookup"><span data-stu-id="5894b-157">Step 3: Set hello Recovery Services vault context</span></span>
1. <span data-ttu-id="5894b-158">Ange hello valvet kontexten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="5894b-158">Set hello vault context by running hello following command:</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-hello-site"></a><span data-ttu-id="5894b-159">Steg 4: Skapa ett Hyper-V-platsen och generera en ny registreringsnyckel i valvet för hello plats.</span><span class="sxs-lookup"><span data-stu-id="5894b-159">Step 4: Create a Hyper-V site and generate a new vault registration key for hello site.</span></span>
1. <span data-ttu-id="5894b-160">Skapa en ny Hyper-V-plats på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5894b-160">Create a new Hyper-V site as follows:</span></span>

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    <span data-ttu-id="5894b-161">Denna cmdlet startar en Platsåterställning jobbet toocreate hello plats och returnerar ett jobbobjekt för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="5894b-161">This cmdlet starts a Site Recovery job toocreate hello site, and returns a Site Recovery job object.</span></span> <span data-ttu-id="5894b-162">Vänta tills hello jobbet toocomplete och kontrollera hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="5894b-162">Wait for hello job toocomplete and verify that hello job completed successfully.</span></span>

    <span data-ttu-id="5894b-163">Du kan hämta hello jobbobjektet och därmed kontrollera hello hello jobbets aktuella status genom att använda hello Get-AzureRmSiteRecoveryJob cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5894b-163">You can retrieve hello job object, and thereby check hello current status of hello job, by using hello Get-AzureRmSiteRecoveryJob cmdlet.</span></span>
2. <span data-ttu-id="5894b-164">Generera och ladda ned en registreringsnyckel för hello plats på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5894b-164">Generate and download a registration key for hello site, as follows:</span></span>

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    <span data-ttu-id="5894b-165">Kopiera hello ned viktiga toohello Hyper-V-värden.</span><span class="sxs-lookup"><span data-stu-id="5894b-165">Copy hello downloaded key toohello Hyper-V host.</span></span> <span data-ttu-id="5894b-166">Du behöver hello viktiga tooregister hello Hyper-V-värd toohello plats.</span><span class="sxs-lookup"><span data-stu-id="5894b-166">You need hello key tooregister hello Hyper-V host toohello site.</span></span>

## <a name="step-5-install-hello-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a><span data-ttu-id="5894b-167">Steg 5: Installera hello Azure Site Recovery-providern och Azure Recovery Services-agenten på Hyper-V-värd</span><span class="sxs-lookup"><span data-stu-id="5894b-167">Step 5: Install hello Azure Site Recovery provider and Azure Recovery Services Agent on your Hyper-V host</span></span>
1. <span data-ttu-id="5894b-168">Hämta hello installationsprogrammet för hello senaste versionen av hello providern från [Microsoft](https://aka.ms/downloaddra).</span><span class="sxs-lookup"><span data-stu-id="5894b-168">Download hello installer for hello latest version of hello provider from [Microsoft](https://aka.ms/downloaddra).</span></span>
2. <span data-ttu-id="5894b-169">Kör hello installer på Hyper-V-värd och hello slutet av hello installationen fortsätta toohello registreringssteget.</span><span class="sxs-lookup"><span data-stu-id="5894b-169">Run hello installer on your Hyper-V host, and at hello end of hello installation continue toohello registration step.</span></span>
3. <span data-ttu-id="5894b-170">När du uppmanas ange hello hämtat plats registreringsnyckel och slutföra registreringen av hello Hyper-V-värd toohello plats.</span><span class="sxs-lookup"><span data-stu-id="5894b-170">When prompted, provide hello downloaded site registration key, and complete registration of hello Hyper-V host toohello site.</span></span>
4. <span data-ttu-id="5894b-171">Kontrollera att hello Hyper-V-värd är registrerade toohello plats med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5894b-171">Verify that hello Hyper-V host is registered toohello site by using hello following command:</span></span>

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-hello-protection-container"></a><span data-ttu-id="5894b-172">Steg 6: Skapa en replikeringsprincip och koppla den till hello skyddsbehållaren</span><span class="sxs-lookup"><span data-stu-id="5894b-172">Step 6: Create a replication policy and associate it with hello protection container</span></span>
1. <span data-ttu-id="5894b-173">Skapa en replikeringsprincip enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5894b-173">Create a replication policy as follows:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    <span data-ttu-id="5894b-174">Kontrollera hello returnerade jobbet tooensure som hello replikering princip skapandet lyckas.</span><span class="sxs-lookup"><span data-stu-id="5894b-174">Check hello returned job tooensure that hello replication policy creation succeeds.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="5894b-175">hello storage-konto som angetts ska vara i hello samma Azure-region som Recovery Services-valvet och ska ha geo-replikering aktiverat.</span><span class="sxs-lookup"><span data-stu-id="5894b-175">hello storage account specified should be in hello same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="5894b-176">Om hello anges Recovery storage-konto är av typen Azure Storage (klassisk) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (klassisk).</span><span class="sxs-lookup"><span data-stu-id="5894b-176">If hello specified Recovery storage account is of type Azure Storage (Classic), failover of hello protected machines recover hello machine tooAzure IaaS (Classic).</span></span>
   > * <span data-ttu-id="5894b-177">Om hello anges Recovery storage-konto är av typen Azure Storage (Azure Resource Manager) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="5894b-177">If hello specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of hello protected machines recover hello machine tooAzure IaaS (Azure Resource Manager).</span></span>
   >
   >
2. <span data-ttu-id="5894b-178">Hämta hello skydd behållaren motsvarande toohello plats, på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5894b-178">Get hello protection container corresponding toohello site, as follows:</span></span>

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. <span data-ttu-id="5894b-179">Starta hello associering av hello skyddsbehållaren med hello replikeringsprincip på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5894b-179">Start hello association of hello protection container with hello replication policy, as follows:</span></span>

     <span data-ttu-id="5894b-180">$Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-princip $Policy - PrimaryProtectionContainer $protectionContainer</span><span class="sxs-lookup"><span data-stu-id="5894b-180">$Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer</span></span>

   <span data-ttu-id="5894b-181">Vänta tills hello association jobbet toocomplete och se till att den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="5894b-181">Wait for hello association job toocomplete, and ensure that it completed successfully.</span></span>

## <a name="step-7-enable-protection-for-virtual-machines"></a><span data-ttu-id="5894b-182">Steg 7: Aktivera skydd för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5894b-182">Step 7: Enable protection for virtual machines</span></span>
1. <span data-ttu-id="5894b-183">Hämta hello skydd entitet motsvarande toohello VM som du vill tooprotect, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5894b-183">Get hello protection entity corresponding toohello VM you want tooprotect, as follows:</span></span>

        $VMFriendlyName = "Fabrikam-app"                    #Name of hello VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. <span data-ttu-id="5894b-184">Börja skydda hello virtuell dator på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5894b-184">Start protecting hello virtual machine, as follows:</span></span>

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > <span data-ttu-id="5894b-185">hello storage-konto som angetts ska vara i hello samma Azure-region som Recovery Services-valvet och ska ha geo-replikering aktiverat.</span><span class="sxs-lookup"><span data-stu-id="5894b-185">hello storage account specified should be in hello same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="5894b-186">Om hello anges Recovery storage-konto är av typen Azure Storage (klassisk) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (klassisk).</span><span class="sxs-lookup"><span data-stu-id="5894b-186">If hello specified Recovery storage account is of type Azure Storage (Classic), failover of hello protected machines recover hello machine tooAzure IaaS (Classic).</span></span>
   > * <span data-ttu-id="5894b-187">Om hello anges Recovery storage-konto är av typen Azure Storage (Azure Resource Manager) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="5894b-187">If hello specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of hello protected machines recover hello machine tooAzure IaaS (Azure Resource Manager).</span></span>
   >
   > <span data-ttu-id="5894b-188">Om hello VM som du skyddar har fler än en disk ansluten tooit, ange hello operativsystemdisk med hello *OSDiskName* parameter.</span><span class="sxs-lookup"><span data-stu-id="5894b-188">If hello VM you are protecting has more than one disk attached tooit, specify hello operating system disk by using hello *OSDiskName* parameter.</span></span>
   >
   >
3. <span data-ttu-id="5894b-189">Vänta tills hello virtuella datorer tooreach ett skyddat läge efter hello inledande replikering.</span><span class="sxs-lookup"><span data-stu-id="5894b-189">Wait for hello virtual machines tooreach a protected state after hello initial replication.</span></span> <span data-ttu-id="5894b-190">Detta kan ta en stund, beroende på faktorer som hello mängden data toobe replikeras och hello överordnade bandbredd tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5894b-190">This can take a while, depending on factors such as hello amount of data toobe replicated and hello available upstream bandwidth tooAzure.</span></span> <span data-ttu-id="5894b-191">hello jobbstatus och StateDescription uppdateras enligt följande vid hello VM når ett skyddat läge.</span><span class="sxs-lookup"><span data-stu-id="5894b-191">hello job State and StateDescription are updated as follows, upon hello VM reaching a protected state.</span></span>

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. <span data-ttu-id="5894b-192">Uppdatera egenskaperna för återställning, till exempel hello Virtuella rollstorlek och hello Azure-nätverk tooattach hello virtuella datorns nätverk gränssnittet kort tooupon redundans.</span><span class="sxs-lookup"><span data-stu-id="5894b-192">Update recovery properties, such as hello VM role size, and hello Azure network tooattach hello virtual machine's network interface cards tooupon failover.</span></span>

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update hello virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a><span data-ttu-id="5894b-193">Steg 8: Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="5894b-193">Step 8: Run a test failover</span></span>
1. <span data-ttu-id="5894b-194">Kör ett testjobb växling vid fel, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5894b-194">Run a test failover job, as follows:</span></span>

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. <span data-ttu-id="5894b-195">Kontrollera att hello testa virtuell dator skapas i Azure.</span><span class="sxs-lookup"><span data-stu-id="5894b-195">Verify that hello test VM is created in Azure.</span></span> <span data-ttu-id="5894b-196">(hello testa redundans jobb pausas, när du har skapat hello test VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="5894b-196">(hello test failover job is suspended, after creating hello test VM in Azure.</span></span> <span data-ttu-id="5894b-197">hello jobbet har slutförts genom att rensa hello som skapats av då du återupptar hello jobb, enligt beskrivningen i hello nästa steg.)</span><span class="sxs-lookup"><span data-stu-id="5894b-197">hello job completes by cleaning up hello created artefacts upon resuming hello job, as illustrated in hello next step.)</span></span>
3. <span data-ttu-id="5894b-198">Fullständig hello testa redundans, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5894b-198">Complete hello test failover, as follows:</span></span>

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a><span data-ttu-id="5894b-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5894b-199">Next Steps</span></span>
<span data-ttu-id="5894b-200">[Läs mer](https://msdn.microsoft.com/library/azure/mt637930.aspx) om Azure Site Recovery med Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="5894b-200">[Read more](https://msdn.microsoft.com/library/azure/mt637930.aspx) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
