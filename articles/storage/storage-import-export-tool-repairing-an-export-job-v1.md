---
title: aaaRepairing ett Azure Import/Export-exportjobb - v1 | Microsoft Docs
description: "Lär dig hur toorepair ett exportjobb som har skapats och att köras med hello Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 96c674fc7c697c37882fb2980c340303896ac6c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-export-job"></a>Reparera ett exportjobb
När ett exportjobb har slutförts kan köra du hello Microsoft Azure Import/Export-verktyget lokalt till:  
  
1.  Hämta filer att hello Azure Import/Export-tjänsten kunde tooexport.  
  
2.  Verifiera att hello filer på hello enhet exporterats korrekt.  
  
Du måste ha anslutningen tooAzure lagring toouse den här funktionen.  
  
hello-kommandot för att reparera ett importjobb **RepairExport**.

## <a name="repairexport-parameters"></a>RepairExport parametrar

hello följande parametrar kan anges med **RepairExport**:  
  
|Parameter|Beskrivning|  
|---------------|-----------------|  
|**/ r: < RepairFile\>**|Krävs. Sökvägen toohello reparera filen som spårar hello fortskrider hello reparation och låter dig tooresume en avbruten reparation. Varje enhet måste ha en repair-fil. När du startar en reparation för en viss enhet skickar hello sökvägen tooa reparera filen inte finns ännu. tooresume en avbruten reparation, överför du i hello namnet på en befintlig fil för reparation. hello reparera filen-målenheten med motsvarande toohello måste alltid anges.|  
|**/logdir: < LogDirectory\>**|Valfri. hello loggkatalogen. Utförlig loggfilerna skrivs toothis directory. Om inga loggkatalogen anges används hello katalogen som hello loggkatalogen.|  
|**/ d: < TargetDirectory\>**|Krävs. hello directory toovalidate och reparation. Detta är vanligtvis hello rotkatalogen hello export enheten, men kan också vara en nätverksfilresurs med en kopia av hello exporterade filerna.|  
|**/BK: < BitLockerKey\>**|Valfri. Du bör ange hello BitLocker-nyckel om du vill att hello verktyget toounlock krypterade där hello exporterade filer lagras.|  
|**/SN: < StorageAccountName\>**|Krävs. hello namnet på hello storage-konto för hello exportera jobb.|  
|**/Sk: < StorageAccountKey\>**|**Krävs** endast om en behållare SAS inte har angetts. Hej kontonyckel hello storage-konto för hello exportera jobb.|  
|**/csas: < ContainerSas\>**|**Krävs** om hello lagringskontonyckel inte har angetts. hello behållare SAS för att komma åt hello blobbar som är associerade med hello exportjobb.|  
|**/ CopyLogFile: < DriveCopyLogFile\>**|Krävs. loggfil för kopiera hello sökvägen toohello enhet. hello filen har genererats av hello Windows Azure Import/Export service och kan hämtas från hello blob storage som är associerade med hello jobb. hello kopiera loggfilen innehåller information om misslyckade blobbar eller filer som är toobe repareras.|  
|**/ ManifestFile: < DriveManifestFile\>**|Valfri. hello sökvägen toohello export enhetens manifestfilen. Den här filen genereras av hello Windows Azure Import/Export-tjänsten och lagras på hello export enhet och eventuellt i en blob i hello storage-konto som är associerade med hello jobb.<br /><br /> hello kommer innehåll hello filer på hello export enhet att verifieras med hello MD5 hash-värden i den här filen. Filer som är definieras toobe skadad kommer att hämtas och omskrivet toohello mål kataloger.|  
  
## <a name="using-repairexport-mode-toocorrect-failed-exports"></a>Med RepairExport misslyckades läge toocorrect export  
Du kan använda hello Azure Import/Export verktyget toodownload filer som misslyckades tooexport. hello kopiera loggfilen innehåller en lista över filer som misslyckades tooexport.  
  
hello orsaker export fel hello följande möjligheter:  
  
-   Skadade enheter  
  
-   Hej lagringskontonyckel har ändrats under överföringen hello  
  
toorun hello verktyg i **RepairExport** läge, måste du först tooconnect hello enheten som innehåller hello exporterade filerna tooyour dator. Kör därefter hello Azure Import/Export-verktyget om du anger hello sökvägen toothat enhet med hello `/d` parameter. Du måste också loggfilen för toospecify hello sökvägen toohello enhetens kopia som du hämtat. hello körs följande kommandorad exemplet nedan hello verktyget toorepair alla filer som misslyckades tooexport:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
hello följande är ett exempel på en kopia loggfil som visar att ett block i hello blob misslyckades tooexport:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
hello kopiera loggfilen anger att ett fel uppstod när hello Windows Azure Import/Export service hämta någon av hello blob block toohello-hello export enheten. Hej andra komponenter i hello-filen som hämtades och hello fillängden angavs korrekt. I det här fallet kommer hello verktyget öppna hello-hello enheten Hämta hello block från hello storage-konto och skriver den toohello filen intervallet från förskjutningen 65536 med längden 65536.  
  
## <a name="using-repairexport-toovalidate-drive-contents"></a>Med hjälp av RepairExport toovalidate enhet innehållet  
Du kan också använda Azure Import/Export med hello **RepairExport** alternativet toovalidate hello innehållet på hello enhet är korrekta. hello manifestfilen på varje enhet export innehåller MD5s för hello innehållet i hello enhet.  
  
hello Azure Import/Export service kan också spara hello manifestfiler tooa storage-konto under hello exporten. hello hello manifestfiler finns tillgängliga via hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) när hello jobbet har slutförts. Se [Import/Export service Manifest filformatet](storage-import-export-file-format-metadata-and-properties.md) för mer information om hello format för en enhet manifestfil.  
  
hello följande exempel visas hur toorun hello Azure Import/Export verktyget med hello **/ManifestFile** och **/CopyLogFile** parametrar:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
hello följande är ett exempel på en manifestfil:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Efter färdigställa hello reparationsprocessen hello verktyget Läs igenom varje fil som refereras i hello manifestfilen och verifiera hello filernas integritet med hello MD5 hash-värden. För hello manifestet ovan går den igenom hello följande komponenter.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

Någon komponent misslyckas hello verifiering kommer att hämtas av hello verktyget och skrivas om toohello samma fil på hello enhet.  
  
## <a name="next-steps"></a>Nästa steg
 
* [Ställa in hello Azure Import/Export-verktyget](storage-import-export-tool-setup-v1.md)   
* [Förbereda hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Granska jobbstatus med kopiera loggfiler](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Reparera ett importjobb](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Felsöka hello Azure Import/Export-verktyget](storage-import-export-tool-troubleshooting-v1.md)
