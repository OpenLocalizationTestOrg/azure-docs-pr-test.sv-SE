---
title: "loggfilsformat för aaaAzure Import/Export | Microsoft Docs"
description: "Läs mer om hello format hello loggfilerna som skapades när åtgärder utförs för ett jobb med Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 15a652455aa947922af0aa39ccefe68811a3db19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-log-file-format"></a>Azure Import/Export service loggfilsformat
När hello Microsoft Azure Import/Export service utför en åtgärd på en enhet som en del av ett importjobb eller ett exportjobb skrivs loggarna tooblock blobbar i hello storage-konto som är associerade med jobbet.  
  
Det finns två loggar som kan skrivas av hello Import/Export service:  
  
-   hello felloggen skapas alltid i hello händelse av ett fel.  
  
-   hello utförlig loggen är inte aktiverad som standard, men kan aktiveras genom att ange hello `EnableVerboseLog` -egenskapen i en [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) igen.  
  
## <a name="log-file-location"></a>Plats för loggfilen  
hello loggarna skrivs tooblock blobbar i behållaren hello eller virtuella katalogen som anges av hello `ImportExportStatesPath` inställning som du kan ange för en `Put Job` igen. hello plats toowhich hello loggarna skrivs beror på hur autentisering har angetts för hello jobb, tillsammans med hello-värde som angetts för `ImportExportStatesPath`. Autentisering för hello jobb kan anges via en lagringskontonyckel eller en behållare SAS (signatur för delad åtkomst).  
  
hello namnet på behållaren hello eller virtuell katalog kan antingen vara hello standardnamnet `waimportexport`, eller en annan behållare eller en virtuell katalog som du anger.  
  
hello tabellen nedan visar hello möjliga alternativ:  
  
|Autentiseringsmetod|Värdet för `ImportExportStatesPath`Element|Plats för logg-BLOB|  
|---------------------------|----------------------------------------------|---------------------------|  
|Lagringskontonyckel|Standardvärde|En behållare med namnet `waimportexport`, vilket är hello standardbehållaren. Exempel:<br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|Lagringskontonyckel|Värdet som angetts av användaren|En behållare med namnet av hello användare. Exempel:<br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|Behållaren SAS|Standardvärde|En virtuell katalog med namnet `waimportexport`, vilket är hello för standardnamnet under hello-behållaren som angetts i hello SAS.<br /><br /> Till exempel om hello SAS för hello jobbet är `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, hello loggplatsen skulle vara`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`|  
|Behållaren SAS|Värdet som angetts av användaren|En virtuell katalog med namnet av hello användare under hello-behållaren som angetts i hello SAS.<br /><br /> Till exempel om hello SAS för hello jobbet är `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, och hello angivna virtuella katalogen heter `mylogblobs`, hello loggplatsen skulle vara `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.|  
  
Du kan hämta hello URL: en för hello fel och utförlig loggar genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen. hello loggar är tillgängliga när bearbetningen av hello enheten är klar.  
  
## <a name="log-file-format"></a>Loggfilsformat  
hello format för båda loggarna är hello samma: en blob som innehåller XML-beskrivningar av hello händelser som inträffade vid kopiering av BLOB mellan hello hårddisken och hello kundens konto.  
  
hello utförlig loggen innehåller fullständig information om status för hello hello kopieringsåtgärden för varje blob (för ett importjobb) eller filen (för ett exportjobb), hello felloggen innehåller endast hello information för blobbar eller filer som påträffade fel under hello Importera eller exportera jobb.  
  
hello utförlig loggformatet visas nedan. hello felloggen har hello samma struktur, men filtrerar ut lyckade åtgärder.  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

hello beskrivs följande tabell hello element i hello loggfilen.  
  
|XML-Element|Typ|Beskrivning|  
|-----------------|----------|-----------------|  
|`DriveLog`|XML-Element|Representerar en logg för enheten.|  
|`Version`|Attributet sträng|hello version av hello loggformat.|  
|`DriveId`|Sträng|Hej enhetens serienummer för maskinvara.|  
|`Status`|Sträng|Status för hello enhet bearbetning. Se hello `Drive Status Codes` tabellen nedan för mer information.|  
|`Blob`|Kapslade XML-element|Representerar en blob.|  
|`Blob/BlobPath`|Sträng|hello hello blob-URI.|  
|`Blob/FilePath`|Sträng|hello relativ sökväg toohello-hello enheten.|  
|`Blob/Snapshot`|Datum och tid|hello ögonblicksbild version av hello blob för ett exportjobb endast.|  
|`Blob/Length`|Integer|hello totala längden för hello blob i byte.|  
|`Blob/LastModified`|Datum och tid|hello tidsvärdet hello blobben senast ändrades för ett exportjobb endast.|  
|`Blob/ImportDisposition`|Sträng|hello importera disposition av hello blob för en importjobb.|  
|`Blob/ImportDisposition/@Status`|Attributet sträng|hello status för hello importera disposition.|  
|`PageRangeList`|Kapslade XML-element|Representerar en lista över sidintervall för en sidblobb.|  
|`PageRange`|XML-element|Representerar ett sidintervall.|  
|`PageRange/@Offset`|Attributet heltal|Startar förskjutningen för hello sidintervall i hello-blob.|  
|`PageRange/@Length`|Attributet heltal|Längden i byte på hello sidintervallet.|  
|`PageRange/@Hash`|Attributet sträng|Base16-kodade MD5-hash för hello sidintervallet.|  
|`PageRange/@Status`|Attributet sträng|Status för bearbetning av hello sidintervallet.|  
|`BlockList`|Kapslade XML-element|Representerar en lista över block för en blockblob.|  
|`Block`|XML-element|Representerar ett block.|  
|`Block/@Offset`|Attributet heltal|Första förskjutningen av hello block i hello-blob.|  
|`Block/@Length`|Attributet heltal|Längden i byte på hello-block.|  
|`Block/@Id`|Attributet sträng|hello block-ID.|  
|`Block/@Hash`|Attributet sträng|Base16-kodade MD5-hash för hello-block.|  
|`Block/@Status`|Attributet sträng|Status för bearbetning av hello-block.|  
|`Metadata`|Kapslade XML-element|Representerar metadata för hello blob.|  
|`Metadata/@Status`|Attributet sträng|Status för bearbetning av hello blobbmetadata.|  
|`Metadata/GlobalPath`|Sträng|Relativ sökväg toohello globala metadatafil.|  
|`Metadata/GlobalPath/@Hash`|Attributet sträng|Base16-kodade MD5-hash för hello globala metadatafil.|  
|`Metadata/Path`|Sträng|Relativ sökväg toohello metadatafil.|  
|`Metadata/Path/@Hash`|Attributet sträng|Base16-kodade MD5-hash för hello metadatafil.|  
|`Properties`|Kapslade XML-element|Representerar hello blob-egenskaper.|  
|`Properties/@Status`|Attributet sträng|Status för bearbetning av hello blob-egenskaper, t.ex. inte att hitta filen, slutförts.|  
|`Properties/GlobalPath`|Sträng|Relativ sökväg toohello globala egenskaper för filen.|  
|`Properties/GlobalPath/@Hash`|Attributet sträng|Base16-kodade MD5-hash för hello globala egenskaper för filen.|  
|`Properties/Path`|Sträng|Relativ sökväg toohello egenskaper för filen.|  
|`Properties/Path/@Hash`|Attributet sträng|Base16-kodade MD5-hash för hello egenskapsfil.|  
|`Blob/Status`|Sträng|Status för bearbetning av hello-blob.|  
  
# <a name="drive-status-codes"></a>Statuskoder för enhet  
hello visar följande tabell hello statuskoder för bearbetning av en enhet.  
  
|Statuskod|Beskrivning|  
|-----------------|-----------------|  
|`Completed`|hello enhet har bearbetat utan fel.|  
|`CompletedWithWarnings`|hello enheten har slutförts med varningar i en eller flera blobbar per hello importera disposition som angetts för hello-BLOB.|  
|`CompletedWithErrors`|hello enheten har slutförts med fel i en eller flera blobbar segment.|  
|`DiskNotFound`|Ingen disk finns på hello enhet.|  
|`VolumeNotNtfs`|hello första datavolym på hello disk är inte i NTFS-format.|  
|`DiskOperationFailed`|Ett okänt fel uppstod när du utför åtgärder på hello enhet.|  
|`BitLockerVolumeNotFound`|Ingen encryptable volym BitLocker finns.|  
|`BitLockerNotActivated`|BitLocker har inte aktiverats på hello volym.|  
|`BitLockerProtectorNotFound`|nyckelskydd för hello numeriskt lösenord finns inte på hello volym.|  
|`BitLockerKeyInvalid`|hello numeriskt lösenord angavs inte låsa upp hello volym.|  
|`BitLockerUnlockVolumeFailed`|Okänt fel uppstod vid försök toounlock hello volym.|  
|`BitLockerFailed`|Ett okänt fel uppstod vid BitLocker-åtgärder.|  
|`ManifestNameInvalid`|hello Manifestfilens filnamn är ogiltigt.|  
|`ManifestNameTooLong`|hello Manifestfilens filnamn är för långt.|  
|`ManifestNotFound`|hello manifestfilen hittades inte.|  
|`ManifestAccessDenied`|Åtkomst toohello manifestfilen nekas.|  
|`ManifestCorrupted`|hello manifest-filen är skadad (hello innehållet matchar inte dess hash).|  
|`ManifestFormatInvalid`|Manifestet hello-innehåll följer inte toohello format som krävs.|  
|`ManifestDriveIdMismatch`|hello enheten inte matchar ID i hello manifestfilen hello en lästes från hello enhet.|  
|`ReadManifestFailed`|En disk-i/o-fel uppstod vid läsning från hello manifest.|  
|`BlobListFormatInvalid`|hello export blob listan blob följer inte toohello format som krävs.|  
|`BlobRequestForbidden`|Få åtkomst till toohello blobbar i hello storage-konto är förbjuden. Detta kan bero på att tooinvalid lagringskontonyckel eller SAS-behållare.|  
|`InternalError`|Och internt fel uppstod under bearbetning av hello enhet.|  
  
## <a name="blob-status-codes"></a>BLOB-statuskoder  
hello visar följande tabell hello statuskoder för bearbetning av en blob.  
  
|Statuskod|Beskrivning|  
|-----------------|-----------------|  
|`Completed`|hello-blob har bearbetat utan fel.|  
|`CompletedWithErrors`|hello-blob har bearbetat med fel i en eller flera sidintervall block, metadata eller egenskaper.|  
|`FileNameInvalid`|hello-filnamnet är ogiltigt.|  
|`FileNameTooLong`|hello-filnamnet är för långt.|  
|`FileNotFound`|hello-filen hittades inte.|  
|`FileAccessDenied`|Åtkomst toohello filen nekades.|  
|`BlobRequestFailed`|Det gick inte att hello Blob tjänstbegäran tooaccess hello blob.|  
|`BlobRequestForbidden`|hello Blob tjänstbegäran tooaccess hello blob tillåts inte. Detta kan bero på att tooinvalid lagringskontonyckel eller SAS-behållare.|  
|`RenameFailed`|Det gick inte toorename hello blob (för ett importjobb) eller hello-filen (för ett exportjobb).|  
|`BlobUnexpectedChange`|En oväntad ändring har uppstått med hello blob (för ett exportjobb).|  
|`LeasePresent`|Det finns ett lån som finns på hello-blob.|  
|`IOFailed`|En disk eller nätverk i/o-fel uppstod under bearbetning av hello blob.|  
|`Failed`|Ett okänt fel uppstod under bearbetning av hello-blob.|  
  
## <a name="import-disposition-status-codes"></a>Importera disposition statuskoder  
hello visar följande tabell hello statuskoder för att lösa ett import-disposition.  
  
|Statuskod|Beskrivning|  
|-----------------|-----------------|  
|`Created`|hello-blob har skapats.|  
|`Renamed`|hello-blob har bytt per Byt namn på import disposition. Hej `Blob/BlobPath` elementet innehåller hello URI för hello byta namn på blob.|  
|`Skipped`|hello-blob har hoppats över `no-overwrite` importera disposition.|  
|`Overwritten`|hello-blob har skrivit över en befintlig blob per `overwrite` importera disposition.|  
|`Cancelled`|Ett tidigare fel har stoppats vidare bearbetning av hello importera disposition.|  
  
## <a name="page-rangeblock-status-codes"></a>Statuskoder för sidan intervall /-block  
hello visar följande tabell hello statuskoder för bearbetning av ett sidintervall eller ett block.  
  
|Statuskod|Beskrivning|  
|-----------------|-----------------|  
|`Completed`|hello sidan intervall eller block har bearbetat utan fel.|  
|`Committed`|hello block som har allokerats, men inte i hello fullständig Blockeringslista eftersom andra block har misslyckats eller placera fullständig Blockeringslista själva har misslyckats.|  
|`Uncommitted`|hello block överförs men inte allokerad.|  
|`Corrupted`|hello sidan intervall eller block är skadad (hello innehållet matchar inte dess hash).|  
|`FileUnexpectedEnd`|Ett oväntat filslut påträffades.|  
|`BlobUnexpectedEnd`|Det inträffade ett oväntat slut på blob.|  
|`BlobRequestFailed`|Hej Blob tjänstbegäran tooaccess hello sidintervall eller blockera misslyckades.|  
|`IOFailed`|En disk eller nätverk i/o-fel inträffade vid bearbetning hello sidan intervall eller block.|  
|`Failed`|Ett okänt fel uppstod under bearbetning av hello sidan intervall eller block.|  
|`Cancelled`|Ett tidigare fel har stoppats vidare bearbetning av hello sidan intervall eller block.|  
  
## <a name="metadata-status-codes"></a>Statuskoder för metadata  
hello visar följande tabell hello statuskoder för bearbetning blob-metadata.  
  
|Statuskod|Beskrivning|  
|-----------------|-----------------|  
|`Completed`|hello metadata har bearbetat utan fel.|  
|`FileNameInvalid`|hello metadatafilnamnet är ogiltigt.|  
|`FileNameTooLong`|hello metadata-filnamnet är för långt.|  
|`FileNotFound`|hello metadatafil hittades inte.|  
|`FileAccessDenied`|Metadatafil toohello för åtkomst nekas.|  
|`Corrupted`|hello metadatafil är skadad (hello innehållet matchar inte dess hash).|  
|`XmlReadFailed`|hello metadata innehåll följer inte toohello format som krävs.|  
|`XmlWriteFailed`|Skriva hello metadata XML misslyckades.|  
|`BlobRequestFailed`|Det gick inte att hello Blob tjänstbegäran tooaccess hello metadata.|  
|`IOFailed`|Ett disk- eller i/o-fel uppstod under bearbetning av hello metadata.|  
|`Failed`|Ett okänt fel uppstod under bearbetning av hello metadata.|  
|`Cancelled`|Ett tidigare fel har stoppats vidare bearbetning av hello metadata.|  
  
## <a name="properties-status-codes"></a>Statuskoder för egenskaper  
hello visar följande tabell hello statuskoder för bearbetning av blob-egenskaper.  
  
|Statuskod|Beskrivning|  
|-----------------|-----------------|  
|`Completed`|hello egenskaper har slutfört bearbetningen utan fel.|  
|`FileNameInvalid`|hello egenskaper filnamnet är ogiltigt.|  
|`FileNameTooLong`|hello egenskaper filnamnet är för långt.|  
|`FileNotFound`|hello egenskaper filen hittades inte.|  
|`FileAccessDenied`|Åtkomst till toohello egenskapsfil nekas.|  
|`Corrupted`|hello egenskaper för filen är skadad (hello innehållet matchar inte dess hash).|  
|`XmlReadFailed`|hello egenskaper innehåll följer inte toohello format som krävs.|  
|`XmlWriteFailed`|Skriver hello egenskaper XML misslyckades.|  
|`BlobRequestFailed`|Det gick inte att hello Blob tjänstbegäran tooaccess hello egenskaper.|  
|`IOFailed`|Ett disk- eller i/o-fel uppstod under bearbetning av hello egenskaper.|  
|`Failed`|Ett okänt fel uppstod under bearbetning av hello egenskaper.|  
|`Cancelled`|Ett tidigare fel har stoppats vidare bearbetning av hello egenskaper.|  
  
## <a name="sample-logs"></a>Exempel loggar  
hello följande är ett exempel på detaljerade loggen.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
hello motsvarande felloggen visas nedan.  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 hello Följ felloggen för importen innehåller ett fel om en fil inte hittas på hello importera enhet. Observera att hello status för efterföljande komponenter är `Cancelled`.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

hello visar följande felloggen för ett exportjobb att hello blob-innehåll har skrivits toohello enheten utan att ett fel inträffade vid export av hello blob-egenskaper.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a>Nästa steg
 
* [REST-API för Storage Import/Export](/rest/api/storageimportexport/)
