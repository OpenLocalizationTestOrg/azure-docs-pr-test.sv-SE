---
title: aaaTroubleshooting hello Azure Import/Export verktyget | Microsoft Docs
description: "Lär dig mer om hello vanliga problem som kan uppstå när du använder hello Azure Import/Export-verktyget och hur toohandle dem."
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
ms.openlocfilehash: 5445cefe2703edf4d9d285f761433b7b66d8cb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-azure-importexport-tool"></a><span data-ttu-id="323b7-103">Felsöka hello Azure Import/Export-verktyget</span><span class="sxs-lookup"><span data-stu-id="323b7-103">Troubleshooting hello Azure Import/Export Tool</span></span>
<span data-ttu-id="323b7-104">hello verktyget Azure Import/Export returnerar felmeddelanden om den körs i problem.</span><span class="sxs-lookup"><span data-stu-id="323b7-104">hello Microsoft Azure Import/Export Tool returns error messages if it runs into issues.</span></span> <span data-ttu-id="323b7-105">Det här avsnittet beskrivs några vanliga problem som användarna kan köra till.</span><span class="sxs-lookup"><span data-stu-id="323b7-105">This topic lists some common issues that users may run into.</span></span>  
  
## <a name="a-copy-session-fails-what-i-should-do"></a><span data-ttu-id="323b7-106">En kopia session misslyckas vad jag ska göra?</span><span class="sxs-lookup"><span data-stu-id="323b7-106">A copy session fails, what I should do?</span></span>  
 <span data-ttu-id="323b7-107">När en kopia session misslyckas, finns det två alternativ:</span><span class="sxs-lookup"><span data-stu-id="323b7-107">When a copy session fails, there are two options:</span></span>  
  
 <span data-ttu-id="323b7-108">Om hello fel är återförsökbart, till exempel om hello nätverksresurs var offline för en kort tidsperiod och nu är online igen kan återuppta du hello kopiera session.</span><span class="sxs-lookup"><span data-stu-id="323b7-108">If hello error is retryable, for example if hello network share was offline for a short period and now is back online, you can resume hello copy session.</span></span> <span data-ttu-id="323b7-109">Om hello fel inte återförsökbart, till exempel om du har angett hello fel fil källkatalogen i hello kommandoradsparametrar måste tooabort hello kopiera session.</span><span class="sxs-lookup"><span data-stu-id="323b7-109">If hello error is not retryable, for example if you specified hello wrong source file directory in hello command line parameters, you need tooabort hello copy session.</span></span> <span data-ttu-id="323b7-110">Se [förbereder hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md) för mer information om att återuppta och avbryter kopiera sessioner.</span><span class="sxs-lookup"><span data-stu-id="323b7-110">See [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md) for more information about resuming and aborting copy sessions.</span></span>  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a><span data-ttu-id="323b7-111">Jag kan inte återuppta eller avsluta en session kopia.</span><span class="sxs-lookup"><span data-stu-id="323b7-111">I can't resume or abort a copy session.</span></span>  
 <span data-ttu-id="323b7-112">Om hello kopiera sessionen är hello den första kopian sessionen för en enhet och sedan hello felmeddelandet ska ange: ”hello första kopian sessionen inte kan återupptas eller avbröts”.</span><span class="sxs-lookup"><span data-stu-id="323b7-112">If hello copy session is hello first copy session for a drive, then hello error message should state: "hello first copy session cannot be resumed or aborted."</span></span> <span data-ttu-id="323b7-113">I det här fallet kan du ta bort hello gamla journal-fil och kör hello kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="323b7-113">In this case, you can delete hello old journal file and rerun hello command.</span></span>  
  
 <span data-ttu-id="323b7-114">Om en kopia session inte är hello förstnämnda för en enhet, den alltid återupptas eller avbröts.</span><span class="sxs-lookup"><span data-stu-id="323b7-114">If a copy session is not hello first one for a drive, it can always be resumed or aborted.</span></span>  
  
## <a name="i-lost-hello-journal-file-can-i-still-create-hello-job"></a><span data-ttu-id="323b7-115">Försvann hello journal-fil, kan jag fortfarande skapa hello jobbet?</span><span class="sxs-lookup"><span data-stu-id="323b7-115">I lost hello journal file, can I still create hello job?</span></span>  
 <span data-ttu-id="323b7-116">hello journal-fil för en enhet innehåller hello fullständig information för att kopiera data toothis enheten och den nödvändiga tooadd flera filer toohello enhet och kommer att använda toocreate importen.</span><span class="sxs-lookup"><span data-stu-id="323b7-116">hello journal file for a drive contains hello complete information of copying data toothis drive, and it is needed tooadd more files toohello drive and will be used toocreate an import job.</span></span> <span data-ttu-id="323b7-117">Om hello journalfilen tappas bort, måste tooredo alla hello kopiera sessioner hello-enheten.</span><span class="sxs-lookup"><span data-stu-id="323b7-117">If hello journal file is lost, you will have tooredo all hello copy sessions for hello drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="323b7-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="323b7-118">Next steps</span></span>
 
* [<span data-ttu-id="323b7-119">Konfigurera hello azure import/export-verktyget</span><span class="sxs-lookup"><span data-stu-id="323b7-119">Setting up hello azure import/export tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="323b7-120">Förbereda hårddiskar för ett importjobb</span><span class="sxs-lookup"><span data-stu-id="323b7-120">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="323b7-121">Granska jobbstatus med kopiera loggfiler</span><span class="sxs-lookup"><span data-stu-id="323b7-121">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="323b7-122">Reparera ett importjobb</span><span class="sxs-lookup"><span data-stu-id="323b7-122">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="323b7-123">Reparera ett exportjobb</span><span class="sxs-lookup"><span data-stu-id="323b7-123">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)
