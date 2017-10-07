---
title: "aaaSAP HANA Azure Backup på filnivå | Microsoft Docs"
description: "Det finns två huvudsakliga säkerhetskopiering möjligheter för SAP HANA på Azure virtual machines, den här artikeln beskriver SAP HANA Azure Backup på filnivå"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: d5a55de5634ac7724e7fd0fa3760c6c408c3db74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-azure-backup-on-file-level"></a>SAP HANA Azure Backup på filnivå

## <a name="introduction"></a>Introduktion

Detta är en del av en serie i tre delar av relaterade artiklar på SAP HANA-säkerhetskopia. [Säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](./sap-hana-backup-guide.md) innehåller en översikt och information om att komma igång och [SAP HANA säkerhetskopiering baserat på lagring ögonblicksbilder](./sap-hana-backup-storage-snapshots.md) omfattar hello snapshot-baserad säkerhetskopiering lagringsalternativ.

Tittar på hello Azure VM-storlekar, kan en se att en GS5 tillåter 64 anslutna diskar. För stora SAP HANA vidtagit ett stort antal diskar redan för data och loggfiler, eventuellt i kombination med programvarubaserad RAID för disken i/o-genomflöde. hello fråga sedan är där toostore SAP HANA säkerhetskopiera filer, vilket kan fylla hello kopplade data diskar över tid? Se [storlekar för virtuella Linux-datorer i Azure](../../linux/sizes.md) för hello Azure VM storlek tabeller.

Det finns ingen säkerhetskopiering SAP HANA-integrering med Azure Backup-tjänsten just nu. hello standardmetoden toomanage säkerhetskopiering/återställning till hello filnivå är med en filbaserad säkerhetskopia via SAP HANA Studio eller via SAP HANA SQL-instruktioner. Se [System vyer referens och SAP HANA SQL](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) för mer information.

![Den här bilden visar hello dialogrutan för hello säkerhetskopiering menyalternativet i SAP HANA Studio](media/sap-hana-backup-file-level/image022.png)

Den här bilden visar hello dialogrutan för hello säkerhetskopiering menyalternativet i SAP HANA Studio. När du väljer typen &quot;filen&quot; har toospecify en sökväg i filsystemet för hello där SAP HANA skriver hello säkerhetskopior. Återställningen fungerar hello samma sätt.

Det här alternativet låter enkel och direkt framåt, finns men det några överväganden. Som tidigare nämnts, har en begränsning av antalet datadiskar som kan bifogas en Azure VM. Det kanske inte kapacitet toostore SAP HANA-säkerhetskopior på hello filsystem för hello VM, beroende på hello storlek hello databas och disk genomströmning krav, som kan handla om programvara RAID med striping över flera diskar. Olika alternativ för att flytta dessa säkerhetskopior, och hantera filen storlek restriktioner och prestanda vid hantering av terabyte data, tillhandahålls nedan.

Ett annat alternativ som ger större frihet om total kapacitet är Azure blob storage. En enda blob är också begränsad too1 TB, är hello total kapacitet för en enda blob-behållare för närvarande 500 TB. Dessutom ger kunder hello choice tooselect s.k. &quot;kall&quot; blob-lagring, som har en kostnad. Se [Azure Blob Storage: frekvent och kall lagringsnivåer](../../../storage/blobs/storage-blob-storage-tiers.md) för ytterligare information om kall blob-lagring.

Använd en georeplikerad storage-konto toostore hello SAP HANA säkerhetskopieringar för ytterligare säkerhet. Se [Azure Storage-replikering](../../../storage/common/storage-redundancy.md) mer information om replikering av lagringskonton.

En kunde placera dedikerade virtuella hårddiskar för SAP HANA säkerhetskopieringar i en dedikerad backup storage-konto som är georeplikerad. Annars en gick att kopiera hello virtuella hårddiskar som behåller hello SAP HANA säkerhetskopior tooa georeplikerad lagringskonto eller tooa storage-konto som tillhör en annan region.

## <a name="azure-backup-agent"></a>Azure backup-agenten

Azure backup erbjuder hello alternativet toonot bara säkerhetskopiera fullständig virtuella datorer, men även filer och kataloger via hello säkerhetskopieringsagent som har toobe installerad på hello gästoperativsystemet. Men från och med December 2016 agenten bara stöds på Windows (se [säkerhetskopiering av en Windows Server eller klient tooAzure med hello Resource Manager-distributionsmodellen](../../../backup/backup-configure-vault.md)).

En lösning är toofirst kopia SAP HANA säkerhetskopior tooa Windows VM i Azure (till exempel via SAMBA-resurs) och sedan använda hello Azure backup-agenten därifrån. När det är tekniskt möjligt skulle den komplexiteten och långsammare hello säkerhetskopieringen eller återställningen ganska lite på grund av toohello kopiera mellan hello Linux och hello Windows VM. Det är inte rekommenderat toofollow den här metoden.

## <a name="azure-blobxfer-utility-details"></a>Information om verktyget Azure blobxfer

toostore kataloger och filer på Azure storage, en kan använda CLI eller PowerShell, eller utveckla ett verktyg med någon av hello [Azure SDK](https://azure.microsoft.com/downloads/). Det finns också en färdiga att använda verktyget AzCopy, för att kopiera tooAzure datalagring, men det är bara Windows (se [överföra data med hello AzCopy Command-Line verktyget](../../../storage/common/storage-use-azcopy.md)).

Därför användes blobxfer för att kopiera säkerhetskopieringsfilerna för SAP HANA. Det är öppen källkod som används av många kunder i produktionsmiljöer och finns på [GitHub](https://github.com/Azure/blobxfer). Det här verktyget kan en toocopy data direkt tooeither Azure blob storage eller Azure-filresursen. Den erbjuder också ett antal användbara funktioner, till exempel md5-hash eller automatisk parallellitet när du kopierar en katalog med flera filer.

## <a name="sap-hana-backup-performance"></a>SAP HANA säkerhetskopieringsprestanda

![Den här skärmbilden har hello SAP HANA säkerhetskopiering konsolen i SAP HANA Studio](media/sap-hana-backup-file-level/image023.png)

Den här skärmbilden har hello SAP HANA säkerhetskopiering konsolen i SAP HANA Studio. Det tog ungefär 42 minuter toodo hello säkerhetskopiering av hello 230 GB på en enda Azure standardlagring disk ansluten toohello HANA VM XFS-filsystemet.

![Den här skärmbilden har YaST på hello SAP HANA test VM](media/sap-hana-backup-file-level/image024.png)

Den här skärmbilden har YaST på hello SAP HANA test VM. En kan se hello 1 TB enskild disk för SAP HANA-säkerhetskopiering som tidigare nämnts. Det tog ungefär 42 minuter toobackup 230 GB. Dessutom fem 200 GB diskar har anslutits och programvara RAID md0 skapats med striping ovanpå diskarna fem Azure data.

![Upprepa samma säkerhetskopiera på programvarubaserad RAID med striping över fem hello anslutna Azure standardlagring datadiskar](media/sap-hana-backup-file-level/image025.png)

Upprepande hello samma säkerhetskopiera på programvarubaserad RAID med striping över fem anslutna Azure standardlagring data diskar tas hello tid för säkerhetskopiering från 42 minuter ned too10 minuter. hello diskar har anslutits utan att cachelagra toohello VM. Det är därför uppenbara hur viktigt disk genomströmning för skrivning avser hello tid för säkerhetskopiering. Något gick sedan växeln tooAzure premium storage toofurther påskynda hello process för optimala prestanda. I allmänhet ska Azure premium-lagring användas för produktionssystem.

## <a name="copy-sap-hana-backup-files-tooazure-blob-storage"></a>Kopiera SAP HANA säkerhetskopior tooAzure blob-lagring

December 2016 hello bästa alternativet tooquickly store SAP HANA säkerhetskopior är Azure-blobblagring. En enda blob-behållare är begränsad till 500 TB tillräckligt för de flesta SAP HANA-system, körs i en GS5 virtuell dator på Azure, tookeep tillräckligt SAP HANA-säkerhetskopieringar. Kunder har hello val mellan &quot;varm&quot; och &quot;kalla&quot; blob-lagring (finns [Azure Blob Storage: frekvent och kall lagringsnivåer](../../../storage/blobs/storage-blob-storage-tiers.md)).

Med hello blobxfer verktyget är det enkelt toocopy hello SAP HANA säkerhetskopierade filer direkt tooAzure blob storage.

![Här ser en hello filer av en fullständig säkerhetskopia för SAP HANA](media/sap-hana-backup-file-level/image026.png)

Här ser en hello filer av en fullständig säkerhetskopia för SAP HANA. Det finns fyra filer och hello största har ungefär 230 GB.

![Det tog ungefär 3000 sekunder toocopy hello 230 GB tooan Azure standardlagring konto blob-behållare](media/sap-hana-backup-file-level/image027.png)

Inte använder md5-hash i hello inledande test, som det tog ungefär 3000 sekunder toocopy hello 230 GB tooan Azure standardlagring konto blob-behållare.

![I den här skärmbilden kan en se hur den ser ut på hello Azure-portalen](media/sap-hana-backup-file-level/image028.png)

I den här skärmbilden kan en se hur den ser ut på hello Azure-portalen. En blobbbehållare med namnet &quot;sap-hana-säkerhetskopior&quot; har skapats och innehåller hello fyra blobbar som representerar hello SAP HANA-säkerhetskopior. En av dem har en storlek på ungefär 230 GB.

hello HANA Studio säkerhetskopiering konsolen kan en toorestrict hello maximal filstorlek HANA säkerhetskopiering av filer. I hello exempel miljö bättre prestanda genom att göra det möjligt toohave flera mindre säkerhetskopieringsfilerna, i stället för en stor 230 GB-fil.

![Ange storleksgränsen för hello säkerhetskopian på hello HANA på klientsidan & #39, t förbättra hello tid för säkerhetskopiering](media/sap-hana-backup-file-level/image029.png)

Ange storleksgränsen för hello säkerhetskopian på hello HANA på klientsidan &#39; t förbättra hello tid för säkerhetskopiering, eftersom hello skrivs i tur och ordning som visas i bilden. hello storleksbegränsningen angavs too60 GB, så hello säkerhetskopiering skapas fyra stora datafiler i stället för hello 230 GB med en fil.

![tootest parallellitet hello blobxfer verktyget Hej max storlek för HANA säkerhetskopieringar angavs sedan too15 GB](media/sap-hana-backup-file-level/image030.png)

tootest parallellitet hello blobxfer verktyget Hej max storlek för HANA säkerhetskopieringar angavs sedan too15 GB, vilket resulterade i 19 säkerhetskopior. Den här konfigurationen tas hello tid för blobxfer toocopy hello 230 GB tooAzure blob storage från 3000 sekunder too875 sekunder.

Det här resultatet är på grund av toohello högst 60 MB per sekund för att skriva en Azure blob. Parallellitet via flera BLOB löser hello flaskhals, men det finns en nackdelen: öka prestanda för hello blobxfer verktyget toocopy alla dessa HANA säkerhetskopior tooAzure blobblagring placerar belastningen på både hello HANA VM och hello nätverk. HANA systemet blir påverkas.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>BLOB-kopia av dedikerade Azure diskar i säkerhetskopieringsprogrammet RAID

Till skillnad från hello manuell VM data disksäkerhetskopiering, i den här metoden en loggar säkerhetskopiera alla data hello-diskar på en VM toosave hello hela SAP-installation, inklusive HANA data HANA inte filer och konfigurationsfiler. I stället hello idé är toohave dedikerad programvarubaserad RAID med striping över flera Azure data virtuella hårddiskar för lagring av en fullständig säkerhetskopia för SAP HANA. En kopieras endast de diskar som har hello SAP HANA-säkerhetskopia. De enkelt ska lagras i ett dedikerat HANA backup storage-konto eller kopplade tooa dedikerade &quot;säkerhetskopiera management VM&quot; för vidare bearbetning.

![Alla virtuella hårddiskar som har kopierats med hello ** start-azurestorageblobcopy ** PowerShell-kommando](media/sap-hana-backup-file-level/image031.png)

När hello säkerhetskopiering toohello lokala programvarubaserad RAID har slutfört alla virtuella hårddiskar som kopierades med hello **start azurestorageblobcopy** PowerShell-kommando (se [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Eftersom det påverkar endast hello dedikerad filsystemet för att hålla hello säkerhetskopior, finns det inga frågor om konsekvens för SAP HANA-data eller loggfil fil på disken hello. En fördel med det här kommandot är att det fungerar samtidigt hello VM fortfarande är online. toobe vissa att det finns ingen process skrivs toohello säkerhetskopiering speglings vara säker på att toounmount den innan hello blob kopia och montera den igen efteråt. En kan också använda ett lämpligt sätt för&quot;låsa&quot; hello filsystem. Till exempel via xfs\_Lås för hello XFS filsystem.

![Den här skärmbilden visar hello lista över blobbar i behållaren för hello virtuella hårddiskar på hello Azure-portalen](media/sap-hana-backup-file-level/image032.png)

Den här skärmbilden visar hello lista över blobbar på hello &quot;virtuella hårddiskar&quot; behållare på hello Azure-portalen. hello skärmbild som visar hello fem VHD som bifogades toohello SAP HANA server VM tooserve som hello programvara RAID tookeep SAP HANA säkerhetskopior. Den visar även hello fem kopior som togs via hello blob kopieringskommandot.

![I testsyfte kan var hello kopior av hello SAP HANA säkerhetskopieringsprogrammet RAID-diskar anslutna toohello applikationsserver VM](media/sap-hana-backup-file-level/image033.png)

För testning, kunde hello kopior av hello SAP HANA säkerhetskopieringsprogrammet RAID-diskar anslutna toohello app-servern VM.

![hello app-servern VM stängdes tooattach hello diskkopior](media/sap-hana-backup-file-level/image034.png)

hello app-servern VM stängdes tooattach hello diskkopior. När du har startat hello VM hello diskar och hello RAID har identifierats på rätt sätt (monterade via UUID). Endast hello monteringspunkt saknas, som har skapats via hello YaST partitioneraren. Hello SAP HANA säkerhetskopian kopior blev därefter visas på OS-nivå.

## <a name="copy-sap-hana-backup-files-toonfs-share"></a>Kopiera SAP HANA säkerhetskopieringsfiler tooNFS dela

toolessen hello inverkan på hello SAP HANA-system från ett prestandaproblem eller disk space perspektiv, ett kan du överväga att spara hello SAP HANA säkerhetskopierade filer på en NFS-resurs. Tekniskt sett fungerar, men innebär med andra Azure-VM som hello värden för hello NFS-resursens. Det får inte vara en mindre VM-storlek på grund av toohello VM nätverkets bandbredd. Det blir meningsfullt sedan tooshut ned detta &quot;säkerhetskopiera VM&quot; och bara ta dig köra hello SAP HANA-säkerhetskopiering. Skriva på en NFS resursen placerar belastningen på nätverket hello och påverkan hello SAP HANA-system, men bara hantera hello säkerhetskopior efteråt på hello &quot;säkerhetskopiera VM&quot; inte skulle påverka hello SAP HANA system alls.

![En NFS-resurs från en annan Azure VM är monterade toohello SAP HANA server VM](media/sap-hana-backup-file-level/image035.png)

tooverify hello NFS användningsfall, en NFS-resurs från en annan Azure VM är monterade toohello SAP HANA server VM. Det fanns ingen särskild NFS justera tillämpas.

![Det tog 1 timme och 46 minuter toodo hello säkerhetskopiering direkt](media/sap-hana-backup-file-level/image036.png)

hello NFS-resurs har en snabb stripe-uppsättning som hello en på hello SAP HANA-servern. Dock tog det 1 timme och 46 minuter toodo hello säkerhetskopiering direkt på hello NFS-resurs i stället för tio minuter när du skriver tooa lokala stripe-uppsättning.

![hello alternativ inte mycket snabbare på 1 timme och 43 minuter](media/sap-hana-backup-file-level/image037.png)

hello alternativ för att göra en säkerhetskopiering tooa lokala stripe-uppsättning och kopiera toohello NFS-resursens på OS-nivå (en enkel **cp - avr** kommando) inte mycket snabbare. Det tog 1 timme och 43 minuter.

Så här fungerar det, men prestanda inte bra för hello 230 GB säkerhetskopiering test. Det ser ut ännu större för flera terabyte.

## <a name="copy-sap-hana-backup-files-tooazure-file-service"></a>Kopiera SAP HANA säkerhetskopior tooAzure filtjänst

Det är möjligt toomount en Azure-filresurs i en Azure Linux-dator. hello artikel [hur toouse Azure File storage med Linux](../../../storage/files/storage-how-to-use-files-linux.md) innehåller information om hur toodo den. Tänk på att det finns för närvarande en 5 TB kvotgräns för en Azure-filresursen och begränsningar för filstorleken på 1 TB per fil. Se [Azure Storage skalbarhets- och prestandamål](../../../storage/common/storage-scalability-targets.md) information om Lagringsgränser.

Testerna har visas dock som SAP HANA säkerhetskopiering inte &#39; t just nu arbetar direkt med den här typen av CIFS monteringspunkt. Anges också i [SAP Obs 1820529](https://launchpad.support.sap.com/#/notes/1820529) som CIFS rekommenderas inte.

![Den här bilden visar ett fel i hello säkerhetskopiering i SAP HANA Studio](media/sap-hana-backup-file-level/image038.png)

Den här bilden visar ett fel i hello säkerhetskopiering i SAP HANA Studio vid tooback upp direkt tooa CIFS monterade Azure-filresursen. Så att en har toodo en standard säkerhetskopiering för SAP HANA i ett VM-filsystem först och sedan kopiera hello säkerhetskopieringsfilerna från tooAzure filtjänst.

![Den här bilden visar att det tog om 929 sekunder toocopy 19 SAP HANA-säkerhetskopior](media/sap-hana-backup-file-level/image039.png)

Den här bilden visar att det tog om 929 sekunder toocopy 19 SAP HANA säkerhetskopieringsfiler med en total storlek på ungefär 230 GB toohello Azure-filresursen.

![hello källa katalogstrukturen på hello SAP HANA VM har kopierade toohello Azure-filresursen](media/sap-hana-backup-file-level/image040.png)

I den här skärmbilden kan en se var kopierade toohello Azure-filresursen genom att hello källa katalogstrukturen på hello SAP HANA VM: en katalog (hana\_säkerhetskopiering\_fsl\_15 gb) och 19 enskilda säkerhetskopior.

Lagra säkerhetskopior för SAP HANA på Azure-filer kan vara ett intressanta alternativ i hello framtida när säkerhetskopiorna för SAP HANA stöder direkt. Eller när den blir möjligt toomount Azure filer via NFS och hello största kvot är avsevärt högre än 5 TB.

## <a name="next-steps"></a>Nästa steg
* [Säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md) ger en översikt och information om att komma igång.
* [SAP HANA säkerhetskopiering baserat på lagring ögonblicksbilder](sap-hana-backup-storage-snapshots.md) beskriver hello snapshot-baserad säkerhetskopiering lagringsalternativ.
* hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser), se toolearn [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).
