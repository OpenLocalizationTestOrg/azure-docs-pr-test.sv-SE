---
title: "aaaUse inkrementell ögonblicksbilder för säkerhetskopiering och återställning av ohanterade Virtuella Azure-diskar | Microsoft Docs"
description: "Skapa en anpassad lösning för säkerhetskopiering och återställning av Azure virtuella diskar med hjälp av inkrementell ögonblicksbilder."
services: storage
documentationcenter: na
author: aungoo-msft
manager: tadb
editor: tysonn
ms.assetid: 3524b987-bd65-4e35-83e7-fbc2136643e5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: aungoo
ms.openlocfilehash: 6d3e6d78e953f77a1028ac35dcde1ef046dbc3bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder
## <a name="overview"></a>Översikt
Azure Storage ger hello tootake ögonblicksbilder av blobbar. Ögonblicksbilder avbilda hello blob tillstånd vid den punkten i tid. I den här artikeln beskriver vi ett scenario för hur du kan upprätthålla säkerhetskopior av virtuella diskar med hjälp av ögonblicksbilder. Du kan använda den här metoden när du väljer att inte toouse Azure Backup och Recovery-tjänsten och vill toocreate en anpassad strategi för säkerhetskopiering för virtuella diskar.

Azure virtuella diskar lagras som sidblobbar i Azure Storage. Eftersom vi pratar om strategi för säkerhetskopiering för virtuella diskar i den här artikeln ska vi hänvisar toosnapshots hello gäller sidblobar. toolearn mer om ögonblicksbilder finns för[skapa en ögonblicksbild av en Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Vad är en ögonblicksbild?
En blob-ögonblicksbild är en skrivskyddad version av en blob som hämtas vid en viss tidpunkt. När en ögonblicksbild har skapats, kan den läsa, kopieras, eller ta bort, men inte har ändrats. Ögonblicksbilder ger ett sätt tooback upp en blob som det visas vid en tidpunkt. Tills REST version 2015-04-05 hade hello möjlighet toocopy fullständiga ögonblicksbilder. Med hello REST version 2015-07-08 och senare, du kan också kopiera inkrementell ögonblicksbilder.

## <a name="full-snapshot-copy"></a>Fullständig ögonblicksbild kopia
Ögonblicksbilder kan vara kopieras tooanother storage-konto som en blob tookeep säkerhetskopior av grundläggande hello-blob. Du kan också kopiera en ögonblicksbild över grundläggande blob, vilket är så återställer hello blob tooan tidigare version. När en ögonblicksbild har kopierats från en storage-konto tooanother innehar hello samma utrymme som hello bassida blob. Därför kopiera hela ögonblicksbilder från en storage-konto tooanother kommer att vara långsamt och kommer även att använda mängd utrymme i hello mål-lagringskontot.

> [!NOTE]
> Om du kopierar hello grundläggande blob tooanother mål, kopieras inte hello ögonblicksbilder av hello blob tillsammans med den. Om du skriver över en grundläggande blob med en kopia ögonblicksbilder kopplade till grundläggande hello-blob påverkas inte och ändras inte under grundläggande blobbnamnet.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Säkerhetskopiera diskar med hjälp av ögonblicksbilder
Som en strategi för säkerhetskopiering för virtuella diskar, kan du ta regelbundna ögonblicksbilder av hello disk- eller blob och kopiera dem tooanother lagringskonto med hjälp av verktyg som [kopiera Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) åtgärden eller [AzCopy](storage-use-azcopy.md). Du kan kopiera en ögonblicksbild tooa mål sidblob med ett annat namn. hello resulterande mål sidblob är en skrivbar sidblob och inte en ögonblicksbild. Senare i den här artikeln beskriver vi steg tootake säkerhetskopior av virtuella diskar med hjälp av ögonblicksbilder.

### <a name="restore-disks-using-snapshots"></a>Återställa diskar med hjälp av ögonblicksbilder
När det är tid toorestore en disk tooa stabil tidigare lagts till i en hello ögonblicksbilder av säkerhetskopior kan du kopiera en ögonblicksbild över hello bassida blob. Efter hello ögonblicksbild är upphöjt toohello bassida blob, hello ögonblicksbild fortfarande, men dess källa skrivs över med en kopia som kan vara både läses och skrivs. Senare i den här artikeln beskriver vi steg toorestore en tidigare version av disken från dess ögonblicksbild.

### <a name="implementing-full-snapshot-copy"></a>Implementera kopia för fullständig ögonblicksbild
Du kan implementera en fullständig ögonblicksbild kopia hello följande

* Ta först en ögonblicksbild av hello grundläggande blob med hello [ögonblicksbild Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) igen.
* Sedan kopiera hello ögonblicksbild tooa mål lagring kontot med [kopiera Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).
* Upprepa den här processen toomaintain säkerhetskopior av din grundläggande blob.

## <a name="incremental-snapshot-copy"></a>Inkrementell ögonblicksbild kopia
hello ny funktion i [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API ger en mycket bättre sätt tooback in hello ögonblicksbilder av din sidblobbar eller diskar. hello API returnerar hello lista över ändringar mellan grundläggande hello-blob och hello ögonblicksbilder. Detta minskar hello mängden lagringsutrymme som används för säkerhetskopiering hello-konto. hello API stöder sidblobar på Premium-lagring samt standardlagring. Använder detta API, kan du nu skapa snabbare och effektiva lösningar för säkerhetskopiering för virtuella Azure-datorer. Det här är tillgängliga med hello REST-version 2015-07-08 och högre.

Inkrementell ögonblicksbild kopia kan du toocopy från en storage-konto tooanother hello skillnaden mellan

* Grundläggande blob och dess ögonblicksbild eller
* Två ögonblicksbilder av grundläggande hello-blob

Om hello följande villkor är uppfyllda,

* hello-blob har skapats på Jan 1 2016 eller senare.
* hello blob som inte skrivas över med [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) eller [kopiera Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) mellan två ögonblicksbilder.

**Obs**: den här funktionen är tillgänglig för Premium- och Standard Sidblobbar för Azure.

När har en egen strategi för säkerhetskopiering som använder ögonblicksbilder, kopiera hello ögonblicksbilder från en tooanother för storage-konto kan vara mycket långsamt och konsumerar mycket lagringsutrymme. Du kan skriva hello skillnaden mellan på varandra följande ögonblicksbilder tooa säkerhetskopiering sidblob istället för att kopiera hello hela ögonblicksbild tooa backup storage-konto. Det här sättet hello tid toocopy utrymme toostore säkerhetskopiering och betydligt.

### <a name="implementing-incremental-snapshot-copy"></a>Implementera inkrementell ögonblicksbild kopia
Du kan implementera inkrementell ögonblicksbild kopiera hello följande

* Ta en ögonblicksbild av hello grundläggande blob med [ögonblicksbild Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx).
* Kopiera hello ögonblicksbild toohello mål säkerhetskopieringslagring kontot med [kopiera Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Detta kan vara hello säkerhetskopiering sidblob. Ta en ögonblicksbild av den här säkerhetskopieringen sidblob och lagra i säkerhetskopiering konto.
* Ta en annan ögonblicksbild av hello grundläggande blob med hjälp av ögonblicksbilder Blob.
* Hämta hello skillnaden mellan första och andra ögonblicksbilder av grundläggande blob med [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Använd hello nya parametern **prevsnapshot** toospecify hello hello ögonblicksbild som du vill tooget hello skillnaden med DateTime-värdet. Om den här parametern finns innehåller hello REST-svaret bara hello-sidor som har ändrats mellan target ögonblicksbild och tidigare ögonblicksbild, inklusive Rensa sidor.
* Använd [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) tooapply dessa ändringar toohello säkerhetskopiering sidblob.
* Slutligen kan ta en ögonblicksbild av hello säkerhetskopiering sidblob och lagra den i hello backup storage-konto.

I nästa avsnitt om hello, kommer beskrivs i detalj hur du kan upprätthålla säkerhetskopior av diskar med hjälp av inkrementell ögonblicksbild kopia

## <a name="scenario"></a>Scenario
I det här avsnittet beskriver vi ett scenario som omfattar en anpassad strategi för säkerhetskopiering för virtuella diskar med hjälp av ögonblicksbilder.

Beakta en Azure VM DS-serien med premium-lagring P30 disk ansluten. Hej P30 disk med namnet *mypremiumdisk* lagras i ett premiumlagringskonto som kallas *mypremiumaccount*. Ett standardlagringskonto kallas *mybackupstdaccount* ska användas för att lagra hello säkerhetskopiering av *mypremiumdisk*. Vi vill gärna tookeep en ögonblicksbild av *mypremiumdisk* var 12: e timme.

toolearn om att skapa storage-konto och diskar, referera för[om Azure storage-konton](storage-create-storage-account.md).

toolearn om hur du säkerhetskopierar virtuella Azure-datorer finns för[planera Azure VM-säkerhetskopieringar](../backup/backup-azure-vms-introduction.md).

## <a name="steps-toomaintain-backups-of-a-disk-using-incremental-snapshots"></a>Steg toomaintain säkerhetskopieringar av en disk med hjälp av inkrementell ögonblicksbilder
hello stegen som beskrivs nedan kommer ta ögonblicksbilder av *mypremiumdisk* och underhålla hello säkerhetskopieringar i *mybackupstdaccount*. hello säkerhetskopian ska vara en standard sidblob kallas *mybackupstdpageblob*. hello säkerhetskopiering sidblob alltid visar hello samma tillstånd som hello senaste ögonblicksbild av *mypremiumdisk*.

1. Först skapa hello säkerhetskopiering sidblob för premium-Lagringsdisken. toodo detta, ta en ögonblicksbild av *mypremiumdisk* kallas *mypremiumdisk_ss1*.
2. Kopiera den här ögonblicksbilden toomybackupstdaccount som en sidblobb kallas *mybackupstdpageblob*.
3. Ta en ögonblicksbild av *mybackupstdpageblob* kallas *mybackupstdpageblob_ss1*med hjälp av [ögonblicksbild Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) och lagra den i *mybackupstdaccount*.
4. Under hello säkerhetskopieringsintervallet, skapar du en annan ögonblicksbild av *mypremiumdisk*, säg *mypremiumdisk_ss2*, och lagra den i *mypremiumaccount*.
5. Hämta hello inkrementella ändringar mellan hello två ögonblicksbilder *mypremiumdisk_ss2* och *mypremiumdisk_ss1*med hjälp av [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) på  *mypremiumdisk_ss2* med **prevsnapshot** parameterinställning toohello tidsstämpel *mypremiumdisk_ss1*. Skriva dessa inkrementella ändringar toohello säkerhetskopiering sidblob *mybackupstdpageblob* i *mybackupstdaccount*. Om det finns borttagna intervall i hello inkrementella ändringar, måste de tas bort från hello säkerhetskopiering sidblob. Använd [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) toowrite inkrementella ändringar toohello säkerhetskopiering sidblob.
6. Ta en ögonblicksbild av hello säkerhetskopiering sidblob *mybackupstdpageblob*, kallat *mybackupstdpageblob_ss2*. Ta bort hello tidigare ögonblicksbild *mypremiumdisk_ss1* från premium storage-konto.
7. Upprepa steg 4 – 6 var säkerhetskopieringsintervallet. På så sätt kan du hantera säkerhetskopior av *mypremiumdisk* i ett standardlagringskonto.

![Säkerhetskopiera disk med hjälp av inkrementell ögonblicksbilder](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-toorestore-a-disk-from-snapshots"></a>Steg toorestore en disk från ögonblicksbilder
hello stegen som beskrivs nedan kommer att återställa premium disk *mypremiumdisk* tooan tidigare ögonblicksbild från hello säkerhetskopiering lagringskonto *mybackupstdaccount*.

1. Identifiera hello tidpunkt som du vill toorestore hello premium disk till. Anta att som är en ögonblicksbild *mybackupstdpageblob_ss2*, som lagras i hello backup storage-konto *mybackupstdaccount*.
2. Befordra hello ögonblicksbild i mybackupstdaccount, *mybackupstdpageblob_ss2* som hello nya säkerhetskopiering bassida blob *mybackupstdpageblobrestored*.
3. Ta en ögonblicksbild av den här återställda säkerhetskopiering sidblob, kallas *mybackupstdpageblobrestored_ss1*.
4. Kopiera hello återställts sidblob *mybackupstdpageblobrestored* från *mybackupstdaccount* för*mypremiumaccount* som hello nya premium disk  *mypremiumdiskrestored*.
5. Ta en ögonblicksbild av *mypremiumdiskrestored*, kallat *mypremiumdiskrestored_ss1* för att göra framtida inkrementella säkerhetskopieringar.
6. Peka hello DS-serien VM toohello återställts disk *mypremiumdiskrestored* och frånkoppling hello gamla *mypremiumdisk* från hello VM.
7. Börja hello säkerhetskopiering process som beskrivs i föregående avsnitt för hello återställts disken *mypremiumdiskrestored*, med hjälp av hello *mybackupstdpageblobrestored* som hello säkerhetskopiering sidblobb.

![Återställa disk från ögonblicksbilder](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Nästa steg
Läs mer om att skapa ögonblicksbilder av en blob och planera din Virtuella infrastrukturen för säkerhetskopiering med hello länkarna nedan.

* [Skapa en ögonblicksbild av en Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
* [Planera infrastrukturen för säkerhetskopiering VM](../backup/backup-azure-vms-introduction.md)

