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
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Hantera datorkraft i Azure SQL Data Warehouse (Azure portal)
> [!div class="op_single_selector"]
> * [Översikt](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Skala datorkraft
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

toochange beräkningsresurser:

1. Öppna hello [Azure-portalen][Azure portal], öppna databasen och på **skala**.

    ![Klicka på skalan][1]
2. Dra reglaget hello åt vänster eller höger toochange hello DWU-inställningar i hello skala bladet.

    ![Flytta skjutreglaget][2]
3. Klicka på **Spara**. Ett meddelande visas. Klicka på **Ja** tooconfirm eller **inga** toocancel.

    ![Klicka på Spara][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pausa beräkning
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

toopause en-databas:

1. Öppna hello [Azure-portalen] [ Azure portal] och öppna databasen. Observera att hello Status är **Online**.

    ![Onlinestatus][6]
2. toosuspend beräknings- och resurser, klickar du på **paus**, och sedan visas ett bekräftelsemeddelande. Klicka på **Ja** tooconfirm eller **inga** toocancel.

    ![Bekräfta paus][7]
3. Medan SQL Data Warehouse startar hello databasen hello status är **pausa**.
4. När hello status är **pausad**hello paus åtgärden är klar och du inte längre att debiteras för dwu: er.

    ![Pausa status][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Återuppta beräkning
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

tooresume en-databas:

1. Öppna hello [Azure-portalen] [ Azure portal] och öppna databasen. Observera att hello Status är **pausad**.

    ![Pausa databas][4]
2. tooresume hello databasen klickar du på **starta**, och sedan visas ett bekräftelsemeddelande. Klicka på **Ja** tooconfirm eller **inga** toocancel.

    ![Bekräfta återuppta][5]
3. Medan SQL Data Warehouse startar hello databasen är hello status inställd på ”återupptar”.
4. När hello status är **online**, hello-databasen är klar.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nästa steg
Mer information finns i [översikt över][Management overview].

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
