---
title: "aaaManage beräkningskraft i Azure SQL Data Warehouse (Azure portal) | Microsoft Docs"
description: "Azure portal uppgifter toomanage beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkning resurser toosave kostnader."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: b2e84b3763e97ce88c190eecfb64b2d06f727229
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a><span data-ttu-id="a5b82-105">Hantera datorkraft i Azure SQL Data Warehouse (Azure portal)</span><span class="sxs-lookup"><span data-stu-id="a5b82-105">Manage compute power in Azure SQL Data Warehouse (Azure portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5b82-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="a5b82-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="a5b82-107">Portal</span><span class="sxs-lookup"><span data-stu-id="a5b82-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="a5b82-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5b82-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="a5b82-109">REST</span><span class="sxs-lookup"><span data-stu-id="a5b82-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="a5b82-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="a5b82-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a><span data-ttu-id="a5b82-111">Skala datorkraft</span><span class="sxs-lookup"><span data-stu-id="a5b82-111">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="a5b82-112">toochange beräkningsresurser:</span><span class="sxs-lookup"><span data-stu-id="a5b82-112">toochange compute resources:</span></span>

1. <span data-ttu-id="a5b82-113">Öppna hello [Azure-portalen][Azure portal], öppna databasen och på **skala**.</span><span class="sxs-lookup"><span data-stu-id="a5b82-113">Open hello [Azure portal][Azure portal], open your database, and click **Scale**.</span></span>

    ![Klicka på skalan][1]
2. <span data-ttu-id="a5b82-115">Dra reglaget hello åt vänster eller höger toochange hello DWU-inställningar i hello skala bladet.</span><span class="sxs-lookup"><span data-stu-id="a5b82-115">In hello Scale blade, move hello slider left or right toochange hello DWU setting.</span></span>

    ![Flytta skjutreglaget][2]
3. <span data-ttu-id="a5b82-117">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a5b82-117">Click **Save**.</span></span> <span data-ttu-id="a5b82-118">Ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="a5b82-118">A confirmation message appears.</span></span> <span data-ttu-id="a5b82-119">Klicka på **Ja** tooconfirm eller **inga** toocancel.</span><span class="sxs-lookup"><span data-stu-id="a5b82-119">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Klicka på Spara][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="a5b82-121">Pausa beräkning</span><span class="sxs-lookup"><span data-stu-id="a5b82-121">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="a5b82-122">toopause en-databas:</span><span class="sxs-lookup"><span data-stu-id="a5b82-122">toopause a database:</span></span>

1. <span data-ttu-id="a5b82-123">Öppna hello [Azure-portalen] [ Azure portal] och öppna databasen.</span><span class="sxs-lookup"><span data-stu-id="a5b82-123">Open hello [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="a5b82-124">Observera att hello Status är **Online**.</span><span class="sxs-lookup"><span data-stu-id="a5b82-124">Notice that hello Status is **Online**.</span></span>

    ![Onlinestatus][6]
2. <span data-ttu-id="a5b82-126">toosuspend beräknings- och resurser, klickar du på **paus**, och sedan visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="a5b82-126">toosuspend compute and memory resources, click **Pause**, and then a confirmation message appears.</span></span> <span data-ttu-id="a5b82-127">Klicka på **Ja** tooconfirm eller **inga** toocancel.</span><span class="sxs-lookup"><span data-stu-id="a5b82-127">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Bekräfta paus][7]
3. <span data-ttu-id="a5b82-129">Medan SQL Data Warehouse startar hello databasen hello status är **pausa**.</span><span class="sxs-lookup"><span data-stu-id="a5b82-129">While SQL Data Warehouse is starting hello database, hello status is **Pausing**.</span></span>
4. <span data-ttu-id="a5b82-130">När hello status är **pausad**hello paus åtgärden är klar och du inte längre att debiteras för dwu: er.</span><span class="sxs-lookup"><span data-stu-id="a5b82-130">When hello status is **Paused**, hello pause operation is done and you are no longer being charged for DWUs.</span></span>

    ![Pausa status][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="a5b82-132">Återuppta beräkning</span><span class="sxs-lookup"><span data-stu-id="a5b82-132">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="a5b82-133">tooresume en-databas:</span><span class="sxs-lookup"><span data-stu-id="a5b82-133">tooresume a database:</span></span>

1. <span data-ttu-id="a5b82-134">Öppna hello [Azure-portalen] [ Azure portal] och öppna databasen.</span><span class="sxs-lookup"><span data-stu-id="a5b82-134">Open hello [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="a5b82-135">Observera att hello Status är **pausad**.</span><span class="sxs-lookup"><span data-stu-id="a5b82-135">Notice that hello Status is **Paused**.</span></span>

    ![Pausa databas][4]
2. <span data-ttu-id="a5b82-137">tooresume hello databasen klickar du på **starta**, och sedan visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="a5b82-137">tooresume hello database click **Start**, and then a confirmation message appears.</span></span> <span data-ttu-id="a5b82-138">Klicka på **Ja** tooconfirm eller **inga** toocancel.</span><span class="sxs-lookup"><span data-stu-id="a5b82-138">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Bekräfta återuppta][5]
3. <span data-ttu-id="a5b82-140">Medan SQL Data Warehouse startar hello databasen är hello status inställd på ”återupptar”.</span><span class="sxs-lookup"><span data-stu-id="a5b82-140">While SQL Data Warehouse is starting hello database, hello status is "Resuming".</span></span>
4. <span data-ttu-id="a5b82-141">När hello status är **online**, hello-databasen är klar.</span><span class="sxs-lookup"><span data-stu-id="a5b82-141">When hello status is **online**, hello database is ready.</span></span>

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="a5b82-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5b82-143">Next steps</span></span>
<span data-ttu-id="a5b82-144">Mer information finns i [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="a5b82-144">For more information, see [Management overview][Management overview].</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
