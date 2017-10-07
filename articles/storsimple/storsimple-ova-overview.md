---
title: "Översikt över Azure StorSimple virtuell matris av aaaMicrosoft | Microsoft Docs"
description: "Beskriver hello StorSimple virtuell matris, en integrerad lagringslösning som hanterar lagringsuppgifter mellan en lokal virtuell matris och Microsoft Azure-molnlagring."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/09/2016
ms.author: alkohli
ms.openlocfilehash: 8978e074142940748857150cc93b37272349d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-storsimple-virtual-array"></a>Introduktion toohello virtuella StorSimple-matris
## <a name="overview"></a>Översikt
hello Microsoft Azure StorSimple virtuell matrisen är en integrerad lagringslösning som hanterar lagringsuppgifter mellan en lokal virtuell matris körs i ett hypervisor-programmet och Microsoft Azure-molnlagring. hello virtuella matrisen är ett effektivt, kostnadseffektiv och lätthanterade filserver eller iSCSI-server-lösning som eliminerar många av hello problem och kostnader som är associerade med enterprise lagring och dataskydd. hello virtuella matrisen är särskilt väl lämpade för scenarier med fjärråtkomst office/avdelningskontor.

Det här avsnittet innehåller en översikt över hello virtuella matris - nedan följer några andra resurser:

* Bästa praxis, se [virtuella StorSimple-matris metodtips](storsimple-ova-best-practices.md).
* En översikt över hello StorSimple 8000-serien enheter gå för[StorSimple 8000-serien: en hybridmolnlösningen](storsimple-overview.md). 
* Information om hello StorSimple 5000/7000-serien enheter gå för[StorSimple onlinehjälp](http://onlinehelp.storsimple.com/).

hello virtuella matris stöder hello iSCSI eller Server Message Block (SMB)-protokollet. Det körs på din befintliga infrastruktur för hypervisor och ger lagringsnivåer toohello molnet, säkerhetskopiering i molnet, snabb återställning, objektnivååterställning och disaster recovery-funktioner.

hello sammanfattas följande tabell hello viktiga funktioner i hello virtuella StorSimple-matris.

| Funktion | StorSimple virtuell matris |
| --- | --- |
| Installationskrav |Använder infrastrukturen för virtualisering (Hyper-V eller VMware) |
| Tillgänglighet |Enkel nod |
| Total kapacitet (inklusive moln) |Konfigurera too64 TB användbar kapacitet per virtuell matris |
| Lokala kapacitet |390 GB too6.4 TB användbar kapacitet per virtuell matris (behovet tooprovision 500 GB too8 TB diskutrymme) |
| Intern protokoll |iSCSI- eller SMB |
| Mål för återställningstid (RTO) |iSCSI: 2 minuter oavsett storlek |
| Mål för återställningspunkt (RPO) |Dagliga säkerhetskopieringar och säkerhetskopieringar på begäran |
| Lagringsnivåer |Utnyttjar värme mappning toodetermine vilka data ska vara nivåer in eller ut |
| Support |Virtualiseringsinfrastruktur som stöds av hello-leverantör |
| Prestanda |Varierar beroende på underliggande infrastruktur |
| Data mobility |Kan återställa toohello samma enhet eller objektnivå återställningen (filserver) |
| Lagringsnivåer |Lokala hypervisorn lagring och molnet |
| Dela storlek |Nivåindelad: upp too20 TB; lokalt Fäst: upp too2 TB |
| Volymens storlek |Nivåindelad: 500 GB too5 TB; lokalt Fäst: 50 GB too500 GB |
| Volymens storlek |Nivåindelad: upp too5 TB; lokalt Fäst: upp too500 GB |
| Ögonblicksbilder |Kraschkonsekventa |
| Objektnivååterställning |Ja. användare kan återställa från resurser |

## <a name="why-use-storsimple"></a>Varför använda StorSimple?
StorSimple ansluter användare och servrar tooAzure lagring i minuter, utan att några ändringar i programmet.

hello beskrivs följande tabell några av hello viktiga fördelar som hello StorSimple virtuell matris ger lösningen.

| Funktion | Fördelar |
| --- | --- |
| Transparent integrering |hello virtuella matris stöder hello iSCSI eller hello SMB-protokollet. hello dataförflyttning mellan hello lokal nivå och hello molnet nivå är sömlös och transparent toohello användare. |
| Minskad lagringskostnader |Med StorSimple, kan du etablera tillräcklig lokala toomeet aktuella lagringsbehov för hello används oftast varm data. När lagring behöver växa, molnlagring StorSimple nivåerna kall data till kostnadseffektiv. hello data är deduplicerad och komprimerade innan du skickar toohello moln toofurther minska utrymmeskraven och utgifter. |
| Förenklad lagringshantering |StorSimple tillhandahåller centraliserad hantering i hello molnet med hjälp av Enhetshanteraren StorSimple toomanage flera enheter. |
| Förbättrad återställning och efterlevnad |StorSimple underlättar snabbare katastrofåterställning genom att återställa hello metadata omedelbart och återställa hello data efter behov. Det innebär att normal drift kan fortsätta med minimala störningar. |
| Data mobility |Data nivåindelade toohello moln kan nås från andra platser för återställning och migrering. Observera att du kan återställa data endast toohello ursprungliga virtuella matris. Dock kan du använda disaster recovery funktioner toorestore hello hela virtuella matris tooanother virtuella matris. |

## <a name="storsimple-workload-summary"></a>Översikt över arbetsbelastningar för StorSimple

En sammanfattning av StorSimple-arbetsbelastningar som stöds visas i tabellen nedan.

|Scenario     |Arbetsbelastning     |Stöds      |Begränsningar               |
|-------------|-------------|---------------|---------------------------|
|ROBO samarbete |Fildelning     |Ja      |Se [gränsvärden för filserver](storsimple-ova-limits.md).<br></br>Se [systemkrav för SMB-versioner som stöds](storsimple-ova-system-requirements.md).| Alla versioner     |

## <a name="workflows"></a>Arbetsflöden
hello virtuella StorSimple-matrisen är särskilt lämpad för hello följande arbetsflöden:

* [Molnbaserad lagringshantering](#cloud-based-storage-management)
* [Platsoberoende säkerhetskopiering](#location-independent-backup)
* [Återställning av data protection och katastrofåterställning](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>molnbaserad lagringshantering
Du kan använda hello StorSimple Device Manager-tjänsten körs i hello Azure portal toomanage data som lagras på flera enheter och på flera platser. Detta är särskilt användbart i scenarier med distribuerade grenen. Observera att du måste skapa separata instanser hello StorSimple Enhetshanteraren service toomanage virtuella eller fysiska StorSimple-enheter. Observera också att hello virtuella matris använder nu hello nya Azure-portalen i stället för hello klassiska Azure-portalen.

![molnbaserad lagringshantering](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Platsoberoende säkerhetskopiering
Med virtuella hello-matris ge molnögonblicksbilder en Platsoberoende, point-in-time kopia av en volym eller resurs. Molnögonblicksbilder är aktiverade som standard och kan inte inaktiveras. Alla volymer och resurser är en säkerhetskopia på hello samma tidpunkt via en enda princip för daglig säkerhetskopiering, och du kan vidta ytterligare ad hoc-säkerhetskopiering vid behov.

### <a name="data-protection-and-disaster-recovery"></a>Återställning av data protection och katastrofåterställning
hello virtuella matris stöder följande scenarier för data protection och disaster recovery hello:

* **Volym eller resurs återställning** – Använd hello återställning som nytt arbetsflöde toorecover en volym eller dela. Använd den här metoden toorecover hello hela volymen eller resursen.
* **Återställa** – resurser Tillåt förenklad åtkomst toorecent säkerhetskopior. Du kan enkelt återställa en enskild fil från en särskild *.backup* mapp som är tillgänglig i hello molnet. Den här funktionen för återställning är-användardrivna och inga administrativa åtgärder krävs.
* **Katastrofåterställning** – Använd hello redundans kapaciteten toorecover alla volymer eller resurser tooa nya virtuella matrisen. Du skapar nya virtuella hello-matris och registrera den med hello StorSimple enheten Manager-tjänsten och sedan växla över hello ursprungliga virtuella matris. hello nya virtuella matrisen antar sedan hello etableras resurser. 

## <a name="storsimple-virtual-array-components"></a>StorSimple virtuell matris komponenter
hello virtuella matris innehåller hello följande komponenter:

* [Virtuella matris](#virtual-array) – en hybrid cloud lagringsenhet baserat på en virtuell dator etablerats i din virtualiserade miljö eller hypervisor.  
* [StorSimple Enhetshanteraren service](#storsimple-device-manager-service) – en förlängning av hello Azure-portalen där du kan hantera en eller flera StorSimple-enheter från en enda webbgränssnitt som du kan komma åt från olika geografiska platser. Du kan använda hello StorSimple Enhetshanteraren service toocreate och hantera tjänster, visa och hantera enheter och aviseringar och hantera volymer, resurser och befintliga ögonblicksbilder.
* [Lokala webbanvändargränssnitt](#local-web-user-interface) – ett webbaserat användargränssnitt som är används tooconfigure hello enhet så att den kan ansluta toohello lokala nätverket och sedan registrera hello enheten med hello StorSimple Device Manager-tjänsten. 
* [Kommandoradsgränssnittet](#command-line-interface) – ett Windows PowerShell-gränssnitt som du kan använda toostart en supportsession på hello virtuella matrisen.
  hello följande avsnitt beskrivs var och en av dessa komponenter i detalj och förklara hur hello lösning ordnar data allokerar lagringsutrymme och underlättar lagringshantering och dataskydd.

### <a name="virtual-array"></a>Virtuell matris
hello virtuella matrisen är en nod lagringslösning som ger primär lagring, hanterar kommunikationen med molnlagring och hjälper tooensure hello säkerhet och sekretess för alla data som lagras på hello enhet.

hello virtuella matrisen är tillgängliga i en modell som är tillgänglig för hämtning. hello virtuella matris har en maximal kapacitet för 6.4 TB på hello-enhet (med en underliggande utrymmeskravet på 8 TB) och 64 TB, inklusive molnlagring. 

hello virtuella matris har hello följande funktioner:

* Det är kostnadseffektiv. Det gör användning av din befintliga virtualiseringsinfrastruktur för och kan distribueras på din befintliga Hyper-V- eller VMware hypervisor-programmet.
* Det finns i hello datacenter och kan konfigureras som en iSCSI-server eller en filserver. 
* Det är integrerat med hello molnet.
* Säkerhetskopior lagras i hello moln, vilket kan underlätta återställning och förenkla återställning på objektnivå (ILR). 
* Du kan tillämpa uppdateringar toohello virtuella matris, precis som du kan använda dem tooa fysisk enhet.

> [!NOTE]
> En virtuell matris kan inte expanderas. Därför är det viktigt tooprovision tillräcklig lagring när du skapar virtuella hello-matris. 
> 
> 

### <a name="storsimple-device-manager-service"></a>StorSimple enheten Manager-tjänsten
Microsoft Azure StorSimple innehåller ett webbaserat användargränssnitt hello Enhetshanteraren för StorSimple-tjänst som gör att du toocentrally hantera StorSimple lagring. Du kan använda hello StorSimple Enhetshanteraren service tooperform hello följande uppgifter:

* Hantera flera StorSimple virtuell matriser från en enskild tjänst. 
* Konfigurera och hantera säkerhetsinställningar för virtuell StorSimple-matriser. (Kryptering i hello molnet är beroende av Microsoft Azure API: er).
* Konfigurera lagringskontouppgifter och egenskaper.
* Konfigurera och hantera volymer eller resurser.
* Säkerhetskopiera och återställa data på volymer eller resurser.
* Övervaka prestanda.
* Granska systeminställningarna för och identifiera eventuella problem.

Du kan använda hello StorSimple Enhetshanteraren service tooperform daglig administration av virtuella matrisen.

Mer information finns för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Lokala webbanvändargränssnitt
hello virtuella matris innehåller ett webbaserat gränssnitt som används för engångskonfiguration och registrering av hello enheten med hello StorSimple enheten Manager-tjänsten. Du kan använda tooshut ned och starta om hello virtuella matris, kör diagnostiktest, uppdatera programvara, ändra hello enhetens administratörslösenord, visa systemloggar och kontakta Microsoft Support toofile en tjänstbegäran. 

Information om hur du använder Hej webbaserat användargränssnitt, gå för[Använd hello webbaserat användargränssnitt tooadminister din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Kommandoradsgränssnittet
hello med Windows PowerShell-gränssnittet kan du tooinitiate en supportsession med Microsoft-supporten så att de kan hjälpa dig att felsöka och lösa problem som kan uppstå på din virtuella matrisen.

## <a name="storage-management-technologies"></a>Tekniker för hantering av lagring
Dessutom hello toohello virtuella matris och andra komponenter, StorSimple lösningen använder hello följande tekniker tooprovide Snabbåtkomst tooimportant programdata, minska lagringsanvändningen och skydda data som lagras på din virtuella matrisen:

* [Automatisk lagringsnivåer](#automatic-storage-tiering) 
* [Lokalt Fäst filresurser och volymer](#locally-pinned-shares-and-volumes)
* [Deduplicering och komprimering av data nivåer eller säkerhetskopierades toohello moln](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
* [Schemalagda säkerhetskopieringar och på begäran säkerhetskopieringar](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>automatisk lagringsnivåer
hello virtuella matris används en ny lagringsnivåer mekanism toomanage lagras data över hello virtuella matris och hello moln. Det finns bara två nivåer: hello lokala virtuella matris och Azure molnlagring. hello virtuella StorSimple-matris ordnar automatiskt data i hello nivåer, baserat på en termisk karta som spårar aktuella användning, ålder och relationer tooother data. Data som är de flesta active (senaste) lagras lokalt, medan mindre aktiva och inaktiva data automatiskt migreras toohello moln. (Alla säkerhetskopior lagras i molnet hello.) StorSimple anpassar och ordnar om data och ändra lagring tilldelningar som användningsmönster. Viss information kan till exempel bli mindre aktiva över tid. Vartefter det blir mindre progressivt aktiva är den nivåer ut toohello moln. Om samma data blir aktiva igen, är det nivåer i toohello lagringsmatris.

Data för en viss nivåindelade resursen eller volymen är garanterat egna lokala nivån utrymme. (cirka 10% av hello totala allokerade utrymmet för resursen eller volymen). När det här minskar hello tillgängligt lagringsutrymme på hello virtuella matrisen för resursen eller volymen säkerställer att skiktning för en resurs eller volym inte påverkas av hello lagringsnivåer behoven hos andra resurser eller volymer. Därför kan inte en hårt arbetsbelastning på en resurs eller volym tvinga alla andra arbetsbelastningar toohello moln. 

![automatisk lagringsnivåer](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Du kan ange en lokalt Fäst volym, i vilket fall hello data förblir på hello virtuella matrisen och aldrig nivåer toohello moln. Mer information finns för[lokalt Fäst filresurser och volymer](#locally-pinned-shares-and-volumes).
> 
> 

### <a name="locally-pinned-shares-and-volumes"></a>Lokalt Fäst filresurser och volymer
Du kan skapa lämpliga resurser och lokalt fästa volymer. Den här funktionen säkerställer att data som krävs av kritiska program finns kvar i hello virtuella matris och aldrig nivåer toohello moln. Lokalt Fäst filresurser och volymer har hello följande funktioner: 

* De är inte ämne toocloud fördröjningar eller problem med nätverksanslutningen.
* De fortfarande dra nytta av StorSimple moln säkerhetskopiering och katastrofåterställning återställningsfunktioner.

Du kan återställa en lokalt Fäst resursen eller volymen som nivåer eller en skiktad resursen eller volymen lokalt Fäst. 

Mer information om lokalt fästa volymer gå för[använda hello StorSimple Enhetshanteraren service toomanage volymer](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-toohello-cloud"></a>Deduplicering och komprimering av data nivåer eller säkerhetskopierades toohello moln
StorSimple använder deduplicering och data komprimering toofurther minska utrymmeskraven i hello molnet. Deduplicering minskar hello övergripande mängden data som lagras genom att eliminera redundans i hello lagras datamängden. När informationen ändras StorSimple ignorerar hello oförändrade data och registrerar hello endast ändringar. Dessutom minskar StorSimple hello mängden lagrade data genom att identifiera och ta bort dubblettinformation. 

> [!NOTE]
> Data som lagras på hello virtuella matrisen är inte dedupliceras eller komprimerad. Alla dedupliceringen och komprimering sker precis innan hello data skickas toohello moln.
> 
> 

### <a name="scheduled-and-on-demand-backups"></a>Schemalagda säkerhetskopieringar och på begäran säkerhetskopieringar
StorSimple Dataskyddsfunktioner kan du säkerhetskopieringar för toocreate på begäran. Dessutom garanterar ett schema för säkerhetskopiering av standard att data säkerhetskopieras varje dag. Säkerhetskopieringar vidtas i hello form av inkrementell ögonblicksbilder som lagras i hello molnet. Ögonblicksbilder som registrerar endast hello ändringar sedan den senaste säkerhetskopieringen av hello kan skapas och återställs snabbt. Dessa ögonblicksbilder kan vara ytterst viktigt i katastrofåterställning eftersom de ersätta sekundära lagringssystem (till exempel bandsäkerhetskopiering) och Tillåt toorestore data tooyour datacenter eller tooalternate platser om det behövs.

## <a name="next-steps"></a>Nästa steg
Lär dig hur för[förbereda hello virtuella matris portal](storsimple-virtual-array-deploy1-portal-prep.md).

