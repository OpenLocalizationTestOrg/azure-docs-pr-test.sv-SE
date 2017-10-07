---
title: aaaReplicate virtuella VMware-datorer och fysiska servrar tooAzure i hello klassiska portal | Microsoft Docs
description: "Den här artikeln beskriver hur toodeploy Azure Site Recovery tooorchestrate replikering, redundans och återställning av lokala virtuella VMware-datorer och Windows-/ Linux fysiska servrar tooAzure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a9022c1f-43c1-4d38-841f-52540025fb46
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-vmware-to-azure
ms.openlocfilehash: f85e4139ad45552ce963072e14d71d279bb7dac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-tooazure-with-azure-site-recovery"></a>Replikera virtuella VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery
> [!div class="op_single_selector"]
> * [hello Azure-portalen](site-recovery-vmware-to-azure.md)
> * [hello klassiska portalen](site-recovery-vmware-to-azure-classic.md)
> * [hello klassiska portalen (bakåtkompatibelt)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

hello Azure Site Recovery-tjänsten bidrar tooyour affärskontinuitet och haveriberedskap (BCDR) genom att samordna replikering, redundans och återställning av virtuella datorer och fysiska servrar. Datorer kan vara replikerade tooAzure eller tooa sekundärt lokalt datacenter. En snabb översikt finns [vad är Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur du:

* **Replikera VMware virtuella datorer tooAzure**: distribuera Site Recovery toocoordinate replikering, redundans och återställning av tooAzure lagring för lokal VMware virtuella datorer.
* **Replikera fysiska servrar tooAzure**: distribuera Azure Site Recovery toocoordinate replikering, redundans och återställning av lokala fysiska Windows- och Linux-servrar tooAzure.

> [!NOTE]
> Den här artikeln beskriver hur tooreplicate tooAzure. Om du vill tooreplicate virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooa sekundärt datacenter, se [Site Recovery VMware tooVMware](site-recovery-vmware-to-vmware.md).
>
>

Skriv dina kommentarer eller frågor längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Förbättrad distribution
Den här artikeln innehåller anvisningar för en förbättrad distribution i hello klassiska Azure-portalen. Vi rekommenderar att du använder den här versionen för alla nya distributioner. Om du redan har distribuerat genom att använda Hej tidigare äldre version, rekommenderar vi att du migrerar toohello ny version. Mer information om migrering finns i [Site Recovery VMware tooAzure klassisk äldre](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

hello förbättrad distribution är en större uppdatering. Här följer en sammanfattning av hello förbättringar som vi har gjort:

* **Inga virtuella datorer i Azure-infrastrukturen**: Data replikeras direkt tooan Azure storage-konto. Dessutom finns inga behov tooset konfigurerar någon infrastruktur för virtuella datorer (till exempel konfigurationsservern eller huvudmålservern) för replikering och redundans som krävs i äldre hello-distribution.  
* **Enhetlig installation**: en enda installation ger enkel installation och skalbarhet för lokala komponenter.
* **Säker distribution**: All trafik är krypterad och replikering management-meddelanden skickas via HTTPS 443.
* **Återställningspunkter**: stöd för krascher och programkonsekventa recovery punkter för Windows och Linux-miljöer och stöd för både enskilda virtuella datorn och multi-VM konsekventa konfigurationer.
* **Testa redundans**: stöd för utan avbrott testa redundans tooAzure utan att påverka tillverkningen eller pausa replikering.
* **Oplanerad växling**: stöd för oplanerad redundans tooAzure med ett avancerat alternativ tooautomatically stänga av virtuella datorer före redundans.
* **Återställning efter fel**: integrerad återställning efter fel som replikeras endast deltaändringar tillbaka toohello lokal plats.
* **vSphere 6.0**: begränsat stöd för VMware vSphere 6.0 distributioner.

## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Hur Site Recovery skyddar virtuella datorer och fysiska servrar?
* VMware-administratörer kan konfigurera externt skydd tooprotect Azure från arbetsbelastningar och program som körs på virtuella VMware-datorer. Servern chefer kan replikera fysiska, lokala Windows- och Linux-servrar tooAzure.
* hello Azure Site Recovery-konsolen tillhandahåller en enda plats för enkel installation och hantering av replikering, redundans och processer.
* Om du replikerar virtuella VMware-datorer som hanteras av en vCenter-server kan Site Recovery kan identifiera dessa virtuella datorer automatiskt. Om datorer på ESXi-värd, identifierar Site Recovery virtuella datorer på hello värden.
* Om du kör enkelt växling vid fel från din lokala infrastruktur tooAzure kan du växla tillbaka (återställa) från Azure tooVMware Virtuella servrar i hello lokal plats.
* Du kan konfigurera återställningsplaner som grupperar programbelastningar nivåer på flera datorer. Om du växlar över dessa planer tillhandahåller Site Recovery konsekvens för flera så att datorer som kör samma arbetsbelastningar kan vara hello återställas tillsammans tooa konsekvent datapunkt.

## <a name="supported-operating-systems"></a>Operativsystem som stöds
### <a name="windows-64-bit-only"></a>Windows (endast 64-bitars)
* Windows Server 2008 R2 SP1 +
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (endast 64-bitars)
* Red Hat Enterprise Linux 7.2, 6.7 och 7.1
* CentOS 6.5, 6.6, 6.7, 7.0, 7.1 och 7.2
* Oracle Enterprise Linux 6.4 och 6.5 som kör antingen hello Red Hat kompatibel kerneln eller Unbreakable Enterprise Kernel version 3 (UEK3)
* SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Scenariots arkitektur
Scenariokomponenter:

* **En lokal hanteringsserver**: hello hanteringsservern kör Site Recovery-komponenter:
  * **Konfigurationsservern**: samordnar kommunikationen och hanterar processer för replikering och återställning av data.
  * **Processervern**: fungerar som en gateway för replikering. Den tar emot data från skyddade källdatorer; optimerar dem med cachelagring, komprimering och kryptering; och skickar replikering tooAzure datalagring. Den hanterar push-installation av Mobility service tooprotected datorer och utför automatisk identifiering av virtuella VMware-datorer.
  * **Huvudmålserver**: Hanterar replikeringsdata vid återställning från Azure.
    Du kan också distribuera en hanteringsserver som fungerar bara som en process server tooscale din distribution.
* **Hej mobilitetstjänsten**: den här komponenten distribueras på varje dator (VMware VM eller fysiska server) som du vill tooreplicate tooAzure. Den samlar in dataskrivningar på hello datorn och vidarebefordrar dem toohello processervern.
* **Azure**: du behöver inte toocreate alla virtuella datorer i Azure toohandle replikering och redundans. hello Site Recovery-tjänsten hanterar datahantering och data replikeras direkt tooAzure lagring. Replikerade virtuella Azure-datorer skapas automatiskt endast när tooAzure för växling vid fel inträffar. Men om du vill toofail tillbaka från Azure toohello lokal plats måste tooset upp en virtuell dator i Azure-tooact som en processerver.

hello följande bild (skapas av Henry Robalino) visar hur komponenterna samverkar:

![Arkitektur för grupprincipkomponent](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

## <a name="capacity-planning"></a>Kapacitetsplanering
När du planerar kapaciteten är här vad du behöver toothink om:

* **hello källmiljön**: planering eller hello VMware infrastruktur- och datorn kapacitetsbehov.
* **hello hanteringsservern**: Planera för hello lokala management-servrar som kör Site Recovery-komponenter.
* **Nätverksbandbredd från källan tootarget**: planering för nätverksbandbredd krävs för replikering mellan hello käll- och Azure.

### <a name="source-environment-considerations"></a>Överväganden för käll-miljö
* **Maximal dagliga förändringstakten**: en skyddad dator kan använda endast en processerver. En enda process-server kan hantera upp too2 TB data ändra per dag. Därför är 2 TB hello maximala dagliga data ändra hastigheten som stöds för en skyddad dator.
* **Maximalt dataflöde**: en replikerad dator kan höra tooone storage-konto i Azure. Ett standardlagringskonto kan hantera upp till 20 000 begäranden per sekund och vi rekommenderar att du behåller hello antalet IOPS mellan en källa datorn too20, 000. Till exempel om du har en källdator med 5 diskar och varje disk genererar 120 IOPS (8 KB storleken) på hello källan, blir inom hello Azure per disk IOPS högst 500. Hej antalet storage-konton som krävs = totala source IOPs/20 000.

### <a name="management-server-considerations"></a>Management server-överväganden
hello hanteringsservern kör Site Recovery-komponenter som hanterar dataoptimering, replikering och hantering. Det ska vara kan toohandle hello daglig ändra frekvensen kapacitet för alla arbetsbelastningar som körs på skyddade datorer och har tillräckligt mycket bandbredd toocontinuously replikera tooAzure datalagring. Närmare bestämt:

* Hej processervern tar emot replikeringsdata från skyddade datorer och optimerar dem med cachelagring, komprimering och kryptering innan du skickar den tooAzure. hello hanteringsservern ska ha tillräckliga resurser tooperform dessa uppgifter.
* Hej processervern använder diskbaserad cacheminne. Vi rekommenderar en separat cache disk 600 GB eller mer toohandle dataändringarna lagras i hello-händelse flaskhalsar i nätverket eller avbrott. Du kan konfigurera hello cache på alla enheter som har minst 5 GB tillgängligt utrymme under distributionen, men 600 GB är hello minsta rekommendation.
* Som bästa praxis, rekommenderar vi att hello management server finns på hello samma nätverk och LAN-segment som hello datorer som du vill tooprotect. Det kan finnas på ett annat nätverk, men datorer som du vill att tooprotect ska ha L3 nätverket synlighet tooit.

Storlek rekommendationer för hello management server sammanfattas i följande tabell hello:

| **Hanteringsservern CPU** | **Minne** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz) |16 GB |300 GB |500 GB eller mindre |Distribuera en hanteringsserver med dessa inställningar tooreplicate färre än 100 datorer. |
| 12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor) |18 GB |600 GB |500 GB too1 TB |Distribuera en hanteringsserver med dessa inställningar tooreplicate 100 150 datorer. |
| 16 vCPUs (2 sockets * 8 kärnor @ 2,5 GHz) |32 GB |1 TB |1 TB too2 TB |Distribuera en hanteringsserver med dessa inställningar tooreplicate 150 200 datorer. |
| Distribuera en annan processerver | | |> 2 TB |Distribuera ytterligare servrar om du replikerar mer än 200 datorer eller ändrar hello dagliga data frekvensen överskrider 2 TB. |

Där:

* Varje källdatorn har konfigurerats med 3 diskar på 100 GB vardera.
* Vi använde benchmarking lagring av 8 SAS-enheter med 10 000 RPM med RAID 10 för cache disk mått.

### <a name="network-bandwidth-from-source-tootarget"></a>Nätverksbandbredd från källan tootarget
Kontrollera att du beräkna hello bandbredd som krävs för inledande replikering och deltareplikering med hjälp av hello [kapacitetsplaneringsverktyget för](site-recovery-capacity-planner.md).

#### <a name="throttling-bandwidth-used-for-replication"></a>Begränsning av bandbredd som används för replikering
VMware trafik som replikeras tooAzure genomgår en specifik process-server. Du kan begränsa hello bandbredden som är tillgänglig för Site Recovery replikering på servern på följande sätt:

1. Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello huvudsakliga hanteringsservern eller på en hanteringsserver kör ytterligare etablerats servrar. Som standard skapas en genväg till Microsoft Azure Backup på hello skrivbordet. Eller, du kan hitta den i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello, klickar du på **ändra egenskaper för**.

    ![Begränsa bandbredden ändra egenskaper](./media/site-recovery-vmware-to-azure-classic/throttle1.png)
3. På hello **begränsning** anger hello bandbredd som kan användas för Site Recovery replikering och hello tillämpliga schemaläggning.

    ![Begränsning av bandbredd replikering](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Du kan också ange begränsning med hjälp av PowerShell. Här är ett exempel:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Maximera användningen av nätverksbandbredd
tooincrease hello bandbredd som används för replikering av Azure Site Recovery, behöver du toochange en registernyckel.

hello hello följande viktiga kontroller antalet trådar per replikerar disk som används vid replikering av:

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 I en överetablerad nätverk måste den här registernyckeln toobe har ändrats från standardvärdena. Vi stöder högst 32.  

Mer information om detaljerad kapacitetsplanering finns [kapacitetsplaneringsverktyget för Site Recovery](site-recovery-capacity-planner.md).

### <a name="additional-process-servers"></a>Ytterligare servrar
Om du behöver tooprotect fler än 200 datorer eller din dagliga förändringstakten större att 2 TB, du kan lägga till ytterligare servrar toohandle hello belastningen. tooscale ut kan du:

* Öka hello antalet hanteringsservrar. Du kan till exempel skydda too400 datorer med två hanteringsservrar.
* Lägga till ytterligare servrar och använder dessa toohandle trafik i stället för (eller förutom) hello management server.

Den här tabellen beskriver ett scenario där:

* Du ställa in hello ursprungliga management server toouse som bara en konfigurationsserver.
* Du kan konfigurera en server med ytterligare processer.
* Du konfigurerar skyddade virtuella datorer toouse hello ytterligare processer server.
* Varje skyddade källdatorn har konfigurerats med tre diskar på 100 GB vardera.

| **Den ursprungliga management server**<br/><br/>(konfigurationsserver) | **Ytterligare processervern** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 16 GB RAM-minne |4 vCPUs (2 sockets * 2 kärnor @ 2,5 GHz), 8 GB RAM-minne |300 GB |250 GB eller mindre |Du kan replikera 85 eller färre datorer. |
| 8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 16 GB RAM-minne |8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 12 GB RAM-minne |600 GB |250 GB too1 TB |Du kan replikera 85 150 datorer. |
| 12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz), 18 GB RAM-minne |12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz) 24 GB RAM-minne |1 TB |1 TB too2 TB |Du kan replikera 150 225 datorer. |

Hur du skala dina servrar beror på dina inställningar för en modell skala upp eller skala ut. Du skala upp genom att distribuera några avancerade hanterings- och servrar eller skala ut genom att distribuera flera servrar med färre resurser. Exempel: Om du behöver tooprotect 220 datorer kan du göra något av följande hello:

* Konfigurera ursprungliga hello-hanteringsservern med 12 vCPUs och 18 GB RAM-minne. Konfigurera en server med ytterligare processer med 12 vCPUs och 24 GB RAM-minne. Konfigurera skyddade datorer toouse endast hello ytterligare processervern.
* Konfigurera två hanteringsservrar (2 x 8 vCPUs, 16 GB RAM-minne) och två ytterligare servrar (1 x 8 vCPUs och 4vCPUs x 1 toohandle 135 + 85 (220) datorer). Konfigurera skyddade datorer toouse endast hello ytterligare servrar.

Följ anvisningarna för hello i [distribuera ytterligare servrar](#deploy-additional-process-servers) tooset in en ytterligare processer-server.

## <a name="before-you-start-deployment"></a>Innan du börjar distributionen
hello sammanfattar följande tabeller hello krav för distribution av det här scenariot.

### <a name="azure-prerequisites"></a>Krav för Azure
| **Krav** | **Detaljer** |
| --- | --- |
| **Azure-konto** |Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). Mer information om priserna för Site Recovery finns [Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Azure Storage** |Du behöver ett Azure storage-konto toostore replikerade data. Replikerade data lagras i Azure-lagring och virtuella Azure-datorer skapas i samband med redundansväxlingen. <br/><br/>Du behöver ett [geo-redundant standardlagringskonto](../storage/storage-redundancy.md#geo-redundant-storage). hello-kontot måste vara i hello samma region som hello Site Recovery-tjänsten och vara associerat med hello samma prenumeration. Replikering toopremium storage-konton stöds inte för närvarande och bör inte användas.<br/><br/>Vi stöder inte flytta lagringskonton som skapats med hjälp av hello [Azure-portalen](../storage/storage-create-storage-account.md) över resursgrupper. Mer information finns i [introduktion tooMicrosoft Azure Storage](../storage/storage-introduction.md).<br/><br/> |
| **Azure-nätverk** |Du behöver ett Azure-virtuella nätverk som virtuella Azure-datorer ska ansluta toowhen växling vid fel. hello Azure virtual network måste finnas i hello samma region som hello Site Recovery-valvet.<br/><br/>toofail tillbaka efter redundans tooAzure behöver du en VPN-anslutning (eller Azure ExpressRoute) ställa in från hello Azure toohello lokal nätverksplats. |

### <a name="on-premises-prerequisites"></a>Krav för det lokala systemet
| **Krav** | **Detaljer** |
| --- | --- |
| **Hanteringsserver** |Du behöver en lokal Windows 2012 R2-server som körs på en virtuell dator eller fysisk server. Alla hello lokalt Site Recovery-komponenter är installerade på den här hanteringsservern.<br/><br/> Vi rekommenderar att du distribuerar hello-server som en högtillgänglig VMware VM. Återställning efter fel toohello lokal plats från Azure är alltid tooVMware VMs oavsett om du redundansväxlade virtuella datorer eller fysiska servrar. Om du inte konfigurerar hello hanteringsservern som en VMware VM, måste tooset in en separat huvudmålserver som en VMware VM tooreceive redundanstrafik.<br/><br/>hello server får inte vara en domänkontrollant.<br/><br/>hello-server bör ha en statisk IP-adress.<br/><br/>hello värdnamnet hello servern ska vara högst 15 tecken eller mindre.<br/><br/>hello operativsystemspråk ska bara vara engelska.<br/><br/>hello hanteringsservern kräver tillgång till Internet.<br/><br/>Du behöver utgående åtkomst från hello-servern på följande sätt: tillfällig åtkomst på 80 för HTTP-under installationen av hello Site Recovery-komponenter (toodownload MySQL) Pågående utgående åtkomst på HTTPS-port 443 för replikeringshantering; Pågående utgående åtkomst på HTTPS 9443 för replikeringstrafik (den här porten kan ändras).<br/><br/> Kontrollera att dessa URL: er är tillgängliga från hello management server: <br/>- \*.hypervrecoverymanager.windowsazure.com<br/>- \*.accesscontrol.windows.net<br/>- \*.backup.windowsazure.com<br/>- \*.blob.core.windows.net<br/>- \*.store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi] (https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Om du har IP-adressbaserade brandväggsregler på hello-servern kontrollerar du att hello reglerna tillåter kommunikation tooAzure. Du behöver tooallow hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) och hello HTTPS (443)-port. Du måste också toowhitelist IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra. hello URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi] (https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") är för att ladda ned MySQL. |
| **VMware vCenter/ESXi-värd** |Behöver du en eller flera VMware vSphere ESX/ESXi hypervisorer hantera dina virtuella datorer VMware ESX/ESXi version 6.0, 5.5 eller 5.1 kör hello senaste uppdateringarna.<br/><br/> Vi rekommenderar att du distribuerar en VMware vCenter server toomanage ESXi-värdar. Den bör köras vCenter version 6.0 eller 5.5 hello senaste uppdateringarna.<br/><br/>Observera att Site Recovery har inte stöd för nya vCenter och vSphere 6.0 funktioner såsom mellan vCenter vMotion, virtuella volymerna och lagring DRS. Site Recovery-stöd är begränsad toofeatures som också var tillgängliga i version 5.5. |
| **Skyddade datorer** |**Azure**<br/><br/>Datorer som du vill tooprotect måste uppfylla [krav för Azure](site-recovery-prereq.md) för att skapa virtuella Azure-datorer.<br><br/>Om du vill ha tooconnect toohello virtuella Azure-datorer efter redundans måste tooenable fjärrskrivbordsanslutningar hello lokala brandväggen.<br/><br/>Utrymmet på en enskild disk på skyddade datorer får inte vara över 1 023 GB. En virtuell dator kan ha upp too64 diskar (alltså upp too64 TB). Om du har diskar som är större än 1 TB kan du använda databasreplikering, till exempel SQL Server alltid aktiverad eller Oracle Data Guard.<br/><br/>Minst 2 GB ledigt utrymme på hello installationsenheten för komponentinstallation.<br/><br/>Gästkluster med delade diskar stöds inte. Om du har en klustrad distribution, Överväg att använda databasreplikering som SQL Server alltid aktiverad eller Oracle Data Guard.<br/><br/>Unified Extensible Firmware Interface (UEFI) / Start Extensible Firmware via stöds inte.<br/><br/>Datornamn får innehålla mellan 1 och 63 tecken (bokstäver, siffror och bindestreck). hello namn måste börja med en bokstav eller siffra och sluta med en bokstav eller siffra. När en dator skyddas kan du ändra hello Azure namn.<br/><br/>**Virtuella VMware-datorer**<br/><br>Du måste tooinstall VMware vSphere PowerCLI 6.0. Om hello management server (konfigurationsserver).<br/><br/>Virtuella VMware-datorer du vill att tooprotect ska ha VMware-verktyg installerat och körs.<br/><br/>Om Virtuella hello källdatorn har NIC-teamindelning kan konverteras tooa enkel NIC efter växling vid fel tooAzure.<br/><br/>Om skyddade virtuella datorerna har en iSCSI-disk, Site Recovery konverterar hello skyddade Virtuella iSCSI-disken till en VHD-fil när hello Virtuella växlar över tooAzure. Om iSCSI-målet kan nås av hello Azure VM, den ansluta tooiSCSI mål och finns i stort sett två diskar: hello VHD-disk på hello Azure VM och hello källa iSCSI-disken. I det här fallet behöver du toodisconnect hello iSCSI-mål som visas på hello misslyckades över virtuella Azure-datorn.<br/><br/>Mer information om hello VMware användarbehörigheter som Site Recovery behov, se [VMware åtkomstbehörighet vCenter](#vmware-permissions-for-vcenter-access).<br/><br/> **Windows Server-datorer (på VMware VM eller fysisk server)**<br/><br/>hello-servern ska köras på ett 64-bitars operativsystem: Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2 med på minst SP1.<br/><br/>hello operativsystemet ska installeras på enhet C och hello OS-disken bör vara en standarddisk i Windows. (hello OS bör inte installeras på en dynamisk disk i Windows.)<br/><br/>För Windows Server 2008 R2-datorer måste du toohave .NET Framework 3.5.1 installerad.<br/><br/>Du behöver tooprovide ett administratörskontot (måste vara lokal administratör på hello Windows-dator) för hello push-installation hello Mobilitetstjänsten på Windows-servrar. Om hello angetts kontot ett domänkonto, måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. Mer information finns i [installera hello mobilitetstjänsten med push-installation av](#install-the-mobility-service-with-push-installation).<br/><br/>Site Recovery har stöd för virtuella datorer med en RDM-disken. Under återställning efter fel återanvända Site Recovery hello RDM-disken om hello ursprungliga Virtuella källdatorn och RDM-disken är tillgängliga. Om de inte tillgängliga under återställning efter fel, skapas en ny VMDK-fil för varje disk med Site Recovery.<br/><br/>**Linux-datorer**<br/><br/>Du behöver ett 64-bitars operativsystem: Red Hat Enterprise Linux 6.7; Centos 6.5, 6.6 eller 6.7; Oracle Enterprise Linux 6.4 eller 6.5 kör hello Red Hat kompatibel kernel eller Unbreakable Enterprise Kernel version 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts-filer på skyddade datorer bör innehålla poster som mappar hello lokal värd name tooIP adresser som är associerade med alla nätverkskort. <br/><br/>Om du vill tooconnect tooan Azure-dator som kör Linux efter en redundansväxling med hjälp av en Secure Shell-klient (ssh), se till att hello Secure Shell-tjänsten på hello skyddade datorn är toostart automatiskt på systemstart, och att brandväggsreglerna tillåter en ssh anslutningen tooit.<br/><br/>Skydd kan bara aktiveras på Linux-datorer med följande lagring hello: File system (EXT3, ETX4, ReiserFS, XFS); Multipath programvara enhet Mapper (MPIO) Volymhanterare (LVM2). Fysiska servrar med HP CCISS domänkontrollant lagring stöds inte. Hej ReiserFS filsystem stöds endast för SUSE Linux Enterprise Server 11 SP3.<br/><br/>Site Recovery har stöd för virtuella datorer med en RDM-disken. Site Recovery återanvända inte hello RDM-disken under återställning efter fel för Linux. I stället skapar en ny VMDK-fil för varje motsvarande RDM-disken. |

Endast för en Linux-VM: Kontrollera att du ställt in hello disk.enableUUID=true inställningen i hello konfigurationsparametrar för hello VM i VMware. Om den här raden inte finns, kan du lägga till den. Detta är obligatorisk tooprovide en konsekvent UUID toohello VMDK så att den monterar korrekt. Utan den här inställningen kommer återställning av en fullständig hämtning även om hello VM är tillgängliga lokalt. Att lägga till den här inställningen garanterar att endast deltaändringar överförs tillbaka under återställning efter fel.

## <a name="step-1-create-a-vault"></a>Steg 1: Skapa ett valv
1. Logga in toohello [Azure-portalen](https://manage.windowsazure.com/).
2. Expandera **datatjänster** > **återställningstjänster** och på **Site Recovery-valv**.
3. Klicka på **Skapa nytt** > **Snabbregistrering**.
4. I **namn**, ange ett eget namn tooidentify hello valv.
5. I **Region**, Välj hello geografiskt område för hello-valvet. toocheck stöds regioner, se [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klicka på **Skapa valv**.
    ![Skapa valv](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Kontrollera hello status fältet tooconfirm som hello valvet har skapats. hello valvet visas som **Active** på hello huvudsakliga **återställningstjänster** sidan.

## <a name="step-2-set-up-an-azure-network"></a>Steg 2: Konfigurera ett Azure-nätverk
Skapa ett Azure-nätverk så att virtuella Azure-datorer kommer att vara anslutna tooa nätverk efter växling vid fel och så att återställning efter fel toohello lokal plats kan fungera som förväntat.

1. Välj i hello Azure-portalen, **skapa virtuellt nätverk** och ange hello nätverksnamnet och IP-adressintervall undernätsnamn.
2. Om du behöver toodo återställning kan du lägga till VPN/ExpressRoute toohello nätverk. VPN/ExpressRoute kan läggas toohello nätverket även efter växling vid fel.

Läs mer om Azure nätverk [virtuella nätverk översikt](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> [Migrering av nätverk](../azure-resource-manager/resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för nätverk som används för att distribuera Site Recovery.
>
>

## <a name="step-3-install-hello-vmware-components"></a>Steg 3: Installera hello VMware-komponenter
Om du vill tooreplicate virtuella VMware-datorer, Följ dessa steg om hello management server:

1. [Hämta](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) och installera VMware vSphere PowerCLI 6.0.
2. Starta om hello-servern.

## <a name="step-4-download-a-vault-registration-key"></a>Steg 4: Hämta en valvregistreringsnyckel
1. Hello hanteringsservern, öppna hello Site Recovery-konsolen i Azure. På hello **återställningstjänster** klickar du på hello valvet tooopen hello **Snabbstart** sidan. Du kan också öppna hello **Snabbstart** sida när som helst genom att klicka på ikonen hello.

    ![Snabb Start-ikonen](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)
2. På hello **Snabbstart** klickar du på **förbereda Målresurser** > **ladda ned en registreringsnyckel**. hello registreringsfilen genereras automatiskt. Det är giltig i fem dagar efter att den har skapats.

## <a name="step-5-install-hello-management-server"></a>Steg 5: Installera hello management server
> [!TIP]
> Kontrollera att dessa URL: er är tillgängliga från hello management server:
>
> * *.hypervrecoverymanager.windowsazure.com
> * *.accesscontrol.windows.net
> * *.backup.windowsazure.com
> * *.blob.core.windows.net
> * *.store.core.windows.net
> * https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi
> * https://www.msftncsi.com/ncsi.txt
>
>



>[!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Setup-Registration/player]



1. På hello **Snabbstart** sidan måste du ladda ned hello enhetlig installation toohello filserver.
2. Kör installationsprogrammet för hello installationen filen toostart i hello **Unified installationsprogram för Site Recovery** guiden.
3. I **innan du börjar**väljer **installera hello konfigurationsservern och processervern**.

   ![Innan du börjar](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. I **licensvillkoren för programvara från tredje parts**, klickar du på **jag accepterar** toodownload och installera MySQL.

    ![Tredjepartsprogram](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)
5. I **registrering**, bläddra och välj hello registreringsnyckel som du hämtade från hello-valvet.

    ![Registrering](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)
6. I **Internetinställningar**, ange hur hello-providern som körs på konfigurationsservern hello ansluter tooAzure Site Recovery via hello Internet.

   * Om du vill tooconnect med hello-proxy som är för tillfället inställd på hello datorn markerar **Anslut med befintliga proxyinställningar**.
   * Om du vill hello providern tooconnect direkt markerar **Anslut direkt utan en proxy**.
   * Om hello befintliga proxyservern kräver autentisering, eller om du vill toouse en anpassad proxyserver för hello leverantörsanslutning, Välj **Anslut med anpassade proxyinställningar**.
     * Om du använder en anpassad proxyserver måste toospecify hello adress, port och autentiseringsuppgifter.
     * Om du använder en proxyserver bör du redan ha tillåtit hello följande URL: er:
       * *.hypervrecoverymanager.windowsazure.com    
       * *.accesscontrol.windows.net
       * *.backup.windowsazure.com
       * *.blob.core.windows.net
       * *.store.core.windows.net

    ![Brandvägg](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

1. I **Kravkontroll**, installationsprogrammet körs en kontroll toomake till att hello installation kan köras.

    ![Krav](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     En varning visas om hello **Global kontroll när synkronisering**, kontrollera då hello på hello systemklockan (**datum och tid** inställningar) hello är samma som hello tidszon.

     ![Tid synkroniseringsproblem](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

1. I **MySQL Configuration**, skapa autentiseringsuppgifter för att logga in toohello MySQL server-instans som ska installeras.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)
2. I **miljö information**väljer du om du ska tooreplicate virtuella VMware-datorer. Om du är kontrolleras att PowerCLI 6.0 är installerad.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)
3. I **installera platsen**väljer där tooinstall hello binärfiler och om du vill lagra hello cache. Du kan välja en enhet som har minst 5 GB ledigt diskutrymme, men vi rekommenderar en cacheenhet med minst 600 GB ledigt diskutrymme.

   ![Installationsplats](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)
4. I **val av nätverk**, ange hello lyssnare (nätverkskort och SSL-port) på vilken hello konfigurationsservern kan skicka och ta emot data för replikering. Du kan ändra hello standard port (9443). Port 443 kommer att användas av en webbserver som samordnar replikeringsåtgärder i tillägg toothis porten. Använd inte 443 för att ta emot replikeringstrafik.

    ![Val av nätverk](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



1. I **sammanfattning**, granska hello information och klickar på **installera**. När installationen är klar skapas en lösenfras. Du behöver den när du aktiverar replikering, så kopiera den och förvara den på en säker plats.

   ![Sammanfattning](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)


> [!WARNING]
> Hej agentproxy för Microsoft Azure Recovery Service måste ställas in.
> När hello installationen är klar startar du hello Microsoft Azure Recovery Services-gränssnitt från hello Start-menyn. Kör följande uppsättning kommandon tooset in hello proxyserverinställningar hello i hello kommando-fönstret som öppnas.
>
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
>


### <a name="run-setup-from-hello-command-line"></a>Kör installationsprogrammet från hello kommandorad
Du kan också köra hello enhetlig guiden hello kommandoraden på följande sätt:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]

Där:

* / ServerMode: Obligatorisk. Anger om hello installation ska installera hello konfiguration och processen servrar eller hello processervern endast (används tooinstall ytterligare servrar). Indatavärden: CS, PS.
* InstallDrive: obligatorisk. Anger hello mappen där hello-komponenter installeras.
* / MySQLCredFilePath. Obligatorisk. Anger hello sökvägen tooa fil där hello MySQL autentiseringsuppgifter lagras. Hämta hello mallfilen toocreate hello.
* / VaultCredFilePath. Obligatorisk. Platsen för hello valvautentiseringsfilen.
* /EnvType. Obligatorisk. Typ av installation. Värden: VMware, NonVMware.
* /PSIP och /CSIP. Obligatorisk. IP-adress hello processervern och konfigurationsservern.
* /PassphraseFilePath. Obligatorisk. Sökväg till hello lösenfras.
* / ByPassProxy. Valfri. Anger hello hanteringsservern som ansluter tooAzure utan en proxy.
* /ProxySettingsFilePath. Valfri. Anger inställningar för en anpassad proxy (standardproxy på hello-server som kräver autentisering) eller anpassad proxyserver.

## <a name="step-6-set-up-credentials-for-hello-vcenter-server"></a>Steg 6: Konfigurera autentiseringsuppgifter för hello vCenter server
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Discovery/player]
>
>

Hej processervern kan automatiskt identifiera virtuella VMware-datorer som hanteras av en vCenter-server. Site Recovery måste ett konto och autentiseringsuppgifter som har åtkomst till hello vCenter-servern för automatisk identifiering. Det är inte relevant om du replikerar endast fysiska servrar.

tooset hello konto och autentiseringsuppgifter:

1. Skapa en roll på hello vCenter server (**Azure_Site_Recovery**) på hello vCenter nivå med hello [nödvändiga behörigheter](#vmware-permissions-for-vcenter-access).
2. Tilldela hello **Azure_Site_Recovery** rollen tooa vCenter användare.

   > [!NOTE]
   > En vCenter-användarkonto som har hello skrivskyddad roll kan köra redundans utan att stänga av skyddade källdatorer. Om du vill tooshut ned dessa datorer måste hello Azure_Site_Recovery roll. Om du migrerar endast virtuella datorer från VMware tooAzure och behöver inte toofail tillbaka räcker hello skrivskyddad roll.
   >
   >
3. tooadd hello konto, öppna **cspsconfigtool**. Det är tillgängligt som en genväg på skrivbordet hello och finns i hello [installera plats] \home\svsystems\bin mapp.
4. På hello **hantera konton** klickar du på **Lägg till konto**.

    ![Lägg till konto](./media/site-recovery-vmware-to-azure-classic/credentials1.png)
5. I **kontoinformation**, lägga till autentiseringsuppgifter som kan använda tooaccess hello vCenter-servern. Det kan ta mer än 15 minuter för hello konto namn tooappear hello-portalen. tooupdate omedelbart, klickar du på **uppdatera** på hello **Konfigurationsservrar** fliken.

    ![Information](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Steg 7: Lägga till vCenter-servrar och ESXi-värdar
Om du replikerar virtuella VMware-datorer måste tooadd en vCenter-server (eller ESXi-värd).

1. På hello **servrar** > **Konfigurationsservrar** väljer **Lägg till vCenter-servern**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)
2. Lägg till hello vCenter-servern eller ESXi-värd information, hello namnet på hello-konto som du angav tooaccess hello vCenter-servern i hello föregående steg och hello processervern som kommer att använda toodiscover virtuella VMware-datorer som hanteras av hello vCenter-servern. Hej vCenter-servern eller ESXi-värd måste finnas i samma nätverk som hello-server på vilken hello processervern installeras hello.

   > [!NOTE]
   > Om du lägger till hello vCenter-servern eller ESXi-värd med ett konto som inte har administratörsbehörighet på hello vCenter eller värden server Kontrollera hello vCenter eller ESXi konton har dessa behörigheter aktiveras: Datacenter, datalagret, mapp, Jost, nätverk, Resurs, virtuell dator och vSphere distribuerade växel. Hej vCenter-servern måste hello lagring vyer privilegiet aktiverat.
   >
   >

    ![Lägg till vCenter-servern](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)
3. När identifieringen har slutförts visas hello vCenter-servern på hello **Konfigurationsservrar** fliken.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)

## <a name="step-8-create-a-protection-group"></a>Steg 8: Skapa en skyddsgrupp
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Protection/player]
>
>

Skyddsgrupper är logiska grupperingar av virtuella datorer eller fysiska servrar som du vill tooprotect med hjälp av hello samma skyddsinställningar. Du gäller skydd inställningar tooa skyddsgrupp, och dessa inställningar är tillämpade tooall datorer (virtuella eller fysiska) som du lägger till toohello grupp.

1. Gå för**skyddade objekt** > **Skyddsgrupp** och klicka på hello ikonen tooadd en skyddsgrupp.

    ![Skapa en skyddsgrupp](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)
2. På hello **ange inställningarna för Skyddsgruppen** anger du ett namn för hello-gruppen. I hello **från** listrutan, Välj hello konfigurationsservern som du vill använda toocreate hello grupp. **Målet** är Microsoft Azure.

    ![Inställningarna för skyddsgruppen](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)
3. På hello **ange replikeringsinställningarna** konfigurerar hello replikeringsinställningarna som ska användas för alla hello-datorer i hello grupp.

    ![Skydd grupp replikering](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

   * **Multi VM konsekvenskontroll**: Om du aktiverar detta när skapar den delade programkonsekventa återställningspunkter på hello datorer i hello skyddsgrupp. Den här inställningen är mycket användbar när alla datorer i skyddsgruppen hello körs hello samma arbetsbelastning. Alla datorer kommer att återställda toohello samma datapunkt. Detta är tillgängligt om du replikerar virtuella VMware-datorer eller fysiska servrar (Windows eller Linux).
   * **Tröskelvärde för Återställningspunktmål**: Anger hello Återställningspunktmål. Aviseringar genereras när hello kontinuerliga data protection replication överskrider tröskelvärdet hello konfigurerats.
   * **Kvarhållningstid för återställningspunkten**: Anger hello kvarhållningsperiod. Skyddade datorer kan återställas tooany punkt inom det här fönstret.
   * **Programkonsekventa ögonblicksbilder frekvens**: Anger hur ofta återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.

När du väljer hello är markerat, skapas en skyddsgrupp med hello namn du angav. Dessutom kan en andra skyddsgrupp skapas med hello namn *skyddsgruppnamn-*- återställning efter fel. Om du växlar tillbaka toohello den lokala platsen efter redundans tooAzure används den här skyddsgruppen. Du kan övervaka hello skyddsgrupper som de skapas på hello **skyddade objekt** sidan.

## <a name="step-9-install-hello-mobility-service"></a>Steg 9: Installera hello mobilitetstjänsten
hello första steget i att aktivera skydd för virtuella datorer och fysiska servrar är tooinstall hello mobilitetstjänsten. Du kan göra detta på två sätt:

* Push- och installera hello-tjänsten på varje dator från hello processervern automatiskt. När du lägger till datorer tooa skyddsgruppen och de redan kör en lämplig version av hello mobilitetstjänsten sker push-installation inte. Du kan också automatiskt installera hello-tjänsten med hjälp av ditt företag metoden som WSUS eller System Center Configuration Manager. Kontrollera att du har konfigurerat hello hanteringsservern innan du gör detta.
* Installera hello-tjänsten manuellt på varje dator som du vill tooprotect. Kontrollera att du har konfigurerat hello hanteringsservern innan du gör detta.

### <a name="install-hello-mobility-service-with-push-installation"></a>Installera mobilitetstjänsten hello med push-installation
När du lägger till datorer tooa skyddsgrupp pushas hello mobilitetstjänsten automatiskt och installeras på varje dator med hello processervern.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Förbered för automatisk push-installation på Windows-datorer
tooprepare Windows-datorer så som hello mobilitetstjänsten kan installeras automatiskt av processervern hello:

1. Skapa ett konto som hello processervern kan använda tooaccess hello dator. hello kontot ska ha administratörsbehörighet (lokalt eller via domänadministratör). Dessa autentiseringsuppgifter används endast för push-installation av hello mobilitetstjänsten.

   > [!NOTE]
   > Om du inte använder ett domänkonto, behöver du toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo, hello registret under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, lägga till hello DWORD-post LocalAccountTokenFilterPolicy med värdet 1 under. tooadd hello registerposten från en CLI öppna eller med hjälp av PowerShell ange  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .
   >
   >
2. Windows-brandväggen på hello datorn som du vill tooprotect Välj **tillåta en app eller funktion via brandväggen**, och aktivera **File and Printer Sharing** och **Windows Management Instrumentation**. Du kan konfigurera hello-brandväggsprincip för datorer som tillhör tooa domän, med ett grupprincipobjekt.

   ![Brandväggsinställningar](./media/site-recovery-vmware-to-azure-classic/mobility1.png)
3. Lägg till hello-kontot som du skapade:

   * Öppna **cspsconfigtool**. Det är tillgängligt som en genväg på skrivbordet hello och finns i hello [installera plats] \home\svsystems\bin mapp.
   * På hello **hantera konton** klickar du på **Lägg till konto**.
   * Lägg till hello-konto som du skapade. När du lägger till hello konto måste tooprovide hello autentiseringsuppgifter när du lägger till en dator tooa skyddsgrupp.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Förbered för automatisk push-installation på Linux-servrar
1. Kontrollera att hello Linux-dator som du vill tooprotect stöds enligt beskrivningen i [krav på lokal](#on-premises-prerequisites). Kontrollera att det finns en nätverksanslutning mellan hello datorn du vill tooprotect och hello management-server som kör hello processervern.
2. Skapa ett konto som hello processervern kan använda tooaccess hello dator. hello kontot ska vara en rotanvändare på Linux-hello källservern. Dessa autentiseringsuppgifter används endast för push-installation av hello mobilitetstjänsten.

   * Öppna **cspsconfigtool**. Det är tillgängligt som en genväg på skrivbordet hello och finns i hello [installera plats] \home\svsystems\bin mapp.
   * På hello **hantera konton** klickar du på **Lägg till konto**.
   * Lägg till hello-konto som du skapade. När du lägger till hello konto måste tooprovide hello autentiseringsuppgifter när du lägger till en dator tooa skyddsgrupp.
3. Kontrollera att hello/etc/hosts-filen på hello källan Linux-servern innehåller poster som mappar hello lokala värdnamnet tooIP adresser som är associerade med alla nätverkskort.
4. Installera hello senaste openssh, openssh-server och openssl paket på hello datorn som du vill tooprotect.
5. Se till att SSH är aktiverade och körs på port 22.
6. Aktivera autentisering med SFTP undersystemet och lösenord i hello sshd_config fil på följande sätt:

   * Logga in som rot.
   * Hitta hello rad som börjar med PasswordAuthentication i hello /etc/ssh/sshd_config-filen.
   * Ta bort kommentarerna hello rad och ändra hello-värde från **inga** för**Ja**.
   * Hitta hello rad som börjar med **undersystemet** och Avkommentera hello rad.

     ![Linux åsidosätta standardvärdet för inga undersystem](./media/site-recovery-vmware-to-azure-classic/mobility2.png)

### <a name="install-hello-mobility-service-manually"></a>Installera hello mobilitetstjänsten manuellt
Installationsprogram för hello finns i C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

| Källoperativsystem | Installationsfil för mobilitetstjänsten |
| --- | --- |
| Windows Server (endast 64-bitars) |Microsoft-ASR_UA_9*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (endast 64 bitars) |Microsoft-ASR_UA_9*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (endast 64 bitars) |Microsoft-ASR_UA_9*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (endast 64 bitars) |Microsoft-ASR_UA_9*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-hello-mobility-service-manually-on-a-windows-server"></a>Installera hello mobilitetstjänsten manuellt på en Windows server
1. Hämta och kör installationsprogrammet för hello relevanta.
2. Välj **mobilitetstjänsten** i **innan du börjar**.

    ![Installationen av Mobility](./media/site-recovery-vmware-to-azure-classic/mobility3.png)
3. I **Server konfigurationsinformation**anger hello hello management serverns IP-adress och hello lösenfras som skapades när du installerade hello management server-komponenter. Du kan hämta hello lösenfras genom att köra  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** på hello-hanteringsservern.

    ![Mobilitetstjänsten](./media/site-recovery-vmware-to-azure-classic/mobility6.png)
4. I **installera platsen**, hålla hello standardplatsen och klickar på **nästa** toobegin installation.
5. I **Installationsförlopp**, kontrollera installationen och starta om hello datorn om du uppmanas till detta.

Du kan också installera genom att ange hello följande text hello kommandoraden:

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]

I föregående kommando hello:

* / Roll: obligatorisk. Anger om hello mobilitetstjänsten ska installeras.
* / InstallLocation: obligatorisk. Anger om tooinstall hello service.
* / PassphraseFilePath: obligatorisk. Anger hello konfigurationsserverns lösenfras.
* / LogFilePath: obligatorisk. Anger plats för hello loggfilen installationen.

#### <a name="uninstall-hello-mobility-service-manually"></a>Avinstallera mobilitetstjänsten för hello manuellt
Du kan avinstallera mobilitetstjänsten hello med **avinstallera eller ändra ett program** på Kontrollpanelen eller genom att använda hello-kommandoraden.

hello kommandot toouninstall mobilitetstjänsten med hjälp av kommandoraden hello är:

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="change-hello-ip-address-of-hello-management-server"></a>Ändra hello hello management serverns IP-adress
När du har kört hello guiden kan du ändra hello IP-adressen för hello hanteringsservern enligt följande:

1. Öppna hello filen hostconfig.exe (som finns på hello skrivbord).
2. På hello **Global** fliken, ändrar hello hello management serverns IP-adress.

   > [!NOTE]
   > Ändra bara hello IP-adressen för hello-hanteringsservern. hello portnumret för management server-kommunikation måste vara 443, och **Använd HTTPS** vänster måste vara aktiverat. Ändra inte hello lösenfras.
   >
   >

    ![Hantering av IP-adress](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-hello-mobility-service-manually-on-a-linux-server"></a>Installera hello mobilitetstjänsten manuellt på en Linux-server
1. Kopiera hello lämpliga tar Arkiv toohello Linux dator som du vill tooprotect. Se tabellen hello under [installera hello mobilitetstjänsten manuellt](#install-the-mobility-service-manually) toodetermine vilka tar Arkivera du ska använda.
2. Öppna ett shell-program och extrahera hello komprimerade tar-Arkiv tooa lokal sökväg genom att köra:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Skapa en fil med namnet passphrase.txt hello lokal katalog toowhich du extraherade hello innehållet i hello tar arkivet. toodo, kopiera hello lösenfrasen från C:\ProgramData\Microsoft Azure plats Recovery\private\connection.passphrase på hello-hanteringsservern och spara den i passphrase.txt genom att köra  *`echo <passphrase> >passphrase.txt`*  hello Shell.
4. Installera hello mobilitetstjänsten genom att ange hello följande kommando:*`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*
5. Ange hello interna IP-adress hello hanteringsservern och kontrollera port 443.

#### <a name="install-hello-mobility-service-from-hello-command-line"></a>Installera hello mobilitetstjänsten från hello kommandorad

Kopiera hello lösenfras från C:\Program Files (x86) \InMage Systems\private\connection på hello-hanteringsservern och spara den som ”passphrase.txt” på hello-hanteringsservern. Kör sedan följande kommandon hello. I vårt exempel hello management server IP-adressen är 104.40.75.37 och hello HTTPS-porten är 443:

tooinstall på en produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

tooinstall på huvudmålservern för hello:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Steg 10: Aktivera skydd på en dator
tooenable skydd, Lägg till virtuella datorer och fysiska servrar tooa skyddsgruppen. Innan du börjar bör du Observera hello följande om du skyddar virtuella VMware-datorer:

* Virtuella VMware-datorer identifieras var 15: e minut och det kan ta mer än 15 minuter för dem tooappear i hello Site Recovery-portalen efter identifiering.
* Ändringar i miljön på hello virtuella datorn (till exempel VMware verktyg installation) kan ta mer än 15 minuter toobe uppdateras i Site Recovery.
* Du kan kontrollera hello senast identifierade tid för VMwares virtuella datorer i hello **senaste kontakt på** för hello vCenter server/ESXi-värd på hello **Konfigurationsservrar** fliken.
* Om du lägger till en vCenter-servern eller ESXi-värd när du har skapat en skyddsgrupp, kan det ta mer än 15 minuter för hello Azure Site Recovery-portalen toorefresh och virtuella datorer toobe som anges i hello **Lägg till datorer tooa skyddsgrupp**dialogrutan.
* Om du vill tooproceed omedelbart och lägga till skyddsgruppen tooa för datorer utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den) och klicka på **uppdatera**.

Följande gäller också:

* Vi rekommenderar att du skapa dina skyddsgrupper så att de speglar dina arbetsbelastningar. Till exempel lägga till datorer som kör ett visst program toohello samma grupp.
* När du lägger till datorer tooa skyddsgrupp hello processervern push-meddelanden och automatiskt installerar hello mobilitetstjänsten om den inte redan är installerad. Du behöver toohave hello push mekanism förberetts enligt beskrivningen i föregående steg i hello.

### <a name="add-machines-tooa-protection-group"></a>Lägg till datorer tooa skyddsgrupp

1. Gå för**skyddade objekt** > **Skyddsgrupp** > **datorer** > **lägga till datorer** .
2. Om du skyddar virtuella VMware-datorer, i **Välj virtuella datorer**, väljer en vCenter-server som hanterar virtuella datorer (eller hello EXSi värden som de använder) och väljer sedan hello datorer.

    ![Aktivera skydd för virtuella datorer](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)
3. Om du skyddar fysiska servrar i **Välj virtuella datorer**öppnar hello **lägga till fysiska datorer** guiden och ange hello IP-adress och ett eget namn. Välj sedan hello operativsystemsfamiljen.

   ![Aktivera skydd för fysiska servrar](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)
4. I **ange Målresurser**, Välj hello storage-konto som du använder för replikering och välj om hello inställningar ska användas för alla arbetsbelastningar. Premium-lagringskonton stöds inte för närvarande.

   > [!NOTE]
   > Vi stöder inte flytta lagringskonton som skapats med hjälp av hello [Azure-portalen](../storage/storage-create-storage-account.md) över resursgrupper.                           
   > [Migrering av lagringskonton](../azure-resource-manager/resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för storage-konton som används för att distribuera Site Recovery.
   >
   >

    ![Konfigurera Målinställningar för](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)
5. I **ange konton**väljer hello konto som du [konfigurera](#install-the-mobility-service-with-push-installation) toouse för automatisk installation av hello mobilitetstjänsten.

    ![Ange konton](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)
6. Klicka på hello markerat toofinish att lägga till datorer toohello skydd grupp och toostart inledande replikering för varje dator.

   > [!NOTE]
   > Om push-installation har förberetts, installeras hello mobilitetstjänsten automatiskt på datorer som inte har det, så läggs de till toohello skyddsgruppen. När hello-tjänsten installeras ett skyddsjobb startar och misslyckas. Efter hello fel behöver toomanually starta om varje dator som har haft hello mobilitetstjänsten installeras. Hello-skyddsjobb startar igen efter hello omstart och inledande replikering.
   >
   >

Du kan övervaka status för hello **jobb** sidan.

![Övervaka status på sidan för hello-jobb](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Skyddsstatus kan också övervakas i **skyddade objekt** > *skyddsgruppnamn* > **virtuella datorer**. När den inledande replikeringen är klar och synkroniseras data, datorn status ändras för**skyddade**.

![Övervakaren status i skyddade objekt](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)

## <a name="step-11-set-protected-machine-properties"></a>Steg 11: Ange skyddade datoregenskaperna
1. När en dator har en **skyddade** status, du kan konfigurera egenskaperna för växling vid fel. I hello gruppinformation för skydd, Välj hello maskin- och öppna hello **konfigurera** fliken.
2. Site Recovery automatiskt föreslår egenskaper för hello Azure VM och identifierar hello lokalt nätverksinställningar.

    ![Ange egenskaper för virtuell dator](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)
3. Du kan ändra dessa inställningar:

   * **Namn på Azure VM**: Detta är hello-namn som kommer att få toohello datorn i Azure efter växling vid fel. hello namnet måste uppfylla kraven för Azure.
   * **Azure VM-storlek**: hello antalet nätverkskort beror hello storleken du anger för hello virtuella måldatorn. Mer information om storlek och nätverkskort finns hello [storlek tabeller](../virtual-machines/linux/sizes.md). Tänk på följande:

     * När du ändrar hello storleken på en virtuell dator och spara inställningarna för hello hello antalet nätverkskort ändras när du öppnar hello **konfigurera** fliken hello nästa gång. hello minsta antalet nätverkskort på virtuella måldatorer är lika toohello minsta antalet nätverkskort på en virtuell källdator. hello högsta antalet nätverkskort beror hello storleken på hello virtuella datorn.
       * Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika toohello antal nätverkskort som tillåts för hello måldatorn hello målet att ha hello samma antal kort som hello källa.
       * Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello antalet för målstorleken hello, används hello högsta.
        Om en källdator har två nätverkskort och hello måldatorn stöder fyra, kommer hello måldatorn ha två nätverkskort. Om hello källdatorn har två nätverkskort men målstorleken hello stöds stöder endast en att hello måldatorn ha en enda nätverkskort.
     * Om hello virtuella datorn har flera nätverkskort, alla nätverkskort vara anslutna toohello samma Azure-nätverk.
   * **Azure-nätverk**: du måste ange ett Azure-nätverk att virtuella Azure-datorer är anslutna tooafter växling vid fel. Om du inte anger något hello Azure virtuella datorer inte anslutna tooany nätverk. Du måste dessutom toospecify ett Azure-nätverk om du vill toofailback från Azure toohello lokal plats. Återställning kräver en VPN-anslutning mellan ett Azure-nätverk och ett lokalt nätverk.
   * **Azure IP-adressundernät**: Välj hello undernät toowhich hello Azure VM ska ansluta för varje nätverkskort. Observera att om hello nätverkskort på källdatorn hello konfigurerade toouse statisk IP-adress, kan du ange en statisk IP-adress för hello Azure VM. Om du inte anger en statisk IP-adress tilldelas en tillgänglig IP-adress. Om hello mål-IP-adress har angetts, men den används redan av en annan virtuell dator i Azure, misslyckas redundansväxlingen. Om hello nätverkskort på källdatorn hello konfigurerade toouse DHCP, har du DHCP som hello inställning för Azure.      

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Steg 12: Skapa en återställningsplan och kör en växling vid fel
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failover/player]
>
>

Du kan köra en växling vid fel för en enskild dator eller kan du växla över flera virtuella datorer som utför hello samma uppgift eller köra hello samma arbetsbelastning. toofail över flera datorer på hello samma time, du lägger till dem tooa återställningsplan.

toocreate en återställningsplan:

1. På hello **Återställningsplaner** klickar du på **lägga till Återställningsplanen** och lägga till en återställningsplan. Ange information för hello plan och välj **Azure** som hello mål.

 ![Konfigurera återställningsplan](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)
2. I **Välj virtuell dator**, väljer du en skyddsgrupp och välj sedan datorer i återställningsplanen för hello grupp tooadd toohello.

 ![Lägg till virtuella datorer](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Du kan anpassa hello plan toocreate grupper och sekvens hello ordning där datorer i återställningsplanen hello har redundansväxlats. Du kan också lägga till skript och frågar efter manuella åtgärder. Skript som kan skapas manuellt eller med hjälp av [Azure Automation-Runbooks](site-recovery-runbook-automation.md). Mer information om hur du anpassar återställningsplaner finns [skapa återställningsplaner](site-recovery-create-recovery-plans.md).

## <a name="run-a-failover"></a>Kör en redundansväxling
Innan du kör en växling vid fel:

* Kontrollera den hello-hanteringsservern och är tillgängliga. Om det inte är misslyckas växling vid fel.
* Om du kör en oplanerad växling:

  * Om möjligt bör du stänga av primära datorer innan du kör en oplanerad redundansväxling. Detta säkerställer att du inte har både hello käll- och datorer som körs på hello samtidigt. Om du replikerar virtuella VMware-datorer när du kör en oplanerad redundans, kan du ange att Site Recovery bör försöka tooshut ned hello källdatorer. Beroende på hello tillståndet för hello primär plats, kanske eller kanske inte fungerar. Om du replikerar fysiska servrar kan inte Site Recovery det här alternativet.
  * En oplanerad redundansväxling stoppas datareplikeringen från primära datorer så att alla data delta inte överförs när en oplanerad redundans börjar.
  * Om du vill tooconnect toohello replikerade virtuella datorn i Azure efter växling vid fel, måste du aktivera anslutning till fjärrskrivbord på hello källdatorn innan du kör hello redundans. Låt RDP-anslutning via hello-brandväggen. Du måste också tooallow RDP på hello offentlig slutpunkt för hello virtuella Azure-datorn efter redundans. Följ [metodtips](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) tooensure RDP fungerar efter en redundansväxling.

> [!NOTE]
> tooget hello bästa prestanda när du gör en tooAzure för växling vid fel, kontrollera att du har installerat hello Azure-agenten på hello skyddade datorn. Detta hjälper hello datorn Start snabbare och hjälper dig att diagnostisera problem. hello Azure-agenten är tillgänglig för [Linux](https://github.com/Azure/WALinuxAgent) och [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="run-a-test-failover"></a>Köra ett redundanstest
Kör ett test redundans toosimulate din redundans och återställning processer i ett isolerat nätverk som påverkar inte din produktionsmiljö och du kan vanlig replikering fortsätta som vanligt. Testa redundans är initiatd på hello källan, och du kan köra den på ett par olika sätt:

* **Anger ett Azure-nätverk inte**: Om du kör ett redundanstest utan nätverk hello testet kontrollerar att virtuella datorer startar och visas korrekt i Azure. Virtuella datorer inte anslutna tooan Azure-nätverket efter redundans.
* **Ange ett Azure-nätverk**: den här typen av redundans kontrollerar att hello hela replikeringsmiljön som förväntat och att Azure virtuella datorer är anslutna toohello angivna nätverket.

toorun ett redundanstest:

1. På hello **Återställningsplaner** väljer hello plan och klicka på **Redundanstest**.

 ![Välj hello plan](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)
2. I **bekräfta Redundanstestning**väljer **ingen** tooindicate att du inte vill toouse ett Azure-nätverk för att testa redundans hello eller välj hello toowhich hello nätverkstest virtuella datorer ansluts efter växling vid fel. Klicka på hello markerat toostart hello växling vid fel.

 ![Gör ett val](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)
3. Övervaka förloppet för växling vid fel på hello **jobb** fliken.

 ![Övervaka förloppet](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)
4. När hello växling vid fel är klar kan du bör även kunna toosee hello replik Azure datorn visas i **virtuella datorer** i hello Azure-portalen. Om du vill tooinitiate en RDP-anslutning toohello Azure VM, behöver tooopen port 3389 på hello VM slutpunkt.
5. När du har slutförts, när redundansväxling når hello **Slutför** testning fasen klickar du på **testning** toofinish. I **anteckningar**, registrera och spara observationer från redundanstestningen hello.
6. Klicka på **hello redundanstestet är klart** tooautomatically Rensa hello testmiljö. När det är klart hello testa redundans visas en **Slutför** status. Alla element eller virtuella datorer skapas automatiskt under hello testa redundans tas bort. Om ett redundanstest fortsätter längre än två veckor, det tvingas toofinish.

### <a name="run-an-unplanned-failover"></a>Kör en oplanerad redundans
Oplanerad redundans initieras från Azure och kan utföras även om hello primära platsen inte är tillgänglig.

1. På hello **Återställningsplaner** väljer hello plan och klicka på **redundans** > **oplanerad växling**.

 ![Välj hello plan](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)
2. Om du replikerar virtuella VMware-datorer kan du försöka tooshut ned lokala virtuella datorer. Det här är en åtgärd för bästa prestanda och redundans fortsätter om hello arbete lyckas eller inte. Om det inte lyckas visas information om fel på hello **jobb** fliken **oplanerad växling vid fel jobb**.

 ![Alternativet för att stänga lokala virtuella datorer](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

 > [!NOTE]
 > Det här alternativet är inte tillgängligt om du replikerar fysiska servrar. Du behöver tootry tooshut de ned manuellt om möjligt.
 >
 >

3. I **bekräfta redundans**kontrollerar hello redundansriktning (tooAzure) och välj hello återställningspunkt som du vill toouse hello växling vid fel. Om du har aktiverat multi-VM när du konfigurerade replikeringsegenskaper kan du återställa toohello senaste program eller kraschkonsekvent återställningspunkten. Du kan också välja **anpassad återställningspunkt** toorecover tooan tidigare punkt i tiden. Klicka på hello markerat toostart hello växling vid fel.

 ![Bekräfta redundansriktning](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)
4. Vänta tills hello oplanerad växling jobbet toofinish. Du kan övervaka förloppet för växling vid fel på hello **jobb** fliken. Även om fel uppstår under oplanerad växling vid fel, körs hello återställningsplan tills den är klar. Du bör även kunna toosee hello replik Azure datorn visas i **virtuella datorer** i hello Azure-portalen.

### <a name="connect-tooreplicated-azure-virtual-machines-after-failover"></a>Ansluta tooreplicated Azure virtuella datorer efter redundans
tooconnect tooreplicated virtuella datorer i Azure efter växling vid fel, behöver du:

- En anslutning till fjärrskrivbord aktiverat på hello primära datorn.
- Windows-brandväggen på hello primära datorn ange tooallow RDP.
- RDP lägga toohello offentlig slutpunkt för hello virtuella Azure-datorn.

Mer information om hur du konfigurerar detta finns i [Felsöka anslutning till fjärrskrivbord efter redundans med ASR](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="deploy-additional-process-servers"></a>Distribuera ytterligare servrar
Om du måste skala upp distributionen utöver 200 källdatorer, eller om den totala dagliga omsättningen överskrider 2 TB, behöver ytterligare processer servrar toohandle hello trafikvolym. tooset in en ytterligare processer server, kontrollera hello krav i [ytterligare servrar](#additional-process-servers), och sedan konfigurera hello processervern toohello enligt följande anvisningar. När du konfigurerar hello server, kan du konfigurera källa datorer toouse den.

### <a name="set-up-an-additional-process-server"></a>Konfigurera en server med ytterligare processer
Konfigurera en server med ytterligare processer på följande sätt:

* Kör hello unified guiden tooconfigure en hanteringsserver som endast en processerver.
* Om du vill toomanage replikering med hjälp av endast hello nya processervern måste toomigrate dina skyddade datorer.

### <a name="install-hello-process-server"></a>Installera hello processervern
1. På hello **Snabbstart** sidan måste du ladda ned hello enhetlig installationsfilen för hello Site Recovery komponentinstallation. Kör installationsprogrammet.
2. I **innan du börjar**väljer **lägga till ytterligare processer servrar tooscale ut distribution**.

 ![Lägga till processerver](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)
3. Slutför guiden för hello som du gjorde när du [konfigurera](#step-5-install-the-management-server) hello första hanteringsservern.
4. I **Server konfigurationsinformation**, ange hello IP-adressen för hello ursprungliga hanteringsservern där du installerade hello konfigurationsservern och sedan ange hello lösenfras. Om du inte har hello lösenfras kör  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** på hello ursprungliga management server tooretrieve den.

 ![Konfigurationsinformation för server](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-toouse-hello-new-process-server"></a>Migrera datorer toouse hello nya processervern
1. Öppna **Konfigurationsservrar** > **Server** > *namn på hello ursprungliga hanteringsservern*  >   **Information om**.

 ![Serverinformation](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)
2. I hello **servrar** väljer **ändra Processerver** nästa toohello server som du vill toochange.

 ![Uppdatera hello processervern](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)
3. Välj **ändra Processerver**väljer **Målprocesserver**, och sedan väljer hello nya hanteringsservern. Sedan kommer att hantera select hello virtuella datorer som hello nya processervern. Klicka på hello information ikonen tooget information om hello-server. hello genomsnittlig utrymme som behövs för tooreplicate varje valda virtuella datorn toohello nya processervern är visas toohelp som du gör att läsa in beslut. Klicka på hello markerat toostart replikerar toohello nya processervern.

 ![Ändra hello processerver](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)

## <a name="vmware-permissions-for-vcenter-access"></a>VMware-behörigheter för vCenter-åtkomst
Hej processervern kan automatiskt identifiera virtuella datorer på en vCenter-server. tooperform automatisk identifiering, behöver du toodefine en roll (Azure_Site_Recovery) på hello vCenter nivå tooallow Site Recovery tooaccess hello vCenter-servern. Om du bara behöver toomigrate VMware datorer tooAzure och behöver inte toofailback från Azure, kan du definiera en skrivskyddad roll som är tillräckligt. Ange hello behörigheter som beskrivs i [steg 6: Konfigurera autentiseringsuppgifter för hello vCenter server](#step-6-set-up-credentials-for-the-vcenter-server). hello rollbehörigheter sammanfattas i följande tabell hello:

| **Roll** | **Detaljer** | **Behörigheter** |
| --- | --- | --- |
| Azure_Site_Recovery roll |Identifiering av VMware VM |Tilldela dessa behörigheter för hello v Center-servern:<br/><br/>DataStore: Allokera utrymme, bläddra datalagret, låg nivå filåtgärder, ta bort filen, uppdateringsfiler för virtuell dator<br/><br/>Nätverk: Tilldela av nätverket<br/><br/>Resurs: Tilldela virtuella tooresource pool, migrera drivs av virtuella datorn, migrera slås på virtuell dator<br/><br/>Aktiviteter: Skapa uppdatera aktiviteten<br/><br/>Virtuell dator > konfiguration<br/><br/>Virtuell dator > interagera > besvara frågan, enhetsanslutning, konfigurera CD-skivor, konfigurera diskettenheter media, Stäng av slå på Installera för VMware-verktyg<br/><br/>Virtuell dator > Lager > Skapa, registrera, avregistrera<br/><br/>Virtuell dator > etablering > Tillåt virtuella hämtning, Tillåt virtuella filer överför<br/><br/>Virtuell dator > ögonblicksbilder > Ta bort ögonblicksbilder |
| vCenter-användarroll |VMware VM identifiering/redundans utan avstängning av Virtuella källdatorn |Tilldela dessa behörigheter för hello v Center-servern:<br/><br/>Data Center objekt > sprida tooChild objekt, roll = skrivskyddad <br/><br/>hello användaren tilldelas på hello datacenter nivå och har därför åtkomst tooall hello objekt i hello datacenter. Om du vill toorestrict hello, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (ESX-värdar, datastores, virtuella datorer och nätverk). |
| vCenter-användarroll |Redundans och återställning efter fel |Tilldela dessa behörigheter för hello v Center-servern:<br/><br/>Datacenter-objekt – sprida toochild objekt, roll = Azure_Site_Recovery<br/><br/>hello användaren tilldelas på nivån för datacenter och har därför åtkomst tooall hello objekt i hello datacenter.  Om du vill toorestrict hello, tilldela hello ** ingen åtkomst ** rollen med hello **sprida toochild objektet** toohello underordnat objekt (ESX-värdar, datastores, virtuella datorer och nätverk). |

## <a name="third-party-software-notices-and-information"></a>Meddelanden om programvara från tredje part och information
<!--Do Not Translate or Localize-->

hello programvara och inbyggd programvara som körs i hello Microsoft-produkt eller tjänst som baseras på eller innehåller material från hello projekt nedan (gemensamt kallade ”kod från tredje part”).  Microsoft är hello inte ursprungliga författaren av hello kod från tredje part.  hello ursprungliga upphovsrättsmeddelandet och licensen, enligt vilka Microsoft fått sådan kod från tredje part, är anges nedan.

hello informationen i avsnittet A gäller kod från tredje part-komponenter från hello projekt som anges nedan. Sådana licenser och information tillhandahålls endast i informationssyfte.  Den här koden från tredje part är att relicensed tooyou av Microsoft under Microsofts licensvillkoren för hello Microsoft-produkt eller tjänst.  

hello informationen i avsnittet B gäller kod från tredje part-komponenter som blir tillgängliga tooyou av Microsoft under hello ursprungliga licensvillkoren.

hello hela filen finns på hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft förbehåller sig alla rättigheter som inte uttryckligen beviljas, vare sig indirekt, via estoppel eller på annat sätt.

## <a name="next-steps"></a>Nästa steg
[Mer information om återställning efter fel](site-recovery-failback-azure-to-vmware-classic.md) toobring dina misslyckades över datorer som körs i Azure tillbaka tooyour lokala miljö.
