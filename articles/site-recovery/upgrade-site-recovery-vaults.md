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
# <a name="upgrade-a-site-recovery-vault-tooan-azure-resource-manager-based-recovery-services-vault"></a>Uppgradera en Site Recovery-valvet tooan Azure Resource Manager-baserade Recovery Services-valvet

Den här artikeln beskriver hur tooupgrade Azure Site Recovery-valv tooAzure Resource Manager-baserade återställningstjänsten valv utan någon inverkan på pågående replikering. Mer information om Azure Resource Manager-funktioner och fördelar finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Introduktion
Recovery Services-valvet är en Azure Resource Manager-resurs för att hantera säkerhetskopiering och katastrofåterställning recovery internt i hello molnet. Det är en enhetlig valvet som du kan använda i hello nya Azure-portalen och den ersätter hello klassiska säkerhetskopiering och Site Recovery-valv.

Recovery Services-valv erbjuder en matris med-funktioner, inklusive:

* Azure Resource Manager-stöd: du kan skydda och växla över dina virtuella datorer och fysiska datorer till en Azure Resource Manager hög.

* Utesluta disk: Om du har temporära filer eller hög omsättning data som du inte vill toouse alla bandbredd för, kan du undanta volymer från replikering. Den här funktionen har aktiverats för närvarande i *VMware tooAzure* och *Hyper-V tooAzure* och utökas tooother-scenarier.

* Stöd för premium och lokalt redundant lagring: nu kan du skydda servrar i premium-lagring konton som låter kunder tooprotect program med högre i/o-åtgärder per sekund (IOPS). Den här funktionen är aktiverat i *VMware tooAzure*.

* Effektiv Kom igång-upplevelse: hello förbättrad Kom igång-upplevelse har utformats toomake katastrofåterställning installationsprogrammet enkelt.

* Säkerhetskopiering och Site Recovery-hantering från hello samma valvet: du kan nu skydda servrar för katastrofåterställning eller utför säkerhetskopiering från hello samma valvet, som kan försämras avsevärt minska din hantering.

Mer information om hello uppgraderas upplevelse och funktioner finns hello [lagring, säkerhetskopiering och återställning blogg](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Viktigaste funktioner

* Ingen inverkan på pågående replikering: pågående replikeringar fortsätta utan avbrott under och efter uppgraderingen.

* Utan extra kostnad: hämta en uppsättning uppdaterade funktioner utan extra kostnad.

* Ingen dataförlust: eftersom den här processen är inte en migrering och uppgradering, inställningar och befintliga återställningspunkter för replikeringen bevaras under och efter hello uppgraderingen.


## <a name="what-happens-during-hello-vault-upgrade"></a>Vad händer under hello valvet uppgraderingen?

Du kan inte utföra åtgärder som registrerar en ny server eller att aktivera replikering för en virtuell dator (VM) under hello uppgraderingen. Åtgärder som innefattar läsning av data från eller skriva data toohello valvet, till exempel pågående replikering av skyddade objekt toohello valvet, fortsätta utan avbrott.

### <a name="changes-tooautomation-and-tooling-after-hello-upgrade"></a>Ändrar tooautomation och verktygsuppsättning efter hello uppgradering
När du uppgraderar hello valvet typ från hello klassiska modellen toohello Resource Manager distribution distributionsmodell uppdatera hello befintlig automation eller verktygsuppsättning tooensure den fortsätter toowork efter hello uppgradering.

### <a name="prepare-your-environment-for-hello-upgrade"></a>Förbereda miljön för hello uppgradering

* [Installera PowerShell eller uppgradera den tooversion 5 eller senare](https://www.microsoft.com/download/details.aspx?id=50395)
* [Installera hello senaste versionen av Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases)
* [Hämta uppgraderingsskript för hello Recovery Services-valvet](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Krav
tooupgrade från Site Recovery-valv tooAzure Resource Manager-baserade återställningstjänsten valv, hello följande krav måste uppfyllas:

* Minsta agent-version: hello-versionen av Azure Site Recovery-Provider installerad på servern måste vara 5.1.1700.0 eller senare.

* Konfigurationer som stöds: du kan inte konfigurera ditt valv med lagringsnätverk (SAN) eller SQL Server AlwaysOn-Tillgänglighetsgrupper. Alla andra konfigurationer som stöds.

    >[!NOTE]
    >Du kan hantera lagring mappning endast via PowerShell efter hello uppgradering.

* Scenario för distribution som stöds: ditt valv får inte vara hello *VMware tooAzure* äldre distributionsmodell. Innan du fortsätter bör du först flytta toohello förbättrad distributionsmodell.

* Inga aktiva användarinitierad jobb som rör hantering plan operations: eftersom åtkomst toohello management plan begränsas under uppgraderingen, Slutför alla din plan hanteringsåtgärder innan du utlösa hello uppgraderingen. Den här processen innehåller pågående replikering.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**Påverkar den här uppgraderingen min pågående replikering?**

Nej. Din pågående replikering fortsätter utan avbrott under och efter hello uppgraderingen.

**Vad händer toonetwork inställningar, till exempel plats-till-plats VPN och IP-inställningar?**

hello uppgraderingen påverkar inte hello nätverksinställningar. Alla Azure till lokala anslutningar påverkas inte.

**Vad händer toomy valv om du inte planerar tooupgrade i hello nära framtiden?**

Stöd för Site Recovery-valvet i hello gamla Azure-portalen att bli inaktuell från September 2017. Vi rekommenderar starkt att du använder hello uppgraderingsfunktionen toomove toohello nya portalen.

**Vad innebär det här migreringsplan för Mina befintliga verktygsuppsättning?**  

Uppdatera din verktygsuppsättning toohello Resource Manager-distributionsmodellen är en av hello viktigaste ändringarna som du måste ta hänsyn till i din uppgradering planer. hello Recovery Services-valv baseras på hello Resource Manager-modellen. 

**Hur länge hello management-plan driftstopp senast?**

hello uppgraderingen tar normalt ungefär 15 minuter för too30 och det kan ta upp tooa maximalt en timme.

**Kan jag återställa efter uppgraderingen?**

Nej. Återställning stöds inte när hello resurser har uppgraderats.

**Kan jag verifiera min prenumeration eller resurser toosee om de kan uppgraderas?**

Ja. Hello första steget i hello uppgraderingen är i hello stöds av plattformen uppgraderingsalternativet, toovalidate att hello resurser är utföra en uppgradering. Om hello valideringen misslyckas, får du felmeddelanden eller varningar.

**Hur rapporterar ett problem med hello uppgraderingen?**

Om det uppstår fel under uppgraderingen hello Observera hello åtgärds-ID som anges i hello-fel. Microsoft-supporten fungerar proaktivt om hur du löser problemet hello. Du kan också kontakta hello supportgrupp med ditt prenumerations-ID och valvnamnet åtgärds-ID. Stöd för fungerar tooresolve hello problemet så snabbt som möjligt. Försök inte hello åtgärden om du inte uttryckligen har angett toodo så av Microsoft.

## <a name="run-hello-script"></a>Kör hello-skript

I PowerShell kör du följande kommando hello:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* Prenumerations-ID: hello prenumerations-ID som är kopplad till hello valv som du uppgraderar.

* VaultName: hello namnet på hello valv som du uppgraderar.

* Plats: hello plats hello valv som du uppgraderar.

* ResourceType: HyperVRecoveryManagerVault för Site Recovery-valv.

* TargetResourceGroupName: hello resursgrupp som du vill hello uppgraderas valvet toobe placeras. TargetResourceGroupName kan vara en befintlig resursgrupp i Azure Resource Manager eller en ny. Om hello TargetResourceGroupName som har angetts inte finns, skapas den som en del av hello uppgraderingen i hello samma plats som hello-valvet. Mer information finns i avsnittet hello ”resursgrupper” av [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Resurs grupp naming är ämne toocertain begränsningar. tooprevent valvet uppgradera fel, vara säker på att tooobserve hello naming convention noggrant.
    >
    >Exempel:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 SubscriptionId - 1234-54123-354354-56416-8645 - VaultName gen2dr-platsen ”Norra Europa” - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc

Du kan också köra hello följande skript. Ange hello värden för parametrarna hello krävs.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for hello following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. hello PowerShell-skript uppmanas du tooenter dina autentiseringsuppgifter. Ange dessa två gånger, en gång för hello klassisk distribution modellen konto och en gång för hello Azure Resource Manager-konto.

2. När du har angett dina autentiseringsuppgifter körs hello skriptet en kontroll toodetermine om din infrastruktur installationen uppfyller hello tidigare nämnts krav.

3. När hello kraven har kontrollerats och bekräftat, är du tillfrågas tooproceed med hello valvet uppgraderingen. hello uppgraderingsprocessen börja uppgradera ditt valv. hello hela uppgraderingen kan ta 15 too30 minuter toocomplete.

4. När hello uppgraderingen har slutförts, du kan komma åt hello uppgraderade valvet i hello nya Azure-portalen.

## <a name="post-upgrade-vault-management"></a>Efter uppgraderingen valvet management

### <a name="replicate-by-using-azure-site-recovery-in-hello-recovery-services-vault"></a>Replikera med hjälp av Azure Site Recovery i hello Recovery Services-valvet

* Nu kan du skydda dina virtuella Azure-datorer från en region tooanother. Mer information finns i [replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery](site-recovery-azure-to-azure.md).

* Läs mer om att replikera virtuella VMware-datorer tooAzure [tooAzure replikera virtuella VMware-datorer med Site Recovery](vmware-walkthrough-overview.md).

* Läs mer om att replikera virtuella Hyper-V-datorer (utan VMM) tooAzure [replikera Hyper-V virtuella datorer (utan VMM) tooAzure](hyper-v-site-walkthrough-overview.md).

* Läs mer om att replikera virtuella Hyper-V-datorer (med VMM) tooAzure [replikera Hyper-V virtuella datorer i VMM-moln tooAzure med Site Recovery hello Azure-portalen](vmm-to-azure-walkthrough-overview.md).

* Mer information om replikering av Hyper-virtuella datorer (med VMM) tooa sekundär plats finns [replikera Hyper-V virtuella datorer i VMM moln tooa sekundära VMM-plats använder hello Azure-portalen](site-recovery-vmm-to-vmm.md).

* Mer information om att replikera virtuella VMware-datorer tooa sekundär plats finns [replikera lokala virtuella VMware-datorer eller fysiska servrar tooa sekundär plats i hello klassiska Azure-portalen](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>Visa dina replikerade objekt

hello visar följande bild hello Recovery Services-valvet instrumentpanelens sida som visar viktiga entiteter för hello-valvet. tooview en lista över skyddade enheter i hello valvet, Välj **Site Recovery** > **replikerade objekt**.


![Replikerade objekt](./media/upgrade-site-recovery-vaults/replicateditems.png)

hello följande bild visar hello arbetsflöde för att visa dina replikerade objekt och hello **redundans** kommandot för att initiera en växling vid fel.

![Replikerade objekt](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Visa replikeringsinställningarna för

Varje skyddsgrupp har konfigurerats med kopieringsfrekvensen, kvarhållningstid för återställningspunkten, frekvensen för programkonsekventa ögonblicksbilder och andra replikeringsinställningarna i hello Site Recovery-valvet. De här inställningarna är konfigurerade som en replikeringsprincip i hello Recovery Services-valvet. hello hello princip heter hello namnet på skyddsgruppen hello eller hello *primarycloud_Policy*.

Mer information om replikeringsprinciper finns [hantera replikeringsprincip för VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).
