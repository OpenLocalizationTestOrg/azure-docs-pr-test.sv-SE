---
title: Hantera datorkraft i Azure SQL Data Warehouse (Azure portal) | Microsoft Docs
description: "Azure portal uppgifter för hantering av beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkningsresurser för att spara kostnader."
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
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a><span data-ttu-id="b2eb8-105">Hantera datorkraft i Azure SQL Data Warehouse (Azure portal)</span><span class="sxs-lookup"><span data-stu-id="b2eb8-105">Manage compute power in Azure SQL Data Warehouse (Azure portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b2eb8-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="b2eb8-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="b2eb8-107">Portal</span><span class="sxs-lookup"><span data-stu-id="b2eb8-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="b2eb8-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b2eb8-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="b2eb8-109">REST</span><span class="sxs-lookup"><span data-stu-id="b2eb8-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="b2eb8-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="b2eb8-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a><span data-ttu-id="b2eb8-111">Skala datorkraft</span><span class="sxs-lookup"><span data-stu-id="b2eb8-111">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="b2eb8-112">Ändra beräkningsresurser:</span><span class="sxs-lookup"><span data-stu-id="b2eb8-112">To change compute resources:</span></span>

1. <span data-ttu-id="b2eb8-113">Öppna den [Azure-portalen][Azure portal], öppna databasen och på **skala**.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-113">Open the [Azure portal][Azure portal], open your database, and click **Scale**.</span></span>

    ![Klicka på skalan][1]
2. <span data-ttu-id="b2eb8-115">Flytta skjutreglaget åt vänster eller höger för att ändra DWU-inställningen i bladet skala.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-115">In the Scale blade, move the slider left or right to change the DWU setting.</span></span>

    ![Flytta skjutreglaget][2]
3. <span data-ttu-id="b2eb8-117">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-117">Click **Save**.</span></span> <span data-ttu-id="b2eb8-118">Ett meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-118">A confirmation message appears.</span></span> <span data-ttu-id="b2eb8-119">Klicka på **Ja** att bekräfta eller **inga** att avbryta.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-119">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Klicka på Spara][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="b2eb8-121">Pausa beräkning</span><span class="sxs-lookup"><span data-stu-id="b2eb8-121">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="b2eb8-122">Så här pausar en-databas:</span><span class="sxs-lookup"><span data-stu-id="b2eb8-122">To pause a database:</span></span>

1. <span data-ttu-id="b2eb8-123">Öppna den [Azure-portalen] [ Azure portal] och öppna databasen.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-123">Open the [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="b2eb8-124">Observera att statusen är **Online**.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-124">Notice that the Status is **Online**.</span></span>

    ![Onlinestatus][6]
2. <span data-ttu-id="b2eb8-126">Om du vill pausa beräknings-och minnesresurser klickar du på **paus**, och sedan visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-126">To suspend compute and memory resources, click **Pause**, and then a confirmation message appears.</span></span> <span data-ttu-id="b2eb8-127">Klicka på **Ja** att bekräfta eller **inga** att avbryta.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-127">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Bekräfta paus][7]
3. <span data-ttu-id="b2eb8-129">Medan SQL Data Warehouse startar databasen status är **pausa**.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-129">While SQL Data Warehouse is starting the database, the status is **Pausing**.</span></span>
4. <span data-ttu-id="b2eb8-130">När statusen är **pausad**, pausa åtgärden är klar och du inte längre att debiteras för dwu: er.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-130">When the status is **Paused**, the pause operation is done and you are no longer being charged for DWUs.</span></span>

    ![Pausa status][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="b2eb8-132">Återuppta beräkning</span><span class="sxs-lookup"><span data-stu-id="b2eb8-132">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="b2eb8-133">Så här återupptar en-databas:</span><span class="sxs-lookup"><span data-stu-id="b2eb8-133">To resume a database:</span></span>

1. <span data-ttu-id="b2eb8-134">Öppna den [Azure-portalen] [ Azure portal] och öppna databasen.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-134">Open the [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="b2eb8-135">Observera att statusen är **pausad**.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-135">Notice that the Status is **Paused**.</span></span>

    ![Pausa databas][4]
2. <span data-ttu-id="b2eb8-137">Att återuppta databasen Klicka **starta**, och sedan visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-137">To resume the database click **Start**, and then a confirmation message appears.</span></span> <span data-ttu-id="b2eb8-138">Klicka på **Ja** att bekräfta eller **inga** att avbryta.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-138">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Bekräfta återuppta][5]
3. <span data-ttu-id="b2eb8-140">Medan SQL Data Warehouse startar databasen är status inställd på ”återupptar”.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-140">While SQL Data Warehouse is starting the database, the status is "Resuming".</span></span>
4. <span data-ttu-id="b2eb8-141">När statusen är **online**, databasen är klar.</span><span class="sxs-lookup"><span data-stu-id="b2eb8-141">When the status is **online**, the database is ready.</span></span>

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="b2eb8-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2eb8-143">Next steps</span></span>
<span data-ttu-id="b2eb8-144">Mer information finns i [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="b2eb8-144">For more information, see [Management overview][Management overview].</span></span>

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
