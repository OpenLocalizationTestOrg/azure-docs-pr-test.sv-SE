---
title: aaaReviewing jobbstatus Azure Import/Export - v1 | Microsoft Docs
description: "Lär dig hur toouse hello loggfilerna som skapades när hello importera eller exportera jobbet kördes toosee hello status för hello Import/Export-jobb."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: 378bc9a7ef6bfe65209413c8c4134f313a2c0d79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Granska Azure Import/Export jobbstatus med copy-loggfiler
När hello Microsoft Azure Import/Export-tjänsten bearbetar enheter som är kopplade till ett jobb som importeras eller exporteras, skriver kopiera loggen filer toohello storage-konto tooor som du importerar eller exporterar BLOB. Hej loggfilen innehåller detaljerad statusinformation om varje fil som importeras eller exporteras. loggfil för hello URL tooeach kopia returneras när du frågar hello status för slutförda jobb. Se [Get Job](/rest/api/storageservices/Get-Job3) för mer information.  

## <a name="example-urls"></a>Exempel-URL: er

hello följande är exempel URL: er för kopiera loggfiler för ett importjobb med två enheter:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Se [Import/Export service loggfilsformat](storage-import-export-file-format-log.md) hello format kopierar loggar och hello fullständig lista över statuskoder.  
  
## <a name="next-steps"></a>Nästa steg
 
 * [Ställa in hello Azure Import/Export-verktyget](storage-import-export-tool-setup-v1.md)   
 * [Förbereda hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Reparera ett importjobb](storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Reparera ett exportjobb](storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Felsöka hello Azure Import/Export-verktyget](storage-import-export-tool-troubleshooting-v1.md)
