---
title: "aaaPreparing hårddiskar för Azure Import/Export-importjobb | Microsoft Docs"
description: "Lär dig hur tooprepare hårddiskar med hello WAImportExport verktyget toocreate ett importjobb för hello Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: dc4f4f580f70dbd504317f59df142b3e4c48cb66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Förbereda hårddiskar för ett importjobb

Hej WAImportExport verktyget är hello enhet förberedelse och reparera verktyg som du kan använda med hello [Microsoft Azure Import/Export service](../storage-import-export-service.md). Du kan använda det här verktyget toocopy data toohello hårddiskar du kommer tooship tooan Azure-datacenter. När importen har slutförts kan använda du det här verktyget toorepair alla blobbar som skadades saknades eller stod i konflikt med andra blobbar. När du får hello enheter från en slutförd exportjobb kan du använda det här verktyget toorepair filer som var skadade eller saknas på hello enheter. I den här artikeln går vi över hello användning av det här verktyget.

## <a name="prerequisites"></a>Krav

### <a name="requirements-for-waimportexportexe"></a>Krav för WAImportExport.exe

- **Konfigurationen av datorn**
  - Windows 7, Windows Server 2008 R2 eller ett nyare Windows-operativsystem
  - .NET framework 4 måste installeras. Se [vanliga frågor och svar](#faq) om hur toocheck om .net Framework är installerat på datorn hello.
- **Lagringskontonyckel** -du behöver minst en av hello nycklar för hello storage-konto.

### <a name="preparing-disk-for-import-job"></a>Förbereda disk för importjobb

- **BitLocker -** BitLocker måste vara aktiverat på hello datorn körs hello WAImportExport verktyget. Se hello [vanliga frågor och svar](#faq) för hur tooenable BitLocker.
- **Diskar** nås från datorn där WAImportExport verktyget körs. Se [vanliga frågor och svar](#faq) för disk-specifikationen.
- **Källfiler** -hello-filer som du planerar tooimport måste vara tillgänglig från hello kopiera datorn, om de finns på en nätverksresurs eller en lokal hårddisk.

### <a name="repairing-a-partially-failed-import-job"></a>Reparera felaktiga importjobb

- **Kopiera loggfilen** som genereras när Azure Import/Export service kopierar data mellan Lagringskontot och Disk. Det finns i mål-lagringskontot.

### <a name="repairing-a-partially-failed-export-job"></a>Reparera felaktiga exportjobb

- **Kopiera loggfilen** som genereras när Azure Import/Export service kopierar data mellan Lagringskontot och Disk. Det finns i ditt lagringskonto för källa.
- **Manifestfilen** -[valfritt] Located på exporterade enhet som returnerades av Microsoft.

## <a name="download-and-install-waimportexport"></a>Hämta och installera WAImportExport

Hämta hello [senaste versionen av WAImportExport.exe](https://www.microsoft.com/download/details.aspx?id=55280). Extrahera hello komprimerade innehåll tooa katalog på din dator.

Nästa uppgift är toocreate CSV-filer.

## <a name="prepare-hello-dataset-csv-file"></a>Förbereda hello dataset CSV-fil

### <a name="what-is-dataset-csv"></a>Vad är dataset CSV

DataSet CSV-fil är hello värdet för /dataset flaggan är en CSV-fil som innehåller en lista över kataloger och/eller en lista över filer toobe kopieras tootarget enheter. hello första steg toocreating importen är toodetermine vilka kataloger och filer som ska tooimport. Detta kan vara en lista över kataloger, en lista med unika filer eller en kombination av dessa två. När en katalog finns ska alla filer i hello katalogen och dess underkataloger ingå i hello importjobbet.

För varje mapp eller fil toobe importeras, måste du identifiera en virtuell katalog för mål eller blob i hello Azure Blob-tjänsten. Du använder dessa mål som indata toohello WAImportExport verktyg. Kataloger ska avgränsas med hello snedstreck ”/”.

hello följande tabell visas några exempel på blob mål:

| Käll-filen eller katalogen | Mål-blob eller virtuell katalog |
| --- | --- |
| H:\Video | https://mystorageaccount.BLOB.Core.Windows.NET/video |
| H:\Photo | https://mystorageaccount.BLOB.Core.Windows.NET/Photo |
| K:\Temp\FavoriteVideo.ISO | https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteVideo.ISO |
| \\myshare\john\music | https://mystorageaccount.BLOB.Core.Windows.NET/Music |

### <a name="sample-datasetcsv"></a>Exempel på dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
"F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
"F:\50M_original\","containername/",BlockBlob,rename,"None",None
```

### <a name="dataset-csv-file-fields"></a>Fälten för DataSet CSV-filen

| Fält | Beskrivning |
| --- | --- |
| BasePath | **[Krävs]**<br/>hello-värdet för den här parametern representerar hello källa där hello data toobe importeras finns. hello verktyget kommer rekursivt kopiera alla data som finns under den här sökvägen.<br><br/>**Tillåtna värden**: detta har toobe en giltig sökväg på lokal dator eller en giltig resurssökväg och ska vara tillgänglig hello-användare. hello sökväg måste vara en absolut sökväg (inte en relativ sökväg). Om sökvägen hello slutar med ”\\”, en annan katalog representerar en sökväg utan avslutande ”\\” representerar en fil.<br/>Inga regex tillåts i det här fältet. Om hello sökvägen innehåller blanksteg, placera den i ””.<br><br/>**Exempel**: ”c:\Directory\c\Directory\File.txt”<br>”\\\\FBaseFilesharePath.domain.net\sharename\directory\"  |
| DstBlobPathOrPrefix | **[Krävs]**<br/> hello sökvägen toohello mål virtuell katalog på din Windows Azure storage-konto. hello virtuell katalog kan eller kan inte redan finns. Om det inte finns, skapas en Import/Export service.<br/><br/>Vara säker på att toouse giltig behållarnamn när du ska ange mål virtuella kataloger eller BLOB. Tänk på att behållarnamn måste vara gemener. Behållaren namngivningsregler, se [namnge och referera till behållare, Blobbar och Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata). Om endast roten anges replikeras hello katalogstrukturen på hello källa i hello mål blob-behållaren. Om en annan katalogstruktur önskas än hello i källan, flera rader med mappning i CSV<br/><br/>Du kan ange en behållare eller ett blob-prefix som musik/70-talet /. hello målkatalogen måste börja med hello behållarens namn följt av ett snedstreck ”/”, och du kan också omfatta en virtuell blob-katalog som slutar med ”/”.<br/><br/>När hello målbehållare är hello Rotbehållare, måste du uttryckligen ange hello Rotbehållare, inklusive hello snedstreck som $root /. Eftersom blobbar under hello Rotbehållare inte får innehålla ”/” i sina namn, kopieras inte eventuella underkataloger hello källkatalogen när hello målkatalogen är hello Rotbehållare.<br/><br/>**Exempel**<br/>Om hello blob målsökväg https://mystorageaccount.blob.core.windows.net/video hello värdet för det här fältet kan vara video /  |
| BlobType | **[Valfritt]**  block &#124; sidan<br/>Import/Export service stöder för närvarande 2 typer av Blobbar. Sidan blobbar och blockera BlobsBy standard importeras alla filer som Blockblobar. Och \*.vhd och \*.vhdx importeras som Page BlobsThere finns en gräns på hello blockblob och sidblob tillåtna storleken. Se [lagring skalbarhetsmål](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files) för mer information.  |
| Disposition | **[Valfritt]**  Byt namn på &#124; Nej skriva över &#124; skriva över <br/> Det här fältet anger hello kopia-beteende under import engångsfaktorautentisering När data som överförs toohello storage-konto från hello disken. Alternativen är: Byt namn på &#124; Skriv över &#124; Nej skriva över. Standardinställningar för ”Byt namn på” om inget anges. <br/><br/>**Byt namn på**: om ett objekt med samma namn finns, skapas en kopia i målet.<br/>Skriv över: ersätts hello-filen med nyare fil. hello-filen med last-modified wins.<br/>**Nej, skriva över**: hoppar över skrivning hello filen om den redan finns.|
| MetadataFile | **[Valfritt]** <br/>Hej toothis värdefält är hello metadatafil som kan anges om hello en måste toopreserve hello metadata för hello objekt eller ange anpassade metadata. Sökvägen toohello metadatafil för hello mål-BLOB. Se [Import/Export service Metadata och egenskaper filformatet](../storage-import-export-file-format-metadata-and-properties.md) mer information |
| PropertiesFile | **[Valfritt]** <br/>Sökvägen toohello egenskapsfil för hello mål-BLOB. Se [Import/Export service Metadata och egenskaper filformatet](../storage-import-export-file-format-metadata-and-properties.md) för mer information. |

## <a name="prepare-initialdriveset-or-additionaldriveset-csv-file"></a>Förbereda InitialDriveSet eller AdditionalDriveSet CSV-fil

### <a name="what-is-driveset-csv"></a>Vad är driveset CSV

hello är värdet för hello /InitialDriveSet eller /AdditionalDriveSet flaggan en CSV-fil som innehåller hello lista över diskar toowhich hello enhetsbeteckningar mappas så att hello verktyget kan korrekt välja hello lista över diskar toobe förberedd. Om hello Datastorleken är större än storleken för en enskild disk ska hello WAImportExport verktyget distribuera hello data över flera diskar som är registrerad i CSV-filen på ett optimerat sätt.

Det finns ingen gräns för hello antalet diskar hello data kan skrivas tooin en enda session. hello verktyget ska distribuera data baserat på diskens storlek och mappstorleken. Väljs hello-disk som är de flesta optimerade för hello Objektstorlek. hello-data när överförs toohello storage-konto kommer att konvergerade tillbaka toohello katalogstruktur som angavs i dataset-filen. Följ hello stegen nedan i ordning toocreate driveset CSV.

### <a name="create-basic-volume-and-assign-drive-letter"></a>Skapa en enkel volym och tilldela enhetsbeteckning

I ordning toocreate en enkel volym och tilldela en enhetsbeteckning genom att följa hello anvisningar för ”enklare att skapa partitioner” anges i [översikt över Diskhantering](https://technet.microsoft.com/library/cc754936.aspx).

### <a name="sample-initialdriveset-and-additionaldriveset-csv-file"></a>Exempel InitialDriveSet och AdditionalDriveSet CSV-fil

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
H,Format,SilentMode,Encrypt,
```

### <a name="driveset-csv-file-fields"></a>Fälten för Driveset CSV-filen

| Fält | Värde |
| --- | --- |
| Enhetsbeteckning | **[Krävs]**<br/> Varje enhet som tillhandahålls toohello verktyget som hello mål måste ha en enkel NTFS-volym på den och en enhetsbeteckning tilldelad tooit.<br/> <br/>**Exempel**: R eller r |
| FormatOption | **[Krävs]**  Format &#124; AlreadyFormatted<br/><br/> **Formatet**: ange detta formaterar alla hello data på hello disk. <br/>**AlreadyFormatted**: hello verktyget hoppar över formatering när det här värdet anges. |
| SilentOrPromptOnFormat | **[Krävs]**  SilentMode &#124; PromptOnFormat<br/><br/>**SilentMode**: det här värdet kommer att aktivera användaren toorun hello verktyg i tyst läge. <br/>**PromptOnFormat**: hello ombeds hello användaren tooconfirm om hello åtgärden verkligen avsedda vid varje format.<br/><br/>Om inte har angetts kommandot kommer att avbryta och visar felmeddelande ”: Ogiltigt värde för SilentOrPromptOnFormat: ingen” |
| Kryptering | **[Krävs]**  Kryptera &#124; AlreadyEncrypted<br/> hello-värdet för det här fältet bestämmer vilka disk tooencrypt och som inte. <br/><br/>**Kryptera**: verktyget formaterar hello enhet. Om värdet för fältet ”FormatOption” är ”Format” är det här värdet krävs toobe ”kryptera”. Om ”AlreadyEncrypted” anges i det här fallet, leder till ett fel ”när formatet anges kryptera måste du även ange”.<br/>**AlreadyEncrypted**: verktyget dekrypterar hello-enhet med hello BitLockerKey som anges i fältet ”ExistingBitLockerKey”. Om värdet för fältet ”FormatOption” är ”AlreadyFormatted”, kan sedan det här värdet vara antingen ”kryptera” eller ”AlreadyEncrypted” |
| ExistingBitLockerKey | **[Krävs]**  Om värdet för fältet ”kryptering” är ”AlreadyEncrypted”<br/> hello-värdet för det här fältet är hello BitLocker-nyckel som är associerad med hello viss disk. <br/><br/>Det här fältet måste vara tomt om hello värdet för fältet ”kryptering” är ”kryptera”.  Om BitLocker Key anges i det här fallet är resulterar det i ett fel ”Bitlocker Key ska inte anges”.<br/>  **Exempel**: 060456-014509-132033-080300-252615-584177-672089-411631|

##  <a name="preparing-disk-for-import-job"></a>Förbereda disk för importjobb

tooprepare enheter för ett importjobb anropa hello WAImportExport verktyg med hello **PrepImport** kommando. Vilka parametrar du inkludera beror på om detta är hello första kopian session eller en efterföljande kopia-session.

### <a name="first-session"></a>Första sessionen

Första kopian Session tooCopy ett Single/Multiple Directory tooa single/flera Disk (beroende på vad som anges i CSV-fil) WAImportExport verktyg PrepImport kommandot för hello först kopiera session toocopy kataloger och/eller filer med en ny kopia-session:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Exempel:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:\*\*\*\*\*\*\*\*\*\*\*\*\* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

### <a name="add-data-in-subsequent-session"></a>Lägg till data i efterföljande sessionen

I efterföljande kopiera sessioner behöver inte toospecify hello inledande parametrar. Du behöver toouse hello samma journalfil för hello verktyget tooremember där det kvar i hello tidigare session. hello tillståndet för hello kopiera session skrivs toohello journal-fil. Här är hello syntax för en efterföljande kopia session toocopy ytterligare kataloger och eller filer:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<DifferentSessionId>  [DataSet:<differentdataset.csv>]
```

**Exempel:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

### <a name="add-drives-toolatest-session"></a>Lägg till enheter toolatest session

Om hello data inte fick plats i enheter i InitialDriveset, använda en hello verktyget tooadd ytterligare enheter toosame kopiera session. 

>[!NOTE] 
>hello sessions-id ska matcha hello tidigare sessions-id. Journal-fil ska matcha hello som har angetts i tidigare session.
>
```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AdditionalDriveSet:<newdriveset.csv>
```

**Exempel:**

```
WAImportExport.exe PrepImport /j:SameJournalTest.jrn /id:session#2  /AdditionalDriveSet:driveset-2.csv
```

### <a name="abort-hello-latest-session"></a>Avbryt hello senaste sessionen:

Om en kopia sessionen avbryts och det är inte möjligt tooresume (till exempel om en källkatalog visat tillgänglig), du måste avbryta hello aktuell session så att den kan återställas startas tillbaka och nya kopiera sessioner:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /AbortSession
```

**Exempel:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /AbortSession
```

Endast hello kan senaste kopiera sessionen om avslutats onormalt, avbrytas. Observera att du inte kan avbryta hello den första kopian sessionen för en enhet. I stället måste du starta om hello kopiera session med en ny journalfil.

### <a name="resume-a-latest-interrupted-session"></a>Återuppta en senaste avbruten session

Om en kopia sessionen avbryts av någon anledning, kan du återuppta den genom att köra verktyget hello med endast hello journal angivna filen:

```
WAImportExport.exe PrepImport /j:<SameJournalFile> /id:<SameSessionId> /ResumeSession
```

**Exempel:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /ResumeSession
```

> [!IMPORTANT] 
> När du återupptar en kopia session inte ändra hello Källdatafiler och kataloger genom att lägga till eller ta bort filer.

## <a name="waimportexport-parameters"></a>WAImportExport parametrar

| Parametrar | Beskrivning |
| --- | --- |
|     /j:&lt;JournalFile&gt;  | **Krävs**<br/> Sökvägen toohello journal-fil. En journal-fil spårar en uppsättning enheter och poster hello förlopp förbereder dessa enheter. hello journal-fil måste alltid anges.  |
|     /logdir:&lt;LogDirectory&gt;  | **Valfritt**. hello loggkatalogen.<br/> Utförlig loggfiler samt vissa temporära filer skrivs toothis directory. Om inte angivna, aktuell katalog kommer att användas som hello loggkatalogen. hello loggkatalogen kan endast anges en gång för hello samma journal-fil.  |
|     /ID:&lt;sessions-ID&gt;  | **Krävs**<br/> hello sessionens Id är används tooidentify en kopia session. Det är används tooensure korrekt återställning av en session avbryts kopia.  |
|     / ResumeSession  | Valfri. Om hello senaste kopiera sessionen har avslutats onormalt, kan den här parametern vara angivna tooresume hello session.   |
|     / AbortSession  | Valfri. Om hello senaste kopiera sessionen har avslutats onormalt, kan den här parametern vara angivna tooabort hello session.  |
|     /SN:&lt;StorageAccountName&gt;  | **Krävs**<br/> Gäller endast för RepairImport och RepairExport. hello namnet på hello storage-konto.  |
|     /Sk:&lt;StorageAccountKey&gt;  | **Krävs**<br/> hello nyckeln för hello storage-konto. |
|     / InitialDriveSet:&lt;driveset.csv&gt;  | **Krävs** när den första kopian sessionen körs hello<br/> En CSV-fil som innehåller en lista över enheter tooprepare.  |
|     / AdditionalDriveSet:&lt;driveset.csv&gt; | **Krävs**. När du lägger till enheter toocurrent kopiera session. <br/> En CSV-fil som innehåller en lista över ytterligare enheter toobe lagts till.  |
|      / r:&lt;RepairFile&gt; | **Krävs** gäller endast för RepairImport och RepairExport.<br/> Sökvägen toohello fil för spårning av reparation pågår. Varje enhet måste ha en repair-fil.  |
|     / d:&lt;TargetDirectories&gt; | **Krävs**. Gäller endast för RepairImport och RepairExport. För RepairImport, en eller flera semikolonavgränsad kataloger toorepair; För RepairExport toorepair för en katalog, t.ex. rotkatalogen för hello enheten.  |
|     / CopyLogFile:&lt;DriveCopyLogFile&gt; | **Krävs** gäller endast för RepairImport och RepairExport. Sökvägen toohello enhet kopiera loggfilen (utförlig eller fel).  |
|     / ManifestFile:&lt;DriveManifestFile&gt; | **Krävs** gäller bara för RepairExport.<br/> Sökvägen toohello enhet manifestfilen.  |
|     / PathMapFile:&lt;DrivePathMapFile&gt; | **Valfritt**. Gäller endast för RepairImport.<br/> Sökvägen toohello-fil som innehåller mappningar av filen sökvägar relativa toohello enhet rot toolocations faktiska filer (tabbavgränsad). Om du först fylls den med sökvägar med tomt mål, vilket innebär att de inte hittades i TargetDirectories åtkomst nekad med ogiltigt namn eller de finns i flera kataloger. mappningsfilen för hello sökvägen kan vara manuellt redigerade tooinclude hello rätt målsökvägar och angetts igen för hello verktyget tooresolve hello sökvägar på rätt sätt.  |
|     / ExportBlobListFile:&lt;ExportBlobListFile&gt; | **Krävs**. Gäller endast för PreviewExport.<br/> Sökvägen toohello XML-fil som innehåller listan över blob-sökvägar eller blob sökväg-prefix för hello blobbar toobe exporteras. hello-filformatet är hello samma hello blob listformat blob i hello placera Job-åtgärden för hello Import/Export service REST API.  |
|     / DriveSize:&lt;DriveSize&gt; | **Krävs**. Gäller endast för PreviewExport.<br/>  Storleken på enheter toobe som används för export. Till exempel 500 GB 1,5 TB. Obs: 1 GB = 1 000 000 000 bytes1 TB = 1,000,000,000,000 byte  |
|     / Datauppsättning:&lt;dataset.csv&gt; | **Krävs**<br/> En CSV-fil som innehåller en lista över kataloger och/eller en lista över filer toobe kopieras tootarget enheter.  |
|     /silentmode  | **Valfritt**.<br/> Om inget anges påminna du hello behovet av enheter och behöver bekräftelse-toocontinue.  |

## <a name="tool-output"></a>Verktyget utdata

### <a name="sample-drive-manifest-file"></a>Manifestet exempelfil för enhet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DriveManifest Version="2011-MM-DD">
   <Drive>
      <DriveId>drive-id</DriveId>
      <StorageAccountKey>storage-account-key</StorageAccountKey>
      <ClientCreator>client-creator</ClientCreator>
      <!-- First Blob List -->
      <BlobList Id="session#1-0">
         <!-- Global properties and metadata that applies tooall blobs -->
         <MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>
         <PropertiesPath Hash="md5-hash">global-properties-file-path</PropertiesPath>
         <!-- First Blob -->
         <Blob>
            <BlobPath>blob-path-relative-to-account</BlobPath>
            <FilePath>file-path-relative-to-transfer-disk</FilePath>
            <ClientData>client-data</ClientData>
            <Length>content-length</Length>
            <ImportDisposition>import-disposition</ImportDisposition>
            <!-- page-range-list-or-block-list -->
            <!-- page-range-list -->
            <PageRangeList>
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
               <PageRange Offset="1073741824" Length="512" Hash="md5-hash" />
            </PageRangeList>
            <!-- block-list -->
            <BlockList>
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
               <Block Offset="1073741824" Length="4194304" Id="block-id" Hash="md5-hash" />
            </BlockList>
            <MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>
            <PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>
         </Blob>
      </BlobList>
   </Drive>
</DriveManifest>
```

### <a name="sample-journal-file-xml-for-each-drive"></a>Ändringsjournalen exempelfilen (XML) för varje enhet

```xml
[BeginUpdateRecord][2016/11/01 21:22:25.379][Type:ActivityRecord]
ActivityId: DriveInfo
DriveState: [BeginValue]
<?xml version="1.0" encoding="UTF-8"?>
<Drive>
   <DriveId>drive-id</DriveId>
   <BitLockerKey>*******</BitLockerKey>
   <ManifestFile>\DriveManifest.xml</ManifestFile>
   <ManifestHash>D863FE44F861AE0DA4DCEAEEFFCCCE68</ManifestHash> </Drive>
[EndValue]
SaveCommandOutput: Completed
[EndUpdateRecord]
```

### <a name="sample-journal-file-jrn-for-session-that-records-hello-trail-of-sessions"></a>Ändringsjournalen exempelfilen (JRN) för session som registrerar hello spår efter sessioner

```
[BeginUpdateRecord][2016/11/02 18:24:14.735][Type:NewJournalFile]
VocabularyVersion: 2013-02-01
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.749][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
LogDirectory: F:\logs
[EndUpdateRecord]
[BeginUpdateRecord][2016/11/02 18:24:14.754][Type:ActivityRecord]
ActivityId: PrepImportDriveCommandContext
StorageAccountKey: *******
[EndUpdateRecord]
```

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

### <a name="general"></a>Allmänt

#### <a name="what-is-waimportexport-tool"></a>Vad är WAImportExport tool?

Hej WAImportExport verktyget är hello enhet förberedelse och reparera verktyg som du kan använda med hello Microsoft Azure Import/Export service. Du kan använda det här verktyget toocopy data toohello hårddiskar du kommer tooship tooan Azure-datacenter. När importen har slutförts kan använda du det här verktyget toorepair alla blobbar som skadades saknades eller stod i konflikt med andra blobbar. När du får hello enheter från en slutförd exportjobb kan du använda det här verktyget toorepair filer som var skadade eller saknas på hello enheter.

#### <a name="how-does-hello-waimportexport-tool-work-on-multiple-source-dir-and-disks"></a>Hur fungerar har hello WAImportExport verktyget på flera källa dir och diskar?

Om hello Datastorleken är större än hello diskstorleken distribuerar hello WAImportExport verktyget hello data över hello diskar på ett optimerat sätt. hello kopiera toomultiple datadiskar som kan göras parallell eller sekventiellt. Det finns ingen gräns för hello antalet diskar hello data kan skrivas toosimultaneously. hello verktyget ska distribuera data baserat på diskens storlek och mappstorleken. Väljs hello-disk som är de flesta optimerade för hello Objektstorlek. hello-data när överförs toohello lagringskonto konvergerat tillbaka toohello angetts katalogstruktur.

#### <a name="where-can-i-find-previous-version-of-waimportexport-tool"></a>Där hittar du en tidigare version av WAImportExport verktyget?

WAImportExport verktyg har alla funktioner som hade WAImportExport V1-verktyget. WAImportExport verktyget kan användarna toospecify flera källor och skriva toomultiple enheter. Dessutom kan ett enkelt hantera flera källplatser som hello data måste toobe kopieras i en enskild CSV-fil. I fallet behöver du dock SAS stöder eller vill toocopy enda toosingle källdisken, du kan [hämta WAImportExport V1 Tool] (http://go.microsoft.com/fwlink/? ID-numret = 301900&amp;clcid = 0x409) och Läs för[WAImportExport V1 referens](storage-import-export-tool-how-to-v1.md) hjälp med WAImportExport V1 användning.

#### <a name="what-is-a-session-id"></a>Vad är ett sessions-ID?

hello verktyget förväntar hello kopiera session (/ id) parametern toobe hello samma om hello avsikt toospread hello data över flera diskar. Underhålla hello samma namnet på hello kopiera session aktiverar toocopy användardata från en eller flera källplatser i en eller flera diskar/kataloger. Upprätthålla samma sessions-id kan hello verktyget toopick in hello kopiering av filer från när den har lämnat hello senast.

Samma kopia session måste använda tooimport data toodifferent storage-konton.

När kopiera sessionsnamnet är samma i flera körningar av hello verktyget, hello logfile (/ logdir) och lagringskontonyckel (/ sk) är också förväntade toobe hello samma.

Sessions-ID kan bestå av bokstäver, 0 ~ 9, understore (\_), bindestreck (-) eller hash (#), och längden måste vara 3 ~ 30.

t.ex. sessions-1 eller session #1 eller session\_1

#### <a name="what-is-a-journal-file"></a>Vad är en journal-fil?

Varje gång du kör hello WAImportExport verktyget toocopy filer toohello hårddisk, skapar hello verktyget en kopia-session. hello tillståndet för hello kopiera session skrivs toohello journal-fil. Om en kopia sessionen avbryts (t.ex, på grund av strömavbrott för tooa system), kan det återupptas genom att köra verktyget hello igen och ange hello journal-fil på hello-kommandoraden.

För varje hårddisk som du förbereder med hello Azure Import/Export verktyget hello verktyget skapar en enda journal-fil med namnet ”&lt;enhets-ID&gt;XML” där enheten Id är hello serienumret kopplat toohello enhet som hello verktyget läser från hello-disk. Du behöver hello journalfiler från alla dina enheter toocreate hello-importjobbet. hello journal-fil kan också vara används tooresume förberedelse av enheten om hello verktyget avbryts.

#### <a name="what-is-a-log-directory"></a>Vad är en loggkatalog?

hello loggkatalogen anger en katalog toobe toostore detaljerade loggarna som tillfällig manifest-filer. Om inget anges används hello katalogen som hello loggkatalogen. hello loggarna är detaljerade loggarna.

### <a name="prerequisites"></a>Krav

#### <a name="what-are-hello-specifications-of-my-disk"></a>Vad är hello specifikationer av disken?

En eller flera tom 2,5-tums eller 3,5-tums SATA II eller III eller hårda SSD-enheter anslutna toohello kopiera datorn.

#### <a name="how-can-i-enable-bitlocker-on-my-machine"></a>Hur kan jag aktivera BitLocker på min dator?

Det är enkelt toocheck genom att högerklicka på systemenheten. Den visar alternativ för Bitlocker om hello-funktionen är aktiverad. Om det är inaktiverat kan du inte se den.

![Kontrollera BitLocker](./media/storage-import-export-tool-preparing-hard-drives-import/BitLocker.png)

Här är en artikel om [hur tooenable BitLocker](https://technet.microsoft.com/library/cc766295.aspx)

Det är möjligt att din dator inte har TPM-chip. Om du inte får utdata med tpm.msc titta på hello nästa vanliga frågor och svar.

#### <a name="how-toodisable-trusted-platform-module-tpm-in-bitlocker"></a>Hur toodisable Trusted Platform Module (TPM) i BitLocker?
> [!NOTE]
> Endast om det finns ingen TPM i sina servrar, måste toodisable TPM-principen. Det är inte nödvändigt toodisable TPM om det finns en betrodd TPM i användarens server. 
> 

Gå igenom hello följa stegen i ordning toodisable TPM i BitLocker:<br/>
1. Starta **Redigeraren för** genom att skriva gpedit.msc i en kommandotolk. Om **Redigeraren för** visas toobe tillgänglig för att aktivera BitLocker först. Se föregående vanliga frågor och svar.
2. Öppna **lokal datorprincip &gt; Datorkonfiguration &gt; Administrationsmallar &gt; Windows-komponenter&gt; BitLocker-diskkryptering &gt; operativsystemsenheter**.
3. Redigera **kräver ytterligare autentisering vid start** princip.
4. Ange hello princip för**aktiverad** och se till att **Tillåt BitLocker utan en kompatibel TPM** är markerad.

####  <a name="how-toocheck-if-net-4-or-higher-version-is-installed-on-my-machine"></a>Hur toocheck om .NET 4 eller högre version är installerad på datorn?

Alla versioner av Microsoft .NET Framework är installerat i följande katalog: %windir%\Microsoft.NET\Framework\

Navigera toohello ovan nämnda del på din måldatorn där hello verktyget måste toorun. Sök efter namn börjar med ”v4”. Frånvaron av sådana directory innebär .NET 4 inte har installerats på datorn. Du kan hämta .net 4 på datorn med hjälp av [Microsoft .NET Framework 4 (Webbinstallationsprogram)](https://www.microsoft.com/download/details.aspx?id=17851).

### <a name="limits"></a>Begränsningar

#### <a name="how-many-drives-can-i-preparesend-at-hello-same-time"></a>Hur många enheter kan jag förbereda skickar vid hello samtidigt?

Det finns ingen gräns hello antalet diskar som hello verktyget kan förbereda. Dock förväntas hello verktyget enhetsbeteckningar som indata. Som begränsar den too25 samtidiga förberedelser. Ett enda jobb kan hantera högst 10 diskar i taget. Om du förbereder fler än 10 diskar riktad Hej samma lagringskonto, hello diskar kan fördelas på flera jobb.

#### <a name="can-i-target-more-than-one-storage-account"></a>Kan jag använda mer än en storage-konto?

Endast ett lagringskonto kan skickas per jobb och kopia session.

### <a name="capabilities"></a>Funktioner

#### <a name="does-waimportexportexe-encrypt-my-data"></a>Krypterar WAImportExport.exe Mina data?

Ja. BitLocker-kryptering har aktiverats och krävs för den här processen.

#### <a name="what-will-be-hello-hierarchy-of-my-data-when-it-appears-in-hello-storage-account"></a>Vad är hello hierarkin data när det visas i hello storage-konto?

Även om data distribueras över diskar hello data när det överförs toohello storage-konto kommer att konvergerat tillbaka toohello katalogstruktur som anges i hello dataset CSV-fil.

#### <a name="how-many-of-hello-input-disks-will-have-active-io-in-parallel-when-copy-is-in-progress"></a>Hur många av hello indata diskar ha aktiva IO parallellt, när kopiera pågår?

hello verktyget distribuerar data över hello inkommande diskar baserat på hello storleken på hello indatafiler. Namn, hello antalet aktiva diskar parallellt delends helt på hello uppbyggnad hello indata. Beroende på hello storleken på enskilda filer i hello inkommande dataset, kanske en eller flera diskar visas active-i/o parallellt. Nästa fråga mer information finns i.

#### <a name="how-does-hello-tool-distribute-hello-files-across-hello-disks"></a>Hur hello verktyget distribuerar hello filer över hello diskar?

WAImportExport verktyget läser och skriver filer batch av batch en batch innehåller max 100000 filer. Detta innebär att max 100000 filer kan skrivas parallellt. Flera diskar skrivs toosimultaneously om filerna 100000 distribuerade toomulti enheter. Men om hello skrivs toomultiple diskar samtidigt eller en enskild disk beror på hello sammanlagda storleken på hello batch. Till exempel vid mindre filer skriver om alla 10,0000 filer kan toofit i en enda enhet, verktyget tooonly en disk under hello bearbetningen av den här batchen.

### <a name="waimportexport-output"></a>WAImportExport utdata

#### <a name="there-are-two-journal-files-which-one-should-i-upload-tooazure-portal"></a>Det finns två journalfiler, vilket ska överföra tooAzure portal?

**XML** -för varje hårddisk som du förbereder med hello WAImportExport verktyget hello verktyget skapar en enda journal-fil med namnet `<DriveID>.xml` där enhets-ID är hello serienumret kopplat toohello enhet som hello verktyget läser från hello disk. Du behöver hello journalfiler från alla dina enheter toocreate hello importjobb i hello Azure-portalen. Journal-fil kan också vara används tooresume förberedelse av enheten om hello verktyget avbryts.

**.jrn** -hello journal-fil med suffixet `.jrn` innehåller hello status för alla sessioner med kopia för en hårddisk. Det innehåller även information om hello behövs toocreate hello importjobbet. Du måste alltid ange en journal-fil när körs hello WAImportExport verktyg, samt en kopia session-ID.

## <a name="next-steps"></a>Nästa steg

* [Ställa in hello Azure Import/Export-verktyget](storage-import-export-tool-setup.md)
* [Ange egenskaper och metadata under hello import-processen](storage-import-export-tool-setting-properties-metadata-import.md)
* [Exempel arbetsflödet tooprepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
* [Snabbreferens för vanliga kommandon](storage-import-export-tool-quick-reference.md) 
* [Granska jobbstatus med kopiera loggfiler](storage-import-export-tool-reviewing-job-status-v1.md)
* [Reparera ett importjobb](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Reparera ett exportjobb](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Felsöka hello Azure Import/Export-verktyget](storage-import-export-tool-troubleshooting-v1.md)
