---
title: Granska jobbstatus Azure Import/Export - v1 | Microsoft Docs
description: "Lär dig hur du använder loggfilerna som skapades när importera och exportera jobbet kördes för att se status för jobbet Import/Export."
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
ms.openlocfilehash: bdb30bc28c36ab9e969efc8be3b87b97e4027b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a><span data-ttu-id="8178b-103">Granska Azure Import/Export jobbstatus med copy-loggfiler</span><span class="sxs-lookup"><span data-stu-id="8178b-103">Reviewing Azure Import/Export job status with copy log files</span></span>
<span data-ttu-id="8178b-104">När tjänsten Microsoft Azure Import/Export bearbetar enheter som är kopplade till ett jobb som importeras eller exporteras, skriver kopia loggfiler till storage-konto till eller från vilken du importerar eller exporterar BLOB.</span><span class="sxs-lookup"><span data-stu-id="8178b-104">When the Microsoft Azure Import/Export service processes drives associated with an import or export job, it writes copy log files to the storage account to or from which you are importing or exporting blobs.</span></span> <span data-ttu-id="8178b-105">Loggfilen innehåller detaljerad statusinformation om varje fil som importeras eller exporteras.</span><span class="sxs-lookup"><span data-stu-id="8178b-105">The log file contains detailed status about each file that was imported or exported.</span></span> <span data-ttu-id="8178b-106">URL till en loggfil för varje kopia returneras när du frågar status för slutförda jobb. Se [Get Job](/rest/api/storageservices/Get-Job3) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8178b-106">The URL to each copy log file is returned when you query the status of a completed job; see [Get Job](/rest/api/storageservices/Get-Job3) for more information.</span></span>  

## <a name="example-urls"></a><span data-ttu-id="8178b-107">Exempel-URL: er</span><span class="sxs-lookup"><span data-stu-id="8178b-107">Example URLs</span></span>

<span data-ttu-id="8178b-108">Följande är exempel URL: er för kopiera loggfiler för ett importjobb med två enheter:</span><span class="sxs-lookup"><span data-stu-id="8178b-108">The following are example URLs for copy log files for an import job with two drives:</span></span>  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 <span data-ttu-id="8178b-109">Se [Import/Export service loggfilsformat](../storage-import-export-file-format-log.md) för kopierar loggar och en fullständig lista över statuskoder format.</span><span class="sxs-lookup"><span data-stu-id="8178b-109">See [Import/Export service Log File Format](../storage-import-export-file-format-log.md) for the format of copy logs and the full list of status codes.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="8178b-110">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8178b-110">Next steps</span></span>
 
 * [<span data-ttu-id="8178b-111">Konfigurera verktyget Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="8178b-111">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
 * [<span data-ttu-id="8178b-112">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="8178b-112">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [<span data-ttu-id="8178b-113">Reparera ett importjobb</span><span class="sxs-lookup"><span data-stu-id="8178b-113">Repairing an import job</span></span>](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [<span data-ttu-id="8178b-114">Reparera ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="8178b-114">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [<span data-ttu-id="8178b-115">Felsökning av Azures import-/exportverktyg</span><span class="sxs-lookup"><span data-stu-id="8178b-115">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
