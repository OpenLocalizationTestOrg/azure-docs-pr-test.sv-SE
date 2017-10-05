---
title: "Felsökning av verktyget Azure Import/Export | Microsoft Docs"
description: "Lär dig mer om några vanliga problem som visas när du använder verktyget Azure Import/Export och hur du hanterar dem.."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7bfda602dbc0ea47828a7c9243b8b9b09ec78432
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-the-azure-importexport-tool"></a><span data-ttu-id="a337d-103">Felsökning av Azures import-/exportverktyg</span><span class="sxs-lookup"><span data-stu-id="a337d-103">Troubleshooting the Azure Import/Export Tool</span></span>
<span data-ttu-id="a337d-104">Verktyget Microsoft Azure Import/Export returnerar felmeddelanden om den körs i problem.</span><span class="sxs-lookup"><span data-stu-id="a337d-104">The Microsoft Azure Import/Export Tool returns error messages if it runs into issues.</span></span> <span data-ttu-id="a337d-105">Det här avsnittet beskrivs några vanliga problem som användarna kan köra till.</span><span class="sxs-lookup"><span data-stu-id="a337d-105">This topic lists some common issues that users may run into.</span></span>  
  
## <a name="a-copy-session-fails-what-i-should-do"></a><span data-ttu-id="a337d-106">En kopia session misslyckas vad jag ska göra?</span><span class="sxs-lookup"><span data-stu-id="a337d-106">A copy session fails, what I should do?</span></span>  
 <span data-ttu-id="a337d-107">När en kopia session misslyckas, finns det två alternativ:</span><span class="sxs-lookup"><span data-stu-id="a337d-107">When a copy session fails, there are two options:</span></span>  
  
 <span data-ttu-id="a337d-108">Om felet är återförsökbart, till exempel om nätverksresursen var offline under en kort period och nu är online igen kan återuppta du kopiera sessionen.</span><span class="sxs-lookup"><span data-stu-id="a337d-108">If the error is retryable, for example if the network share was offline for a short period and now is back online, you can resume the copy session.</span></span> <span data-ttu-id="a337d-109">Om felet inte är återförsökbart, till exempel om du har angett fel fil källkatalogen i parametrar på kommandoraden, måste du avbryta kopiera sessionen.</span><span class="sxs-lookup"><span data-stu-id="a337d-109">If the error is not retryable, for example if you specified the wrong source file directory in the command line parameters, you need to abort the copy session.</span></span> <span data-ttu-id="a337d-110">Se [förbereder hårddiskar för ett importjobb](../storage-import-export-tool-preparing-hard-drives-import-v1.md) för mer information om att återuppta och avbryter kopiera sessioner.</span><span class="sxs-lookup"><span data-stu-id="a337d-110">See [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md) for more information about resuming and aborting copy sessions.</span></span>  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a><span data-ttu-id="a337d-111">Jag kan inte återuppta eller avsluta en session kopia.</span><span class="sxs-lookup"><span data-stu-id="a337d-111">I can't resume or abort a copy session.</span></span>  
 <span data-ttu-id="a337d-112">Om den kopia sessionen är den första kopia sessionen för en enhet och sedan felmeddelandet ska ange: ”den första kopia sessionen inte kan återupptas eller avbröts”.</span><span class="sxs-lookup"><span data-stu-id="a337d-112">If the copy session is the first copy session for a drive, then the error message should state: "The first copy session cannot be resumed or aborted."</span></span> <span data-ttu-id="a337d-113">I det här fallet kan du ta bort den gamla journalfilen och kör kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="a337d-113">In this case, you can delete the old journal file and rerun the command.</span></span>  
  
 <span data-ttu-id="a337d-114">Om en kopia session inte är den första för en enhet, den alltid återupptas eller avbröts.</span><span class="sxs-lookup"><span data-stu-id="a337d-114">If a copy session is not the first one for a drive, it can always be resumed or aborted.</span></span>  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a><span data-ttu-id="a337d-115">Journalfilen gick förlorad, kan jag fortfarande skapa jobbet?</span><span class="sxs-lookup"><span data-stu-id="a337d-115">I lost the journal file, can I still create the job?</span></span>  
 <span data-ttu-id="a337d-116">Journal-fil för en enhet innehåller fullständig information för att kopiera data till den här enheten och den krävs för att lägga till fler filer på enheten och används för att skapa ett importjobb.</span><span class="sxs-lookup"><span data-stu-id="a337d-116">The journal file for a drive contains the complete information of copying data to this drive, and it is needed to add more files to the drive and will be used to create an import job.</span></span> <span data-ttu-id="a337d-117">Om journalfilen tappas bort, behöver du göra om alla kopiera sessioner för enheten.</span><span class="sxs-lookup"><span data-stu-id="a337d-117">If the journal file is lost, you will have to redo all the copy sessions for the drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="a337d-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a337d-118">Next steps</span></span>
 
* [<span data-ttu-id="a337d-119">Konfigurera verktyget azure import/export</span><span class="sxs-lookup"><span data-stu-id="a337d-119">Setting up the azure import/export tool</span></span>](../storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="a337d-120">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="a337d-120">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="a337d-121">Granska jobbstatus med kopiera loggfiler</span><span class="sxs-lookup"><span data-stu-id="a337d-121">Reviewing job status with copy log files</span></span>](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="a337d-122">Reparera ett importjobb</span><span class="sxs-lookup"><span data-stu-id="a337d-122">Repairing an import job</span></span>](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="a337d-123">Reparera ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="a337d-123">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)
