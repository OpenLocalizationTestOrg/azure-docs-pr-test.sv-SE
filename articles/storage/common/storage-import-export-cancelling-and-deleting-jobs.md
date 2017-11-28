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
ms.openlocfilehash: 13456a8e7652850baacb53730cc7bb1520b0a4c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="44452-103">Avbryter och ta bort Azure Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="44452-103">Canceling and deleting Azure Import/Export jobs</span></span>

 <span data-ttu-id="44452-104">toorequest att ett jobb avbrytas innan den är i hello `Packaging` tillstånd, anrop hello [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) igen och ange hello `CancelRequested` element för`true`.</span><span class="sxs-lookup"><span data-stu-id="44452-104">toorequest that a job be canceled before it is in hello `Packaging` state, call hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and set hello `CancelRequested` element too`true`.</span></span> <span data-ttu-id="44452-105">hello avbryts för bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="44452-105">hello job is canceled on a best-effort basis.</span></span> <span data-ttu-id="44452-106">Om enheter är i hello processen att överföra data, kan data fortsätta toobe överförs även efter annullering har begärts.</span><span class="sxs-lookup"><span data-stu-id="44452-106">If drives are in hello process of transferring data, data may continue toobe transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="44452-107">Avbrutna jobb har flyttats toohello `Completed` tillstånd och sparas i 90 dagar, då det har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="44452-107">A canceled job is moved toohello `Completed` state and is kept for 90 days, at which point it is deleted.</span></span>

 <span data-ttu-id="44452-108">toodelete ett jobb anropet hello [ta bort jobbet](/rest/api/storageimportexport/jobs#Jobs_Delete) åtgärden innan hello jobbet har skickats (det vill säga medan hello jobb i hello `Creating` tillstånd).</span><span class="sxs-lookup"><span data-stu-id="44452-108">toodelete a job, call hello [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before hello job has shipped (that is, while hello job is in hello `Creating` state).</span></span> <span data-ttu-id="44452-109">Du kan också ta bort ett jobb när den är i hello `Completed` tillstånd.</span><span class="sxs-lookup"><span data-stu-id="44452-109">You can also delete a job when it is in hello `Completed` state.</span></span> <span data-ttu-id="44452-110">När ett jobb har tagits bort, och dess status inte längre är tillgänglig via hello REST API eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="44452-110">After a job is deleted, its information and status are no longer accessible via hello REST API or hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44452-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44452-111">Next steps</span></span>

* [<span data-ttu-id="44452-112">Med hjälp av hello Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="44452-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
