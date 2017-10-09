---
title: "aaaPrerequisites för replikering tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Läs mer om hello krav för att replikera virtuella datorer och fysiska datorer tooAzure med hjälp av hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: rajanaki
ms.openlocfilehash: 0e32ab7cd7c65a3f67ffa2f2c15af189c15b6f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-tooazure-by-using-site-recovery"></a>Krav för replikeringen från lokala tooAzure genom att använda Site Recovery

> [!div class="op_single_selector"]
> * [Replikera från Azure tooAzure](site-recovery-azure-to-azure-prereq.md)
> * [Replikera från lokala tooAzure](site-recovery-prereq.md)

Azure Site Recovery kan hjälpa dig att uppfylla din affärskontinuitet och haveriberedskap (BCDR) strategi genom att samordna replikeringen av en virtuell Azure-dator (VM) tooanother Azure-region. Site Recovery replikerar också lokala fysiska servrar och virtuella datorer toohello molnet (Azure) eller tooa sekundärt datacenter. Om ett avbrott uppstår på den primära platsen, kan du växla över tooa sekundär plats tookeep appar och arbetsbelastningar som är tillgängliga. När hello primär plats returnerar toonormal åtgärder kan du växla tillbaka tooyour primära platsen. Mer information om Platsåterställning finns [vad är Site Recovery?](site-recovery-overview.md).

I den här artikeln sammanfattar vi hello krav för börjar Site Recovery replikering från en lokal dator tooAzure.

Du kan skicka kommentarer längst ned hello hello artikel. Du kan också begära tekniska frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Krav för Azure

**Krav** | **Detaljer**
--- | ---
**Azure-konto** | En [Microsoft Azure-konto](http://azure.microsoft.com/). Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
**Site Recovery-tjänsten** | Mer information om priser för hello Azure Site Recovery-tjänsten finns [priserna för Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage** | Du behöver ett Azure Storage-konto toostore replikerade data. hello storage-konto måste vara i hello samma region som hello Azure Recovery Services-valvet. Virtuella Azure-datorer skapas när redundansväxlingen.<br/><br/> Beroende på hello resursmodell önskade toouse för Virtuella Azure-redundans, kan du konfigurera ett konto med hjälp av hello [Azure Resource Manager-distributionsmodellen](../storage/common/storage-create-storage-account.md) eller hello [klassiska distributionsmodellen](../storage/common/storage-create-storage-account.md).<br/><br/>Du kan använda [geo-redundant lagring](../storage/common/storage-redundancy.md#geo-redundant-storage) eller lokalt redundant lagring. Vi rekommenderar geo-redundant lagring. Data är flexibla med geo-redundant lagring, om ett regionalt strömavbrott, eller om hello primära region inte kan återställas.<br/><br/> Du kan använda Azure storage-standardkonto eller du kan använda Azure Premium-lagring. [Premium-lagring](https://docs.microsoft.com/azure/storage/storage-premium-storage) används vanligtvis för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg latens. Med premium-lagring, en virtuell dator kan vara värd för I/O-intensiva arbetsbelastningar. Om du använder premiumlagring för replikerade data behöver du även ett standardlagringskonto. Ett standardlagringskonto lagrar replikeringsloggar som samlar in löpande ändringar tooon lokala data.<br/><br/>
**Begränsningar för lagring** | Du kan inte flytta storage-konton som du använder i Site Recovery tooa annan resursgrupp eller flytta tooor användning med en annan prenumeration.<br/><br/> För närvarande replikerar toopremium storage-konton i centrala Indien och södra Indien är inte tillgänglig.
**Azure-nätverk** | Du behöver ett Azure-nätverk, toowhich virtuella Azure-datorer ansluta efter växling vid fel. hello Azure nätverket måste finnas i hello samma region som hello Recovery Services-valvet.<br/><br/> I hello Azure-portalen, kan du skapa ett Azure-nätverk med hjälp av hello [Resource Manager-distributionsmodellen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) eller hello [klassiska distributionsmodellen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> Om du replikerar från System Center Virtual Machine Manager (VMM) tooAzure måste du konfigurera nätverksmappning mellan VMM VM-nätverk och Azure-nätverk. Detta säkerställer att virtuella datorer i Azure ansluter tooappropriate nätverk efter redundansväxling.
**Begränsningar för** | Du kan inte flytta nätverkskonton som du använder i Site Recovery tooa annan resursgrupp eller flytta tooor användning med en annan prenumeration.
**Nätverksmappning** | Om du replikerar Microsoft Hyper-V virtuella datorer i VMM-moln, måste du konfigurera nätverksmappning. Detta säkerställer att virtuella Azure-datorer ansluta tooappropriate nätverk när de skapas efter växling vid fel.

> [!NOTE]
> hello beskrivs följande avsnitt hello krav för olika delar av hello kundmiljö. Mer information om stöd för specifika konfigurationer finns hello [supportmatrisen](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-tooazure"></a>Återställning av virtuella VMware-datorer eller fysiska tooAzure för Windows- eller Linux-servrar
hello följande komponenter krävs för återställning av virtuella VMware-datorer eller fysiska Windows- eller Linux-servrar. Dessa dessutom är de som beskrivs i toohello [krav för Azure](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Konfigurationsservern eller ytterligare processervern

Konfigurera en lokal dator som hello configuration serverkommunikation tooorchestrate mellan hello lokal plats och Azure. hello lokal dator hanterar också datareplikering. <br/></br>

*   **Krav på VMware vCenter eller vSphere värdar**

    | **Komponent** | **Krav** |
    | --- | --- |
    | **vSphere** | Du måste ha en eller flera VMware vSphere-hypervisor-program.<br/><br/>Hypervisorer måste köra vSphere version 6.0, 5.5 eller 5.1 med hello senaste uppdateringarna.<br/><br/>Vi rekommenderar att vSphere-värdar och vCenter-servrar vara på hello samma nätverk som hello processervern. Om du har konfigurerat en dedikerad processervern är detta hello nätverk där konfigurationsservern hello finns. |
    | **vCenter** | Vi rekommenderar att du distribuerar en VMware vCenter server toomanage vSphere-värdar. Det måste köras vCenter version 6.0 eller 5.5, med hello senaste uppdateringarna.<br/><br/>**Begränsning**: Site Recovery har inte stöd för replikering mellan olika instanser av vCenter vMotion. Lagring DRS och Storage vMotion även stöds inte på Huvudmålet VM efter en skyddar åtgärd.||

* **Förutsättningar för replikerad dator**

    | **Komponent** | **Krav** |
    | --- | --- |
    | **Lokala datorer** (VMwares virtuella datorer) | Replikerade virtuella datorer måste ha VMware-verktyg installerat och körs.<br/><br/> Virtuella datorer måste uppfylla [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) för att skapa en Azure VM.<br/><br/>Diskutrymme på varje skyddad dator får inte vara mer än 1,023 GB. <br/><br/>Minst 2 GB diskutrymme på installationsenheten för hello krävs för installationen.<br/><br/>Om du vill tooenable konsekvens för flera måste du öppna port 20004 hello VM lokala brandväggen.<br/><br/>Datornamn måste vara mellan 1 och 63 tecken (du kan använda bokstäver, siffror och bindestreck). hello namn måste börja med en bokstav eller siffra och sluta med en bokstav eller siffra. <br/><br/>När du har aktiverat replikering för en dator kan ändra du hello Azure namn.<br/><br/> |
    | **Windows-datorer** (fysisk eller VMware) | hello datorn måste köra något av följande för hello stöds 64-bitars operativsystem: <br/>– Windows Server 2012 R2<br/>– Windows Server 2012<br/>-Windows Server 2008 R2 med SP1 eller senare<br/><br/> hello operativsystemet installerat på enhet C. hello OS-disk måste vara en standarddisk för Windows, och inte dynamiska. Hej datadisk kan vara dynamisk.<br/><br/>|
    | **Linux-datorer** (fysisk eller VMware) | hello datorn måste köra något av följande för hello stöds 64-bitars operativsystem: <br/>-Red Hat Enterprise Linux 7.2, 7.1, 6.8 eller 6.7<br/>-Centos 7.2, 7.1, 7.0, 6.8, 6.7, 6.6 eller 6.5<br/>-Ubuntu 14.04 LTS server (en lista över stöds kernel-versioner för Ubuntu finns [operativsystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Oracle Enterprise Linux 6.5 eller 6.4, kör antingen hello Red Hat-kompatibel kerneln eller Unbreakable Enterprise Kernel version 3 (UEK3)<br/>-SUSE Linux Enterprise Server 11 SP4 eller SUSE Linux Enterprise Server 11 SP3<br/><br/>/ Etc/hosts filer på skyddade datorer måste ha poster som mappar hello lokal värd name tooIP adresser som är associerade med alla nätverkskort.<br/><br/>Efter växling vid fel, om du vill tooconnect tooan virtuella Azure-datorn som kör Linux med hjälp av en klient med SSH (Secure Shell), kontrollera att hello SSH-tjänsten på hello skyddad dator är toostart automatiskt på datorn startas. Kontrollera också att brandväggsreglerna tillåter en SSH-anslutning toohello skyddad dator.<br/><br/>hello värdnamn, monteringspunkter, enhetsnamn, och Linux system sökvägar och filnamn (t.ex, / etc / och/usr) finnas endast på engelska.<br/><br/>hello följande kataloger (om konfigurerad som separata partitioner eller filsystem) måste vara på hello samma disk (hello OS-disk) på hello källservern:<br/>- / (rot)<br/>- / Starta<br/>-/ usr<br/>-/ usr/lokal<br/>-/var<br/>- / m.m.<br/><br/>XFS v5 funktioner, till exempel metadata kontrollsumma stöds för närvarande inte av Site Recovery på XFS filsystem. Se till att din XFS filsystem inte använder några v5-funktioner. Du kan använda hello xfs_info verktyget toocheck hello XFS superblock för hello partition. Om **ftype** har angetts för**1**, XFS v5 funktionerna används.<br/><br/>Hello lsof verktyget måste vara installerade och tillgängliga på Red Hat Enterprise Linux 7 och CentOS 7-servrar.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-tooazure-no-vmm"></a>Återställning av Hyper-V VMs tooAzure (ingen VMM)

hello följande komponenter krävs för katastrofåterställning för Hyper-V virtuella datorer i VMM-moln. Dessa dessutom är de som beskrivs i toohello [krav för Azure](#azure-requirements).

| **Krav** | **Detaljer** |
| --- | --- |
| **Hyper-V-värd** |En eller flera lokala servrar måste köra Windows Server 2012 R2 med hello senaste uppdateringarna och hello Hyper-V-rollen aktiverad eller Microsoft Hyper-V Server 2012 R2.<br/><br/>Hyper-V-servrar måste ha en eller flera virtuella datorer.<br/><br/>Hyper-V-servrar måste vara anslutna toohello Internet, antingen direkt eller via en proxyserver.<br/><br/>Hyper-V-servrar måste ha hello korrigeringar som beskrivs i Knowledge Base-artikel hello [2961977](https://support.microsoft.com/kb/2961977) installerad.
|**Provider och agent**| Under distributionen av Site Recovery kan installera du hello Azure Site Recovery-providern. Hej providerinstallationen installeras också hello Azure Recovery Services-agenten på varje Hyper-V-server som kör virtuella datorer som du vill tooprotect. <br/><br/>Alla Hyper-V-servrar i en Platsåterställning valvet måste ha hello samma versioner av hello providern och hello-agenten.<br/><br/>hello måste tooconnect tooSite återställning över hello Internet. Trafik kan skickas direkt eller via en proxyserver. HTTPS-baserade proxyservrar stöds inte. hello proxyserver måste tillåta åtkomst toohello följande URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Se till att hello reglerna tillåter kommunikation tooAzure om du har IP-adressbaserade brandväggsregler på hello-servern.<br/><br/> Tillåt hello [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och HTTPS (port 443).<br/><br/> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och hello västra USA (används för hantering av kontrollen och identitet).


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-tooazure"></a>Katastrofåterställning för Hyper-V virtuella datorer i VMM-moln tooAzure

hello följande komponenter krävs för katastrofåterställning för Hyper-V virtuella datorer i VMM-moln. Dessa dessutom är de som beskrivs i toohello [krav för Azure](#azure-requirements).

| **Krav** | **Detaljer** |
| --- | --- |
| **Virtual Machine Manager** |Du måste ha en eller flera VMM-servrar som körs på System Center 2012 R2 eller senare. Varje VMM-servern måste ha en eller flera moln har konfigurerats. <br/><br/>Ett moln måste ha:<br/>-En eller flera VMM-värdgrupper.<br/>-En eller flera servrar med Hyper-V-värd eller kluster i varje värdgrupp.<br/><br/>Läs mer om hur du konfigurerar VMM-moln, [hur toocreate ett moln i Virtual Machine Manager 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Hyper-V-värdservrar måste köra minst Windows Server 2012 R2 med hello Hyper-V-rollen aktiverad eller Microsoft Hyper-V Server 2012 R2. hello senaste uppdateringar måste installeras.<br/><br/> En Hyper-V-servern måste ha en eller flera virtuella datorer.<br/><br/> En Hyper-V-värdserver eller kluster som innehåller virtuella datorer som du vill tooreplicate måste hanteras i VMM-moln.<br/><br/>Hyper-V-servrar måste vara anslutna toohello Internet, antingen direkt eller via en proxyserver.<br/><br/>Hyper-V-servrar måste ha hello korrigeringar som beskrivs i Knowledge Base-artikel hello [2961977](https://support.microsoft.com/kb/2961977) installerad.<br/><br/>Hyper-V-värdservrar behöver Internetåtkomst för tooAzure för replikering av data. |
| **Provider och agent** |Installera Azure Site Recovery Provider på hello VMM-servern under distributionen av Azure Site Recovery. Installera Recovery Services-agenten på Hyper-V-värdar. hello-providern och agenten måste tooconnect tooAzure direkt via hello Internet eller via en proxyserver. HTTPS-baserade proxyservrar stöds inte. hello proxyservern på hello VMM-servern och Hyper-V-värdar måste tillåta åtkomst till: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Om du har IP-adressbaserade brandväggsregler på hello VMM-servern kontrollerar du att hello reglerna tillåter kommunikation tooAzure.<br/><br/> Tillåt hello [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) och HTTPS (port 443).<br/><br/>Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och hello västra USA (används för hantering av kontrollen och identitet).<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Förutsättningar för replikerad dator

| **Komponent** | **Detaljer** |
| --- | --- |
| **Skyddade virtuella datorer** | Site Recovery har stöd för alla operativsystem som stöds av [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Virtuella datorer måste uppfylla hello [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) för att skapa en Azure VM. Datornamn måste vara mellan 1 och 63 tecken (du kan använda bokstäver, siffror och bindestreck). hello namn måste börja med en bokstav eller siffra och sluta med en bokstav eller siffra. <br/><br/>Du kan ändra hello namn när du har aktiverat replikering för hello VM. <br/><br/> Diskkapacitet för varje skyddad dator får inte vara mer än 1,023 GB. En virtuell dator kan ha upp too16 diskar (upp too16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-tooa-customer-owned-site"></a>Katastrofåterställning för Hyper-V virtuella datorer i VMM-moln tooa ägs av kunden platsen

hello följande komponenter krävs för katastrofåterställning för Hyper-V virtuella datorer i VMM-moln tooa ägs av kunden platsen. Dessa dessutom är de som beskrivs i toohello [krav för Azure](#azure-requirements).

| **Komponent** | **Detaljer** |
| --- | --- |
| **Virtual Machine Manager** |  Vi rekommenderar att du distribuerar en VMM-server på både hello primär plats och hello sekundär plats.<br/><br/> Du kan [replikera mellan moln på en VMM-server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). tooreplicate mellan moln på en VMM-server behöver du minst två moln som konfigurerats på hello VMM-servern.<br/><br/> VMM-servrar måste köra minst System Center 2012 SP1 med hello senaste uppdateringarna.<br/><br/> Varje VMM-servern måste ha ett eller flera moln. Alla moln måste ha hello ange Hyper-V-kapacitetsprofilen. <br/><br/>Moln måste ha en eller flera VMM-värdgrupper. Läs mer om hur du konfigurerar VMM-moln, [förbereda för distribution av Azure Site Recovery](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Hyper-V-servrar måste köra minst Windows Server 2012 med hello Hyper-V-rollen aktiverad och ha hello senaste uppdateringarna installerade.<br/><br/> En Hyper-V-servern måste ha en eller flera virtuella datorer.<br/><br/>  Hyper-V-värdservrar måste finnas i värdgrupper i hello primära och sekundära VMM-moln.<br/><br/> Om du kör Hyper-V i ett kluster i Windows Server 2012 R2, rekommenderar vi att du installerar hello uppdateringen som beskrivs i Knowledge Base-artikel [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Om du kör Hyper-V i ett kluster på Windows Server 2012 och har en statisk IP-adress-kluster skapas klusterutjämning inte automatiskt. Du måste konfigurera hello klusterutjämning manuellt. Läs mer om hello klusterutjämning [konfigurera hello replikroll broker för replikering av kluster till kluster](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Leverantör** | Under distributionen av Site Recovery installera hello Azure Site Recovery-providern på VMM-servrar. hello providern kommunicerar med Site Recovery via HTTPS (port 443) tooorchestrate replikering. Replikering sker mellan hello primära och sekundära Hyper-V-servrar över hello LAN eller via en VPN-anslutning.<br/><br/> hello-providern som körs på hello VMM-servern måste komma åt toohello följande URL: er:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>hello Site Recovery-providern måste tillåta brandväggen kommunikation från hello VMM-servrar toohello [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och tillåta hello HTTPS (port 443)-protokollet. |


## <a name="url-access"></a>URL-åtkomst
Dessa URL: er måste vara tillgänglig från VMware, VMM och Hyper-V-värdservrar:

|**URL: EN** | **VMM tooVMM** | **VMM tooAzure** | **Hyper-V tooAzure** | **VMware tooAzure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Tillåt | Tillåt | Tillåt | Tillåt |
|``*.backup.windowsazure.com`` | Krävs inte | Tillåt | Tillåt | Tillåt |
|``*.hypervrecoverymanager.windowsazure.com`` | Tillåt | Tillåt | Tillåt | Tillåt |
|``*.store.core.windows.net`` | Tillåt | Tillåt | Tillåt | Tillåt |
|``*.blob.core.windows.net`` | Krävs inte | Tillåt | Tillåt | Tillåt |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Krävs inte | Krävs inte | Krävs inte | Tillåt för SQL-hämtning |
|``time.windows.com`` | Tillåt | Tillåt | Tillåt | Tillåt|
|``time.nist.gov`` | Tillåt | Tillåt | Tillåt | Tillåt |
