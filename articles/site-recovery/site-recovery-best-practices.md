---
title: "Metodtips för aaaAzure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver metodtips för Azure Site Recovery-distribution"
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
ms.openlocfilehash: 288df858a0e1c1f5ad96dbe8b9dd0dc69d8f56ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-ready-toodeploy-azure-site-recovery"></a>Hämta redo toodeploy Azure Site Recovery

Den här artikeln beskriver hur tooprepare för Azure Site Recovery-distribution.

## <a name="protecting-hyper-v-virtual-machines"></a>Skydda virtuella Hyper-V-datorer

Du har ett par distributionsalternativ för att skydda virtuella Hyper-V-datorer. Du kan replikera lokala virtuella Hyper datorer tooAzure eller tooa sekundärt datacenter. Det finns olika krav för varje distribution.

**Krav** | **Replikera tooAzure (med VMM)** | **Replikera virtuella Hyper-V-datorer tooAzure (ingen VMM)** | **Replikera virtuella Hyper-V-datorer toosecondary plats (med VMM)** | **Detaljer**
---|---|---|---|---
**VMM** | VMM körs på System Center 2012 R2 <br/><br/>Minst ett VMM-moln som innehåller en eller flera VMM-värdgrupper. | Ej tillämpligt | VMM-servrar i hello primära och sekundära platser som körs på minst System Center 2012 SP1 med de senaste uppdateringarna. <br/><br/> Minst ett moln på varje VMM-server. Moln ska ha angetts hello-Hyper-V-kapacitetsprofilen.<br/><br/> hello-källmolnet bör ha minst en värdgrupp i VMM. | Valfri. Du behöver inte toohave System Center VMM distribueras i ordning tooreplicate Hyper-V virtuella datorer tooAzure men om du behöver du toomake att hello VMM-servern är korrekt konfigurerad. Som innehåller gör att du kör hello rätt VMM-versionen och att moln har ställts in.
**Hyper-V** | Minst en Hyper-V-värdserver i hello lokal plats som kör Windows Server 2012 R2 eller senare | Minst en Hyper-V-server i hello käll- och platser kör minst Windows Server 2012 med hello senaste uppdateringarna installeras och anslutna toohello internet.<br/><br/> hello Hyper-V-servrar måste vara i en värdgrupp i ett VMM-moln. | Minst en Hyper-V-server i hello käll- och platser kör minst Windows Server 2012 med hello senaste uppdateringarna installeras och anslutna toohello internet.<br/><br/> hello Hyper-V-servrar måste finnas i en värdgrupp i ett VMM-moln. |
**Virtuella datorer** | Minst en virtuell dator på hello källa Hyper-V-värdservern | Minst en virtuell dator på hello Hyper-V-värdserver i hello källan VMM-moln | Minst en virtuell dator på hello Hyper-V-värdserver i hello källan VMM-moln. |  Virtuella datorer replikeras tooAzure måste överensstämma med förutsättningar för Azure virtuell dator
**Azure-konto** | Du behöver ett Azure-konto och en prenumeration toohello Site Recovery-tjänsten. | Du behöver ett Azure-konto och en prenumeration toohello Site Recovery-tjänsten. | Ej tillämpligt | Om du inte har ett konto kan du börja med en kostnadsfri utvärderingsversion.
**Azure Storage** | Du behöver en prenumeration för ett Azure Storage-konto som har geo-replikering aktiverat. | Du behöver en prenumeration för ett Azure Storage-konto som har geo-replikering aktiverat. | Ej tillämpligt | hello-konto måste finnas i hello samma region som hello Azure Site Recovery-valvet och associeras med hello samma prenumeration.
**Nätverk** | Konfigurera nätverket mappning tooensure att alla datorer som redundansväxlas hello samma Azure-nätverk kan ansluta tooeach andra, oavsett vilken återställningsplan de finns i. Om en nätverksgateway har på hello målet konfigurera Azure-nätverk, ansluta virtuella datorer dessutom tooother lokala virtuella datorer. Om du inte konfigurerar nätverks-mappning av datorer som växlar över i samma återställningsplan ansluta hello. | Ej tillämpligt |  <br/><br/>Konfigurera nätverket mappning tooensure att virtuella datorer är anslutna tooappropriate nätverk efter växling vid fel och att replikera virtuella datorer placeras optimalt på Hyper-V-värdservrar. Om du inte konfigurerar nätverket mappning replikerade datorer kan inte anslutna tooany nätverket efter redundans. |  tooset in nätverksmappning med VMM måste du toomake till att VMM logiska och Virtuella nätverk konfigureras på rätt sätt.
**Leverantörer och agenter** | Under distributionen installerar hello Azure Site Recovery-providern på VMM-servrar. Du ska installera hello Azure Recovery Services-agenten på Hyper-V-servrar i VMM-moln. | Under distributionen installerar både hello Azure Site Recovery-providern och hello Azure Recovery Services-agenten på hello Hyper-V-värdserver eller kluster| Under distributionen installerar hello Azure Site Recovery-providern på VMM-servrar. Du ska installera hello Azure Recovery Services-agenten på Hyper-V-servrar i VMM-moln. | Leverantörer och ombud ansluta tooSite återställning över hello internet via en krypterad HTTPS-anslutning. Du inte behöver tooadd brandväggsundantag eller skapa en specifik proxy för hello leverantörsanslutning.
**Internetanslutning** | Endast hello VMM-servrar måste ha en Internetanslutning | Endast hello Hyper-V-värdservrar behöver en internet-anslutning | Endast VMM-servrar måste ha en internetanslutning | Virtuella datorer behöver inte något installerat och Anslut inte direkt toohello internet.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>Skydda virtuella VMware-datorer eller fysiska servrar

Det finns ett par distributionsalternativ för att skydda virtuella VMware-datorer eller fysiska Windows-/Linux-servrar. Du kan replikera dem tooAzure eller tooa sekundärt datacenter. Det finns olika krav för varje distribution.

**Krav** | **Replikera VMware virtuella datorer/fysiska servrar tooAzure)** | * **Replikera VMware virtuella datorer/fysiska servrar toosecondary plats**  
---|---|---
**Primär plats** | **Processerver**: en dedikerad Windows-server (fysisk eller virtuell) | **Processerver**: en dedikerad Windows-server (fysisk eller virtuell VMware-maskin)<br/><br/>  
**Sekundär lokal plats** | Ej tillämpligt | **Konfigurationsserver**: en dedikerad Windows-server (fysisk eller virtuell) <br/><br/> **Huvudmålserver**: en dedikerad server (fysisk eller virtuell). Konfigurera med Windows tooprotect Windows-datorer eller Linux tooprotect Linux.
**Azure** | **Prenumerationen**: du behöver en prenumeration för hello Site Recovery-tjänsten. <br/><br/> **Lagringskonto**: du behöver ett lagringskonto med geo-replikering aktiverad. hello konto måste finnas i samma region som Site Recovery-valvet hello hello och associeras med hello samma prenumeration. <br/><br/> **Konfigurationsservern**: du behöver tooset upp hello konfigurationsservern som en Azure VM <br/><br/> **Huvudmålservern**: du behöver tooset upp hello huvudmålservern som en Azure VM <br/><br/> Konfigurera med Windows tooprotect Windows-datorer eller Linux tooprotect Linux.<br/><br/> **Virtuella Azure-nätverket**: du behöver ett Azure-virtuella nätverk på vilka hello konfigurationsservern och huvudmålservern kommer att distribueras. Det bör vara i hello samma prenumeration och region som hello Azure Site Recovery-valvet | Ej tillämpligt  
**Virtuella datorer/fysiska servrar** | Minst en virtuell VMware-dator eller fysisk Windows-/Linux-server.<br/><br/>Under distributionen installeras hello mobilitetstjänsten på varje dator| Minst en virtuell VMware-dator eller fysisk Windows-/Linux-server.<br/><br/> Under distributionen installeras hello Unified agent på varje dator.




## <a name="azure-virtual-machine-requirements"></a>Krav för virtuell Azure-dator

Du kan distribuera Site Recovery tooreplicate virtuella datorer och fysiska servrar som kör ett operativsystem som stöds av Azure. Detta omfattar de flesta versioner av Windows och Linux. Du behöver toomake Kontrollera som lokala virtuella datorer som du vill tooprotect överensstämmer med kraven för Azure.


## <a name="optimizing-your-deployment"></a>Optimera distributionen

Använd hello följande tips toohelp du optimera och skala distributionen.

- **Operativsystemet volymens storlek**: när du replikera en virtuell dator tooAzure hello operativsystem systemvolymen måste vara mindre än 1 TB. Om du har fler volymer än så kan du flytta dem manuellt tooa annan disk före distributionen.
- **Data diskstorlek**: Om du replikerar tooAzure som du kan ha upp too32 datadiskar på en virtuell dator med högst 1 TB. Du kan effektivt replikera och redundansväxla en virtuell dator på ~32 TB.
- **Återställning plan gränser**: Site Recovery kan skala toothousands av virtuella datorer. Återställningsplaner är utformade som en modell för program som ska växlas över tillsammans så vi begränsa hello antalet datorer i en plan too50 för återställning.
- **Begränsningar för Azure-tjänster**: varje Azure-prenumeration har en uppsättning standardgränser för kärnor, molntjänster osv. Vi rekommenderar att du kör ett test redundans toovalidate hello tillgängligheten för resurser i din prenumeration. Du kan ändra dessa gränser via Azure-supporten.
- **Kapacitetsplanering**: planera för skalning och prestanda.
- **Replikeringsbandbredd**: om du har för lite replikeringsbandbredd kan du tänka på följande:
    - **ExpressRoute**: Site Recovery fungerar med Azure ExpressRoute och WAN-optimerare, till exempel Riverbed.
    - **Replikeringstrafik**: Site Recovery använder utför en smart inledande replikering med endast datablock och inte hello hela den virtuella Hårddisken. Endast ändringar replikeras under pågående replikering.
    - **Nätverkstrafik**: du kan styra nätverkstrafik som används för replikering genom att ställa in Windows QoS med en princip baserat på hello mål-IP-adress och port.  Dessutom om du replikerar tooAzure Site Recovery med hello Azure Backup-agenten. Du kan konfigurera begränsning för den agenten.
- **Återställningstidsmålet**: Om du vill toomeasure hello recovery tid mål för Återställningstid du kan förvänta dig med Site Recovery vi rekommenderar att du kör ett redundanstest och visa hello Site Recovery-jobb tooanalyze hur mycket tid det tar toocomplete hello åtgärder. Om du växlar över tooAzure för hello planer bästa RTO rekommenderar vi att du automatisera alla manuella åtgärder genom att integrera med Azure automation och återställning.
- **Återställningspunktmålet**: Site Recovery stöder nästintill synkron mål för återställningspunkt (RPO) när du replikerar tooAzure. Detta förutsätter tillräckligt mycket bandbredd mellan datacentret och Azure.
