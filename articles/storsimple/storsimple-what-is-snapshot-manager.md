---
title: "aaaWhat är StorSimple Snapshot Manager? | Microsoft Docs"
description: Beskriver hello StorSimple Snapshot Manager, dess arkitektur och dess funktioner.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79ce7b7e1970ac862038af2a0e67065b6fb6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-toostorsimple-snapshot-manager"></a>En introduktion tooStorSimple Snapshot Manager

## <a name="overview"></a>Översikt
StorSimple Snapshot Manager är en snapin-modul i Microsoft Management Console (MMC) som förenklar dataskydd och hantering av säkerhetskopiering i en miljö med Microsoft Azure StorSimple. Med StorSimple Snapshot Manager hantera Microsoft Azure StorSimple-data i hello datacenter och i molnet hello som en enda integrerad lagringslösning därmed förenkla processer för säkerhetskopiering och minska kostnaderna.

Den här översikten beskriver hello StorSimple Snapshot Manager, beskriver funktioner och förklarar sin roll i Microsoft Azure StorSimple. 

En översikt över hello hela Microsoft Azure StorSimple-system, inklusive hello StorSimple-enhet, StorSimple Manager-tjänst, StorSimple Snapshot Manager och StorSimple nätverkskort för SharePoint, se [StorSimple 8000-serien: ett hybridmoln lagringslösning](storsimple-overview.md). 

> [!NOTE]
> * Du kan inte använda StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuell matriser (även kallat StorSimple lokala virtuella enheter).
> * Om du planerar tooinstall StorSimple uppdatering 2 på StorSimple-enheten vara säker på att toodownload hello senaste versionen av StorSimple Snapshot Manager och installera den **innan du installerar StorSimple uppdatering 2**. hello senaste versionen av StorSimple Snapshot Manager är bakåtkompatibla och fungerar med alla versioner av Microsoft Azure StorSimple. Om du använder hello tidigare version av StorSimple Snapshot Manager behöver du tooupdate it (du inte behöver toouninstall hello tidigare versionen innan du installerar en ny version av hello).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager syfte och arkitektur
StorSimple Snapshot Manager ger en central hanteringskonsol som du kan använda toocreate konsekvent, point-in-time-säkerhetskopior av lokala och molnet data. Du kan till exempel använda hello-konsolen för att:

* Konfigurera, säkerhetskopiera och ta bort volymer.
* Konfigurera volym grupper tooensure säkerhetskopierade data är programkonsekventa.
* Hantera principer för säkerhetskopiering så att data säkerhetskopieras enligt ett förutbestämt schema.
* Skapa lokala och molnbaserade ögonblicksbilder som kan lagras i hello molnet och används för katastrofåterställning.

Hej StorSimple Snapshot Manager hämtar hello lista över program som är registrerade med hello VSS-provider på hello värden. Sedan toocreate programkonsekvent säkerhetskopiering, kontrollerar den hello volymer används av ett program och ger förslag på volymen grupper tooconfigure. StorSimple Snapshot Manager använder dessa volym grupper toogenerate säkerhetskopior som är programkonsekventa. (Programkonsekvens finns när alla relaterade filer och databaser som ska synkroniseras och representerar hello true tillståndet för hello-programmet vid en viss tidpunkt.) 

StorSimple Snapshot Manager säkerhetskopieringar ta hello form av inkrementell ögonblicksbilder som avbildar endast hello ändringar sedan den senaste säkerhetskopieringen av hello. Därför kan säkerhetskopior kräver mindre lagringsutrymme och skapas och återställs snabbt. StorSimple Snapshot Manager använder hello Windows Volume Shadow Copy Service (VSS) tooensure att ögonblicksbilder avbilda programkonsekvent data. (Mer information finns i toohello integrering med Windows Volume Shadow Copy Service-avsnitt.) Med StorSimple Snapshot Manager som du kan skapa scheman för säkerhetskopiering eller vidta omedelbara säkerhetskopieringar efter behov. Om du behöver toorestore data från en säkerhetskopiering, StorSimple Snapshot Manager kan välja du från en katalog i lokala eller molnögonblicksbilder. Azure StorSimple återställer endast hello data som krävs när det behövs, vilket förhindrar fördröjningar i datatillgänglighet under återställning.)

![Arkitektur för StorSimple Snapshot Manager](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**Arkitektur för StorSimple Snapshot Manager** 

## <a name="support-for-multiple-volume-types"></a>Stöd för flera volymtyper av
Du kan använda hello StorSimple Snapshot Manager tooconfigure och säkerhetskopiera hello följande typer av volymer: 

* **Enkla volymer** – en enkel volym är en enskild partition på en standarddisk. 
* **Enkla volymer** – en enkel volym är en dynamisk volym som innehåller diskutrymmet från en enda dynamisk disk. En enkel volym består av en enda region på en disk eller flera regioner som är länkade på hello samma disk. (Du kan skapa enkla volymer på dynamiska diskar.) Enkla volymer är inte feltoleranta.
* **Dynamiska volymer** – en dynamisk volym är en volym som har skapats på en dynamisk disk. Dynamiska diskar använder ett databasen tootrack informationen om volymer som finns på dynamiska diskar på en dator. 
* **Dynamiska volymer med spegling** – dynamiska volymer med spegling bygger på hello RAID 1 arkitektur. Med RAID 1 skrivs identiska data på två eller flera disk, producerar en speglad uppsättning. En läsbegäran kan sedan hanteras av en disk som innehåller hello begärde data.
* **Klusterdelade volymer** – med klusterdelade volymer (CSV), kan flera noder i ett redundanskluster samtidigt läsa eller skriva toohello samma disk. Växling vid fel från en nod tooanother nod kan inträffa snabbt, utan en ändring i äganderätt till enhet eller montera demontera och ta bort en volym. 

> [!IMPORTANT]
> Blanda inte CSV: er och icke-CSV: er i hello samma ögonblicksbild. Blandning av CSV: er och icke-CSV: er i en ögonblicksbild stöds inte. 
> 
> 

Du kan använda grupper för StorSimple Snapshot Manager toorestore hela volymen eller klona enskilda volymer och återställa enskilda filer.

* [Volymer och volymen grupper](#volumes-and-volume-groups) 
* [Typer av säkerhetskopiering och principer för säkerhetskopiering](#backup-types-and-backup-policies) 

Mer information om funktioner för StorSimple Snapshot Manager och hur toouse dem, se [StorSimple Snapshot Manager användargränssnittet](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Volymer och volymen grupper
Med StorSimple Snapshot Manager som du skapar volymer och konfigurera dem i grupper på volymen. 

StorSimple Snapshot Manager använder volymen grupper toocreate säkerhetskopior som är programkonsekventa. Det finns programkonsekvens när alla relaterade filer och databaser som ska synkroniseras och representerar hello true status för ett program vid en viss tidpunkt. Volymen grupper (som kallas även *konsekvenskontroll grupper*) utgör hello grund av en säkerhetskopia eller återställa jobb.

Volymen grupper är inte hello samma som volymbehållare. En volymbehållare innehåller en eller flera volymer som delar ett lagringskonto i molnet och andra attribut, till exempel kryptering och bandbredd. En enda volymbehållare kan innehålla upp too256 tunt allokerad StorSimple-volymer. Mer information om volymbehållare gå för[hantera din volymbehållare](storsimple-manage-volume-containers.md). Volymen grupper är samlingar av volymer som du konfigurerar toofacilitate säkerhetskopieringsåtgärder. Om du markerar två volymer som tillhör toodifferent volymbehållare, placerar dem på en enda volym-grupp och sedan skapa en princip för säkerhetskopiering för den volym gruppen ska varje volym säkerhetskopieras i hello lämplig volym-behållaren som använder hello lämplig lagring konto.

> [!NOTE]
> Alla volymer i en volym-grupp måste komma från en enda molntjänstleverantören.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integrering med Windows Volume Shadow Copy-tjänsten
StorSimple Snapshot Manager använder hello Windows Volume Shadow Copy Service (VSS) toocapture programkonsekvent data. VSS underlättar programkonsekvens genom att kommunicera med VSS-medvetna program toocoordinate hello inkrementell att ögonblicksbilder skapas. VSS säkerställer att hello program är tillfälligt inaktiv eller overksamt, när ögonblicksbilder tas. 

Hej StorSimple Snapshot Manager implementering av VSS fungerar med SQL Server och allmänna NTFS-volymer. hello-processen är som följer: 

1. En begärande som är vanligtvis en datahantering och lösning för skydd av (till exempel StorSimple Snapshot Manager) eller ett program för säkerhetskopiering, anropar VSS och frågar den toogather information från hello writer programvara i hello målprogrammet.
2. VSS-kontakter hello writer komponenten tooretrieve en beskrivning av hello data. hello writer returnerar hello beskrivning av hello data toobe säkerhetskopieras. 
3. VSS signalerar hello writer tooprepare hello program för säkerhetskopiering. hello writer förbereder hello data för säkerhetskopiering genom att öppna transaktioner uppdaterar transaktionsloggar och så vidare och meddelar VSS.
4. VSS instruerar hello writer tootemporarily stoppa hello programdata lagras och se till att inga data skrivs toohello volym medan hello skuggkopian skapats. Det här steget säkerställer datakonsekvens och tar mer än 60 sekunder.
5. VSS instruerar hello providern toocreate hello skuggkopian. Providers, vilket kan vara program - eller maskinvarubaserade, hantera hello volymer som för närvarande körs och skapa skuggkopior av dem på begäran. hello-providern skapar hello skuggkopian och meddelar VSS när den har slutförts.
6. VSS kontakter hello writer toonotify hello program som kan återuppta i/o och tooconfirm att i/o paus under shadow kopiera skapas. 
7. Om hello kopiera lyckades VSS returnerar hello kopians plats toohello begärande. 
8. Om data skrevs medan hello skuggkopian skapats, kommer hello säkerhetskopian vara inkonsekvent. VSS tar bort hello skuggkopian och meddelar hello begärande. hello begärande kan upprepa processen för att säkerhetskopiera hello automatiskt eller meddela Hej administratör tooretry den vid ett senare tillfälle.

Se följande illustration hello.

![VSS-processen](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Tjänsten Windows Volume Shadow Copy-processen** 

## <a name="backup-types-and-backup-policies"></a>Typer av säkerhetskopiering och principer för säkerhetskopiering
Med StorSimple Snapshot Manager kan du säkerhetskopiera data och lagrar den lokalt och i molnet hello. Du kan använda StorSimple Snapshot Manager tooback in data direkt eller du kan använda en princip för säkerhetskopiering toocreate ett schema för säkerhetskopiering automatiskt. Principer för säkerhetskopiering kan du också toospecify hur många ögonblicksbilder ska behållas. 

### <a name="backup-types"></a>Typer av säkerhetskopiering
Du kan använda StorSimple Snapshot Manager toocreate hello följande typer av säkerhetskopiering:

* **Lokala ögonblicksbilder** – lokala ögonblicksbilder är i tidpunkt kopior av volymens data som lagras på hello StorSimple-enhet. Normalt kan den här typen av säkerhetskopiering skapas och återställs snabbt. Du kan använda en lokal ögonblicksbild som en lokal säkerhetskopia.
* **Molnbaserade ögonblicksbilder** – molnögonblicksbilder är i tidpunkt kopior av volymens data som lagras i hello molnet. En ögonblicksbild i molnet motsvarar tooa ögonblicksbild replikeras på ett annat, externt lagringssystem. Molnögonblicksbilder är särskilt användbart i katastrofåterställning.

### <a name="on-demand-and-scheduled-backups"></a>På begäran och schemalagda säkerhetskopieringar
Med StorSimple Snapshot Manager kan du initiera en engångsförbikoppling säkerhetskopiering toobe omedelbart eller du kan använda en princip för säkerhetskopiering tooschedule återkommande säkerhetskopieringsåtgärder.

En princip för säkerhetskopiering är en uppsättning automatiska regler som du kan använda tooschedule regelbundna säkerhetskopieringar. En princip för säkerhetskopiering kan du toodefine hello ofta och parametrar för att ta ögonblicksbilder av en viss volym-grupp. Du kan använda principer toospecify start- och utgångsdatum datum, tider, frekvenser och krav på datalagring, för både lokala och molnbaserade ögonblicksbilder. En princip tillämpas omedelbart när du har definierat den. 

Du kan använda StorSimple Snapshot Manager tooconfigure eller konfigurera om principer för säkerhetskopiering när så krävs. 

Du kan konfigurera följande information för varje princip för säkerhetskopiering som du skapar hello:

* **Namnet** – hello unika namn hello valt princip för säkerhetskopiering.
* **Typen** – hello typ av princip för säkerhetskopiering, antingen lokal ögonblicksbild eller ögonblicksbild i molnet.
* **Volymen grupp** – hello volym toowhich hello valt säkerhetskopiering Grupprincip är tilldelad.
* **Kvarhållning** – hello antal säkerhetskopior tooretain. Om du markerar hello **alla** rutan alla säkerhetskopior bevaras tills hello maximalt antal säkerhetskopior per volym har uppnåtts, vid vilken punkt hello principen misslyckas och genererar ett felmeddelande. Alternativt kan du ange ett antal säkerhetskopior tooretain (mellan 1 och 64).
* **Datum** – hello datumet då hello säkerhetskopieringsprincip skapades.

Information om hur du konfigurerar principer för säkerhetskopiering finns för[Använd StorSimple Snapshot Manager toocreate och hantera principer för säkerhetskopiering](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Säkerhetskopieringsjobbet övervakning och hantering
Du kan använda hello StorSimple Snapshot Manager toomonitor och hantera kommande planerade och slutförda säkerhetskopieringsjobb. Dessutom ger StorSimple Snapshot Manager en översikt över too64 slutförts säkerhetskopiering. Du kan använda hello katalogen toofind och återställa volymer eller enskilda filer. 

Information om hur du övervakar säkerhetskopieringsjobb finns för[Använd StorSimple Snapshot Manager tooview och hantera säkerhetskopieringsjobb](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [med hjälp av StorSimple Snapshot Manager tooadminister StorSimple-lösningen](storsimple-snapshot-manager-admin.md).
* Hämta [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).

