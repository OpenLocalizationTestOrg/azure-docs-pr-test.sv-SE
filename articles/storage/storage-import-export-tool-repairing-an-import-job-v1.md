---
title: Reparera ett Azure Import/Export-importjobb - v1 | Microsoft Docs
description: "Lär dig mer om att reparera ett importjobb som har skapats och att köras med tjänsten Azure Import/Export."
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
ms.openlocfilehash: b374eabcbafa6ffe64c639fb6c89be857ecfc221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="repairing-an-import-job"></a><span data-ttu-id="b36be-103">Reparera ett importjobb</span><span class="sxs-lookup"><span data-stu-id="b36be-103">Repairing an import job</span></span>
<span data-ttu-id="b36be-104">Microsoft Azure Import/Export-tjänsten kanske inte kan kopiera vissa av dina filer eller delar av en fil till Windows Azure Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b36be-104">The Microsoft Azure Import/Export service may fail to copy some of your files or parts of a file to the Windows Azure Blob service.</span></span> <span data-ttu-id="b36be-105">Vissa fel anledningar:</span><span class="sxs-lookup"><span data-stu-id="b36be-105">Some reasons for failures include:</span></span>  
  
-   <span data-ttu-id="b36be-106">Skadade filer</span><span class="sxs-lookup"><span data-stu-id="b36be-106">Corrupted files</span></span>  
  
-   <span data-ttu-id="b36be-107">Skadade enheter</span><span class="sxs-lookup"><span data-stu-id="b36be-107">Damaged drives</span></span>  
  
-   <span data-ttu-id="b36be-108">Lagringskontots åtkomstnyckel ändras när filen överfördes.</span><span class="sxs-lookup"><span data-stu-id="b36be-108">The storage account key changed while the file was being transferred.</span></span>  
  
<span data-ttu-id="b36be-109">Du kan köra verktyget Microsoft Azure Import/Export med importen jobbets kopiera loggfiler och verktyget kommer att överföra filer som saknas (eller delar av en fil) till Windows Azure storage-konto att slutföra importen.</span><span class="sxs-lookup"><span data-stu-id="b36be-109">You can run the Microsoft Azure Import/Export Tool with the import job's copy log files, and the tool will upload the missing files (or parts of a file) to your Windows Azure storage account to complete import job.</span></span>  
  
## <a name="repairimport-parameters"></a><span data-ttu-id="b36be-110">RepairImport parametrar</span><span class="sxs-lookup"><span data-stu-id="b36be-110">RepairImport parameters</span></span>

<span data-ttu-id="b36be-111">Följande parametrar kan anges med **RepairImport**:</span><span class="sxs-lookup"><span data-stu-id="b36be-111">The following parameters can be specified with **RepairImport**:</span></span> 
  
|||  
|-|-|  
|<span data-ttu-id="b36be-112">**/ r:**< RepairFile\></span><span class="sxs-lookup"><span data-stu-id="b36be-112">**/r:**<RepairFile\></span></span>|<span data-ttu-id="b36be-113">**Krävs.**</span><span class="sxs-lookup"><span data-stu-id="b36be-113">**Required.**</span></span> <span data-ttu-id="b36be-114">Sökvägen till filen reparation, som spårar förloppet för att reparera och gör att du kan återuppta en avbruten reparation.</span><span class="sxs-lookup"><span data-stu-id="b36be-114">Path to the repair file, which tracks the progress of the repair, and allows you to resume an interrupted repair.</span></span> <span data-ttu-id="b36be-115">Varje enhet måste ha en repair-fil.</span><span class="sxs-lookup"><span data-stu-id="b36be-115">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="b36be-116">När du startar en reparation för en viss enhet skickar du i sökvägen till en repair-fil som ännu inte finns.</span><span class="sxs-lookup"><span data-stu-id="b36be-116">When you start a repair for a given drive, you will pass in the path to a repair file which does not yet exist.</span></span> <span data-ttu-id="b36be-117">Om du vill återuppta ett avbryts reparation, överför du namnet på en befintlig fil för reparation.</span><span class="sxs-lookup"><span data-stu-id="b36be-117">To resume an interrupted repair, you should pass in the name of an existing repair file.</span></span> <span data-ttu-id="b36be-118">Reparera filen motsvarar målenheten måste alltid anges.</span><span class="sxs-lookup"><span data-stu-id="b36be-118">The repair file corresponding to the target drive must always be specified.</span></span>|  
|<span data-ttu-id="b36be-119">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="b36be-119">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="b36be-120">**Valfritt.**</span><span class="sxs-lookup"><span data-stu-id="b36be-120">**Optional.**</span></span> <span data-ttu-id="b36be-121">Loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="b36be-121">The log directory.</span></span> <span data-ttu-id="b36be-122">Utförlig loggfilerna skrivs till den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="b36be-122">Verbose log files will be written to this directory.</span></span> <span data-ttu-id="b36be-123">Om inga loggkatalogen anges, används den aktuella katalogen som loggkatalogen.</span><span class="sxs-lookup"><span data-stu-id="b36be-123">If no log directory is specified, the current directory will be used as the log directory.</span></span>|  
|<span data-ttu-id="b36be-124">**/ d:**< TargetDirectories\></span><span class="sxs-lookup"><span data-stu-id="b36be-124">**/d:**<TargetDirectories\></span></span>|<span data-ttu-id="b36be-125">**Krävs.**</span><span class="sxs-lookup"><span data-stu-id="b36be-125">**Required.**</span></span> <span data-ttu-id="b36be-126">En eller flera semikolonavgränsad kataloger som innehåller de ursprungliga filerna som har importerats.</span><span class="sxs-lookup"><span data-stu-id="b36be-126">One or more semicolon-separated directories that contain the original files that were imported.</span></span> <span data-ttu-id="b36be-127">Importera enheten kan också användas, men är inte nödvändigt om alternativa platser för ursprungliga filerna är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b36be-127">The import drive may also be used, but is not required if alternate locations of original files are available.</span></span>|  
|<span data-ttu-id="b36be-128">**/BK:**< BitLockerKey\></span><span class="sxs-lookup"><span data-stu-id="b36be-128">**/bk:**<BitLockerKey\></span></span>|<span data-ttu-id="b36be-129">**Valfritt.**</span><span class="sxs-lookup"><span data-stu-id="b36be-129">**Optional.**</span></span> <span data-ttu-id="b36be-130">Du bör ange BitLocker-nyckel om du vill använda verktyget för att låsa upp en krypterad enhet där de ursprungliga filerna är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b36be-130">You should specify the BitLocker key if you want the tool to unlock an encrypted drive where the original files are available.</span></span>|  
|<span data-ttu-id="b36be-131">**/SN:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="b36be-131">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="b36be-132">**Krävs.**</span><span class="sxs-lookup"><span data-stu-id="b36be-132">**Required.**</span></span> <span data-ttu-id="b36be-133">Namnet på lagringskontot för importjobbet.</span><span class="sxs-lookup"><span data-stu-id="b36be-133">The name of the storage account for the import job.</span></span>|  
|<span data-ttu-id="b36be-134">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="b36be-134">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="b36be-135">**Krävs** endast om en behållare SAS inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="b36be-135">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="b36be-136">Kontonyckel för lagringskontot för importjobbet.</span><span class="sxs-lookup"><span data-stu-id="b36be-136">The account key for the storage account for the import job.</span></span>|  
|<span data-ttu-id="b36be-137">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="b36be-137">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="b36be-138">**Krävs** om lagringskontots åtkomstnyckel inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="b36be-138">**Required** if and only if the storage account key is not specified.</span></span> <span data-ttu-id="b36be-139">Behållaren SAS för att komma åt blobar som är associerad med importjobbet.</span><span class="sxs-lookup"><span data-stu-id="b36be-139">The container SAS for accessing the blobs associated with the import job.</span></span>|  
|<span data-ttu-id="b36be-140">**/ CopyLogFile:**< DriveCopyLogFile\></span><span class="sxs-lookup"><span data-stu-id="b36be-140">**/CopyLogFile:**<DriveCopyLogFile\></span></span>|<span data-ttu-id="b36be-141">**Krävs.**</span><span class="sxs-lookup"><span data-stu-id="b36be-141">**Required.**</span></span> <span data-ttu-id="b36be-142">Sökvägen till loggfilen för enheten kopia (antingen utförlig loggen eller fel logga).</span><span class="sxs-lookup"><span data-stu-id="b36be-142">Path to the drive copy log file (either verbose log or error log).</span></span> <span data-ttu-id="b36be-143">Filen har genererats av Windows Azure Import/Export-tjänsten och kan hämtas från blob storage som är associerat med jobbet.</span><span class="sxs-lookup"><span data-stu-id="b36be-143">The file is generated by the Windows Azure Import/Export service and can be downloaded from the blob storage associated with the job.</span></span> <span data-ttu-id="b36be-144">Kopiera loggfilen innehåller information om misslyckade blobbar eller filer som är repareras.</span><span class="sxs-lookup"><span data-stu-id="b36be-144">The copy log file contains information about failed blobs or files which are to be repaired.</span></span>|  
|<span data-ttu-id="b36be-145">**/ PathMapFile:**< DrivePathMapFile\></span><span class="sxs-lookup"><span data-stu-id="b36be-145">**/PathMapFile:**<DrivePathMapFile\></span></span>|<span data-ttu-id="b36be-146">**Valfritt.**</span><span class="sxs-lookup"><span data-stu-id="b36be-146">**Optional.**</span></span> <span data-ttu-id="b36be-147">Sökväg till en textfil som kan användas för att lösa tvetydigheter om du har flera filer med samma namn som du importerar i samma jobb.</span><span class="sxs-lookup"><span data-stu-id="b36be-147">Path to a text file that can be used to resolve ambiguities if you have multiple files with the same name that you were importing in the same job.</span></span> <span data-ttu-id="b36be-148">Första gången verktyget körs kan det fylla i den här filen med alla tvetydigt namn.</span><span class="sxs-lookup"><span data-stu-id="b36be-148">The first time the tool is run, it can populate this file with all of the ambiguous names.</span></span> <span data-ttu-id="b36be-149">Efterföljande körningar av verktyget använder den här filen för att lösa tvetydigheter.</span><span class="sxs-lookup"><span data-stu-id="b36be-149">Subsequent runs of the tool will use this file to resolve the ambiguities.</span></span>|  
  
## <a name="using-the-repairimport-command"></a><span data-ttu-id="b36be-150">Med hjälp av kommandot RepairImport</span><span class="sxs-lookup"><span data-stu-id="b36be-150">Using the RepairImport command</span></span>  
<span data-ttu-id="b36be-151">Om du vill reparera importera data med strömmande data över nätverket, måste du ange de kataloger som innehåller de ursprungliga filerna som du importerar med hjälp av den `/d` parameter.</span><span class="sxs-lookup"><span data-stu-id="b36be-151">To repair import data by streaming the data over the network, you must specify the directories that contain the original files you were importing using the `/d` parameter.</span></span> <span data-ttu-id="b36be-152">Du måste också ange kopiera loggfilen som du hämtade från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b36be-152">You must also specify the copy log file that you downloaded from your storage account.</span></span> <span data-ttu-id="b36be-153">Det ser ut som en typisk kommandoraden för att reparera ett importjobb med partiellt fel:</span><span class="sxs-lookup"><span data-stu-id="b36be-153">A typical command line to repair an import job with partial failures looks like:</span></span>  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
<span data-ttu-id="b36be-154">Följande är ett exempel på en loggfil för kopia.</span><span class="sxs-lookup"><span data-stu-id="b36be-154">The following is an example of a copy log file.</span></span> <span data-ttu-id="b36be-155">I det här fallet en 64 K del av en fil är skadad på enheten som levererades för importjobbet.</span><span class="sxs-lookup"><span data-stu-id="b36be-155">In this case, one 64K piece of a file was corrupted on the drive that was shipped for the import job.</span></span> <span data-ttu-id="b36be-156">Eftersom detta är endast felet anges resten av blobbar i jobbet har importerats.</span><span class="sxs-lookup"><span data-stu-id="b36be-156">Since this is the only failure indicated, the rest of the blobs in the job were successfully imported.</span></span>  
  
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
  
<span data-ttu-id="b36be-157">När den här loggfilen kopia skickas till verktyget Azure Import/Export försöker verktyget Slutför import för den här filen genom att kopiera saknas innehållet över nätverket.</span><span class="sxs-lookup"><span data-stu-id="b36be-157">When this copy log is passed to the Azure Import/Export Tool, the tool will try to finish the import for this file by copying the missing contents across the network.</span></span> <span data-ttu-id="b36be-158">Följande exempel ovan, verktyget söker efter den ursprungliga filen `\animals\koala.jpg` i de två katalogerna `C:\Users\bob\Pictures` och `X:\BobBackup\photos`.</span><span class="sxs-lookup"><span data-stu-id="b36be-158">Following the example above, the tool will look for the original file `\animals\koala.jpg` within the two directories `C:\Users\bob\Pictures` and `X:\BobBackup\photos`.</span></span> <span data-ttu-id="b36be-159">Om filen `C:\Users\bob\Pictures\animals\koala.jpg` finns verktyget Azure Import/Export saknas dataområdet kopieras till motsvarande blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.</span><span class="sxs-lookup"><span data-stu-id="b36be-159">If the file `C:\Users\bob\Pictures\animals\koala.jpg` exists, the Azure Import/Export Tool will copy the missing range of data to the corresponding blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.</span></span>  
  
## <a name="resolving-conflicts-when-using-repairimport"></a><span data-ttu-id="b36be-160">Lösa konflikter när du använder RepairImport</span><span class="sxs-lookup"><span data-stu-id="b36be-160">Resolving conflicts when using RepairImport</span></span>  
<span data-ttu-id="b36be-161">I vissa situationer kan verktyget kanske inte kan hitta eller öppna filen behövs för en av följande orsaker: gick inte att hitta filen, filen är inte tillgänglig, filnamnet är tvetydigt eller innehållet i filen är inte korrekta längre.</span><span class="sxs-lookup"><span data-stu-id="b36be-161">In some situations, the tool may not be able to find or open the necessary file for one of the following reasons: the file could not be found, the file is not accessible, the file name is ambiguous, or the content of the file is no longer correct.</span></span>  
  
<span data-ttu-id="b36be-162">Ett tvetydigt fel kan inträffa om verktyget försöker hitta `\animals\koala.jpg` och det finns en fil med detta namn under både `C:\Users\bob\pictures` och `X:\BobBackup\photos`.</span><span class="sxs-lookup"><span data-stu-id="b36be-162">An ambiguous error could occur if the tool is trying to locate `\animals\koala.jpg` and there is a file with that name under both `C:\Users\bob\pictures` and `X:\BobBackup\photos`.</span></span> <span data-ttu-id="b36be-163">Det vill säga både `C:\Users\bob\pictures\animals\koala.jpg` och `X:\BobBackup\photos\animals\koala.jpg` finns på import jobbet enheter.</span><span class="sxs-lookup"><span data-stu-id="b36be-163">That is, both `C:\Users\bob\pictures\animals\koala.jpg` and `X:\BobBackup\photos\animals\koala.jpg` exist on the import job drives.</span></span>  
  
<span data-ttu-id="b36be-164">Den `/PathMapFile` alternativ tillåter dig att lösa dessa problem.</span><span class="sxs-lookup"><span data-stu-id="b36be-164">The `/PathMapFile` option will allow you to resolve these errors.</span></span> <span data-ttu-id="b36be-165">Du kan ange namnet på filen som ska innehåller en lista över filer som verktyget går inte att identifiera.</span><span class="sxs-lookup"><span data-stu-id="b36be-165">You can specify the name of the file which will contains the list of files that the tool was not able to correctly identify.</span></span> <span data-ttu-id="b36be-166">Följande är ett exempel kommandorad som ska fylla `9WM35C2V_pathmap.txt`:</span><span class="sxs-lookup"><span data-stu-id="b36be-166">The following is an example command line that would populate `9WM35C2V_pathmap.txt`:</span></span>  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
<span data-ttu-id="b36be-167">Verktyget kommer sedan skriva problematiska sökvägarna till `9WM35C2V_pathmap.txt`, ett på varje rad.</span><span class="sxs-lookup"><span data-stu-id="b36be-167">The tool will then write the problematic file paths to `9WM35C2V_pathmap.txt`, one on each line.</span></span> <span data-ttu-id="b36be-168">Filen kan exempelvis innehålla följande poster efter körning av kommandot:</span><span class="sxs-lookup"><span data-stu-id="b36be-168">For instance, the file may contain the following entries after running the command:</span></span>  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 <span data-ttu-id="b36be-169">För varje fil i listan, bör du försöka hitta och öppna filen för att säkerställa att den är tillgänglig för verktyget.</span><span class="sxs-lookup"><span data-stu-id="b36be-169">For each file in the list, you should attempt to locate and open the file to ensure it is available to the tool.</span></span> <span data-ttu-id="b36be-170">Du kan ändra sökvägen mappningsfilen och Lägg till sökvägen till varje fil på samma rad, avgränsade med semikolon fliken om du vill berätta verktyget uttryckligen var du hittar en fil:</span><span class="sxs-lookup"><span data-stu-id="b36be-170">If you wish to tell the tool explicitly where to find a file, you can modify the path map file and add the path to each file on the same line, separated by a tab character:</span></span>  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
<span data-ttu-id="b36be-171">Efter att göra nödvändiga filer tillgängliga för verktyget eller uppdatering av mappningsfilen sökväg, kan du köra verktyget för att slutföra importen.</span><span class="sxs-lookup"><span data-stu-id="b36be-171">After making the necessary files available to the tool, or updating the path map file, you can rerun the tool to complete the import process.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="b36be-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b36be-172">Next steps</span></span>
 
* [<span data-ttu-id="b36be-173">Konfigurera verktyget Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="b36be-173">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="b36be-174">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="b36be-174">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="b36be-175">Granska jobbstatus med kopiera loggfiler</span><span class="sxs-lookup"><span data-stu-id="b36be-175">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="b36be-176">Reparera ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="b36be-176">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)   
* [<span data-ttu-id="b36be-177">Felsökning av Azures import-/exportverktyg</span><span class="sxs-lookup"><span data-stu-id="b36be-177">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
