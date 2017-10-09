# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Säkerhetskopiera Azure ohanterade Virtuella diskar med inkrementell ögonblicksbilder
## <a name="overview"></a>Översikt
Azure Storage ger hello tootake ögonblicksbilder av blobbar. Ögonblicksbilder avbilda hello blob tillstånd vid den punkten i tid. I den här artikeln beskriver vi ett scenario där du kan underhålla säkerhetskopior av virtuella diskar med hjälp av ögonblicksbilder. Du kan använda den här metoden när du väljer att inte toouse Azure Backup och Recovery-tjänsten och vill toocreate en anpassad strategi för säkerhetskopiering för virtuella diskar.

Azure virtuella diskar lagras som sidblobbar i Azure Storage. Eftersom vi som en strategi för säkerhetskopiering för virtuella diskar i den här artikeln beskriver refererar vi toosnapshots hello gäller sidblobar. toolearn mer om ögonblicksbilder finns för[skapa en ögonblicksbild av en Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Vad är en ögonblicksbild?
En blob-ögonblicksbild är en skrivskyddad version av en blob som hämtas vid en viss tidpunkt. När en ögonblicksbild har skapats, kan den läsa, kopieras, eller ta bort, men inte har ändrats. Ögonblicksbilder ger ett sätt tooback upp en blob som det visas vid en tidpunkt. Tills REST version 2015-04-05 hade hello möjlighet toocopy fullständiga ögonblicksbilder. Med hello REST version 2015-07-08 och senare, du kan också kopiera inkrementell ögonblicksbilder.

## <a name="full-snapshot-copy"></a>Fullständig ögonblicksbild kopia
Ögonblicksbilder kan vara kopieras tooanother storage-konto som en blob tookeep säkerhetskopior av grundläggande hello-blob. Du kan också kopiera en ögonblicksbild över grundläggande blob, vilket är så återställer hello blob tooan tidigare version. När en ögonblicksbild har kopierats från en storage-konto tooanother upptar hello samma utrymme som hello bassida blob. Därför kopiera hela ögonblicksbilder från en storage-konto tooanother går långsamt och tar upp mycket utrymme i hello mål-lagringskontot.

> [!NOTE]
> Om du kopierar hello grundläggande blob tooanother mål, kopieras inte hello ögonblicksbilder av hello blob tillsammans med den. Om du skriver över en grundläggande blob med en kopia ögonblicksbilder kopplade till grundläggande hello-blob påverkas inte och ändras inte under hello grundläggande blobbnamnet.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Säkerhetskopiera diskar med hjälp av ögonblicksbilder
Som en strategi för säkerhetskopiering för virtuella diskar, kan du ta regelbundna ögonblicksbilder av hello disk- eller blob och kopiera dem tooanother lagringskonto med hjälp av verktyg som [kopiera Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) åtgärden eller [AzCopy](../articles/storage/common/storage-use-azcopy.md). Du kan kopiera en ögonblicksbild tooa mål sidblob med ett annat namn. hello resulterande mål sidblob är en skrivbar sidblob och inte en ögonblicksbild. Senare i den här artikeln beskrivs stegen tootake säkerhetskopior av virtuella diskar med hjälp av ögonblicksbilder.

### <a name="restore-disks-using-snapshots"></a>Återställa diskar med hjälp av ögonblicksbilder
När det är tid toorestore disk-tooa stabil version som tidigare har hämtats i ett av hello ögonblicksbilder av säkerhetskopior kan du kopiera en ögonblicksbild över hello bassida blob. Efter hello ögonblicksbild är upphöjt toohello bassida blob, hello ögonblicksbild fortfarande, men dess källa skrivs över med en kopia som kan vara både läses och skrivs. Nedan beskrivs stegen toorestore en tidigare version av disken från dess ögonblicksbild.

### <a name="implementing-full-snapshot-copy"></a>Implementera kopia för fullständig ögonblicksbild
Du kan implementera en fullständig ögonblicksbild kopia hello följande

* Ta först en ögonblicksbild av hello grundläggande blob med hello [ögonblicksbild Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) igen.
* Sedan kopiera hello ögonblicksbild tooa mål lagring kontot med [kopiera Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Upprepa den här processen toomaintain säkerhetskopior av din grundläggande blob.

## <a name="incremental-snapshot-copy"></a>Inkrementell ögonblicksbild kopia
hello ny funktion i hello [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API ger en mycket bättre sätt tooback in hello ögonblicksbilder av din sidblobbar eller diskar. hello API returnerar hello lista över ändringar mellan grundläggande hello-blob och hello ögonblicksbilder, vilket minskar hello mängden lagringsutrymme som används för säkerhetskopiering hello-konto. hello API stöder sidblobar på Premium-lagring samt standardlagring. Med detta API kan skapa du snabbare och effektivare säkerhetskopieringslösningar för virtuella Azure-datorer. Detta API är tillgängliga med hello REST-version 2015-07-08 och högre.

Inkrementell ögonblicksbild kopia kan du toocopy från en storage-konto tooanother hello skillnaden mellan

* Grundläggande blob och dess ögonblicksbild eller
* Två ögonblicksbilder av grundläggande hello-blob

Om hello följande villkor är uppfyllda,

* hello-blob har skapats på Jan 1 2016 eller senare.
* hello blob som inte skrivas över med [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) eller [kopiera Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) mellan två ögonblicksbilder.

**Obs**: den här funktionen är tillgänglig för Premium- och Standard Sidblobbar för Azure.

När du har en anpassad strategi för säkerhetskopiering med hjälp av ögonblicksbilder, kopiera hello ögonblicksbilder från en storage-konto tooanother kan ta lång tid och kan förbruka mycket lagringsutrymme. Du kan skriva hello skillnaden mellan på varandra följande ögonblicksbilder tooa säkerhetskopiering sidblob istället för att kopiera hello hela ögonblicksbild tooa backup storage-konto. Det här sättet hello tid toocopy hello utrymme toostore säkerhetskopiering och betydligt.

### <a name="implementing-incremental-snapshot-copy"></a>Implementera inkrementell ögonblicksbild kopia
Du kan implementera inkrementell ögonblicksbild kopiera hello följande

* Ta en ögonblicksbild av hello grundläggande blob med [ögonblicksbild Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Kopiera hello ögonblicksbild toohello mål säkerhetskopieringslagring kontot med [kopiera Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Detta är hello säkerhetskopiering sidblob. Ta en ögonblicksbild av hello säkerhetskopiering sidblob och lagra den i säkerhetskopieringen hello-konto.
* Ta en annan ögonblicksbild av hello grundläggande blob med hjälp av ögonblicksbilder Blob.
* Hämta hello skillnaden mellan hello första och andra ögonblicksbilder av hello grundläggande blob med [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Använd hello nya parametern **prevsnapshot**, toospecify hello hello ögonblicksbild som du vill tooget hello skillnaden med DateTime-värdet. Om den här parametern finns innehåller hello REST-svaret bara hello-sidor som har ändrats mellan target ögonblicksbild och tidigare ögonblicksbild, inklusive Rensa sidor.
* Använd [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) tooapply dessa ändringar toohello säkerhetskopiering sidblob.
* Slutligen kan ta en ögonblicksbild av hello säkerhetskopiering sidblob och lagra den i hello backup storage-konto.

I nästa avsnitt om hello, kommer beskrivs i detalj hur du kan upprätthålla säkerhetskopior av diskar med hjälp av inkrementell ögonblicksbild kopia

## <a name="scenario"></a>Scenario
I det här avsnittet beskriver vi ett scenario som omfattar en anpassad strategi för säkerhetskopiering för virtuella diskar med hjälp av ögonblicksbilder.

Beakta en Azure VM DS-serien med premium-lagring P30 disk ansluten. Hej P30 disk med namnet *mypremiumdisk* lagras i ett premiumlagringskonto som kallas *mypremiumaccount*. Ett standardlagringskonto kallas *mybackupstdaccount* används för att lagra hello säkerhetskopiering av *mypremiumdisk*. Vi vill gärna tookeep en ögonblicksbild av *mypremiumdisk* var 12: e timme.

toolearn om att skapa storage-konto och diskar, referera för[om Azure storage-konton](../articles/storage/storage-create-storage-account.md).

toolearn om hur du säkerhetskopierar virtuella Azure-datorer finns för[planera Azure VM-säkerhetskopieringar](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-toomaintain-backups-of-a-disk-using-incremental-snapshots"></a>Steg toomaintain säkerhetskopieringar av en disk med hjälp av inkrementell ögonblicksbilder
hello följande steg beskriver hur tootake ögonblicksbilder av *mypremiumdisk* och underhålla hello säkerhetskopieringar i *mybackupstdaccount*. hello säkerhetskopiering är en standard sidblob kallas *mybackupstdpageblob*. hello säkerhetskopiering sidblob alltid visar hello samma tillstånd som hello senaste ögonblicksbild av *mypremiumdisk*.

1. Skapa hello säkerhetskopiering sidblob för hårddisken premium-lagring genom att ta en ögonblicksbild av *mypremiumdisk* kallas *mypremiumdisk_ss1*.
2. Kopiera den här ögonblicksbilden toomybackupstdaccount som en sidblobb kallas *mybackupstdpageblob*.
3. Ta en ögonblicksbild av *mybackupstdpageblob* kallas *mybackupstdpageblob_ss1*med hjälp av [ögonblicksbild Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) och lagra den i *mybackupstdaccount*.
4. Under hello säkerhetskopieringsintervallet, skapar du en annan ögonblicksbild av *mypremiumdisk*, säg *mypremiumdisk_ss2*, och lagra den i *mypremiumaccount*.
5. Hämta hello inkrementella ändringar mellan hello två ögonblicksbilder *mypremiumdisk_ss2* och *mypremiumdisk_ss1*med hjälp av [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) på  *mypremiumdisk_ss2* med hello **prevsnapshot** parameterinställning toohello tidsstämpel *mypremiumdisk_ss1*. Skriva dessa inkrementella ändringar toohello säkerhetskopiering sidblob *mybackupstdpageblob* i *mybackupstdaccount*. Om det finns borttagna intervall i hello inkrementella ändringar, måste de tas bort från hello säkerhetskopiering sidblob. Använd [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) toowrite inkrementella ändringar toohello säkerhetskopiering sidblob.
6. Ta en ögonblicksbild av hello säkerhetskopiering sidblob *mybackupstdpageblob*, kallat *mybackupstdpageblob_ss2*. Ta bort hello tidigare ögonblicksbild *mypremiumdisk_ss1* från premium storage-konto.
7. Upprepa steg 4 – 6 var säkerhetskopieringsintervallet. På så sätt kan du hantera säkerhetskopior av *mypremiumdisk* i ett standardlagringskonto.

![Säkerhetskopiera disk med hjälp av inkrementell ögonblicksbilder](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-toorestore-a-disk-from-snapshots"></a>Steg toorestore en disk från ögonblicksbilder
Hej följande steg, som beskriver hur toorestore hello premium disk *mypremiumdisk* tooan tidigare ögonblicksbild från hello säkerhetskopiering lagringskonto *mybackupstdaccount*.

1. Identifiera hello tidpunkt som du vill toorestore hello premium disk till. Anta att den har ögonblicksbilder *mybackupstdpageblob_ss2*, som lagras i hello backup storage-konto *mybackupstdaccount*.
2. Befordra hello ögonblicksbild i mybackupstdaccount, *mybackupstdpageblob_ss2* som hello nya säkerhetskopiering bassida blob *mybackupstdpageblobrestored*.
3. Ta en ögonblicksbild av den här återställda säkerhetskopiering sidblob, kallas *mybackupstdpageblobrestored_ss1*.
4. Kopiera hello återställts sidblob *mybackupstdpageblobrestored* från *mybackupstdaccount* för*mypremiumaccount* som hello nya premium disk  *mypremiumdiskrestored*.
5. Ta en ögonblicksbild av *mypremiumdiskrestored*, kallat *mypremiumdiskrestored_ss1* för att göra framtida inkrementella säkerhetskopieringar.
6. Peka hello DS-serien VM toohello återställts disk *mypremiumdiskrestored* och frånkoppling hello gamla *mypremiumdisk* från hello VM.
7. Börja hello säkerhetskopiering process som beskrivs i föregående avsnitt för hello återställts disken *mypremiumdiskrestored*, med hjälp av hello *mybackupstdpageblobrestored* som hello säkerhetskopiering sidblobb.

![Återställa disk från ögonblicksbilder](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Nästa steg
Använd hello följande länkar toolearn mer om att skapa ögonblicksbilder av en blob och planera din infrastruktur för säkerhetskopiering av VM.

* [Skapa en ögonblicksbild av en Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [Planera infrastrukturen för säkerhetskopiering VM](../articles/backup/backup-azure-vms-introduction.md)

