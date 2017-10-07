---
title: aaaCopy eller flytta data tooAzure lagring med AzCopy i Windows | Microsoft Docs
description: "Använd hello AzCopy på Windows-verktyget toomove eller kopiera data tooor från blob-, tabell- och innehåll. Kopiera data tooAzure lagring från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data tooAzure lagring."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: a063a0380b7b8e6b212d0cec276f7d0f421936ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-azcopy-on-windows"></a>Överföra data med hello AzCopy i Windows
AzCopy är ett kommandoradsverktyg som utformats för att kopiera data tooand från Microsoft Azure Blob-, fil- och Table storage med hjälp av enkla kommandon med optimala prestanda. Du kan kopiera data från en objektet tooanother inom ditt lagringskonto eller mellan lagringskonton.

Det finns två versioner av AzCopy som du kan hämta. AzCopy i Windows har skapats med .NET Framework och erbjuder kommandoradsalternativ för Windows-formatet. [AzCopy på Linux](storage-use-azcopy-linux.md) har byggts med .NET Core Framework som riktar sig till Linux-plattformar som erbjuder POSIX format kommandoradsalternativ. Den här artikeln beskriver AzCopy i Windows.

## <a name="download-and-install-azcopy"></a>Hämta och installera AzCopy
### <a name="azcopy-on-windows"></a>AzCopy i Windows
Hämta hello [senaste versionen av AzCopy på Windows](http://aka.ms/downloadazcopy).

#### <a name="installation-on-windows"></a>Installation på Windows
När du har installerat AzCopy i Windows med hello installer, öppna ett kommandofönster och gå toohello installationskatalogen för AzCopy på din dator - där hello `AzCopy.exe` körbara finns. Om du vill kan du lägga till hello AzCopy tooyour systemsökvägen installationsplatsen. Som standard installeras AzCopy för`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` eller `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

## <a name="writing-your-first-azcopy-command"></a>Skriva ditt första AzCopy-kommandot
grundläggande hello-syntaxen för AzCopy kommandon är:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

hello som följande exempel visar en mängd olika scenarier för att kopiera data tooand från Microsoft Azure-Blobbar, filer och tabeller. Se toohello [AzCopy parametrar](#azcopy-parameters) avsnittet för en detaljerad förklaring av hello parametrar som används i varje prov.

## <a name="blob-download"></a>BLOB: ladda ned
### <a name="download-single-blob"></a>Hämta en enda blob

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Observera att om hello mappen `C:\myfolder` finns inte, AzCopy skapar den och ladda ned `abc.txt ` till hello ny mapp.

### <a name="download-single-blob-from-secondary-region"></a>Hämta en enda blob från sekundär region

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.

### <a name="download-all-blobs"></a>Hämta alla BLOB

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Anta hello följande blobbar finns i angivna hello-behållaren:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Efter hello hämtningen hello directory `C:\myfolder` innehåller hello följande filer:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Om du inte anger alternativet `/S`, hämtas inga blobbar.

### <a name="download-blobs-with-specified-prefix"></a>Ladda ned blobbar med angivna prefix

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Anta hello följande blobbar finns i hello angivna behållaren. Alla blobbar som börjar med prefixet hello `a` hämtas:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Efter hello hämtningen hello mappen `C:\myfolder` innehåller hello följande filer:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

hello prefix gäller toohello virtuella katalogen, som utgör hello första delen av hello blob-namnet. I hello exemplet ovan matchar hello virtuella katalogen inte hello angivna prefix, så inte hämtas. Dessutom, om hello alternativet `\S` anges AzCopy inte hämta alla blobbar.

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a>Ange hello modifierades senast av exporterade filerna toobe samma som hello källa blobbar

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Du kan också utesluta blobbar från hello hämtningen baserat på deras tid för senaste ändring. Till exempel om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller en senare än hello målfilen, lägga till hello `/XN` alternativ:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Eller om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller äldre än hello målfilen, lägger du till hello `/XO` alternativ:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a>BLOB: Överför
### <a name="upload-single-file"></a>Överför en fil

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Om hello angivna målbehållare inte finns, AzCopy skapar den och överföringar hello filen till den.

### <a name="upload-single-file-toovirtual-directory"></a>Överför en fil toovirtual directory

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Om hello angivna virtuella katalogen inte finns, överför AzCopy hello filen tooinclude hello virtuell katalog på sitt namn (*t.ex.*, `vd/abc.txt` i hello-exemplet ovan).

### <a name="upload-all-files"></a>Ladda upp alla filer

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Om du anger alternativet `/S` överföringar hello innehållet i hello angetts directory tooBlob lagring rekursivt, vilket innebär att alla undermappar och filer överförs också. Anta exempelvis att hello följande filer finns i mappen `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Efter hello överföringen innehåller hello behållaren hello följande filer:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Om du inte anger alternativet `/S`, AzCopy inte överföra rekursivt. Efter hello överföringen innehåller hello behållaren hello följande filer:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Överföra filer som matchar angivna mönstret

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Anta hello följande filer finns i mappen `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Efter hello överföringen innehåller hello behållaren hello följande filer:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Om du inte anger alternativet `/S`, AzCopy Överför bara blob som inte finns i en virtuell katalog:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a>Ange hello MIME content-type för en mål-blob
Som standard AzCopy anger hello innehållstypen för en mål-blob för`application/octet-stream`. Från och med version 3.1.0, anger du hello innehållstyp via hello alternativet `/SetContentType:[content-type]`. Den här syntaxen anger hello content-type för alla blobbar i en överföringen.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Om du anger `/SetContentType` utan värde, AzCopy anger varje blob eller filens innehållstyp enligt tooits filnamnstillägg.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a>BLOB: kopiera
### <a name="copy-single-blob-within-storage-account"></a>Kopiera enda blob inom lagringskonto

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

När du kopierar en blobb inom ett lagringskonto, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopiera enda blob på Storage-konton

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

När du kopierar en blobb mellan lagringskonton, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a>Kopiera enda blob från sekundär region tooprimary region

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopiera enda blob och dess ögonblicksbilder på Storage-konton

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Efter hello kopieringen innehåller hello Målbehållaren hello blob och dess ögonblicksbilder. Under förutsättning att hello blob i hello-exemplet ovan har två ögonblicksbilder, hello behållaren omfattar hello följande blob och ögonblicksbilder:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Kopiera synkront blobar på lagringskonton
AzCopy som standard kopierar data mellan två slutpunkter för lagring asynkront. Därför kopieras hello Kopiera åtgärden körs i hello bakgrunden med hjälp av ledig bandbreddskapacitet som har inga SLA vad gäller hur snabbt en blob och AzCopy söker regelbundet hello-kopian status tills hello kopiera är slutförd eller misslyckad.

Hej `/SyncCopy` alternativet ser du till att hello kopieringsåtgärden hämtar konsekvent hastighet. AzCopy utför hello synkron kopia genom att hämta hello blobbar toocopy från hello anges källa toolocal minne, och överföra dem toohello Blob-lagringsplats.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

`/SyncCopy`kan skapa ytterligare utgång kostnaden jämfört med tooasynchronous kopiera hello rekommenderade metoden är toouse det här alternativet i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.

## <a name="file-download"></a>Fil: ladda ned
### <a name="download-single-file"></a>Hämta en fil

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Om hello anges källan är en Azure-filresurs, måste du antingen ange hello exakta filnamnet (*t.ex.* `abc.txt`) toodownload en enskild fil, eller ange alternativet `/S` toodownload alla filer i hello resurs rekursivt. Försök toospecify både filmönstret och alternativet `/S` tillsammans resulterar i ett fel.

### <a name="download-all-files"></a>Hämta alla filer

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Observera att alla tomma mappar inte laddas ned.

## <a name="file-upload"></a>Fil: Överför
### <a name="upload-single-file"></a>Överför en fil

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a>Ladda upp alla filer

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Observera att alla tomma mappar inte upp.

### <a name="upload-files-matching-specified-pattern"></a>Överföra filer som matchar angivna mönstret

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a>Fil: kopiera
### <a name="copy-across-file-shares"></a>Kopiera på filresurser

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
När du kopierar en fil på filresurser, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-from-file-share-tooblob"></a>Kopiera från filen resursen tooblob

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
När du kopierar en fil från filen resursen tooblob en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.


### <a name="copy-from-blob-toofile-share"></a>Kopiera från blob toofile resurs

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
När du kopierar en fil från blob toofile resurs, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="synchronously-copy-files"></a>Synkront kopiera filer
Du kan ange hello `/SyncCopy` alternativet toocopy data från fillagring tooFile lagring, från fillagring tooBlob lagring och från Blobblagring tooFile lagring synkront, AzCopy gör detta genom att hämta hello källa data toolocal minne och ladda upp den toodestination igen. Standard utgång kostnaden gäller.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

När du kopierar från fillagring tooBlob lagring hello standard blob-datatyp är blockblob, kan användaren ange alternativet `/BlobType:page` toochange hello blob måltypen.

Observera att `/SyncCopy` kan skapa ytterligare utgång kostnad jämför tooasynchronous kopia, hello rekommenderade metoden är det här alternativet i hello Azure VM som är i hello toouse samma region som din datakälla konto tooavoid utgång lagringskostnaden.

## <a name="table-export"></a>Tabell: exportera
### <a name="export-table"></a>Exportera tabell

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy skriver en manifestfil toohello angivna målmapp. hello manifestfilen används i hello importera processen toolocate hello nödvändiga filer och utföra dataverifiering. hello manifestfilen använder hello följa en namngivningskonvention som standard:

    <account name>_<table name>_<timestamp>.manifest

Användaren kan även ange hello alternativet `/Manifest:<manifest file name>` tooset hello Manifestfilens filnamn.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a>Dela export i flera filer

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy använder en *volymindex* i hello dela data filnamnen toodistinguish flera filer. hello volymindex består av två delar, en *partition viktiga områdesindex* och en *dela filen index*. Både index är nollbaserade.

hello Partitionsindex viktiga intervallet är 0 om hello användare inte anger alternativet `/PKRS`.

Anta exempelvis AzCopy genererar två datafiler när hello användaren anger alternativet `/SplitSize`. hello resulterande filnamn data kan vara:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Observera att hello minsta möjliga värde för alternativet `/SplitSize` 32 MB. Om du hello målet är Blob storage AzCopy delar hello datafil när dess storlek når hello blob storleksbegränsning (200GB), oavsett om alternativet `/SplitSize` har angetts av hello användare.

### <a name="export-table-toojson-or-csv-data-file-format"></a>Exportera tabell tooJSON eller data för filformatet CSV
AzCopy som standard exporterar tabeller tooJSON datafiler. Du kan ange hello alternativet `/PayloadFormat:JSON|CSV` tooexport hello tabeller som JSON- eller CSV.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

När du anger nyttolast hello CSV-format, AzCopy också genererar en schemafil med filnamnstillägget `.schema.csv` för varje datafil.

### <a name="export-table-entities-concurrently"></a>Exportera tabellentiteter samtidigt

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy startar samtidiga åtgärder tooexport entiteter när hello användaren anger alternativet `/PKRS`. Varje åtgärd exporterar en partitionsnyckelintervallet.

Observera att hello antalet samtidiga åtgärder också styrs av alternativet `/NC`. AzCopy använder hello antalet kärnprocessorer som hello standardvärdet `/NC` när du kopierar tabellen enheter, även om `/NC` har inte angetts. När hello användaren anger alternativet `/PKRS`, AzCopy använder hello mindre hello två värden - partition nyckelintervall jämfört med implicit eller explicit angivna samtidiga åtgärder - toodetermine hello antalet samtidiga åtgärder toostart. Mer information, Skriv `AzCopy /?:NC` hello-kommandoraden.

### <a name="export-table-tooblob"></a>Exportera tabellen tooblob

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy genererar en JSON-datafil till hello blob-behållare med följande namngivningskonvention:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

hello genereras JSON-datafilen följer hello nyttolastformat för minimal metadata. Mer information om den här nyttolastformatet finns [Nyttolastformat för tabellen tjänståtgärder](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Observera att när du exporterar tabeller tooblobs AzCopy hämtar hello tabell entiteter toolocal temporära filer och överför dessa enheter toohello blob. Dessa temporära datafiler placeras i hello journalmapp med hello standardsökvägen ”<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>”, du kan ange alternativet/Z: [journal-filmapp] toochange hello journal sökvägen för mappen och därmed ändra hello plats för temporära filer. hello tillfälliga data filernas storlek bestäms av din tabellentiteter och hello storleken som du har angett med hello alternativet /SplitSize även om hello temporär fil i lokal disk tas bort omedelbart när det har laddats upp toohello blob, kontrollera att du har tillräckligt med lokalt ledigt utrymme toostore filerna tillfälliga data innan de tas bort.

## <a name="table-import"></a>Tabell: Import
### <a name="import-table"></a>Import av tabell

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

Hej alternativet `/EntityOperation` anger hur hello tooinsert entiteter i tabellen. Möjliga värden:

* `InsertOrSkip`: Hoppar över en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.
* `InsertOrMerge`: Sammanfogar en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.
* `InsertOrReplace`: Ersätter en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.

Observera att du inte kan ange alternativet `/PKRS` i hello importera scenario. Till skillnad från hello export scenario där du måste ange alternativet `/PKRS` toostart samtidiga åtgärder, AzCopy samtidiga åtgärder som standard startar när du importerar en tabell. hello standardantalet samtidiga åtgärder igång är lika toohello antalet kärnprocessorer; Du kan dock ange ett annat antal samtidiga med alternativet `/NC`. Mer information, Skriv `AzCopy /?:NC` hello-kommandoraden.

Observera att AzCopy endast stöder import för JSON inte CSV. AzCopy har inte stöd för import av tabell från användarskapade JSON och manifest filer. Båda dessa filer måste komma från en tabell AzCopy-export. tooavoid fel, ändra inte hello exporteras JSON eller manifestfilen.

### <a name="import-entities-tootable-using-blobs"></a>Importera entiteter tootable blobbar
Anta en Blob-behållare innehåller hello följande: en JSON-fil som representerar en Azure-tabellen och dess tillhörande manifestfilen.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Du kan köra hello efter kommandot tooimport enheter till en tabell med hello manifestfilen i den blob-behållaren:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Andra AzCopy-funktioner
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a>Endast kopiera data som inte finns i hello mål
Hej `/XO` och `/XN` parametrar kan du tooexclude äldre eller nyare resurser från att kopieras respektive. Om du bara vill toocopy källa resurser som inte finns i hello målet kan du ange båda parametrarna i hello AzCopy-kommandot:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Observera att detta inte stöds när hello källan eller målet är en tabell.

### <a name="use-a-response-file-toospecify-command-line-parameters"></a>Använd en toospecify kommandoradsverktyget svarsfilparametrar

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

Du kan inkludera eventuella kommandoradsparametrar för AzCopy i en svarsfil. AzCopy processer hello parametrar i hello-filen som om de hade angetts på kommandoraden för hello utför en direkt ersättning med hello innehållet i hello-fil.

Anta en svarsfil med namnet `copyoperation.txt`, som innehåller hello följande rader. Varje AzCopy-parameter kan anges på en enda rad

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

eller på separata rader:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy misslyckas om du vill dela hello parametern på två rader som visas här för hello `/sourcekey` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-toospecify-command-line-parameters"></a>Använda flera svar filer toospecify-kommandoradsparametrar
Anta en svarsfil med namnet `source.txt` som anger en käll-behållare:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Och en svarsfil med namnet `dest.txt` som anger en målmapp i hello file system:

    /Dest:C:\myfolder

Och en svarsfil med namnet `options.txt` som anger alternativ för AzCopy:

    /S /Y

toocall AzCopy med dessa svarsfiler som finns i en katalog `C:\responsefiles`, använder du följande kommando:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy bearbetar det här kommandot precis som om du har lagt till alla hello enskilda parametrar på kommandoraden för hello:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Ange en signatur för delad åtkomst (SAS)

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Du kan också ange en SAS för hello behållaren URI:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Ändringsjournalen mapp
Varje gång du utfärda ett kommando tooAzCopy kontrollerar den om en journal-fil finns i hello standardmappen eller om den finns i en mapp som du har angett via det här alternativet. Om hello journalfilen inte finns på någon plats, behandlar hello åtgärden som nya AzCopy och genererar en ny journalfil.

Om hello journalfilen finns kontrollerar AzCopy om hello-kommandoraden som du matar in matchar hello kommandoraden i hello journal-fil. Om hello två kommandorader matchar återupptar AzCopy hello ofullständiga igen. Om de inte matchar kan du ange tooeither överskrivning hello journal filen toostart en ny åtgärd eller toocancel hello aktuella åtgärden.

Om du vill toouse hello standardplatsen för hello journal-fil:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Om du utelämnar alternativet `/Z`, eller ange alternativet `/Z` utan hello mappsökvägen, enligt ovan, AzCopy skapar hello journal-fil i hello standardplatsen, som är `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Om hello journalfilen redan finns återupptas AzCopy hello-åtgärden utifrån hello journal-fil.

Om du vill toospecify en egen placering för hello journal-fil:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

Det här exemplet skapar hello journal-filen om den inte redan finns. Om den finns, återupptar AzCopy hello-åtgärden utifrån hello journal-fil.

Om du vill tooresume en AzCopy-åtgärd:

```azcopy
AzCopy /Z:C:\journalfolder\
```

Det här exemplet återupptar hello senaste åtgärd, som kan ha misslyckats toocomplete.

### <a name="generate-a-log-file"></a>Generera en loggfil

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Om du anger alternativet `/V` utan att ange en sökväg toohello utförlig filloggen sedan AzCopy hello loggfil skapas i hello standardplatsen, som är `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Annars kan du skapa en loggfil på en annan plats:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Observera att om du anger en relativ sökväg efter alternativet `/V`, som `/V:test/azcopy1.log`, och sedan hello utförlig loggen skapas i hello aktuella arbetskatalogen i en undermapp som heter `test`.

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a>Ange hello antalet samtidiga åtgärder toostart
Alternativet `/NC` anger hello antalet samtidiga kopieringsåtgärd. Som standard startar AzCopy antalet samtidiga åtgärder tooincrease hello data transfer genomflöde. För tabellen är hello antalet samtidiga åtgärder lika toohello antal processorer som du har. För Blob- och fil-åtgärder, hello antalet samtidiga åtgärder är lika med 8 gånger hello antal processorer som du har. Om du använder AzCopy över ett nätverk med låg bandbredd, kan du ange ett lägre värde /NC tooavoid misslyckades på grund av konkurrens om resurser.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Köra AzCopy mot Azure Storage-emulatorn
Du kan köra AzCopy mot hello [Azure Storage-emulatorn](storage-use-emulator.md) för BLOB:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

och tabeller:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>AzCopy-parametrar
Parametrar för AzCopy beskrivs nedan. Du kan också ange ett av följande kommandon från hello kommandoraden för att få hjälp med AzCopy hello:

* För detaljerad hjälp för AzCopy:`AzCopy /?`
* För detaljerad hjälp om någon AzCopy-parameter:`AzCopy /?:SourceKey`
* Kommandoradsverktyget exempel:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Source: ”källa”
Anger hello källdata från vilken toocopy. hello källan kan vara en katalog i filsystemet, en blob-behållare, en virtuell katalog för blob, en lagringsfilresurs, en katalog i lagring eller en Azure-tabellen.

**Gäller för:** Blobbar, filer, tabeller

### <a name="destdestination"></a>/ Dest: ”mål”
Anger hello mål toocopy till. hello målet kan vara en katalog i filsystemet, en blob-behållare, en virtuell katalog för blob, en lagringsfilresurs, en katalog i lagring eller en Azure-tabellen.

**Gäller för:** Blobbar, filer, tabeller

### <a name="patternfile-pattern"></a>/ Mönstret: ”filmönstret”
Anger ett fil-mönster som anger vilka filer toocopy. hello funktionssätt hello /Pattern parametern bestäms av hello plats hello källdata och hello förekomst av hello rekursiv läge. Rekursiva läge har angetts via alternativet/s.

Om hello angiven källa är en katalog i hello filsystemet standard jokertecken är aktiva och hello filen mönster som matchas mot filer i hello katalog. Om alternativet /S anges sedan AzCopy också matchar hello angivna mönstret mot alla filer i alla undermappar under hello directory.

Om hello angiven källa är en blob-behållare eller virtuell katalog, tillämpas inte jokertecken. Om alternativet /S anges sedan AzCopy tolkar hello angivna filen mönster som ett blob-prefix. Om alternativet inte anges /S sedan AzCopy matchar hello filmönstret mot exakt blob-namn.

Om hello anges källan är en Azure-filresurs, måste du antingen ange hello exakt namnet på filen (t.ex. abc.txt) toocopy en enskild fil, eller ange alternativet /S toocopy alla filer i hello resursen rekursivt. Försöker toospecify både en fil mönstret och alternativet /S tillsammans resulterar i ett fel.

AzCopy använder skiftlägeskänsliga matchar när hello/Source är en blob-behållare eller blob virtuell katalog och använder skiftlägeskänsliga matchar alla hello andra fall.

hello filen Standardmönster som används när inga filmönstret anges är *.* för en plats för system eller ett tomt prefix för en Azure-lagringsplats. Ange flera filen mönster stöds inte.

**Gäller för:** Blobbar, filer

### <a name="destkeystorage-key"></a>/ DestKey: ”lagringsnyckel”
Anger hello lagringskontonyckel för hello målresurs.

**Gäller för:** Blobbar, filer, tabeller

### <a name="destsassas-token"></a>/ DestSAS: ”sas-token”
Anger en delad signatur åtkomst (SAS) med behörigheter för Läs- och skrivbehörighet för hello mål (om tillämpligt). Omge hello SAS med dubbla citattecken, eftersom det kanske innehåller kommandoradsverktyget specialtecken.

Du kan antingen ange det här alternativet följt av hello SAS-token om hello målresurs är ett blob-behållare, en filresurs eller en tabell, eller kan du ange hello SAS som en del av hello mål blob-behållare, filresurs eller tabellens URI, utan det här alternativet.

Om hello källa och mål är båda blobbar och sedan hello mål blob måste finnas inom hello samma lagringskonto som hello källa blob.

**Gäller för:** Blobbar, filer, tabeller

### <a name="sourcekeystorage-key"></a>/ SourceKey: ”lagringsnyckel”
Anger hello lagringskontonyckel för hello källa resurs.

**Gäller för:** Blobbar, filer, tabeller

### <a name="sourcesassas-token"></a>/ SourceSAS: ”sas-token”
Anger en signatur för delad åtkomst med läs- och behörigheter för hello källan (om tillämpligt). Omge hello SAS med dubbla citattecken, eftersom det kanske innehåller kommandoradsverktyget specialtecken.

Om varken en nyckel eller en SAS tillhandahålls hello källa resursen är en blob-behållare, läsa hello blob-behållare via anonym åtkomst.

Om hello källan är en filresurs eller tabell, en nyckel eller en SAS måste anges.

**Gäller för:** Blobbar, filer, tabeller

### <a name="s"></a>/ S
Anger rekursiv kopieringsåtgärd. AzCopy kopierar alla blobbar eller filer som matchar mönstret för hello angivna filen, inklusive de på undermappar i rekursiva läge.

**Gäller för:** Blobbar, filer

### <a name="blobtypeblock--page--append"></a>/ BlobType: ”block” | ”sidan” | ”Lägg till”
Anger om hello mål blob är en blockblobb, en sidblob eller en tilläggsblobb. Det här alternativet gäller bara när du laddar upp en blob. Annars genereras ett fel. Om hello målet är en blob och det här alternativet inte anges som standard skapas en blockblobb AzCopy.

**Gäller för:** Blobbar

### <a name="checkmd5"></a>/ CheckMD5
Beräknar en MD5-hash för hämtade data och verifierar att hello MD5-hash lagras i hello blob eller filens Content-MD5-egenskap stämmer med hello beräknade hash. hello MD5-kontroll är inaktiverad som standard, så du måste ange det här alternativet tooperform hello MD5 kontroll när du hämtar data.

Observera att Azure Storage inte garanterar att hello MD5-hash lagras för hello blob eller filen är uppdaterad. Det är klientens ansvar tooupdate hello MD5 när hello blob eller fil ändras.

AzCopy anger alltid hello Content-MD5-egenskapen för ett Azure blob eller en fil efter att överföra den toohello service.  

**Gäller för:** Blobbar, filer

### <a name="snapshot"></a>/ Ögonblicksbild
Anger om tootransfer ögonblicksbilder. Det här alternativet gäller endast om hello källan är en blob.

hello överförda blob ögonblicksbilder ändras i det här formatet: .extension blobbnamnet (snapshot-time)

Som standard kopieras inte ögonblicksbilder.

**Gäller för:** Blobbar

### <a name="vverbose-log-file"></a>/ V: [utförlig log-fil]
Utdata utförlig statusmeddelanden till en loggfil.

Som standard heter hello detaljerad loggfil AzCopyVerbose.log i `%LocalAppData%\Microsoft\Azure\AzCopy`. Om du anger en befintlig plats för det här alternativet är hello utförlig log läggs toothat fil.  

**Gäller för:** Blobbar, filer, tabeller

### <a name="zjournal-file-folder"></a>/ Z: [journal-filmapp]
Anger en mapp för att återuppta en åtgärd journalen.

AzCopy stöder alltid fortsätter om en åtgärd har avbrutits.

Om det här alternativet inte har angetts eller om den har angetts utan en mappsökväg, skapar AzCopy hello journal-fil i hello standardplatsen, som är % LocalAppData%\Microsoft\Azure\AzCopy.

Varje gång du utfärda ett kommando tooAzCopy kontrollerar den om en journal-fil finns i hello standardmappen eller om den finns i en mapp som du har angett via det här alternativet. Om hello journalfilen inte finns på någon plats, behandlar hello åtgärden som nya AzCopy och genererar en ny journalfil.

Om hello journalfilen finns kontrollerar AzCopy om hello-kommandoraden som du matar in matchar hello kommandoraden i hello journal-fil. Om hello två kommandorader matchar återupptar AzCopy hello ofullständiga igen. Om de inte matchar kan du ange tooeither överskrivning hello journal filen toostart en ny åtgärd eller toocancel hello aktuella åtgärden.

hello journalfilen tas bort vid slutförande av hello igen.

Observera att återuppta en åtgärd från en journal-fil som skapats av en tidigare version av AzCopy inte stöds.

**Gäller för:** Blobbar, filer, tabeller

### <a name="parameter-file"></a>/@:"parameter-File”
Anger en fil som innehåller parametrar. AzCopy processer hello parametrar i hello-filen som om de hade angetts på kommandoraden för hello.

I en svarsfil, kan du antingen ange flera parametrar på en enda rad eller ange varje parameter på en egen rad. Observera att en enskild parameter inte kan innehålla flera rader.

Svarsfiler kan innehålla kommentarer rader som börjar med hello # symbolen.

Du kan ange flera svarsfiler. Observera dock att AzCopy inte stöder kapslade svarsfiler.

**Gäller för:** Blobbar, filer, tabeller

### <a name="y"></a>/Y
Förhindrar att alla meddelanden för bekräftelse av AzCopy.

**Gäller för:** Blobbar, filer, tabeller

### <a name="l"></a>/ L
Anger en åtgärd. Inga data kopieras.

AzCopy tolkar hello använder för det här alternativet som en simulering för körs hello kommandoraden utan det här alternativet /L och antal kopieras hur många objekt, kan du ange alternativet /V på hello samma tid toocheck vilka objekt kopieras i hello utförlig loggen.

hello beteendet för det här alternativet beror också hello plats hello källdata och hello förekomst av hello rekursiv alternativet /S och filen mönster läge /Pattern.

AzCopy kräver behörighet att lista och läsa den här källplatsen när du använder det här alternativet.

**Gäller för:** Blobbar, filer

### <a name="mt"></a>/MT
Anger hello nedladdade filen modifierades senast toobe hello samma som hello källa blob eller filen.

**Gäller för:** Blobbar, filer

### <a name="xn"></a>/XN
Undantar en nyare käll-resurs. hello kopieras resurs inte om hello senast ändrad av hello källa är hello samma eller en senare än målet.

**Gäller för:** Blobbar, filer

### <a name="xo"></a>/XO
Undantar en äldre käll-resurs. hello kopieras resurs inte om hello senast ändrad av hello källa är hello samma eller äldre än målet.

**Gäller för:** Blobbar, filer

### <a name="a"></a>/A
Överför bara filer som har hello Arkiv-attributet inställt.

**Gäller för:** Blobbar, filer

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]
Överföringar endast filer med någon av hello angiven attribut.

Tillgängliga attribut inkluderar:

* R = skrivskyddade filer
* A = filer som är klara för arkivering
* S = systemfiler
* H = dolda filer
* C = komprimerade filer
* N = normala filer
* E = krypterade filer
* T = temporära filer
* O = Offline-filer
* Jag = icke-indexerat filer

**Gäller för:** Blobbar, filer

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]
Undantar filer med någon av hello angivna attribut uppsättningen.

Tillgängliga attribut inkluderar:

* R = skrivskyddade filer
* A = filer som är klara för arkivering
* S = systemfiler
* H = dolda filer
* C = komprimerade filer
* N = normala filer
* E = krypterade filer
* T = temporära filer
* O = Offline-filer
* Jag = icke-indexerat filer

**Gäller för:** Blobbar, filer

### <a name="delimiterdelimiter"></a>/ Avgränsare: ”avgränsare”
Anger hello avgränsningstecken används toodelimit virtuella kataloger i ett blob-namn.

Som standard använder AzCopy / som hello avgränsningstecken. AzCopy stöder dock använda alla vanliga tecken (t ex @, # eller %) som avgränsare. Om du behöver tooinclude något av dessa specialtecken på kommandoraden för hello omge hello filnamn med dubbla citattecken.

Det här alternativet gäller endast för att ladda ned blobbar.

**Gäller för:** Blobbar

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: ”många-för-samtidiga-operationer”
Anger hello antalet samtidiga åtgärder.

AzCopy som standard startar ett visst antal samtidiga åtgärder tooincrease hello data transfer genomflöde. Observera att många samtidiga åtgärder i en miljö med låg bandbredd kan överväldigande hello nätverksanslutning och förhindra hello operations fullständigt slutförs. Begränsning samtidiga åtgärder baserat på faktiska tillgänglig nätverksbandbredd.

hello övre gräns för samtidiga åtgärder är 512.

**Gäller för:** Blobbar, filer, tabeller

### <a name="sourcetypeblob--table"></a>/ SourceType: ”Blob” | ”Table”
Anger att hello `source` resursen är en blob som är tillgängliga i hello lokala utvecklingsmiljö körs i hello storage-emulatorn.

**Gäller för:** Blobbar, tabeller

### <a name="desttypeblob--table"></a>/ DestType: ”Blob” | ”Table”
Anger att hello `destination` resursen är en blob som är tillgängliga i hello lokala utvecklingsmiljö körs i hello storage-emulatorn.

**Gäller för:** Blobbar, tabeller

### <a name="pkrskey1key2key3"></a>/ PKRS ”: key&#1;key2 key&#3;...”
Delningar hello partition viktiga intervallet tooenable Exportera tabelldata parallellt, vilket ökar hello exportåtgärden hello hastighet.

Om det här alternativet inte anges används AzCopy tabellentiteter för tooexport en enkel tråd. Till exempel om hello användaren anger /PKRS: ”aa #bb” och sedan AzCopy startar tre samtidiga åtgärder.

Varje åtgärd exporterar en av tre partition nyckelintervall, enligt nedan:

  [första partitionsnyckel, aa)

  [aa, bb)

  [bb senaste partitionsnyckel]

**Gäller för:** tabeller

### <a name="splitsizefile-size"></a>/ SplitSize: ”filstorlek”
Anger hello exporterade filen dela storlek i MB hello tillåtna minimalt värde är 32.

Om det här alternativet inte anges, exporterar AzCopy tabell data tooa en fil.

Om hello tabelldata är exporterade tooa blob och hello exporterade filstorleken är hello 200 GB gränsen för blob-storlek, sedan delar AzCopy hello exporterade filen, även om det här alternativet inte har angetts.

**Gäller för:** tabeller

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: ”InsertOrSkip” | ”InsertOrMerge” | ”InsertOrReplace”
Anger hello Tabellfunktioner data import.

* InsertOrSkip - hoppar över en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.
* InsertOrMerge - sammanfogar en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.
* InsertOrReplace - ersätter en befintlig entitet eller infogar en ny entitet om den inte finns i hello tabell.

**Gäller för:** tabeller

### <a name="manifestmanifest-file"></a>/ Manifest: ”manifest-fil”
Anger hello manifestfilen för hello tabell exportera och importera igen.

Det här alternativet är valfritt under exportåtgärden hello, AzCopy genererar en manifestfil med fördefinierade namnet om det här alternativet inte anges.

Det här alternativet krävs under hello importen för att hitta hello datafiler.

**Gäller för:** tabeller

### <a name="synccopy"></a>/ SyncCopy
Anger om toosynchronously kopiera BLOB eller filer mellan två Azure Storage-slutpunkter.

AzCopy som standard använder serversidan asynkron kopia. Ange det här alternativet tooperform en synkron kopiera som laddar ned blobbar eller filer toolocal minne och överför dem tooAzure lagring.

Du kan använda det här alternativet när du kopierar filer i Blob storage fillagring, eller från Blob storage tooFile lagringsutrymme eller tvärtom.

**Gäller för:** Blobbar, filer

### <a name="setcontenttypecontent-type"></a>/ SetContentType: ”content-type”
Anger hello MIME content-type för mål-BLOB eller filer.

Uppsättningar av AzCopy hello content-type för en blob eller filen tooapplication/oktett-ström som standard. Du kan ange hello content-type för alla blobbar eller filer genom att ange ett värde för det här alternativet.

Om du anger det här alternativet utan ett värde som anger AzCopy varje blob eller filens innehållstyp enligt tooits filnamnstillägg.

**Gäller för:** Blobbar, filer

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: ”JSON” | ”CSV”
Anger hello formatet på filen med exporterade data hello tabell.

Om det här alternativet inte anges, exporterar AzCopy tabell datafilen i JSON-format som standard.

**Gäller för:** tabeller

## <a name="known-issues-and-best-practices"></a>Kända problem och bästa praxis
### <a name="limit-concurrent-writes-while-copying-data"></a>Begränsa samtidiga skrivningar vid kopiering av data
När du kopierar blobar eller filer med AzCopy, Tänk på att ett annat program kanske ändra hello data när du kopierar den. Kontrollera om det är möjligt att hello data som du kopierar inte ändras under hello kopieringen. Till exempel när du kopierar en virtuell Hårddisk som är kopplad till en virtuell Azure-dator, kontrollera att inga andra program för närvarande skriver toohello VHD. Ett bra sätt toodo denna genom leasing hello resurs toobe kopieras. Alternativt kan du skapa en ögonblicksbild av hello VHD först och sedan kopiera hello ögonblicksbild.

Om du inte hindra andra program från att skriva tooblobs eller filer när de kopieras och Kom ihåg att av hello hello jobbet har slutförts kan hello kopiera resurserna inte längre ha fullständig paritet med hello källa resurser.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Köra en AzCopy-instans på en dator.
AzCopy är utformad toomaximize hello användning av din dator resurs tooaccelerate hello dataöverföring, rekommenderar vi du kör endast en AzCopy-instans på en dator och ange hello alternativet `/NC` om du behöver fler samtidiga åtgärder. Mer information, Skriv `AzCopy /?:NC` hello-kommandoraden.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Aktivera FIPS-kompatibla MD5 algoritmer för AzCopy när du ”Använd FIPS-kompatibla algoritmer för kryptering, hashing och signering”.
AzCopy som standard använder .NET MD5 implementering toocalculate hello MD5 när du kopierar objekt, men det finns vissa säkerhetskrav som behöver AzCopy-tooenable FIPS kompatibla MD5-inställningen.

Du kan skapa en app.config-fil `AzCopy.exe.config` med egenskapen `AzureStorageUseV1MD5` och placera den reservera med AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

För egenskapen ”AzureStorageUseV1MD5” • True - använder hello standardvärdet, AzCopy .NET MD5-implementering.
• False – AzCopy använder FIPS-kompatibla MD5-algoritmen.

Observera att FIPS-kompatibla algoritmer är inaktiverad som standard på din Windows-dator, du kan skriva secpol.msc i din körningsfönstret och kontrollera den här växeln på Säkerhet -> lokala principer -> säkerhet alternativ -> Systemkryptografi: Använd FIPS-kompatibel algoritmer för kryptering, hashing och signering.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Storage och AzCopy finns hello följande resurser:

### <a name="azure-storage-documentation"></a>Azure Storage-dokumentation:
* [Introduktion tooAzure lagring](../storage-introduction.md)
* [Hur toouse Blob storage från .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Hur toouse File storage från .NET](../storage-dotnet-how-to-use-files.md)
* [Hur toouse Table storage från .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Hur toocreate, hantera eller ta bort ett lagringskonto](../storage-create-storage-account.md)
* [Överföra data med AzCopy på Linux](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage blogginlägg:
* [Introduktion till Azure Storage Data Movement Library Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Introduktion till synkron kopia och anpassade innehållstyp](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Om allmän tillgänglighet AzCopy 3.0 plus förhandsversionen av AzCopy 4.0 med tabell- och support](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Optimerade för storskaliga kopiera scenarier](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Stöd för geo-redundant lagring med läsbehörighet](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Överföra data med nytt starta läge och SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Använder mellan konto kopiera Blob](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Överför/hämta filer för Azure-BLOB](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

