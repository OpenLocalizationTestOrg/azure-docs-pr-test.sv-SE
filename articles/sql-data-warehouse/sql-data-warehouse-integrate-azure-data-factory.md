---
title: aaaUse Azure Data Factory med SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda Azure Data Factory (ADM) med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 492de762-c7a2-4cdb-943f-3135230e94f1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: d40a547830f9681504253d39ae3066800a955c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a><span data-ttu-id="b5024-103">Använda Azure Data Factory med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b5024-103">Use Azure Data Factory with SQL Data Warehouse</span></span>
<span data-ttu-id="b5024-104">Azure Data Factory tillhandahåller en helt hanterad metod för att samordna hello överföring av data och körning av lagrade procedurer i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="b5024-104">Azure Data Factory provides a fully managed method for orchestrating hello transfer of data and execution of stored procedures on SQL Data Warehouse.</span></span>  <span data-ttu-id="b5024-105">Detta gör att du toomore enkelt inställning och schema komplexa extrahera Transform och Load (ETL) procedurer med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="b5024-105">This will allow you toomore easily set-up and schedule complex Extract Transform and Load (ETL) procedures with SQL Data Warehouse.</span></span> <span data-ttu-id="b5024-106">En fullständig översikt över Azure Data Factory finns hello [dokumentation för Azure Data Factory][Azure Data Factory documentation].</span><span class="sxs-lookup"><span data-stu-id="b5024-106">For a more complete overview of Azure Data Factory, see hello [Azure Data Factory documentation][Azure Data Factory documentation].</span></span>

## <a name="data-movement"></a><span data-ttu-id="b5024-107">Dataförflyttning</span><span class="sxs-lookup"><span data-stu-id="b5024-107">Data Movement</span></span>
<span data-ttu-id="b5024-108">Azure Data Factory gör det möjligt för flytt av data mellan både lokala källor och olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b5024-108">Azure Data Factory enables data movement between both on-premises sources and different Azure services.</span></span>  <span data-ttu-id="b5024-109">Övergripande, aktuell integrering med Azure Data Factory stöder tooand för flytt av data från hello följande platser:</span><span class="sxs-lookup"><span data-stu-id="b5024-109">Overall, current integration with Azure Data Factory supports data movement tooand from hello following locations:</span></span>

* <span data-ttu-id="b5024-110">Azure-blobblagring</span><span class="sxs-lookup"><span data-stu-id="b5024-110">Azure blob storage</span></span>
* <span data-ttu-id="b5024-111">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b5024-111">Azure SQL Database</span></span>
* <span data-ttu-id="b5024-112">Lokal SQLServer</span><span class="sxs-lookup"><span data-stu-id="b5024-112">On-premises SQL Server</span></span>
* <span data-ttu-id="b5024-113">SQLServer på IaaS</span><span class="sxs-lookup"><span data-stu-id="b5024-113">SQL Server on IaaS</span></span>

<span data-ttu-id="b5024-114">Mer information om hur tooset upp en data-kopieringsaktiviteten finns [kopiera data med Azure Data Factory][Copy data with Azure Data Factory]</span><span class="sxs-lookup"><span data-stu-id="b5024-114">For information on how tooset up a data copy activity see [Copy data with Azure Data Factory][Copy data with Azure Data Factory]</span></span>

## <a name="stored-procedures"></a><span data-ttu-id="b5024-115">Lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="b5024-115">Stored Procedures</span></span>
 <span data-ttu-id="b5024-116">I hello samma sätt kan det vara används tooschedule dataöverföring, Azure Data Factory kan också vara används tooorchestrate hello körning av lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="b5024-116">In hello same way it can be used tooschedule data transfer, Azure Data Factory can also be used tooorchestrate hello execution of stored procedures.</span></span>  <span data-ttu-id="b5024-117">Detta gör mer komplexa pipelines toobe skapas och utökar Azure Data Factory möjlighet tooleverage hello dataresurser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="b5024-117">This allows more complex pipelines toobe created and extends Azure Data Factory's ability tooleverage hello computational power of SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5024-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5024-118">Next steps</span></span>
<span data-ttu-id="b5024-119">En översikt över integrering finns [översikt över SQL Data Warehouse-integration][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="b5024-119">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>
<span data-ttu-id="b5024-120">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="b5024-120">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

