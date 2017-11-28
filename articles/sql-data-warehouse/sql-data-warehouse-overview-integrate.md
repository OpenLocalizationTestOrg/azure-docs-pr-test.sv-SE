---
title: "aaaBuild integrerad lösningar med SQL Data Warehouse | Microsoft Docs"
description: "Verktyg och partners med lösningar som kan integreras med SQL Data Warehouse. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: c8a4202dd84305bea4e4c2faf0e4791d026e794f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a><span data-ttu-id="46072-103">Dra nytta av andra tjänster med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="46072-103">Leverage other services with SQL Data Warehouse</span></span>
<span data-ttu-id="46072-104">Dessutom tooits viktiga funktioner, SQL Data Warehouse gör det möjligt för användare tooleverage många av hello andra tjänster i Azure tillsammans med den.</span><span class="sxs-lookup"><span data-stu-id="46072-104">In addition tooits core functionality, SQL Data Warehouse enables users tooleverage many of hello other services in Azure alongside it.</span></span>  <span data-ttu-id="46072-105">Vi har särskilt för närvarande vidtagit åtgärder för toodeeply integreras med hello följande:</span><span class="sxs-lookup"><span data-stu-id="46072-105">Specifically, we have currently taken steps toodeeply integrate with hello following:</span></span>

* <span data-ttu-id="46072-106">Power BI</span><span class="sxs-lookup"><span data-stu-id="46072-106">Power BI</span></span>
* <span data-ttu-id="46072-107">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="46072-107">Azure Data Factory</span></span>
* <span data-ttu-id="46072-108">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="46072-108">Azure Machine Learning</span></span>
* <span data-ttu-id="46072-109">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="46072-109">Azure Stream Analytics</span></span>

<span data-ttu-id="46072-110">Vi arbetar tooconnect med fler tjänster över hello Azure-ekosystemet.</span><span class="sxs-lookup"><span data-stu-id="46072-110">We are working tooconnect with more services across hello Azure ecosystem.</span></span>

## <a name="power-bi"></a><span data-ttu-id="46072-111">Power BI</span><span class="sxs-lookup"><span data-stu-id="46072-111">Power BI</span></span>
<span data-ttu-id="46072-112">Integrering med Power BI kan du tooleverage hello datorkraft för SQL Data Warehouse med dynamiska hello-rapportering och visualisering av Power BI.</span><span class="sxs-lookup"><span data-stu-id="46072-112">Power BI integration allows you tooleverage hello compute power of SQL Data Warehouse with hello dynamic reporting and visualization of Power BI.</span></span> <span data-ttu-id="46072-113">Power BI-integrering innehåller för närvarande:</span><span class="sxs-lookup"><span data-stu-id="46072-113">Power BI integration currently includes:</span></span>

* <span data-ttu-id="46072-114">**Direct Connect**: en mer avancerad anslutning med logiska pushdown mot SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="46072-114">**Direct Connect**: A more advanced connection with logical pushdown against SQL Data Warehouse.</span></span>  <span data-ttu-id="46072-115">Det ger snabbare analys av en större skala.</span><span class="sxs-lookup"><span data-stu-id="46072-115">This provides faster analysis on a larger scale.</span></span>
* <span data-ttu-id="46072-116">**Öppna i Power BI**: hello ”öppna i Power BI-knappen skickar instans information tooPower BI, vilket ger en mer sömlös anslutning.</span><span class="sxs-lookup"><span data-stu-id="46072-116">**Open in Power BI**: hello 'Open in Power BI' button passes instance information tooPower BI, allowing for a more seamless connection.</span></span>

<span data-ttu-id="46072-117">Se [integrera med Power BI](sql-data-warehouse-integrate-power-bi.md) eller hello [Power BI-dokumentationen](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="46072-117">See [Integrate with Power BI](sql-data-warehouse-integrate-power-bi.md) or hello [Power BI documentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) for more information.</span></span>

## <a name="azure-data-factory"></a><span data-ttu-id="46072-118">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="46072-118">Azure Data Factory</span></span>
<span data-ttu-id="46072-119">Azure Data Factory ger användarna en hanterad plattform toocreate rörledningar komplexa extrahera belastning.</span><span class="sxs-lookup"><span data-stu-id="46072-119">Azure Data Factory gives users a managed platform toocreate complex Extract-Load pipelines.</span></span>  <span data-ttu-id="46072-120">SQL Data Warehouse-integrering med Azure Data Factory omfattar hello följande:</span><span class="sxs-lookup"><span data-stu-id="46072-120">SQL Data Warehouse's integration with Azure Data Factory includes hello following:</span></span>

* <span data-ttu-id="46072-121">**Lagrade procedurer**: Dirigera hello körning av lagrade procedurer i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="46072-121">**Stored Procedures**: Orchestrate hello execution of stored procedures on SQL Data Warehouse.</span></span>
* <span data-ttu-id="46072-122">**Kopiera**: Använd ADF toomove data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="46072-122">**Copy**: Use ADF toomove data into SQL Data Warehouse.</span></span>  <span data-ttu-id="46072-123">Den här åtgärden kan använda ADF: s standard data movement mekanism eller PolyBase under hello omfattar.</span><span class="sxs-lookup"><span data-stu-id="46072-123">This operation can use ADF's standard data movement mechanism or PolyBase under hello covers.</span></span> 

<span data-ttu-id="46072-124">Se [integrera med Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) eller hello [dokumentation för Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="46072-124">See [Integrate with Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) or hello [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/) for more information.</span></span>

## <a name="azure-machine-learning"></a><span data-ttu-id="46072-125">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="46072-125">Azure Machine Learning</span></span>
<span data-ttu-id="46072-126">Azure Machine Learning är en helt hanterad analytics-tjänst som gör att användarna toocreate invecklade modeller som utnyttjar en stor mängd förutsägande verktyg.</span><span class="sxs-lookup"><span data-stu-id="46072-126">Azure Machine Learning is a fully managed analytics service which allows users toocreate intricate models leveraging a large set of predictive tools.</span></span>  <span data-ttu-id="46072-127">SQL Data Warehouse stöds som en källa och ett mål för dessa modeller med hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="46072-127">SQL Data Warehouse is supported as both a source and destination for these models with hello following functionality:</span></span>

* <span data-ttu-id="46072-128">**Läs Data:** enhet modeller i stor skala med T-SQL mot SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="46072-128">**Read Data:** Drive models at scale using T-SQL against SQL Data Warehouse.</span></span>
* <span data-ttu-id="46072-129">**Skriva Data:** att ändringar ska utföras från någon modell tillbaka tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="46072-129">**Write Data:** Commit changes from any model back tooSQL Data Warehouse.</span></span>

<span data-ttu-id="46072-130">Se [integrera med Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) eller hello [Azure Machine Learning dokumentationen](https://azure.microsoft.com/services/machine-learning/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="46072-130">See [Integrate with Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) or hello [Azure Machine Learning documentation](https://azure.microsoft.com/services/machine-learning/) for more information.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="46072-131">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="46072-131">Azure Stream Analytics</span></span>
<span data-ttu-id="46072-132">Azure Stream Analytics är en komplex, helt hanterad infrastruktur för bearbetning och förbrukar händelsedata genererade från Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="46072-132">Azure Stream Analytics is a complex, fully managed infrastructure for processing and consuming event data generated from Azure Event Hub.</span></span>  <span data-ttu-id="46072-133">Integrering med SQL Data Warehouse kan för strömmande data toobe effektivt bearbetas och lagras tillsammans med relationsdata aktivera djupare mer avancerad analys.</span><span class="sxs-lookup"><span data-stu-id="46072-133">Integration with SQL Data Warehouse allows for streaming data toobe effectively processed and stored alongside relational data enabling deeper, more advanced analysis.</span></span>  

* <span data-ttu-id="46072-134">**Jobbutdata:** skicka utdata från Stream Analytics-jobb direkt tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="46072-134">**Job Output:** Send output from Stream Analytics jobs directly tooSQL Data Warehouse.</span></span>

<span data-ttu-id="46072-135">Se [integrera med Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) eller hello [Azure Stream Analytics dokumentationen](https://azure.microsoft.com/documentation/services/stream-analytics/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="46072-135">See [Integrate with Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) or hello [Azure Stream Analytics documentation](https://azure.microsoft.com/documentation/services/stream-analytics/) for more information.</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
