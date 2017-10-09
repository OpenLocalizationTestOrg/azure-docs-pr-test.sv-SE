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
ms.openlocfilehash: 363731ede4751124a714b4ce96852e0b8c4dbca4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a><span data-ttu-id="ba3c0-103">Granska Azure Import/Export jobbstatus med copy-loggfiler</span><span class="sxs-lookup"><span data-stu-id="ba3c0-103">Reviewing Azure Import/Export job status with copy log files</span></span>
<span data-ttu-id="ba3c0-104">När hello Microsoft Azure Import/Export-tjänsten bearbetar enheter som är kopplade till ett jobb som importeras eller exporteras, skriver kopiera loggen filer toohello storage-konto tooor som du importerar eller exporterar BLOB.</span><span class="sxs-lookup"><span data-stu-id="ba3c0-104">When hello Microsoft Azure Import/Export service processes drives associated with an import or export job, it writes copy log files toohello storage account tooor from which you are importing or exporting blobs.</span></span> <span data-ttu-id="ba3c0-105">Hej loggfilen innehåller detaljerad statusinformation om varje fil som importeras eller exporteras.</span><span class="sxs-lookup"><span data-stu-id="ba3c0-105">hello log file contains detailed status about each file that was imported or exported.</span></span> <span data-ttu-id="ba3c0-106">loggfil för hello URL tooeach kopia returneras när du frågar hello status för slutförda jobb. Se [Get Job](/rest/api/storageservices/Get-Job3) för mer information.</span><span class="sxs-lookup"><span data-stu-id="ba3c0-106">hello URL tooeach copy log file is returned when you query hello status of a completed job; see [Get Job](/rest/api/storageservices/Get-Job3) for more information.</span></span>  

## <a name="example-urls"></a><span data-ttu-id="ba3c0-107">Exempel-URL: er</span><span class="sxs-lookup"><span data-stu-id="ba3c0-107">Example URLs</span></span>

<span data-ttu-id="ba3c0-108">hello följande är exempel URL: er för kopiera loggfiler för ett importjobb med två enheter:</span><span class="sxs-lookup"><span data-stu-id="ba3c0-108">hello following are example URLs for copy log files for an import job with two drives:</span></span>  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 <span data-ttu-id="ba3c0-109">Se [Import/Export service loggfilsformat](../storage-import-export-file-format-log.md) hello format kopierar loggar och hello fullständig lista över statuskoder.</span><span class="sxs-lookup"><span data-stu-id="ba3c0-109">See [Import/Export service Log File Format](../storage-import-export-file-format-log.md) for hello format of copy logs and hello full list of status codes.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="ba3c0-110">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba3c0-110">Next steps</span></span>
 
 * [<span data-ttu-id="ba3c0-111">Ställa in hello Azure Import/Export-verktyget</span><span class="sxs-lookup"><span data-stu-id="ba3c0-111">Setting Up hello Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
 * [<span data-ttu-id="ba3c0-112">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="ba3c0-112">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [<span data-ttu-id="ba3c0-113">Reparera ett importjobb</span><span class="sxs-lookup"><span data-stu-id="ba3c0-113">Repairing an import job</span></span>](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [<span data-ttu-id="ba3c0-114">Reparera ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="ba3c0-114">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [<span data-ttu-id="ba3c0-115">Felsöka hello Azure Import/Export-verktyget</span><span class="sxs-lookup"><span data-stu-id="ba3c0-115">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
