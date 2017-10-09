---
title: aaaRepairing ett Azure Import/Export-importjobb - v1 | Microsoft Docs
description: "Lär dig hur toorepair ett importjobb som har skapats och att köras med hello Azure Import/Export service."
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
ms.openlocfilehash: b1cb7d7832276a05c0912cf57505e2a5d79e7846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-import-job"></a>Reparera ett importjobb
hello Microsoft Azure Import/Export-tjänsten kanske inte toocopy vissa av dina filer eller delar av en fil toohello Windows Azure Blob-tjänsten. Vissa fel anledningar:  
  
-   Skadade filer  
  
-   Skadade enheter  
  
-   Hej lagringskontonyckel ändras medan hello fil överfördes.  
  
Du kan köra hello Microsoft Azure Import/Export-verktyget med hello importera jobbet Kopiera loggen filer, och hello verktyget överföringar hello saknade filer (eller delar av en fil) tooyour Windows Azure storage-konto toocomplete hello importjobbet.  
  
## <a name="repairimport-parameters"></a>RepairImport parametrar

hello följande parametrar kan anges med **RepairImport**: 
  
|||  
|-|-|  
|**/ r:**< RepairFile\>|**Krävs.** Sökvägen toohello reparera filen som spårar hello fortskrider hello reparation och låter dig tooresume en avbruten reparation. Varje enhet måste ha en repair-fil. När du startar en reparation för en viss enhet kan skicka in hello sökvägen tooa repair-fil, som ännu inte finns. tooresume en avbruten reparation, överför du i hello namnet på en befintlig fil för reparation. hello reparera filen-målenheten med motsvarande toohello måste alltid anges.|  
|**/logdir:**< LogDirectory\>|**Valfritt.** hello loggkatalogen. Utförlig loggfilerna skrivs toothis directory. Om inga loggkatalogen anges används hello katalogen som hello loggkatalogen.|  
|**/ d:**< TargetDirectories\>|**Krävs.** En eller flera semikolonavgränsad kataloger som innehåller hello ursprungliga filer som har importerats. hello importera enheten kan också användas, men är inte nödvändigt om alternativa platser för ursprungliga filerna är tillgängliga.|  
|**/BK:**< BitLockerKey\>|**Valfritt.** Du bör ange hello BitLocker-nyckel om du vill att hello verktyget toounlock en krypterad enhet där hello ursprungliga filerna är tillgängliga.|  
|**/SN:**< StorageAccountName\>|**Krävs.** hello namnet på hello storage-konto för hello importera jobbet.|  
|**/Sk:**< StorageAccountKey\>|**Krävs** endast om en behållare SAS inte har angetts. Hej kontonyckel hello storage-konto för hello importera jobbet.|  
|**/csas:**< ContainerSas\>|**Krävs** om hello lagringskontonyckel inte har angetts. hello behållare SAS för att komma åt hello blobbar som är associerade med hello importjobbet.|  
|**/ CopyLogFile:**< DriveCopyLogFile\>|**Krävs.** Sökvägen toohello enhet kopiera loggfilen (utförlig loggen eller felloggen). hello filen har genererats av hello Windows Azure Import/Export service och kan hämtas från hello blob storage som är associerade med hello jobb. hello kopiera loggfilen innehåller information om misslyckade blobbar eller filer, vilket är toobe repareras.|  
|**/ PathMapFile:**< DrivePathMapFile\>|**Valfritt.** Sökvägen tooa textfil som kan användas för tooresolve tvetydigheter om du har flera filer med samma namn som du importerar i hello hello samma jobb. hello första gången hello verktyget körs, den kan fylla i den här filen med alla hello tvetydiga namn. Efterföljande körningar av hello verktyget använder den här filen tooresolve hello tvetydigheter.|  
  
## <a name="using-hello-repairimport-command"></a>Hej kommandot RepairImport  
toorepair importera data av strömmande hello data över hello nätverk måste du ange hello kataloger som innehåller ursprungliga hello filerna som du importerar med hello `/d` parameter. Du måste också ange hello kopiera loggfilen som du hämtade från ditt lagringskonto. En typisk kommandoraden toorepair ett importjobb med partiellt fel ser ut:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
Följande exempel på en kopia loggfil en 64-K del av en fil är skadad på hello-enhet som har levererats för hello importjobb i hello. Eftersom det är hello endast fel anges hello resten av hello blobbar i hello-jobbet har importerats.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
När den här kopiera loggen skickas toohello Azure Import/Export-verktyget, försöker hello verktyget toofinish hello import för filen genom hello saknas innehållet över hello nätverk. Följande hello-exemplet ovan, hello verktyget söker efter hello originalfilen `\animals\koala.jpg` i hello två kataloger `C:\Users\bob\Pictures` och `X:\BobBackup\photos`. Om hello filen `C:\Users\bob\Pictures\animals\koala.jpg` finns hello Azure Import/Export verktyget kopior hello saknas mängd data toohello motsvarande blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>Lösa konflikter när du använder RepairImport  
I vissa situationer hello kanske inte kan toofind eller öppna hello nödvändig fil för en av följande orsaker hello: hello filen kunde inte hittas, hello-filen är inte tillgänglig, hello filnamn är tvetydigt eller hello innehållet hello-filen är inte korrekta längre.  
  
Ett tvetydigt fel kan inträffa om hello verktyget försöker toolocate `\animals\koala.jpg` och det finns en fil med detta namn under både `C:\Users\bob\pictures` och `X:\BobBackup\photos`. Det vill säga både `C:\Users\bob\pictures\animals\koala.jpg` och `X:\BobBackup\photos\animals\koala.jpg` finns på hello importera jobbet enheter.  
  
Hej `/PathMapFile` alternativet kan du tooresolve felen. Du kan ange hello namnet på hello-filen som innehåller hello lista över filer som hello verktyget inte kunde identifiera toocorrectly. hello följande kommandorad exempel fyller `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
hello verktyget kommer sedan att skriva hello problematiska sökvägar för`9WM35C2V_pathmap.txt`, ett på varje rad. Hello filen kan exempelvis innehålla hello följande poster när du har kört kommandot hello:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 För varje fil i listan hello bör du försöka toolocate och öppna hello filen tooensure det är tillgängligt toohello verktyg. Om du inte vill tootell hello verktyget uttryckligen där toofind en fil som du kan ändra hello sökväg mappa filen och Lägg till fil vid hello-tooeach på hello samma rad, avgränsade med semikolon fliken:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Efter att hello nödvändiga filer tillgängliga toohello verktyg och uppdaterar hello sökväg mappningsfilen, kan du köra hello verktyget toocomplete hello importen.  
  
## <a name="next-steps"></a>Nästa steg
 
* [Ställa in hello Azure Import/Export-verktyget](storage-import-export-tool-setup-v1.md)   
* [Förbereda hårddiskar för ett importjobb](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Granska jobbstatus med kopiera loggfiler](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Reparera ett exportjobb](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Felsöka hello Azure Import/Export-verktyget](storage-import-export-tool-troubleshooting-v1.md)
