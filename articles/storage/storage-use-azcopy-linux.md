---
title: "aaaCopy eller flytta data tooAzure lagring med AzCopy på Linux | Microsoft Docs"
description: "Använd hello AzCopy på Linux-verktyget toomove eller kopiera data tooor från blob- och innehåll. Kopiera data tooAzure lagring från lokala filer eller kopiera data inom eller mellan lagringskonton. Enkelt migrera dina data tooAzure lagring."
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
ms.date: 05/11/2017
ms.author: seguler
ms.openlocfilehash: dccb03c9e8cc3ea661494e7834f307b0e3e30cb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a>Överföra data med AzCopy på Linux
AzCopy på Linux är ett kommandoradsverktyg som utformats för att kopiera data tooand från Microsoft Azure-Blob och fillagring med enkla kommandon med optimal prestanda. Du kan kopiera data från en objektet tooanother inom ditt lagringskonto eller mellan lagringskonton.

Det finns två versioner av AzCopy som du kan hämta. AzCopy på Linux är byggd med .NET Core Framework, som riktar sig till Linux-plattformar som erbjuder POSIX format kommandoradsalternativ. [AzCopy på Windows](storage-use-azcopy.md) har skapats med .NET Framework och erbjuder kommandoradsalternativ för Windows-formatet. Den här artikeln beskriver AzCopy på Linux.

## <a name="download-and-install-azcopy"></a>Hämta och installera AzCopy
### <a name="installation-on-linux"></a>Installation på Linux

AzCopy på Linux kräver .NET Core framework på hello-plattformen. Se hello installationsinstruktioner på hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) sidan.

Exempelvis kan vi installera .NET Core på Ubuntu 16,10. Hello senaste installationsguiden finns [.NET Core på Linux](https://www.microsoft.com/net/core#linuxubuntu) installationssidan.


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

När du har installerat .NET Core, hämta och installera AzCopy.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

Du kan ta bort hello extraherade filer när AzCopy på Linux har installerats. Du kan också om du inte har superanvändare, kan du också köra AzCopy med hello kommandoskript 'azcopy' i hello extraherade mappen. 

### <a name="alternative-installation-on-ubuntu"></a>Alternativ Installation på Ubuntu

**Ubuntu 14.04**

Lägg till lgh källa för .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16.04**

Lägg till lgh källa för .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

**Ubuntu 16,10**

Lägg till lgh källa för .net Core:

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

Lägg till lgh källa för Microsoft Linux produkten lagringsplatsen och installera AzCopy:

```bash
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a>Skriva ditt första AzCopy-kommandot
grundläggande hello-syntaxen för AzCopy kommandon är:

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

hello som följande exempel visar olika scenarier för att kopiera data tooand från Microsoft Azure-BLOB och filer. Se toohello `azcopy --help` menyn för en detaljerad förklaring av hello parametrar som används i varje prov.

## <a name="blob-download"></a>BLOB: ladda ned
### <a name="download-single-blob"></a>Hämta en enda blob

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Om hello mappen `/mnt/myfiles` finns inte, AzCopy skapar den och hämtar `abc.txt ` till hello ny mapp.

### <a name="download-single-blob-from-secondary-region"></a>Hämta en enda blob från sekundär region

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.

### <a name="download-all-blobs"></a>Hämta alla BLOB

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Anta hello följande blobbar finns i angivna hello-behållaren:  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

Efter hello hämtningen hello directory `/mnt/myfiles` innehåller hello följande filer:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

Om du inte anger alternativet `--recursive`, ingen blob som ska hämtas.

### <a name="download-blobs-with-specified-prefix"></a>Ladda ned blobbar med angivna prefix

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

Anta hello följande blobbar finns i hello angivna behållaren. Alla blobbar som börjar med prefixet hello `a` hämtas.

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

Efter hello hämtningen hello mappen `/mnt/myfiles` innehåller hello följande filer:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

hello prefix gäller toohello virtuella katalogen, som utgör hello första delen av hello blob-namnet. I hello exemplet ovan matchar hello virtuella katalogen inte hello angivna prefix, så inga blob hämtas. Dessutom, om hello alternativet `--recursive` anges AzCopy inte hämta alla blobbar.

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a>Ange hello modifierades senast av exporterade filerna toobe samma som hello källa blobbar

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

Du kan också utesluta blobbar från hello hämtningen baserat på deras tid för senaste ändring. Till exempel om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller en senare än hello målfilen, lägga till hello `--exclude-newer` alternativ:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

Eller om du vill tooexclude blobbar vars senaste ändringstiden är hello samma eller äldre än hello målfilen, lägger du till hello `--exclude-older` alternativ:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a>BLOB: Överför
### <a name="upload-single-file"></a>Överför en fil

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Om hello angivna målbehållare inte finns, AzCopy skapar den och överföringar hello filen till den.

### <a name="upload-single-file-toovirtual-directory"></a>Överför en fil toovirtual directory

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

Om hello angivna virtuella katalogen inte finns, överför AzCopy hello filen tooinclude hello virtuell katalog på hello blobbnamnet (*t.ex.*, `vd/abc.txt` i hello-exemplet ovan).

### <a name="upload-all-files"></a>Ladda upp alla filer

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

Om du anger alternativet `--recursive` överföringar hello innehållet i hello angetts directory tooBlob lagring rekursivt, vilket innebär att alla undermappar och filer överförs också. Anta exempelvis att hello följande filer finns i mappen `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Efter hello överföringen innehåller hello behållaren hello följande filer:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

När hello alternativet `--recursive` anges endast hello följande tre filer överförs:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a>Överföra filer som matchar angivna mönstret

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

Anta hello följande filer finns i mappen `/mnt/myfiles`:

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

Efter hello överföringen innehåller hello behållaren hello följande filer:

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

När hello alternativet `--recursive` anges AzCopy hoppar över filer som finns i underkataloger:

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a>Ange hello MIME content-type för en mål-blob
Som standard AzCopy anger hello innehållstypen för en mål-blob för`application/octet-stream`. Du kan dock uttryckligen ange hello innehållstyp via hello alternativet `--set-content-type [content-type]`. Den här syntaxen anger hello content-type för alla blobbar i en överföringen.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

Om hello alternativet `--set-content-type` anges utan värde, och sedan AzCopy anger varje blob eller en fil vars innehållstyp enligt tooits filnamnstillägg.

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a>BLOB: kopiera
### <a name="copy-single-blob-within-storage-account"></a>Kopiera enda blob inom lagringskonto

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

När du kopierar en blobb utan--synkroniserad kopia alternativet en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopiera enda blob på Storage-konton

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

När du kopierar en blobb utan--synkroniserad kopia alternativet en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a>Kopiera enda blob från sekundär region tooprimary region

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

Observera att du måste ha läsbehörighet geo-redundant lagring aktiverad.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopiera enda blob och dess ögonblicksbilder på Storage-konton

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

Efter hello kopieringen innehåller hello Målbehållaren hello blob och dess ögonblicksbilder. hello behållaren omfattar hello följande blob och dess ögonblicksbilder:

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Kopiera synkront blobar på lagringskonton
AzCopy som standard kopierar data mellan två slutpunkter för lagring asynkront. Därför kopieras hello Kopiera åtgärden körs i hello bakgrunden med hjälp av ledig bandbreddskapacitet som har inga SLA vad gäller hur snabbt en blob. 

Hej `--sync-copy` alternativet ser du till att hello kopieringsåtgärden hämtar konsekvent hastighet. AzCopy utför hello synkron kopia genom att hämta hello blobbar toocopy från hello anges källa toolocal minne, och överföra dem toohello Blob-lagringsplats.

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

`--sync-copy`kan skapa ytterligare utgång kostnaden jämfört med tooasynchronous kopia. hello rekommenderade metoden är toouse det här alternativet i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.

## <a name="file-download"></a>Fil: ladda ned
### <a name="download-single-file"></a>Hämta en fil

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

Om hello anges källan är en Azure-filresurs, måste du antingen ange hello exakta filnamnet (*t.ex.* `abc.txt`) toodownload en enskild fil, eller ange alternativet `--recursive` toodownload alla filer i hello resurs rekursivt. Försök toospecify både filmönstret och alternativet `--recursive` tillsammans resulterar i ett fel.

### <a name="download-all-files"></a>Hämta alla filer

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

Observera att alla tomma mappar inte laddas ned.

## <a name="file-upload"></a>Fil: Överför
### <a name="upload-single-file"></a>Överför en fil

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a>Ladda upp alla filer

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

Observera att alla tomma mappar inte upp.

### <a name="upload-files-matching-specified-pattern"></a>Överföra filer som matchar angivna mönstret

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a>Fil: kopiera
### <a name="copy-across-file-shares"></a>Kopiera på filresurser

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
När du kopierar en fil på filresurser, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-from-file-share-tooblob"></a>Kopiera från filen resursen tooblob

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
När du kopierar en fil från filen resursen tooblob en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="copy-from-blob-toofile-share"></a>Kopiera från blob toofile resurs

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
När du kopierar en fil från blob toofile resurs, en [serversidan kopiera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) åtgärden utförs.

### <a name="synchronously-copy-files"></a>Synkront kopiera filer
Du kan ange hello `--sync-copy` alternativet toocopy data från fillagring tooFile lagring, File Storage tooBlob lagring och Blob Storage tooFile lagring synkront. AzCopy körs åtgärden genom att hämta hello källa data toolocal minne och överföra den toodestination. I det här fallet gäller standard utgång kostnaden.

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

När du kopierar från fillagring tooBlob lagring hello standard blob-datatyp är blockblob, kan användaren ange alternativet `/BlobType:page` toochange hello blob måltypen.

Observera att `--sync-copy` kan skapa ytterligare utgång kostnad jämför tooasynchronous kopia. hello rekommenderade metoden är toouse det här alternativet i en Azure VM i hello samma region som din datakälla konto tooavoid utgång lagringskostnaden.

## <a name="other-azcopy-features"></a>Andra AzCopy-funktioner
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a>Endast kopiera data som inte finns i hello mål
Hej `--exclude-older` och `--exclude-newer` parametrar kan du tooexclude äldre eller nyare resurser från att kopieras respektive. Om du bara vill toocopy källa resurser som inte finns i hello målet kan du ange båda parametrarna i hello AzCopy-kommandot:

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-toospecify-command-line-parameters"></a>Använd en fil toospecify kommandoradsverktyget konfigurationsparametrar

```azcopy
azcopy --config-file "azcopy-config.ini"
```

Du kan inkludera eventuella kommandoradsparametrar för AzCopy i en konfigurationsfil. AzCopy processer hello parametrar i hello-filen som om de hade angetts på kommandoraden för hello utför en direkt ersättning med hello innehållet i hello-fil.

Anta en fil med namnet `copyoperation`, som innehåller hello följande rader. Varje AzCopy-parameter kan anges på en enda rad.

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

eller på separata rader:

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

AzCopy misslyckas om du vill dela hello parametern på två rader som visas här för hello `--source-key` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a>Ange en signatur för delad åtkomst (SAS)

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

Du kan också ange en SAS för hello behållaren URI:

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

Observera att AzCopy för närvarande endast stöder hello [kontots SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).

### <a name="journal-file-folder"></a>Ändringsjournalen mapp
Varje gång du utfärda ett kommando tooAzCopy kontrollerar den om en journal-fil finns i hello standardmappen eller om den finns i en mapp som du har angett via det här alternativet. Om hello journalfilen inte finns på någon plats, behandlar hello åtgärden som nya AzCopy och genererar en ny journalfil.

Om hello journalfilen finns kontrollerar AzCopy om hello-kommandoraden som du matar in matchar hello kommandoraden i hello journal-fil. Om hello två kommandorader matchar återupptar AzCopy hello ofullständiga igen. Om de inte matchar uppmanas AzCopy användaren tooeither överskrivning hello journal filen toostart en ny åtgärd eller toocancel hello den aktuella åtgärden.

Om du vill toouse hello standardplatsen för hello journal-fil:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

Om du utelämnar alternativet `--resume`, eller ange alternativet `--resume` utan hello mappsökvägen, enligt ovan, AzCopy skapar hello journal-fil i hello standardplatsen, som är `~\Microsoft\Azure\AzCopy`. Om hello journalfilen redan finns återupptas AzCopy hello-åtgärden utifrån hello journal-fil.

Om du vill toospecify en egen placering för hello journal-fil:

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

Det här exemplet skapar hello journal-filen om den inte redan finns. Om den finns, återupptar AzCopy hello-åtgärden utifrån hello journal-fil.

Om du vill tooresume en AzCopy-åtgärd, upprepa hello samma kommando. AzCopy på Linux sedan uppmanas att bekräfta:

```azcopy
Incomplete operation with same command line detected at hello journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want tooresume hello operation? Choose Yes tooresume, choose No toooverwrite hello journal toostart a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a>Detaljerade loggarna för utdata

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a>Ange hello antalet samtidiga åtgärder toostart
Alternativet `--parallel-level` anger hello antalet samtidiga kopieringsåtgärd. Som standard startar AzCopy antalet samtidiga åtgärder tooincrease hello data transfer genomflöde. hello antalet samtidiga åtgärder är lika åtta gånger hello antal processorer som du har. Om du använder AzCopy över ett nätverk med låg bandbredd, kan du ange ett lägre värde--parallell nivå tooavoid misslyckades på grund av konkurrens om resurser.

[!TIP]
>fullständig lista över tooview hello av AzCopy-parametrar, kolla 'azcopy--Hjälp-menyn.

## <a name="known-issues-and-best-practices"></a>Kända problem och bästa praxis
### <a name="error-net-core-is-not-found-in-hello-system"></a>Fel: .NET Core finns inte i hello system.
Om du får ett felmeddelande om att .NET Core inte är installerat i systemet hello hello SÖKVÄGEN toohello .NET Core binär `dotnet` kanske saknas.

I ordning tooaddress problemet, hitta hello .NET Core binary i hello system:
```bash
sudo find / -name dotnet
```

Detta returnerar hello sökvägen toohello dotnet binär. 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

Nu ska du lägga till den här sökvägen toohello PATH-variabeln. För sudo, redigera secure_path toocontain hello sökvägen toohello dotnet binära:
```bash 
sudo visudo
### Append hello path found in hello preceding example too'secure_path' variable
```

I det här exemplet läser secure_path variabeln som:

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

Redigera.bash_profile/.profile tooinclude hello sökvägen toohello dotnet binära i PATH-variabeln för hello aktuell användare 
```bash
vi ~/.bash_profile
### Append hello path found in hello preceding example too'PATH' variable
```

Kontrollera att .NET Core finns i SÖKVÄGEN:
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a>Fel vid installation av AzCopy
Om du får problem med installation av AzCopy försök toorun AzCopy använda hello bash-skript på hello extraherade `azcopy` mapp.

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a>Begränsa samtidiga skrivningar vid kopiering av data
När du kopierar blobar eller filer med AzCopy, Tänk på att ett annat program kanske ändra hello data när du kopierar den. Kontrollera om det är möjligt att hello data som du kopierar inte ändras under hello kopieringen. Till exempel när du kopierar en virtuell Hårddisk som är kopplad till en virtuell Azure-dator, kontrollera att inga andra program för närvarande skriver toohello VHD. Ett bra sätt toodo denna genom leasing hello resurs toobe kopieras. Alternativt kan du skapa en ögonblicksbild av hello VHD först och sedan kopiera hello ögonblicksbild.

Om du inte hindra andra program från att skriva tooblobs eller filer när de kopieras och Kom ihåg att av hello hello jobbet har slutförts kan hello kopiera resurserna inte längre ha fullständig paritet med hello källa resurser.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Köra en AzCopy-instans på en dator.
AzCopy är utformad toomaximize hello användning av din dator resurs tooaccelerate hello dataöverföring, rekommenderar vi du kör endast en AzCopy-instans på en dator och ange hello alternativet `--parallel-level` om du behöver fler samtidiga åtgärder. Mer information, Skriv `AzCopy --help parallel-level` hello-kommandoraden.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Storage och AzCopy finns hello följande resurser:

### <a name="azure-storage-documentation"></a>Azure Storage-dokumentation:
* [Introduktion tooAzure lagring](storage-introduction.md)
* [Skapa ett lagringskonto](storage-create-storage-account.md)
* [Hantera blobbar med Lagringsutforskaren](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [Med Azure Storage hello Azure CLI 2.0](storage-azure-cli.md)
* [Hur toouse Blob storage från C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Hur toouse Blob storage från Java](storage-java-how-to-use-blob-storage.md)
* [Hur toouse Blob storage från Node.js](storage-nodejs-how-to-use-blob-storage.md)
* [Hur toouse Blob storage från Python](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage blogginlägg:
* [Om AzCopy på Linux-Preview](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [Introduktion till Azure Storage Data Movement Library Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Introduktion till synkron kopia och anpassade innehållstyp](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Om allmän tillgänglighet AzCopy 3.0 plus förhandsversionen av AzCopy 4.0 med tabell- och support](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Optimerade för storskaliga kopiera scenarier](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Stöd för geo-redundant lagring med läsbehörighet](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Överföra data med omstartsläge och SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Använder mellan konto kopiera Blob](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Överför/hämta filer för Azure-BLOB](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

