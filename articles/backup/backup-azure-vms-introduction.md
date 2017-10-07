---
title: "aaaPlanning din infrastruktur för VM-säkerhetskopiering i Azure | Microsoft Docs"
description: "Viktiga överväganden när du planerar tooback av virtuella datorer i Azure"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Säkerhetskopiera virtuella datorer, säkerhetskopiera virtuella datorer"
ms.assetid: 19d2cf82-1f60-43e1-b089-9238042887a9
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: d11982431610000293038ee6aa7df8e7bc2d8b70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Planera din infrastruktur för VM-säkerhetskopiering i Azure
Den här artikeln innehåller prestanda och resursen förslag toohelp du planerar din infrastruktur för säkerhetskopiering av VM. Den definierar även viktiga aspekter av hello-säkerhetskopieringstjänsten; dessa aspekter kan vara avgörande för att fastställa din arkitektur kapacitetsplanering och schemaläggning. Om du har [förbereda din miljö](backup-azure-vms-prepare.md), planering är hello nästa steg innan du börjar [tooback upp virtuella datorer](backup-azure-vms.md). Om du behöver mer information om virtuella Azure-datorer, se hello [virtuella datorer dokumentationen](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Hur fungerar Azure säkerhetskopiera virtuella datorer?
När hello Azure Backup-tjänsten startar ett säkerhetskopieringsjobb hello schemalagda tidpunkt, utlöser hello reservanknytning tootake en tidpunkt i ögonblicksbild. hello Azure Backup-tjänsten använder hello _VMSnapshot_ tillägg i Windows och hello _VMSnapshotLinux_ tillägget Linux. hello tillägget installeras under hello första säkerhetskopiering. tooinstall hello tillägget hello VM måste köras. Om hello inte körs, tar en ögonblicksbild av hello underliggande lagring (eftersom inga program skrivningar sker när hello VM stoppas) hello Backup-tjänsten.

När en ögonblicksbild av virtuella Windows-datorer, samordnar hello Backup-tjänsten med hello Volume Shadow Copy Service (VSS) tooget en programkonsekvent ögonblicksbild av hello virtuella datorns diskar. Om du säkerhetskopierar virtuella Linux-datorer, kan du skriva egna anpassade skript tooensure konsekvenskontroll när en VM-ögonblicksbild. Information om att aktivera dessa skript finns senare i den här artikeln.

När hello Azure Backup-tjänsten tar hello ögonblicksbild, är hello data överförda toohello valvet. toomaximize effektivitet hello-tjänsten identifierar och överför bara hello datablock som har ändrats sedan tidigare hello-säkerhetskopiering.

![Arkitektur för virtuell dator i Azure-säkerhetskopiering](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

När hello dataöverföring är klar hello ögonblicksbild tas bort och skapa en återställningspunkt.

> [!NOTE]
> 1. Under hello säkerhetskopieringen innehåller Azure Backup hello tillfälliga disk ansluten toohello virtuella datorn. Mer information finns i hello blogg på [tillfällig lagring](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Eftersom Azure Backup tar en ögonblicksbild av en lagringsnivå och överföringar som ögonblicksbilden toovault, ändras inte hello lagringskontonycklar tills hello jobbet har slutförts.
> 3. För premium virtuella datorer kan kopiera vi hello ögonblicksbild toostorage konto. Detta är säker på att Azure Backup-tjänsten hämtar tillräcklig IOPS för att överföra data toovault toomake. Den här ytterligare en kopia av lagring debiteras enligt hello VM allokerade storleken. 
>

### <a name="data-consistency"></a>Datakonsekvens
Säkerhetskopiera och återställa affärskritiska data är komplicerade av hello faktum att affärskritiska data måste säkerhetskopieras när hello program producerar hello data körs. tooaddress detta, Azure Backup stöder programkonsekventa säkerhetskopieringar för både Windows- och Linux virtuella datorer
#### <a name="windows-vm"></a>Windows VM
Azure Backup tar Fullständig VSS-säkerhetskopiering på virtuella Windows-datorer (Läs mer om [Fullständig VSS-säkerhetskopiering](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). tooenable VSS säkerhetskopior, hello följa registret nyckel måste toobe set på hello VM.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Virtuella Linux-datorer
Azure Backup är ett ramverk för skript. tooensure programkonsekvens när du säkerhetskopierar virtuella Linux-datorer, skapa skript före och efter skript som styr hello säkerhetskopiering arbetsflödet och miljö. Azure-säkerhetskopiering anropar hello före skript före hello VM-ögonblicksbild och anropar hello efter skriptet när hello VM ögonblicksbild jobbet har slutförts. Mer information finns i [programmet konsekvent VM säkerhetskopior med hjälp av skript före och efter skriptet](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Azure-säkerhetskopiering endast anropar hello customer skrivna före och efter skript. Om hello före skript och efter skript köras, markerar Azure Backup hello återställningspunkt som programmet konsekvent. Hello kunden är slutgiltigt ansvariga för hello programkonsekvens när du använder anpassade skript.
>


Den här tabellen beskrivs hello typerna av konsekvens och hello villkor att de inträffa under Virtuella Azure-säkerhetskopiering och återställning procedurer.

| Konsekvens | VSS-baserade | Förklaring och information |
| --- | --- | --- |
| Programkonsekvens |Ja för Windows|Programkonsekvens är idealisk för arbetsbelastningar som det säkerställer att:<ol><li> hello VM *startar*. <li>Det finns *inte är skadad*. <li>Det finns *ingen dataförlust*.<li> hello data är konsekventa toohello program som använder hello data genom att hello-programmet vid hello av säkerhetskopiering – med hjälp av VSS eller före/post skript.</ol> <li>*Virtuella Windows-datorer*-mest Microsoft-arbetsbelastningar har VSS-skrivare som utför belastningsspecifikt åtgärder relaterade toodata konsekvenskontroll. Till exempel har Microsoft SQL Server en VSS-skrivare som garanterar att hello skrivningar toohello transaktionsloggfilen och hello databasen utförs korrekt. För Virtuella för Windows Azure-säkerhetskopiering, toocreate en programkonsekvent återställningspunkt hello reservanknytning anropa hello VSS arbetsflöde och slutför den innan hello VM-ögonblicksbild. För hello Azure VM ögonblicksbild toobe korrekt, hello VSS-skrivare för alla Virtuella Azure-program måste du slutföra samt. (Läs hello [grunderna i VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) och fördjupa dig djupare i hello information om [hur det fungerar](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Virtuella Linux-datorer*-kunder kan köra [anpassade skript före och efter skriptet tooensure programkonsekvens](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Filsystemskonsekvens |Ja – för Windows-baserade datorer |Det finns två scenarier där hello återställningspunkt kan vara *konsekvent filsystemet*:<ul><li>Säkerhetskopior av virtuella Linux-datorer i Azure, utan Förproduktion-script/efter-script eller om Förproduktion-script/efter-script misslyckades. <li>VSS-fel vid säkerhetskopiering för virtuella Windows-datorer i Azure.</li></ul> I båda dessa fall är hello som är bäst att göra tooensure som: <ol><li> hello VM *startar*. <li>Det finns *inte är skadad*.<li>Det finns *ingen dataförlust*.</ol> Program behöver tooimplement sina egna ”korrigering av” mekanism på hello återställa data. |
| Kraschkonsekvens |Nej |Den här situationen är likvärdiga tooa virtuella datorn upplever en ”krasch” (via antingen en mjuk eller hård reset). Kraschkonsekvens inträffar oftast när hello virtuella Azure-datorn är avstängd vid hello tidpunkten för säkerhetskopieringen. En kraschkonsekvent återställningspunkt ger inga garantier runt hello konsekvenskontroll av hello data på hello lagringsmedium--antingen från hello perspektiv hello operativsystem eller hello program. Endast hello-data som redan finns på hello disk på hello tidpunkten för säkerhetskopieringen fångas och säkerhetskopieras. <br/> <br/> Det finns inga garantier, vanligtvis, hello operativsystemet startas, följt av disk-kontrollerar proceduren som chkdsk, toofix eventuella skador fel. Alla data i minnet eller skrivningar som inte har överfört toohello disk går förlorade. hello program följer vanligtvis med sin egen kontroll om återställning av data behöver toobe klar. <br><br>Exempelvis om hello transaktionsloggen har poster som inte finns i hello databasen sedan hello databasprogramvaran gör en återställning tills hello data är konsekventa. När data sprids över flera virtuella diskar (till exempel volymer), ger en kraschkonsekvent återställningspunkt inga garantier hello är korrekt hello data. |

## <a name="performance-and-resource-utilization"></a>Prestanda och Resursanvändning
Som säkerhetskopieringsprogram som är distribuerade på plats bör du planera för kapacitet och resursanvändningen behov när du säkerhetskopierar virtuella datorer i Azure. Hej [Azure Storage begränsar](../azure-subscription-service-limits.md#storage-limits) definiera hur toostructure VM distributioner tooget maximala prestanda med minst påverkar toorunning arbetsbelastningar.

Betala uppmärksamhet toohello följande Azure Storage begränsar när du planerar prestanda vid säkerhetskopiering:

* Maximalt antal utgående per lagringskonto
* Totalt antal förfrågningar per lagringskonto

### <a name="storage-account-limits"></a>Lagringskontogränser
Säkerhetskopierade data kopieras från ett lagringskonto, lägger till toohello i/o-åtgärder per sekund (IOPS) och utgående trafik (eller genomströmning) mätvärden för hello storage-konto. AT hello samma tid, virtuella datorer också förbrukar IOPS och genomflöde. hello målet tooensure säkerhetskopiering och trafik för virtuella datorer överskrider inte din lagringskontogränser.

### <a name="number-of-disks"></a>Antal diskar
hello säkerhetskopieringen försöker toocomplete en säkerhetskopiering så snabbt som möjligt. På så sätt kan förbrukar den så många resurser som möjligt. Men alla i/o-åtgärder begränsas av hello *mål genomströmning för enda Blob*, som har högst 60 MB per sekund. I ett försök toomaximize dess hastighet hello säkerhetskopieringen försöker tooback består av hello Virtuella diskar *parallellt*. Om en virtuell dator har fyra diskar, försöker hello-tjänsten tooback upp alla fyra diskar parallellt. Hej **antal diskar** säkerhetskopieras, är hello viktigaste faktor för att fastställa säkerhetskopiering lagringstrafik för kontot.

### <a name="backup-schedule"></a>Schemat för säkerhetskopiering
Ytterligare en faktor som påverkar prestanda är hello **Säkerhetskopieringsschemat**. Om du konfigurerar hello principer så att alla virtuella datorer säkerhetskopieras på hello samma tid och du har schemalagt en trafik har fastnat. hello säkerhetskopieringen försöker tooback upp alla diskar parallellt. tooreduce hello säkerhetskopiering trafik från ett lagringskonto, säkerhetskopiera olika virtuella datorer på olika tidpunkten hello, utan överlappande.

## <a name="capacity-planning"></a>Kapacitetsplanering
Kombinera hello tidigare faktorer behöver du tooplan för hello lagringsbehov konto användning. Hämta hello [VM säkerhetskopiering kapacitetsplanering Excel-kalkylblad](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) toosee hello inverkan på disk och schemat för säkerhetskopiering val.

### <a name="backup-throughput"></a>Säkerhetskopiering genomflöde
Azure Backup läser hello block på disken hello och lagrar bara hello ändrade data (inkrementell säkerhetskopiering) för varje disk som säkerhetskopieras. hello visar följande tabell hello Backup service genomströmning genomsnittsvärden. Använda hello följande data kan beräkna du hello tidsperiod behövs tooback in en disk för en given storlek.

| Säkerhetskopieringen | Bästa genomflöde |
| --- | --- |
| Den första säkerhetskopieringen |160 Mbit/s |
| Inkrementell säkerhetskopiering (DR) |640 Mbit/s <br><br> Genomströmning således avsevärt om hello ändrat data (som måste säkerhetskopieras toobe) sprids över hello disk.|

## <a name="total-vm-backup-time"></a>Total tid för säkerhetskopiering av VM
Medan de flesta av hello säkerhetskopieringstiden används till att läsa och kopiera data, bidra andra åtgärder toohello totala tid som behövs tooback upp en virtuell dator:

* Tid som krävs för[installera eller uppdatera hello reservanknytning](backup-azure-vms.md).
* Snapshot-tid, vilket är hello tidsåtgång tootrigger en ögonblicksbild. Ögonblicksbilder är utlösta Stäng toohello schemalagd tid för säkerhetskopiering.
* Kötid. Eftersom hello Backup-tjänsten bearbetar säkerhetskopior från flera kunder, kanske kopiera säkerhetskopierade data från ögonblicksbilden toohello säkerhetskopiering eller återställningstjänster valvet inte startar omedelbart. I tidpunkter på belastning hello vänta sträcka ut upp tooeight timmar på grund av toohello antal säkerhetskopior som bearbetas. Hello totala VM tid för säkerhetskopiering är dock mindre än 24 timmar för principer för daglig säkerhetskopiering.
* Data överföringstiden, tid som krävs för säkerhetskopiering tjänsten toocompute hello inkrementella ändringar från tidigare säkerhetskopia och överför dessa ändringar toovault lagring.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Varför jag sett longer(>12 hours) Säkerhetskopiera tid?
Säkerhetskopiering består av två faser: ta ögonblicksbilder och överföra hello ögonblicksbilder toohello valvet. Hej säkerhetskopieringstjänst optimerar för lagring. När du överför hello ögonblicksbild data tooa valvet överförs hello endast inkrementella ändringar från hello tidigare ögonblicksbild.  toodetermine hello inkrementella ändringar hello service beräknar hello kontrollsumma hello-block. Om ett block ändras identifieras hello block som ett block toobe skickas toohello valvet. Sedan identifieras hello tjänsten flyttar ytterligare i var och en av hello block, söker efter affärsmöjligheter toominimize hello data tootransfer. Efter utvärdering av alla ändrade block hello service slår samman hello ändringar och skickar dem toohello valvet. I vissa äldre program är liten, fragmenterade skrivningar inte optimalt för lagring. Om hello ögonblicksbild innehåller många små, fragmenterade skrivningar, tillbringar hello service extra tid för bearbetning av hello data skrivits av hello program. hello rekommenderas programmet write block från Azure, för program som körs på hello VM, är minst 8 KB. Om programmet använder ett block med mindre än 8 KB, påverkas Säkerhetskopieringens prestanda. Justera prestanda vid säkerhetskopiering ditt program tooimprove finns [justera program för optimala prestanda med Azure storage](../storage/common/storage-premium-storage-performance.md). Men hello artikeln på säkerhetskopieringsprestanda används Premium storage exempel, gäller hello riktlinjer för standardlagring diskar.

## <a name="total-restore-time"></a>Totalt antal återställningstid
En återställningsåtgärd består av två huvudsakliga sub aktiviteter: kopiera data från hello valvet toohello valt kundlagringskontot och skapar hello virtuell dator. Kopiera data från valvet hello beror på var hello säkerhetskopior lagras internt i Azure och där hello kundlagringskontot lagras. Tidsåtgång toocopy data beror på:
* Tid - Kötid eftersom hello-tjänsten bearbetar återställningsjobb från flera kunder på hello samtidigt, restore-begäranden är placeras i en kö.
* Kopiera data tid - Data kopieras från hello valvet toohello kundlagringskontot. Återställa tiden beror på IOPS och genomströmning Azure Backup-tjänsten hämtar på hello valt kundlagringskontot. tooreduce Hej kopiera under hello återställningsprocessen, Välj ett lagringskonto som inte lästs in med andra program skrivningar och läsningar.

## <a name="best-practices"></a>Bästa praxis
Vi rekommenderar följande dessa metoder när du konfigurerar säkerhetskopieringar för virtuella datorer:

* Inte schemalägga fler än 10 klassiska virtuella datorer från samma cloud service tooback på hello hello samtidigt. Om du vill tooback in flera virtuella datorer från samma tjänst i molnet, starta Sprid hello säkerhetskopiering gånger med en timme.
* Schemalägg inte fler än 40 VMs tooback in på hello samtidigt.
* Schemalägga VM säkerhetskopior vid låg belastning. Det här sättet hello Backup-tjänsten använder IOPS för överföring av data från hello kunden storage-konto toohello valv.
* Se till att en principen tillämpas på virtuella datorer som är fördelade på olika lagringskonton. Vi rekommenderar högst 20 totala diskar från en enda lagringskonto skyddas av hello samma schema för säkerhetskopiering. Om du har mer än 20 diskar i ett lagringskonto sprider sig hello dessa virtuella datorer över flera principer tooget krävs IOPS under hello överföring fasen av hello säkerhetskopieringsprocessen.
* Återställ inte en virtuell dator som körs på toosame lagring premiumlagringskonto. Om hello återställningsprocessen åtgärden sammanfaller med hello säkerhetskopieringen, minskas hello tillgängliga IOPS för säkerhetskopiering.
* Se till att lagringskontot att värdar premiumdiskar har minst 50% ledigt utrymme för mellanlagring ögonblicksbild för en lyckad säkerhetskopia för Premium VM-säkerhetskopiering. 
* Kontrollera att python-version på virtuella Linux-datorer aktiverad för säkerhetskopiering är 2.7

## <a name="data-encryption"></a>Datakryptering
Azure-säkerhetskopiering Kryptera inte data som en del av hello säkerhetskopieringsprocessen. Men du kan kryptera data i hello VM och säkerhetskopiera hello skyddade data sömlöst (Läs mer om [säkerhetskopiering av krypterade data](backup-azure-vms-encryption.md)).

## <a name="calculating-hello-cost-of-protected-instances"></a>Beräknar hello kostnad för skyddade instanser
Virtuella Azure-datorer som säkerhetskopieras via Azure Backup är föremål för[priser för Azure Backup](https://azure.microsoft.com/pricing/details/backup/). hello skyddade instanser beräkningen baseras på hello *faktiska* storlek hello virtuell dator som är hello summan av alla hello data i hello virtuella datorn och exklusive hello ”resurs-disk”.

Priser för säkerhetskopiering av virtuella datorer är *inte* baserat på hello maximala stöds storlek för varje virtuell dator för data disk bifogade toohello. Priser baseras på hello faktiska data som lagras i hello datadisk. På liknande sätt baseras hello säkerhetskopieringslagring faktura på hello mängden data som lagras i Azure Backup, vilket är hello summan av hello faktiska data i varje återställningspunkt.

Till exempel ta en Standard A2-storlek virtuell dator med två ytterligare hårddiskar med en maximal storlek på 1 TB. hello ger följande tabell hello faktiska data som lagras på var och en av dessa diskar:

| Disktyp | Maxstorlek | Faktiska data finns |
| --- | --- | --- |
| Operativsystemdisk |1 023 GB |17 GB |
| Lokal disk / resurs disk |135 GB |5 GB (ingår inte för säkerhetskopiering) |
| Datadisk 1 |1 023 GB |30 GB |
| Datadisk 2 |1 023 GB |0 GB |

Hej *faktiska* storleken på hello virtuella datorn är i det här fallet 17 GB + 30 GB + 0 GB = 47 GB. Den här skyddade instansstorleken (47 GB) blir hello grunden för hello månadsfaktura. Som hello växer mängden data i hello virtuella hello skyddade instansstorleken används för fakturering ändringar därefter.

Faktureringen startar inte förrän hello första lyckade säkerhetskopieringen är klar. Nu inleds hello fakturering för både lagring och skyddade instanser. Fakturering fortsätter så länge det finns *någon säkerhetskopierade data lagras i ett valv* för hello virtuella datorn. Om du stoppar skydd på hello virtuell dator, men säkerhetskopierade data för virtuella datorer finns i ett valv, fortsätter fakturering.

Faktureringen för en angiven virtuell dator slutar endast om hello skyddet har avbrutits *och* alla säkerhetskopierade data tas bort. När skydd stanna och det finns inga aktiva jobb för säkerhetskopiering, hello storleken på hello senaste lyckade säkerhetskopiering blir hello skyddade instansstorleken används för hello månadsfaktura.

## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nästa steg
* [Säkerhetskopiera virtuella datorer](backup-azure-vms.md)
* [Hantera säkerhetskopiering av virtuella datorer](backup-azure-manage-vms.md)
* [Återställa virtuella datorer](backup-azure-restore-vms.md)
* [Felsökning av problem med backup VM](backup-azure-vms-troubleshoot.md)
