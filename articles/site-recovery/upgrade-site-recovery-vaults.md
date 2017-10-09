---
title: aaaUpgrade Site Recovery-valvet tooan Azure Recovery Services-valvet
description: "Lär dig hur tooupgrade ett Azure Site Recovery-valv tooa återställningstjänster valvet"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: a18a923ee3bad91873e654c9b9b5bf8b83acc123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-site-recovery-vault-tooan-azure-resource-manager-based-recovery-services-vault"></a><span data-ttu-id="e4c7c-103">Uppgradera en Site Recovery-valvet tooan Azure Resource Manager-baserade Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="e4c7c-103">Upgrade a Site Recovery vault tooan Azure Resource Manager-based Recovery Services vault</span></span>

<span data-ttu-id="e4c7c-104">Den här artikeln beskriver hur tooupgrade Azure Site Recovery-valv tooAzure Resource Manager-baserade återställningstjänsten valv utan någon inverkan på pågående replikering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-104">This article describes how tooupgrade Azure Site Recovery vaults tooAzure Resource Manager-based Recovery Service vaults without any impact on ongoing replication.</span></span> <span data-ttu-id="e4c7c-105">Mer information om Azure Resource Manager-funktioner och fördelar finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-105">For more information about Azure Resource Manager features and benefits, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="e4c7c-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="e4c7c-106">Introduction</span></span>
<span data-ttu-id="e4c7c-107">Recovery Services-valvet är en Azure Resource Manager-resurs för att hantera säkerhetskopiering och katastrofåterställning recovery internt i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-107">A Recovery Services vault is an Azure Resource Manager resource for managing backup and disaster recovery natively in hello cloud.</span></span> <span data-ttu-id="e4c7c-108">Det är en enhetlig valvet som du kan använda i hello nya Azure-portalen och den ersätter hello klassiska säkerhetskopiering och Site Recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-108">It is a unified vault that you can use in hello new Azure portal, and it replaces hello classic backup and Site Recovery vaults.</span></span>

<span data-ttu-id="e4c7c-109">Recovery Services-valv erbjuder en matris med-funktioner, inklusive:</span><span class="sxs-lookup"><span data-stu-id="e4c7c-109">Recovery Services vaults offer an array of features, including:</span></span>

* <span data-ttu-id="e4c7c-110">Azure Resource Manager-stöd: du kan skydda och växla över dina virtuella datorer och fysiska datorer till en Azure Resource Manager hög.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-110">Azure Resource Manager support: You can protect and fail over your virtual machines and physical machines into an Azure Resource Manager stack.</span></span>

* <span data-ttu-id="e4c7c-111">Utesluta disk: Om du har temporära filer eller hög omsättning data som du inte vill toouse alla bandbredd för, kan du undanta volymer från replikering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-111">Exclude disk: If you have temp files or high churn data that you don’t want toouse all your bandwidth for, you can exclude volumes from replication.</span></span> <span data-ttu-id="e4c7c-112">Den här funktionen har aktiverats för närvarande i *VMware tooAzure* och *Hyper-V tooAzure* och utökas tooother-scenarier.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-112">This capability has been enabled currently in *VMware tooAzure* and *Hyper-V tooAzure* and is extended tooother scenarios as well.</span></span>

* <span data-ttu-id="e4c7c-113">Stöd för premium och lokalt redundant lagring: nu kan du skydda servrar i premium-lagring konton som låter kunder tooprotect program med högre i/o-åtgärder per sekund (IOPS).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-113">Support for premium and locally redundant storage: You can now protect servers in premium storage accounts that allow customers tooprotect applications with higher input/output operations per second (IOPS).</span></span> <span data-ttu-id="e4c7c-114">Den här funktionen är aktiverat i *VMware tooAzure*.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-114">This capability is currently enabled in *VMware tooAzure*.</span></span>

* <span data-ttu-id="e4c7c-115">Effektiv Kom igång-upplevelse: hello förbättrad Kom igång-upplevelse har utformats toomake katastrofåterställning installationsprogrammet enkelt.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-115">Streamlined getting-started experience: hello enhanced getting-started experience has been designed toomake disaster-recovery setup easy.</span></span>

* <span data-ttu-id="e4c7c-116">Säkerhetskopiering och Site Recovery-hantering från hello samma valvet: du kan nu skydda servrar för katastrofåterställning eller utför säkerhetskopiering från hello samma valvet, som kan försämras avsevärt minska din hantering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-116">Backup and Site Recovery management from hello same vault: You can now protect servers for disaster recovery or perform backup from hello same vault, which can reduce your management overhead significantly.</span></span>

<span data-ttu-id="e4c7c-117">Mer information om hello uppgraderas upplevelse och funktioner finns hello [lagring, säkerhetskopiering och återställning blogg](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-117">For more information about hello upgraded experience and features, see hello [Storage, Backup, and Recovery blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span></span>

## <a name="salient-features"></a><span data-ttu-id="e4c7c-118">Viktigaste funktioner</span><span class="sxs-lookup"><span data-stu-id="e4c7c-118">Salient features</span></span>

* <span data-ttu-id="e4c7c-119">Ingen inverkan på pågående replikering: pågående replikeringar fortsätta utan avbrott under och efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-119">No impact on ongoing replication: Ongoing replications continue without any interruption during and post upgrade.</span></span>

* <span data-ttu-id="e4c7c-120">Utan extra kostnad: hämta en uppsättning uppdaterade funktioner utan extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-120">No additional cost: Get an entire set of updated capabilities at no additional cost.</span></span>

* <span data-ttu-id="e4c7c-121">Ingen dataförlust: eftersom den här processen är inte en migrering och uppgradering, inställningar och befintliga återställningspunkter för replikeringen bevaras under och efter hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-121">No data loss: Because this process is an upgrade and not a migration, existing replication recovery points and settings remain intact during and after hello upgrade.</span></span>


## <a name="what-happens-during-hello-vault-upgrade"></a><span data-ttu-id="e4c7c-122">Vad händer under hello valvet uppgraderingen?</span><span class="sxs-lookup"><span data-stu-id="e4c7c-122">What happens during hello vault upgrade?</span></span>

<span data-ttu-id="e4c7c-123">Du kan inte utföra åtgärder som registrerar en ny server eller att aktivera replikering för en virtuell dator (VM) under hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-123">During hello upgrade, you cannot perform operations such as registering a new server or enabling replication for a virtual machine (VM).</span></span> <span data-ttu-id="e4c7c-124">Åtgärder som innefattar läsning av data från eller skriva data toohello valvet, till exempel pågående replikering av skyddade objekt toohello valvet, fortsätta utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-124">Operations that involve reading data from or writing data toohello vault, such as ongoing replication of protected items toohello vault, continue uninterrupted.</span></span>

### <a name="changes-tooautomation-and-tooling-after-hello-upgrade"></a><span data-ttu-id="e4c7c-125">Ändrar tooautomation och verktygsuppsättning efter hello uppgradering</span><span class="sxs-lookup"><span data-stu-id="e4c7c-125">Changes tooautomation and tooling after hello upgrade</span></span>
<span data-ttu-id="e4c7c-126">När du uppgraderar hello valvet typ från hello klassiska modellen toohello Resource Manager distribution distributionsmodell uppdatera hello befintlig automation eller verktygsuppsättning tooensure den fortsätter toowork efter hello uppgradering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-126">As you upgrade hello vault type from hello classic deployment model toohello Resource Manager deployment model, update hello existing automation or tooling tooensure that it continues toowork after hello upgrade.</span></span>

### <a name="prepare-your-environment-for-hello-upgrade"></a><span data-ttu-id="e4c7c-127">Förbereda miljön för hello uppgradering</span><span class="sxs-lookup"><span data-stu-id="e4c7c-127">Prepare your environment for hello upgrade</span></span>

* [<span data-ttu-id="e4c7c-128">Installera PowerShell eller uppgradera den tooversion 5 eller senare</span><span class="sxs-lookup"><span data-stu-id="e4c7c-128">Install PowerShell or upgrade it tooversion 5 or later</span></span>](https://www.microsoft.com/download/details.aspx?id=50395)
* [<span data-ttu-id="e4c7c-129">Installera hello senaste versionen av Azure PowerShell MSI</span><span class="sxs-lookup"><span data-stu-id="e4c7c-129">Install hello latest version of Azure PowerShell MSI</span></span>](https://github.com/Azure/azure-powershell/releases)
* [<span data-ttu-id="e4c7c-130">Hämta uppgraderingsskript för hello Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="e4c7c-130">Download hello Recovery Services vault upgrade script</span></span>](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a><span data-ttu-id="e4c7c-131">Krav</span><span class="sxs-lookup"><span data-stu-id="e4c7c-131">Prerequisites</span></span>
<span data-ttu-id="e4c7c-132">tooupgrade från Site Recovery-valv tooAzure Resource Manager-baserade återställningstjänsten valv, hello följande krav måste uppfyllas:</span><span class="sxs-lookup"><span data-stu-id="e4c7c-132">tooupgrade from Site Recovery vaults tooAzure Resource Manager-based Recovery Service vaults, you must meet hello following requirements:</span></span>

* <span data-ttu-id="e4c7c-133">Minsta agent-version: hello-versionen av Azure Site Recovery-Provider installerad på servern måste vara 5.1.1700.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-133">Minimum agent version: hello version of Azure Site Recovery Provider installed on your server must be 5.1.1700.0 or later.</span></span>

* <span data-ttu-id="e4c7c-134">Konfigurationer som stöds: du kan inte konfigurera ditt valv med lagringsnätverk (SAN) eller SQL Server AlwaysOn-Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-134">Supported configuration: You cannot configure your vault with storage area network (SAN) or SQL Server AlwaysOn Availability Groups.</span></span> <span data-ttu-id="e4c7c-135">Alla andra konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-135">All other configurations are supported.</span></span>

    >[!NOTE]
    ><span data-ttu-id="e4c7c-136">Du kan hantera lagring mappning endast via PowerShell efter hello uppgradering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-136">After hello upgrade, you can manage storage mapping only via PowerShell.</span></span>

* <span data-ttu-id="e4c7c-137">Scenario för distribution som stöds: ditt valv får inte vara hello *VMware tooAzure* äldre distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-137">Supported deployment scenario: Your vault shouldn’t be hello *VMware tooAzure* legacy deployment model.</span></span> <span data-ttu-id="e4c7c-138">Innan du fortsätter bör du först flytta toohello förbättrad distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-138">Before you proceed, first move toohello enhanced deployment model.</span></span>

* <span data-ttu-id="e4c7c-139">Inga aktiva användarinitierad jobb som rör hantering plan operations: eftersom åtkomst toohello management plan begränsas under uppgraderingen, Slutför alla din plan hanteringsåtgärder innan du utlösa hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-139">No active user-initiated jobs that involve management plane operations: Because access toohello management plane is restricted during upgrade, complete all your management plane actions before you trigger hello upgrade.</span></span> <span data-ttu-id="e4c7c-140">Den här processen innehåller pågående replikering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-140">This process doesn’t include ongoing replication.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="e4c7c-141">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="e4c7c-141">Frequently asked questions</span></span>

<span data-ttu-id="e4c7c-142">**Påverkar den här uppgraderingen min pågående replikering?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-142">**Does this upgrade affect my ongoing replication?**</span></span>

<span data-ttu-id="e4c7c-143">Nej.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-143">No.</span></span> <span data-ttu-id="e4c7c-144">Din pågående replikering fortsätter utan avbrott under och efter hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-144">Your ongoing replication continues uninterrupted during and after hello upgrade.</span></span>

<span data-ttu-id="e4c7c-145">**Vad händer toonetwork inställningar, till exempel plats-till-plats VPN och IP-inställningar?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-145">**What happens toonetwork settings such as site-to-site VPN and IP settings?**</span></span>

<span data-ttu-id="e4c7c-146">hello uppgraderingen påverkar inte hello nätverksinställningar.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-146">hello upgrade doesn't affect hello network settings.</span></span> <span data-ttu-id="e4c7c-147">Alla Azure till lokala anslutningar påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-147">All Azure-to-on-premises connections remain intact.</span></span>

<span data-ttu-id="e4c7c-148">**Vad händer toomy valv om du inte planerar tooupgrade i hello nära framtiden?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-148">**What happens toomy vaults if I don’t plan tooupgrade in hello near future?**</span></span>

<span data-ttu-id="e4c7c-149">Stöd för Site Recovery-valvet i hello gamla Azure-portalen att bli inaktuell från September 2017.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-149">Support for Site Recovery vault in hello old Azure portal will be deprecated starting September 2017.</span></span> <span data-ttu-id="e4c7c-150">Vi rekommenderar starkt att du använder hello uppgraderingsfunktionen toomove toohello nya portalen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-150">We strongly recommend that you use hello upgrade feature toomove toohello new portal.</span></span>

<span data-ttu-id="e4c7c-151">**Vad innebär det här migreringsplan för Mina befintliga verktygsuppsättning?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-151">**What does this migration plan mean for my existing tooling?**</span></span>  

<span data-ttu-id="e4c7c-152">Uppdatera din verktygsuppsättning toohello Resource Manager-distributionsmodellen är en av hello viktigaste ändringarna som du måste ta hänsyn till i din uppgradering planer.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-152">Updating your tooling toohello Resource Manager deployment model is one of hello most important changes that you must account for in your upgrade plans.</span></span> <span data-ttu-id="e4c7c-153">hello Recovery Services-valv baseras på hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-153">hello Recovery Services vaults are based on hello Resource Manager deployment model.</span></span> 

<span data-ttu-id="e4c7c-154">**Hur länge hello management-plan driftstopp senast?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-154">**How long does hello management-plane downtime last?**</span></span>

<span data-ttu-id="e4c7c-155">hello uppgraderingen tar normalt ungefär 15 minuter för too30 och det kan ta upp tooa maximalt en timme.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-155">hello upgrade ordinarily takes about 15 too30 minutes, and it could take up tooa maximum of one hour.</span></span>

<span data-ttu-id="e4c7c-156">**Kan jag återställa efter uppgraderingen?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-156">**Can I roll back after upgrading?**</span></span>

<span data-ttu-id="e4c7c-157">Nej.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-157">No.</span></span> <span data-ttu-id="e4c7c-158">Återställning stöds inte när hello resurser har uppgraderats.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-158">Rollback is not supported after hello resources have been successfully upgraded.</span></span>

<span data-ttu-id="e4c7c-159">**Kan jag verifiera min prenumeration eller resurser toosee om de kan uppgraderas?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-159">**Can I validate my subscription or resources toosee whether they can be upgraded?**</span></span>

<span data-ttu-id="e4c7c-160">Ja.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-160">Yes.</span></span> <span data-ttu-id="e4c7c-161">Hello första steget i hello uppgraderingen är i hello stöds av plattformen uppgraderingsalternativet, toovalidate att hello resurser är utföra en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-161">In hello platform-supported upgrade option, hello first step in hello upgrade is toovalidate that hello resources are capable of an upgrade.</span></span> <span data-ttu-id="e4c7c-162">Om hello valideringen misslyckas, får du felmeddelanden eller varningar.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-162">If hello validation fails, you will receive appropriate error messages or warnings.</span></span>

<span data-ttu-id="e4c7c-163">**Hur rapporterar ett problem med hello uppgraderingen?**</span><span class="sxs-lookup"><span data-stu-id="e4c7c-163">**How do I report an issue with hello upgrade?**</span></span>

<span data-ttu-id="e4c7c-164">Om det uppstår fel under uppgraderingen hello Observera hello åtgärds-ID som anges i hello-fel.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-164">If you experience any failures during hello upgrade, note hello operation ID that's listed in hello error.</span></span> <span data-ttu-id="e4c7c-165">Microsoft-supporten fungerar proaktivt om hur du löser problemet hello.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-165">Microsoft Support will proactively work on resolving hello issue.</span></span> <span data-ttu-id="e4c7c-166">Du kan också kontakta hello supportgrupp med ditt prenumerations-ID och valvnamnet åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-166">You can also contact hello Support team with your subscription ID, vault name, and operation ID.</span></span> <span data-ttu-id="e4c7c-167">Stöd för fungerar tooresolve hello problemet så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-167">Support will work tooresolve hello issue as quickly as possible.</span></span> <span data-ttu-id="e4c7c-168">Försök inte hello åtgärden om du inte uttryckligen har angett toodo så av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-168">Do not retry hello operation unless you are explicitly instructed toodo so by Microsoft.</span></span>

## <a name="run-hello-script"></a><span data-ttu-id="e4c7c-169">Kör hello-skript</span><span class="sxs-lookup"><span data-stu-id="e4c7c-169">Run hello script</span></span>

<span data-ttu-id="e4c7c-170">I PowerShell kör du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="e4c7c-170">In PowerShell, run hello following command:</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* <span data-ttu-id="e4c7c-171">Prenumerations-ID: hello prenumerations-ID som är kopplad till hello valv som du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-171">SubscriptionID: hello subscription ID that's associated with hello vault that you're upgrading.</span></span>

* <span data-ttu-id="e4c7c-172">VaultName: hello namnet på hello valv som du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-172">VaultName: hello name of hello vault that you're upgrading.</span></span>

* <span data-ttu-id="e4c7c-173">Plats: hello plats hello valv som du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-173">Location: hello location of hello vault that you're upgrading.</span></span>

* <span data-ttu-id="e4c7c-174">ResourceType: HyperVRecoveryManagerVault för Site Recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-174">ResourceType: HyperVRecoveryManagerVault for Site Recovery vaults.</span></span>

* <span data-ttu-id="e4c7c-175">TargetResourceGroupName: hello resursgrupp som du vill hello uppgraderas valvet toobe placeras.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-175">TargetResourceGroupName: hello resource group into which you want hello upgraded vault toobe placed.</span></span> <span data-ttu-id="e4c7c-176">TargetResourceGroupName kan vara en befintlig resursgrupp i Azure Resource Manager eller en ny.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-176">TargetResourceGroupName can be an existing resource group in Azure Resource Manager or a new one.</span></span> <span data-ttu-id="e4c7c-177">Om hello TargetResourceGroupName som har angetts inte finns, skapas den som en del av hello uppgraderingen i hello samma plats som hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-177">If hello TargetResourceGroupName that's supplied does not exist, it is created as part of hello upgrade in hello same location as hello vault.</span></span> <span data-ttu-id="e4c7c-178">Mer information finns i avsnittet hello ”resursgrupper” av [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-178">For more information, see hello "Resource groups" section of [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

    >[!NOTE]
    ><span data-ttu-id="e4c7c-179">Resurs grupp naming är ämne toocertain begränsningar.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-179">Resource group naming is subject toocertain constraints.</span></span> <span data-ttu-id="e4c7c-180">tooprevent valvet uppgradera fel, vara säker på att tooobserve hello naming convention noggrant.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-180">tooprevent vault upgrade failure, be sure tooobserve hello naming convention carefully.</span></span>
    >
    ><span data-ttu-id="e4c7c-181">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e4c7c-181">For example:</span></span>
    >
    ><span data-ttu-id="e4c7c-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 SubscriptionId - 1234-54123-354354-56416-8645 - VaultName gen2dr-platsen ”Norra Europa” - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc</span><span class="sxs-lookup"><span data-stu-id="e4c7c-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc</span></span>

<span data-ttu-id="e4c7c-183">Du kan också köra hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-183">Alternatively, you can run hello following script.</span></span> <span data-ttu-id="e4c7c-184">Ange hello värden för parametrarna hello krävs.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-184">Enter hello values for hello required parameters.</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for hello following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. <span data-ttu-id="e4c7c-185">hello PowerShell-skript uppmanas du tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-185">hello PowerShell script prompts you tooenter your credentials.</span></span> <span data-ttu-id="e4c7c-186">Ange dessa två gånger, en gång för hello klassisk distribution modellen konto och en gång för hello Azure Resource Manager-konto.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-186">Enter them twice, once for hello classic deployment model account and once for hello Azure Resource Manager account.</span></span>

2. <span data-ttu-id="e4c7c-187">När du har angett dina autentiseringsuppgifter körs hello skriptet en kontroll toodetermine om din infrastruktur installationen uppfyller hello tidigare nämnts krav.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-187">After you've entered your credentials, hello script runs a check toodetermine whether your infrastructure setup meets hello previously mentioned requirements.</span></span>

3. <span data-ttu-id="e4c7c-188">När hello kraven har kontrollerats och bekräftat, är du tillfrågas tooproceed med hello valvet uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-188">After hello prerequisites have been checked and confirmed, you are prompted tooproceed with hello vault upgrade.</span></span> <span data-ttu-id="e4c7c-189">hello uppgraderingsprocessen börja uppgradera ditt valv.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-189">hello upgrade process starts upgrading your vault.</span></span> <span data-ttu-id="e4c7c-190">hello hela uppgraderingen kan ta 15 too30 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-190">hello entire upgrade can take 15 too30 minutes toocomplete.</span></span>

4. <span data-ttu-id="e4c7c-191">När hello uppgraderingen har slutförts, du kan komma åt hello uppgraderade valvet i hello nya Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-191">After hello upgrade has been completed successfully, you can access hello upgraded vault in hello new Azure portal.</span></span>

## <a name="post-upgrade-vault-management"></a><span data-ttu-id="e4c7c-192">Efter uppgraderingen valvet management</span><span class="sxs-lookup"><span data-stu-id="e4c7c-192">Post-upgrade vault management</span></span>

### <a name="replicate-by-using-azure-site-recovery-in-hello-recovery-services-vault"></a><span data-ttu-id="e4c7c-193">Replikera med hjälp av Azure Site Recovery i hello Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="e4c7c-193">Replicate by using Azure Site Recovery in hello Recovery Services vault</span></span>

* <span data-ttu-id="e4c7c-194">Nu kan du skydda dina virtuella Azure-datorer från en region tooanother.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-194">You can now protect your Azure VMs from one region tooanother.</span></span> <span data-ttu-id="e4c7c-195">Mer information finns i [replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-195">For more information, see [Replicate Azure VMs between regions with Azure Site Recovery](site-recovery-azure-to-azure.md).</span></span>

* <span data-ttu-id="e4c7c-196">Läs mer om att replikera virtuella VMware-datorer tooAzure [tooAzure replikera virtuella VMware-datorer med Site Recovery](vmware-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-196">For more information about replicating VMware VMs tooAzure, see [Replicate VMware VMs tooAzure with Site Recovery](vmware-walkthrough-overview.md).</span></span>

* <span data-ttu-id="e4c7c-197">Läs mer om att replikera virtuella Hyper-V-datorer (utan VMM) tooAzure [replikera Hyper-V virtuella datorer (utan VMM) tooAzure](hyper-v-site-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-197">For more information about replicating Hyper-V VMs (without VMM) tooAzure, see [Replicate Hyper-V virtual machines (without VMM) tooAzure](hyper-v-site-walkthrough-overview.md).</span></span>

* <span data-ttu-id="e4c7c-198">Läs mer om att replikera virtuella Hyper-V-datorer (med VMM) tooAzure [replikera Hyper-V virtuella datorer i VMM-moln tooAzure med Site Recovery hello Azure-portalen](vmm-to-azure-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-198">For more information about replicating Hyper-V VMs (with VMM) tooAzure, see [Replicate Hyper-V virtual machines in VMM clouds tooAzure using Site Recovery in hello Azure portal](vmm-to-azure-walkthrough-overview.md).</span></span>

* <span data-ttu-id="e4c7c-199">Mer information om replikering av Hyper-virtuella datorer (med VMM) tooa sekundär plats finns [replikera Hyper-V virtuella datorer i VMM moln tooa sekundära VMM-plats använder hello Azure-portalen](site-recovery-vmm-to-vmm.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-199">For more information about replicating Hyper-VMs (with VMM) tooa secondary site, see [Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site using hello Azure portal](site-recovery-vmm-to-vmm.md).</span></span>

* <span data-ttu-id="e4c7c-200">Mer information om att replikera virtuella VMware-datorer tooa sekundär plats finns [replikera lokala virtuella VMware-datorer eller fysiska servrar tooa sekundär plats i hello klassiska Azure-portalen](site-recovery-vmware-to-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-200">For more information about replicating VMware VMs tooa secondary site, see [Replicate on-premises VMware virtual machines or physical servers tooa secondary site in hello classic Azure portal](site-recovery-vmware-to-vmware.md).</span></span>

### <a name="view-your-replicated-items"></a><span data-ttu-id="e4c7c-201">Visa dina replikerade objekt</span><span class="sxs-lookup"><span data-stu-id="e4c7c-201">View your replicated items</span></span>

<span data-ttu-id="e4c7c-202">hello visar följande bild hello Recovery Services-valvet instrumentpanelens sida som visar viktiga entiteter för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-202">hello following image shows hello Recovery Services vault dashboard page that displays key entities for hello vault.</span></span> <span data-ttu-id="e4c7c-203">tooview en lista över skyddade enheter i hello valvet, Välj **Site Recovery** > **replikerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-203">tooview a list of protected entities in hello vault, select **Site Recovery** > **Replicated items**.</span></span>


![Replikerade objekt](./media/upgrade-site-recovery-vaults/replicateditems.png)

<span data-ttu-id="e4c7c-205">hello följande bild visar hello arbetsflöde för att visa dina replikerade objekt och hello **redundans** kommandot för att initiera en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-205">hello following image shows hello workflow for viewing your replicated items and hello **Failover** command for initiating a failover.</span></span>

![Replikerade objekt](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a><span data-ttu-id="e4c7c-207">Visa replikeringsinställningarna för</span><span class="sxs-lookup"><span data-stu-id="e4c7c-207">View your replication settings</span></span>

<span data-ttu-id="e4c7c-208">Varje skyddsgrupp har konfigurerats med kopieringsfrekvensen, kvarhållningstid för återställningspunkten, frekvensen för programkonsekventa ögonblicksbilder och andra replikeringsinställningarna i hello Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-208">In hello Site Recovery vault, each protection group is configured with copy frequency, recovery point retention, frequency of application consistent snapshots, and other replication settings.</span></span> <span data-ttu-id="e4c7c-209">De här inställningarna är konfigurerade som en replikeringsprincip i hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-209">In hello Recovery Services vault, these settings are configured as a replication policy.</span></span> <span data-ttu-id="e4c7c-210">hello hello princip heter hello namnet på skyddsgruppen hello eller hello *primarycloud_Policy*.</span><span class="sxs-lookup"><span data-stu-id="e4c7c-210">hello name of hello policy is hello name of hello protection group or hello *primarycloud_Policy*.</span></span>

<span data-ttu-id="e4c7c-211">Mer information om replikeringsprinciper finns [hantera replikeringsprincip för VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="e4c7c-211">For more information about replication policy, see [Manage replication policy for VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).</span></span>
