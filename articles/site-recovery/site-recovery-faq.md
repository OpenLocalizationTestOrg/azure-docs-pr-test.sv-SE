---
title: "Azure Site Recovery: Vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln beskrivs vanliga frågor om Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5cdc4bcd-b4fe-48c7-8be1-1db39bd9c078
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/22/2017
ms.author: raynew
ms.openlocfilehash: 6d0bd2475466e5745e1f084bd2267d954d624ebd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site Recovery: Vanliga frågor och svar (FAQ)
Den här artikeln innehåller vanliga frågor och svar om Azure Site Recovery. Om du har frågor när du har läst den här artikeln kan publicera dem på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## <a name="general"></a>Allmänt
### <a name="what-does-site-recovery-do"></a>Vad gör Site Recovery?
Site Recovery bidrar tooyour affärskontinuitet och haveriberedskap (BCDR), genom att samordna och automatisera replikering av virtuella Azure-datorer mellan regioner, lokala virtuella datorer och fysiska servrar tooAzure och lokala datorer tooa sekundärt datacenter. [Läs mer](site-recovery-overview.md).

### <a name="what-can-site-recovery-protect"></a>Vad kan Site Recovery skydda?
* **Virtuella Azure-datorer**: Site Recovery kan replikera alla arbetsbelastningar som körs på en stöds Azure VM
* **Hyper-V-datorer**: Site Recovery kan skydda alla arbetsbelastningar som körs på en Hyper-V virtuell dator.
* **Fysiska servrar**: Site Recovery kan skydda fysiska servrar som kör Windows eller Linux.
* **Virtuella VMware-datorer**: Site Recovery kan skydda alla arbetsbelastningar som körs i ett VMware-VM.

### <a name="does-site-recovery-support-hello-azure-resource-manager-model"></a>Stöder Site Recovery hello Azure Resource Manager-modellen?
Site Recovery är tillgänglig i hello Azure-portalen med stöd för Resource Manager. Site Recovery stöder äldre distributioner i hello klassiska Azure-portalen. Du kan inte skapa nya valv i hello klassiska portalen och nya funktioner stöds inte.

### <a name="can-i-replicate-azure-vms"></a>Kan jag replikera virtuella datorer i Azure?
Ja, kan du replikera virtuella datorer som stöds Azure mellan Azure-regioner. [Läs mer](site-recovery-azure-to-azure.md).

### <a name="what-do-i-need-in-hyper-v-tooorchestrate-replication-with-site-recovery"></a>Vad behöver jag i Hyper-V tooorchestrate replikeringen med Site Recovery?
För hello Hyper-V-värdservern beror vad du behöver på hello distributionsscenariot. Checka ut hello Hyper-V-krav i:

* [Replikera virtuella Hyper-V-datorer (utan VMM) tooAzure](site-recovery-hyper-v-site-to-azure.md)
* [Replikera virtuella Hyper-V-datorer (med VMM) tooAzure](site-recovery-vmm-to-azure.md)
* [Replikera virtuella Hyper-V-datorer tooa sekundärt datacenter](site-recovery-vmm-to-vmm.md)
* Om du replikerar tooa sekundära datacenter Läs om [operativsystem som stöds för Hyper-V virtuella datorer](https://technet.microsoft.com/library/mt126277.aspx).
* Om du replikerar tooAzure Site Recovery stöder alla hello gästoperativsystem som är [stöds av Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kan jag skydda virtuella datorer när Hyper-V körs på ett klientoperativsystem?
Nej, virtuella datorer måste finnas på en Hyper-V-värdserver som körs på en Windows-serverdator som stöds. Om du behöver tooprotect en klientdator kan du replikera den som en fysisk dator för[Azure](site-recovery-vmware-to-azure.md) eller en [sekundärt datacenter](site-recovery-vmware-to-vmware.md).

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Vilka arbetsbelastningar kan jag skydda med Site Recovery?
Du kan använda Site Recovery tooprotect de flesta arbetsbelastningar som körs på en stöds VM eller fysiska servrar. Site Recovery har stöd för Programmedveten replikering så att appar kan återställas tooan intelligent tillstånd. Den kan integreras med Microsoft-program som SharePoint, Exchange, Dynamics, SQL Server och Active Directory och användas tillsammans med ledande, inklusive Oracle, SAP, IBM och Red Hat. [Lär dig mer](site-recovery-workload.md) om arbetsbelastningsskydd.

### <a name="do-hyper-v-hosts-need-toobe-in-vmm-clouds"></a>Behöver Hyper-V-värdar toobe i VMM-moln?
Om du vill tooreplicate tooa sekundärt datacenter, Hyper-V-datorer måste vara på Hyper-V är värd för servrar som finns i ett VMM-moln. Om du vill tooreplicate tooAzure, kan du replikera virtuella datorer på Hyper-V-värdservrar med eller utan VMM-moln. [Läs mer](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Kan jag distribuera Site Recovery med VMM om jag bara har en VMM-server?

Ja. Du kan antingen replikera virtuella datorer i Hyper-V-servrar i hello VMM molnet tooAzure eller kan du replikera mellan VMM-moln på hello samma server. För lokal tooon lokal replikering rekommenderar vi att du har en VMM-server i båda hello primära och sekundära platser.  

### <a name="what-physical-servers-can-i-protect"></a>Vilka fysiska servrar kan jag skydda?
Du kan replikera fysiska servrar som kör Windows och Linux tooAzure eller tooa sekundär plats. [Lär dig mer om](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) krav på operativsystem.  hello samma krav gäller oavsett om du replikerar fysiska servrar tooAzure eller tooa sekundär plats.


Observera att fysiska servrar körs som virtuella datorer i Azure om lokal server kraschar. Återställning efter fel tooan lokal fysisk server inte stöds för närvarande. För en dator som skyddas som fysiska kan du endast återställning tooa virtuell VMware-dator.

### <a name="what-vmware-vms-can-i-protect"></a>Vilka virtuella VMware-datorer kan jag skydda?

tooprotect virtuella VMware-datorer måste en vSphere-hypervisor och virtuella datorer som kör VMware-verktyg. Vi rekommenderar också att du har en VMware vCenter server toomanage hello hypervisorer. [Lär dig mer](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) om de exakta kraven för att replikera VMware-servrar och virtuella datorer tooAzure eller tooa sekundär plats.


### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan jag hantera haveriberedskap för mina avdelningskontor med Site Recovery?
Ja. När du använder Site Recovery tooorchestrate replikering och redundans på avdelningskontor, får du en enhetlig orchestration och visning av alla arbetsbelastningar på en central plats. Du kan enkelt köra redundansväxlingar och administrera haveriberedskap för alla avdelningskontor från huvudkontoret, utan att besöka hello filialer.

## <a name="pricing"></a>Prissättning

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>Vilka avgifter utsätts för när du använder Azure Site Recovery?
När du använder Site Recovery kan betalar du avgifter för hello Site Recovery-licens, Azure storage, lagringstransaktioner och utgående dataöverföring. [Läs mer](https://azure.microsoft.com/pricing/details/site-recovery).

hello Site Recovery-licens kan per skyddad instans, där en instans är en virtuell dator eller en fysisk server.

- Om en virtuell disk replikeras tooa standardlagringskonto, är hello Azure storage kostnad för hello användningen av lagringsutrymme. Till exempel hello källa diskens storlek är 1 TB och 400 GB används i Azure Site Recovery skapas en 1 TB VHD, men hello lagringen debiteras är 400 GB (plus hello mängden lagringsutrymme som används för replikeringsloggar).
- Om en virtuell disk replikeras tooa premiumlagringskonto, är hello Azure storage kostnad för hello etablerats lagringsstorlek avrundat för hello närmsta premium lagringsalternativ för disken. Till exempel hello källa diskens storlek är 50 GB, skapar Site Recovery en disk på 50 GB i Azure och Azure mappar den här toohello närmsta premium storage disk (P10).  Kostnader beräknas på P10 och inte på diskstorleken för hello 50 GB.  [Läs mer](https://aka.ms/premium-storage-pricing).  Om du använder premiumlagring krävs också ett standardlagringskonto för replikering loggning och hello mängden standard lagringsutrymme som används för dessa loggar också debiteras.
- Inga diskar skapas till ett redundanstest eller en växling vid fel. I hello replikeringstillståndet lagring avgifter under hello kategori ”sidblob och disk” enligt hello [prisnivå lagringsberäknaren](https://azure.microsoft.com/en-in/pricing/calculator/) uppkommer. Dessa debiteringar baseras på hello lagringstyp för premium-standarden och hello dataredundans Skriv - LRS, GRS RA-GRS osv.
- Om hello alternativet toouse hanterade diskar på en växling vid fel är markerad, [avgifter för hanterade diskar](https://azure.microsoft.com/en-in/pricing/details/managed-disks/) användas efter en växling vid fel för växling vid fel och testning. Hanterade diskar avgifter inte gäller vid replikering.
- Om hello alternativet toouse hanterade diskar på en växling vid fel inte är markerad lagring avgifter under hello kategori ”sidblob och disk” enligt hello [prisnivå lagringsberäknaren](https://azure.microsoft.com/en-in/pricing/calculator/) uppkommit efter växling vid fel. Dessa debiteringar baseras på hello lagringstyp för premium-standarden och hello dataredundans Skriv - LRS, GRS RA-GRS osv.
- Lagringstransaktioner debiteras under stabilitet och för normal drift för VM efter en växling vid fel eller testa redundans. Men dessa tillägg är försumbar.

Också är kostnader under testning av redundans, där hello VM, lagring, utgång och lagring transaktioner kostnader tillämpas.



## <a name="security"></a>Säkerhet
### <a name="is-replication-data-sent-toohello-site-recovery-service"></a>Skickas replikeringsdata toohello Site Recovery-tjänsten?
Nej, Site Recovery fånga upp inte replikerade data och har inte någon information om vad som körs på virtuella datorer eller fysiska servrar.
Replikeringsdata utbyts mellan lokala Hyper-V-värdar, VMware-hypervisorer eller fysiska servrar och Azure-lagring eller din sekundära plats. Site Recovery har ingen möjlighet toointercept data. Endast hello metadata behövs tooorchestrate replikering och redundans skickas toohello Site Recovery-tjänsten.  

Site Recovery är 27001: ISO 2013, 27018, HIPAA, DPA certifierad och pågår hello SOC2 och FedRAMP JAB-utvärderingar.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-hello-same-geographic-region-can-site-recovery-help-us"></a>Av kompatibilitetsskäl, även våra lokala metadata måste vara inom hello samma geografiska region. Site Recovery kan hjälpa oss?
Ja. När du skapar ett Site Recovery-valv i en region, kontrollera att alla metadata som vi behöver tooenable och samordna replikering och redundans finns kvar i regionen är geografisk gräns.

### <a name="does-site-recovery-encrypt-replication"></a>Krypterar Site Recovery replikering?
För virtuella datorer och fysiska servrar stöds replikering mellan lokala platser kryptering under överföring. För virtuella datorer och fysiska servrar som replikerar tooAzure, både kryptering under överföring och [kryptering i vila (i Azure)](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) stöds.

## <a name="replication"></a>Replikering

### <a name="can-i-replicate-over-a-site-to-site-vpn-tooazure"></a>Kan jag replikera via ett plats-till-plats VPN-tooAzure?
Azure Site Recovery replikerar data tooan Azure storage-konto via en offentlig slutpunkt. Replikering är inte via ett plats-till-plats-VPN. Du kan skapa en plats-till-plats-VPN med Azure-nätverk. Detta stör inte Site Recovery replikering.

### <a name="can-i-use-expressroute-tooreplicate-virtual-machines-tooazure"></a>Kan jag använda ExpressRoute tooreplicate virtuella datorer tooAzure?
Ja, ExpressRoute kan vara används tooreplicate tooAzure för virtuella datorer. Azure Site Recovery replikerar data tooan Azure Storage-konto via en offentlig slutpunkt. Du behöver tooset in [offentlig peering](../expressroute/expressroute-circuit-peerings.md#public-peering) toouse ExpressRoute för Site Recovery replikering. När hello virtuella datorer har tagits redundansväxlas tooan virtuella Azure-nätverket kan du komma åt dem med hjälp av hello [privat peering](../expressroute/expressroute-circuit-peerings.md#private-peering) installation med hello virtuella Azure-nätverket.

### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-tooazure"></a>Finns det några krav för att replikera virtuella datorer tooAzure?
Virtuella datorer som du vill tooreplicate tooAzure måste uppfylla [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-tooazure"></a>Kan jag replikera tooAzure för Hyper-V generation 2 virtuella datorer?
Ja. Site Recovery konverterar från generation 2 toogeneration 1 under växling vid fel. Vid återställningen är hello datorn konverterade tillbaka toogeneration 2. [Läs mer](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-tooazure-how-do-i-pay-for-azure-vms"></a>Om jag replikerar tooAzure hur betalar jag för virtuella Azure-datorer?
Data är replikerade toogeo redundant Azure-lagring och du behöver inte toopay belasta Azure IaaS-virtuella datorn att tillhandahålla en stor fördel vid vanlig replikering. När du kör en växling vid fel tooAzure Site Recovery automatiskt skapar Azure IaaS-virtuella datorer och efter det att du faktureras för hello beräkningsresurser som du använder i Azure.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Kan jag automatisera Site Recovery-scenarier med en SDK?
Ja. Du kan automatisera Site Recovery arbetsflöden med hjälp av hello Rest-API, PowerShell eller hello Azure SDK. Scenarier som stöds för närvarande för att distribuera Site Recovery med hjälp av PowerShell:

* [Replikera virtuella Hyper-V-datorer i VMMs moln tooAzure PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
* [Replikera virtuella Hyper-V-datorer utan VMM tooAzure PowerShell Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)

### <a name="if-i-replicate-tooazure-what-kind-of-storage-account-do-i-need"></a>Om jag replikerar tooAzure vilken typ av lagringskonto behöver jag?
* **Klassiska Azure-portalen**: Om du distribuerar Site Recovery i hello klassiska Azure-portalen, behöver du en [geo-redundant standardlagringskonto](../storage/common/storage-redundancy.md#geo-redundant-storage). Premium-lagring stöds inte för närvarande. hello-kontot måste vara i hello samma region som hello Site Recovery-valvet.
* **Azure-portalen**: Om du distribuerar Site Recovery i hello Azure-portalen, behöver du ett LRS eller GRS-lagringskonto. Vi rekommenderar GRS så att data är flexibla om ett regionalt strömavbrott, eller om hello primära region inte kan återställas. hello-kontot måste vara i hello samma region som hello Recovery Services-valvet. Premium-lagring har nu stöd för VMware VM, Hyper-V virtuell och fysisk server replication när du distribuerar Site Recovery i hello Azure-portalen.

### <a name="how-often-can-i-replicate-data"></a>Hur ofta kan jag replikera data?
* **Hyper-V:** Hyper-V virtuella datorer kan replikeras var 30 sekunder (förutom för premium-lagring), 5 minuter eller 15 minuter. Om du har konfigurerat SAN-replikering är replikeringen synkron.
* **VMware och fysiska servrar:** Replikeringsfrekvensen är irrelevant här. Replikeringen är kontinuerlig.

### <a name="can-i-extend-replication-from-existing-recovery-site-tooanother-tertiary-site"></a>Kan jag utöka replikeringen från den befintliga platsen tooanother tertiär återställningsplatsen?
Utökad eller länkad replikering stöds inte. Begär den här funktionen i [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-replication-hello-first-time-i-replicate-tooazure"></a>Kan jag göra en offlinereplikering hello första gången jag replikerar tooAzure?
Det stöds inte. Begär den här funktionen i hello [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan jag undanta specifika diskar från replikeringen?
Detta stöds när du är [replikera virtuella VMware-datorer och datorer med Hyper-V](site-recovery-exclude-disk.md) tooAzure, med hjälp av hello Azure-portalen.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan jag replikera virtuella datorer med dynamiska diskar?
Dynamiska diskar stöds vid replikering av virtuella Hyper-V-datorer. De stöds också vid replikering av virtuella VMware-datorer och fysiska datorer tooAzure. Hej operativsystemdisk måste vara en standarddisk.

### <a name="can-i-add-a-new-machine-tooan-existing-replication-group"></a>Kan jag lägga till en ny dator tooan befintlig replikeringsgrupp?
Lägga till nya datorer tooexisting replikeringsgrupper stöds. toodo så Välj hello replikeringsgrupp (från bladet ”replikerade objekt”) och högerklicka på/Välj snabbmenyn på hello replikeringsgrupp och välj hello alternativ.

![Lägg till tooreplication grupp](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Kan jag begränsa bandbredden som tilldelas för Hyper-V-replikeringstrafik?
Ja. Du kan läsa mer om hur du begränsar bandbredd i hello distribution artiklar:

* [Kapacitetsplanering för att replikera virtuella VMware-datorer och fysiska servrar](site-recovery-plan-capacity-vmware.md)
* [Kapacitetsplanering för att replikera virtuella Hyper-V-datorer i VMM-moln](site-recovery-vmm-to-azure.md#capacity-planning)
* [Kapacitetsplanering för replikering av Hyper-V virtuella datorer utan VMM](site-recovery-hyper-v-site-to-azure.md)

## <a name="failover"></a>Redundans
### <a name="if-im-failing-over-tooazure-how-do-i-access-hello-azure-virtual-machines-after-failover"></a>Om jag inte att redundansväxla tooAzure, hur kommer jag åt hello Azure virtuella datorer efter redundans?
Du kan komma åt hello Azure virtuella datorer via en säker Internetanslutning, via ett plats-till-plats-VPN eller över Azure ExpressRoute. Du behöver tooprepare ett antal saker i ordning tooconnect. [Läs mer](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)


### <a name="if-i-fail-over-tooazure-how-does-azure-make-sure-my-data-is-resilient"></a>Om jag växla över tooAzure hur ser Azure till att Mina data är flexibla?
Azure är utformat med flexibilitet i fokus. Site Recovery är redan utformad för växling vid fel tooa sekundära Azure-datacenter, i enlighet med hello Azure-serviceavtalet om hello behöver uppstår. Om detta händer ser vi till att dina metadata och valv finns kvar i hello samma geografiska region som du valde för ditt valv.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Om jag replikerar mellan två Datacenter vad händer om min primära datacenter uppstår ett oväntat avbrott?
Du kan utlösa en oplanerad redundans från hello sekundär plats. Site Recovery behöver ingen anslutning från hello primär plats tooperform hello växling vid fel.

### <a name="is-failover-automatic"></a>Sker redundansväxlingen automatisk?
Den sker inte automatiskt. Du startar redundansväxlingar med en enda klickning i hello-portalen eller använda [Site Recovery PowerShell](/powershell/module/azurerm.siterecovery) tootrigger en växling vid fel. Återställas är en enkel åtgärd i hello Site Recovery-portalen.

tooautomate som du kan använda lokala Orchestrator eller Operations Manager toodetect fel på virtuella datorer och sedan utlösaren hello redundans med hello SDK.

* [Läs mer](site-recovery-create-recovery-plans.md) om återställningsplaner.
* [Läs mer](site-recovery-failover.md) om redundans.
* [Läs mer](site-recovery-failback-azure-to-vmware.md) om misslyckas säkerhetskopiera virtuella VMware-datorer och fysiska servrar

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-tooa-different-host"></a>Om min lokala värden inte svarar eller krasch, kan jag redundans tillbaka tooa olika värden?
Ja, du kan använda hello alternativ plats recovery toofailback tooa olika värden från Azure. Läs mer om alternativen för hello i hello länkarna nedan för VMware och Hyper-v virtuella datorer.

* [För virtuella VMware-datorer](site-recovery-how-to-failback-azure-to-vmware.md#fail-back-to-the-original-or-alternate-location)
* [För Hyper-v-datorer](site-recovery-failback-from-azure-to-hyper-v.md#failback-to-an-alternate-location)

## <a name="service-providers"></a>Leverantörer
### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Jag är en tjänstprovider. Fungerar Site Recovery för dedikerade och delade infrastrukturmodeller?
Ja, Site Recovery stöder både dedikerade och delade infrastrukturmodeller.

### <a name="for-a-service-provider-is-hello-identity-of-my-tenant-shared-with-hello-site-recovery-service"></a>För en tjänstprovider är hello identiteten för min klient som delas med hello Site Recovery-tjänsten?
Nej. Klient-ID är anonym. Klienterna behöver inte åtkomst toohello Site Recovery-portalen. Endast hello tjänstproviderns administratör interagerar med hello-portalen.

### <a name="will-tenant-application-data-ever-go-tooazure"></a>Kommer klient programdata någonsin tooAzure?
När du replikerar mellan platser ägs av tjänstproviders hamnar programdata aldrig hos tooAzure. Data krypteras under överföring och replikeras direkt mellan tjänstproviderns platser hello.

Om du replikerar tooAzure skickas programdata tooAzure lagring men inte toohello Site Recovery-tjänsten. Data krypteras under överföringen, och förblir krypterade i Azure.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Får mina klienter en faktura för några Azure-tjänster?
Nej. Azures faktureringsrelation sker direkt med hello-leverantör. Tjänstprovidern ansvarar för att skapa fakturor för sina klienter.

### <a name="if-im-replicating-tooazure-do-we-need-toorun-virtual-machines-in-azure-at-all-times"></a>Om jag replikerar tooAzure, behöver vi toorun virtuella datorer i Azure hela tiden?
Nej, är Data replikerade tooan Azure storage-konto i din prenumeration. När du utför ett redundanstest (DR-test) eller en riktig redundansväxling skapar Site Recovery automatiskt virtuella datorer i din prenumeration.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-tooazure"></a>Garanterar ni isolering klient på klientnivå när jag replikerar tooAzure?
Ja.

### <a name="what-platforms-do-you-currently-support"></a>Vilka plattformar stöds för närvarande?
Vi stöder Azure Pack, Cloud Platform System och System Center-baserade distributioner (2012 och senare). [Lär dig mer](https://technet.microsoft.com/library/dn850370.aspx) om Azure-paket och Site Recovery-integration.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Finns det stöd för distributioner med ett enda Azure Pack och en enda VMM-server?
Ja, du kan replikera Hyper-V virtuella datorer tooAzure eller mellan en tjänstproviders platser.  Observera att om du replikerar mellan en tjänstproviders platser Azure runbook-integrering är inte tillgänglig.

## <a name="next-steps"></a>Nästa steg
* Läs hello [Site Recovery-översikten](site-recovery-overview.md)
* Lär dig mer om [Site Recovery-arkitekturen](site-recovery-components.md)  
