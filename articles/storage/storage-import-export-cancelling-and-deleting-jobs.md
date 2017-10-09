---
title: aaaCancel och ta bort ett Azure Import/Export-jobb | Microsoft Docs
description: "Lär dig hur toocancel och ta bort jobb för hello Microsoft Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 5d2aba510dafd0ca9a10f5643f721e7059a6a8f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="877b7-103">Avbryter och ta bort Azure Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="877b7-103">Canceling and deleting Azure Import/Export jobs</span></span>

<span data-ttu-id="877b7-104">Du kan begära att ett jobb avbryts innan du använder hello `Packaging` tillstånd genom att anropa hello [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) åtgärden och inställningen hello `CancelRequested` element för`true`.</span><span class="sxs-lookup"><span data-stu-id="877b7-104">You can request that a job be cancelled before it is in hello `Packaging` state by calling hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and setting hello `CancelRequested` element too`true`.</span></span> <span data-ttu-id="877b7-105">hello-jobbet kommer att avbrytas för bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="877b7-105">hello job will be cancelled on a best-effort basis.</span></span> <span data-ttu-id="877b7-106">Om enheter är i hello processen att överföra data, kan data fortsätta toobe överförs även efter annullering har begärts.</span><span class="sxs-lookup"><span data-stu-id="877b7-106">If drives are in hello process of transferring data, data may continue toobe transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="877b7-107">Avbrutna jobb flyttas toohello `Completed` tillstånd och bevaras under 90 dagar, då tas den bort.</span><span class="sxs-lookup"><span data-stu-id="877b7-107">A cancelled job will move toohello `Completed` state and be kept for 90 days, at which point it will be deleted.</span></span>

 <span data-ttu-id="877b7-108">toodelete ett jobb anropet hello [ta bort jobbet](/rest/api/storageimportexport/jobs#Jobs_Delete) åtgärden innan hello jobbet har skickats (*d.v.s.*, medan hello jobb i hello `Creating` tillstånd).</span><span class="sxs-lookup"><span data-stu-id="877b7-108">toodelete a job, call hello [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before hello job has shipped (*i.e.*, while hello job is in hello `Creating` state).</span></span> <span data-ttu-id="877b7-109">Du kan också ta bort ett jobb när den är i hello `Completed` tillstånd.</span><span class="sxs-lookup"><span data-stu-id="877b7-109">You can also delete a job when it is in hello `Completed` state.</span></span> <span data-ttu-id="877b7-110">När ett jobb har tagits bort, och dess status inte längre är tillgänglig via hello REST API eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="877b7-110">After a job has been deleted, its information and status are no longer accessible via hello REST API or hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="877b7-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="877b7-111">Next steps</span></span>

* [<span data-ttu-id="877b7-112">Med hjälp av hello Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="877b7-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
