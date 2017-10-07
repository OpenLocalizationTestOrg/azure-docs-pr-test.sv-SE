---
title: "aaaPreparing hårddiskar för Azure Import/Export-importjobb - v1 | Microsoft Docs"
description: "Lär dig hur tooprepare hårddiskar med hello WAImportExport v1 verktyget toocreate ett importjobb för hello Azure Import/Export-tjänsten."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 3d818245-8b1b-4435-a41f-8e5ec1f194b2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 8803ac01b7c7a2ec2e3199231d7b4c04ceafd5a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-hard-drives-for-an-import-job"></a>Förbereda hårddiskar för ett importjobb
tooprepare en eller flera hårddiskar för ett importjobb följa dessa steg:

-   Identifiera hello data tooimport till hello Blob-tjänsten

-   Identifiera mål virtuella kataloger respektive blobbar i hello Blob-tjänsten

-   Avgöra hur många enheter som du behöver

-   Kopiera hello data tooeach för dina enheter

 För ett Exempelarbetsflöde finns [Exempelarbetsflöde tooPrepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md).

## <a name="identify-hello-data-toobe-imported"></a>Identifiera hello data toobe importeras
 hello första steg toocreating importen är toodetermine vilka kataloger och filer som ska tooimport. Detta kan vara en lista över kataloger, en lista med unika filer eller en kombination av dessa två. När en katalog finns ska alla filer i hello katalogen och dess underkataloger ingå i hello importjobbet.

> [!NOTE]
>  Eftersom underkataloger är inkluderade rekursivt när en överordnad katalog finns, ange bara hello överordnade katalogen. Även ange inte någon av dess underkataloger.
>
>  För närvarande hello Microsoft Azure Import/Export-verktyget har hello efter begränsning: om en katalog innehåller mer data än en hårddisk kan innehålla, måste hello directory toobe uppdelade mindre kataloger. Till exempel om en katalog innehåller 2,5 TB data och hello-hårddiskkapacitet är endast 2TB och du behöver toobreak hello 2,5 TB directory mindre kataloger. Den här begränsningen åtgärdas i en senare version av hello-verktyget.

## <a name="identify-hello-destination-locations-in-hello-blob-service"></a>Identifiera hello målplatser i hello blob-tjänsten
 För varje mapp eller fil som ska importeras måste tooidentify en virtuell katalog för mål eller blob i hello Azure Blob-tjänsten. Du använder dessa mål som indata toohello Azure Import/Export-verktyget. Observera att kataloger ska avgränsas med hello snedstreck ”/”.

 hello följande tabell visas några exempel på blob mål:

|Käll-filen eller katalogen|Mål-blob eller virtuell katalog|
|------------------------------|-------------------------------------------|
|H:\Video|https://mystorageaccount.BLOB.Core.Windows.NET/video|
|H:\Photo|https://mystorageaccount.BLOB.Core.Windows.NET/Photo|
|K:\Temp\FavoriteVideo.ISO|https://mystorageaccount.BLOB.Core.Windows.NET/favorite/FavoriteVideo.ISO|
|\\\myshare\john\music|https://mystorageaccount.BLOB.Core.Windows.NET/Music|

## <a name="determine-how-many-drives-are-needed"></a>Avgör hur många enheter som krävs
 Därefter behöver du toodetermine:

-   hello antalet hårddiskar behövs toostore hello data.

-   hello kataloger och/eller fristående filer som ska kopieras tooeach på hårddisken.

 Se till att du har hello antalet hårddiskar som du behöver toostore hello data överförs.

## <a name="copy-data-tooyour-hard-drive"></a>Kopiera data tooyour hårddisk
 Det här avsnittet beskrivs hur toocall hello Azure Import/Export verktyget toocopy data-tooone eller hårddiskar. Varje gång du anropar hello Azure Import/Export-verktyget kan du skapa en ny *kopiera session*. Du skapar minst en kopia sessionen för varje enhet toowhich du kopiera data. i vissa fall kan du behöva mer än en kopia session toocopy alla data toosingle enheten. Här följer några orsaker till att du kanske måste flera kopiera sessioner:

-   Du måste skapa en separat kopia-session för varje enhet som du kopierar till.

-   En kopia session kan kopiera en katalog eller en enda blob toohello enhet. Om du kopierar flera kataloger, flera blobbar eller en kombination av båda, behöver du toocreate flera kopiera sessioner.

-   Du kan ange egenskaper och metadata som ställs in på hello BLOB importeras som en del av ett importjobb. hello egenskaper eller metadata som du anger för en kopia session gäller tooall blobbar som anges av den aktuella sessionen kopia. Om du vill toospecify olika egenskaper eller metadata för vissa BLOB, behöver toocreate som en separat kopiera session. Se [egenskaper och Metadata under hello importen](storage-import-export-tool-setting-properties-metadata-import-v1.md)för mer information.

> [!NOTE]
>  Om du har flera datorer som uppfyller hello kraven som anges i [ställa in hello Azure Import/Export verktyget](storage-import-export-tool-setup-v1.md), du kan kopiera data toomultiple hårddiskar parallellt genom att köra en instans av det här verktyget på varje dator.

 För varje hårddisk som du förbereder med hello Azure Import/Export verktyget skapar hello verktyget en enda journal-fil. Du behöver hello journalfiler från alla dina enheter toocreate hello-importjobbet. hello journal-fil kan också vara används tooresume förberedelse av enheten om hello verktyget avbryts.

### <a name="azure-importexport-tool-syntax-for-an-import-job"></a>Azure Import/Export-verktyget syntaxen för ett importjobb
 tooprepare enheter för ett importjobb anropa hello Azure Import/Export verktyget med hello **PrepImport** kommando. Vilka parametrar du inkludera beror på om detta är hello första kopian session eller en efterföljande kopia-session.

 hello kräver den första kopian sessionen för en enhet vissa ytterligare parametrar toospecify hello lagringskontonyckel; hello mål enhetsbeteckning; Om hello måste vara formaterad; Om hello enheten måste krypteras och i så fall, hello BitLocker-nyckel. och hello loggkatalogen. Här är hello syntax för en inledande kopia session toocopy en katalog eller en enskild fil:

 **Först kopiera session toocopy en katalog**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Först kopiera session toocopy en enda fil**

 `WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 I efterföljande kopiera sessioner behöver inte toospecify hello inledande parametrar. Här är hello syntax för en efterföljande kopiera session toocopy en katalog eller en enskild fil:

 **Efterföljande kopiera sessioner toocopy en katalog**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

 **Efterföljande kopiera sessioner toocopy en enda fil**

 `WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]`

### <a name="parameters-for-hello-first-copy-session-for-a-hard-drive"></a>Parametrar för hello först kopiera sessionen för en hårddisk
 Varje gång du kör hello Azure Import/Export verktyget toocopy filer toohello hårddisk, skapar hello verktyget en kopia-session. Varje kopia session kopierar en katalog eller en enskild fil tooa hårddisk. hello tillståndet för hello kopiera session skrivs toohello journal-fil. Om en kopia sessionen avbryts (t.ex, på grund av strömavbrott för tooa system), kan det återupptas genom att köra verktyget hello igen och ange hello journal-fil på hello-kommandoraden.

> [!WARNING]
>  Om du anger hello **/formatera** parameter för hello första kopian sessionen hello enheten formateras och raderas alla data på hello enhet. Det rekommenderas att du använder tomma enheter endast för kopian sessionen.

 hello-kommando som används för hello den första kopian sessionen för varje enhet kräver olika parametrar än hello kommandon för efterföljande kopiera sessioner. hello visas följande tabell hello ytterligare parametrar som är tillgängliga för hello första kopian sessionen:

|Kommandoradsparametern|Beskrivning|
|-----------------------------|-----------------|
|**/Sk:**< StorageAccountKey\>|`Optional.`Hej lagringskontonyckel för hello storage-konto toowhich hello data importeras. Du måste inkludera antingen **/sk:**< StorageAccountKey\> eller **/csas:**< ContainerSas\> i hello-kommandot.|
|**/csas:**< ContainerSas\>|`Optional`. hello behållare SAS toouse tooimport data toohello storage-konto. Du måste inkludera antingen **/sk:**< StorageAccountKey\> eller **/csas:**< ContainerSas\> i hello-kommandot.<br /><br /> hello-värdet för den här parametern måste börja med hello behållarens namn följt av ett frågetecken (?) och hello SAS-token. Exempel:<br /><br /> `mycontainer?sv=2014-02-14&sr=c&si=abcde&sig=LiqEmV%2Fs1LF4loC%2FJs9ZM91%2FkqfqHKhnz0JM6bqIqN0%3D&se=2014-11-20T23%3A54%3A14Z&sp=rwdl`<br /><br /> hello behörigheter om anges på hello-URL eller i en lagrad åtkomstprincip, måste innehålla läsa, skriva och ta bort för importjobb och läsa, skriva och lista för exportjobb.<br /><br /> När den här parametern anges måste alla blobbar toobe importeras eller exporteras ligga inom hello-behållare som angavs i hello signatur för delad åtkomst.|
|**/ t:**< TargetDriveLetter\>|`Required.`hello enhetsbeteckningen för hello mål hårddisk för hello aktuella kopia sessionen, utan hello avslutande kolon.|
|**/ format**|`Optional.`Ange den här parametern när hello enhet behöver toobe formaterad; Annars utelämna den. Innan hello verktyget formaterar hello enheten, uppmanas en bekräftelse från konsolen. toosuppress hello bekräftelse, ange hello /silentmode parameter.|
|**/silentmode**|`Optional.`Ange den här parametern toosuppress hello bekräftelse för att formatera hello targert enhet.|
|**/ encrypt**|`Optional.`Ange den här parametern när hello enhet ännu inte har krypterats med BitLocker och behov toobe som krypterats av hello-verktyget. Om hello enheten redan har krypterats med BitLocker utelämna parametern och ange hello `/bk` parameter för att tillhandahålla hello befintlig BitLocker-nyckel.<br /><br /> Om du anger hello `/format` parameter och du måste också ange hello `/encrypt` parameter.|
|**/BK:**< BitLockerKey\>|`Optional.`Om `/encrypt` har angetts utesluter den här parametern. Om `/encrypt` är utelämnas måste toohave har redan krypterat enheten hello med BitLocker. Använd den här parametern toospecify hello BitLocker-nyckel. BitLocker-kryptering krävs för alla hårddiskar för importjobb.|
|**/logdir:**< LogDirectory\>|`Optional.`hello loggkatalogen anger en katalog toobe toostore detaljerade loggarna som tillfällig manifest-filer. Om inget anges används hello katalogen som hello loggkatalogen.|

### <a name="parameters-required-for-all-copy-sessions"></a>Parametrar som krävs för alla kopiera sessioner
 hello journal-fil innehåller hello status för alla sessioner med kopia för en hårddisk. Det innehåller även information om hello behövs toocreate hello importjobbet. Du måste alltid ange en journal-fil när körs hello Azure Import/Export-verktyg, samt en kopia sessions-ID:

|||
|-|-|
|Kommandoradsparameter|Beskrivning|
|**/j:**< JournalFile\>|`Required.`hello sökvägen toohello journal-fil. Varje enhet måste ha exakt en journal-fil. Observera hello journalfilen inte måste finnas på hello målenheten. hello journal filnamnstillägget `.jrn`.|
|**/ID:**< sessions-ID\>|`Required.`hello sessions-ID identifierar en kopia-session. Det är används tooensure korrekt återställning av en session avbryts kopia. Filer som kopieras i en kopia session lagras i katalogen efter hello sessions-ID på hello målenheten.|

### <a name="parameters-for-copying-a-single-directory"></a>Parametrar för att kopiera en katalog
 När du kopierar en katalog gäller hello följande nödvändiga och valfria parametrar finns:

|Kommandoradsparameter|Beskrivning|
|----------------------------|-----------------|
|**/srcdir:**< SourceDirectory\>|`Required.`hello källkatalog som innehåller filer toobe kopieras toohello målenheten. hello sökväg måste vara en absolut sökväg (inte en relativ sökväg).|
|**/dstdir:**< DestinationBlobVirtualDirectory\>|`Required.`hello sökvägen toohello mål virtuell katalog på din Windows Azure storage-konto. hello virtuell katalog kan eller kan inte redan finns.<br /><br /> Du kan ange en behållare eller ett blob-prefix som `music/70s/`. hello målkatalogen måste börja med hello behållarens namn följt av ett snedstreck ”/”, och du kan också omfatta en virtuell blob-katalog som slutar med ”/”.<br /><br /> När hello målbehållare är hello Rotbehållare, måste du uttryckligen ange hello Rotbehållare, inklusive hello snedstreck som `$root/`. Eftersom blobbar under hello Rotbehållare inte får innehålla ”/” i sina namn, kopieras inte eventuella underkataloger hello källkatalogen när hello målkatalogen är hello Rotbehållare.<br /><br /> Vara säker på att toouse giltig behållarnamn när du ska ange mål virtuella kataloger eller BLOB. Tänk på att behållarnamn måste vara gemener. Behållaren namngivningsregler, se [namnge och referera till behållare, Blobbar och Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).|
|**/ Disposition:**< Byt namn på &#124; Nej skriva över &#124; skriva över >|`Optional.`Anger hello beteendet när en blob med hello anges adressen finns redan. Giltiga värden för den här parametern är: `rename`, `no-overwrite` och `overwrite`. Observera att dessa värden är skiftlägeskänsliga. Om inget värde anges är standardvärdet för hello `rename`.<br /><br /> Hej värde som angetts för denna parameter påverkar alla hello-filer i hello-katalogen som anges av hello `/srcdir` parameter.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Anger hello blob-datatyp för hello mål-BLOB. Giltiga värden är: `BlockBlob` och `PageBlob`. Observera att dessa värden är skiftlägeskänsliga. Om inget värde anges är standardvärdet för hello `BlockBlob`.<br /><br /> I de flesta fall `BlockBlob` rekommenderas. Om du anger `PageBlob`, hello längden på varje fil i hello måste vara delbar med 512, hello storleken på en sida av sidblobar.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Sökvägen toohello egenskapsfil för hello mål-BLOB. Se [Import/Export service Metadata och egenskaper filformatet](storage-import-export-file-format-metadata-and-properties.md) för mer information.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Sökvägen toohello metadatafil för hello mål-BLOB. Se [Import/Export service Metadata och egenskaper filformatet](storage-import-export-file-format-metadata-and-properties.md) för mer information.|

### <a name="parameters-for-copying-a-single-file"></a>Parametrar för att kopiera en fil
 När du kopierar en fil hello följande obligatoriska och valfria parametrar gäller:

|Kommandoradsparameter|Beskrivning|
|----------------------------|-----------------|
|**/srcfile:**< SourceFile\>|`Required.`hello fullständig sökväg toohello filen toobe kopieras. hello sökväg måste vara en absolut sökväg (inte en relativ sökväg).|
|**/dstblob:**< DestinationBlobPath\>|`Required.`hello sökvägen toohello mål blob i ditt Windows Azure storage-konto. hello blob kanske eller kanske inte redan finns.<br /><br /> Ange hello blob namn som börjar med hello behållarnamn. hello blob-namnet får inte börja med ”/” eller hello lagringskontonamn. Blob namngivningsregler, se [namnge och referera till behållare, Blobbar och Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).<br /><br /> När hello målbehållare är hello Rotbehållare, måste du uttryckligen ange `$root` som hello-behållaren som `$root/sample.txt`. Observera att blobbar under hello Rotbehållare får inte innehålla ”/” i sina namn.|
|**/ Disposition:**< Byt namn på &#124; Nej skriva över &#124; skriva över >|`Optional.`Anger hello beteendet när en blob med hello anges adressen finns redan. Giltiga värden för den här parametern är: `rename`, `no-overwrite` och `overwrite`. Observera att dessa värden är skiftlägeskänsliga. Om inget värde anges är standardvärdet för hello `rename`.|
|**/ BlobType:**< BlockBlob &#124; PageBlob >|`Optional.`Anger hello blob-datatyp för hello mål-BLOB. Giltiga värden är: `BlockBlob` och `PageBlob`. Observera att dessa värden är skiftlägeskänsliga. Om inget värde anges är standardvärdet för hello `BlockBlob`.<br /><br /> I de flesta fall `BlockBlob` rekommenderas. Om du anger `PageBlob`, hello längden på varje fil i hello måste vara delbar med 512, hello storleken på en sida av sidblobar.|
|**/ PropertyFile:**< PropertyFile\>|`Optional.`Sökvägen toohello egenskapsfil för hello mål-BLOB. Se [Import/Export service Metadata och egenskaper filformatet](storage-import-export-file-format-metadata-and-properties.md) för mer information.|
|**/ MetadataFile:**< MetadataFile\>|`Optional.`Sökvägen toohello metadatafil för hello mål-BLOB. Se [Import/Export service Metadata och egenskaper filformatet](storage-import-export-file-format-metadata-and-properties.md) för mer information.|

### <a name="resuming-an-interrupted-copy-session"></a>Återupptar en session avbryts kopia
 Om en kopia sessionen avbryts av någon anledning, kan du återuppta den genom att köra verktyget hello med endast hello journal angivna filen:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession
```

 Om avslutats onormalt, kan bara hello senaste kopiera session, återupptas.

> [!IMPORTANT]
>  När du återupptar en kopia session inte ändra hello Källdatafiler och kataloger genom att lägga till eller ta bort filer.

### <a name="aborting-an-interrupted-copy-session"></a>Avbryter en session avbryts kopia
 Om en kopia sessionen avbryts och det är inte möjligt tooresume (till exempel om en källkatalog visat tillgänglig), du måste avbryta hello aktuell session så att den kan återställas startas tillbaka och nya kopiera sessioner:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession
```

 Endast hello kan senaste kopiera sessionen om avslutats onormalt, avbrytas. Observera att du inte kan avbryta hello den första kopian sessionen för en enhet. I stället måste du starta om hello kopiera session med en ny journalfil.

## <a name="next-steps"></a>Nästa steg

* [Ställa in hello Azure Import/Export-verktyget](storage-import-export-tool-setup-v1.md)
* [Ange egenskaper och metadata under hello import-processen](storage-import-export-tool-setting-properties-metadata-import-v1.md)
* [Exempel arbetsflödet tooprepare hårddiskar för ett importjobb](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
* [Snabbreferens för vanliga kommandon](storage-import-export-tool-quick-reference-v1.md) 
* [Granska jobbstatus med kopiera loggfiler](storage-import-export-tool-reviewing-job-status-v1.md)
* [Reparera ett importjobb](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Reparera ett exportjobb](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Felsöka hello Azure Import/Export-verktyget](storage-import-export-tool-troubleshooting-v1.md)
