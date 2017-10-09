---
title: aaaAzure Import/Export manifestfilen format | Microsoft Docs
description: "Läs mer om hello format hello manifestfilen för enheten som beskriver hello mappning mellan blobbar i Azure Blob storage och filer på en enhet i ett import- eller jobb i hello Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f3119e1c-2c25-48ad-8752-a6ed4adadbb0
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: d7e5e1990482916f7ff5f891c97343b52e82b2f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure Import/Export service manifest filformat
hello enhet manifestfilen beskriver hello mappning mellan blobbar i Azure Blob storage och filer på enheten som består av ett jobb som importeras eller exporteras. För en importen hello manifestfilen skapas som en del av hello enhet förberedelseprocessen och lagras på hello enhet innan hello enhet skickas toohello Azure-datacenter. När du exporterar, hello manifestet skapas och lagras på hello enhet av hello Azure Import/Export service.  
  
För både import och exportjobb hello enhet manifestfilen lagras på hello importera eller exportera enhet; Det är inte överförs toohello tjänsten via API-åtgärd.  
  
hello nedan beskrivs hello formatet av en manifestfil på enheten:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveManifest Version="2014-11-01">  
  <Drive>  
    <DriveId>drive-id</DriveId>  
    import-export-credential  
  
    <!-- First Blob List -->  
    <BlobList>  
      <!-- Global properties and metadata that applies tooall blobs -->  
      [<MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>]  
      [<PropertiesPath   
        Hash="md5-hash">global-properties-file-path</PropertiesPath>]  
  
      <!-- First Blob -->  
      <Blob>  
        <BlobPath>blob-path-relative-to-account</BlobPath>  
        <FilePath>file-path-relative-to-transfer-disk</FilePath>  
        [<ClientData>client-data</ClientData>]  
        [<Snapshot>snapshot</Snapshot>]  
        <Length>content-length</Length>  
        [<ImportDisposition>import-disposition</ImportDisposition>]  
        page-range-list-or-block-list          
        [<MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>]  
        [<PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>]  
      </Blob>  
  
      <!-- Second Blob -->  
      <Blob>  
      . . .  
      </Blob>  
    </BlobList>  
  
    <!-- Second Blob List -->  
    <BlobList>  
    . . .  
    </BlobList>  
  </Drive>  
</DriveManifest>  
  
import-export-credential ::=   
  <StorageAccountKey>storage-account-key</StorageAccountKey> | <ContainerSas>container-sas</ContainerSas>  
  
page-range-list-or-block-list ::=   
  page-range-list | block-list  
  
page-range-list ::=   
    <PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
    </PageRangeList>  
  
block-list ::=  
    <BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       Hash="md5-hash"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       Hash="md5-hash"/>]  
    </BlockList>  

```

## <a name="manifest-xml-elements-and-attributes"></a>Manifestets XML-element och attribut

hello dataelement och attribut i hello enhet Manifestets XML-formatet har angetts i hello i den följande tabellen.  
  
|XML-Element|Typ|Beskrivning|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Rotelementet|hello rotelement i hello manifestfilen. Alla andra element i hello-filen är under det här elementet.|  
|`Version`|Attributet sträng|hello version av hello manifestfilen.|  
|`Drive`|Kapslade XML-element|Innehåller hello manifest för varje enhet.|  
|`DriveId`|Sträng|hello unika Enhetsidentifieraren hello-enheten. Hej Enhetsidentifieraren hittas genom att fråga hello enhet för dess serienummer. serienumret för hello enhet skrivs vanligtvis på hello utanför hello-enhet. Hej `DriveID` elementet måste finnas före alla `BlobList` element i hello manifestfilen.|  
|`StorageAccountKey`|Sträng|Krävs för import jobb om och bara om `ContainerSas` har inte angetts. Hej kontonyckel för hello Azure storage-konto som är associerade med hello jobb.<br /><br /> Det här elementet har utelämnats från hello manifest för exporten.|  
|`ContainerSas`|Sträng|Krävs för import jobb om och bara om `StorageAccountKey` har inte angetts. hello behållare SAS för att komma åt hello blobbar som är associerade med hello jobb. Se [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) för dess format. Det här elementet har utelämnats från hello manifest för exporten.|  
|`ClientCreator`|Sträng|Anger hello-klienten som skapade hello XML-fil. Det här värdet är inte tolkas av hello Import/Export service.|  
|`BlobList`|Kapslade XML-element|Innehåller en lista över blobbar som är en del av hello importera eller exportera jobb. Varje blobb i en blob-lista resurser hello samma metadata och egenskaper.|  
|`BlobList/MetadataPath`|Sträng|Valfri. Anger hello relativa sökvägen till en fil på hello-disk som innehåller hello standard metadata som ställs in på blobbar i hello blob-listan för en importen. Dessa metadata kan åsidosättas på grundval av blob av blob.<br /><br /> Det här elementet har utelämnats från hello manifest för exporten.|  
|`BlobList/MetadataPath/@Hash`|Attributet sträng|Anger hello Base16-kodade MD5 hash-värdet för hello metadatafil.|  
|`BlobList/PropertiesPath`|Sträng|Valfri. Anger hello relativa sökvägen till en fil på hello-disk som innehåller hello standardegenskaper som ställs in på blobbar i hello blob-listan för en importen. De här egenskaperna kan åsidosättas på grundval av blob av blob.<br /><br /> Det här elementet har utelämnats från hello manifest för exporten.|  
|`BlobList/PropertiesPath/@Hash`|Attributet sträng|Anger hello Base16-kodade MD5 hash-värde för hello egenskaper för filen.|  
|`Blob`|Kapslade XML-element|Innehåller information om varje blobb i varje blob-lista.|  
|`Blob/BlobPath`|Sträng|hello relativ URI toohello blob, från och med hello behållarnamn. Om hello blob finns i Rotbehållare, det måste börja med `$root`.|  
|`Blob/FilePath`|Sträng|Anger hello relativ sökväg toohello-hello enheten. För exportjobb kommer hello blobbsökvägen att användas för hello filsökväg om möjligt. *t.ex.*, `pictures/bob/wild/desert.jpg` exporteras för`\pictures\bob\wild\desert.jpg`. På grund av begränsningar i toohello av NTFS-namnen kan en blob emellertid exporterade tooa fil med en sökväg som inte liknar hello blob-sökväg.|  
|`Blob/ClientData`|Sträng|Valfri. Innehåller kommentarer från hello kund. Det här värdet är inte tolkas av hello Import/Export service.|  
|`Blob/Snapshot`|Datum och tid|Valfritt för exportjobb. Anger hello ögonblicksbild identifierare för en ögonblicksbild av exporterade blob.|  
|`Blob/Length`|Integer|Anger hello hello blob totala längd i byte. hello-värdet kan vara too200 GB för en blockblobb och in too1 TB för en sidblobb. Det här värdet måste vara delbar med 512 för en sidblobb.|  
|`Blob/ImportDisposition`|Sträng|Valfritt för importjobb utelämnas för exportjobb. Detta anger hur hello Import/Export service ska hantera hello fallet för ett importjobb där en blob med hello samma namn redan finns. Om det här värdet utelämnas från hello importera manifest hello standardvärdet är `rename`.<br /><br /> hello värden för det här elementet är:<br /><br /> -   `no-overwrite`: Om det finns redan en mål-blob med hello samma namn, hello importen ska hoppa över att importera den här filen.<br />-   `overwrite`: Alla befintliga mål-blob skrivs över helt av hello nyligen importerade filen.<br />-   `rename`: hello nya blob kommer upp med ett ändrat namn.<br /><br /> hello byta namn på regeln är följande:<br /><br /> -Om hello blob-namnet inte innehåller en punkt, skapas ett nytt namn genom att lägga till `(2)` toohello ursprungliga blob name; om det här nya namnet också står i konflikt med blobbnamnet på en befintlig sedan `(3)` läggs i stället för `(2)`; och så vidare.<br />-Om hello blob-namnet innehåller en punkt, anses hello del följande hello senaste punkt hello Tilläggsnamn. Liknande toohello ovan proceduren `(2)` införas före hello senaste punkt toogenerate ett nytt namn, om hello nytt namn är fortfarande i konflikt med ett befintligt blob-namn, sedan hello-tjänsten försöker `(3)`, `(4)`och så vidare tills en icke motstridig hitta namnet.<br /><br /> Några exempel:<br /><br /> hello blob `BlobNameWithoutDot` får namnet:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> hello blob `Seattle.jpg` får namnet:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|Kapslade XML-element|Krävs för en sidblobb.<br /><br /> För en import anger åtgärden, du en lista med byte-intervall för en fil toobe importeras. Varje sidintervall beskrivs av en förskjutning och längd i hello källfil som beskriver hello sidintervallet, tillsammans med en MD5-hash för hello region. Hej `Hash` attribut för ett sidintervall krävs. hello tjänsten verifierar att hello hashdata hello i hello blob matchar hello beräknad MD5-hash från hello sidintervallet. Valfritt antal sidintervall kanske används toodescribe en fil för en import med hello totala storleken upp too1 TB. Alla sidintervall måste vara sorterade efter förskjutningen och utan överlappningar tillåts.<br /><br /> Anger en uppsättning byteintervall av en blob som har exporterats toohello enhet för en export-åtgärd.<br /><br /> hello sidintervall kan tillsammans omfatta endast underintervall för en blob eller fil.  hello resten av hello-fil som inte omfattas av några sidintervall är förväntat och dess innehåll kan vara odefinierat.|  
|`PageRange`|XML-element|Representerar ett sidintervall.|  
|`PageRange/@Offset`|Attributet heltal|Anger hello förskjutningen i hello överföring fil- och hello blob där hello angivna sidintervallet börjar. Det här värdet måste vara delbar med 512.|  
|`PageRange/@Length`|Attributet heltal|Anger hello hello sidintervallet. Det här värdet måste vara delbar med 512 och högst 4 MB.|  
|`PageRange/@Hash`|Attributet sträng|Anger hello Base16-kodade MD5 hash-värdet för hello sidintervallet.|  
|`BlockList`|Kapslade XML-element|Krävs för en blockblobb med namngivna block.<br /><br /> En importen anger hello Blockeringslista en uppsättning block som ska importeras till Azure Storage. Hello Blockeringslista anger en exportåtgärden, där varje block har lagrats i hello filen hello export. Varje block beskrivs av en förskjutning i hello fil- och Blockets längd; varje block dessutom namnges av ett block-ID-attribut och innehåller en MD5-hash för hello-block. Konfigurera too50 kanske 000 block används toodescribe en blob.  Alla block måste beställas av Förskjutning, tillsammans med bör omfatta hello fullständig uppsättning hello filen *d.v.s.*, det måste finnas inget mellanrum mellan block. Om hello blob är mer än 64 MB, hello block-ID: N för varje block måste vara alla saknas eller alla finns. Block-ID: N är obligatoriska toobe Base64-kodade strängar. Se [placera Block](/rest/api/storageservices/put-block) ytterligare krav för block-ID: N.|  
|`Block`|XML-element|Representerar ett block.|  
|`Block/@Offset`|Attributet heltal|Anger hello förskjutningen där hello angivet block börjar.|  
|`Block/@Length`|Attributet heltal|Anger hello antalet byte i hello-block. Det här värdet måste vara fler än 4MB.|  
|`Block/@Id`|Attributet sträng|Anger en sträng som representerar hello block-ID för hello-block.|  
|`Block/@Hash`|Attributet sträng|Anger hello Base16-kodade MD5-hash för hello-block.|  
|`Blob/MetadataPath`|Sträng|Valfri. Anger hello relativ sökväg för en metadatafil. Under en import är hello metadata inställd på hello mål-blob. När du exporterar lagras hello blob metadata i hello metadatafil på hello enhet.|  
|`Blob/MetadataPath/@Hash`|Attributet sträng|Anger hello Base16-kodade MD5-hash för hello blob-metadatafil.|  
|`Blob/PropertiesPath`|Sträng|Valfri. Anger hello relativa sökvägen till en egenskapsfil. Under en import anges hello egenskaper på hello mål-blob. När du exporterar lagras hello blob egenskaper i hello egenskaper-hello enheten.|  
|`Blob/PropertiesPath/@Hash`|Attributet sträng|Anger hello Base16-kodade MD5-hash för hello blob egenskaper-fil.|  
  
## <a name="next-steps"></a>Nästa steg
 
* [REST-API för Storage Import/Export](/rest/api/storageimportexport/)
