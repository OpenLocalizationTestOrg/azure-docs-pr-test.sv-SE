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
# <a name="repairing-an-export-job"></a><span data-ttu-id="1a6c8-103">Reparera ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="1a6c8-103">Repairing an export job</span></span>
<span data-ttu-id="1a6c8-104">När ett exportjobb har slutförts kan köra du hello Microsoft Azure Import/Export-verktyget lokalt till:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-104">After an export job has completed, you can run hello Microsoft Azure Import/Export Tool on-premises to:</span></span>  
  
1.  <span data-ttu-id="1a6c8-105">Hämta filer att hello Azure Import/Export-tjänsten kunde tooexport.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-105">Download any files that hello Azure Import/Export service was unable tooexport.</span></span>  
  
2.  <span data-ttu-id="1a6c8-106">Verifiera att hello filer på hello enhet exporterats korrekt.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-106">Validate that hello files on hello drive were correctly exported.</span></span>  
  
<span data-ttu-id="1a6c8-107">Du måste ha anslutningen tooAzure lagring toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-107">You must have connectivity tooAzure Storage toouse this functionality.</span></span>  
  
<span data-ttu-id="1a6c8-108">hello-kommandot för att reparera ett importjobb **RepairExport**.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-108">hello command for repairing an import job is **RepairExport**.</span></span>

## <a name="repairexport-parameters"></a><span data-ttu-id="1a6c8-109">RepairExport parametrar</span><span class="sxs-lookup"><span data-stu-id="1a6c8-109">RepairExport parameters</span></span>

<span data-ttu-id="1a6c8-110">hello följande parametrar kan anges med **RepairExport**:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-110">hello following parameters can be specified with **RepairExport**:</span></span>  
  
|<span data-ttu-id="1a6c8-111">Parameter</span><span class="sxs-lookup"><span data-stu-id="1a6c8-111">Parameter</span></span>|<span data-ttu-id="1a6c8-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1a6c8-112">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="1a6c8-113">**/ r: < RepairFile\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-113">**/r:<RepairFile\>**</span></span>|<span data-ttu-id="1a6c8-114">Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-114">Required.</span></span> <span data-ttu-id="1a6c8-115">Sökvägen toohello reparera filen som spårar hello fortskrider hello reparation och låter dig tooresume en avbruten reparation.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-115">Path toohello repair file, which tracks hello progress of hello repair, and allows you tooresume an interrupted repair.</span></span> <span data-ttu-id="1a6c8-116">Varje enhet måste ha en repair-fil.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-116">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="1a6c8-117">När du startar en reparation för en viss enhet skickar hello sökvägen tooa reparera filen inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-117">When you start a repair for a given drive, you will pass in hello path tooa repair file which does not yet exist.</span></span> <span data-ttu-id="1a6c8-118">tooresume en avbruten reparation, överför du i hello namnet på en befintlig fil för reparation.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-118">tooresume an interrupted repair, you should pass in hello name of an existing repair file.</span></span> <span data-ttu-id="1a6c8-119">hello reparera filen-målenheten med motsvarande toohello måste alltid anges.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-119">hello repair file corresponding toohello target drive must always be specified.</span></span>|  
|<span data-ttu-id="1a6c8-120">**/logdir: < LogDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-120">**/logdir:<LogDirectory\>**</span></span>|<span data-ttu-id="1a6c8-121">Valfri.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-121">Optional.</span></span> <span data-ttu-id="1a6c8-122">hello loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-122">hello log directory.</span></span> <span data-ttu-id="1a6c8-123">Utförlig loggfilerna skrivs toothis directory.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-123">Verbose log files will be written toothis directory.</span></span> <span data-ttu-id="1a6c8-124">Om inga loggkatalogen anges används hello katalogen som hello loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-124">If no log directory is specified, hello current directory will be used as hello log directory.</span></span>|  
|<span data-ttu-id="1a6c8-125">**/ d: < TargetDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-125">**/d:<TargetDirectory\>**</span></span>|<span data-ttu-id="1a6c8-126">Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-126">Required.</span></span> <span data-ttu-id="1a6c8-127">hello directory toovalidate och reparation.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-127">hello directory toovalidate and repair.</span></span> <span data-ttu-id="1a6c8-128">Detta är vanligtvis hello rotkatalogen hello export enheten, men kan också vara en nätverksfilresurs med en kopia av hello exporterade filerna.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-128">This is usually hello root directory of hello export drive, but could also be a network file share containing a copy of hello exported files.</span></span>|  
|<span data-ttu-id="1a6c8-129">**/BK: < BitLockerKey\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-129">**/bk:<BitLockerKey\>**</span></span>|<span data-ttu-id="1a6c8-130">Valfri.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-130">Optional.</span></span> <span data-ttu-id="1a6c8-131">Du bör ange hello BitLocker-nyckel om du vill att hello verktyget toounlock krypterade där hello exporterade filer lagras.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-131">You should specify hello BitLocker key if you want hello tool toounlock an encrypted where hello exported files are stored.</span></span>|  
|<span data-ttu-id="1a6c8-132">**/SN: < StorageAccountName\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-132">**/sn:<StorageAccountName\>**</span></span>|<span data-ttu-id="1a6c8-133">Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-133">Required.</span></span> <span data-ttu-id="1a6c8-134">hello namnet på hello storage-konto för hello exportera jobb.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-134">hello name of hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="1a6c8-135">**/Sk: < StorageAccountKey\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-135">**/sk:<StorageAccountKey\>**</span></span>|<span data-ttu-id="1a6c8-136">**Krävs** endast om en behållare SAS inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-136">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="1a6c8-137">Hej kontonyckel hello storage-konto för hello exportera jobb.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-137">hello account key for hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="1a6c8-138">**/csas: < ContainerSas\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-138">**/csas:<ContainerSas\>**</span></span>|<span data-ttu-id="1a6c8-139">**Krävs** om hello lagringskontonyckel inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-139">**Required** if and only if hello storage account key is not specified.</span></span> <span data-ttu-id="1a6c8-140">hello behållare SAS för att komma åt hello blobbar som är associerade med hello exportjobb.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-140">hello container SAS for accessing hello blobs associated with hello export job.</span></span>|  
|<span data-ttu-id="1a6c8-141">**/ CopyLogFile: < DriveCopyLogFile\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-141">**/CopyLogFile:<DriveCopyLogFile\>**</span></span>|<span data-ttu-id="1a6c8-142">Krävs.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-142">Required.</span></span> <span data-ttu-id="1a6c8-143">loggfil för kopiera hello sökvägen toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-143">hello path toohello drive copy log file.</span></span> <span data-ttu-id="1a6c8-144">hello filen har genererats av hello Windows Azure Import/Export service och kan hämtas från hello blob storage som är associerade med hello jobb.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-144">hello file is generated by hello Windows Azure Import/Export service and can be downloaded from hello blob storage associated with hello job.</span></span> <span data-ttu-id="1a6c8-145">hello kopiera loggfilen innehåller information om misslyckade blobbar eller filer som är toobe repareras.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-145">hello copy log file contains information about failed blobs or files which are toobe repaired.</span></span>|  
|<span data-ttu-id="1a6c8-146">**/ ManifestFile: < DriveManifestFile\>**</span><span class="sxs-lookup"><span data-stu-id="1a6c8-146">**/ManifestFile:<DriveManifestFile\>**</span></span>|<span data-ttu-id="1a6c8-147">Valfri.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-147">Optional.</span></span> <span data-ttu-id="1a6c8-148">hello sökvägen toohello export enhetens manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-148">hello path toohello export drive's manifest file.</span></span> <span data-ttu-id="1a6c8-149">Den här filen genereras av hello Windows Azure Import/Export-tjänsten och lagras på hello export enhet och eventuellt i en blob i hello storage-konto som är associerade med hello jobb.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-149">This file is generated by hello Windows Azure Import/Export service and stored on hello export drive, and optionally in a blob in hello storage account associated with hello job.</span></span><br /><br /> <span data-ttu-id="1a6c8-150">hello kommer innehåll hello filer på hello export enhet att verifieras med hello MD5 hash-värden i den här filen.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-150">hello content of hello files on hello export drive will be verified with hello MD5 hashes contained in this file.</span></span> <span data-ttu-id="1a6c8-151">Filer som är definieras toobe skadad kommer att hämtas och omskrivet toohello mål kataloger.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-151">Any files that are determined toobe corrupted will be downloaded and rewritten toohello target directories.</span></span>|  
  
## <a name="using-repairexport-mode-toocorrect-failed-exports"></a><span data-ttu-id="1a6c8-152">Med RepairExport misslyckades läge toocorrect export</span><span class="sxs-lookup"><span data-stu-id="1a6c8-152">Using RepairExport mode toocorrect failed exports</span></span>  
<span data-ttu-id="1a6c8-153">Du kan använda hello Azure Import/Export verktyget toodownload filer som misslyckades tooexport.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-153">You can use hello Azure Import/Export Tool toodownload files that failed tooexport.</span></span> <span data-ttu-id="1a6c8-154">hello kopiera loggfilen innehåller en lista över filer som misslyckades tooexport.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-154">hello copy log file will contain a list of files that failed tooexport.</span></span>  
  
<span data-ttu-id="1a6c8-155">hello orsaker export fel hello följande möjligheter:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-155">hello causes of export failures include hello following possibilities:</span></span>  
  
-   <span data-ttu-id="1a6c8-156">Skadade enheter</span><span class="sxs-lookup"><span data-stu-id="1a6c8-156">Damaged drives</span></span>  
  
-   <span data-ttu-id="1a6c8-157">Hej lagringskontonyckel har ändrats under överföringen hello</span><span class="sxs-lookup"><span data-stu-id="1a6c8-157">hello storage account key changed during hello transfer process</span></span>  
  
<span data-ttu-id="1a6c8-158">toorun hello verktyg i **RepairExport** läge, måste du först tooconnect hello enheten som innehåller hello exporterade filerna tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-158">toorun hello tool in **RepairExport** mode, you first need tooconnect hello drive containing hello exported files tooyour computer.</span></span> <span data-ttu-id="1a6c8-159">Kör därefter hello Azure Import/Export-verktyget om du anger hello sökvägen toothat enhet med hello `/d` parameter.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-159">Next, run hello Azure Import/Export Tool, specifying hello path toothat drive with hello `/d` parameter.</span></span> <span data-ttu-id="1a6c8-160">Du måste också loggfilen för toospecify hello sökvägen toohello enhetens kopia som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-160">You also need toospecify hello path toohello drive's copy log file that you downloaded.</span></span> <span data-ttu-id="1a6c8-161">hello körs följande kommandorad exemplet nedan hello verktyget toorepair alla filer som misslyckades tooexport:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-161">hello following command line example below runs hello tool toorepair any files that failed tooexport:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
<span data-ttu-id="1a6c8-162">hello följande är ett exempel på en kopia loggfil som visar att ett block i hello blob misslyckades tooexport:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-162">hello following is an example of a copy log file that shows that one block in hello blob failed tooexport:</span></span>  
  
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
  
<span data-ttu-id="1a6c8-163">hello kopiera loggfilen anger att ett fel uppstod när hello Windows Azure Import/Export service hämta någon av hello blob block toohello-hello export enheten.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-163">hello copy log file indicates that a failure occurred while hello Windows Azure Import/Export service was downloading one of hello blob's blocks toohello file on hello export drive.</span></span> <span data-ttu-id="1a6c8-164">Hej andra komponenter i hello-filen som hämtades och hello fillängden angavs korrekt.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-164">hello other components of hello file downloaded successfully, and hello file length was correctly set.</span></span> <span data-ttu-id="1a6c8-165">I det här fallet kommer hello verktyget öppna hello-hello enheten Hämta hello block från hello storage-konto och skriver den toohello filen intervallet från förskjutningen 65536 med längden 65536.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-165">In this case, hello tool will open hello file on hello drive, download hello block from hello storage account, and write it toohello file range starting from offset 65536 with length 65536.</span></span>  
  
## <a name="using-repairexport-toovalidate-drive-contents"></a><span data-ttu-id="1a6c8-166">Med hjälp av RepairExport toovalidate enhet innehållet</span><span class="sxs-lookup"><span data-stu-id="1a6c8-166">Using RepairExport toovalidate drive contents</span></span>  
<span data-ttu-id="1a6c8-167">Du kan också använda Azure Import/Export med hello **RepairExport** alternativet toovalidate hello innehållet på hello enhet är korrekta.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-167">You can also use Azure Import/Export with hello **RepairExport** option toovalidate hello contents on hello drive are correct.</span></span> <span data-ttu-id="1a6c8-168">hello manifestfilen på varje enhet export innehåller MD5s för hello innehållet i hello enhet.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-168">hello manifest file on each export drive contains MD5s for hello contents of hello drive.</span></span>  
  
<span data-ttu-id="1a6c8-169">hello Azure Import/Export service kan också spara hello manifestfiler tooa storage-konto under hello exporten.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-169">hello Azure Import/Export service can also save hello manifest files tooa storage account during hello export process.</span></span> <span data-ttu-id="1a6c8-170">hello hello manifestfiler finns tillgängliga via hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-170">hello location of hello manifest files is available via hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation when hello job has completed.</span></span> <span data-ttu-id="1a6c8-171">Se [Import/Export service Manifest filformatet](storage-import-export-file-format-metadata-and-properties.md) för mer information om hello format för en enhet manifestfil.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-171">See [Import/Export service Manifest File Format](storage-import-export-file-format-metadata-and-properties.md) for more information about hello format of a drive manifest file.</span></span>  
  
<span data-ttu-id="1a6c8-172">hello följande exempel visas hur toorun hello Azure Import/Export verktyget med hello **/ManifestFile** och **/CopyLogFile** parametrar:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-172">hello following example shows how toorun hello Azure Import/Export Tool with hello **/ManifestFile** and **/CopyLogFile** parameters:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
<span data-ttu-id="1a6c8-173">hello följande är ett exempel på en manifestfil:</span><span class="sxs-lookup"><span data-stu-id="1a6c8-173">hello following is an example of a manifest file:</span></span>  
  
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
  
<span data-ttu-id="1a6c8-174">Efter färdigställa hello reparationsprocessen hello verktyget Läs igenom varje fil som refereras i hello manifestfilen och verifiera hello filernas integritet med hello MD5 hash-värden.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-174">After finishing hello repair process, hello tool will read through each file referenced in hello manifest file and verify hello file's integrity with hello MD5 hashes.</span></span> <span data-ttu-id="1a6c8-175">För hello manifestet ovan går den igenom hello följande komponenter.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-175">For hello manifest above, it will go through hello following components.</span></span>  

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

<span data-ttu-id="1a6c8-176">Någon komponent misslyckas hello verifiering kommer att hämtas av hello verktyget och skrivas om toohello samma fil på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="1a6c8-176">Any component failing hello verification will be downloaded by hello tool and rewritten toohello same file on hello drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="1a6c8-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a6c8-177">Next steps</span></span>
 
* [<span data-ttu-id="1a6c8-178">Ställa in hello Azure Import/Export-verktyget</span><span class="sxs-lookup"><span data-stu-id="1a6c8-178">Setting Up hello Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="1a6c8-179">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="1a6c8-179">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="1a6c8-180">Granska jobbstatus med kopiera loggfiler</span><span class="sxs-lookup"><span data-stu-id="1a6c8-180">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="1a6c8-181">Reparera ett importjobb</span><span class="sxs-lookup"><span data-stu-id="1a6c8-181">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="1a6c8-182">Felsöka hello Azure Import/Export-verktyget</span><span class="sxs-lookup"><span data-stu-id="1a6c8-182">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
