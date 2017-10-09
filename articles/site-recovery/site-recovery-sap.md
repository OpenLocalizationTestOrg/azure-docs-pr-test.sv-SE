---
title: "aaaProtect en flera nivåer SAP NetWeaver programdistribution med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprotect SAP NetWeaver programdistributioner med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: manayar
ms.openlocfilehash: 34651c7b14d23a44005372f4f923c401e0224231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Skydda en flera nivåer SAP NetWeaver programdistribution med hjälp av Azure Site Recovery

De flesta distributioner för små och medelstora SAP har någon form av Disaster Recovery-lösning.  hello vikten av robust och kan testas Disaster Recovery-lösningar har ökat som flera kärnor business processer flyttade tooapplications, till exempel SAP.  Azure Site Recovery har testats och är integrerad med SAP-program och överskrider hello funktioner i de flesta lokala Disaster Recovery-lösningar på en lägre ägandekostnader (TCO) än konkurrerande lösningar.
Med Azure Site Recovery kan du:
* Aktivera skydd för SAP NetWeaver och icke - NetWeaver produktion program som körs lokalt, genom att replikera komponenter tooAzure.
* Aktivera skydd för SAP NetWeaver och icke - NetWeaver produktion program som körs Azure, genom att replikera komponenter tooanother Azure-datacenter.
* Förenkla molnmigreringen, genom att använda Site Recovery toomigrate tooAzure din SAP-distribution.
* Förenkla SAP-projektuppgradering, testning och prototyper genom att skapa en produktionsklon på begäran för att testa SAP-program.

Den här artikeln beskriver hur tooprotect SAP NetWeaver programdistributioner med [Azure Site Recovery](site-recovery-overview.md). Den här artikeln beskriver hello Metodtips för att skydda en distribution med tre nivåer SAP NetWeaver i Azure genom att replikera tooanother Azure datacenter med hjälp av Azure Site Recovery hello stöds scenarier och konfigurationer och hur tooperform redundans både testa växling vid fel (haveriberedskap) och faktiska växling vid fel.


## <a name="prerequisites"></a>Krav
Innan du börjar, kontrollera att du förstår hello följande:

1. [Replikera en virtuell dator tooAzure](azure-to-azure-walkthrough-enable-replication.md)
2. Hur för[utforma ett nätverk för återställning](site-recovery-azure-to-azure-networking-guidance.md)
3. [Gör en testa redundans tooAzure](azure-to-azure-walkthrough-test-failover.md)
4. [Gör en tooAzure för växling vid fel](site-recovery-failover.md)
5. Hur för[replikera en domänkontrollant](site-recovery-active-directory.md)
6. Hur för[replikera SQL Server](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Scenarier som stöds
Med Azure Site Recovery kan du implementera en lösning för katastrofåterställning för hello följande scenarier:
* SAP-system som körs i en Azure-datacenter som replikerar tooanother Azure-datacenter (Azure till Azure DR) som konstruerad [här](https://aka.ms/asr-a2a-architecture).
* SAP-system som körs på VMWare (eller fysisk) servrar lokalt replikering tooa DR-plats i ett Azure-datacenter (VMware till Azure DR) som kräver vissa ytterligare komponenter som konstruerad [här](https://aka.ms/asr-v2a-architecture).
* SAP-system som kör Hyper-V lokalt replikering tooa DR-plats i ett Azure-datacenter (Hyper-V-till-Azure DR) som kräver vissa ytterligare komponenter som konstruerad [här](https://aka.ms/asr-h2a-architecture).

Det här dokumentet använder hello första case - Azure till Azure DR - toodemonstrate Azure Site Recovery's SAP återställningskapacitet. Azure Site Recovery replikering är oberoende av programmet, är hello process som beskrivs förväntade toohold för samt andra scenarier.

### <a name="required-foundation-services"></a>Nödvändiga foundation-tjänster
Scenariot dokumentationen alla har distribuerats med hello följa distribuerade foundation-tjänster:
* ExpressRoute eller plats-till-plats virtuellt privat nätverk (VPN)
* Minst en Active Directory-domänkontrollant och DNS-server som körs i Azure

Du rekommenderas att hello-infrastrukturen ovan är etablerade tidigare toodeploying Azure Site Recovery.


## <a name="typical-sap-application-deployment"></a>Vanliga SAP-programdistribution
Stora SAP-kunder distribuera vanligtvis mellan 6 too20 enskilda SAP-program.  De flesta av dessa program är baserade på hello SAP NetWeaver ABAP eller Java motorer.  Stöd för programmen core NetWeaver är många mindre specifika icke - NetWeaver SAP fristående motorer och vanligtvis vissa icke-SAP-program.  

Det är kritiska tooinventory alla hello SAP-program körs i ett liggande och toodetermine hello distributionsläget (nivå 2 eller 3-skikts), versioner, korrigeringsfiler, storlek, omsättningsuppdateringar frekvens och beständiga diskkrav.

![Mönster för distribution](./media/site-recovery-sap/sap-typical-deployment.png)

hello SAP-databasen beständiga layer borde vara skyddad via hello interna DBMS-verktyg som SQL Server AlwaysOn, Oracle DataGuard eller HANA System replikering. hello klientnivå också skyddas av Azure Site Recovery, men det är viktigt tooconsider ämnen som påverkar detta skikt som DNS-spridningen fördröjning, säkerhet och fjärråtkomst toohello DR-datacenter.

Azure Site Recovery kan hello rekommenderade lösningen för hello programnivå, inklusive (A) SCS. Andra program som utgör en del av hello NetWeaver SAP-program och icke-SAP-program övergripande SAP-distributionsmiljö och bör också skyddas med Azure Site Recovery.

## <a name="replicate-virtual-machines"></a>Replikera virtuella datorer
Följ [vägledningen](azure-to-azure-walkthrough-enable-replication.md) toostart replikering av alla hello SAP programmet virtuella datorer toohello Azure DR-datacenter.

Om du använder en statisk IP-adress kan du ange hello IP-adress som du vill hello virtuella tootake under hello Network interface kort i beräknings-och nätverksinställningarna.

![Mål-IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Skapa en återställningsplan
En återställningsplan kan ordningsföljd hello växling vid fel på olika nivåer i en flernivåapp, därför kan upprätthålla programkonsekvens. Följ stegen hello [här](site-recovery-create-recovery-plans.md) när du skapar en återställningsplan för ett webbprogram med flera nivåer.

### <a name="adding-scripts-toohello-recovery-plan"></a>Lägga till skript toohello återställningsplan
Du kan behöva toodo vissa åtgärder på hello Azure virtuella datorer efter redundans/testa redundans för ditt program toofunction korrekt. Du kan automatisera hello efter redundansväxlingen, till exempel uppdatera DNS-post och ändra bindningar och anslutningar, genom att lägga till motsvarande skript i hello återställningsplan enligt beskrivningen i [i den här artikeln](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>DNS-uppdatering
Om hello DNS har konfigurerats för dynamisk DNS-uppdatering och virtuella datorer vanligtvis uppdatera hello DNS med nya IP-hello när de startar. Om du vill tooadd explicit steg-tooupdate DNS med hello nya IP-adresser hello virtuella datorer lägger du till detta [skript tooupdate IP-Adressen i DNS-](https://aka.ms/asr-dns-update) som en post-åtgärd på recovery planeringsgrupper.  

## <a name="example-azure-to-azure-deployment"></a>Exempel på Azure-Azure-distribution
I hello diagrammet nedan hello Azure Site Recovery Azure till Azure katastrofåterställning visas:
* hello primära Datacenter är i Singapore (Azure sydöstra Asien) och hello DR datacenter är Hongkong (Azure Östasien).  I det här scenariot som lokal hög tillgänglighet har två virtuella datorer som kör SQL Server AlwaysOn i synkront läge i Singapore.
* hello filen resursen ASCS kan vara används tooprovide hög tillgänglighet för hello SAP enskilda felpunkter. File Share ASCS kräver inte en delad klusterdisk och program, till exempel SIOS krävs inte.
* DR-skydd för hello DBMS layer uppnås med hjälp av asynkron replikering.
* Det här scenariot visar ”symmetrisk DR” – en term som används toodescribe en DR-lösning som är en exakt replik av produktion, därför hello DR SQL Server-lösning har lokal hög tillgänglighet. hello användning av symmetrisk DR är inte obligatoriskt för hello databasen lager och många kunder utnyttja snabbt hello flexibilitet i molnet distributioner toobuild en lokal nod för hög tillgänglighet efter en DR-händelse.
* hello diagram visar hello SAP NetWeaver ASCS- och programnivå server som replikeras av Azure Site Recovery.

![Scenario för replikering](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Gör ett redundanstest
Följ [vägledningen](azure-to-azure-walkthrough-test-failover.md) toodo testa redundans.

1.  Gå tooAzure portal och välj Recovery Services-valvet.
2.  Klicka på hello återställningsplan som skapats för SAP-program (s).
3.  Klicka på Testa redundans.
4.  Välj återställningspunkten och den virtuella Azure-nätverket toostart hello test failover-processen.
5.  Du kan utföra din verifieringar när hello sekundära miljön är upp.
6.  När hello verifieringar har slutförts, klicka på 'Rensa testa redundans- och tooclean hello redundans miljö.

## <a name="doing-a-failover"></a>Genomför en redundansväxling enligt
Följ [vägledningen](site-recovery-failover.md) när du gör en redundansväxling.

1.  Gå tooAzure portal och välj Recovery Services-valvet.
2.  Klicka på hello återställningsplan som skapats för SAP-program.
3.  Klicka på 'Redundans'.
4.  Välj återställningsprocessen punkt toostart hello växling vid fel.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om hur du skapar en lösning för katastrofåterställning för SAP NetWeaver distributioner med hjälp av Azure Site Recovery i [rapporten](http://aka.ms/asr-sap). hello whitepaper diskuteras också rekommendationer för olika arkitekturer för SAP, visar vilka program som stöds och VM-typer för SAP på Azure och beskriver möjliga testplaner för din lösning för katastrofåterställning.

Lär dig mer om [replikerar andra arbetsbelastningar](site-recovery-workload.md) med Site Recovery.
