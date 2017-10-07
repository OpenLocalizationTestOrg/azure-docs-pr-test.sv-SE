---
title: "aaaBest praxis för virtuell StorSimple-matris | Microsoft Docs"
description: "Beskriver hello bästa praxis för att distribuera och hantera hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2017
ms.author: alkohli
ms.openlocfilehash: f242364365e07f86b78e2f9bb2acfb3aa98e83e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-best-practices"></a>Metodtips för virtuell StorSimple-matris
## <a name="overview"></a>Översikt
Microsoft Azure StorSimple virtuell matrisen är en integrerad lagringslösning som hanterar lagringsuppgifter mellan en lokal virtuell enhet körs i ett hypervisor-programmet och Microsoft Azure-molnlagring. StorSimple virtuell matrisen är en effektiv och mer kostnadseffektivt alternativa toohello 8000-serien fysiska matris. hello virtuella matris kan köras på din befintliga infrastruktur för hypervisor-programmet stöder både hello iSCSI och hello SMB-protokoll och lämpar sig väl för scenarier med fjärråtkomst office/avdelningskontor. Mer information om hello StorSimple lösningar finns för[översikt över Microsoft Azure StorSimple](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Den här artikeln beskriver hello metodtips implementeras under hello installationen, distribution och hantering av hello virtuella StorSimple-matris. Följande rekommendationer innehåller validerade riktlinjer för hello-installation och hantering av virtuella matrisen. Den här artikeln är riktad mot hello IT-administratörer som distribuerar och hanterar hello virtuella matriser i sina datacenter.

Vi rekommenderar en regelbunden granskning av hello bästa praxis toohelp se till att enheten är fortfarande i kompatibilitet när ändringar görs toohello installations- eller flöda. Ska det uppstår problem vid implementera tipsen på din virtuella matrisen [kontaktar Microsoft Support](storsimple-virtual-array-log-support-ticket.md) om du behöver hjälp.

## <a name="configuration-best-practices"></a>Metodtips för konfiguration
Följande rekommendationer omfatta hello riktlinjer som behöver toobe vid hello inledande installation och distribution av virtuella hello-matriser. Följande rekommendationer innehåller de relaterade toohello etablering av virtuella hello, grupprincipinställningar, storlek, ställa in hello nätverk, konfigurera storage-konton och skapa filresurser och volymer för hello virtuella matris. 

### <a name="provisioning"></a>Etablering
StorSimple virtuell matrisen är en virtuell dator (VM) som har etablerats på hello hypervisor (Hyper-V eller VMware) på värdservern. Etablera hello virtuell dator, kontrollera att värden kan toodedicate tillräckligt med resurser. Mer information finns för[minsta resurskraven](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) tooprovision en matris.

Implementera hello följa bästa praxis vid etablering av virtuella hello-matris:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Typ av virtuell dator** |**Generation 2** virtuell dator för användning med Windows Server 2012 eller senare och en *.vhdx* bild. <br></br> **Generation 1** virtuell dator för användning med Windows Server 2008 eller senare och en *VHD* bild. |Använd virtuella version 8-11 när du använder *.vmdk* bild. |
| **Typ av minne** |Konfigurera som **statiskt minne**. <br></br> Använd inte hello **dynamiskt minne** alternativet. | |
| **Datatypen för disk** |Tillhandahålla som **dynamiskt expanderande**.<br></br> **En fast storlek** tar lång tid. <br></br> Använd inte hello **differentierande** alternativet. |Använd hello **tunn etablering** alternativet. |
| **Disk dataändring** |Utökningen eller krympa tillåts inte. Ett försök toodo leder så hello förlust av alla hello lokala data på enheten. |Utökningen eller krympa tillåts inte. Ett försök toodo leder så hello förlust av alla hello lokala data på enheten. |

### <a name="sizing"></a>Storlek
Tänk hello följande faktorer när du ändrar storlek på din virtuella StorSimple-matris:

* Lokala reservation för volymer eller resurser. Cirka 12% hello utrymme har reserverats på hello lokala nivån för varje etablerade nivåindelad volym eller resurs. 10% av hello utrymme är ungefär också reserverat för en lokalt Fäst volym för filsystem.
* Snapshot-omkostnader. Ungefär är 15% utrymme på hello lokala nivån reserverat för ögonblicksbilder.
* Behovet av återställningar. Om utför återställning som en ny operation bör storlek hänsyn till hello utrymmet som krävs för återställning. Återställning görs tooa resursen eller volymen av hello samma storlek.
* Vissa buffert bör tilldelas för eventuella oväntade tillväxt.

Baserat på hello föregående faktorer kan hello storleksanpassa krav representeras av hello följande formel:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

hello följande exempel visar hur du kan ändra storlek på en virtuell matris baserat på dina krav.

#### <a name="example-1"></a>Exempel 1:
På din virtuella matrisen önskade toobe kunna

* etablera en 2 TB nivåindelad volym eller resurs.
* etablera en 1 TB nivåindelad volym eller resurs.
* etablera en lokalt Fäst volym 300 GB eller filresurs.

För hello Låt föregående volymer eller resurser, oss beräkna hello kraven på diskutrymme på hello lokala nivån.

För varje nivåindelad volym/resurs, först blir lokala reservation lika too12% av hello volym/dela storlek. För hello lokalt Fäst volym/dela är lokala reservationen 10% av hello lokalt Fäst volym/dela storlek (dessutom toohello etablerats storlek). I det här exemplet behöver du

* 240 GB lokala reservation (2 TB skikt i volym/dela)
* 120 GB lokala reservation (skikt volym/dela för 1 TB)
* 330 GB för lokalt Fäst volym eller resurs (lägga till 10% av lokala toohello 300 GB etableras Reservationsstorleken)

hello totalt utrymme som krävs på hello lokala nivån hittills är: 240 GB + 120 GB + 330 GB = 690 GB.

Dessutom måste minst lika mycket utrymme på hello lokala nivån som hello största enskild reservation. Den här extra belopp används om du behöver toorestore från en ögonblicksbild i molnet. I det här exemplet hello största lokala reservationen är 330 GB (inklusive reservation för file system), så lägger du till att toohello 690 GB: 690 GB + 330 GB = 1020 GB.
Om vi har genomfört efterföljande ytterligare återställningar kan vi alltid frigöra hello utrymmet från hello föregående återställningsåtgärd.

Det tredje vi behöver 15% av den totala lokalt utrymme hittills toostore lokala ögonblicksbilder, så att endast 85% av den är tillgänglig. I det här exemplet som skulle vara runt 1020 GB = 0.85&ast;etablerade data disk TB. Därför hello etablerade datadisk skulle vara (1020&ast;(1/0.85)) = 1200 GB = 1,20 TB ~ 1,25 TB (avrundning av toonearest KVARTIL)

Factoring i oväntat tillväxt och nya återställningar, bör du etablerar en lokal disk av runt 1,25-1,5 TB.

> [!NOTE]
> Vi rekommenderar också att hello lokal disk är tunt etablerad. Denna rekommendation beror hello återställning utrymme behövs bara om du vill toorestore data som är äldre än fem dagar. Objektnivååterställning kan du toorestore data för hello sista fem dagar utan hello extra utrymme för återställning.


#### <a name="example-2"></a>Exempel 2:
På din virtuella matrisen önskade toobe kunna

* etablera en 2 TB nivåer volym
* etablera en 300 GB lokalt Fäst volym

Baserat på 12% lokalt utrymme reservation för nivåindelade volymer-resurser och 10% för lokalt fästa volymer-resurser, behöver vi

* 240 GB lokala reservation (2 TB skikt i volym/dela)
* 330 GB för lokalt Fäst volym eller resurs (lägga till 10% av lokala reservation toohello 300 GB allokerade utrymme)

Totalt utrymme som krävs på hello lokala nivån är: 240 GB + 330 GB = 570 GB

hello minsta lokalt utrymme behövs för återställning är 330 GB.

15% av totalt disken är används toostore ögonblicksbilder så att endast 0.85 är tillgänglig. Hello diskens storlek är därför, (900&ast;(1/0.85)) = 1,06 TB ~ 1,25 TB (avrundning av toonearest KVARTIL)

Du kan etablera en lokal disk 1,25-1,5 TB factoring i eventuella oväntade tillväxt.

### <a name="group-policy"></a>Grupprincip
Grupprincip är en infrastruktur som låter dig tooimplement specifika konfigurationer för användare och datorer. Grupprincipinställningar finns i grupprincipobjekt (GPO) som är länkade toohello följande Active Directory Domain Services (AD DS)-behållare: platser, domäner eller organisationsenheter (OU). 

Om din virtuella matris är ansluten till domänen, kan grupprincipobjekt vara tillämpade tooit. Dessa grupprincipobjekt kan installera program, till exempel ett antivirusprogram som kan påverka hello driften av hello virtuella StorSimple-matris negativt.

Därför rekommenderar vi att du:

* Se till att virtuella matrisen i sin egen organisationsenhet (OU) för Active Directory.
* Se till att inga grupprincipobjekt (GPO) är tillämpade tooyour virtuella matris. Du kan blockera arv tooensure som hello virtuella matris (underordnad nod) inte ärver grupprincipobjekt automatiskt från hello överordnad. Mer information finns för[blockera arv](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Nätverk
hello nätverkskonfigurationen för din virtuella matrisen görs via hello lokala webbgränssnittet. Ett virtuellt nätverksgränssnitt aktiveras via hello hypervisor som hello virtuella matris har etablerats. Använd hello [nätverksinställningar](storsimple-virtual-array-deploy3-fs-setup.md) tooconfigure hello virtuellt nätverk gränssnittet IP-adress, nätmask och gateway.  Du kan också konfigurera hello primära och sekundära DNS-server och tidsinställningar proxyinställningar för din enhet. De flesta av hello nätverkskonfigurationen är en gång. Granska hello [StorSimple nätverkskrav](storsimple-ova-system-requirements.md#networking-requirements) tidigare toodeploying hello virtuella matris.

Vi rekommenderar att du följa dessa rekommendationer när du distribuerar virtuella matrisen:

* Kontrollera att hello-nätverk i vilka hello distribuerade virtuella matris alltid har hello kapacitet toodedicate 5 Mbit/s Internet-bandbredd (eller mer).
  
  * Behovet av Internet-bandbredd varierar beroende på din arbetsbelastning egenskaper och hello ofta data ändras.
  * ändring av hello data som kan hanteras är proportionell tooyour Internet-bandbredd. Exempelvis när du tar en säkerhetskopia av kan en 5 Mbit/s bandbredd hantera cirka 18 GB data ändras i 8 timmar. Med fyra gånger så mycket bandbredd (20 Mbit/s), kan du hantera ändring av fyra gånger mer data (72 GB).
* Kontrollera anslutningen toohello Internet alltid är tillgängligt. Sporadiska eller otillförlitliga Internet-anslutningar toohello enheter kan resultera i förlust av åtkomst toodata i hello molnet och kan resultera i en konfiguration som inte stöds.
* Om du planerar toodeploy enheten som en iSCSI-server:
  
  * Vi rekommenderar att du inaktiverar hello **hämta IP-adress automatiskt** alternativet (DHCP).
  * Konfigurera statiska IP-adresser. Du måste konfigurera en primär och en sekundär DNS-server.
  * Om definiera flera nätverksgränssnitt på din virtuella matrisen endast hello första nätverksgränssnitt (det här gränssnittet är som standard **Ethernet**) kan nå hello molnet. toocontrol hello typ av trafik, kan du skapa flera virtuella nätverksgränssnitt på din virtuella matriskonfiguration (som en iSCSI-server) och ansluta dessa gränssnitt toodifferent undernät.
* toothrottle hello molnet bandbredd (används av virtuella hello-matris), konfigurera begränsning på hello router eller hello brandvägg. Om du definierar begränsning i din hypervisor-program, kommer den begränsning alla hello protokoll inklusive iSCSI och SMB i stället för bara hello molnet bandbredd.
* Se till att tidssynkronisering för hypervisorer är aktiverat. Om du använder Hyper-V väljer virtuella matrisen i hello Hyper-V Manager, gå för**inställningar &gt; Integration Services**, och se till att hello **tidssynkronisering** är markerad.

### <a name="storage-accounts"></a>Lagringskonton
StorSimple virtuell matris kan associeras med ett enda storage-konto. Det här lagringskontot kan vara ett automatiskt genererat storage-konto, ett konto i hello samma prenumeration som hello tjänst eller ett lagringskonto relaterade tooanother prenumeration. Mer information finns i hur för[hantera storage-konton för din virtuella matrisen](storsimple-virtual-array-manage-storage-accounts.md).

Använd hello följande rekommendationer för lagringskonton som är kopplade till virtuella matrisen.

* När du länkar flera virtuella matriser med ett enda lagringskonto hänsyn till hello maximal kapacitet (64 TB) för en virtuell matris och maxstorleken hello (500 TB) för ett lagringskonto. Detta begränsar hello antal fullskaligt virtuella matriser som kan associeras med den storage-konto tooabout 7.
* När du skapar ett nytt lagringskonto
  
  * Vi rekommenderar att du skapar den i hello region närmaste toohello office/filialkontor där din virtuella StorSimple-matrisen är distribuerade toominimize svarstider.
  * Tänk dessutom på att du inte kan flytta ett lagringskonto över olika regioner. Du kan också flytta en tjänst för alla prenumerationer.
  * Använd ett lagringskonto som implementerar redundans mellan hello datacenter. GEO-Redundant lagring (GRS), zonen Redundant lagring (ZRS) och lokalt Redundant lagring (LRS) stöds för användning med virtuella matrisen. Mer information om hello olika typer av lagringskonton går för[Azure storage-replikering](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Filresurser och volymer
Du kan etablera resurser när den är konfigurerad som en filserver och volymer när konfigurerad som en iSCSI-server för din virtuella StorSimple-matris. hello bästa praxis för att skapa filresurser och volymer är relaterade toohello storlek och hello typen konfigurerats.

#### <a name="volumeshare-size"></a>Volym/dela storlek
Du kan etablera resurser när den är konfigurerad som en filserver och volymer när konfigurerad som en iSCSI-server på din virtuella matrisen. hello bästa praxis för att skapa filresurser och volymer rör toohello storlek och hello typen av konfigurerad. 

Kom ihåg hello följa bästa praxis vid etablering av resurser eller volymer på din virtuella enhet.

* Hej lagringsnivåer prestanda kan påverkas av hello storlekar relativa toohello etablerats filstorleken för en skiktad resurs. Arbeta med stora filer kan resultera i en långsam nivån. När du arbetar med stora filer, rekommenderar vi att största hello-filen är mindre än 3% av hello resursen storlek.
* Högst 16 volymer-resurser kan skapas på virtuella hello-matrisen. För hello storleksgränser för hello lokalt Fäst och nivåer volymer/resurser, refererar alltid toohello [virtuella StorSimple-matris begränsar](storsimple-ova-limits.md).
* När du skapar en volym, faktor i hello förväntade data förbrukning samt framtida tillväxt. hello volymen kan inte expanderas senare.
* När hello volymen har skapats, kan du minska hello storleken på hello volym på StorSimple.
* När du skriver tooa nivåer volymen på StorSimple, när hello volymdata når ett visst tröskelvärde (relativa toohello lokala utrymme som är reserverat för hello volym), begränsas hello-i/o. Fortsätter toowrite toothis volym långsammare hello IO avsevärt. Även om du kan skriva tooa nivåer volym utöver dess etablerad kapacitet (vi inte aktivt stoppar hello användaren skriver utöver hello etablerad kapacitet) ser du en avisering toohello effekt som du har övertecknat. När du ser hello aviseringen, är det viktigt att du vidtar åtgärder, till exempel ta bort hello volymdata (volym expansion är för närvarande stöds inte).
* För användningsområden för disaster recovery hello antalet tillåtna resurser-volymer är 16 och hello högsta antalet resurser/volymer som kan bearbetas parallellt är också 16, hello antalet resurser/volymer har inte betydelse i din Återställningspunktmål och RTOs.

#### <a name="volumeshare-type"></a>Volym/dela-typ
StorSimple stöder två volym/dela typer baserat på hello användning: lokalt Fäst och nivåer. Lokalt fästa volymer-resurser etableras tjockt, vilket medan hello nivåindelade volymer-resurser är tunt allokerad. Du kan inte konvertera en lokalt Fäst volym/dela tootiered eller *tvärtom* skapas en gång.

Vi rekommenderar att du implementera hello följande metodtips när du konfigurerar StorSimple-volymer/resurser:

* Identifiera hello volymtyp baserat på hello arbetsbelastningar som du avser toodeploy innan du skapar en volym. Använd lokalt fästa volymer för arbetsbelastningar som kräver lokala garantier data (även under ett avbrott i molnet) och som kräver molnet låg latens. När du skapar en volym på din virtuella matrisen kan du inte ändra hello volymtyp från lokalt Fäst tootiered eller *vice versa*. Skapa lokalt fästa volymer som exempel, när du distribuerar SQL arbetsbelastningar eller arbetsbelastningar som är värd för virtuella datorer (VM) Använd nivåindelade volymer för filen resursen arbetsbelastningar.
* Kontrollera hello alternativet för mindre vanliga arkiveringsdata när du hanterar mycket stora filer. En större segmentstorlek för deduplicering på 512 kB används när det här alternativet aktiveras tooexpedite hello dataöverföring toohello moln.

#### <a name="volume-format"></a>Volymen format
När du har skapat StorSimple-volymer på iSCSI-servern måste tooinitialize, montera och formatera hello volymer. Den här åtgärden utförs på hello värddatorn ansluten tooyour StorSimple-enhet. Följande metodtips rekommenderas när montera och formatera volymer på hello StorSimple värden.

* Utför en snabbformatering på alla StorSimple-volymer.
* Formaterar en StorSimple-volym och använda en storlek på allokeringsenhet (Australien) på 64 KB (standard är 4 KB). hello baseras 64 KB Australien på Testa görs inom företaget för vanliga arbetsbelastningar på StorSimple och andra arbetsbelastningar.
* När du använder hello StorSimple virtuell matriskonfiguration som en iSCSI-server, Använd inte volymer eller dynamiska diskar som dessa volymer eller diskar stöds inte av StorSimple.

#### <a name="share-access"></a>Åtkomst till resursen
När du skapar resurser på filservern virtuella matris Följ dessa riktlinjer:

* När du skapar en resurs kan du tilldela en användargrupp som administratör av en resurs i stället för en enskild användare.
* Du kan hantera hello NTFS-behörigheter när hello resursen har skapats genom att redigera hello resurser via Utforskaren.

#### <a name="volume-access"></a>Åtkomst till en volym
När du konfigurerar hello iSCSI-volymer på din virtuella StorSimple-matrisen är viktiga toocontrol åtkomst behov. toodetermine som är värd för servrar kan komma åt volymer, skapar och associerar access control-poster (ACRs) med StorSimple-volymer.

Använd följande metodtips när du konfigurerar ACRs för StorSimple-volymer hello:

* Koppla alltid minst en ACR till en volym.

* När du tilldelar mer än en ACR tooa volym, kontrollera hello volym inte exponeras på ett sätt där den kan samtidigt användas av fler än en icke-klustrad värd. Om du har tilldelat flera ACRs tooa volym, visas ett varningsmeddelande för du tooreview din konfiguration.

### <a name="data-security-and-encryption"></a>Datasäkerhet och kryptering
Din virtuella StorSimple-matris har data säkerhet och kryptering funktioner som Kontrollera hello sekretess och integritet för dina data. När du använder dessa funktioner, bör du följa dessa rekommendationer: 

* Definiera en cloud storage viktiga toogenerate AES 256 kryptering innan hello data skickas från ditt virtuella matris toohello moln. Den här nyckeln är inte nödvändigt om dina data är krypterade toobegin med. hello key kan skapas och förblir säkra med en nyckelhanteringssystem [Azure key vault](../key-vault/key-vault-whatis.md).
* När du konfigurerar hello lagringskonto via hello StorSimple Manager-tjänsten, se till att du aktiverar hello SSL-läge toocreate en säker kanal för nätverkskommunikation mellan din StorSimple-enhet och hello moln.
* Generera hello nycklar för storage-konton (genom att öppna hello Azure Storage service) med jämna mellanrum tooaccount för alla ändringar du tooaccess baserat på hello ändra listan över administratörer.
* Data på virtuella matrisen komprimeras och deduplicerade innan den skickas tooAzure. Vi rekommenderar inte med rolltjänsten för hello Datadeduplicering på Windows Server-värd.

## <a name="operational-best-practices"></a>Metodtips för fortlöpande
hello operativa metodtips finns riktlinjer som ska följas under eller hello dagliga hanteringen av virtuella hello-matris. Praxis täcka specifika hanteringsuppgifter, till exempel säkerhetskopiering, återställning från en säkerhetskopia, utför en växling vid fel, inaktivera och ta bort hello-matris, övervakning systemanvändning och hälsotillstånd, och körs virus genomsökningar på din virtuella matrisen.

### <a name="backups"></a>Säkerhetskopior
hello data på din virtuella matrisen säkerhetskopieras toohello moln på två sätt, standard automatiserad daglig säkerhetskopiering av hello hela enheten startar klockan 22.30 eller via en manuell säkerhetskopiering på begäran. Som standard skapar hello enheten automatiskt dagliga molnögonblicksbilder av alla hello data som finns på den. Mer information finns för[säkerhetskopiera din virtuella StorSimple-matris](storsimple-virtual-array-backup.md).

hello frekvens och lagring som är associerade med hello standard säkerhetskopieringar kan inte ändras, men du kan konfigurera hello tid vid vilken hello initieras daglig säkerhetskopiering varje dag. När du konfigurerar hello starttiden för hello automatisk säkerhetskopiering, rekommenderar vi att:

* Schemalägga säkerhetskopiorna för låg belastning. Starttid för säkerhetskopiering bör inte sammanfaller med flera i/o-värden.
* Starta en manuell säkerhetskopiering på begäran när du planerar tooperform en enhet med växling vid fel eller tidigare toohello underhållsperiod, tooprotect hello data på virtuella matrisen.

### <a name="restore"></a>Återställ
Du kan återställa från en säkerhetskopia på två sätt: Återställ tooanother volym eller resurs eller utföra en återställning (endast tillgängligt på en virtuell matris som konfigurerats som en filserver). Objektnivååterställning kan toodo en detaljerad återställning av filer och mappar från en säkerhetskopiering i molnet över alla resurser som hello på hello StorSimple-enhet. Mer information finns för[återställa från en säkerhetskopia](storsimple-virtual-array-clone.md).

När du utför en återställning, hålla hello följande riktlinjer i åtanke:

* Din virtuella StorSimple-matrisen har inte stöd för återställning på plats. Detta kan dock vara lätt uppnås genom en tvåstegsprocess: Frigör diskutrymme på hello virtuella matrisen och sedan återställa tooanother volym/dela.
* När du återställer från en lokal volym, kommer Kom ihåg hello restore att långvarig åtgärd. Även om hello volymen kan snabbt anslutits fortsätter hello data toobe ur i hello bakgrund.
* hello volym typen förblir hello samma under hello återställningsprocessen. En nivåindelad volym återställs tooanother nivåer volym och en lokalt Fäst volym tooanother lokalt Fäst volym.
* När försök toorestore en volym eller en resurs från en säkerhetskopia om hello återställningsjobbet misslyckas, en målvolym eller resurs kan fortfarande skapas i hello-portalen. Det är viktigt att du tar bort den här oanvända målvolymen eller dela i hello portal toominimize eventuella framtida problem som härrör från det här elementet.

### <a name="failover-and-disaster-recovery"></a>Redundans och katastrofåterställning
En enhet redundans kan du toomigrate dina data från en *källa* enhet i hello datacenter tooanother *mål* enhet finns i hello samma eller en annan geografisk plats. hello enheten redundans är för hello hela enheten. Hello molndata för hello källenheten ändras under växling vid fel, ägarskap toothat av hello målenhet.

För din virtuella StorSimple-matris kan du endast redundans tooanother virtuella matrisen hanteras av hello samma StorSimple Manager-tjänsten. En växling vid fel tooan 8000-serieenhet eller en matris som hanteras av en annan StorSimple Manager-tjänst (än hello en för hello källenheten) är inte tillåtet. Gå toolearn mer om hello redundans överväganden för[krav för hello enheten redundans](storsimple-virtual-array-failover-dr.md).

När du utför en misslyckas över för din virtuella matrisen bör du tänka hello följande:

* För en planerad redundans är det en rekommenderad god rutin tootake alla hello volymer/resurser offline tidigare tooinitiating hello växling vid fel. Följ instruktionerna för hello operativsystemspecifika tootake hello volymer/resurser offline på hello värd först och sedan vidta de offline på din virtuella enhet.
* För en fil server katastrofåterställning (DR) rekommenderar vi att du ansluter hello mål enheten toohello samma domän som hello källa så som hello resursbehörigheter löses automatiskt. Endast hello redundans tooa målenhet i hello samma domän som stöds i den här versionen.
* När hello DR har slutförts, tas hello källenheten bort automatiskt. Även om hello enheten inte är längre tillgänglig, förbrukar hello virtuell dator som du har etablerat hello värdsystemet fortfarande resurser. Vi rekommenderar att du tar bort den här virtuella datorn från din värd system tooprevent eventuella kostnader från uppstår.
* Att Observera att även om hello redundans misslyckas, **hello data alltid är säkert i molnet hello**. Överväg följande tre scenarier i vilka hello redundans inte slutfördes hello:
  
  * Ett fel uppstod i hello första steg i hello växling vid fel, till exempel när hello DR före systemhälsokontroller utförs. I det här fallet kan målenheten fortfarande användas. Du kan försöka hello redundans på hello samma målenhet.
  * Ett fel uppstod under hello faktiska failover-processen. I det här fallet markeras hello målenhet inte kan användas. Du måste etablera och konfigurera en annan virtuell Målmatrisen och Använd som för redundans.
  * hello redundans slutfördes efter vilken hello källenheten har tagits bort men hello målenhet har problem och du kan inte komma åt alla data. hello data är fortfarande säkert i molnet hello och enkelt kan hämtas genom att skapa en annan virtuell matris och använda det som en målenhet för hello DR.

### <a name="deactivate"></a>Inaktivera
När du inaktiverar en virtuell StorSimple-matris sever hello anslutningen mellan hello enheten och hello motsvarande StorSimple Manager-tjänsten. Inaktiveringen en **permanent** åtgärden och kan inte ångras. En inaktiverad enhet kan inte registreras med hello StorSimple Manager-tjänsten igen. Mer information finns för[inaktivera och ta bort din virtuella StorSimple-matris](storsimple-virtual-array-deactivate-and-delete-device.md).

Behåll hello följa bästa praxis i åtanke när du inaktiverar virtuella matrisen:

* Ta en ögonblicksbild i molnet för alla hello data tidigare toodeactivating en virtuell enhet. När du inaktiverar en virtuell matris alla hello lokala enhetsdata går förlorade. Tar en ögonblicksbild i molnet kan du toorecover data i ett senare skede.
* Innan du inaktiverar en virtuell StorSimple-matris, se till att toostop eller ta bort klienter och värdar som beror på enheten.
* Ta bort en inaktiverad enhet om du inte längre använder så att den inte påförs kostnader.

### <a name="monitoring"></a>Övervakning
tooensure som din virtuella StorSimple-matris i kontinuerlig felfritt tillstånd, behöver du toomonitor hello matris och se till att du får information från hello system, inklusive aviseringar. toomonitor hello övergripande hälsa för hello virtuella matris, implementera hello följande metodtips:

* Konfigurera övervakning tootrack hello diskanvändning för din virtuella matris datadisk samt hello OS-disken. Om du kör Hyper-V, kan du använda en kombination av System Center Virtual Machine Manager (SCVMM) och System Center Operations Manager (SCOM) toomonitor virtualiseringsvärdar.
* Konfigurera e-postaviseringar på din virtuella matris toosend aviseringar på vissa nivåer för användning.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Indexsökning och virus söka program
En virtuell StorSimple-matris kan automatiskt datanivå data från hello lokala nivån toohello Microsoft Azure-molnet. När ett program, till exempel en indexsökning eller en viruskontroll är används tooscan hello data som lagras på StorSimple, behöver du tootake försiktighet att hello molndata inte komma åt och dras tillbaka toohello lokala nivån.

Vi rekommenderar att du implementera följande metodtips när du konfigurerar hello sökning eller virus indexsökning på din virtuella matrisen hello:

* Inaktivera alla operationer som automatiskt konfigurerade fullständig genomsökning.
* Konfigurera hello index sökning eller virus genomsökning programmet tooperform en inkrementell skanning för nivåindelade volymer. Detta skulle skanna endast hello nya data förmodligen som finns på hello lokala nivån. hello-data som är nivåindelade toohello molnet inte kan kommas åt under en stegvis åtgärd.
* Kontrollera hello rätt sökfilter och inställningar har konfigurerats så att endast hello avsedd filtyper hämta genomsöks. Till exempel bildfiler (JPEG, GIF- och TIFF) och tekniska ritningar bör inte genomsökas under hello inkrementell eller fullständig index rebuild.

Om du använder Windows indexering process, Följ dessa riktlinjer:

* Använd inte hello Windows indexerare för nivåindelade volymer som återkallar stora mängder data (TBs) från hello molnet om hello index måste toobe ofta har återskapats. Återskapa hello indexet skulle hämta alla filen typer tooindex deras innehåll.
* Använda Windows hello-indexering process för lokalt fästa volymer eftersom detta skulle endast åtkomst till data på hello lokala nivåerna toobuild hello index (hello moln inte kommer att komma åt data).

### <a name="byte-range-locking"></a>Låsa byte-intervall
Program kan låsa ett angivet antal byte i hello-filer. Om låsning byte-intervallet är aktiverad på hello-program som skriver tooyour StorSimple, fungerar inte sedan skiktning på din virtuella matrisen. För hello lagringsnivåer toowork, ska alla delar av hello filer åt låsas. Låsa byte-intervallet stöds inte med nivåindelade volymer på din virtuella matrisen.

Rekommenderade åtgärder tooalleviate byte-intervallet låsning inkluderar:

* Inaktivera låsning i applogiken byte-intervallet.
* Använd lokalt fästa volymer (i stället för med skikt) för hello data som är associerade med det här programmet. Inte på datanivå lokalt fästa volymer i hello moln.
* När du använder lokalt Fäst volymer med byte intervallet låsning aktiverad, kan hello volym anslutas innan hello återställningen är slutförd. I så fall måste vänta du hello återställning toobe slutförts.

## <a name="multiple-arrays"></a>Flera matriser
Flera virtuella matriser kan behöva toobe distribueras tooaccount för ett växande arbetsminnet för data som kan läcker över till hello molnet därmed påverka hello prestanda för hello enhet. I dessa fall är det bästa tooscale enheter hello arbetsminnet växer. Detta kräver en eller flera enheter toobe lades till i hello lokalt datacenter. När du lägger till hello enheter, kan du:

* Dela hello aktuella uppsättning data.
* Distribuera nya arbetsbelastningar toonew enheter.
* Om du distribuerar flera virtuella matriser, rekommenderar vi att från belastningsutjämning perspektiv, distribuerar hello matris över olika hypervisor-värdar.
* Flera virtuella matriser (om konfigurerad som en filserver eller en iSCSI-server) kan distribueras i en Distributed File System Namespace. Detaljerade anvisningar finns för[Distributed File System Namespace lösning med Hybrid Cloud Storage Deployment Guide](https://www.microsoft.com/download/details.aspx?id=45507). Distributed File System Replication rekommenderas för närvarande inte för användning med hello virtuella matris. 

## <a name="see-also"></a>Se även
Lär dig hur för[administrera din virtuella StorSimple-matris](storsimple-virtual-array-manager-service-administration.md) via hello StorSimple Manager-tjänsten.

