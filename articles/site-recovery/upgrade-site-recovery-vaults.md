---
title: Uppgradera ett Site Recovery-valv till ett Azure Recovery Services-valv
description: "Lär dig hur du uppgraderar ett Azure Site Recovery-valv till Recovery Services-valvet"
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
ms.openlocfilehash: fdb33ea0d08353b491f2934fcf885fcb6910b9a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a><span data-ttu-id="de971-103">Uppgradera ett Site Recovery-valv till en Azure Resource Manager-baserade Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="de971-103">Upgrade a Site Recovery vault to an Azure Resource Manager-based Recovery Services vault</span></span>

<span data-ttu-id="de971-104">Den här artikeln beskriver hur du uppgraderar Azure Site Recovery-valv till Azure Resource Manager-baserade återställningstjänsten valv utan någon inverkan på pågående replikering.</span><span class="sxs-lookup"><span data-stu-id="de971-104">This article describes how to upgrade Azure Site Recovery vaults to Azure Resource Manager-based Recovery Service vaults without any impact on ongoing replication.</span></span> <span data-ttu-id="de971-105">Mer information om Azure Resource Manager-funktioner och fördelar finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de971-105">For more information about Azure Resource Manager features and benefits, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="de971-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="de971-106">Introduction</span></span>
<span data-ttu-id="de971-107">Recovery Services-valvet är en Azure Resource Manager-resurs för att hantera säkerhetskopiering och katastrofåterställning recovery internt i molnet.</span><span class="sxs-lookup"><span data-stu-id="de971-107">A Recovery Services vault is an Azure Resource Manager resource for managing backup and disaster recovery natively in the cloud.</span></span> <span data-ttu-id="de971-108">Det är en enhetlig valv som du kan använda i den nya Azure-portalen och den ersätter den klassiska säkerhetskopieringen och Site Recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="de971-108">It is a unified vault that you can use in the new Azure portal, and it replaces the classic backup and Site Recovery vaults.</span></span>

<span data-ttu-id="de971-109">Recovery Services-valv erbjuder en matris med-funktioner, inklusive:</span><span class="sxs-lookup"><span data-stu-id="de971-109">Recovery Services vaults offer an array of features, including:</span></span>

* <span data-ttu-id="de971-110">Azure Resource Manager-stöd: du kan skydda och växla över dina virtuella datorer och fysiska datorer till en Azure Resource Manager hög.</span><span class="sxs-lookup"><span data-stu-id="de971-110">Azure Resource Manager support: You can protect and fail over your virtual machines and physical machines into an Azure Resource Manager stack.</span></span>

* <span data-ttu-id="de971-111">Utesluta disk: Om du har temporära filer eller hög omsättning data som du inte vill använda alla bandbredd för, kan du undanta volymer från replikering.</span><span class="sxs-lookup"><span data-stu-id="de971-111">Exclude disk: If you have temp files or high churn data that you don’t want to use all your bandwidth for, you can exclude volumes from replication.</span></span> <span data-ttu-id="de971-112">Den här funktionen har aktiverats för närvarande i *VMware till Azure* och *Hyper-V till Azure* och har utökats till andra scenarier samt.</span><span class="sxs-lookup"><span data-stu-id="de971-112">This capability has been enabled currently in *VMware to Azure* and *Hyper-V to Azure* and is extended to other scenarios as well.</span></span>

* <span data-ttu-id="de971-113">Stöd för premium och lokalt redundant lagring: nu kan du skydda servrar i premium storage-konton som utvecklats att skydda program med högre i/o-åtgärder per sekund (IOPS).</span><span class="sxs-lookup"><span data-stu-id="de971-113">Support for premium and locally redundant storage: You can now protect servers in premium storage accounts that allow customers to protect applications with higher input/output operations per second (IOPS).</span></span> <span data-ttu-id="de971-114">Den här funktionen är aktiverat i *VMware till Azure*.</span><span class="sxs-lookup"><span data-stu-id="de971-114">This capability is currently enabled in *VMware to Azure*.</span></span>

* <span data-ttu-id="de971-115">Effektiv Kom igång-upplevelse: förbättrad Kom igång-upplevelse är utformad för att göra det lättare katastrofåterställning installationen.</span><span class="sxs-lookup"><span data-stu-id="de971-115">Streamlined getting-started experience: The enhanced getting-started experience has been designed to make disaster-recovery setup easy.</span></span>

* <span data-ttu-id="de971-116">Säkerhetskopiering och Site Recovery-hantering från samma valvet: du kan nu skydda servrar för katastrofåterställning eller utför säkerhetskopiering från samma valvet, som kan försämras avsevärt minska din hantering.</span><span class="sxs-lookup"><span data-stu-id="de971-116">Backup and Site Recovery management from the same vault: You can now protect servers for disaster recovery or perform backup from the same vault, which can reduce your management overhead significantly.</span></span>

<span data-ttu-id="de971-117">Mer information om uppgraderade upplevelse och funktioner finns i [lagring, säkerhetskopiering och återställning blogg](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span><span class="sxs-lookup"><span data-stu-id="de971-117">For more information about the upgraded experience and features, see the [Storage, Backup, and Recovery blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span></span>

## <a name="salient-features"></a><span data-ttu-id="de971-118">Viktigaste funktioner</span><span class="sxs-lookup"><span data-stu-id="de971-118">Salient features</span></span>

* <span data-ttu-id="de971-119">Ingen inverkan på pågående replikering: pågående replikeringar fortsätta utan avbrott under och efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-119">No impact on ongoing replication: Ongoing replications continue without any interruption during and post upgrade.</span></span>

* <span data-ttu-id="de971-120">Utan extra kostnad: hämta en uppsättning uppdaterade funktioner utan extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="de971-120">No additional cost: Get an entire set of updated capabilities at no additional cost.</span></span>

* <span data-ttu-id="de971-121">Ingen dataförlust: eftersom den här processen är inte en migrering och uppgradering, inställningar och befintliga återställningspunkter för replikeringen bevaras under och efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-121">No data loss: Because this process is an upgrade and not a migration, existing replication recovery points and settings remain intact during and after the upgrade.</span></span>


## <a name="what-happens-during-the-vault-upgrade"></a><span data-ttu-id="de971-122">Vad händer under uppgraderingen valvet?</span><span class="sxs-lookup"><span data-stu-id="de971-122">What happens during the vault upgrade?</span></span>

<span data-ttu-id="de971-123">Du kan inte utföra åtgärder som registrerar en ny server eller att aktivera replikering för en virtuell dator (VM) under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-123">During the upgrade, you cannot perform operations such as registering a new server or enabling replication for a virtual machine (VM).</span></span> <span data-ttu-id="de971-124">Åtgärder som innefattar läsning av data från eller skriva data till valvet, till exempel pågående replikering av skyddade objekt till valvet, fortsätta utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="de971-124">Operations that involve reading data from or writing data to the vault, such as ongoing replication of protected items to the vault, continue uninterrupted.</span></span>

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a><span data-ttu-id="de971-125">Ändringar av automation och verktygsuppsättning efter uppgraderingen</span><span class="sxs-lookup"><span data-stu-id="de971-125">Changes to automation and tooling after the upgrade</span></span>
<span data-ttu-id="de971-126">När du uppgraderar typen valvet från den klassiska distributionsmodellen till Resource Manager-distributionsmodellen, Uppdatera befintlig automation eller verktygsuppsättning så att den fortsätter att fungera efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-126">As you upgrade the vault type from the classic deployment model to the Resource Manager deployment model, update the existing automation or tooling to ensure that it continues to work after the upgrade.</span></span>

### <a name="prepare-your-environment-for-the-upgrade"></a><span data-ttu-id="de971-127">Förbereda miljön för uppgraderingen</span><span class="sxs-lookup"><span data-stu-id="de971-127">Prepare your environment for the upgrade</span></span>

* [<span data-ttu-id="de971-128">Installera PowerShell eller uppgradera till version 5 eller senare</span><span class="sxs-lookup"><span data-stu-id="de971-128">Install PowerShell or upgrade it to version 5 or later</span></span>](https://www.microsoft.com/download/details.aspx?id=50395)
* [<span data-ttu-id="de971-129">Installera den senaste versionen av Azure PowerShell MSI</span><span class="sxs-lookup"><span data-stu-id="de971-129">Install the latest version of Azure PowerShell MSI</span></span>](https://github.com/Azure/azure-powershell/releases)
* [<span data-ttu-id="de971-130">Hämta uppgraderingsskriptet Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="de971-130">Download the Recovery Services vault upgrade script</span></span>](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a><span data-ttu-id="de971-131">Krav</span><span class="sxs-lookup"><span data-stu-id="de971-131">Prerequisites</span></span>
<span data-ttu-id="de971-132">Om du vill uppgradera från Site Recovery-valv till Azure Resource Manager-baserade återställningstjänsten valv, måste du uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="de971-132">To upgrade from Site Recovery vaults to Azure Resource Manager-based Recovery Service vaults, you must meet the following requirements:</span></span>

* <span data-ttu-id="de971-133">Minsta agent-version: versionen av Azure Site Recovery-Provider installerad på servern måste vara 5.1.1700.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="de971-133">Minimum agent version: The version of Azure Site Recovery Provider installed on your server must be 5.1.1700.0 or later.</span></span>

* <span data-ttu-id="de971-134">Konfigurationer som stöds: du kan inte konfigurera ditt valv med lagringsnätverk (SAN) eller SQL Server AlwaysOn-Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="de971-134">Supported configuration: You cannot configure your vault with storage area network (SAN) or SQL Server AlwaysOn Availability Groups.</span></span> <span data-ttu-id="de971-135">Alla andra konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="de971-135">All other configurations are supported.</span></span>

    >[!NOTE]
    ><span data-ttu-id="de971-136">Du kan hantera lagring mappning endast via PowerShell efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-136">After the upgrade, you can manage storage mapping only via PowerShell.</span></span>

* <span data-ttu-id="de971-137">Scenario för distribution som stöds: ditt valv får inte vara den *VMware till Azure* äldre distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="de971-137">Supported deployment scenario: Your vault shouldn’t be the *VMware to Azure* legacy deployment model.</span></span> <span data-ttu-id="de971-138">Innan du fortsätter först flytta förbättrad distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="de971-138">Before you proceed, first move to the enhanced deployment model.</span></span>

* <span data-ttu-id="de971-139">Inga aktiva användarinitierad jobb som rör hantering plan operations: eftersom åtkomst till management-plan är begränsad under uppgraderingen, Slutför alla din plan hanteringsåtgärder innan du utlöser uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-139">No active user-initiated jobs that involve management plane operations: Because access to the management plane is restricted during upgrade, complete all your management plane actions before you trigger the upgrade.</span></span> <span data-ttu-id="de971-140">Den här processen innehåller pågående replikering.</span><span class="sxs-lookup"><span data-stu-id="de971-140">This process doesn’t include ongoing replication.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="de971-141">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="de971-141">Frequently asked questions</span></span>

<span data-ttu-id="de971-142">**Påverkar den här uppgraderingen min pågående replikering?**</span><span class="sxs-lookup"><span data-stu-id="de971-142">**Does this upgrade affect my ongoing replication?**</span></span>

<span data-ttu-id="de971-143">Nej.</span><span class="sxs-lookup"><span data-stu-id="de971-143">No.</span></span> <span data-ttu-id="de971-144">Din pågående replikering fortsätter utan avbrott under och efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-144">Your ongoing replication continues uninterrupted during and after the upgrade.</span></span>

<span data-ttu-id="de971-145">**Vad händer med nätverksinställningar som plats-till-plats VPN och IP-inställningar?**</span><span class="sxs-lookup"><span data-stu-id="de971-145">**What happens to network settings such as site-to-site VPN and IP settings?**</span></span>

<span data-ttu-id="de971-146">Uppgraderingen påverkar inte nätverksinställningarna.</span><span class="sxs-lookup"><span data-stu-id="de971-146">The upgrade doesn't affect the network settings.</span></span> <span data-ttu-id="de971-147">Alla Azure till lokala anslutningar påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="de971-147">All Azure-to-on-premises connections remain intact.</span></span>

<span data-ttu-id="de971-148">**Vad händer med min valv om jag inte tänker uppgradera i den nära framtiden?**</span><span class="sxs-lookup"><span data-stu-id="de971-148">**What happens to my vaults if I don’t plan to upgrade in the near future?**</span></span>

<span data-ttu-id="de971-149">Stöd för Site Recovery-valvet i gamla Azure-portalen att bli inaktuell från September 2017.</span><span class="sxs-lookup"><span data-stu-id="de971-149">Support for Site Recovery vault in the old Azure portal will be deprecated starting September 2017.</span></span> <span data-ttu-id="de971-150">Vi rekommenderar starkt att du använder funktionen uppgraderingen för att flytta till den nya portalen.</span><span class="sxs-lookup"><span data-stu-id="de971-150">We strongly recommend that you use the upgrade feature to move to the new portal.</span></span>

<span data-ttu-id="de971-151">**Vad innebär det här migreringsplan för Mina befintliga verktygsuppsättning?**</span><span class="sxs-lookup"><span data-stu-id="de971-151">**What does this migration plan mean for my existing tooling?**</span></span>  

<span data-ttu-id="de971-152">Uppdatera din verktygsuppsättning till Resource Manager-distributionsmodellen är en av de viktigaste ändringarna som du måste ta hänsyn till i din uppgradering planer.</span><span class="sxs-lookup"><span data-stu-id="de971-152">Updating your tooling to the Resource Manager deployment model is one of the most important changes that you must account for in your upgrade plans.</span></span> <span data-ttu-id="de971-153">Recovery Services-valv baseras på Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="de971-153">The Recovery Services vaults are based on the Resource Manager deployment model.</span></span> 

<span data-ttu-id="de971-154">**Hur lång tid tar management-plan avbrottstiden senaste?**</span><span class="sxs-lookup"><span data-stu-id="de971-154">**How long does the management-plane downtime last?**</span></span>

<span data-ttu-id="de971-155">Uppgraderingen tar normalt ungefär 15 till 30 minuter och det kan ta upp till högst en timme.</span><span class="sxs-lookup"><span data-stu-id="de971-155">The upgrade ordinarily takes about 15 to 30 minutes, and it could take up to a maximum of one hour.</span></span>

<span data-ttu-id="de971-156">**Kan jag återställa efter uppgraderingen?**</span><span class="sxs-lookup"><span data-stu-id="de971-156">**Can I roll back after upgrading?**</span></span>

<span data-ttu-id="de971-157">Nej.</span><span class="sxs-lookup"><span data-stu-id="de971-157">No.</span></span> <span data-ttu-id="de971-158">Återställning stöds inte när resurserna har uppgraderats.</span><span class="sxs-lookup"><span data-stu-id="de971-158">Rollback is not supported after the resources have been successfully upgraded.</span></span>

<span data-ttu-id="de971-159">**Kan jag verifiera min prenumeration eller resurser för att se om de kan uppgraderas?**</span><span class="sxs-lookup"><span data-stu-id="de971-159">**Can I validate my subscription or resources to see whether they can be upgraded?**</span></span>

<span data-ttu-id="de971-160">Ja.</span><span class="sxs-lookup"><span data-stu-id="de971-160">Yes.</span></span> <span data-ttu-id="de971-161">Det första steget i uppgraderingen är att verifiera att resurserna är utföra en uppgradering i uppgraderingsalternativet plattform som stöds.</span><span class="sxs-lookup"><span data-stu-id="de971-161">In the platform-supported upgrade option, the first step in the upgrade is to validate that the resources are capable of an upgrade.</span></span> <span data-ttu-id="de971-162">Om verifieringen misslyckas, får du felmeddelanden eller varningar.</span><span class="sxs-lookup"><span data-stu-id="de971-162">If the validation fails, you will receive appropriate error messages or warnings.</span></span>

<span data-ttu-id="de971-163">**Hur rapporterar ett problem med uppgraderingen?**</span><span class="sxs-lookup"><span data-stu-id="de971-163">**How do I report an issue with the upgrade?**</span></span>

<span data-ttu-id="de971-164">Observera åtgärds-ID som anges i fel om det uppstår fel under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="de971-164">If you experience any failures during the upgrade, note the operation ID that's listed in the error.</span></span> <span data-ttu-id="de971-165">Microsoft-supporten fungerar proaktivt för att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="de971-165">Microsoft Support will proactively work on resolving the issue.</span></span> <span data-ttu-id="de971-166">Du kan också Kontakta supportteamet med ditt prenumerations-ID och valvnamnet åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="de971-166">You can also contact the Support team with your subscription ID, vault name, and operation ID.</span></span> <span data-ttu-id="de971-167">Stöd för fungerar för att lösa problemet så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="de971-167">Support will work to resolve the issue as quickly as possible.</span></span> <span data-ttu-id="de971-168">Försök inte igen om du inte uttryckligen uppmanas att göra det från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="de971-168">Do not retry the operation unless you are explicitly instructed to do so by Microsoft.</span></span>

## <a name="run-the-script"></a><span data-ttu-id="de971-169">Kör skriptet</span><span class="sxs-lookup"><span data-stu-id="de971-169">Run the script</span></span>

<span data-ttu-id="de971-170">I PowerShell kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="de971-170">In PowerShell, run the following command:</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* <span data-ttu-id="de971-171">Prenumerations-ID: Prenumerations-ID som är kopplad till valvet som du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="de971-171">SubscriptionID: The subscription ID that's associated with the vault that you're upgrading.</span></span>

* <span data-ttu-id="de971-172">VaultName: Namnet på valvet som du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="de971-172">VaultName: The name of the vault that you're upgrading.</span></span>

* <span data-ttu-id="de971-173">Plats: Platsen för valvet som du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="de971-173">Location: The location of the vault that you're upgrading.</span></span>

* <span data-ttu-id="de971-174">ResourceType: HyperVRecoveryManagerVault för Site Recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="de971-174">ResourceType: HyperVRecoveryManagerVault for Site Recovery vaults.</span></span>

* <span data-ttu-id="de971-175">TargetResourceGroupName: Resursgruppen som du vill att placera uppgraderade valvet.</span><span class="sxs-lookup"><span data-stu-id="de971-175">TargetResourceGroupName: The resource group into which you want the upgraded vault to be placed.</span></span> <span data-ttu-id="de971-176">TargetResourceGroupName kan vara en befintlig resursgrupp i Azure Resource Manager eller en ny.</span><span class="sxs-lookup"><span data-stu-id="de971-176">TargetResourceGroupName can be an existing resource group in Azure Resource Manager or a new one.</span></span> <span data-ttu-id="de971-177">Om TargetResourceGroupName som har angetts inte finns, skapas den som en del av uppgraderingen på samma plats som valvet.</span><span class="sxs-lookup"><span data-stu-id="de971-177">If the TargetResourceGroupName that's supplied does not exist, it is created as part of the upgrade in the same location as the vault.</span></span> <span data-ttu-id="de971-178">Mer information finns i avsnittet ”resursgrupper” i [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="de971-178">For more information, see the "Resource groups" section of [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

    >[!NOTE]
    ><span data-ttu-id="de971-179">Resurs grupp naming är vissa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="de971-179">Resource group naming is subject to certain constraints.</span></span> <span data-ttu-id="de971-180">Om du vill förhindra valvet uppgraderingen skulle misslyckas, bör du se namngivningskonvention noggrant.</span><span class="sxs-lookup"><span data-stu-id="de971-180">To prevent vault upgrade failure, be sure to observe the naming convention carefully.</span></span>
    >
    ><span data-ttu-id="de971-181">Exempel:</span><span class="sxs-lookup"><span data-stu-id="de971-181">For example:</span></span>
    >
    ><span data-ttu-id="de971-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 SubscriptionId - 1234-54123-354354-56416-8645 - VaultName gen2dr-platsen ”Norra Europa” - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc</span><span class="sxs-lookup"><span data-stu-id="de971-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc</span></span>

<span data-ttu-id="de971-183">Du kan också köra följande skript.</span><span class="sxs-lookup"><span data-stu-id="de971-183">Alternatively, you can run the following script.</span></span> <span data-ttu-id="de971-184">Ange värden för de obligatoriska parametrarna.</span><span class="sxs-lookup"><span data-stu-id="de971-184">Enter the values for the required parameters.</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. <span data-ttu-id="de971-185">PowerShell-skriptet uppmanas du att ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="de971-185">The PowerShell script prompts you to enter your credentials.</span></span> <span data-ttu-id="de971-186">Ange dessa två gånger, en gång för kontot klassisk distribution modell och en gång för Azure Resource Manager-kontot.</span><span class="sxs-lookup"><span data-stu-id="de971-186">Enter them twice, once for the classic deployment model account and once for the Azure Resource Manager account.</span></span>

2. <span data-ttu-id="de971-187">När du har angett dina autentiseringsuppgifter, körs skriptet en kontroll för att fastställa om din infrastrukturinställningar uppfyller krav som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="de971-187">After you've entered your credentials, the script runs a check to determine whether your infrastructure setup meets the previously mentioned requirements.</span></span>

3. <span data-ttu-id="de971-188">Efter att kraven har kontrollerats och bekräftat, uppmanas du att fortsätta med uppgraderingen valvet.</span><span class="sxs-lookup"><span data-stu-id="de971-188">After the prerequisites have been checked and confirmed, you are prompted to proceed with the vault upgrade.</span></span> <span data-ttu-id="de971-189">Uppgraderingen kan du börja uppgradera ditt valv.</span><span class="sxs-lookup"><span data-stu-id="de971-189">The upgrade process starts upgrading your vault.</span></span> <span data-ttu-id="de971-190">Hela uppgraderingen kan ta 15 till 30 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="de971-190">The entire upgrade can take 15 to 30 minutes to complete.</span></span>

4. <span data-ttu-id="de971-191">När uppgraderingen har slutförts, kan du komma åt uppgraderade valvet i den nya Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de971-191">After the upgrade has been completed successfully, you can access the upgraded vault in the new Azure portal.</span></span>

## <a name="post-upgrade-vault-management"></a><span data-ttu-id="de971-192">Efter uppgraderingen valvet management</span><span class="sxs-lookup"><span data-stu-id="de971-192">Post-upgrade vault management</span></span>

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a><span data-ttu-id="de971-193">Replikera med hjälp av Azure Site Recovery i Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="de971-193">Replicate by using Azure Site Recovery in the Recovery Services vault</span></span>

* <span data-ttu-id="de971-194">Nu kan du skydda dina virtuella Azure-datorer från en region till en annan.</span><span class="sxs-lookup"><span data-stu-id="de971-194">You can now protect your Azure VMs from one region to another.</span></span> <span data-ttu-id="de971-195">Mer information finns i [replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="de971-195">For more information, see [Replicate Azure VMs between regions with Azure Site Recovery](site-recovery-azure-to-azure.md).</span></span>

* <span data-ttu-id="de971-196">Mer information om att replikera virtuella VMware-datorer till Azure finns [replikera virtuella VMware-datorer till Azure med Site Recovery](vmware-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de971-196">For more information about replicating VMware VMs to Azure, see [Replicate VMware VMs to Azure with Site Recovery](vmware-walkthrough-overview.md).</span></span>

* <span data-ttu-id="de971-197">Mer information om att replikera virtuella Hyper-V-datorer (utan VMM) till Azure finns [replikera Hyper-V virtuella datorer (utan VMM) till Azure](hyper-v-site-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de971-197">For more information about replicating Hyper-V VMs (without VMM) to Azure, see [Replicate Hyper-V virtual machines (without VMM) to Azure](hyper-v-site-walkthrough-overview.md).</span></span>

* <span data-ttu-id="de971-198">Mer information om att replikera virtuella Hyper-V-datorer (med VMM) till Azure finns [replikera Hyper-V virtuella datorer i VMM-moln till Azure med Site Recovery på Azure-portalen](vmm-to-azure-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de971-198">For more information about replicating Hyper-V VMs (with VMM) to Azure, see [Replicate Hyper-V virtual machines in VMM clouds to Azure using Site Recovery in the Azure portal](vmm-to-azure-walkthrough-overview.md).</span></span>

* <span data-ttu-id="de971-199">Mer information om replikering av Hyper-virtuella datorer (med VMM) till en sekundär plats finns [replikera Hyper-V virtuella datorer i VMM-moln till en sekundär VMM-plats med hjälp av Azure portal](site-recovery-vmm-to-vmm.md).</span><span class="sxs-lookup"><span data-stu-id="de971-199">For more information about replicating Hyper-VMs (with VMM) to a secondary site, see [Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site using the Azure portal](site-recovery-vmm-to-vmm.md).</span></span>

* <span data-ttu-id="de971-200">Mer information om att replikera virtuella VMware-datorer till en sekundär plats finns [replikera lokala virtuella VMware-datorer eller fysiska servrar till en sekundär plats i den klassiska Azure-portalen](site-recovery-vmware-to-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="de971-200">For more information about replicating VMware VMs to a secondary site, see [Replicate on-premises VMware virtual machines or physical servers to a secondary site in the classic Azure portal](site-recovery-vmware-to-vmware.md).</span></span>

### <a name="view-your-replicated-items"></a><span data-ttu-id="de971-201">Visa dina replikerade objekt</span><span class="sxs-lookup"><span data-stu-id="de971-201">View your replicated items</span></span>

<span data-ttu-id="de971-202">Följande bild visar sidan Recovery Services-valvet instrumentpanelen som visar viktiga entiteter för valvet.</span><span class="sxs-lookup"><span data-stu-id="de971-202">The following image shows the Recovery Services vault dashboard page that displays key entities for the vault.</span></span> <span data-ttu-id="de971-203">Om du vill visa en lista över skyddade enheter i valvet, Välj **Site Recovery** > **replikerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="de971-203">To view a list of protected entities in the vault, select **Site Recovery** > **Replicated items**.</span></span>


![Replikerade objekt](./media/upgrade-site-recovery-vaults/replicateditems.png)

<span data-ttu-id="de971-205">Följande bild visar arbetsflödet för att visa dina replikerade objekt och **redundans** kommandot för att initiera en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="de971-205">The following image shows the workflow for viewing your replicated items and the **Failover** command for initiating a failover.</span></span>

![Replikerade objekt](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a><span data-ttu-id="de971-207">Visa replikeringsinställningarna för</span><span class="sxs-lookup"><span data-stu-id="de971-207">View your replication settings</span></span>

<span data-ttu-id="de971-208">Varje skyddsgrupp har konfigurerats med kopieringsfrekvensen, kvarhållningstid för återställningspunkten, frekvensen för programkonsekventa ögonblicksbilder och andra replikeringsinställningarna i Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="de971-208">In the Site Recovery vault, each protection group is configured with copy frequency, recovery point retention, frequency of application consistent snapshots, and other replication settings.</span></span> <span data-ttu-id="de971-209">De här inställningarna är konfigurerade som en replikeringsprincip i Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="de971-209">In the Recovery Services vault, these settings are configured as a replication policy.</span></span> <span data-ttu-id="de971-210">Namnet på principen är namnet på skyddsgruppen eller *primarycloud_Policy*.</span><span class="sxs-lookup"><span data-stu-id="de971-210">The name of the policy is the name of the protection group or the *primarycloud_Policy*.</span></span>

<span data-ttu-id="de971-211">Mer information om replikeringsprinciper finns [hantera replikeringsprinciper för VMware till Azure](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="de971-211">For more information about replication policy, see [Manage replication policy for VMware to Azure](site-recovery-setup-replication-settings-vmware.md).</span></span>
